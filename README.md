# Qwen3.6-35B-A3B × FlagOS × Ascend A3/910C Validation Control

本仓库是 Qwen3.6-35B-A3B A3 Validation Control Project 的正式 truth source。它管理目标、moving implementation、exact execution identity、Stage gate、Task、Evidence、immutable Result、Codex1 Acceptance 与 A3 Runtime Handoff。

## Project Goal

> 在 Ascend A3/910C 上验证当前 PR #404 / Qwen3.6-35B-A3B Ascend adaptation，在 vLLM 0.20.2 + FL release/0.2 系上完成 A3-native wheel build、standalone FL runtime、TP2 eager、FULL_DECODE_ONLY graph、serve 及必要功能验证，并形成可供未来 GLM-5.2-W8A8 复用的 A3 runtime / container / build / reconstruction 基础。

本项目不是重新开发一个 Qwen3.6 fork；当前路线是 `VERIFY FIRST / FIX ONLY WHEN EVIDENCE REQUIRES IT`。

## Current State

更新时间：2026-08-26

- Stage 0 Control / baseline establishment：**COMPLETE**。
- A3 Stage 1/2、Stage 3 TP2 BF16 eager、Stage 4 bounded `FULL_DECODE_ONLY [1,2,4,8]`及Stage 5 bounded serve correctness：**ACCEPTED**。
- Current tracked implementation：`032fddc9...` / tree `463806ef...`；docs/tests-only moving-head disposition。Runtime artifact仍为`e610a990...` Accepted wheel。
- Next Task：`QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX`，**READY / Awaiting explicit User dispatch**。
- Validation Code repo/fork：**Not needed**。
- GLM-5.2-W8A8项目：由 User Decision 暂停；本仓库不接收 GLM Result。

## Tracked Implementation

GitHub状态核验时间：2026-08-26 Stage 5 Formal Acceptance。移动状态在 dispatch 前必须重新查询。

| Field | Current verified value |
| --- | --- |
| Implementation repo | [`xiemingda-1002/vllm-plugin-FL`](https://github.com/xiemingda-1002/vllm-plugin-FL) |
| Tracked branch | `feature/qwen3.6-35b-a3b-ascend-graph-migration` |
| Current head | `032fddc91b6d013b98aed8e64ff05b54d1435648` |
| Current head tree | `463806ef18e5e31006cd4f59e6a5261fc65cea4a` |
| Stage 1/2 Accepted source | `7beda84f59d7b25f49cdf03bdf6efecd771067ed` / tree `a81eea55c1de548a0a1f182f51089eca0b088c82` |
| Official review | [`flagos-ai/vllm-plugin-FL#404`](https://github.com/flagos-ai/vllm-plugin-FL/pull/404) |
| PR state | `OPEN / DRAFT`；Stage 5 Formal Review live state `CONFLICTING / DIRTY`；dispatch前重查 |
| Official base | `flagos-ai/vllm-plugin-FL:release/0.2` |
| Base/release HEAD | `ef78dec66fea1ae858ef414584be1478929ee9b2` |
| Base/release tree | `7414bac41c39bc445b0cc05dbdaecc0f08231aeb` |

Tracked branch和official base均为moving facts；Stage 6 dispatch前重新查询。任何新HEAD不自动继承历史Acceptance，按diff决定最小regression。

## Technical Baseline

| Item | Baseline / boundary |
| --- | --- |
| vLLM | `0.20.2` |
| FL | `release/0.2` line；adaptation以 PR #404 current tracked head为准 |
| vLLM-Ascend | `0.20.2rc1`，matched-version implementation/oracle reference；不是最终 runtime dependency |
| Stage 1/2 base candidates | `quay.io/ascend/vllm-ascend:v0.20.2rc1-a3` / `...:v0.20.2rc1-a3-openeuler`；only bounded selection |
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
| 6 | A2-equivalent DP1/TP2 `1K/4K/16K/64K x C1/C8/C32/C64, O1024` functional reproduction | **READY / Awaiting explicit User dispatch** |
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

## Current Acceptance / Next Task

- Stage 1/2 Formal Acceptance：[REVIEW-QWEN36-A3-STAGE1-2-ACCEPTANCE-20260826.md](docs/qwen36-35b-a3b-a3-flagos/reviews/REVIEW-QWEN36-A3-STAGE1-2-ACCEPTANCE-20260826.md)
- Stage 3 Formal Acceptance：[REVIEW-QWEN36-A3-STAGE3-TP2-BF16-EAGER-ACCEPTANCE-20260826.md](docs/qwen36-35b-a3b-a3-flagos/reviews/REVIEW-QWEN36-A3-STAGE3-TP2-BF16-EAGER-ACCEPTANCE-20260826.md)
- Stage 4 Formal Acceptance：[REVIEW-QWEN36-A3-STAGE4-FULL-DECODE-ONLY-GRAPH-ACCEPTANCE-20260826.md](docs/qwen36-35b-a3b-a3-flagos/reviews/REVIEW-QWEN36-A3-STAGE4-FULL-DECODE-ONLY-GRAPH-ACCEPTANCE-20260826.md)
- Stage 5 Formal Acceptance：[REVIEW-QWEN36-A3-STAGE5-SERVE-CORRECTNESS-ACCEPTANCE-20260826.md](docs/qwen36-35b-a3b-a3-flagos/reviews/REVIEW-QWEN36-A3-STAGE5-SERVE-CORRECTNESS-ACCEPTANCE-20260826.md)
- Accepted reconstruction：[A3-STAGE1-2-ACCEPTED-RUNTIME.md](docs/qwen36-35b-a3b-a3-flagos/reconstruction/A3-STAGE1-2-ACCEPTED-RUNTIME.md)
- Next Task：[QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX.md](docs/qwen36-35b-a3b-a3-flagos/tasks/QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX.md)
- Next prompt：[CODEX2-QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX-PROMPT.md](docs/qwen36-35b-a3b-a3-flagos/tasks/CODEX2-QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX-PROMPT.md)

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
| 版本、分支、SHA/tree、baseline | [BASELINE.md](docs/qwen36-35b-a3b-a3-flagos/BASELINE.md) |
| A2 oracle/reference | [A2-REFERENCE.md](docs/qwen36-35b-a3b-a3-flagos/A2-REFERENCE.md) |
| A2 → A3差异与复现状态 | [A2-TO-A3-VALIDATION-DELTA.md](docs/qwen36-35b-a3b-a3-flagos/A2-TO-A3-VALIDATION-DELTA.md) |
| 未来 GLM可继承/不可外推边界 | [A3-RUNTIME-HANDOFF.md](docs/qwen36-35b-a3b-a3-flagos/A3-RUNTIME-HANDOFF.md) |
| Task/Evidence/Result/Acceptance规则 | [REPOSITORY-AND-EVIDENCE-RULES.md](docs/qwen36-35b-a3b-a3-flagos/REPOSITORY-AND-EVIDENCE-RULES.md) |
| 当前 GitHub/source核查 | [CURRENT-IMPLEMENTATION-STATE.md](docs/qwen36-35b-a3b-a3-flagos/research/CURRENT-IMPLEMENTATION-STATE.md) |
| Result与Acceptance索引 | [results/INDEX.md](docs/qwen36-35b-a3b-a3-flagos/results/INDEX.md) |
