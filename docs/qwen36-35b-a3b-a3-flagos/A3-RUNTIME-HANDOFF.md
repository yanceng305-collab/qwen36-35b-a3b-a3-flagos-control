# A3 Runtime Handoff — Qwen Validation to Future GLM Work

更新时间：2026-08-25

当前状态：**EMPTY / A3 UNVERIFIED**。本文件从项目开始建立，但只有经过真实 A3/910C execution并被 Codex1 `ACCEPTED` 的事实才能从 `Candidate`升级为 `Validated handoff`。

## Handoff admission rule

每个可继承事实至少记录：

- source task/run和 Acceptance；
- source SHA/tree/clean state；
- A3 device/driver/CANN/environment tuple；
- image/container/wheel digest或 checksum；
- command/effective config与 Evidence pointer；
- 验证范围、未覆盖边界、失效/重验条件；
- GLM侧仍需独立验证的 contract。

仅 static source、A2 evidence或 Codex2 `Execution PASS`而未 Acceptance的内容不得写成 validated handoff。

## Candidate reusable foundations

| Candidate | Current state | Required Qwen A3 evidence before handoff |
| --- | --- | --- |
| Base image / container pattern | NOT VERIFIED | exact digest、mount/device、runtime、reconstruction Accepted |
| CANN / Python / torch / torch-npu / vLLM 0.20.2 / Triton tuple | NOT VERIFIED | Stage 1 manifest + Stage 2 runtime smoke Accepted |
| A3 wheel build flow / `ascend910_93` detection | NOT VERIFIED | clean A3 build、family/artifact inventory、hash Accepted |
| `_C_ascend` build/package/load infrastructure | NOT VERIFIED | wheel origin、ABI、A3 load、actual NPU op Accepted |
| CANN OPP build/package/expose infrastructure | NOT VERIFIED | 8-OPP inventory、A3 family、runtime registration/execution Accepted |
| HCCL / multiprocessing / device mapping | NOT VERIFIED | TP2 eager/HCCL Accepted；仅已覆盖 topology可继承 |
| PlatformFL / WorkerFL / ModelRunnerFL lifecycle | NOT VERIFIED ON A3 | live A3 model path trace Accepted |
| Standalone FL formal installation | NOT VERIFIED | site-packages origin、no source PYTHONPATH、no vllm-ascend dependency Accepted |
| Cache isolation / compiler identity | NOT VERIFIED | clean/reused cache manifests和对应 correctness范围 Accepted |
| Evidence / immutable Result / reconstruction discipline | CONTROL-DEFINED, FIELD UNVERIFIED | 至少一个 A3 run完成三指针、checksum、Acceptance闭环 |
| Runtime image/wheel/startup handoff | NOT CREATED | Stage 8 freeze/reconstruction Accepted |

## Explicitly non-transferable Qwen claims

即使 Qwen A3验证通过，也不得直接外推到 GLM：

- Qwen3.6 model patch / architecture / weight loader correctness；
- GDN、Mamba state/cache、Qwen full attention；
- Qwen MoE / expert map / EP2 correctness；
- Qwen-specific `FULL_DECODE_ONLY` capture/replay/state behavior；
- Qwen-specific OPP或 `_C_ascend`算子语义正确性；
- Qwen BF16 output correctness、prefix/64K/cache semantics；
- Qwen capacity、latency、throughput、HCCL tuning或 CPU binding性能；
- 任何 GLM-5.2-W8A8、MLA、DSA/SFA、Indexer、W8A8 Linear/MoE结论。

未来 GLM恢复时必须先做 vLLM 0.20.2 GLM contract review，再逐项引用本文件中已经 Accepted且语义通用的 A3基础。

## Validated handoff entries

当前无。新增 entry必须引用 immutable Result与 Acceptance commit，不得改写历史 entry来覆盖新 environment/source。
