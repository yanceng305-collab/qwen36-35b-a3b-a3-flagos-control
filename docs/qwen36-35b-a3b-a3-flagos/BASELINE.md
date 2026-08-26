# Project Baseline and Implementation Tracking

更新时间：2026-08-25

## Current GitHub snapshot

核验于 2026-08-26 10:30 CST；正式 dispatch 前仍必须重新查询。

| Identity | Value |
| --- | --- |
| Implementation repository | `xiemingda-1002/vllm-plugin-FL` |
| Project tracked branch | `feature/qwen3.6-35b-a3b-ascend-graph-migration` |
| Current tracked HEAD | `e610a990d785356bf51a3cad50219d4c03310a31` |
| Current tracked tree | `609ff1ad0f08239f353cb4d8774e504b4deba03b` |
| Stage 1/2 Accepted source | `7beda84f59d7b25f49cdf03bdf6efecd771067ed` / tree `a81eea55c1de548a0a1f182f51089eca0b088c82` |
| Official PR | `flagos-ai/vllm-plugin-FL#404` |
| PR state | OPEN, Draft, MERGEABLE, BLOCKED, REVIEW_REQUIRED |
| Official base branch | `release/0.2` |
| Current release HEAD | `53adefb269571684d83a51e997d3ba9be5f88235` |
| Current release tree | `9ddfd080953ad39b39772e108ff921d2973b0299` |
| Compare | 8 commits ahead / 0 behind |

Direct anchors：

- Current head commit：<https://github.com/xiemingda-1002/vllm-plugin-FL/commit/e610a990d785356bf51a3cad50219d4c03310a31>
- Stage 1/2 Accepted source：<https://github.com/xiemingda-1002/vllm-plugin-FL/commit/7beda84f59d7b25f49cdf03bdf6efecd771067ed>
- Base commit：<https://github.com/flagos-ai/vllm-plugin-FL/commit/53adefb269571684d83a51e997d3ba9be5f88235>
- PR：<https://github.com/flagos-ai/vllm-plugin-FL/pull/404>
- Compare：<https://github.com/flagos-ai/vllm-plugin-FL/compare/release/0.2...xiemingda-1002:feature/qwen3.6-35b-a3b-ascend-graph-migration>

PR timeline证明 tracked branch曾从 `f9281f78...` force-push/rebase到 Stage 1/2 Accepted source `7beda84...`，随后又前进到 current `e610a990...`。每次 Result仍只绑定其 exact execution identity。

## Technical tuple

| Component | Baseline | Evidence state / boundary |
| --- | --- | --- |
| vLLM | `0.20.2` | Current PR/source metadata + User资料；A3 runtime尚未验证 |
| FL | official `release/0.2`系 | base当前为 `53adefb...`；adaptation由 moving PR head提供 |
| Adaptation | PR #404 current tracked head | 当前 snapshot `e610a990...`；run时重查；Stage 1/2 Acceptance绑定`7beda84...` |
| vLLM-Ascend | `0.20.2rc1` | matched-version source/oracle与 official A3 environment carrier；最终 runtime不可依赖 installed package |
| Official A3 image candidates | `v0.20.2rc1-a3` / `v0.20.2rc1-a3-openeuler` | Bounded selection；ordinary unsuffixed tag是A2 route并排除 |
| Model | `Qwen/Qwen3.6-35B-A3B` / BF16 non-quantized | Path已确认；`DOWNLOADING / NOT YET READY FOR STAGE 3`；不阻塞Stage 1/2 |
| Model path | `/data/tiankuan/zyg/FL/workspace/Qwen3.6-35B-A3B` | User-confirmed；Stage 1/2 inventory-only |
| Architecture | `Qwen3_5MoeForConditionalGeneration`；40层，30 GDN/linear attention + 10 full attention；256 experts/top-8 | User资料；Stage 3检查真实 artifact |
| dtype / DP / TP | BF16 / DP1 / first TP2 | Project decision |
| First execution | eager | Stage 3 |
| Graph | `FULL_DECODE_ONLY` | Stage 4 only after eager Acceptance |
| Build family | A3=`ascend910_93`；A2=`ascend910b` | Current source + User资料；A3 build未验证 |
| Required controls | `VLLM_PLUGINS=fl`、`USE_FLAGGEMS=0` | Current source/project decision；A3 runtime未验证 |

MTP、quantization、initial CP、FlashComm、MC2、EPLB不在第一阶段范围。EP2、prefix、64K与性能属于后续独立 Stage。

## Stage 1/2 Accepted exact-source facts

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

详细 source anchors见 [research/CURRENT-IMPLEMENTATION-STATE.md](research/CURRENT-IMPLEMENTATION-STATE.md)。Current `e610a990...`比 Accepted source多2个 runtime commits，涉及 NPU communicator、PlatformFL和 ModelRunnerFL，不涉及 A3 OPP/build packaging；Stage 3 Task必须先对 current head做 wheel rebuild和 bounded C/D regression。

## Official A3 base image route

Official tag/source核对固定两个候选：

```text
quay.io/ascend/vllm-ascend:v0.20.2rc1-a3
quay.io/ascend/vllm-ascend:v0.20.2rc1-a3-openeuler
```

- Ubuntu route基于 `quay.io/ascend/cann:9.0.0-a3-ubuntu22.04-py3.11`。
- openEuler route基于 `quay.io/ascend/cann:9.0.0-a3-openeuler24.03-py3.11`。
- Matched tuple：vLLM 0.20.2、Python 3.11 base（compatibility range `>=3.10,<3.12`）、CANN 9.0.0、torch/torch_npu 2.10.0/2.10.0、Triton Ascend 3.2.1。
- Ordinary `quay.io/ascend/vllm-ascend:v0.20.2rc1`是 official A2 Ubuntu route，明确不作为 A3 baseline。
- Codex2只在两个 A3候选中现场选择；两者均不兼容时 STOP，不使用 A2、nightly或其他 version fallback。

详细 source/registry证据见 [research/OFFICIAL-A3-IMAGE-ROUTE.md](research/OFFICIAL-A3-IMAGE-ROUTE.md)。Carrier中原有 `vllm-ascend`与 editable source状态必须在 final FL transaction后被负向审计；正式 ownership仍是 standalone FL，不由 image名称决定。

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

以下字段已有 Stage 1/2 Accepted Evidence；future Task在 environment/source变化时必须重新记录，不能只引用旧值：

- physical SKU/device count/logical mapping、driver、firmware；
- selected official A3 tag、pull-time digest、platform digest、image ID、OS与选择理由；
- container runtime/mount/device mapping；
- CANN toolkit/runtime/OPP、Python/architecture；
- torch、torch-npu、vLLM、Transformers、Triton/provider、HCCL；
- compiler/CMake/ninja/gcc/build tools与 CATLASS source；
- build-time和 runtime effective `SOC_VERSION`/family；
- wheel filename/hash/Python ABI/OPP/extension inventory；
- site-packages import origin、distribution metadata、negative `vllm_ascend` audit；
- isolated Triton/vLLM cache roots。
