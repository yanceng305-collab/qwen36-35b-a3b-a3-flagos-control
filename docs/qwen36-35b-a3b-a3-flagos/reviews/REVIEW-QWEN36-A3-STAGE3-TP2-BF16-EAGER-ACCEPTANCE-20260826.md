# Codex1 Formal Acceptance — Qwen3.6 A3 Stage 3 TP2 BF16 Eager

Review date：2026-08-26

Acceptance：**ACCEPTED**

## Accepted execution identity

| Field | Accepted identity |
| --- | --- |
| Implementation repo | `https://github.com/xiemingda-1002/vllm-plugin-FL` |
| Tracked branch | `feature/qwen3.6-35b-a3b-ascend-graph-migration` |
| Execution HEAD/tree | `e610a990d785356bf51a3cad50219d4c03310a31` / `609ff1ad0f08239f353cb4d8774e504b4deba03b` |
| Source state | detached clean checkout；无 implementation source patch |
| Official PR base | `flagos-ai/vllm-plugin-FL:release/0.2@53adefb269571684d83a51e997d3ba9be5f88235` |
| A3 image | `quay.io/ascend/vllm-ascend:v0.20.2rc1-a3-openeuler` |
| Manifest/platform digest | `sha256:442363921166771eb82baeec9c1ac0381f46fb830ead8d0e072df6e925f2a958` / `sha256:a55b2b0597f9fdd1882de9bf3b7ebc395dd77c1ca49f251d0cb759d7b2c1a807` |
| Current-head wheel | `vllm_plugin_fl-0.2.0+ge610a990d-cp311-cp311-linux_aarch64.whl` |
| Wheel SHA-256 | `2fcf788660f3fe42b364bc60d593ee1b9b634fc0632de58c444d961bff4aa1bd` |
| A3 family | `ascend910_93` |
| Model | `/data/tiankuan/zyg/FL/workspace/Qwen3.6-35B-A3B`；BF16 non-quantized |
| Parallelism/mode | DP1 / TP2 / eager；HCCL world size 2 |
| Code PR | `N/A` |

Formal Review时重新查询 GitHub，tracked HEAD/tree和 official base仍与 execution identity一致。Codex1未操作 A3 server；本次审查依据 immutable Results、manifest/checksum/raw-log pointers、Stage 1/2 Accepted foundation、Task contract和 live source identity，不补猜 Result未记录的现场事实。

## Reviewed Result chain

1. [`RESULT-QWEN36-A3-S3-TP2-BF16-EAGER-20260826T112011+0800.md`](../results/RESULT-QWEN36-A3-S3-TP2-BF16-EAGER-20260826T112011+0800.md)：Gate R PASS；Gate M因 root缺 index-required shards 16/18/22/25 STOP；Gate E未进入。
2. [`RESULT-QWEN36-A3-S3-TP2-BF16-EAGER-RESUME-20260826T115234+0800.md`](../results/RESULT-QWEN36-A3-S3-TP2-BF16-EAGER-RESUME-20260826T115234+0800.md)：同一 source/wheel/runtime继续；Gate M PASS；Gate E TP2 BF16 eager PASS；Stage 4未进入。

## Gate review

### Gate R — ACCEPTED

- clean exact current-head source完成 A3 `ascend910_93` wheel build，build/install均 exit 0；
- wheel hash、ABI、OPP/schema reconciliation和 A2 residue audit有稳定 Evidence pointer；
- standalone FL来自 wheel/site-packages，无 source shortcut；`vllm-ascend` absent、`vllm_ascend`不可import；
- `VLLM_PLUGINS=fl`、`USE_FLAGGEMS=0`，PlatformFL和 A3 `_C_ascend`/OPP origin正确；
- real A3 `_C_ascend.npu_add_rms_norm_bias` smoke通过 reference、finite、NPU device、synchronize和 no-fallback checks。

Resume复用 Gate R是有效的：它属于同一 Task，source HEAD/tree、clean source path、installed wheel和 wheel SHA-256均未变化，container/runtime也连续保留。Gate R没有因模型 artifact后来完成而失效，无需重复 rebuild。

