# Codex1 Formal Review — Qwen3.6 A3 Stage 6 STOP

Review date：2026-08-26

Disposition：**FORMALLY REVIEWED — valid auditable STOP record preserved；Stage 6 NOT ACCEPTED；NEEDS-FOLLOWUP**

This is a STOP Formal Review, not a Stage 6 Formal Acceptance.

## Formal answers

### A. Is the Codex2 Result a valid execution record?

**YES — preserve it as an immutable, auditable STOP execution record.** It identifies the frozen source/artifact/runtime, run, commands and Evidence roots; reports F0 PASS, F1 STOP, no Stage 6 PASS; discloses that O1024 ran after F1 STOP; preserves the first blocker and keeps underlying-cause confidence low.

The record has execution-control and reconstruction gaps listed below. Those gaps are supplemented by this Review, `STATUS.md`, `results/INDEX.md` and the bounded follow-up Task. The immutable Result is not modified.

### B. Is Stage 6 accepted?

**NO — Stage 6 remains STOP / NOT ACCEPTED.** The frozen O8 validator contract was violated at `I1024 / C64 / O8`, so O8 did not reach 16/16 and Gate F2 was not authorized to start. Performance/capacity, prefix lifecycle, EP2 and all later stages remain locked.

### C. What is the one minimum follow-up?

Only [`QWEN36-A3-S6-UFFFD-OUTPUT-CHAIN-DIAGNOSTIC`](../tasks/QWEN36-A3-S6-UFFFD-OUTPUT-CHAIN-DIAGNOSTIC.md) is Ready. It must first audit the already-preserved Evidence for the exact failing request. Only if a required raw layer is absent may it run at most one faithful `I1024 / C64 / O8` diagnostic cell under the unchanged frozen contract. It cannot produce Stage 6 PASS and does not authorize source change or a full-matrix rerun.

## Live Control provenance

Codex1 live-queried GitHub before this review and did not rely on the prompt's supplied HEAD.

| Field | Live-reviewed identity |
| --- | --- |
| Control repo | `https://github.com/yanceng305-collab/qwen36-35b-a3b-a3-flagos-control` |
| Live `main` before review | `add960850a15a7f72ab89aac8ab050da1dbf0c0a` / tree `d725b841ccdb20c9061ae7a12315bf0e27e9428f` |
| Commit message | `Record Qwen A3 Stage 6 functional matrix STOP` |
| Direct parent | `228891e57b4050a9d68d842216a858eaeec3e006` / tree `710615ea913b05573ec204ff2b5dd86ddd6946b4` |
| Result blob | `e0d98264c5836c6b8b54137d1fd7b1c8f8ada4da` |
| Frozen Task blob | `9e9a0e292f30ac3ca861ff02522bf57101e56803` |
| Decision | `D-030 / FROZEN-UPSTREAM-VALIDATION-BASELINE` remains controlling |

The Result-adding commit is the direct child of the D-030 baseline commit and changes only `STATUS.md`, `results/INDEX.md`, and the new immutable Result. GitHub blobs matched the local review copy.

Codex1 did not access or operate the A3 server, did not rerun workload, and did not inspect later feature-branch, PR #404 or official-base movement. Review statements about raw A3 behavior are bounded to the immutable Result and its preserved Evidence pointers.

## Reviewed records

- Frozen Task：[`QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX.md`](../tasks/QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX.md)
- Immutable Result：[`RESULT-QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX-20260826T180105+0800.md`](../results/RESULT-QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX-20260826T180105+0800.md)
- Original Evidence root：`/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX/evidence/20260826T180105p0800`
- Main server log：`runtime/server.log`
- Functional summaries：`functional/cell_summary.csv`, `functional/summary_o8.json`, `functional/summary_o1024.json`
- First-blocker artifacts：`functional/i1024_c64_o8/result.json`, `functional/i1024_c64_o8/prompt_manifest.json`
- Graph summary：`runtime/graph_event_summary.json`
- Manifest/checksums：`manifest.md`, `checksums.txt`

## Frozen identity and drift review

