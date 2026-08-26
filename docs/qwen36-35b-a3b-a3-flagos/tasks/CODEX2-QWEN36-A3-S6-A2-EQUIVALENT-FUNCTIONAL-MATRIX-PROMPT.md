# Codex2 Prompt — QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX

User formal dispatch：execute only `QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX` after this prompt is explicitly sent to you by the User. Do not enter performance, prefix-lifecycle, EP2, or any later work.

```text
Control repo:
https://github.com/yanceng305-collab/qwen36-35b-a3b-a3-flagos-control

Implementation repo:
https://github.com/xiemingda-1002/vllm-plugin-FL

Historical reference branch (not an execution gate):
feature/qwen3.6-35b-a3b-ascend-graph-migration

Task ID:
QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX
```

This is a new Codex2 execution session. Do not reconstruct facts from prior chat or agent memory. Sync latest Control main, read root `AGENTS.md`, root `README.md`, `STATUS.md`, the exact Stage 6 Task, Stage 5 Formal Acceptance, `A2-REFERENCE.md`, `A2-TO-A3-VALIDATION-DELTA.md`, `A3-RUNTIME-HANDOFF.md`, `REPOSITORY-AND-EVIDENCE-RULES.md`, and reconstruction docs. Control, the exact Frozen commit/artifact, and preserved A3 Evidence are the truth sources; moving upstream is not.

## Objective and non-objective

Prove A3 functional correctness for the colleague-documented DP1/TP2 main service matrix:

```text
Input:       1024 / 4096 / 16384 / 65536 tokens
Concurrency: 1 / 8 / 32 / 64
Output:      1024 tokens per request
Total:       16 cells
```

This is not a performance benchmark. `vllm bench serve` may emit timing fields, but preserve them only as incidental raw data and make no throughput, latency, capacity, startup, or FL/native claim.

Do not run prefix hit/reset lifecycle, EP2, MTP, quantization, profiling/tuning, A3 matched-native comparison, or any later Stage.

## Frozen Validation Baseline

```text
frozen source/tree:
e610a990d785356bf51a3cad50219d4c03310a31
609ff1ad0f08239f353cb4d8774e504b4deba03b

accepted wheel:
vllm_plugin_fl-0.2.0+ge610a990d-cp311-cp311-linux_aarch64.whl

wheel SHA-256:
2fcf788660f3fe42b364bc60d593ee1b9b634fc0632de58c444d961bff4aa1bd

model:
/data/tiankuan/zyg/FL/workspace/Qwen3.6-35B-A3B

last pre-change tracked reference/tree:
032fddc91b6d013b98aed8e64ff05b54d1435648
463806ef18e5e31006cd4f59e6a5261fc65cea4a

later upstream/tracked branch movement:
OUT OF SCOPE / IGNORE FOR EXECUTION
```

Stage 6 uses this Frozen Validation Baseline. Do not live-query future tracked HEAD, PR #404 mergeability, or official base as an execution gate; do not inspect later upstream diffs. Always reuse the Stage 5 Accepted `e610a990...` wheel/runtime. Never label `032fddc9...` as wheel source, and do not rebuild `032fddc9...` or any later upstream commit. Later upstream movement must not trigger STOP or Stage 3/4/5 reruns.

Reuse the exact Accepted container/runtime if present. If absent, reconstruct only from the Accepted reconstruction. STOP on image/CANN/Python/torch/torch_npu/vLLM/Triton/wheel/model/device mapping drift. Read-only preflight and use only the current authorized idle two-device scope; do not kill, pause, reset, preempt, or alter other workloads.

## F0 — runtime and service contract

Prove and save:

- Frozen source/tree and exact Accepted wheel filename/SHA-256 match this prompt;
- `torch_npu` import; NPU available; device count 2; exact device names;
- accepted wheel hash and site-packages origin; no editable/source-tree shortcut;
- `vllm-ascend` absent, `vllm_ascend` not importable, FlagGems absent, `USE_FLAGGEMS=0`;
- PlatformFL / WorkerFL / ModelRunnerFL / GraphWrapper and A3 `_C_ascend`/OPP origins;
- model 26/26 shards, 1045 BF16 tensors, non-quantized identity;
- image tag/digests/ID, container ID, runtime tuple, host/device family and exact visible scope.

