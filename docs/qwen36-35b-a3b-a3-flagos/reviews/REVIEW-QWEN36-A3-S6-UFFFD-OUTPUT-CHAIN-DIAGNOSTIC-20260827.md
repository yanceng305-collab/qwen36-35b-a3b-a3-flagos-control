# Codex1 Formal Review — Qwen3.6 A3 U+FFFD Output-Chain Diagnostic

Review date：2026-08-27

Disposition：**FORMALLY REVIEWED — valid, preservable, auditable DIAGNOSTIC STOP D record；Codex1 NEEDS-FOLLOWUP；Stage 6 remains STOP / NOT ACCEPTED**

This Review validates the bounded diagnostic record and its `D / UNRESOLVED` conclusion. It is not Stage 6 Acceptance and does not establish an A/B/C root cause.

## Live Control provenance

Codex1 live-queried GitHub before review and did not rely on the User-supplied SHA or chat history.

| Field | Live-reviewed identity |
| --- | --- |
| Control repo | `https://github.com/yanceng305-collab/qwen36-35b-a3b-a3-flagos-control` |
| Live `main` before review | `9e66090c7d15692213e69e5b6d5f21e430a66e09` / tree `26da69e934811e7477f1b5bc2152ece44ee2fddd` |
| Direct parent | `09830051a442eb648bc665a3e03a450b21223ce2` / tree `2b270e97c019abfa077fcf1eaded36fb7c4c3d48` |
| Commit message | `Record Qwen A3 U+FFFD output-chain diagnostic STOP` |
| Immutable Result blob | `9d7b54cb5f765e81418d9ead964b9e9ce58ede74` |
| Executed Task blob | `e0c09da3d8cc93b94a94896195280d6ea89a22a7` |

The GitHub blobs matched the review checkout. Codex1 did not access or operate A3, did not open or re-hash server-resident Evidence, did not execute Phase B, and did not inspect moving upstream state.

## Reviewed records

- Executed Task：[`QWEN36-A3-S6-UFFFD-OUTPUT-CHAIN-DIAGNOSTIC`](../tasks/QWEN36-A3-S6-UFFFD-OUTPUT-CHAIN-DIAGNOSTIC.md)
- Immutable Result：[`RESULT-QWEN36-A3-S6-UFFFD-OUTPUT-CHAIN-DIAGNOSTIC-20260827T101217+0800.md`](../results/RESULT-QWEN36-A3-S6-UFFFD-OUTPUT-CHAIN-DIAGNOSTIC-20260827T101217+0800.md)
- Parent Stage 6 Review：[`REVIEW-QWEN36-A3-STAGE6-STOP-20260826.md`](REVIEW-QWEN36-A3-STAGE6-STOP-20260826.md)
- Diagnostic Evidence root：`/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S6-UFFFD-OUTPUT-CHAIN-DIAGNOSTIC/evidence/20260827T101217p0800`
- Parent Evidence root：`/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX/evidence/20260826T180105p0800`
- Named diagnostic artifacts：`manifest.md`, `layer_audit.md`, `tokenizer_audit.txt`, `current_status.md`, `checksums.txt`, `parent/`

## D / UNRESOLVED verdict

**The Result's D classification is valid.**

The parent blocker remains exactly:

```text
cell: I1024 / C64 / O8
dataset seed: 1088
request index: 34
request ID: s6-i1024-c64-o8-34
saved output U+FFFD count: 29
validator reason: unicode_replacement
```

The Result proves the blocker at the saved-result/validator layer. It also records that the following earlier layers are absent from parent Evidence:

- original generated token IDs / token representation;
- tokenizer decoder state at generation time;
- serving response object;
- raw HTTP response bytes before parsing;
- parsed OpenAI response artifact;
- benchmark client in-memory text and reconstruction intermediates.

Therefore the earliest **evidenced** U+FFFD layer is the saved benchmark result, not necessarily the earliest actual layer. A/B/C require a correlated earlier chain and exclusion of competing earlier hypotheses; that condition is not met.

The tokenizer audit shows only that text already containing 29 U+FFFD is stable through the recorded encode/decode/re-encode operation. The 1032 derived IDs are not the original sampled output token sequence and cannot establish tokenizer-native or service/runtime attribution.

Underlying root-cause confidence remains：**LOW / NOT CONFIRMED / UNRESOLVED**.