### Gate M — ACCEPTED

- initial STOP准确阻止了不完整模型进入 Gate E；Codex2未修复或修改模型；
- resume时 index-required 26/26 shards全部位于 model root，`._____temp/`为空，无 `.lock/.tmp/.incomplete/.aria2/.part` marker；
- index共1045个 weight entries，header-only audit读取26个shards并确认1045/1045 tensors均为BF16；
- `quantization_config` absent，architecture和40 layers、30 linear attention + 10 full attention、256 experts/top-8合同一致；
- checksum manifest已保存。

因此 resume直接闭合 initial Gate M blocker；initial STOP Result保持历史事实，不需改写。

### Gate E — ACCEPTED

- effective config为 DP1、TP2、BF16、eager、graph NONE、MTP/quantization/prefix off；
- HCCL backend `hccl`、world size 2、ranks 0/1和两个 Worker均成功初始化；
- architecture成功解析，26/26 checkpoint shards完整加载；
- PlatformFL、WorkerFL、ModelRunnerFL来自 standalone site-packages `vllm_fl`；
- full model generation覆盖40-layer hybrid forward；日志同时证明 Qwen hybrid patch、Mamba metadata/batch path、FL-local GDN path和 Ascend vendor MoE `topk_softmax` ownership。结合模型config中的10个full-attention层与完整forward，Stage 3 live-path合同闭合；
- 两个不同prompt均完成prefill和8-token decode，输出非空、可读且不同，logprobs finite；最终 `torch.npu.synchronize()`成功；
- 未观察model CPU offload/fallback、NaN/Inf、stale identical output或pathological repetition；本结论不外推为逐算子profiling证明；
- `vllm-ascend` distribution absent、`vllm_ascend`不可import，FlagGems absent且 `USE_FLAGGEMS=0`。

## Intermediate failure disposition

### First resume invariant probe — ACCEPTED AS HARNESS/SCOPE CORRECTION

第一次probe未设置 `ASCEND_RT_VISIBLE_DEVICES`，privileged container因此看到16个logical devices并nonzero exit。随后在任何Gate E model execution前恢复 Accepted scope `ASCEND_RT_VISIBLE_DEVICES=0,1` / `ASCEND_VISIBLE_DEVICES=0,1`，rerun证明availability true、device count 2、device names正确；所有正式Gate E命令使用该scope。

该失败不代表Accepted runtime在正式执行中漂移，也未污染模型状态；无需重跑Gate E。

### stdin multiprocessing spawn — ACCEPTED AS PRE-CONSTRUCTION HARNESS FAILURE

第一次Gate E attempt从stdin启动，multiprocessing spawn无法reopen `/tmp/<stdin>`而exit 1。失败发生在model construction前。随后普通file script使用同一冻结配置完成model construction、26/26 load、TP2 generation、final sync并exit 0。

该失败不触及模型或设备计算语义；无需机械重复正式file-script PASS。

## Acceptance boundary

Stage 3 `ACCEPTED`只证明上述 exact source/wheel/model/environment下：

- current-head A3 wheel/standalone/custom-op regression；
- complete BF16 model identity与完整权重加载；
- DP1/TP2 HCCL eager model execution；
- Qwen hybrid GDN/Mamba/full-attention/MoE live forward；
- bounded prefill、多token decode、两次可读finite generation和基础state freshness；
- no installed/runtime `vllm-ascend`、no FlagGems activation和无已观察到的非法CPU fallback。

仍未证明：

- `FULL_DECODE_ONLY` graph capture/replay/fixed-address/state correctness；
- serve/API、prefix、EP2、long context、64K、functional matrix；
- capacity、latency、throughput或任何performance结论。

Source、wheel、model、image/CANN/runtime或device topology变化时，按diff和变化范围重验。Moving branch future HEAD不得自动继承本Acceptance。

## Final conclusion

Stage 3 TP2 BF16 Eager：**ACCEPTED**。

Stage 4 `FULL_DECODE_ONLY` graph：**UNLOCKED / Ready Task awaiting explicit User dispatch**。