| Field | Reviewed disposition |
| --- | --- |
| Source/tree | `e610a990d785356bf51a3cad50219d4c03310a31` / `609ff1ad0f08239f353cb4d8774e504b4deba03b` — aligned |
| Wheel | `vllm_plugin_fl-0.2.0+ge610a990d-cp311-cp311-linux_aarch64.whl`, SHA-256 `2fcf788660f3fe42b364bc60d593ee1b9b634fc0632de58c444d961bff4aa1bd` — aligned |
| Historical reference | `032fddc91b6d013b98aed8e64ff05b54d1435648` / tree `463806ef18e5e31006cd4f59e6a5261fc65cea4a` — reference only, not wheel source |
| Image | `quay.io/ascend/vllm-ascend:v0.20.2rc1-a3-openeuler`; manifest `sha256:442363921166771eb82baeec9c1ac0381f46fb830ead8d0e072df6e925f2a958`; accepted arm64 digest `sha256:a55b2b0597f9fdd1882de9bf3b7ebc395dd77c1ca49f251d0cb759d7b2c1a807`; local ID `sha256:8e7823878349b37d9900984555e28381c25681f88ff678cb4a86f7d1674a67c1` |
| Container | `qw36-a3-s2-gatec-priv-20260826T092617p0800` / `32562c7139600c25e570ec07841713737b0407c7d1bbdc563be41b87ea105f0a` — aligned |
| Device/model | two `Ascend910_9382`, visible `0,1`, `SOC_VERSION=ascend910_93`; 26/26 shards, 1045 BF16 tensors, non-quantized — aligned |

**Drift verdict：no frozen production source/tree/wheel/image/container/model drift is evidenced.** Post-STOP execution does not itself change those identities.

The Result also discloses an Evidence-local `PYTHONPATH` runtime instrumentation overlay but does not identify its exact file/hash/behavior/reason/impact. That is not evidence of a production source or wheel change, but it is an incompletely specified harness/effective-runtime deviation and a possible diagnostic hypothesis. Therefore this Review does not make an unqualified byte-for-byte whole-effective-runtime no-drift claim.

The Result's `vLLM 0.20.2+empty`, `triton 3.5.0` and `triton-ascend 3.2.1` notation is not affirmative drift from the accepted runtime tuple; it reflects package/module notation already present in Control. The follow-up must preserve exact distribution/module/provider identities.

## Gate and formal execution-boundary review

### Gate F0

**PASS at the preserved Control-record level.** The Result records source/wheel/model/runtime continuity, standalone FL/site-packages ownership, no `vllm-ascend`, no FlagGems with `USE_FLAGGEMS=0`, health/models HTTP 200, required service configuration, automatic capture through 64 on both TP workers, and Evidence pointers.

Raw F0 artifacts were not re-derived by Codex1 because this review did not access A3.

### Gate F1 and first blocker

The frozen order starts:

1. `I1024 / C1 / O8` — formal PASS;
2. `I1024 / C8 / O8` — formal PASS;
3. `I1024 / C32 / O8` — formal PASS;
4. `I1024 / C64 / O8` — formal FAIL.

The immutable Result identifies the first failing cell as `I1024 / C64 / O8`; request index `34` has decoded output containing `29` U+FFFD Unicode replacement characters. Under the frozen validator contract, this is sufficient to STOP.

Formal disposition:

- accepted formal execution boundary：**through the `I1024 / C64 / O8` failure**;
- last successful formal cell：**`I1024 / C32 / O8`**;
- first blocker：**decoded output contains U+FFFD and violates the Stage 6 acceptance contract**.

The literal `request index 34` is preserved without assuming whether the saved validator used zero- or one-based indexing.

## Execution-control deviation

The Task requires immediate STOP on the first failed O8 cell and prohibits starting O1024. Actual execution continued after `I1024 / C64 / O8`:

- the remaining 12 O8 cells, beginning at `I4096 / C1 / O8`, ran outside the formal STOP boundary;
- all 16 O1024 cells ran outside the formal STOP boundary.

All 28 post-STOP cells are **diagnostic raw evidence only**. They cannot extend the formal completed-cell set, count toward F1/F2/F3 acceptance, or support Stage 6 PASS. Their observed output-quality symptoms may inform hypotheses only.

Deviation record:

| Field | Review finding |
| --- | --- |
| What | validator STOP was not enforced after the first failing O8 cell; remaining O8 and O1024 ran |
| Why | `UNKNOWN / NOT RECORDED`; deferred validation is possible but not established |
| Impact | governance/contract violation; post-STOP cells downgraded to diagnostic-only; Stage 6 remains NOT ACCEPTED |
| Evidence | immutable Result Gate F1/F2 sections and preserved O8/O1024 summaries/cell results |

The deviation does not retroactively invalidate F0 or the first four formal O8 records and does not by itself prove source/runtime identity drift.

## Positive runtime-path evidence disposition

The following are preserved as **Result-supported observed runtime-path evidence**, not Stage 6 functional Acceptance:

