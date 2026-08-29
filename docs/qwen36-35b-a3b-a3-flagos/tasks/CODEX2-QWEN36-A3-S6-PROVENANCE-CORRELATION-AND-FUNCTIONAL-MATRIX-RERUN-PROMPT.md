# Codex2 Prompt — QWEN36-A3-S6-PROVENANCE-CORRELATION-AND-FUNCTIONAL-MATRIX-RERUN

状态：**DISPATCHABLE ONLY AFTER EXPLICIT USER DISPATCH**

User formal dispatch:

**Execute only `QWEN36-A3-S6-PROVENANCE-CORRELATION-AND-FUNCTIONAL-MATRIX-RERUN` in this completely new Codex2 session. Complete exactly one immutable Result and Control sync, then STOP. Do not execute any other Task.**

```text
Control repo:
https://github.com/yanceng305-collab/qwen36-35b-a3b-a3-flagos-control

Implementation repo:
https://github.com/xiemingda-1002/vllm-plugin-FL

Exact Task ID:
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

This is a completely new session. Do not use prior chat, agent memory, unsynced local Results, supplemental cell counts, or server-local assumptions as formal truth.

Before any A3 mutation:

1. Live-query and sync current Control `main`.
2. Read `AGENTS.md`, root `README.md`, `STATUS.md`, this exact Task, D-034, D-036, the unsynced D-035 Control note, the supplemental mapping record, the ended Task contract, `A3-RUNTIME-HANDOFF.md`, `A2-TO-A3-VALIDATION-DELTA.md`, reconstruction docs, and `REPOSITORY-AND-EVIDENCE-RULES.md`.
3. Load the pinned `long-context-orchestrator` repository in a Task-owned path:

```text
repo:
https://github.com/yanceng305-collab/long-context-orchestrator
commit:
0bb8a5eda9c46f1b170552ba41b871ba141e04b6

