# Codex1 Formal Acceptance — Qwen3.6 A3 Tokenizer-Native U+FFFD Diagnostic

> Historical review snapshot: the D-034 status and next-task readiness statements below describe the review state at its original acceptance time. User Decision D-034 was subsequently approved as the provenance-aware branch; current routing is authoritative in `DECISIONS.md`, `STATUS.md`, and the Ready Task contract.

Review date：2026-08-28

Disposition：**ACCEPTED — scope-limited diagnostic A / tokenizer-decoder-native**

Stage 6：**STOP / NOT ACCEPTED**

This Acceptance covers the diagnostic Result, R0/R1 admission, request-linked R2 classification and immediate STOP only within the exact run scope. It does not revise the frozen Stage 6 validator, retroactively pass the parent run or unlock later stages.

## Live Control provenance

Codex1 live-queried GitHub before review and did not rely on chat history or the supplied SHA.

| Field | Live-reviewed identity |
| --- | --- |
| Control repo | `https://github.com/yanceng305-collab/qwen36-35b-a3b-a3-flagos-control` |
| Live `main` / Result commit | `5d40d63e42ec174dd41a602946ae6c794ed7cf46` / tree `70ba623c5ab06b81e353a31a6cc68bcfda5bf430` |
| Direct parent | `3efdf6014ee89e265fd7940bed8645587bb118a7` |
| Result blob | `fcaac81a53fd84e979d69199525d1a38a83f0133` |
| Executed Task blob | `27bf991b20b3c8bf1eeab27e1742c8f32c491566` |

The Result commit adds the immutable Result and changes only `results/INDEX.md`. The Result does not modify the executed Task, source, reconstruction or any predecessor Result.

Codex1 did not access A3 or independently re-hash raw server Evidence. This Review checks the immutable Control record, internal consistency, contract compliance and stable Evidence pointers.

## Reviewed records

- Immutable Result：[`RESULT-QWEN36-A3-S6-JEMALLOC-RECONSTRUCTION-AND-UFFFD-ROOT-CAUSE-DIAGNOSTIC-20260828T093900+0800.md`](../results/RESULT-QWEN36-A3-S6-JEMALLOC-RECONSTRUCTION-AND-UFFFD-ROOT-CAUSE-DIAGNOSTIC-20260828T093900+0800.md)
- Executed Task：[`QWEN36-A3-S6-JEMALLOC-RECONSTRUCTION-AND-UFFFD-ROOT-CAUSE-DIAGNOSTIC`](../tasks/QWEN36-A3-S6-JEMALLOC-RECONSTRUCTION-AND-UFFFD-ROOT-CAUSE-DIAGNOSTIC.md)
- Evidence root：`/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S6-JEMALLOC-RECONSTRUCTION-AND-UFFFD-ROOT-CAUSE-DIAGNOSTIC/runs/20260828T093900+0800/evidence`
- Main Evidence：`manifest.md`, `checksums.txt`, `runtime/r0_reconstruct_and_invariant.typescript`, `runtime/r1_admission_proof.typescript`, `runtime/server_s1.typescript`, `runtime/server_chain_s1.jsonl`, `workload/s1_c64a/`, `analysis/request_linked_chain.md`, `analysis/request_linked_chain.json`

## Accepted diagnostic claim

The accepted claim is limited to prospective request `ufffd-s1-c64a-035`:

```text
generated IDs:
[113814,100538,105211,99329,109,68,145113,95841]

exact Frozen native tokenizer decode:
token 109 buffered without U+FFFD
token 68 decode first produced U+FFFD

native decoded text/codepoints
== serving object
== serialized SSE/raw HTTP bytes
== parsed JSON/events
== benchmark client memory
== saved/reloaded result
== validator input
```

Independent whole-sequence re-decode of the same eight generated IDs with the exact Frozen tokenizer reproduced identical text and code points.

Therefore the earliest evidenced textual U+FFFD layer for this prospective request is the **native tokenizer decode boundary**.

Formal classification：**A — tokenizer/decoder-native**.

The following are excluded as the first U+FFFD-producing layer for request 035:

- serving-object construction;
- JSON/SSE serialization;
- HTTP transport;
- client parser/event aggregation;
- benchmark accumulator;
- save/reload;
- validator.

Request `ufffd-s1-c64a-034` independently supports the same boundary but its unusual single-token/large-decode observation is treated as corroborative only. Formal A is anchored on request 035 and the independent exact-ID re-decode.

This Acceptance does not label FL, NPUGraph, NPU arithmetic/weights, async/chunked-prefill, transport, client or validator as corrupt. It identifies the earliest evidenced text layer, not why the sampled token IDs were chosen or whether tokenizer-native U+FFFD satisfies product-quality policy.

## Historical parent claim boundary

The parent Stage 6 request `s6-i1024-c64-o8-34` remains a valid historical saved-result/validator failure with 29 U+FFFD under the then-frozen blanket validator.

The prospective run used `temperature=1`, global stream random sampling and a different service history. Dataset seed controls prompts, not sampled-token identity.

Therefore this run proves:

- the Frozen service/tokenizer combination can prospectively produce tokenizer-native U+FFFD under the bounded Stage 6-style workload;
- the prospective request's downstream chain preserved native-decoded text exactly.

It does not prove:

- the historical parent request used the same token IDs or byte-token sequence;
- its 29 U+FFFD arose from the identical mechanism;
- the parent formal boundary or STOP disposition should be rewritten.

## R0 Acceptance — exact Task scope

R0 is **ACCEPTED — Task-owned runtime reconstruction scope**.