Create new initially empty Task-dedicated vLLM and Triton cache roots, isolated from other FL/vLLM-Ascend/software identities. Use one service instance and the same caches for the whole O8 warm-up and O1024 matrix. Do not clear/restart between cells; preserve caches after execution.

Save the actual container invocation or Accepted reconstruction plus exact substitutions, all env vars, the exact serve command, port, effective config, stdout/stderr, and exit identity. Do not write only “same as Stage 5”.

Freeze the service contract:

```text
model=/data/tiankuan/zyg/FL/workspace/Qwen3.6-35B-A3B
served model=qwen3.6-35b-a3b-fl
BF16 non-quantized
DP1 / TP2 / distributed executor mp
max_model_len=66560
max_num_seqs=64
max_num_batched_tokens=16384
gpu_memory_utilization=0.90
prefix caching enabled; mamba_cache_mode=align
chunked prefill enabled
async scheduling enabled
enforce_eager=False
cudagraph_mode=FULL_DECODE_ONLY
no manual capture sizes
expected automatic capture=[1,2,4,8,16,24,32,40,48,56,64]
seed=0
EP/MTP/quantization off
```

Freeze runtime controls:

```text
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

Freeze additional config:

```json
{
  "enable_cpu_binding": true,
  "enable_global_stream_random_sample": true,
  "enable_contiguous_mrope_copy": true,
  "enable_int32_slot_mapping_source": true,
  "enable_npu_memory_profiling": true,
  "enable_hybrid_sampling_stream_return_edge": true,
  "enable_interleaved_graph_task_update": true
}
```

Do not enable shared-expert multistream, async exponential, PA32, `npugraph_ex`, `weight_nz_mode=2`, or `TASK_QUEUE_ENABLE=2`.

F0 passes only when service readiness/health/models return HTTP 200, effective chunked prefill and async scheduling are true, graph mode is `FULL_DECODE_ONLY`, and both TP workers capture the automatic list through 64 without forbidden fallback.

## F1 — deterministic prompt generation and O8 warm-up

Use the image's exact vLLM 0.20.2 `vllm bench serve` random dataset against `/v1/completions`. For every cell freeze:

```text
tokenizer = accepted model tokenizer
dataset = random
random_input_len = INPUT
random_output_len = OUTPUT
random_range_ratio = 0
random_prefix_len = 0
num_prompts = CONCURRENCY
max_concurrency = CONCURRENCY
request_rate = inf
temperature = 1
top_p = 1
top_k = 0
ignore_eos = true
dataset seed = INPUT + CONCURRENCY
request id prefix = s6-i${INPUT}-c${CONCURRENCY}-o${OUTPUT}-
```

The seed controls prompt generation, not sampled-output determinism. Save exact vLLM tool source/version, tokenizer identity, resolved commands, per-cell seed, request IDs/order, API prompt-token counts, and a prompt hash/token manifest. Confirm prompts are unique within each cell and share no fixed prefix; the matrix must not obtain false success from prefix reuse.

Run the full matrix first with `OUTPUT=8` as shape/JIT warm-up. Do not count O8 timing as O1024 acceptance data. Freeze order for both O8 and O1024:

```text
I1024:  C1, C8, C32, C64
I4096:  C1, C8, C32, C64
I16384: C1, C8, C32, C64
I65536: C1, C8, C32, C64
```

If any O8 cell fails, STOP and do not start O1024.

## F2 — O1024 functional matrix

Run all 16 cells once in the frozen order. Use this canonical command shape, substituting the actual preserved Evidence root and unused port:

```bash
INPUT=65536
CONCURRENCY=64
OUTPUT=1024
SEED="$((INPUT + CONCURRENCY))"
RESULT_DIR="${EVIDENCE_ROOT}/functional/i${INPUT}_c${CONCURRENCY}_o${OUTPUT}"

VLLM_PLUGINS=fl vllm bench serve \
  --backend openai \
  --base-url "http://127.0.0.1:${PORT}" \
  --endpoint /v1/completions \
  --model qwen3.6-35b-a3b-fl \
  --tokenizer /data/tiankuan/zyg/FL/workspace/Qwen3.6-35B-A3B \
  --dataset-name random \
  --random-input-len "${INPUT}" \
  --random-output-len "${OUTPUT}" \
  --random-range-ratio 0 \
  --random-prefix-len 0 \
  --num-prompts "${CONCURRENCY}" \
  --max-concurrency "${CONCURRENCY}" \
  --request-rate inf \
  --temperature 1 \
  --top-p 1 \
  --top-k 0 \
  --ignore-eos \
  --seed "${SEED}" \
  --request-id-prefix "s6-i${INPUT}-c${CONCURRENCY}-o${OUTPUT}-" \
  --save-result \
  --save-detailed \
  --result-dir "${RESULT_DIR}" \
  --result-filename result.json
