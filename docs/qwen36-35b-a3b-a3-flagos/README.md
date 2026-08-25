# Qwen3.6 A3 Validation Documentation Index

## Current control

- [STATUS.md](STATUS.md)：current state、current implementation snapshot、Ready缺口。
- [DECISIONS.md](DECISIONS.md)：正式技术/治理决策与 rejected routes。
- [PLAN.md](PLAN.md)：Stage 0–8、关键路径和风险。
- [BASELINE.md](BASELINE.md)：moving branch、exact SHA/tree、version/environment contract。

## Reference and handoff

- [A2-REFERENCE.md](A2-REFERENCE.md)：**A2 REFERENCE ONLY — NOT A3 ACCEPTANCE**。
- [A3-RUNTIME-HANDOFF.md](A3-RUNTIME-HANDOFF.md)：未来 GLM可继承/不可外推边界；当前 A3未验证。
- [reconstruction/README.md](reconstruction/README.md)：A3 runtime reconstruction要求；当前无 validated entry。

## Execution governance

- [REPOSITORY-AND-EVIDENCE-RULES.md](REPOSITORY-AND-EVIDENCE-RULES.md)：三指针、immutable Result、Acceptance、Code fix规则。
- [tasks/QWEN36-A3-S1S2-ENV-BUILD-RUNTIME.md](tasks/QWEN36-A3-S1S2-ENV-BUILD-RUNTIME.md)：parent Task，Gate B STOP。
- [results/RESULT-QWEN36-A3-S1S2-ENV-BUILD-RUNTIME-20260825T205424+0800.md](results/RESULT-QWEN36-A3-S1S2-ENV-BUILD-RUNTIME-20260825T205424+0800.md)：immutable STOP Result。
- [reviews/REVIEW-QWEN36-A3-S1S2-ENV-BUILD-RUNTIME-20260825.md](reviews/REVIEW-QWEN36-A3-S1S2-ENV-BUILD-RUNTIME-20260825.md)：Codex1 Formal Review，**NEEDS-FOLLOWUP**。
- [tasks/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG.md](tasks/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG.md)：bounded follow-up，**READY / Awaiting explicit User dispatch**。
- [tasks/CODEX2-QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG-PROMPT.md](tasks/CODEX2-QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG-PROMPT.md)：self-contained follow-up dispatch prompt。
- [results/INDEX.md](results/INDEX.md)：Execution Result、Control Sync、Codex1 Review/Acceptance索引。

## Research snapshots

- [research/CURRENT-IMPLEMENTATION-STATE.md](research/CURRENT-IMPLEMENTATION-STATE.md)：2026-08-25 live PR/branch/source核查；dispatch前重查。
- [research/OFFICIAL-A3-IMAGE-ROUTE.md](research/OFFICIAL-A3-IMAGE-ROUTE.md)：official v0.20.2rc1 A3 Ubuntu/openEuler image matrix、build definition和 bounded-selection边界。
- [research/SOURCE-MATERIALS.md](research/SOURCE-MATERIALS.md)：三份 User资料的 hash、作用和解释边界。

## Current Stage

Parent Stage 1/2 Task已在 Gate B STOP；Formal Review为 `NEEDS-FOLLOWUP`，Stage 3 locked。下一步只允许 User显式 dispatch bounded Gate B diagnostic；Codex2当前不得自动续跑。
