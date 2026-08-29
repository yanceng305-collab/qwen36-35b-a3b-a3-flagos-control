# Codex2 Prompt — QWEN36-A3-S6-DEVICE-MAPPING-AND-PROVENANCE-CORRELATION-FOLLOWUP

状态：**DO NOT DISPATCH — historical prompt; Task ended with unsynced local STOP**

This historical prompt is retained for audit only. Do not dispatch it. The only next route is `QWEN36-A3-S6-PROVENANCE-CORRELATION-AND-FUNCTIONAL-MATRIX-RERUN`.

Historical User formal dispatch:

**Execute only `QWEN36-A3-S6-DEVICE-MAPPING-AND-PROVENANCE-CORRELATION-FOLLOWUP` in this completely new Codex2 session. Complete its one immutable Result and Control sync, then STOP. Do not execute any other Task.**

```text
Control repo:
https://github.com/yanceng305-collab/qwen36-35b-a3b-a3-flagos-control

Implementation repo:
https://github.com/xiemingda-1002/vllm-plugin-FL

Exact Task ID:
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

This is a completely new session. Do not infer facts or authorization from prior chat, another Codex2 session, or server-local assumptions.

Before A3 mutation, load the pinned context/state-management skill in a Task-owned path:

```text
repo:
https://github.com/yanceng305-collab/long-context-orchestrator

