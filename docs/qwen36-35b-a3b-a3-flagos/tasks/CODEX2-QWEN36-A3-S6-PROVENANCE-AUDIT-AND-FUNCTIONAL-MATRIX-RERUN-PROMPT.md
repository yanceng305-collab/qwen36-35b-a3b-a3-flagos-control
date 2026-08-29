# Codex2 Prompt — QWEN36-A3-S6-PROVENANCE-AUDIT-AND-FUNCTIONAL-MATRIX-RERUN

状态：**DISPATCHABLE ONLY AFTER EXPLICIT USER DISPATCH**

User formal dispatch:

**Execute only `QWEN36-A3-S6-PROVENANCE-AUDIT-AND-FUNCTIONAL-MATRIX-RERUN` in this completely new Codex2 session. Complete exactly one immutable Result and Control sync, then STOP. Do not execute any other Task.**

```text
Control repo:
https://github.com/yanceng305-collab/qwen36-35b-a3b-a3-flagos-control

Implementation repo:
https://github.com/xiemingda-1002/vllm-plugin-FL

Exact Task ID:
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

Manifest digest:
sha256:442363921166771eb82baeec9c1ac0381f46fb830ead8d0e072df6e925f2a958

Arm64 platform digest:
sha256:a55b2b0597f9fdd1882de9bf3b7ebc395dd77c1ca49f251d0cb759d7b2c1a807

Accepted image ID:
sha256:8e7823878349b37d9900984555e28381c25681f88ff678cb4a86f7d1674a67c1

Model:
/data/tiankuan/zyg/FL/workspace/Qwen3.6-35B-A3B

Historical reference branch (not an execution gate):
feature/qwen3.6-35b-a3b-ascend-graph-migration
```

This is a completely new session. Do not use prior chat, agent memory, unsynced Results, supplemental cell counts, or server-local assumptions as formal truth.

Before any A3 mutation:

1. Live-query and sync current Control `main`.
2. Read `AGENTS.md`, root `README.md`, `STATUS.md`, this exact Task, D-034, D-036, the latest provenance STOP Result and Formal Review, the unsynced D-035 note, supplemental mapping record, prior Task contract, `A3-RUNTIME-HANDOFF.md`, `A2-TO-A3-VALIDATION-DELTA.md`, reconstruction docs, and `REPOSITORY-AND-EVIDENCE-RULES.md`.
3. Load this pinned context/state-management skill in a Task-owned path:

```text
repo:
https://github.com/yanceng305-collab/long-context-orchestrator

commit:
0bb8a5eda9c46f1b170552ba41b871ba141e04b6

SKILL.md expected SHA-256:
f4e15f8c5f097d6aec0fe42b58e1cb5386d3104a02b5ebb502e8f7b0041af1f2
```

After loading, compute the `SKILL.md` SHA-256. If it differs from the expected hash, STOP before any A3 workload mutation. Record repository URL, resolved commit, Task-owned load path and computed hash. Maintain Task-owned `WORKPLAN.md` and `INDEX.md`; across compaction preserve current phase/gate, selected physical/logical scope, container/service identity, PID placement, provenance rule/proof state, completed O8/O1024 cells, current/last-success cell, `TOKENIZER_NATIVE_UFFFD`, `UNRESOLVED_PROVENANCE`, `POST_TOKENIZER_CORRUPTION`, first blocker, STOP state, Evidence pointers and Control/Result sync state.

The skill is for context/state management only. Formal Task, User dispatch, D-034, D-036, PASS/STOP and Evidence rules are authoritative. Skill/subagents cannot expand authorization. After formal STOP, do not continue workload. Durable notes do not replace raw Evidence or immutable Result.

Confirm this exact Task is still `READY / Awaiting explicit User dispatch — ONLY NEXT TASK`, this message is the User's explicit dispatch, no immutable Result for this Task exists, no other active session is executing it, and all Frozen identities are unchanged. If any check fails, STOP before container/workload mutation. Use one active formal session and unique run/container/work/Evidence/artifact/cache roots. A Control race stops new workload and must not overwrite remote Control.

## Bounded provenance audit before F0

First perform the smallest exact audit of the Frozen installed vLLM 0.20.2, FL serving, SSE/raw HTTP, client/save and Task-local instrumentation paths that create bare request IDs, internal IDs, `cmpl-<request-id>`, and `cmpl-<request-id>-0`.

Capture exact source locations, versions, runtime event construction and transform direction. Do not repeatedly launch the full model merely to rediscover this gap.

Fuzzy matching is forbidden: no substring/contains, startsWith, blind prefix stripping, edit distance, timestamp proximity, request-order-only attribution or heuristic guessing. Preserve every raw ID exactly.

Define a canonical request identity only if exact source/runtime Evidence proves a deterministic transform that is reversible or uniquely attributable and collision-free in the relevant cell. Record raw-to-canonical mapping, source location/version, transform direction, collision check and impact audit.

Canonicalization affects Evidence correlation only. It must not change request IDs, OpenAI response IDs, generated token IDs, tokenizer return, service/client behavior, sampling, scheduling, request order, concurrency, graph, workload or validator semantics.

If the relationship cannot be proven within this bounded audit, classify affected chains `UNRESOLVED_PROVENANCE`, preserve raw Evidence, STOP, and do not enter F0 or workload. Do not claim `TOKENIZER_NATIVE_UFFFD` or `POST_TOKENIZER_CORRUPTION` from an uncorrelated chain.