## Phase B disposition

Phase B `NOT RUN` is contract-compliant and is not an execution-control deviation. The executed Task permitted, but did not require, at most one conditional faithful cell after Phase A. The immutable Result correctly reports no service launch, no workload run, no Stage 6 resume and no PASS claim.

Because `temperature=1` and `enable_global_stream_random_sample=true` were active, a new cell could only be a faithful configuration/workload reproduction, not exact sampled-token replay. That limitation does not invalidate D and does not negate the parent blocker.

## Result quality and evidence boundary

The Result is sufficiently identity-linked, self-bounded and candid about missing layers to preserve as an immutable diagnostic record. The missing chain is its substantive finding, not grounds to reject the record.

The Result also materially supplements the parent runtime instrumentation record with file, SHA-256, load path, patched areas, purpose and bounded impact. It confirms that this read-only diagnostic added no instrumentation and launched no service.

Control-only review cannot independently certify the contents/hashes of A3-resident artifacts. The Result does not enumerate every reconstruction detail, including the benchmark client hash/save path, validator hash/exact input/command, exact sync SHA self-reference, full arm64/device/model/command/cache tuple. Those gaps limit future attribution but do not invalidate D.

## Current Stage 6 disposition

- Diagnostic Result：**FORMALLY REVIEWED / NEEDS-FOLLOWUP**.
- Stage 6：**STOP / NOT ACCEPTED**.
- Formal Stage 6 boundary：through parent `I1024/C64/O8` failure.
- Last successful formal cell：`I1024/C32/O8`.
- Parent saved-result/validator blocker confidence：**HIGH**.
- Underlying cause：**D / UNRESOLVED**.
- Performance/capacity, prefix lifecycle, EP2, O1024, full matrix and later stages：**LOCKED**.

The executed evidence-only diagnostic is ended and must not be resumed. Another read-only Evidence audit would repeat the already-proven insufficiency and is not authorized.

## New User decision and only next Task

The User now explicitly authorizes one new prospective instrumentation and bounded reproduction Task on A3, without changing the frozen baseline. The only Ready Task is:

[`QWEN36-A3-S6-UFFFD-PROSPECTIVE-ROOT-CAUSE-DIAGNOSTIC`](../tasks/QWEN36-A3-S6-UFFFD-PROSPECTIVE-ROOT-CAUSE-DIAGNOSTIC.md)

Its frozen maximum is:

```text
service launches:           <= 3
target I1024/C64/O8 cells:  <= 4
same-service prehistory:    <= 2 sequences of C1 -> C8 -> C32 -> C64
total workload cells:       <= 10
total generation requests:  <= 338
output length:              exactly O8
instrumentation correction: <= 1 bundle correction for the whole Task
```

The first complete U+FFFD-bearing chain that can locate the earliest changed layer cancels all unused workload budget. Non-reproduction, incomplete capture, invariant drift, unsafe capture or exhausted budget remains D and cannot deny the parent blocker.

The new Task cannot itself produce Stage 6 PASS and does not authorize production source modification, wheel rebuild, upstream switch, parameter changes, O1024, performance, prefix lifecycle or EP2.

## Pinned long-context helper

The reviewed safety change has been fast-forward merged:

```text
repo: https://github.com/yanceng305-collab/long-context-orchestrator
merged commit: 0bb8a5eda9c46f1b170552ba41b871ba141e04b6
SKILL.md git blob: 41373033056391e3c965120baf7c5861e88fddef
```

The new Task and prompt pin this exact commit. Codex2 must use a Task-owned/authorized checkout or load path and record its provenance. The skill manages durable context only; the formal Task, explicit User dispatch, PASS/STOP gates and Evidence contract take precedence. The skill and subagents cannot expand execution authority, and durable notes do not replace formal Evidence, immutable Result or Control sync.

ChatGPT did not modify any locally installed skill while merging the repository commit.

## Final conclusion

`RESULT-QWEN36-A3-S6-UFFFD-OUTPUT-CHAIN-DIAGNOSTIC-20260827T101217+0800`：**FORMALLY REVIEWED — valid DIAGNOSTIC STOP D record / Codex1 NEEDS-FOLLOWUP**.

Stage 6：**STOP / NOT ACCEPTED**.

Only the new prospective root-cause diagnostic may be dispatched by the User. This Review does not dispatch Codex2.
