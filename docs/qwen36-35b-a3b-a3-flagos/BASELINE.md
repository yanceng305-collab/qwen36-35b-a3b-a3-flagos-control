# Project Frozen Validation Baseline

更新时间：2026-08-26

## Frozen validation identity

冻结日期：2026-08-26。依据：`D-030 / FROZEN-UPSTREAM-VALIDATION-BASELINE` User Decision。Stage 6及以后不再跟踪upstream moving HEAD作为execution gate。

| Identity | Value |
| --- | --- |
| Implementation repository | `xiemingda-1002/vllm-plugin-FL` |
| Frozen source/tree | `e610a990d785356bf51a3cad50219d4c03310a31` / `609ff1ad0f08239f353cb4d8774e504b4deba03b` |
| Accepted wheel | `vllm_plugin_fl-0.2.0+ge610a990d-cp311-cp311-linux_aarch64.whl` |
| Accepted wheel SHA-256 | `2fcf788660f3fe42b364bc60d593ee1b9b634fc0632de58c444d961bff4aa1bd` |
| Historical reference branch | `feature/qwen3.6-35b-a3b-ascend-graph-migration` |
| Last pre-change tracked reference/tree | `032fddc91b6d013b98aed8e64ff05b54d1435648` / `463806ef18e5e31006cd4f59e6a5261fc65cea4a` |
| Stage 1/2 Accepted source | `7beda84f59d7b25f49cdf03bdf6efecd771067ed` / tree `a81eea55c1de548a0a1f182f51089eca0b088c82` |
| Official PR | `flagos-ai/vllm-plugin-FL#404` |
| PR/base status | Historical snapshot/reference only；future movement intentionally out of scope |
| Official base branch | `release/0.2` |
| Current release HEAD | `ef78dec66fea1ae858ef414584be1478929ee9b2` |
| Current release tree | `7414bac41c39bc445b0cc05dbdaecc0f08231aeb` |
| Stage 6+ execution gate | Frozen source/wheel/runtime/environment/model/workload identity only |

Direct anchors：

- Frozen source commit：<https://github.com/xiemingda-1002/vllm-plugin-FL/commit/e610a990d785356bf51a3cad50219d4c03310a31>
- Last pre-change tracked reference：<https://github.com/xiemingda-1002/vllm-plugin-FL/commit/032fddc91b6d013b98aed8e64ff05b54d1435648>
- Stage 1/2 Accepted source：<https://github.com/xiemingda-1002/vllm-plugin-FL/commit/7beda84f59d7b25f49cdf03bdf6efecd771067ed>
- Base commit：<https://github.com/flagos-ai/vllm-plugin-FL/commit/ef78dec66fea1ae858ef414584be1478929ee9b2>
- PR：<https://github.com/flagos-ai/vllm-plugin-FL/pull/404>
- Compare：<https://github.com/flagos-ai/vllm-plugin-FL/compare/release/0.2...xiemingda-1002:feature/qwen3.6-35b-a3b-ascend-graph-migration>

历史PR timeline证明branch曾从`f9281f78...`前进到Stage 1/2 source `7beda84...`、Stage 3/4/5 runtime source `e610a990...`及最后pre-change reference `032fddc9...`。已有moving-head Review确认`e610a990... -> 032fddc9...`仅为README/docs+tests变化。Stage 6起该timeline只作来源历史，不再驱动执行。

## Technical tuple

