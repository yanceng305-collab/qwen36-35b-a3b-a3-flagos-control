# RESULT-QWEN36-A3-S6-UFFFD-PROSPECTIVE-ROOT-CAUSE-DIAGNOSTIC

This document reproduces the frozen validation baseline:
`e610a990d785356bf51a3cad50219d4c03310a31`
tree: `609ff1ad0f08239f353cb4d8774e504b4deba03b`
wheel SHA-256: `2fcf788660f3fe42b364bc60d593ee1b9b634fc0632de58c444d961bff4aa1bd`

- Task/run: `QWEN36-A3-S6-UFFFD-PROSPECTIVE-ROOT-CAUSE-DIAGNOSTIC` / `20260827T113500+0800`
- Dispatch: explicit User dispatch to execute only this Task, complete Result and Control sync, then stop.
- Experiment Result: **DIAGNOSTIC STOP — D / UNRESOLVED**
- Earliest layer: not located. No generation was authorized after the mandatory runtime invariant failed.

## Identity and provenance

- Control live-main query: GitHub API head `1c8335e2246b73b28da8e9fba4cc55bb6eb55e`.
- Accepted image: `quay.io/ascend/vllm-ascend:v0.20.2rc1-a3-openeuler`; preserved container `qw36-a3-s2-gatec-priv-20260826T092617p0800` / `32562c7139600c25e570ec07841713737b0407c7d1bbdc563be41b87ea105f0a`.
- Pinned skill: `long-context-orchestrator@0bb8a5eda9c46f1b170552ba41b871ba141e04b6`; load path `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S6-UFFFD-PROSPECTIVE-ROOT-CAUSE-DIAGNOSTIC/work/long-context-orchestrator`; `SKILL.md` SHA-256 `f4e15f8c5f097d6aec0fe42b58e1cb5386d3104a02b5ebb502e8f7b0041af1f2` (Control Git blob `41373033056391e3c965120baf7c5861e88fddef`).
- Code/source pointer: `xiemingda-1002/vllm-plugin-FL@e610a990d785356bf51a3cad50219d4c03310a31`; Code PR=`N/A`.
- Control pointer: this Result and `results/INDEX.md`.
- Evidence pointer: `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S6-UFFFD-PROSPECTIVE-ROOT-CAUSE-DIAGNOSTIC/evidence`.

## Execution and STOP

The accepted container was present and mapped only NPU 0/1. The required handoff invariant probe (`import torch_npu`, `torch.npu.is_available()==True`, device count 2) was run with a bounded 20-second timeout and exited `124` without output. This is an unavailable/unproven runtime invariant, so the handoff requires STOP before FL/model/service diagnosis. No production package, source, wheel, model, image, runtime, or parameters were changed.

- Service launches: `0/3`
- Target C64 cells: `0/4`
- Prehistory sequences: `0/2`
- Total cells/requests: `0/10`, `0/338`
- Instrumentation bundle: v0, not activated; corrections `0/1`
- U+FFFD: not observed in this run; chain incomplete because workload did not start.
- Last successful step: Control sync/document review and container/device mapping inspection.
- First blocker: mandatory NPU runtime invariant probe timeout.

## Classification and decision request

Classification is **D — unresolved**. The parent blocker (request index 34 with 29 U+FFFD) remains preserved and is neither denied nor newly attributed. No A/B/C classification is possible without a complete request-linked chain. Codex1 decision requested: review the runtime probe timeout and authorize a future retry only after a confirmed healthy, task-owned A3 runtime invariant.

## Acceptance boundary

Codex1 Acceptance: **PENDING**. This diagnostic does not produce Stage 6 PASS, does not resume Stage 6, and does not create a next Task.
