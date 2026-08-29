# Codex1 Formal Review — Provenance correlation rerun STOP

Review date: 2026-08-29

Immutable Result: [`RESULT-QWEN36-A3-S6-PROVENANCE-CORRELATION-AND-FUNCTIONAL-MATRIX-RERUN-20260829T120000+0800.md`](../results/RESULT-QWEN36-A3-S6-PROVENANCE-CORRELATION-AND-FUNCTIONAL-MATRIX-RERUN-20260829T120000+0800.md)

Control commit containing Result: `9021242a33b7e47609249c513cf6278b1ffe37bc`

Disposition: **ACCEPTED — valid auditable STOP, scope-limited; Evidence detail gap remains**

Stage 6: **STOP / NOT ACCEPTED**

## Review basis

This review is against live Control `main` at `9021242a33b7e47609249c513cf6278b1ffe37bc`, D-030, D-034, D-036, the exact Task, repository evidence rules, the prior unsynced D-035 note, the 20260829 mapping supplemental record, and prior provenance findings. The immutable Result is not modified.

The Result is concise and points to server Evidence at `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S6-PROVENANCE-CORRELATION-AND-FUNCTIONAL-MATRIX-RERUN/evidence/20260829T120000+0800`. Codex1 did not access A3 in this review, so claims not shown in the repository Result are not independently elevated beyond that pointer.

## Accepted execution facts

- Frozen identity is recorded as source `e610a990d785356bf51a3cad50219d4c03310a31`, tree `609ff1ad0f08239f353cb4d8774e504b4deba03b`, and wheel SHA-256 `2fcf788660f3fe42b364bc60d593ee1b9b634fc0632de58c444d961bff4aa1bd`.
- D-036 device mapping is recorded for this run as container index 0 → host 14 and container index 1 → host 15; no mapping correction was required.
- The bounded provenance audit did not prove a deterministic reversible or uniquely attributable, collision-free transform across the required request-ID layers. The Result correctly classifies this as `UNRESOLVED_PROVENANCE`.
- No fuzzy matching was used to claim correlation. Neither `TOKENIZER_NATIVE_UFFFD` nor `POST_TOKENIZER_CORRUPTION` was claimed. No model, FL, NPUGraph, or NPU corruption conclusion was claimed.
- F0 was not entered; zero O8 and zero O1024 cells were run; no historical or supplemental cell was reused as formal progress.
- The STOP boundary and Task-owned cleanup were respected according to the immutable Result summary.

## Evidence gaps and non-claims

The Result does not include detailed source locations, raw ID mapping tables, transform proof, collision logs, or detailed cleanup artifacts in the repository. Those remain server-Evidence-pointer claims only. This review does not infer them. In particular, the exact missing link among bare request IDs, internal vLLM IDs, OpenAI completion IDs, `cmpl-<request-id>`, `cmpl-<request-id>-0`, SSE/raw HTTP, client, saved result, and validator remains unproven.

The Result does not establish any U+FFFD provenance classification because no workload was run. Historical U+FFFD observations must not be reinterpreted as evidence for this run.

## Acceptance boundary

The STOP itself is contract-compliant and auditable within the concise Result scope. It is accepted only as a valid STOP record. It is not Stage 6 PASS, not F0 PASS, and not functional matrix progress.

The executed Task is **ENDED / STOP — UNRESOLVED_PROVENANCE — DO NOT RESUME**. The shortest next route is a bounded exact Frozen source/runtime provenance audit, optionally with minimal Task/Evidence-local proof, followed directly by a fresh full Stage 6 rerun if and only if provenance is proven. Do not create a separate device-mapping research Task unless contradictory device Evidence appears.
