# QWEN36-A3-S6-DEVICE-MAPPING-AND-PROVENANCE-CORRELATION-FOLLOWUP

状态：**ENDED / STOP / CONTROL SYNC PENDING — DO NOT RESUME**

执行代理：Codex2（local STOP Result unsynced; supplemental continuation recorded）

Formal basis:

- [`REVIEW-QWEN36-A3-S6-TOKENIZER-NATIVE-UFFFD-AWARE-FUNCTIONAL-MATRIX-RERUN-STOP-20260828.md`](../reviews/REVIEW-QWEN36-A3-S6-TOKENIZER-NATIVE-UFFFD-AWARE-FUNCTIONAL-MATRIX-RERUN-STOP-20260828.md)
- [`SUPPLEMENTAL-QWEN36-A3-S6-CONCURRENT-RUN-20260828T180824.md`](../reviews/SUPPLEMENTAL-QWEN36-A3-S6-CONCURRENT-RUN-20260828T180824.md)
- [`D-034`](../DECISIONS.md#d-034--stage-6-tokenizer-native-ufffd-semantics)

This Task ended at its F0 proof-gap. Its local immutable STOP Result remains server-local and `SERVER EVIDENCE EXISTS / CONTROL SYNC PENDING`; the supplemental mapping continuation is recorded separately. Do not resume this Task. The only next route is `QWEN36-A3-S6-PROVENANCE-CORRELATION-AND-FUNCTIONAL-MATRIX-RERUN`.

## Unified identity

```text
Control repo:
https://github.com/yanceng305-collab/qwen36-35b-a3b-a3-flagos-control
Implementation repo:
https://github.com/xiemingda-1002/vllm-plugin-FL
Task ID:
QWEN36-A3-S6-DEVICE-MAPPING-AND-PROVENANCE-CORRELATION-FOLLOWUP
Frozen source:
e610a990d785356bf51a3cad50219d4c03310a31
Frozen tree:
609ff1ad0f08239f353cb4d8774e504b4deba03b
Frozen wheel:
vllm_plugin_fl-0.2.0+ge610a990d-cp311-cp311-linux_aarch64.whl
Wheel SHA-256:
2fcf788660f3fe42b364bc60d593ee1b9b634fc0632de58c444d961bff4aa1bd
Accepted image:
quay.io/ascend/vllm-ascend:v0.20.2rc1-a3-openeuler
Model:
/data/tiankuan/zyg/FL/workspace/Qwen3.6-35B-A3B
```

## Objective and bounded route

In one Task, close only these two Task-local corrections, then run the complete formal Stage 6 matrix from the beginning:

```text
read-only device inventory
-> dynamic safe physical/logical selection
-> fresh container and exact narrowed mount
-> staged torch_npu scope proof
-> preserve resolved mapping through service launch
-> prove actual service/worker PID placement
-> bounded exact-ID provenance audit/canonicalization
-> F0
-> all 16 O8 warm-up cells
-> only after O8 all pass, all 16 O1024 cells
```

The previous `20260828T161700+0800` Result remains an immutable valid STOP at F0. The concurrent `20260828T180824+0800` run is supplemental only; none of its C1/C8/C32 passes count as progress. Use fresh Task-owned work, run, Evidence, artifact, cache, service and Result roots.

## Entry and race guard

Before any A3 mutation, live-query/sync Control `main` and read `AGENTS.md`, root `README.md`, `STATUS.md`, this Task, D-034, the ended Task's immutable Result and Formal Review, the supplemental Evidence record, `A3-RUNTIME-HANDOFF.md`, `A2-TO-A3-VALIDATION-DELTA.md`, reconstruction docs and evidence rules.

If this Task is no longer `READY / ONLY NEXT TASK`, if any immutable Result for this Task already exists, or if another active session is running this exact Task, STOP before workload/container mutation. Never launch a concurrent formal run. Use a unique run ID, container name and fresh roots. If Control changes during execution or a race is detected, stop new workload, preserve local Evidence, do not overwrite remote Control, and report the race.

## Correction A — device scope and mount

Perform read-only inventory of host model, physical NPU, host logical IDs, health, occupancy and owner. Select the minimum safe idle two-device scope dynamically. Record the selected physical and host logical IDs.

Create a fresh Task-owned container from the exact Accepted image and use the User-authorized narrowed bind:

```text
/data/tiankuan:/data/tiankuan
```

All model, wheel, work, Evidence, artifact and cache paths must be beneath `/data/tiankuan`. This is a filesystem-visibility/task-runtime correction only. Do not restore `/data:/data`, mutate the host, change Frozen source/model/runtime/service/workload, or rewrite the historical template.

Do not hard-code `0,1` or `14,15`. Preserve the resolved host logical scope through container and service launch. Only use container-local renumbering if runtime Evidence proves deterministic renumbering. Before readiness, prove `torch_npu` availability/count and actual service APIServer/EngineCore/TP-worker PID-to-host-logical-device placement. Actual placement must equal the authorized selected scope. Any mismatch is immediate F0 STOP. Do not kill, pause, reset, preempt, or alter unrelated devices/processes.

Reuse the Accepted jemalloc compatibility reconstruction inside this container only, keeping `LD_PRELOAD=/usr/lib/aarch64-linux-gnu/libjemalloc.so.2` unchanged.

## Correction B — exact request-ID provenance audit

Before relying on U+FFFD classification, audit the exact Frozen vLLM 0.20.2, FL service and Task-local instrumentation path that emitted bare IDs, `cmpl-<request-id>`, and `cmpl-<request-id>-0`.

No substring, `contains`, `startsWith`, prefix stripping, edit distance, timestamp proximity, order-only guess, or other fuzzy matching is allowed. Preserve every raw ID exactly as emitted.

Use source/runtime Evidence to prove any identifier transform. A canonical request identity is allowed only when the transform is deterministic and reversible or uniquely attributable, collision-free within the cell, and supported by exact code/runtime Evidence. Record the mapping table, source location/version, transform direction, collision check and impact audit. Canonicalization may affect Evidence correlation only; it must not change request IDs, OpenAI response IDs, generated token IDs, service/client behavior, sampling, request order, concurrency, scheduling, graph, tokenizer return, or validator functional semantics.

If the relationship cannot be proven, classify affected chains `UNRESOLVED_PROVENANCE`, fail the provenance gate, STOP before claiming `TOKENIZER_NATIVE_UFFFD` or `POST_TOKENIZER_CORRUPTION`, and preserve all raw evidence.

When proven, for every U+FFFD response preserve request ID, raw IDs at all layers, generated token IDs, exact Frozen-tokenizer native decode, serving/SSE/JSON/raw HTTP/client/saved/validator text and codepoints, canonical mapping and equality result. Apply D-034: native U+FFFD with identical downstream codepoints is `TOKENIZER_NATIVE_UFFFD` and does not alone fail corruption attribution; downstream mutation or mismatch is `POST_TOKENIZER_CORRUPTION` and immediate FAIL/STOP. All other functional/output-quality gates remain mandatory.

## Formal Stage 6 rerun

After both corrections are safely closed and F0 passes, run a single service with fresh caches. Run all 16 O8 cells first in this exact order:

```text
I1024:  C1, C8, C32, C64
I4096:  C1, C8, C32, C64
I16384: C1, C8, C32, C64
I65536: C1, C8, C32, C64
```

Use exact Frozen tokenizer/random dataset, input length as named, output length 8, `random_range_ratio=0`, `random_prefix_len=0`, prompts/concurrency equal to C, request rate `inf`, `temperature=1`, `top_p=1`, `top_k=0`, `ignore_eos=true`, dataset seed `INPUT + CONCURRENCY`, and the frozen request ID/order convention. Any true O8 functional failure, unresolved provenance, mapping drift, runtime drift, or forbidden fallback immediately stops the Task and forbids O1024.

Only after all 16 O8 cells pass, run all 16 O1024 cells once in the same order with output length 1024. Enforce exact prompt/output token counts, expected length finish, HTTP/error semantics, nonempty/readable output, finite logprobs, no NaN/Inf, loops, pathological repetition/repeated 8-grams, contamination/stale state, graph/runtime ownership, chunked prefill, async scheduling, worker health, CPU-fallback absence, no `vllm_ascend`, no FlagGems, and all other frozen Stage 6 gates.

Use unchanged Frozen service configuration: BF16 non-quantized DP1/TP2 mp, `max_model_len=66560`, `max_num_seqs=64`, `max_num_batched_tokens=16384`, memory utilization `0.90`, prefix caching `align`, chunked prefill, async scheduling, `FULL_DECODE_ONLY`, automatic capture `[1,2,4,8,16,24,32,40,48,56,64]`, `VLLM_PLUGINS=fl`, `USE_FLAGGEMS=0`, and the original runtime/additional configuration. Do not rebuild, patch production source, alter sampling, change topology, or run performance/prefix/EP2/new-model work.

## Result boundary

Create one new immutable Result for this new Task, preserve fresh-root manifests/checksums, raw IDs and canonicalization Evidence, device/PID placement, all cell/request artifacts, D-034 classifications, graph/runtime summaries, deviations and shutdown state. Sync `results/INDEX.md` and Control, leave Codex1 Acceptance `PENDING`, and STOP. Execution PASS is possible only after F0-F3 plus 16/16 O8 and 16/16 O1024. No historical or supplemental cell counts may be reused.
