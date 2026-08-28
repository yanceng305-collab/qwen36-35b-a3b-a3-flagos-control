# Codex2 Prompt — QWEN36-A3-S6-TOKENIZER-NATIVE-UFFFD-AWARE-FUNCTIONAL-MATRIX-RERUN

状态：**DISPATCHABLE — ONLY AFTER EXPLICIT USER DISPATCH**

User formal dispatch：**Execute only `QWEN36-A3-S6-TOKENIZER-NATIVE-UFFFD-AWARE-FUNCTIONAL-MATRIX-RERUN` in this new Codex2 session. Complete its immutable Result and Control sync, then stop. Do not execute any other Task.**

```text
Control repo:
https://github.com/yanceng305-collab/qwen36-35b-a3b-a3-flagos-control

Implementation repo:
https://github.com/xiemingda-1002/vllm-plugin-FL

Task ID:
QWEN36-A3-S6-TOKENIZER-NATIVE-UFFFD-AWARE-FUNCTIONAL-MATRIX-RERUN

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
/data/tiankuan/zyg/FL/workspace/Qwen3.6-35B-A3B (BF16, non-quantized)

Historical reference branch (not an execution gate):
feature/qwen3.6-35b-a3b-ascend-graph-migration
```

This is a completely new Codex2 execution session. Do not use previous chat or agent memory as project truth.

Before A3 work, live-query and sync latest Control `main`. Read root `AGENTS.md`, root `README.md`, `docs/qwen36-35b-a3b-a3-flagos/STATUS.md`, the exact Task above, D-034 in `DECISIONS.md`, the latest diagnostic Formal Acceptance and immutable Result, the parent Stage 6 Result/Review, `A2-TO-A3-VALIDATION-DELTA.md`, `A3-RUNTIME-HANDOFF.md`, `reconstruction/A3-STAGE1-2-ACCEPTED-RUNTIME.md`, and `REPOSITORY-AND-EVIDENCE-RULES.md`. Confirm the Task is still `READY / ONLY NEXT TASK` and that this prompt is the User's explicit dispatch; otherwise STOP before mutation.

Execute only the exact Task contract. Preserve all Frozen identities above. Do not rebuild the wheel, move to a newer implementation commit, or use future feature-branch/PR #404/upstream movement as an execution gate. Those movements are `OUT OF SCOPE / NOT AN EXECUTION GATE`.

Start with read-only safe A3 device inventory: identify model, physical/logical mapping, health, occupancy, and owner. Use only the minimum authorized idle two-device scope. Never kill, pause, reset, preempt, inspect unrelated workload content, or alter other users' containers/processes/caches.

Create a fresh Task-owned container from the exact Accepted image/runtime identity and fresh Task-owned work, run, Evidence, artifact, and cache roots. Historical Stage 6 roots and parent post-STOP cells cannot be reused as Acceptance Evidence.

Apply the Accepted clean-container jemalloc compatibility-path method. Keep `LD_PRELOAD=/usr/lib/aarch64-linux-gnu/libjemalloc.so.2` unchanged. Verify the image-provided `/usr/lib64/libjemalloc.so.2` and create only the accepted container-local compatibility path in this Task container. Preserve object/realpath/hash/architecture/loader and relevant-process mapping Evidence. No host mutation, derived image, second method, config change, or new jemalloc research Task is allowed.

Reconstruct and verify the exact Frozen BF16 non-quantized DP1/TP2 standalone FL service: no `vllm-ascend`/`vllm_ascend`, no FlagGems, `USE_FLAGGEMS=0`, exact model, prefix caching in align mode, chunked prefill, async scheduling, `FULL_DECODE_ONLY`, automatic capture through 64, and every original Stage 6 environment/additional config and sampling setting. Require `torch_npu` import, NPU availability, mapped device count 2, worker health, service health/models HTTP 200, correct FL/graph/custom-op ownership, and positive jemalloc maps before workload.

Keep the frozen effective service configuration, including:

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

Run Stage 6 from the beginning. First run the complete O8 warm-up in frozen order:

```text
I1024:  C1, C8, C32, C64
I4096:  C1, C8, C32, C64
I16384: C1, C8, C32, C64
I65536: C1, C8, C32, C64
```

Use the frozen random-dataset contract, exact Frozen tokenizer, `temperature=1`, `top_p=1`, `top_k=0`, `ignore_eos=true`, dataset seed `INPUT + CONCURRENCY`, unchanged request order/IDs, and output length 8. If any O8 cell has a true functional failure, immediately STOP and do not enter O1024.

Only if all 16 O8 cells pass, run the 16 formal O1024 cells once in the same frozen order with output length 1024. Keep one service and the same fresh caches throughout; no restart, cache clear, retry with changed parameters, workload substitution, or silent downgrade.

Apply D-034 provenance-aware U+FFFD semantics to O8 and O1024. For every response containing U+FFFD, obtain its actual generated token IDs and independently decode them with the exact Frozen tokenizer. Preserve request ID, generated IDs, independent decode, serving text, SSE/JSON, raw HTTP decoded representation, client parser/accumulator text, saved result, validator input, and cross-layer text/codepoint equality.

If native decode itself contains U+FFFD and every downstream text/codepoint representation is exactly equal with no mutation, classify `TOKENIZER_NATIVE_UFFFD`. Record it, but do not fail the corruption-attribution gate solely because U+FFFD exists. This is not unconditional PASS: continue every other service, request/error, exact prompt/output count, finish reason, nonempty/readability/final-quality, finite logprobs, NaN/Inf, loop, pathological repetition/repeated-8-gram, contamination, stale-state, graph/runtime ownership, TP health, CPU-fallback, `vllm_ascend`, FlagGems, and frozen Stage 6 gate.

If independent native decode has no U+FFFD but a downstream layer first introduces it, any downstream text/codepoints differ from native decode, or any post-tokenizer text mutation exists, classify `POST_TOKENIZER_CORRUPTION`, preserve the earliest changed-layer Evidence, immediately FAIL the corruption gate, and STOP Stage 6.

Keep provenance instrumentation capture-only, light, Task/Evidence-local, identified, and impact-audited. It must not affect generation, tokenizer return, response content, sampling, scheduling, request order/concurrency, graph, prefix, chunked prefill, async scheduling, or error semantics. Ordinary no-U+FFFD responses need no expanded chain capture.

This Task is functional matrix only. Do not run performance/capacity, profiling/tuning, prefix lifecycle, EP2, another model, source/tokenizer repair, sampling modification, workloads beyond O8/O1024, or moving-upstream work.

On PASS or first attributable STOP, preserve all required raw Evidence/checksums and Code/Control/Evidence pointers, cleanly stop only Task-owned service/resources, publish one new immutable Result, sync `results/INDEX.md` and Control, leave Codex1 Acceptance `PENDING`, and then STOP. Do not automatically enter any later Stage or create/dispatch another Task.
