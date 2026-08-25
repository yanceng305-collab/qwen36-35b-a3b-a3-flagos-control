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
- [tasks/QWEN36-A3-S1S2-ENV-BUILD-RUNTIME.md](tasks/QWEN36-A3-S1S2-ENV-BUILD-RUNTIME.md)：首个 Codex2 Task，**Waiting User input / Not Ready**。
- [tasks/CODEX2-QWEN36-A3-S1S2-ENV-BUILD-RUNTIME-PROMPT.md](tasks/CODEX2-QWEN36-A3-S1S2-ENV-BUILD-RUNTIME-PROMPT.md)：简洁 dispatch prompt。
- [results/INDEX.md](results/INDEX.md)：Execution Result、Control Sync、Codex1 Acceptance索引；当前无 A3 run。

## Research snapshots

- [research/CURRENT-IMPLEMENTATION-STATE.md](research/CURRENT-IMPLEMENTATION-STATE.md)：2026-08-25 live PR/branch/source核查；dispatch前重查。
- [research/SOURCE-MATERIALS.md](research/SOURCE-MATERIALS.md)：三份 User资料的 hash、作用和解释边界。

## Current Stage

Stage 0 Control已建立。Stage 1/2等待 User确认 A3 execution target、device、container、work/Evidence/artifact-cache、dependency access和正式 dispatch；在此之前不得执行。