```

Explicit `top_p=1` / `top_k=0` freezes the documented vLLM/OpenAI default semantics. It is not a sampling change.

Every request in every cell must satisfy:

- HTTP/API success, no request error;
- exact `usage.prompt_tokens == INPUT`;
- exact completion-token count 1024;
- `finish_reason=length` or equivalent raw evidence of length termination under `ignore_eos`;
- nonempty preserved output/token record or hash;
- no Unicode replacement corruption, NaN/Inf, all-identical-token loop, visibly pathological repeated 8-gram behavior, or cross-request state contamination.

The A2 source does not define a numeric 8-gram threshold. Do not invent one and attribute it to A2. Preserve deterministic repetition statistics and validator code; suspicious output is a STOP/Review item with raw Evidence.

Save per-request details plus a 16-row table. Preserve incidental timing fields but label them explicitly as non-acceptance data.

## F3 — graph/chunked/async ownership and shutdown

Prove from server/worker evidence:

- FL-local GraphWrapper and Ascend `NPUGraph` on both TP workers;
- automatic captures `[1,2,4,8,16,24,32,40,48,56,64]` on both workers;
- eager/`forward_mode=NONE` prefill and real `forward_mode=FULL` decode replay; no silent eager fallback;
- 16K/64K requests actually use chunked prefill with the 16,384-token boundary;
- async scheduling is effective and used;
- all C1/C8/C32/C64 cells complete; record observed graph batch/replay sizes without assuming requested concurrency equals every runtime batch;
- prefix caching is enabled in `align` mode but independent prompts prevent matrix dependence on hits; do not claim prefix-lifecycle Acceptance;
- no `507011`, invalid address, OOM, NaN/Inf, CPU fallback, `vllm_ascend`, FlagGems activation, stale state, or unrelated workload impact.

Cleanly stop requests and in-flight work, shut down EngineCore/APIServer, close the port, release only this Task's NPU processes, and preserve container/wheel/cache/Evidence.

## PASS / STOP

Only F0-F3 plus all 16 O1024 cells allow:

```text
Execution PASS — Stage 6 A2-equivalent DP1/TP2 Functional Matrix (16/16)
```

This proves functional reproduction only.

STOP at the first attributable Frozen source/artifact/runtime/environment/model/workload, service, capture, chunked/async, per-cell, output, graph/state, illegal-address/OOM, or Frozen-source cleanliness blocker. Upstream branch/PR/base movement is not a blocker. Return completed cells, last successful gate/cell, first blocker, root-cause confidence, and one minimum follow-up. Do not silently lower workload parameters, disable required features, change source/wheel, restart with changed parameters, inspect/adopt later upstream, or patch production source.

## Evidence and immutable Result

The Result must support the final A3 end-to-end reproduction document without chat memory. Preserve:

- Task/run/timestamps, Control parent/current commit;
- Frozen source/tree, last pre-change tracked reference, actual wheel filename/hash/origin/inventory;
- image tag/digests/ID, container ID, full runtime tuple, host/device/visible scope, model identity;
- actual container/env/serve/benchmark commands, configs, port, cache roots/state, exit codes;
- exact prompt-generation method, vLLM tool source/version, tokenizer, seed formula/per-cell seeds, request order/IDs, prompt hash/token manifest, token-length verification and independence checks;
- O8 and O1024 per-cell/per-request results, HTTP status, prompt/output tokens, finish reason, correctness/finite/corruption/repetition checks and validator version;
- graph/chunked-prefill/async/prefix-effective summaries, negative scan and clean shutdown;
- Evidence root, main log, command/config artifacts, raw JSON/CSV/table, graph summary, checksums and full checksums, preserved artifacts/cache, last gate/cell/blocker;
- every retry, rerun, harness correction, workaround, parameter/source/environment deviation as `what / why / impact / Evidence`.

Do not explain missing facts through chat. Publish one immutable Result, sync `results/INDEX.md`, set Codex1 Acceptance `PENDING`, then stop. Do not execute performance, prefix lifecycle, EP2, or any later Task.