| Component | Baseline | Evidence state / boundary |
| --- | --- | --- |
| vLLM | `0.20.2` | A3 Stage 1-5 Accepted runtime tuple |
| FL | official `release/0.2`系 | adaptation固定为Frozen `e610a990...` artifact |
| Adaptation | Frozen source / Accepted wheel | `e610a990...` / tree `609ff1ad...` / wheel SHA-256 `2fcf788...` |
| vLLM-Ascend | `0.20.2rc1` | matched-version source/oracle与 official A3 environment carrier；最终 runtime不可依赖 installed package |
| Frozen accepted image | `v0.20.2rc1-a3-openeuler` exact digest/ID | Stage 6+只核对identity，不重新selection；ordinary unsuffixed tag仍排除 |
| Model | `Qwen/Qwen3.6-35B-A3B` / BF16 non-quantized | Stage 3/4 identity Accepted；26/26 shards、1045/1045 BF16 tensors |
| Model path | `/data/tiankuan/zyg/FL/workspace/Qwen3.6-35B-A3B` | Stage 3/4 Accepted identity |
| Architecture | `Qwen3_5MoeForConditionalGeneration`；40层，30 GDN/linear attention + 10 full attention；256 experts/top-8 | Stage 3/4 execution-verified |
| dtype / DP / TP | BF16 / DP1 / first TP2 | Project decision |
| First execution | eager | Stage 3 Accepted |
| Graph | `FULL_DECODE_ONLY` | Stage 4 `[1,2,4,8]` bounded correctness Accepted |
| Build family | A3=`ascend910_93`；A2=`ascend910b` | A3 wheel build/load/custom-op Accepted |
| Required controls | `VLLM_PLUGINS=fl`、`USE_FLAGGEMS=0` | Stage 1-4 runtime ownership Accepted |

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

详细历史source anchors见 [research/CURRENT-IMPLEMENTATION-STATE.md](research/CURRENT-IMPLEMENTATION-STATE.md)。`e610a990...`已构建为Stage 3/4/5 Accepted wheel；`032fddc9...`仅改README和unit tests。Later upstream HEAD不再查询、diff或重验，除非User另立new validation baseline。

## Official A3 base image route — historical selection and frozen result

Official tag/source核对固定两个候选：

```text
quay.io/ascend/vllm-ascend:v0.20.2rc1-a3
quay.io/ascend/vllm-ascend:v0.20.2rc1-a3-openeuler
```

- Ubuntu route基于 `quay.io/ascend/cann:9.0.0-a3-ubuntu22.04-py3.11`。
- openEuler route基于 `quay.io/ascend/cann:9.0.0-a3-openeuler24.03-py3.11`。
- Matched tuple：vLLM 0.20.2、Python 3.11 base（compatibility range `>=3.10,<3.12`）、CANN 9.0.0、torch/torch_npu 2.10.0/2.10.0、Triton Ascend 3.2.1。
- Ordinary `quay.io/ascend/vllm-ascend:v0.20.2rc1`是 official A2 Ubuntu route，明确不作为 A3 baseline。
- Stage 1/2已从候选中接受`quay.io/ascend/vllm-ascend:v0.20.2rc1-a3-openeuler`，manifest digest `sha256:442363921166771eb82baeec9c1ac0381f46fb830ead8d0e072df6e925f2a958`、arm64 platform digest `sha256:a55b2b0597f9fdd1882de9bf3b7ebc395dd77c1ca49f251d0cb759d7b2c1a807`、local image ID `sha256:8e7823878349b37d9900984555e28381c25681f88ff678cb4a86f7d1674a67c1`。Stage 6+核对该Accepted image/runtime，不重新做candidate selection。

详细 source/registry证据见 [research/OFFICIAL-A3-IMAGE-ROUTE.md](research/OFFICIAL-A3-IMAGE-ROUTE.md)。Carrier中原有 `vllm-ascend`与 editable source状态必须在 final FL transaction后被负向审计；正式 ownership仍是 standalone FL，不由 image名称决定。

## Frozen source vs historical upstream reference

Stage 6及后续正式run必须写：

```text
Frozen source: e610a990d785356bf51a3cad50219d4c03310a31
Frozen tree: 609ff1ad0f08239f353cb4d8774e504b4deba03b
Installed wheel SHA-256: 2fcf788660f3fe42b364bc60d593ee1b9b634fc0632de58c444d961bff4aa1bd
Wheel/site-packages origin: <actual evidence>
Last pre-change tracked reference: 032fddc91b6d013b98aed8e64ff05b54d1435648 (reference only)
```

如果branch/PR/base更新，记录为`OUT OF SCOPE / IGNORE FOR EXECUTION`即可，不查询diff、不STOP、不rebuild。若未来要验证新HEAD，User必须建立新的validation baseline/project evidence；不得覆盖本项目Frozen results。

## Environment contract to establish on A3

以下字段已有 Accepted Evidence；future Task必须验证Frozen source/artifact及environment continuity，不能只引用旧值：

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
