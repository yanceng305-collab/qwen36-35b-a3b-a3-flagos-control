# A2 REFERENCE ONLY — NOT A3 ACCEPTANCE

本文件整理 User提供的三份 Qwen3.6 FL技术资料中已经在 **2×Ascend 910B1（A2）** 上记录的结果。所有 `PASS`、默认值、性能和容量数字都只在资料描述的 A2环境/版本/任务边界内成立。

```text
A2 DOCUMENTED PASS
!= A3 EXECUTION PASS
!= A3 FORMAL ACCEPTANCE
```

## A2 documented environment

| Component | A2 documented value |
| --- | --- |
| Hardware | 2×Ascend 910B1，64 GiB HBM/device |
| Base image | `quay.io/ascend/vllm-ascend:v0.20.2rc1` |
| Host driver | `26.0.rc1` |
| CANN | Toolkit 9.0.0，`V100R001C10SPC001B250` |
| Python | 3.11.15 / aarch64 |
| PyTorch / torch-npu | `2.10.0+cpu` / `2.10.0` |
| vLLM / Transformers | `0.20.2` / `5.5.3` |
| Triton | 3.2.0，image-provided Ascend build |
| Model | Qwen3.6-35B-A3B BF16，约 67 GiB |
| Parallelism | DP1 / TP2 first；EP2另行验证 |
| Graph | `FULL_DECODE_ONLY` |

Image名称包含 vLLM-Ascend，但资料记录的 final FL runtime卸载了 `vllm-ascend` Python package，使用 image作为 matched CANN/torch-npu/vLLM/Triton carrier。该做法在 A3必须重新验证，不能由 A2外推。

## A2 standalone runtime / build

资料记录：

- clean non-incremental A2 build约 `1,109.25 s`；
- wheel使用 `ascend910b` target / `prebuilt/ascend910b1` family；
- wheel包含 reduced `_C_ascend` 与 8个 Qwen3.6 OPP；
- 离开 source tree后 force-install wheel，`vllm_fl`来自 site-packages；
- `vllm_ascend` absent，`_C_ascend.npu_apply_top_k_top_p`注册成功；
- executable Python没有 direct `vllm_ascend` import；
- runtime ownership走 `VLLM_PLUGINS=fl → PlatformFL → WorkerFL → ModelRunnerFL → FL-local Ascend`。

这些是 A2 build/install事实。A3必须在 `ascend910_93` family compatible环境重建，禁止使用该 A2 wheel。

## A2 functional reference matrix

以下每项都是 **A2 REFERENCE ONLY**：

- TP2 BF16 eager：8个短 prompt、2个重复 prompt、1个 1,215-token prompt完成；输出可读，selected logprob finite，无明显非法字符、长同 token循环或 pathological 8-gram repetition。
- `FULL_DECODE_ONLY`：历史 capture sizes `[1,2,4,8]`通过；最终 `max_num_seqs=64`服务自动 capture `[1,2,4,8,16,24,32,40,48,56,64]`并完成 replay。
- GDN / Mamba state / full attention / MoE均在资料记录的完整模型路径中被覆盖。
- HCCL AIV默认在 TP2 eager、TP2 graph与 EP2 graph中被执行。
- EP2：256 experts线性分片为每 rank 128；correctness-first AllGather。eager batch=2、2×64 output及 graph batch=2、2×32 first/replay在该受限对比中与 matched native token-identical。
- Aligned prefix caching：eager/graph短生命周期 `0 hit → 2,048 hit → reset 0`；long graph使用 65,536-token输入、1,024 output、49,152 aligned shared prefix、两个并发 hit请求与 reset，四个输出完成且 finite。
- `mamba_cache_mode=align`是资料中的支持路径；`all`明确 unsupported。
- chunked prefill与 async scheduling由 vLLM 0.20.2自动启用，并被 eager/graph/EP/concurrency运行覆盖。
- final matrix覆盖 input `1K/4K/16K/64K` × concurrency `C1/C8/C32/C64`，output=1,024、temperature=1、独立 salted prompt及 exact prompt-token检查；FL与 native均为 16/16 strict pass。

Functional equality不等于普通 BF16 token-level numerical parity；资料只对受限 EP2比较记录 token-identical。未来 A3 hard gate应关注 readable output、finite logprobs、stable state、repeatability和无 pathological repetition，并对需要的 strict oracle单独定义 tolerance。

## A2 performance / capacity reference

**以下所有数字都是 A2，不是 A3 SLO或 Acceptance。**

| Input | C | FL output TPS | Native output TPS |
| ---: | ---: | ---: | ---: |
| 1K | 1 | 40.812 | 38.629 |
| 1K | 8 | 274.022 | 269.658 |
| 1K | 32 | 919.742 | 856.692 |
| 1K | 64 | 1429.733 | 1330.189 |
| 4K | 1 | 41.560 | 41.114 |
| 4K | 8 | 268.749 | 263.576 |
| 4K | 32 | 797.960 | 739.176 |
| 4K | 64 | 1105.848 | 1041.375 |
| 16K | 1 | 45.341 | 42.219 |
| 16K | 8 | 259.515 | 260.325 |
| 16K | 32 | 478.954 | 461.407 |
| 16K | 64 | 575.698 | 556.461 |
| 64K | 1 | 40.669 | 35.683 |
| 64K | 8 | 129.146 | 122.846 |
| 64K | 32 | 146.145 | 142.108 |
| 64K | 64 | 152.785 | 149.087 |

资料汇总的 A2 reference：

