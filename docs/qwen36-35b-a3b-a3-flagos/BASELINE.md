# Project Baseline and Implementation Tracking

更新时间：2026-08-25

## Current GitHub snapshot

核验于 2026-08-25 17:02 CST；正式 dispatch 前必须重新查询。

| Identity | Value |
| --- | --- |
| Implementation repository | `xiemingda-1002/vllm-plugin-FL` |
| Project tracked branch | `feature/qwen3.6-35b-a3b-ascend-graph-migration` |
| Current tracked HEAD | `7beda84f59d7b25f49cdf03bdf6efecd771067ed` |
| Current tracked tree | `a81eea55c1de548a0a1f182f51089eca0b088c82` |
| Official PR | `flagos-ai/vllm-plugin-FL#404` |
| PR state | OPEN, Draft, MERGEABLE, BLOCKED, REVIEW_REQUIRED |
| Official base branch | `release/0.2` |
| Current release HEAD | `53adefb269571684d83a51e997d3ba9be5f88235` |
| Current release tree | `9ddfd080953ad39b39772e108ff921d2973b0299` |
| Compare | 6 commits ahead / 0 behind |

Direct anchors：

- Head commit：<https://github.com/xiemingda-1002/vllm-plugin-FL/commit/7beda84f59d7b25f49cdf03bdf6efecd771067ed>
- Base commit：<https://github.com/flagos-ai/vllm-plugin-FL/commit/53adefb269571684d83a51e997d3ba9be5f88235>
- PR：<https://github.com/flagos-ai/vllm-plugin-FL/pull/404>
- Compare：<https://github.com/flagos-ai/vllm-plugin-FL/compare/release/0.2...xiemingda-1002:feature/qwen3.6-35b-a3b-ascend-graph-migration>

PR timeline证明 tracked branch从先前 `f9281f78...` force-push/rebase到当前 `7beda84...`；两者 diverged。User资料没有给出 previous exact full head，所以只能确认“PR timeline中发生更新”，不能制造资料版本的 exact compare anchor。

## Technical tuple

| Component | Baseline | Evidence state / boundary |
| --- | --- | --- |
| vLLM | `0.20.2` | Current PR/source metadata + User资料；A3 runtime尚未验证 |
| FL | official `release/0.2`系 | base当前为 `53adefb...`；adaptation由 moving PR head提供 |
| Adaptation | PR #404 current tracked head | 当前 snapshot `7beda84...`；run时重查 |
| vLLM-Ascend | `0.20.2rc1` | matched-version source/oracle reference；最终 runtime不可依赖 installed package |
| Model | `Qwen/Qwen3.6-35B-A3B` | 完整 Transformers BF16 artifact；A3 path待 User确认 |
| Architecture | `Qwen3_5MoeForConditionalGeneration`；40层，30 GDN/linear attention + 10 full attention；256 experts/top-8 | User资料；Stage 3检查真实 artifact |
| dtype / DP / TP | BF16 / DP1 / first TP2 | Project decision |
| First execution | eager | Stage 3 |
| Graph | `FULL_DECODE_ONLY` | Stage 4 only after eager Acceptance |
| Build family | A3=`ascend910_93`；A2=`ascend910b` | Current source + User资料；A3 build未验证 |
| Required controls | `VLLM_PLUGINS=fl`、`USE_FLAGGEMS=0` | Current source/project decision；A3 runtime未验证 |

MTP、quantization、initial CP、FlashComm、MC2、EPLB不在第一阶段范围。EP2、prefix、64K与性能属于后续独立 Stage。

## Current exact-head source facts

静态核对只证明代码存在，不证明 A3运行：

- `pyproject.toml` test extra固定 `vllm[audio]==0.20.2`；core wheel metadata不强制安装 vLLM。
- platform/general plugin entry point均属于 `vllm_fl`。
- `PlatformFL`、`WorkerFL`、`ModelRunnerFL` 与 FL-local Qwen/GDN/Mamba/Attention/MoE路径存在。
- 没有在 `vllm_fl`/packaging中发现 executable `import vllm_ascend`或 runtime requirement。
- NPU registration和 platform logic强制/维持 `USE_FLAGGEMS=0`。
- NPU graph只接受 `NONE`或 `FULL_DECODE_ONLY`；其他 graph mode回退 `NONE`。
- build/OPP脚本在未设置 `SOC_VERSION`时默认 `ascend910_93`；CMake支持该 family。
- exact source当前选择 **8 OPP**：`add_rms_norm_bias`、`causal_conv1d`、`recurrent_gated_delta_rule`、`chunk_fwd_o`、`chunk_gated_delta_rule_fwd_h`、`moe_gating_top_k`、`moe_init_routing_custom`、`apply_top_k_top_p_custom`。
- `_C_ascend`当前声明 **9 schemas**。PR正文旧的 7 OPP/8 schemas不再作为当前验收计数。
- runtime loader在 `SOC_VERSION`未设置时默认选择 `ascend910b1`，与 A3 build默认不同；Stage 1/2必须观察 actual env和 selected family。

详细 source anchors见 [research/CURRENT-IMPLEMENTATION-STATE.md](research/CURRENT-IMPLEMENTATION-STATE.md)。

## Project tracking target vs run source

`Current tracked HEAD`是 Control snapshot，不是永久冻结 baseline。正式 run必须写：

```text
Tracked branch: feature/qwen3.6-35b-a3b-ascend-graph-migration
Run source SHA: <dispatch-time exact HEAD>
Run source tree: <exact tree>
Working tree: clean | dirty (formal claim restrictions apply)
```

如果 branch更新，last accepted/tested SHA仍保持其结果；新 HEAD必须根据 diff决定重验，不能自动继承 Acceptance。

## Environment contract to establish on A3

当前以下字段全部 `A3 UNKNOWN`，由 Stage 1/2 Evidence填充：

- physical SKU/device count/logical mapping、driver、firmware；
- base image digest/name、container runtime/mount/device mapping；
- CANN toolkit/runtime/OPP、Python/architecture；
- torch、torch-npu、vLLM、Transformers、Triton/provider、HCCL；
- compiler/CMake/ninja/gcc/build tools与 CATLASS source；
- build-time和 runtime effective `SOC_VERSION`/family；
- wheel filename/hash/Python ABI/OPP/extension inventory；
- site-packages import origin、distribution metadata、negative `vllm_ascend` audit；
- isolated Triton/vLLM cache roots。