- One fresh Task-owned container was used.
- `/usr/lib64/libjemalloc.so.2` was recorded as package-owned AArch64 ELF, SHA-256 `2059f0cb5c2b3da8b834f4a54c12a633295eadb01844cef298398f350a2df43e`.
- Exactly one authorized directory/symlink method restored the frozen compatibility path.
- Frozen `LD_PRELOAD=/usr/lib/aarch64-linux-gnu/libjemalloc.so.2` remained unchanged.
- Realpath/hash, non-generative loader activation, NPU invariant and `/proc/self/maps` jemalloc proof passed.
- No host, image, source, wheel or model mutation was recorded; container removal was rollback.

This is an ephemeral Task-bounded correction, not a new baseline, production-image change or Stage 1/2 revalidation.

## R1 Acceptance — exact Task scope

R1 is **ACCEPTED — frozen service admission/readiness scope**.

- Exact BF16 DP1/TP2 mp Stage 6 service contract was used.
- Both workers loaded 26 shards and completed automatic capture `[1,2,4,8,16,24,32,40,48,56,64]`.
- `/health` and `/v1/models` passed.
- API, EngineCore, TP0 and TP1 were PID/role mapped and all mapped the verified jemalloc object.
- The frozen target remained resolved and loader-error scan passed.

The nonfatal usage-reporting `cpuinfo` exception did not stop engine, workers, readiness, workload or shutdown and is not the first blocker.

R1 does not prove the full Stage 6 matrix, performance/capacity or every F3 semantic.

## Execution-control Acceptance

Execution control is **COMPLIANT**:

```text
service launches:                  1/3
C64 targets:                       1/4
prehistory sequences:              0/2
workload cells:                    1/10
generation requests:              64/338
all outputs:                       exactly O8
runtime reconstruction:            1/1 method
instrumentation correction:        0/1
```

The first target produced complete classifiable chains and triggered immediate STOP. No second target or prehistory sequence ran. Task-owned services shut down cleanly, the container was removed, port 8016 was released and NPU 0/1 were reported process-free.

## Metadata and no-drift boundary

The Result records the Frozen source/tree/wheel, Accepted image, runtime packages, model, site-packages ownership, no FlagGems and no vLLM-Ascend. Control supplements CANN `9.0.0` and last pre-change reference `032fddc91b6d013b98aed8e64ff05b54d1435648` from the Frozen project record.

The Result summary does not enumerate every instrumentation filename/hash/load path/patched symbol/rollback; those remain under the named manifest/checksum/Evidence inventory. This Acceptance does not claim a fresh raw-Evidence re-hash or a broader whole-runtime proof.

## Original validator semantic review

The original Stage 6 Task remains binding for the historical run. It required no Unicode replacement-character output and the frozen validator treated any U+FFFD as FAIL.

Current A Evidence proves that blanket `any final-text U+FFFD => corruption FAIL` can misclassify tokenizer-native output as **post-tokenizer corruption**. It is therefore a demonstrated semantic false-positive for corruption attribution.

It is not automatically a false-positive for an absolute final-output quality rule. A User may require zero U+FFFD regardless of provenance.

## A2 oracle fact boundary

The Control-recorded A2 oracle states readable output, no obvious illegal characters, 16/16 strict matrix PASS and that future A3 strict oracles/tolerances must be defined separately.

No Control-visible A2 synthesis or corroborating PR text explicitly defines an absolute zero-U+FFFD product requirement. The blanket rule's first visible formal appearance is the A3 Stage 6 Task.

However, the three hash-registered private source originals are not present in Control/workspace. Therefore this Review may say the zero-U+FFFD rule is **not established as an A2-documented fact**; it may not claim conclusively that the private originals contained no such rule.

`readable` and `no obvious illegal characters` are qualitative and do not settle provenance-aware output acceptance.

## Recommended User Decision

Control records [`D-034 / STAGE6-TOKENIZER-NATIVE-UFFFD-SEMANTICS`](../DECISIONS.md#d-034--stage-6-tokenizer-native-ufffd-semantics) as **PROPOSED / AWAITING USER DECISION**.

Recommended branch, if no external product rule requires zero final-text U+FFFD:

1. Independently decode generated IDs with the exact Frozen tokenizer.
2. If native decode exactly matches serving/wire/client/saved/validator text including U+FFFD, record `TOKENIZER_NATIVE_UFFFD` and do not fail the corruption-attribution gate solely for that observation.
3. If U+FFFD or any text/code-point mutation first appears after native decode, or downstream text differs, fail the corruption gate immediately.
4. Keep readability/output quality, HTTP/error, token counts, finish reason, NaN/Inf, loops, repetition, contamination, identity, graph/chunked/async and STOP gates unchanged.
5. Do not retroactively pass the parent; rerun Stage 6 from the beginning under the revised oracle.

Alternative User branch：require zero final-text U+FFFD regardless of provenance. In that case the original gate remains valid as an output-quality rule, Stage 6 remains failed/not accepted, and an unchanged-source/parameter rerun is not an adequate remedy.

Until the User selects a branch, D-034 is not approved and no Task is Ready.

## Current project disposition

- Diagnostic Result：**ACCEPTED — A / tokenizer-decoder-native, scope-limited**.
- R0/R1/control：**ACCEPTED within exact run scope**.
- Historical parent request mechanism：not proven by this prospective run.
- Stage 6：**STOP / NOT ACCEPTED**.
- Performance/capacity, prefix lifecycle, EP2 and later stages：**LOCKED**.
- Next Task proposal：**NOT READY / Awaiting User Decision — DO NOT DISPATCH**.

This Review does not dispatch Codex2 and does not generate a dispatchable prompt.