If proven, retain all raw IDs and use the proven canonical key only for correlation of generated token IDs, native decode, serving object, SSE/JSON, raw HTTP, client, saved result and validator input. Apply D-034 exactly: identical native U+FFFD across all layers is `TOKENIZER_NATIVE_UFFFD` and does not alone fail corruption attribution; downstream introduction, mismatch or post-tokenizer mutation is `POST_TOKENIZER_CORRUPTION` and immediate FAIL/STOP. All other functional/output-quality gates remain mandatory.

## D-036 device admission

Perform read-only inventory of host model, physical/logical NPU IDs, health, occupancy and owner. A numeric difference between host logical IDs, container-visible IDs and torch indices is not itself a failure. Suspend formal workload and perform bounded deterministic composite mapping diagnosis using host inventory, device-node major/minor, container `--device`, namespace visibility, `ASCEND_VISIBLE_DEVICES`, `ASCEND_RT_VISIBLE_DEVICES`, torch_npu names/count, TP rank and HCCL/runtime rank-device evidence.

Direct `npu-smi info proc` or direct PID-to-NPU reporting is preferred when supported but is not mandatory when unsupported. A composite proof is sufficient when deterministic, auditable and internally consistent. If a Task-local visibility correction is needed, allow one correction while keeping the same authorized idle physical devices and disturbing no unrelated resource; re-prove mapping. Do not hard-code `0,1` or `14,15`, and do not interpret container local indices as host logical IDs without proof.

Read-only inspection required for safe inventory, health, occupancy, ownership and PID-placement verification is allowed. Never kill, pause, reset, preempt, mutate, or otherwise disturb unrelated devices, processes, containers, caches or workloads.

Actual APIServer, EngineCore and TP-worker placement must match the authorized composite mapping before readiness/workload acceptance. STOP only if devices cannot be safely identified, mapping remains ambiguous, unauthorized/occupied devices cannot be safely corrected, correction requires unrelated-workload mutation, or Frozen source/model/runtime change is required. Do not STOP merely because a preferred inspection API is unsupported.

Use `/data/tiankuan:/data/tiankuan`, the exact Accepted image, and the Accepted container-local jemalloc compatibility method. Keep `LD_PRELOAD=/usr/lib/aarch64-linux-gnu/libjemalloc.so.2` unchanged.

## F0 and Stage 6 rerun

After provenance proof and D-036 mapping proof, verify exact Frozen source/tree/wheel/image/model/runtime identity, site-packages FL origin, no `vllm-ascend`/`vllm_ascend`, no FlagGems, torch_npu availability and mapped count, model 26/26 shards and 1045 BF16 tensors, service health/models HTTP 200, both TP workers, accepted jemalloc maps, automatic capture `[1,2,4,8,16,24,32,40,48,56,64]`, prefix align, chunked prefill, async scheduling, `FULL_DECODE_ONLY`, and actual PID/device placement.

Keep the unchanged BF16 non-quantized DP1/TP2 mp service and all original Stage 6 runtime/additional/sampling configuration: `max_model_len=66560`, `max_num_seqs=64`, `max_num_batched_tokens=16384`, memory utilization `0.90`, prefix caching align, chunked prefill, async scheduling, EP/MTP/quantization off, `VLLM_PLUGINS=fl`, `USE_FLAGGEMS=0`, `SOC_VERSION=ascend910_93`.

Create one fresh service and fresh cache roots. Run all 16 O8 warm-up cells first, in this exact order:

```text
I1024:  C1, C8, C32, C64
I4096:  C1, C8, C32, C64
I16384: C1, C8, C32, C64
I65536: C1, C8, C32, C64
```

Use exact Frozen tokenizer/random dataset, output 8, `random_range_ratio=0`, `random_prefix_len=0`, prompts/concurrency equal to C, request rate `inf`, `temperature=1`, `top_p=1`, `top_k=0`, `ignore_eos=true`, seed `INPUT + CONCURRENCY`, and frozen request ID/order. Any true functional failure or `UNRESOLVED_PROVENANCE` immediately stops the Task and forbids O1024.

Only after all 16 O8 cells pass, run all 16 O1024 cells once in the same order with output 1024. Do not reuse historical or supplemental cells, restart/clear caches, change parameters, or enter performance/capacity, prefix lifecycle, EP2, another model, source/tokenizer repair, sampling changes or moving-upstream work.

Enforce all original HTTP/error, exact prompt/output counts, length finish, nonempty/readability, finite values, loop/repetition, contamination/stale-state, graph/runtime ownership, TP health, CPU fallback, no `vllm_ascend`, no FlagGems, chunked/async and shutdown gates. `TOKENIZER_NATIVE_UFFFD` exempts only corruption attribution.

## Result boundary

Create exactly one immutable Result for this Task. Preserve fresh-root manifests/checksums, D-036 composite mapping and PID placement, mount scope, jemalloc reconstruction, exact provenance source/runtime audit, raw/canonical IDs, collision evidence, all U+FFFD chains, workload/cell/request artifacts, graph/chunked/async summaries, deviations, shutdown state and Code/Control/Evidence pointers.

Codex1 Acceptance remains `PENDING` until independent review. STOP at mapping ambiguity, race, unproven provenance, post-tokenizer corruption, true functional/output-quality failure, Frozen/runtime drift, forbidden fallback, graph failure, contamination, OOM or any first attributable blocker. After PASS or STOP, stop new workload, cleanly shut down only Task-owned resources, publish exactly one Result, sync `results/INDEX.md` and Control, then STOP. Do not create another Task or enter a later Stage.
