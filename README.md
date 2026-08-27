# Qwen3.6-35B-A3B × FlagOS × Ascend A3/910C Validation Control

本仓库是 Qwen3.6-35B-A3B A3 Validation Control Project 的正式 truth source。它管理目标、Frozen Validation Baseline、exact execution identity、Stage gate、Task、Evidence、immutable Result、Codex1 Acceptance 与 A3 Runtime Handoff。

## Project Goal

> 在 Ascend A3/910C 上验证同事Qwen3.6-35B-A3B Ascend adaptation的Frozen Validation Baseline，在 vLLM 0.20.2 + FL release/0.2 系上完成 A3-native wheel build、standalone FL runtime、TP2 eager、FULL_DECODE_ONLY graph、serve、功能/性能及最终复现，并形成可供未来 GLM-5.2-W8A8 复用的 A3 runtime / container / build / reconstruction 基础。

本项目不是重新开发一个 Qwen3.6 fork；当前路线是 `VERIFY FIRST / FIX ONLY WHEN EVIDENCE REQUIRES IT`。

## Current State

更新时间：2026-08-27

- Stage 0 Control / baseline establishment：**COMPLETE**。
- A3 Stage 1/2、Stage 3 TP2 BF16 eager、Stage 4 bounded `FULL_DECODE_ONLY [1,2,4,8]`及Stage 5 bounded serve correctness：**ACCEPTED**。
- Frozen Validation Baseline：source `e610a990...` / tree `609ff1ad...`；Accepted wheel SHA-256 `2fcf788...`。`032fddc9...`仅为冻结时最后一个pre-change tracked reference。
- Stage 6：**STOP / NOT ACCEPTED**；new-server NPU invariant在exact scope PASS；prospective run在jemalloc preload reconstruction gap处STOP，0 generation，U+FFFD仍D/unresolved。
- Next Task：**READY — `QWEN36-A3-S6-JEMALLOC-RECONSTRUCTION-AND-UFFFD-ROOT-CAUSE-DIAGNOSTIC` / Awaiting explicit User dispatch**。
- Validation Code repo/fork：**Not needed**。
- GLM-5.2-W8A8项目：由 User Decision 暂停；本仓库不接收 GLM Result。

## Frozen Validation Baseline

冻结日期：2026-08-26；User Decision `D-030 / FROZEN-UPSTREAM-VALIDATION-BASELINE`。