commit:
0bb8a5eda9c46f1b170552ba41b871ba141e04b6
```

Record the repository URL, resolved commit, Task-owned load path, and the `SKILL.md` SHA-256. Maintain Task-owned `WORKPLAN.md` and `INDEX.md`. Across context compaction, these durable files must preserve at least:

```text
current phase/gate
selected physical/logical NPU scope
container/service identity
PID placement
canonical request-ID rule and proof state
completed O8/O1024 cells
current/last-success cell
TOKENIZER_NATIVE_UFFFD events
UNRESOLVED_PROVENANCE state
POST_TOKENIZER_CORRUPTION state
first blocker
STOP state
Evidence pointers
Control/Result sync state
```

The skill manages context and execution state only. The Formal Task, D-034, D-035, User dispatch, PASS/STOP rules, Evidence contract, and Control remain authoritative. The skill or any subagent cannot expand authorization. After a formal STOP, do not continue workload. Durable notes do not replace formal Evidence or the immutable Result.

Before any A3 mutation, live-query and sync Control `main`, then read `AGENTS.md`, root `README.md`, `STATUS.md`, this exact Task, D-034, the ended Task's immutable Result and Formal Review, the supplemental concurrent-run record, `A3-RUNTIME-HANDOFF.md`, `A2-TO-A3-VALIDATION-DELTA.md`, reconstruction docs, and `REPOSITORY-AND-EVIDENCE-RULES.md`.

Confirm all of the following before mutation:

- this exact Task is still `READY / Awaiting explicit User dispatch — ONLY NEXT TASK`;
- this message is the User's explicit dispatch of this exact Task;
- no immutable Result for this Task already exists;
- no other active session is running this exact Task;
- Frozen source/tree/wheel/image/model identity remains unchanged.

If any check fails, STOP before creating a container or starting workload. There may be only one active formal Codex2 session for this Task. Use a unique run ID, container name, work root, Evidence root, artifact root and cache roots. If Control changes during execution or a race is detected, stop new workload, preserve local Evidence, do not overwrite remote Control, and report the race.

This Task is the only follow-up route. It combines:

```text
device-scope correction
-> exact provenance canonical-ID audit/correction
-> F0
-> complete Stage 6 from the beginning
```

The prior `20260828T161700+0800` Result is an immutable valid STOP at F0. The concurrent `20260828T180824+0800` run is supplemental only; its C1/C8/C32 cells are not formal progress and must not be reused.

## Device and mount correction

Start with read-only safe inventory of host model, physical NPU IDs, host logical IDs, health, occupancy and owner. Select the minimum authorized idle two-device scope dynamically and record the selected physical/logical mapping.

Create a fresh Task-owned container from the exact Accepted image. Use the User-authorized narrowed bind:

```text
/data/tiankuan:/data/tiankuan
```

All model, wheel, work, Evidence, artifact and cache paths must be below `/data/tiankuan`. This is a Task/runtime mount-scope narrowing because `/data:/data` caused shared-mount propagation/overlay ENOSPC. Do not restore `/data:/data`, mutate the host, derive an image, or change Frozen source/model/runtime/service/workload.

Do not hard-code `0,1` or `14,15`. Preserve the resolved host logical scope through container creation and service launch. Use container-local numbering only if the exact runtime proves deterministic renumbering. Before readiness, prove:

- `torch_npu` import succeeds;
- `torch.npu.is_available() == True`;
- `torch.npu.device_count()` equals the actual mapped device count;
- device names and physical/logical mapping;
- APIServer, EngineCore and both TP-worker PIDs' actual host logical-device placement.

Actual service/worker PID placement must equal the authorized selected scope before readiness or workload is accepted. Any mismatch is immediate F0 STOP.

Read-only inspection required for safe device inventory, health, occupancy,
ownership and PID-placement verification is allowed.

Never kill, pause, reset, preempt, mutate, or otherwise disturb unrelated
devices, processes, containers, caches, or workloads.

Reuse the Accepted jemalloc reconstruction method inside this Task container only. Keep exactly:

```text
LD_PRELOAD=/usr/lib/aarch64-linux-gnu/libjemalloc.so.2
```

Verify the image-provided `/usr/lib64/libjemalloc.so.2`, reconstruct the accepted compatibility path only inside the container, and preserve file/realpath/ownership/hash/architecture/loader and relevant-process map Evidence. No host mutation, derived image, second method, or service-config change.

## Provenance correction

Before classifying U+FFFD, perform a bounded audit of the exact Frozen vLLM 0.20.2, FL serving, SSE/HTTP, client/save and Task-local instrumentation path that emitted bare IDs, `cmpl-<request-id>`, and `cmpl-<request-id>-0`.

Fuzzy matching is forbidden. Do not use substring/contains, startsWith, prefix stripping, edit distance, timestamp proximity, order-only guesses or any heuristic attribution. Preserve every raw identifier exactly as emitted.

Only if source/runtime Evidence proves a deterministic transform may you define a canonical request identity. The transform must be reversible or uniquely attributable, collision-free within each cell, and recorded with exact source location/version, direction, mapping table and collision check. Raw IDs remain in Evidence. Canonicalization affects Evidence correlation only; it must not change request IDs, OpenAI response IDs, generated token IDs, service/client behavior, sampling, order, concurrency, scheduling, graph, tokenizer return or validator functional semantics.

If the relationship cannot be proven, classify affected chains `UNRESOLVED_PROVENANCE`, preserve raw evidence, fail the provenance gate and STOP. Do not claim `TOKENIZER_NATIVE_UFFFD` or `POST_TOKENIZER_CORRUPTION` from an uncorrelated chain.

When the relationship is proven, for every U+FFFD response preserve request ID, all raw IDs, generated token IDs, exact Frozen-tokenizer independent native decode, serving object, SSE/JSON, raw HTTP decoded representation, client parser/accumulator, saved result, validator input, canonical mapping and text/codepoint equality.

Apply D-034 exactly: native decode itself containing U+FFFD with identical downstream text/codepoints is `TOKENIZER_NATIVE_UFFFD` and does not alone fail corruption attribution; a downstream introduction, mismatch or post-tokenizer mutation is `POST_TOKENIZER_CORRUPTION`, immediate corruption FAIL/STOP. All other functional/output-quality gates remain mandatory.

Instrumentation must be capture-only, light, Task/Evidence-local, source/runtime-identified and impact-audited. It must not alter generation, tokenizer return, response content, request IDs, sampling, order, concurrency, scheduling, graph, prefix, chunked prefill, async scheduling or error semantics.

## Frozen service and F0

Use the exact Accepted BF16 non-quantized standalone FL service:

```text
served-model-name=qwen3.6-35b-a3b-fl
DP1 / TP2 / distributed executor backend mp
max_model_len=66560
max_num_seqs=64
max_num_batched_tokens=16384
gpu_memory_utilization=0.90
prefix caching enabled; mamba_cache_mode=align
chunked prefill enabled
async scheduling enabled
enforce_eager=False
cudagraph_mode=FULL_DECODE_ONLY
automatic capture=[1,2,4,8,16,24,32,40,48,56,64]
seed=0
EP/MTP/quantization off
VLLM_PLUGINS=fl
USE_FLAGGEMS=0
VLLM_WORKER_MULTIPROC_METHOD=spawn
SOC_VERSION=ascend910_93
OMP_NUM_THREADS=1
OMP_PROC_BIND=false
TASK_QUEUE_ENABLE=1
PYTORCH_NPU_ALLOC_CONF=expandable_segments:True
HCCL_OP_EXPANSION_MODE=AIV
HCCL_BUFFSIZE=512
LD_PRELOAD=/usr/lib/aarch64-linux-gnu/libjemalloc.so.2
```

Also preserve the original Stage 6 additional configuration: CPU binding, global stream random sample, contiguous mrope copy, int32 slot mapping source, NPU memory profiling, hybrid sampling stream return edge, and interleaved graph task update. Do not enable multistream overlap, async exponential, pa shape list, `npugraph_ex`, weight NZ mode 2, or `TASK_QUEUE_ENABLE=2`.

F0 passes only after exact identity/origin checks, model 26/26 shards and 1045 BF16 tensors, no vllm-ascend/`vllm_ascend`, no FlagGems, service health/models HTTP 200, both TP workers healthy, automatic capture through 64 on both workers, effective prefix/chunked/async/FULL_DECODE_ONLY, accepted jemalloc maps, and actual PID-to-host-logical placement equal the authorized scope.

## Complete Stage 6 rerun

Use one service and the same fresh cache roots throughout. Run all 16 O8 warm-up cells first, in this exact order:

```text
I1024:  C1, C8, C32, C64
I4096:  C1, C8, C32, C64
I16384: C1, C8, C32, C64
I65536: C1, C8, C32, C64
```

For O8 use the exact Frozen tokenizer and random dataset, `random_input_len=INPUT`, `random_output_len=8`, `random_range_ratio=0`, `random_prefix_len=0`, prompts/concurrency equal to C, request rate `inf`, `temperature=1`, `top_p=1`, `top_k=0`, `ignore_eos=true`, dataset seed `INPUT + CONCURRENCY`, and frozen request-ID/order convention. A true functional failure or `UNRESOLVED_PROVENANCE` in any O8 cell immediately stops the Task and forbids O1024.

Only after all 16 O8 cells pass, run the 16 formal O1024 cells once in the same order with `random_output_len=1024`. Do not reuse any supplemental cell or restart/clear caches/change parameters between cells.

For every O8 and O1024 request enforce HTTP/API correctness, request/error semantics, exact prompt/output token counts, expected length finish, nonempty/readable output, finite logprobs, no NaN/Inf, loops, pathological repetition/repeated 8-grams, contamination/stale state, graph/runtime ownership, TP health, CPU-fallback absence, no `vllm_ascend`, no FlagGems, and all frozen Stage 6 requirements. `TOKENIZER_NATIVE_UFFFD` exempts only corruption attribution, not other output-quality gates.

The formal matrix is functional only. Do not run performance/capacity, profiling, prefix lifecycle, EP2, another model, source/tokenizer repair, sampling changes, workloads beyond O8/O1024, or moving-upstream work.

## Result and STOP boundary

Create exactly one immutable Result for this new Task. Preserve fresh-root manifest/checksums, device inventory and PID placement, mount scope, jemalloc reconstruction, raw and canonical IDs, source-backed transform audit, all U+FFFD chains, workload/cell/request artifacts, graph/chunked/async summaries, deviations and shutdown state, plus Code/Control/Evidence pointers.

PASS is possible only after F0-F3, all 16 O8 cells, and all 16 O1024 cells pass. Codex1 Acceptance remains PENDING until independent review.

STOP immediately at any mapping mismatch, race, unresolved provenance, post-tokenizer corruption, true functional/output-quality failure, runtime drift, forbidden fallback, graph failure, contamination, OOM or other first attributable blocker. Stop new workload, cleanly shut down only Task-owned resources, sync `results/INDEX.md` and Control, leave Codex1 Acceptance PENDING, and STOP. Do not create another Task or enter a later Stage.