- both TP workers recorded automatic capture sizes normalized to `[1,2,4,8,16,24,32,40,48,56,64]`;
- worker PIDs `46333` and `46334` recorded `forward_mode=FULL`, `graph_type=NPUGraph` replay observations at sizes through 64;
- prefill/non-uniform paths recorded `forward_mode=NONE` / eager, while decode graph paths recorded `forward_mode=FULL` / NPUGraph;
- config records `enable_chunked_prefill=True`; scheduler evidence records `max_num_scheduled_tokens=16384` and `is_prefill_chunk=true`;
- config records `async_scheduling=True`; the Result does not expose a separate event/counter that proves effective async use, so the reviewed claim is **enabled**, not full F3 proof;
- the recorded `server.log` substring scan found no `507011`, invalid address, OOM, CPU fallback, eager fallback, `vllm_ascend` or FlagGems strings; this is a bounded negative-scan claim, not global proof over every process/log;
- shutdown artifacts support clean release of this Task's service process and port while preserving the accepted container; no claim is made about unrelated host workloads.

Automatic capture is part of F0 and was reported before workload cells. Replay-size and long-prefill observations that depend on post-STOP cells remain diagnostic-only. In particular, 16K/64K chunked-prefill activity cannot satisfy formal F3 because those cells were outside the formal boundary.

## U+FFFD root-cause disposition

- Observed blocker confidence：**HIGH**.
- Underlying root-cause confidence：**LOW / NOT CONFIRMED / UNRESOLVED**.

The current record does not prove an FL GraphWrapper, NPUGraph state, NPU arithmetic, cross-request state, model weight, HTTP, async scheduling, chunked prefill or other runtime bug. It also does not prove tokenizer-native behavior or a benchmark/validator defect.

Control-visible Evidence is insufficient to locate the first layer that contains U+FFFD. The Result does not enumerate raw HTTP bytes, parsed response JSON, generated output token IDs, tokenizer decode/re-encode behavior, benchmark in-memory versus saved text, validator source/hash/input, or a byte/code-point layer comparison. Whether the original server Evidence already contains some of these unlisted artifacts is **UNKNOWN** until the follow-up performs a read-only audit.

## Immutable Result quality and reconstruction gaps

The Result is sufficiently identified and honest to preserve as a STOP record. It is not sufficient for Stage 6 Acceptance or complete final reconstruction without supplements. This Review records the material gaps:

- exact Result/sync commit `add960850a15a7f72ab89aac8ab050da1dbf0c0a` was omitted;
- last successful formal cell, four-cell formal set and accepted formal boundary were omitted;
- remaining 12 post-STOP O8 cells were not explicitly downgraded;
- deviation `why` and complete `what / why / impact / Evidence` were not recorded;
- accepted arm64 image digest, wheel/custom-op inventory pointer, exact container invocation/reconstruction substitutions, cache roots/state and complete env/config/benchmark artifact map were omitted or too coarse;
- required 16-row tables and row-level formal/post-STOP labels exist only through server-side summary pointers, not in the Result;
- raw response/token/decode/save/validator transformation artifacts are not enumerated;
- runtime instrumentation identity/hash/behavior/reason/impact is incomplete;
- async effective-use proof is not explicit;
- negative scans are bounded to the recorded server log.

These gaps do not invalidate the observed STOP fact. They prevent stronger root-cause, effective-runtime and reconstruction claims and are mandatory inputs to the one diagnostic Task.

## Final conclusion

Stage 6 STOP Result：**FORMALLY REVIEWED / immutable execution record preserved / Codex1 NEEDS-FOLLOWUP**.

Stage 6：**STOP / NOT ACCEPTED**.

Performance/capacity, prefix lifecycle, EP2 and later stages：**LOCKED**.

The only Ready next action is the artifact-first U+FFFD output-chain diagnostic. Codex2 may execute it only after explicit User dispatch. This Review does not dispatch Codex2.

## Three pointers

- Code/source：`https://github.com/xiemingda-1002/vllm-plugin-FL.git` at frozen `e610a990d785356bf51a3cad50219d4c03310a31`, tree `609ff1ad0f08239f353cb4d8774e504b4deba03b`, Accepted wheel SHA-256 `2fcf788660f3fe42b364bc60d593ee1b9b634fc0632de58c444d961bff4aa1bd`.
- Control：the frozen Task, immutable Result, this Review, `STATUS.md`, `results/INDEX.md`, `DECISIONS.md`, and the single follow-up Task.
- Evidence：`/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX/evidence/20260826T180105p0800` with `manifest.md`, `checksums.txt`, `runtime/server.log`, functional results and graph summary.