- FL/native output TPS几何均值 `104.824%`（常见四舍五入 `104.82%`）；minimum cell ratio `99.689%`。
- 64K/C64 batch time：FL `428.943 s` / native `439.581 s`，64/64输出完成。
- KV capacity：FL `1,652,053` tokens / theoretical 66,560-token concurrency `24.82x`；native `1,703,253` / `25.59x`，约 3% gap。
- clean-cache start-to-ready约 `320 s` / `354 s`；ready后第一个 64K/C1/O1024约 `103.407 s` / `104.598 s`。
- persistent-cache FL restart约 `254 s` start-to-ready，第一个 64K/C1/O1024约 `27.34 s`。
- O8 warm-up matrix中 FL没有比 native慢的 cell；64K/C64 `286.625 s` / `287.894 s`。
- long prefix gate：init `188.689 s`；seed `35.031 s`；两个 concurrent hit `35.630 s`；reset `32.940 s`；hits `0 → 49,152 each → 0`。
- EP2 eager 2×64：FL `19.483 s` / native `18.435 s`；graph first 2×32 `2.375 s` / `2.201 s`，replay `1.564 s` / `1.468 s`。

clean start、first external request、shape/JIT warm-up、persistent-cache restart与 steady throughput是不同指标；A3不得合并成一个“首请求”数字。

## A2 memory / Hybrid KV reference

- Qwen hybrid path在 TP2下有 10个 pools；资料给出的每 page `2,121,728 B`，布局 `[conv 24,576 B][SSM/K 1,048,576 B][V 1,048,576 B]`。
- planner与 physical allocation必须一致：一个 planned tensor对应一个 raw backing，`shared_by` layers alias该 backing；Mamba从头部切 state，attention K/V使用尾部。
- 旧实现为 Mamba与Attention分别分配 full raw导致近似 double allocation；资料记录通过真实 NPU memory profile + one shared raw恢复接近 native capacity。
- profile dummy sampling在 `compute_logits`停止；multimodal encoder profile使用 unpadded fused attention，避免 quadratic score OOM。
- graph fixed-address inputs使用 persistent buffers；`reshape_and_cache` slot mapping为 persistent int32，不能 capture临时 `.to(torch.int32)`地址。
- Triton/vLLM caches必须按 FL/vLLM/CANN/torch-npu/Triton/ABI/SoC identity隔离；A2曾出现 cross-plugin cache collision造成 NaN/乱码假象。

## A2 accepted defaults — reference only

- `USE_FLAGGEMS=0`；
- `VLLM_WORKER_MULTIPROC_METHOD=spawn`；
- `TASK_QUEUE_ENABLE=1`，值2与 graph不兼容；
- `HCCL_OP_EXPANSION_MODE=AIV`（除非 User覆盖）；
- `HCCL_BUFFSIZE=512`用于资料中的 A2 DP1/TP2 profile；不是 A3性能结论；
- `enable_cpu_binding=true`；A3 global CPU slices尚未实测；
- `enable_global_stream_random_sample=true`；
- `enable_contiguous_mrope_copy=true`；
- `enable_int32_slot_mapping_source=true`；
- `enable_npu_memory_profiling=true`；
- `enable_hybrid_sampling_stream_return_edge=true`；
- applicable hybrid non-spec `FULL_DECODE_ONLY`启用 interleaved graph task update，metadata不完整时 fallback旧 batch order。

## A2 documented fixes — A3 must reverify

| Problem | A2 root cause / fix |
| --- | --- |
| GDN solve NaN/Inf/乱码 | Cross-plugin Triton cache collision；使用 dedicated symbols和 `~/.triton/vllm-fl-ascend-v1`隔离 |
| Graph replay `507011` / DDR out-of-range | 临时 int32 slot mapping地址失效；改为 per-group persistent full-size int32 buffer |
| 64K eager-FX shape failures | 使用 runtime-derived token dimensions/reshapes、custom-op boundary和 query-derived attention output |
| Prefix cross-block LLVM assertion | port matched Ascend Mamba batch memcpy与 8,192 launcher |
| KV capacity近似减半 | real NPU profiling + one shared hybrid raw backing |
| Multimodal profile OOM | unpadded fused-attention profile path |
| Decode M-RoPE host overhead | copy contiguous full backing；A2 C32 R7记录 +17.65% greedy / +10.29% temperature=1 |
| First GDN collective跨 rank skew | sampling done event/edge + model-layer-order interleaved dynamic task updates |
| multistream address/stream exhaustion | per-layer streams改 process singleton；feature保留但 performance default OFF |

## A2 rejected / deferred candidates

这些候选不得因为迁移到 A3而自动重新启用：

- `multistream_overlap_shared_expert` performance default：rejected，功能开关保留/default false；
- async exponential pre-generation：rejected；
- `pa_shape_list=[32]`：rejected；
- temperature-one division fastpath：rejected/reverted；
- `HCCL_BUFFSIZE=1024`与旧 `200`信号：未优于 512 baseline；
- per-process-group HCCL registration/reuse：证据不足，未迁移；
- current-stream graph replay sync：rejected/reverted；
- deferred hybrid sampling state update：rejected/reverted；
- native `enable_matmul_allreduce`：未优于 default；
- `weight_nz_mode=2`：graph warmup `aclnnMatmulWeightNz`限制失败，保留 ND/mode1；
- AscendCompiler / `npugraph_ex`：约 0.3%收益且 mixed-batch state reuse退化，reverted；
- weight prefetch：C32 decode未 active；
- FlashComm1、MC2、large EP、DP+EP、All-to-All、EPLB、CP、MTP、quantization：out of proven first-phase scope；
- global FlagGems：改变 full-path operator/numerical/lifecycle contract，明确排除。

任何 A3 performance default改变都应使用同卡、同 prompt/order/cache/concurrency/sampling的 adjacent paired or B-A-B evidence；A2 verdict只提供候选优先级，不替代 A3测试。
