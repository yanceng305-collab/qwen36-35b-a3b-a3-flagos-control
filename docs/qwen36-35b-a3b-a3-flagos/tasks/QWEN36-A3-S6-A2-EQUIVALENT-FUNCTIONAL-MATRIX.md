# QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX

Status：**ENDED / STOP / FORMALLY REVIEWED — DO NOT RESUME**

Execution agent：Codex2

Formal Review：[`REVIEW-QWEN36-A3-STAGE6-STOP-20260826.md`](../reviews/REVIEW-QWEN36-A3-STAGE6-STOP-20260826.md)

First blocker：`I1024 / C64 / O8` request index `34` decoded output contains `29` U+FFFD characters. The formal boundary ends at that failed cell; `I1024 / C32 / O8` is the last successful formal cell. Remaining O8 and all O1024 execution are diagnostic-only. Do not resume this Task; the only Ready next Task is [`QWEN36-A3-S6-UFFFD-OUTPUT-CHAIN-DIAGNOSTIC`](QWEN36-A3-S6-UFFFD-OUTPUT-CHAIN-DIAGNOSTIC.md).

## Unified identity

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

Formal prerequisite：[`Stage 5 Acceptance`](../reviews/REVIEW-QWEN36-A3-STAGE5-SERVE-CORRECTNESS-ACCEPTANCE-20260826.md).

Accepted runtime artifact identity：

```text
source: e610a990d785356bf51a3cad50219d4c03310a31
tree: 609ff1ad0f08239f353cb4d8774e504b4deba03b
wheel sha256: 2fcf788660f3fe42b364bc60d593ee1b9b634fc0632de58c444d961bff4aa1bd
model: /data/tiankuan/zyg/FL/workspace/Qwen3.6-35B-A3B
```

Frozen Validation Baseline：

```text
source: e610a990d785356bf51a3cad50219d4c03310a31
tree: 609ff1ad0f08239f353cb4d8774e504b4deba03b
wheel: vllm_plugin_fl-0.2.0+ge610a990d-cp311-cp311-linux_aarch64.whl
wheel SHA-256: 2fcf788660f3fe42b364bc60d593ee1b9b634fc0632de58c444d961bff4aa1bd
last pre-change tracked reference: 032fddc91b6d013b98aed8e64ff05b54d1435648
last reference tree: 463806ef18e5e31006cd4f59e6a5261fc65cea4a
later upstream/tracked branch movement: OUT OF SCOPE / IGNORE FOR EXECUTION
```

## Objective

Restore the colleague-documented A2 main DP1/TP2 service contract on A3 and prove functional correctness for all 16 cells:

```text
Input:       1024 / 4096 / 16384 / 65536 tokens
Concurrency: 1 / 8 / 32 / 64
Output:      1024 tokens per request
Cells:       16
```

This Task is functional reproduction only. The benchmark client may emit timing fields as incidental raw data, but no throughput, latency, capacity, startup, or FL/native performance claim may be made.

## Formal A2 contract source

The contract below is frozen from:

- [`A2-REFERENCE.md`](../A2-REFERENCE.md);
- the three SHA-256-registered A2 source documents in [`research/SOURCE-MATERIALS.md`](../research/SOURCE-MATERIALS.md), especially deployment guide section 8.2;
- PR #404 historical body/source preserved by the frozen project evidence;
- upstream vLLM `v0.20.2@bc150f50299199599673614f80d12a196f377655` random-dataset and `vllm bench serve` implementation.

The A2 documents do not freeze a total order for the 16 cells. This A3 Task therefore freezes an explicit resource-escalating order below for reproducibility; it is an A3 procedural contract, not a claim about the colleague's historical cell order.

## Entry and identity gate

- Sync latest Control and follow `AGENTS.md` navigation.
- Stage 6 uses the Frozen Validation Baseline above. Do not live-query future tracked HEAD/PR/base as an execution gate; do not inspect later upstream diffs.
- Always reuse the Stage 5 Accepted `e610a990...` wheel/runtime. Never label `032fddc9...` as wheel source; do not rebuild `032fddc9...` or any later upstream commit.
- Later branch/PR/base movement is intentionally out of scope and must not trigger STOP, moving-head review, eager/graph/serve rerun, source switch, or rebuild.
- Reuse the Accepted container/runtime if it remains present and exact. If it is absent, reconstruct only from the Accepted reconstruction; stop on image/CANN/Python/torch/torch_npu/vLLM/Triton/wheel/device-mapping drift.
- Read-only preflight the current authorized idle two-device scope. Do not kill, pause, reset, preempt, or inspect unrelated workload content.

## Gate F0 — runtime, model, cache, and command freeze

