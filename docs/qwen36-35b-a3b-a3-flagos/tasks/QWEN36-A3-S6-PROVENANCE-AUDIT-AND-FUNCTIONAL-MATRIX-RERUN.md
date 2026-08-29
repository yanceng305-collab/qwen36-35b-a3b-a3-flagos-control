# QWEN36-A3-S6-PROVENANCE-AUDIT-AND-FUNCTIONAL-MATRIX-RERUN

状态：**READY / Awaiting explicit User dispatch — ONLY NEXT TASK**

执行代理：Codex2 only after explicit User dispatch of this exact Task.

Decision basis:

- [`D-034`](../DECISIONS.md#d-034--stage-6-tokenizer-native-ufffd-semantics) — approved provenance-aware U+FFFD semantics;
- [`D-036`](../DECISIONS.md#d-036--bounded-device-mapping-diagnosis-and-composite-placement-proof) — bounded composite device mapping proof;
- [latest provenance STOP Formal Review](../reviews/REVIEW-QWEN36-A3-S6-PROVENANCE-CORRELATION-AND-FUNCTIONAL-MATRIX-RERUN-STOP-20260829.md).

This is the only Ready Task. It supersedes the ended `QWEN36-A3-S6-PROVENANCE-CORRELATION-AND-FUNCTIONAL-MATRIX-RERUN`; that Task must not be resumed. Ready is not execution: Codex2 must not touch A3 until the User explicitly dispatches this exact Task.

## Unified identity

```text
Control repo:
https://github.com/yanceng305-collab/qwen36-35b-a3b-a3-flagos-control
Implementation repo:
https://github.com/xiemingda-1002/vllm-plugin-FL
Task ID:
QWEN36-A3-S6-PROVENANCE-AUDIT-AND-FUNCTIONAL-MATRIX-RERUN
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

## Objective and route

Close the one remaining technical blocker, then continue directly in the same Task to a fresh full Stage 6 rerun:

```text
fresh safe inventory
-> D-036 composite mapping proof
-> exact Frozen source/runtime provenance audit
-> deterministic canonical request identity + collision proof
-> F0
-> fresh 16-cell O8 warm-up
-> only if all O8 pass
-> fresh 16-cell O1024 matrix
```

The audit must precede formal F0/workload where possible and must not repeatedly launch the full model merely to rediscover the same ID gap. If the transform cannot be proven within the bounded audit, classify `UNRESOLVED_PROVENANCE`, preserve raw Evidence, STOP, and do not enter F0 or workload. Do not create another separate device-mapping or diagnostic Task.

## Entry, race and context state

Before A3 mutation, live-query/sync Control `main`; read `AGENTS.md`, root `README.md`, `STATUS.md`, this Task, D-034, D-036, the ended provenance rerun Result/Formal Review, the prior unsynced D-035 note, supplemental mapping record, `A3-RUNTIME-HANDOFF.md`, `A2-TO-A3-VALIDATION-DELTA.md`, reconstruction docs and evidence rules.

Load pinned `long-context-orchestrator` in a Task-owned path, record repo, resolved commit, load path and computed `SKILL.md` SHA-256, and require:

```text
repo: https://github.com/yanceng305-collab/long-context-orchestrator
commit: 0bb8a5eda9c46f1b170552ba41b871ba141e04b6
SKILL.md SHA-256: f4e15f8c5f097d6aec0fe42b58e1cb5386d3104a02b5ebb502e8f7b0041af1f2
```

Hash mismatch is STOP before A3 workload mutation. Maintain Task-owned `WORKPLAN.md` and `INDEX.md`; durable state does not replace formal Evidence/Result and cannot expand authorization.

Confirm this exact Task is still `READY / ONLY NEXT TASK`, this is explicit User dispatch, no immutable Result for this Task exists, and no other active session executes it. Use one active session and unique run/container/work/Evidence/artifact/cache roots. A Control race stops new workload and must not overwrite remote state.

## D-036 admission proof

Perform read-only safe inventory of physical/logical devices, health, occupancy and owner. A numeric difference between host logical IDs, container IDs and torch indices is not itself failure. Suspend formal workload and use deterministic evidence such as host inventory, device-node major/minor, container `--device`, namespace visibility, `ASCEND_VISIBLE_DEVICES`, `ASCEND_RT_VISIBLE_DEVICES`, torch_npu names/count, TP rank and HCCL/runtime rank-device evidence. Direct `npu-smi info proc` or direct PID→NPU reporting is preferred when available but not mandatory when unsupported.

If needed, allow one Task-local visibility/container correction while keeping the same authorized idle physical devices and disturbing no unrelated resource. Re-prove the composite mapping. Do not hard-code `0,1` or `14,15`, and do not interpret container local indices as host logical IDs without proof. Actual service/worker placement must match the authorized composite mapping before readiness/workload acceptance.

Use `/data/tiankuan:/data/tiankuan`, the exact Accepted image, and the accepted container-local jemalloc compatibility method with unchanged `LD_PRELOAD=/usr/lib/aarch64-linux-gnu/libjemalloc.so.2`.

## Exact provenance audit

Audit the exact installed Frozen vLLM 0.20.2/FL serving/SSE/raw HTTP/client/save and Task-local instrumentation paths that create bare request IDs, internal IDs, `cmpl-<request-id>` and `cmpl-<request-id>-0`. Capture exact source locations, versions, runtime event construction and transform direction.

No substring/contains, startsWith, blind prefix stripping, edit distance, timestamp proximity, request-order-only attribution or heuristic matching. Preserve every raw ID exactly.

Define a canonical request identity only if the source/runtime audit proves a deterministic reversible or uniquely attributable transform with no collision in the cell. Record a raw-to-canonical mapping table, exact source/runtime proof, collision check and impact audit. Canonicalization is Evidence correlation only: it must not change request IDs, OpenAI response IDs, generated token IDs, tokenizer return, service/client behavior, sampling, order, concurrency, scheduling, graph, workload or validator semantics.

If proven, use the canonical key to correlate generated token IDs, native decode, serving object, SSE/JSON, raw HTTP, client, saved result and validator input. Apply D-034 exactly: identical native U+FFFD across all layers is `TOKENIZER_NATIVE_UFFFD` and does not alone fail corruption attribution; downstream introduction/mismatch/mutation is `POST_TOKENIZER_CORRUPTION` and immediate FAIL/STOP. All other functional/output-quality gates remain mandatory.

## F0 and full Stage 6

After D-036 mapping and provenance proof pass, verify exact Frozen source/tree/wheel/image/model/runtime identity, site-packages FL origin, no `vllm-ascend`/`vllm_ascend`, no FlagGems, torch_npu availability/count, model 26/26 shards and 1045 BF16 tensors, service HTTP health/models 200, both TP workers, accepted jemalloc maps, automatic capture `[1,2,4,8,16,24,32,40,48,56,64]`, prefix align, chunked prefill, async scheduling, `FULL_DECODE_ONLY`, and actual PID/device placement.

Keep the unchanged BF16 non-quantized DP1/TP2 mp service, `max_model_len=66560`, `max_num_seqs=64`, `max_num_batched_tokens=16384`, memory utilization `0.90`, original sampling/runtime/additional config, and `/data/tiankuan:/data/tiankuan` mount.

Run fresh O8 cells first, in order:

```text
I1024:  C1, C8, C32, C64
I4096:  C1, C8, C32, C64
I16384: C1, C8, C32, C64
I65536: C1, C8, C32, C64
```

Use exact Frozen tokenizer/random dataset, output 8, `random_range_ratio=0`, `random_prefix_len=0`, prompts/concurrency equal to C, request rate `inf`, `temperature=1`, `top_p=1`, `top_k=0`, `ignore_eos=true`, seed `INPUT + CONCURRENCY`, and frozen request ID/order. Any true functional failure or `UNRESOLVED_PROVENANCE` stops the Task and forbids O1024.

Only after all 16 O8 cells pass, run fresh 16-cell O1024 in the same order with output 1024. Enforce all original HTTP/error, exact token counts, length finish, readability/output quality, finite values, loop/repetition, contamination/stale state, graph/runtime ownership, TP health, CPU-fallback, no `vllm_ascend`, no FlagGems, chunked/async and shutdown gates. Do not reuse historical or supplemental cells, restart/clear caches, change parameters, or enter performance/capacity, prefix lifecycle, EP2, another model, source/tokenizer repair, sampling changes or moving-upstream work.

## Result boundary

Create exactly one immutable Result for this new Task. Preserve fresh-root manifests/checksums, device composite proof, exact provenance source/runtime audit, raw/canonical IDs, collision evidence, all U+FFFD chains, workload/cell/request artifacts, graph/chunked/async summaries, deviations, shutdown state and Code/Control/Evidence pointers. Codex1 Acceptance remains `PENDING` until independent review.

STOP at mapping ambiguity, race, unproven provenance, post-tokenizer corruption, any true functional/output-quality failure, Frozen/runtime drift, forbidden fallback, graph failure, contamination, OOM or first attributable blocker. After STOP do not continue workload. Publish exactly one Result, sync `results/INDEX.md` and Control, then STOP. Do not create another Task.