| Field | Current verified value |
| --- | --- |
| Implementation repo | [`xiemingda-1002/vllm-plugin-FL`](https://github.com/xiemingda-1002/vllm-plugin-FL) |
| Frozen source | `e610a990d785356bf51a3cad50219d4c03310a31` |
| Frozen source tree | `609ff1ad0f08239f353cb4d8774e504b4deba03b` |
| Accepted wheel | `vllm_plugin_fl-0.2.0+ge610a990d-cp311-cp311-linux_aarch64.whl` |
| Wheel SHA-256 | `2fcf788660f3fe42b364bc60d593ee1b9b634fc0632de58c444d961bff4aa1bd` |
| Historical reference branch | `feature/qwen3.6-35b-a3b-ascend-graph-migration` |
| Last pre-change tracked reference | `032fddc91b6d013b98aed8e64ff05b54d1435648` |
| Last reference tree | `463806ef18e5e31006cd4f59e6a5261fc65cea4a` |
| Stage 1/2 Accepted source | `7beda84f59d7b25f49cdf03bdf6efecd771067ed` / tree `a81eea55c1de548a0a1f182f51089eca0b088c82` |
| Official review | [`flagos-ai/vllm-plugin-FL#404`](https://github.com/flagos-ai/vllm-plugin-FL/pull/404) |
| PR/base role | Historical/reference only；future state不作为execution gate |
| Official base | `flagos-ai/vllm-plugin-FL:release/0.2` |
| Base/release HEAD | `ef78dec66fea1ae858ef414584be1478929ee9b2` |
| Base/release tree | `7414bac41c39bc445b0cc05dbdaecc0f08231aeb` |

Stage 6及所有后续A3 execution只核对Frozen source/artifact/runtime/environment/model/workload是否漂移。Later upstream/PR/base movement一律忽略，不STOP、不rebuild、不触发moving-head review。

## Technical Baseline

| Item | Baseline / boundary |
| --- | --- |
| vLLM | `0.20.2` |
| FL | `release/0.2` line；adaptation固定为`e610a990...` Frozen source / Accepted wheel |
| vLLM-Ascend | `0.20.2rc1`，matched-version implementation/oracle reference；不是最终 runtime dependency |
| Frozen accepted image | `quay.io/ascend/vllm-ascend:v0.20.2rc1-a3-openeuler` exact accepted digest/ID；不重新selection |
| Model | `Qwen/Qwen3.6-35B-A3B` |
| Model path/state | `/data/tiankuan/zyg/FL/workspace/Qwen3.6-35B-A3B`；Stage 3 identity Accepted |
| dtype / DP / first TP | BF16 / DP1 / TP2 |
| First model execution | eager |
| Graph | `FULL_DECODE_ONLY`；Stage 4 `[1,2,4,8]` bounded correctness Accepted |
| Required runtime | `VLLM_PLUGINS=fl`、`USE_FLAGGEMS=0`、standalone FL wheel、无 installed `vllm-ascend` |
| Out of first scope | MTP、quantization、CP、FlashComm、MC2、EPLB |

## Required Live Path

```text
VLLM_PLUGINS=fl
  -> PlatformFL
  -> WorkerFL
  -> ModelRunnerFL
  -> FL-local Qwen3.6 patches
  -> GDN / Mamba / Attention / MoE
  -> FL-local Ascend runtime
  -> _C_ascend / CANN OPP / Triton / torch_npu
  -> HCCL
  -> Ascend A3/910C
```

正式 Evidence必须证明 `vllm_fl`来自 wheel/site-packages，不通过源码 `PYTHONPATH`，运行时不 import/依赖 `vllm_ascend`，A3 wheel确实包含并加载 `ascend910_93` family的 `_C_ascend` 与 OPP，且没有复用 A2 binary。

## Stages

| Stage | Goal | Gate |
| --- | --- | --- |
| 0 | Project / baseline establishment | **COMPLETE** |
| 1 | A3 environment + build readiness | **ACCEPTED on `7beda84...`** |
| 2 | A3-native wheel + standalone FL runtime/custom-op smoke | **ACCEPTED on `7beda84...`** |
| 3 | TP2 BF16 eager model correctness | **ACCEPTED** |
| 4 | `FULL_DECODE_ONLY [1,2,4,8]` capture/replay/state correctness | **ACCEPTED — bounded graph correctness** |
| 5 | Serve health/models/completion/chat/repeat/bounded concurrency | **ACCEPTED — bounded service correctness** |
| 6 | A2-equivalent DP1/TP2 `1K/4K/16K/64K x C1/C8/C32/C64, O1024` functional reproduction | **STOP / NOT ACCEPTED**；diagnostic D unresolved |
| Post-functional | 同矩阵performance/capacity | Stage 6 functional 16/16 Accepted后 |
| Specialist | Prefix、EP2及其他专项能力 | 主TP2路线稳定后按价值补齐 |
| Handoff | Runtime freeze / reconstruction / handoff | 所需 A3范围 Accepted后 |

完整关键路径和每阶段证据见 [PLAN.md](docs/qwen36-35b-a3b-a3-flagos/PLAN.md)。

## A2 Evidence Boundary

User资料记录了 2×Ascend 910B1 上的 standalone FL、eager、`FULL_DECODE_ONLY`、prefix、EP2、64K、功能/性能矩阵等完整结果。这些内容统一保存在 [A2-REFERENCE.md](docs/qwen36-35b-a3b-a3-flagos/A2-REFERENCE.md)，状态始终是：

```text
A2 REFERENCE ONLY — NOT A3 ACCEPTANCE
```

任何 A3 PASS/Acceptance必须来自真实 A3/910C execution evidence。

A2 baseline与本项目A3复现状态、环境差异、bounded validation差异及A3-specific source change ledger见 [A2-TO-A3-VALIDATION-DELTA.md](docs/qwen36-35b-a3b-a3-flagos/A2-TO-A3-VALIDATION-DELTA.md)。

## Current Acceptance / Diagnostic Status

- Stage 1/2 Formal Acceptance：[REVIEW-QWEN36-A3-STAGE1-2-ACCEPTANCE-20260826.md](docs/qwen36-35b-a3b-a3-flagos/reviews/REVIEW-QWEN36-A3-STAGE1-2-ACCEPTANCE-20260826.md)
- Stage 3 Formal Acceptance：[REVIEW-QWEN36-A3-STAGE3-TP2-BF16-EAGER-ACCEPTANCE-20260826.md](docs/qwen36-35b-a3b-a3-flagos/reviews/REVIEW-QWEN36-A3-STAGE3-TP2-BF16-EAGER-ACCEPTANCE-20260826.md)
- Stage 4 Formal Acceptance：[REVIEW-QWEN36-A3-STAGE4-FULL-DECODE-ONLY-GRAPH-ACCEPTANCE-20260826.md](docs/qwen36-35b-a3b-a3-flagos/reviews/REVIEW-QWEN36-A3-STAGE4-FULL-DECODE-ONLY-GRAPH-ACCEPTANCE-20260826.md)
- Stage 5 Formal Acceptance：[REVIEW-QWEN36-A3-STAGE5-SERVE-CORRECTNESS-ACCEPTANCE-20260826.md](docs/qwen36-35b-a3b-a3-flagos/reviews/REVIEW-QWEN36-A3-STAGE5-SERVE-CORRECTNESS-ACCEPTANCE-20260826.md)
- Accepted reconstruction：[A3-STAGE1-2-ACCEPTED-RUNTIME.md](docs/qwen36-35b-a3b-a3-flagos/reconstruction/A3-STAGE1-2-ACCEPTED-RUNTIME.md)
- Stage 6 STOP Review：[REVIEW-QWEN36-A3-STAGE6-STOP-20260826.md](docs/qwen36-35b-a3b-a3-flagos/reviews/REVIEW-QWEN36-A3-STAGE6-STOP-20260826.md)
- U+FFFD diagnostic Result：[RESULT-QWEN36-A3-S6-UFFFD-OUTPUT-CHAIN-DIAGNOSTIC-20260827T101217+0800.md](docs/qwen36-35b-a3b-a3-flagos/results/RESULT-QWEN36-A3-S6-UFFFD-OUTPUT-CHAIN-DIAGNOSTIC-20260827T101217+0800.md)
- U+FFFD diagnostic Formal Review：[REVIEW-QWEN36-A3-S6-UFFFD-OUTPUT-CHAIN-DIAGNOSTIC-20260827.md](docs/qwen36-35b-a3b-a3-flagos/reviews/REVIEW-QWEN36-A3-S6-UFFFD-OUTPUT-CHAIN-DIAGNOSTIC-20260827.md)
- New-server Result：[RESULT-QWEN36-A3-S6-UFFFD-PROSPECTIVE-ROOT-CAUSE-DIAGNOSTIC-NEW-SERVER-20260827T173022+0800.md](docs/qwen36-35b-a3b-a3-flagos/results/RESULT-QWEN36-A3-S6-UFFFD-PROSPECTIVE-ROOT-CAUSE-DIAGNOSTIC-NEW-SERVER-20260827T173022+0800.md)
- New-server Formal Review：[REVIEW-QWEN36-A3-S6-UFFFD-PROSPECTIVE-ROOT-CAUSE-DIAGNOSTIC-NEW-SERVER-20260827.md](docs/qwen36-35b-a3b-a3-flagos/reviews/REVIEW-QWEN36-A3-S6-UFFFD-PROSPECTIVE-ROOT-CAUSE-DIAGNOSTIC-NEW-SERVER-20260827.md)
- Next Task：[QWEN36-A3-S6-JEMALLOC-RECONSTRUCTION-AND-UFFFD-ROOT-CAUSE-DIAGNOSTIC.md](docs/qwen36-35b-a3b-a3-flagos/tasks/QWEN36-A3-S6-JEMALLOC-RECONSTRUCTION-AND-UFFFD-ROOT-CAUSE-DIAGNOSTIC.md)；Awaiting explicit User dispatch；performance/prefix/EP2 remain locked.
- New-session Prompt：[CODEX2-QWEN36-A3-S6-JEMALLOC-RECONSTRUCTION-AND-UFFFD-ROOT-CAUSE-DIAGNOSTIC-PROMPT.md](docs/qwen36-35b-a3b-a3-flagos/tasks/CODEX2-QWEN36-A3-S6-JEMALLOC-RECONSTRUCTION-AND-UFFFD-ROOT-CAUSE-DIAGNOSTIC-PROMPT.md)

## What Is Not Done

- 尚未完成A2-equivalent 16-cell functional matrix、capacity或performance。
- 尚未复现final service automatic graph capture through 64、chunked prefill、async scheduling、prefix或EP2。
- Stage 5 Acceptance只覆盖`[1,2,4,8]` bounded service correctness，不外推为A2 final service graph matrix。

## Navigation

| Question | Source of truth |
| --- | --- |
| 当前状态、门禁、User待确认输入 | [STATUS.md](docs/qwen36-35b-a3b-a3-flagos/STATUS.md) |
| 技术/治理决策 | [DECISIONS.md](docs/qwen36-35b-a3b-a3-flagos/DECISIONS.md) |
| Stage与关键路径 | [PLAN.md](docs/qwen36-35b-a3b-a3-flagos/PLAN.md) |
| Frozen source/tree/wheel、historical reference、runtime baseline | [BASELINE.md](docs/qwen36-35b-a3b-a3-flagos/BASELINE.md) |
| A2 oracle/reference | [A2-REFERENCE.md](docs/qwen36-35b-a3b-a3-flagos/A2-REFERENCE.md) |
| A2 → A3差异与复现状态 | [A2-TO-A3-VALIDATION-DELTA.md](docs/qwen36-35b-a3b-a3-flagos/A2-TO-A3-VALIDATION-DELTA.md) |
| 未来 GLM可继承/不可外推边界 | [A3-RUNTIME-HANDOFF.md](docs/qwen36-35b-a3b-a3-flagos/A3-RUNTIME-HANDOFF.md) |
| Task/Evidence/Result/Acceptance规则 | [REPOSITORY-AND-EVIDENCE-RULES.md](docs/qwen36-35b-a3b-a3-flagos/REPOSITORY-AND-EVIDENCE-RULES.md) |
| Historical GitHub/source snapshot | [CURRENT-IMPLEMENTATION-STATE.md](docs/qwen36-35b-a3b-a3-flagos/research/CURRENT-IMPLEMENTATION-STATE.md) |
| Result与Acceptance索引 | [results/INDEX.md](docs/qwen36-35b-a3b-a3-flagos/results/INDEX.md) |