Before service launch, prove continuity of all Stage 5 S0 invariants:

- Frozen source/tree and exact Accepted wheel filename/SHA-256 match this Task;
- `torch_npu` import, NPU available, device count 2, both device identities;
- accepted wheel hash and site-packages origin; no source-tree/editable shortcut;
- no installed/importable `vllm-ascend` / `vllm_ascend`; no FlagGems; `USE_FLAGGEMS=0`;
- PlatformFL / WorkerFL / ModelRunnerFL / GraphWrapper and A3 `_C_ascend`/OPP origins;
- model 26/26 shards, 1045 BF16 tensors, non-quantized identity;
- exact image tag/digests/ID, container ID, runtime tuple, visible-device scope and host family.

Create new Task-dedicated, initially empty vLLM and Triton cache roots. They must be isolated from vLLM-Ascend, other FL versions, other software tuples, and Stage 5 Evidence. Keep one service instance and the same cache roots for all warm-up and O1024 cells; do not clear or restart between cells. Preserve the cache roots after the run.

Save the actual container invocation or an unambiguous Accepted reconstruction pointer plus all task-specific substitutions. Save a command artifact containing every environment variable and the exact serve command. The effective service contract is:

```text
model=/data/tiankuan/zyg/FL/workspace/Qwen3.6-35B-A3B
served-model-name=qwen3.6-35b-a3b-fl
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
no manual cudagraph_capture_sizes
expected automatic capture=[1,2,4,8,16,24,32,40,48,56,64]
seed=0
EP off / MTP off / quantization off
```

Freeze and record the A2 runtime controls that remain applicable:

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

Also freeze:

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

Do not enable `multistream_overlap_shared_expert`, `enable_async_exponential`, `pa_shape_list`, `npugraph_ex`, `weight_nz_mode=2`, or `TASK_QUEUE_ENABLE=2`.

Gate F0 passes only after the service is ready, health/models return HTTP 200, effective chunked prefill and async scheduling are both true, graph mode is `FULL_DECODE_ONLY`, and both TP workers complete automatic capture through 64 without forbidden fallback.

## Gate F1 — reproducible workload generation and O8 warm-up

Use the image's exact vLLM 0.20.2 `vllm bench serve` random dataset against `/v1/completions`. For each cell:

```text
tokenizer = exact model tokenizer at the accepted model path
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
request-id prefix = s6-i${INPUT}-c${CONCURRENCY}-o${OUTPUT}-
```

The seed controls dataset/prompt generation; do not claim it makes sampled output token-identical. vLLM 0.20.2 RandomDataset generates deterministic independent token-id sequences from the per-cell seed and request index, decodes/re-encodes them, and reports actual prompt-token usage. Save the exact tool source/version, resolved command, tokenizer identity, per-request ID/order, prompt-token count, and prompt hash/token manifest. Confirm prompts are unique within each cell and do not create a shared fixed prefix. Do not rely on prefix hits for success.

Before O1024, run the same 16 cells with `OUTPUT=8` as the explicit shape/JIT warm-up matrix. Warm-up is functional preparation only and is excluded from O1024 acceptance metrics.

Freeze both warm-up and main cell order as:

```text
I1024:  C1, C8, C32, C64
I4096:  C1, C8, C32, C64
I16384: C1, C8, C32, C64
I65536: C1, C8, C32, C64
```

If a warm-up cell fails, STOP at that first attributable blocker and do not start O1024. Preserve all completed cells and Evidence.

## Gate F2 — 16-cell O1024 functional matrix

Run all 16 cells once in the frozen order with `OUTPUT=1024`. The canonical command shape is:

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

Explicit `top_p=1` and `top_k=0` freeze the vLLM/OpenAI defaults documented by the A2 source instead of relying on implicit server generation config; they do not change the A2 sampling semantics.

For every cell and every request, functional PASS requires:

- HTTP/API success with no request error;
- exact `usage.prompt_tokens == INPUT`;
- exact completion-token count `== 1024`;
- `finish_reason=length` or an equivalent raw response field proving length termination under `ignore_eos`;
- nonempty output and a preserved token/text record or hash;
- no Unicode replacement-character corruption, NaN/Inf in recorded numeric fields/logs, all-identical-token loop, or visibly pathological repeated 8-gram behavior;
- no cross-request output/state contamination.

The A2 documents do not define a numeric threshold for "pathological 8-gram repetition". Do not invent one and call it the colleague's rule. Save deterministic repetition statistics and the exact validator implementation; any suspicious output is a STOP/Review item with raw token/text Evidence, not a silent pass.

Save a 16-row summary and per-request raw/detailed artifacts. Timing and throughput fields may be preserved for audit but are explicitly **not evaluated** in this Task.

