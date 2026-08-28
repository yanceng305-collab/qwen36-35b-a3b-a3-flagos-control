# Stage 6 tokenizer-native U+FFFD validator contract

Decision: [`D-034`](../DECISIONS.md#d-034--stage-6-tokenizer-native-ufffd-semantics), **APPROVED / provenance-aware branch**.

This contract applies only to the fresh formal Stage 6 rerun. It does not alter or reinterpret immutable historical Results.

For each final response containing U+FFFD, capture the request ID, actual generated token IDs, independent decode with the exact Frozen tokenizer, serving text, SSE/JSON text, raw HTTP decoded representation, client parser/accumulator text, saved result, validator input, and text/codepoint equality across the chain.

Classify `TOKENIZER_NATIVE_UFFFD` only when:

1. the independent exact-tokenizer decode itself contains U+FFFD; and
2. native decode text/codepoints equal serving, SSE/JSON, raw HTTP, client, saved, and validator representations; and
3. no downstream text/codepoint mutation is present.

This classification must be recorded but does not, by itself, fail the corruption-attribution gate. It does not waive readability/final-output quality, token counts, finish reason, finite-number, repetition, contamination, graph/runtime, worker-health, CPU-fallback, `vllm_ascend`, FlagGems, or any other frozen Stage 6 gate.

Classify `POST_TOKENIZER_CORRUPTION` and immediately fail the corruption gate/STOP when native decode has no U+FFFD but a downstream layer introduces one, any downstream representation differs from native decode, or any post-tokenizer text mutation is observed. Preserve the earliest changed layer and raw Evidence.

Instrumentation is capture-only, light, Task/Evidence-local, identified and impact-audited. It must not change token generation, tokenizer return, response content, sampling, scheduling, request order/concurrency, graph, prefix, chunked prefill, async scheduling or error semantics. Responses without U+FFFD do not require expanded provenance capture.

The absolute-zero final-text alternative is not selected by D-034. If a future product requirement mandates zero U+FFFD, it requires a new User Decision and remediation contract.
