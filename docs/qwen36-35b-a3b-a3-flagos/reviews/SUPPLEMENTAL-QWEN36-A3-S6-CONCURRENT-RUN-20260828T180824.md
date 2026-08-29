# Supplemental Evidence Record — late concurrent Stage 6 run

Record date: 2026-08-29

Status: **SUPPLEMENTAL DIAGNOSTIC EVIDENCE — NOT AN IMMUTABLE RESULT**

This record registers the User-provided Evidence from a second session that continued after the exact Task's immutable Result had already been published. The session live-queried Control `da5f8c1f2f59cbd4abefb7d2cb41e091c952bbca`, stopped at approximately `20260828T180824+0800`, published no second immutable Result, did not overwrite `INDEX.md`/`STATUS.md`, did not restore Stage 6, did not enter O1024, and completed cleanup.

## Pointer

```text
/data/tiankuan/zyg/FL/workspace/stage6/QWEN36-A3-S6-TOKENIZER-NATIVE-UFFFD-AWARE-FUNCTIONAL-MATRIX-RERUN-20260828/evidence/20260828T180824p0800/
```

Primary files:

```text
manifest.md
analysis/first_blocker.md
analysis/runtime_summary.md
runtime/shutdown.typescript
checksums.txt
```

Checksums manifest SHA-256 reported for this supplemental Evidence:

```text
5330562f1962765a9166290f0c53c469e9ca929107b206f460d9a554fb64d6e9
```

Codex1 did not access A3 or independently fetch this server path in this Control-only session; the pointer, digest, and facts below are registered from the User-provided concurrent-run record.

## Reported bounded execution

```text
F0 PASS
I1024/C1/O8   PASS
I1024/C8/O8   PASS
I1024/C32/O8  PASS
I1024/C64/O8  STOP
remaining 12 O8 cells: NOT RUN
all O1024 cells: NOT RUN
```

The three reported C1/C8/C32 cells are not formal Stage 6 progress and cannot be inherited by the next run. The next formal Task must use fresh roots and rerun all 16 O8 cells from the beginning, followed only after complete O8 success by all 16 O1024 cells.

## Supplemental first blocker

Requests 34 and 55 in `I1024/C64/O8` reportedly contained U+FFFD, but the same logical request appeared across layers as bare request ID, `cmpl-<request-id>`, and `cmpl-<request-id>-0`. Exact raw-string equality therefore could not correlate generated IDs, native decode, serving object, SSE/raw HTTP, client, saved result, and validator input.

Reported classification: `UNRESOLVED_PROVENANCE`.

No `POST_TOKENIZER_CORRUPTION` was established, and this supplemental record is not evidence of model/runtime corruption. The accurate blocker is a **provenance correlation / validator identity-normalization gap**.

No fuzzy matching is authorized. A follow-up may normalize identifiers only after a bounded audit of the exact Frozen vLLM/service/instrumentation path proves a deterministic, reversible or uniquely attributable transform with no collision in the cell. Raw IDs must remain preserved; canonicalization is Evidence correlation only and must not change request IDs, service/client behavior, sampling, order, concurrency, or functional semantics.

## Control boundary

This supplemental record is a late concurrent-run diagnostic pointer only. It does not create a second immutable execution Result, does not rewrite the 16:17 Result or its F0 blocker, does not count as formal matrix progress, and does not authorize a retry outside the new follow-up Task.