SKILL.md expected SHA-256:
f4e15f8c5f097d6aec0fe42b58e1cb5386d3104a02b5ebb502e8f7b0041af1f2
```

After loading, compute the `SKILL.md` SHA-256 and require it to equal:

```text
f4e15f8c5f097d6aec0fe42b58e1cb5386d3104a02b5ebb502e8f7b0041af1f2
```

If the computed hash differs, STOP before any A3 workload mutation. Record repository URL, resolved commit, Task-owned load path, and computed hash. Maintain Task-owned `WORKPLAN.md` and `INDEX.md`. Across compaction preserve current phase/gate, selected physical/logical scope, container/service identity, PID placement, canonical request-ID rule/proof, completed O8/O1024 cells, current/last-success cell, `TOKENIZER_NATIVE_UFFFD`, `UNRESOLVED_PROVENANCE`, `POST_TOKENIZER_CORRUPTION`, first blocker, STOP state, Evidence pointers, and Control/Result sync state.

The skill manages context/state only. The Formal Task, D-034, D-036, User dispatch, PASS/STOP rules and Evidence contract are authoritative. Skill or subagents cannot expand authorization. After formal STOP do not continue workload. Durable notes do not replace formal Evidence or immutable Result.

Confirm this exact Task is still `READY / Awaiting explicit User dispatch — ONLY NEXT TASK`, this message is explicit User dispatch, no immutable Result for this Task exists, no other active session is executing it, and all Frozen identities are unchanged. If any condition fails, STOP before container/workload mutation. There may be only one active formal session. Use unique run ID, container, work, Evidence, artifact and cache roots. If Control changes or a race is detected, stop new workload, preserve Evidence, do not overwrite remote Control, and report the race.

## D-036 device mapping

Perform read-only inventory of host model, physical NPU, host logical IDs, health, occupancy and owner. Select the minimum authorized idle two-device scope and record it.

A numeric difference between host logical IDs, container-visible IDs and torch indices is not itself failure. Suspend formal workload and perform one bounded read-only mapping diagnosis. Use host physical/logical inventory, device-node major/minor identity, container `--device` configuration, process namespace/device visibility where available, `ASCEND_VISIBLE_DEVICES`, `ASCEND_RT_VISIBLE_DEVICES`, torch_npu count/names, TP rank-to-visible-index, HCCL/runtime rank-device evidence and other deterministic runtime evidence.

Direct `npu-smi info proc` or direct PID-to-NPU reporting is preferred when supported but is not mandatory when unsupported. A composite proof is acceptable when deterministic, auditable, internally consistent, and it demonstrates that the Task-visible devices correspond to the authorized host devices. A single Task-local correction is allowed for visibility environment, container device selection, service-launch visibility, or local-index interpretation, provided the same authorized idle physical devices remain used and no unrelated resource is disturbed.

Do not hard-code `0,1` or `14,15`. Re-prove mapping after any correction. Actual APIServer, EngineCore and TP-worker placement must equal the authorized composite mapping before readiness/workload acceptance.

Read-only inspection required for safe inventory, health, occupancy, ownership and PID-placement verification is allowed. Never kill, pause, reset, preempt, mutate, or otherwise disturb unrelated devices, processes, containers, caches or workloads.

STOP only if authorized devices cannot be safely identified, mapping remains ambiguous after bounded diagnosis, unauthorized/occupied devices are used and cannot be corrected safely, correction requires unrelated-workload mutation, or Frozen source/model/runtime change is required. Do not STOP merely because a preferred inspection API is unsupported.

Use the accepted narrowed bind `/data/tiankuan:/data/tiankuan`; all model, wheel, work, Evidence, artifact and cache paths must be beneath `/data/tiankuan`. Do not restore `/data:/data`, mutate host, derive an image, or change Frozen source/model/runtime/service/workload. Reuse Accepted jemalloc compatibility reconstruction and keep `LD_PRELOAD=/usr/lib/aarch64-linux-gnu/libjemalloc.so.2` unchanged.

## Provenance correlation

Before U+FFFD classification, audit exact Frozen vLLM 0.20.2, FL serving, SSE/raw HTTP, client/save and Task-local instrumentation code/runtime that creates bare IDs, vLLM IDs, `cmpl-<request-id>` and `cmpl-<request-id>-0`.

No substring/`contains`, `startsWith`, blind prefix stripping, edit distance, timestamp proximity, order-only attribution or heuristic matching. Preserve every raw ID exactly.

Define a canonical request identity only when exact source/runtime Evidence proves a deterministic reversible or uniquely attributable transform with no collision in the cell. Record raw IDs, transform direction, exact source location/version, mapping table, collision check and impact audit. Canonicalization affects Evidence correlation only; it must not change request IDs, OpenAI response IDs, generated token IDs, tokenizer return, service/client behavior, sampling, order, concurrency, scheduling, graph, workload or validator functional semantics.

If the relation cannot be proven in the bounded audit, classify affected chains `UNRESOLVED_PROVENANCE`, preserve raw Evidence, fail the provenance gate and STOP. Do not claim `TOKENIZER_NATIVE_UFFFD` or `POST_TOKENIZER_CORRUPTION` from an uncorrelated chain.

When proven, preserve for every U+FFFD response all raw IDs, canonical key, generated token IDs, exact Frozen-tokenizer native decode, serving object, SSE/JSON, raw HTTP decoded representation, client parser/accumulator, saved result, validator input and codepoint equality. Apply D-034: identical native U+FFFD across all layers is `TOKENIZER_NATIVE_UFFFD` and does not alone fail corruption attribution; downstream introduction, mismatch or post-tokenizer mutation is `POST_TOKENIZER_CORRUPTION` and immediate FAIL/STOP. All other functional/output-quality gates remain mandatory.

Instrumentation is capture-only, light, Task/Evidence-local, source/runtime-identified and impact-audited. It must not alter generation, IDs, tokenizer return, response content, sampling, order, concurrency, scheduling, graph, prefix, chunked prefill, async scheduling or error semantics.

## F0 and full Stage 6

Verify exact Frozen source/tree/wheel/image/model/runtime identity, installed site-packages FL origin, no `vllm-ascend`/`vllm_ascend`, no FlagGems, torch_npu availability/count, model 26/26 shards and 1045 BF16 tensors, service health/models HTTP 200, both TP workers, automatic capture `[1,2,4,8,16,24,32,40,48,56,64]`, prefix align, chunked prefill, async scheduling, `FULL_DECODE_ONLY`, accepted jemalloc maps and actual PID placement.

Keep the unchanged BF16 non-quantized DP1/TP2 mp service contract, `max_model_len=66560`, `max_num_seqs=64`, `max_num_batched_tokens=16384`, memory utilization `0.90`, prefix caching align, chunked prefill, async scheduling, `enforce_eager=False`, EP/MTP/quantization off, `VLLM_PLUGINS=fl`, `USE_FLAGGEMS=0`, `SOC_VERSION=ascend910_93`, all original Stage 6 environment/additional configuration and frozen sampling settings.

Use one fresh service and fresh cache roots. Run all 16 O8 warm-up cells first in this order:

```text
I1024:  C1, C8, C32, C64
I4096:  C1, C8, C32, C64
I16384: C1, C8, C32, C64
I65536: C1, C8, C32, C64
```

O8 uses exact Frozen tokenizer/random dataset, output 8, `random_range_ratio=0`, `random_prefix_len=0`, prompts/concurrency equal to C, request rate `inf`, `temperature=1`, `top_p=1`, `top_k=0`, `ignore_eos=true`, seed `INPUT + CONCURRENCY`, and frozen request ID/order. Any true functional failure or `UNRESOLVED_PROVENANCE` immediately stops the Task and forbids O1024.

Only after all 16 O8 cells pass, run all 16 O1024 cells once in the same order with output 1024. Do not reuse historical or supplemental cells, restart/clear caches, change parameters, or enter performance/capacity, prefix lifecycle, EP2, another model, source/tokenizer repair, sampling changes or moving-upstream work.

Enforce all original HTTP/error, exact prompt/output counts, length finish, nonempty/readability, finite-number, loop/repetition, contamination/stale-state, graph/runtime ownership, TP health, CPU-fallback, no `vllm_ascend`, no FlagGems, chunked/async and shutdown gates. `TOKENIZER_NATIVE_UFFFD` exempts only corruption attribution.

## Result boundary

Create exactly one new immutable Result for this Task. Preserve fresh-root manifests/checksums, composite mapping proof, PID placement, mount scope, jemalloc reconstruction, exact ID transform audit, raw/canonical IDs, all U+FFFD chains, workload/cell/request artifacts, graph/chunked/async summaries, deviations and shutdown state, plus Code/Control/Evidence pointers.

Codex1 Acceptance remains `PENDING` until independent review. STOP at mapping ambiguity, race, unproven provenance, post-tokenizer corruption, true functional/output-quality failure, runtime/Frozen drift, forbidden fallback, graph failure, contamination, OOM or any first attributable blocker. After PASS or STOP, stop new workload, cleanly shut down only Task-owned resources, sync `results/INDEX.md` and Control, leave Codex1 Acceptance PENDING, and STOP. Do not create another Task.
