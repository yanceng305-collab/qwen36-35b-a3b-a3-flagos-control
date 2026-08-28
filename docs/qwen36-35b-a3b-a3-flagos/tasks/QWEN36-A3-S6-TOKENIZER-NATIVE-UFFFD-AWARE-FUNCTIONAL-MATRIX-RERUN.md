# QWEN36-A3-S6-TOKENIZER-NATIVE-UFFFD-AWARE-FUNCTIONAL-MATRIX-RERUN

状态：**NOT READY / Awaiting User Decision D-034 — DO NOT DISPATCH**

执行代理：Codex2 only after a future explicit User dispatch

Diagnostic Formal Acceptance：[`REVIEW-QWEN36-A3-S6-JEMALLOC-RECONSTRUCTION-AND-UFFFD-ROOT-CAUSE-DIAGNOSTIC-20260828.md`](../reviews/REVIEW-QWEN36-A3-S6-JEMALLOC-RECONSTRUCTION-AND-UFFFD-ROOT-CAUSE-DIAGNOSTIC-20260828.md)

Pending Decision：[`D-034 / STAGE6-TOKENIZER-NATIVE-UFFFD-SEMANTICS`](../DECISIONS.md#d-034--stage-6-tokenizer-native-ufffd-semantics)

No User dispatch exists for this proposal. Do not create/run a prompt, start A3, reserve devices, create a container or mutate Evidence.

## Why this Task is not Ready

The diagnostic established tokenizer-native U+FFFD and excluded downstream mutation for a prospective request. Current Control does not decide whether tokenizer-native U+FFFD is acceptable final-text quality or only exempt from the corruption-attribution gate.

The original Stage 6 contract remains binding until the User explicitly selects D-034's provenance-aware branch or absolute-zero branch.

## Proposed objective if D-034 provenance-aware branch is approved

Rerun the formal Stage 6 A2-equivalent DP1/TP2 functional matrix from the beginning under the unchanged Frozen source/artifact/runtime/service/workload contract, with one explicit validator delta:

```text
generated token IDs
-> independent exact Frozen tokenizer decode
-> serving/wire/client/saved/validator layer equality
```

For every response containing U+FFFD:

- If independent native decode reproduces the exact returned text/code points and every downstream layer is identical, record `TOKENIZER_NATIVE_UFFFD`; do not fail the **corruption-attribution gate solely** for U+FFFD.
- Continue evaluating readability/final-output quality and every other functional gate; provenance awareness is not automatic product-quality acceptance.
- If U+FFFD or any text/code-point mutation first appears after native decode, or downstream text differs, FAIL the corruption gate and STOP immediately.

## Proposed unchanged contract

If promoted to Ready, retain all original Stage 6 requirements:

- D-030 Frozen source/tree/wheel and Accepted image/runtime/model;
- BF16 non-quantized DP1/TP2 mp service;
- `max_model_len=66560`, `max_num_seqs=64`, `max_num_batched_tokens=16384`, memory utilization `0.90`;
- prefix caching align, chunked prefill, async scheduling, `FULL_DECODE_ONLY`, automatic capture through 64;
- exact workload generation, sampling, seeds, request IDs, O8 warm-up before O1024 and frozen cell order;
- HTTP/error, exact prompt/output token counts, length finish, nonempty/readability, NaN/Inf, loops, repetition review, cross-request contamination, graph/runtime ownership and immediate STOP;
- no performance/capacity, prefix lifecycle or EP2.

Rerun from the beginning with new Task-owned work/Evidence/cache roots. Do not reuse parent post-STOP cells for Acceptance.

## If the User selects absolute zero-U+FFFD

This proposed rerun does not become Ready. The original output-quality gate remains failed, and any tokenizer/model/sampling/source remediation route requires a separate User Decision. No such change is authorized here.

## Promotion requirements

Only after an explicit User D-034 decision may Codex1:

1. finalize the chosen validator semantics;
2. change this Task from NOT READY to READY if applicable;
3. create a complete new-session Codex2 prompt with explicit User dispatch language;
4. authorize A3 execution.

Until then：**DO NOT DISPATCH**.
