# QWEN36-A3-S6-PROVENANCE-CORRELATION-AND-FUNCTIONAL-MATRIX-RERUN

状态：**READY / Awaiting explicit User dispatch — ONLY NEXT TASK**

执行代理：Codex2 only after explicit User dispatch of this exact Task.

Decision basis:

- [`D-034`](../DECISIONS.md#d-034--stage-6-tokenizer-native-ufffd-semantics) — approved provenance-aware U+FFFD semantics;
- [`D-036`](../DECISIONS.md#d-036--bounded-device-mapping-diagnosis-and-composite-placement-proof) — approved bounded device mapping policy;
- [unsynced STOP Control note](../reviews/UNSYNCED-QWEN36-A3-S6-DEVICE-MAPPING-AND-PROVENANCE-CORRELATION-FOLLOWUP-20260829.md);
- [supplemental mapping Evidence boundary](../reviews/SUPPLEMENTAL-QWEN36-A3-S6-DEVICE-MAPPING-20260829T113000.md).

This Task supersedes the ended unsynced device/provenance follow-up route. It is the only Ready Task. Ready is not execution: do not touch A3 without explicit User dispatch.

## Unified identity

```text
Control repo:
https://github.com/yanceng305-collab/qwen36-35b-a3b-a3-flagos-control
Implementation repo:
https://github.com/xiemingda-1002/vllm-plugin-FL
Task ID:
QWEN36-A3-S6-PROVENANCE-CORRELATION-AND-FUNCTIONAL-MATRIX-RERUN
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

## Objective

In one fresh Task, perform a bounded dynamic device mapping proof, an exact source/runtime request-ID correlation audit, then run the complete formal Stage 6 matrix from the beginning:

```text
safe inventory
-> composite physical/logical/device-node/container/torch mapping proof
-> one Task-local visibility correction only if needed
-> exact provenance transform proof and collision check
-> F0
-> 16 O8 warm-up cells
-> only if all O8 pass
-> 16 O1024 cells
```

The previous unsynced STOP Result and the 20260829 supplemental mapping continuation are not repository immutable Results and are not formal matrix progress. Do not fabricate or import them as immutable Result content.

## Entry, race, and sync boundary

Before A3 mutation, live-query and sync Control `main`; read `AGENTS.md`, root `README.md`, `STATUS.md`, this Task, D-034, D-036, the unsynced STOP note, the supplemental mapping record, the prior ended Task contract, `A3-RUNTIME-HANDOFF.md`, `A2-TO-A3-VALIDATION-DELTA.md`, reconstruction docs, and `REPOSITORY-AND-EVIDENCE-RULES.md`.

Confirm this exact Task remains `READY / ONLY NEXT TASK`, this is explicit User dispatch, no immutable Result for this Task exists, no other session is active for this Task, and all Frozen identities are unchanged. If any check fails, STOP before container/workload mutation. Use one active formal Codex2 session, unique run/container/work/Evidence/artifact/cache roots, and one immutable Result. If Control races during execution, stop new workload, preserve local Evidence, do not overwrite remote Control, and report the race.

## D-036 device mapping policy

A numeric mismatch between host logical IDs, container-visible IDs, and torch indices is not by itself a failure. Suspend formal workload and perform one bounded read-only diagnosis. Acceptable evidence includes host physical/logical inventory, device-node major/minor and identity, container `--device` configuration, process namespace/device visibility where available, `ASCEND_VISIBLE_DEVICES`, `ASCEND_RT_VISIBLE_DEVICES`, torch_npu count/names, TP rank-to-visible-index, HCCL/runtime rank-device evidence, and other deterministic runtime evidence.

Direct `npu-smi info proc` or direct PID-to-NPU reporting is preferred when supported but is not mandatory when unsupported. A composite proof is sufficient when deterministic, auditable, internally consistent, and it demonstrates that the Task-visible two-device scope corresponds to the authorized host devices.

If Task-local visibility configuration is wrong, allow one bounded correction to visibility environment, container device selection, service-launch environment, or local-index interpretation. Keep the same authorized idle physical devices; do not kill, pause, reset, preempt, mutate, or otherwise disturb unrelated resources. Re-prove the mapping after correction. Do not hard-code `0,1` or `14,15`, and do not claim universal renumbering from the prior supplemental run.

STOP only if authorized devices cannot be safely identified, mapping remains ambiguous after bounded diagnosis, unauthorized/occupied devices are used and cannot be safely corrected, correction requires unrelated-workload mutation, or Frozen source/model/runtime change is required. Do not STOP merely because a preferred inspection API is unsupported. Actual service/worker placement must equal the authorized composite mapping before readiness/workload acceptance.

Use the accepted `/data/tiankuan:/data/tiankuan` bind and the accepted container-local jemalloc compatibility method. Keep `LD_PRELOAD=/usr/lib/aarch64-linux-gnu/libjemalloc.so.2` unchanged.

## Provenance correlation policy

Before U+FFFD classification, audit the exact Frozen vLLM 0.20.2, FL serving, SSE/raw HTTP, client/save, and Task-local instrumentation path that creates bare IDs, vLLM IDs, `cmpl-<request-id>`, and `cmpl-<request-id>-0`.

Fuzzy matching is forbidden: no substring/contains, startsWith, blind prefix stripping, edit distance, timestamp proximity, order-only attribution, or heuristic guessing. Preserve every raw ID exactly.

Define a canonical request identity only if exact source/runtime Evidence proves a deterministic reversible or uniquely attributable transform with no collision in the cell. Record raw IDs, transform direction, exact source location/version, mapping table, collision check, and impact audit. Canonicalization affects Evidence correlation only. It must not change request IDs, OpenAI response IDs, generated token IDs, tokenizer return, service/client behavior, sampling, scheduling, order, concurrency, graph, workload, or validator functional semantics.

If the relationship cannot be proven in the bounded audit, classify affected chains `UNRESOLVED_PROVENANCE`, preserve raw Evidence, fail the provenance gate, and STOP. Do not claim `TOKENIZER_NATIVE_UFFFD` or `POST_TOKENIZER_CORRUPTION` from an uncorrelated chain.

When proven, capture for every U+FFFD response: all raw IDs, canonical key, generated token IDs, exact Frozen-tokenizer native decode, serving/SSE/JSON/raw HTTP/client/saved/validator text and codepoints, and equality across layers. Apply D-034 exactly: identical native U+FFFD across all layers is `TOKENIZER_NATIVE_UFFFD` and does not alone fail corruption attribution; downstream introduction/mismatch/mutation is `POST_TOKENIZER_CORRUPTION` and immediate FAIL/STOP. All other functional/output-quality gates remain mandatory.

## Frozen F0 and complete matrix

Verify exact Frozen source/tree/wheel/image/model/runtime identity, site-packages FL origin, no `vllm-ascend`/`vllm_ascend`, no FlagGems, `torch_npu` availability and actual mapped count, model 26/26 shards and 1045 BF16 tensors, service health/models HTTP 200, both TP workers, automatic capture `[1,2,4,8,16,24,32,40,48,56,64]`, prefix align, chunked prefill, async scheduling, `FULL_DECODE_ONLY`, accepted jemalloc maps, and actual service/worker placement.

Use the unchanged BF16 non-quantized DP1/TP2 mp service contract: `max_model_len=66560`, `max_num_seqs=64`, `max_num_batched_tokens=16384`, memory utilization `0.90`, prefix caching `align`, chunked prefill, async scheduling, `enforce_eager=False`, `FULL_DECODE_ONLY`, EP/MTP/quantization off, `VLLM_PLUGINS=fl`, `USE_FLAGGEMS=0`, `SOC_VERSION=ascend910_93`, and all original Stage 6 environment/additional configuration.

Create fresh caches and one service. Run all 16 O8 cells first in this order:

```text
I1024:  C1, C8, C32, C64
I4096:  C1, C8, C32, C64
I16384: C1, C8, C32, C64
I65536: C1, C8, C32, C64
```

O8 uses exact Frozen tokenizer/random dataset, output 8, `random_range_ratio=0`, `random_prefix_len=0`, prompts/concurrency equal to C, request rate `inf`, `temperature=1`, `top_p=1`, `top_k=0`, `ignore_eos=true`, seed `INPUT + CONCURRENCY`, and frozen request IDs/order. Any true functional failure or `UNRESOLVED_PROVENANCE` stops the Task and forbids O1024.

Only after all 16 O8 cells pass, run all 16 O1024 cells once in the same order with output 1024. Do not reuse historical or supplemental cells, restart/clear caches, change parameters, or enter performance/capacity, prefix lifecycle, EP2, another model, source/tokenizer repair, sampling changes, or moving-upstream work.

Enforce all original HTTP/error, exact prompt/output counts, length finish, nonempty/readability, finite-number, loop/repetition, contamination/stale-state, graph/runtime ownership, TP health, CPU-fallback, `vllm_ascend`, FlagGems, chunked/async and shutdown gates. `TOKENIZER_NATIVE_UFFFD` exempts only corruption attribution.

## Result boundary

Create exactly one new immutable Result for this Task, preserving fresh-root manifests/checksums, composite mapping proof, PID placement, mount scope, jemalloc reconstruction, exact ID transform audit, raw/canonical IDs, all U+FFFD chains, workload/cell/request artifacts, graph/chunked/async summaries, deviations and shutdown state, plus Code/Control/Evidence pointers.

Codex1 Acceptance remains `PENDING` until independent review. STOP at mapping ambiguity, race, unproven provenance, post-tokenizer corruption, true functional/output-quality failure, runtime/Frozen drift, forbidden fallback, graph failure, contamination, OOM or any first attributable blocker. After PASS or STOP, stop new workload, cleanly shut down only Task-owned resources, sync `results/INDEX.md` and Control, and STOP. Do not create another Task.