## Gate F3 — service-path ownership, long-prefill behavior, and shutdown

Prove from worker/server evidence, not only API results:

- FL-local GraphWrapper and Ascend `NPUGraph` ownership on both TP workers;
- automatic capture list exactly includes `[1,2,4,8,16,24,32,40,48,56,64]` on both workers;
- prefill uses eager/`forward_mode=NONE`, decode uses real `forward_mode=FULL` replay; no silent eager fallback;
- 16K/64K requests exercise chunked prefill with the frozen 16,384-token chunk boundary;
- async scheduling is effective and used;
- requested C1/C8/C32/C64 cells complete; record observed runtime batch/replay sizes without assuming they equal requested concurrency at every step;
- prefix caching is enabled in `align` mode, while independent prompts prevent the matrix from depending on prefix reuse; this does not constitute aligned-prefix lifecycle Acceptance;
- no `507011`, invalid address, OOM, NaN/Inf, CPU fallback, `vllm_ascend`, FlagGems activation, stale state, or unrelated workload impact.

After all cells, stop accepting requests, complete in-flight work, cleanly shut down EngineCore/APIServer, close the port, release this Task's NPU processes, and preserve the accepted container/wheel/cache/Evidence. Do not clean unrelated resources.

## PASS

F0-F3 and all 16 O1024 cells must pass before reporting:

```text
Execution PASS — Stage 6 A2-equivalent DP1/TP2 Functional Matrix (16/16)
```

This PASS proves A3 functional reproduction only. It does not prove performance, capacity, prefix lifecycle, EP2, or FL/native parity.

## STOP

STOP at the first attributable blocker, including Frozen source/artifact/runtime/environment/model/workload drift, service/capture failure, chunked-prefill or async path disabled, any failed/inexact cell, non-finite/corrupt/pathological output, graph fallback, state contamination, illegal address/OOM, or dirty Frozen production source. Upstream branch/PR/base movement is never a blocker for this Task.

Return the completed-cell set, last successful gate/cell, first blocker, root-cause confidence, and one minimum follow-up. Do not silently lower input/output/concurrency, disable prefix/chunked/async/graph, reuse a different source/wheel, restart with changed parameters, or modify production source.

## Prohibited

- performance verdict, capacity measurement, profiling/tuning, or A3 FL/native A/B;
- prefix hit/reset lifecycle beyond recording that the main matrix does not depend on reuse;
- EP2, MTP, quantization, CP, FlashComm, MC2, EPLB, or GLM;
- source patch, Code fork/PR, unrecorded harness correction, or parameter deviation;
- checkout/rebuild/adoption of `032fddc9...` or any later upstream HEAD;
- operations on unrelated NPU/container/process/cache/workload.

## Required Evidence and Result contract

The Result must be sufficient raw material for the final A3 end-to-end reproduction document and must not say only "same as Stage 5". Preserve at least:

### Identity

- Task/run/timestamps; Control parent/current commit;
- Frozen source/tree, last pre-change tracked reference, actual installed wheel/runtime origin;
- wheel filename/SHA-256/inventory; image tag/digests/ID; container ID;
- runtime tuple; host/device family; exact visible-device scope; model identity.

### Commands and configuration

- actual container invocation or accepted reconstruction plus substitutions;
- exact env artifact, serve command, benchmark command, ports, config JSON;
- every non-default server/workload parameter, cache root/state, and command exit code.

### Workload generation

- vLLM tool source/version, tokenizer identity, prompt-generation method;
- seed formula and per-cell seed, prompt/request IDs and order, prompt hashes/token manifest;
- exact token-length verification method, independence/no-shared-prefix checks;
- num prompts, concurrency, output length, sampling contract and warm-up definition.

### Results

- warm-up and O1024 16-row tables; per-cell/per-request pass/fail;
- HTTP/API status, actual prompt/output token counts, finish reason, output/token correctness;
- finite/corruption/repetition checks and validator version;
- incidental timings clearly labeled non-acceptance data;
- graph/chunked-prefill/async/prefix-effective summaries; error/negative scan; shutdown state.

### Evidence pointers

- Evidence root, main server log, command/config artifact, workload manifest;
- raw result JSON/CSV/table and per-request artifacts;
- graph event summary, checksums/full checksums;
- preserved container/wheel/cache/artifact paths;
- last successful gate/cell and first blocker.

### Deviations and corrections

Every harness correction, retry, rerun, workaround, parameter/source/environment change must record `what / why / impact / Evidence`. Never rely on chat to explain a final Result.

Publish one immutable Result and sync `results/INDEX.md`. Set Codex1 Acceptance to `PENDING`; do not start performance, prefix lifecycle, or EP2.
