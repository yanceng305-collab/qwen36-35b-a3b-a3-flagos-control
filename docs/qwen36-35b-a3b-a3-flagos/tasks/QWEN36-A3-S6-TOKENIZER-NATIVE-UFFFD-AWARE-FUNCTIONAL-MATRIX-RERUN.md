# QWEN36-A3-S6-TOKENIZER-NATIVE-UFFFD-AWARE-FUNCTIONAL-MATRIX-RERUN

状态：**ENDED / STOP / FORMALLY REVIEWED — DO NOT RESUME**

执行代理：Codex2（ended run `20260828T161700+0800`）

Decision：[`D-034 / STAGE6-TOKENIZER-NATIVE-UFFFD-SEMANTICS`](../DECISIONS.md#d-034--stage-6-tokenizer-native-ufffd-semantics) — **APPROVED / provenance-aware branch**

Validator contract：[`STAGE6-TOKENIZER-NATIVE-UFFFD-VALIDATOR-CONTRACT.md`](../reconstruction/STAGE6-TOKENIZER-NATIVE-UFFFD-VALIDATOR-CONTRACT.md)

Diagnostic Formal Acceptance：[`REVIEW-QWEN36-A3-S6-JEMALLOC-RECONSTRUCTION-AND-UFFFD-ROOT-CAUSE-DIAGNOSTIC-20260828.md`](../reviews/REVIEW-QWEN36-A3-S6-JEMALLOC-RECONSTRUCTION-AND-UFFFD-ROOT-CAUSE-DIAGNOSTIC-20260828.md)

This Task ended at F0 before readiness due device-scope mapping drift. See the immutable Result and Formal Review. Do not resume it; the only next route is `QWEN36-A3-S6-DEVICE-MAPPING-AND-PROVENANCE-CORRELATION-FOLLOWUP`.

## Unified identity

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

Manifest digest / Arm64 platform digest / image ID:
sha256:442363921166771eb82baeec9c1ac0381f46fb830ead8d0e072df6e925f2a958
sha256:a55b2b0597f9fdd1882de9bf3b7ebc395dd77c1ca49f251d0cb759d7b2c1a807
sha256:8e7823878349b37d9900984555e28381c25681f88ff678cb4a86f7d1674a67c1

Model:
/data/tiankuan/zyg/FL/workspace/Qwen3.6-35B-A3B (BF16, non-quantized)

Last pre-change tracked reference/tree (historical only):
032fddc91b6d013b98aed8e64ff05b54d1435648
463806ef18e5e31006cd4f59e6a5261fc65cea4a
```

Later implementation branch, PR #404, feature-branch, upstream, or official-base movement is `OUT OF SCOPE / NOT AN EXECUTION GATE`. Do not query it as a gate, change source, rebuild the wheel, or rerun Stage 3/4/5 because it moved.

## Objective and acceptance boundary

Rerun Stage 6 from the beginning and prove functional correctness for the complete A2-equivalent DP1/TP2 matrix under unchanged Frozen source/artifact/runtime/model/service/workload identities and D-034 provenance-aware U+FFFD semantics.

```text
Input:       1024 / 4096 / 16384 / 65536
Concurrency: 1 / 8 / 32 / 64
Output:      1024
Total:       16 formal O1024 cells
```

This Task is functional reproduction only. It does not authorize performance/capacity, profiling/tuning, prefix lifecycle, EP2, another model, tokenizer/source repair, sampling changes, new workloads, or moving-upstream work.

Historical parent Result and all post-STOP cells remain immutable historical evidence. They cannot count toward this rerun. Create fresh Task-owned work, run, Evidence, artifact, cache, and Result roots; do not overwrite or reuse historical roots as Acceptance Evidence.

## Entry, server safety, and reconstruction

Before mutation:

- live-query and sync Control `main`; read root `AGENTS.md`, root `README.md`, `STATUS.md`, this exact Task, D-034, the diagnostic Formal Acceptance/Result, parent Stage 6 Result/Review, `A2-TO-A3-VALIDATION-DELTA.md`, `A3-RUNTIME-HANDOFF.md`, `reconstruction/A3-STAGE1-2-ACCEPTED-RUNTIME.md`, and `REPOSITORY-AND-EVIDENCE-RULES.md`;
- confirm this Task remains `READY / ONLY NEXT TASK` and the User dispatched this exact ID; otherwise STOP before A3 mutation;
- perform read-only safe device inventory for model, logical/physical mapping, health, occupancy, and owner; use only the minimum authorized idle two-device scope; never kill, pause, reset, preempt, or alter unrelated workloads;
- create one fresh Task-owned container from the exact Accepted image/runtime identity and fresh Task-owned roots; never reuse a dirty or uncertain container;
- verify the Frozen wheel/site-packages origin and hash, model identity, runtime tuple, device mapping, and all required no-drift invariants.

Use the already Accepted clean-container jemalloc compatibility-path method. Keep:

```text
LD_PRELOAD=/usr/lib/aarch64-linux-gnu/libjemalloc.so.2
```

unchanged. In the Task-owned container only, verify the image-provided `/usr/lib64/libjemalloc.so.2` object and reconstruct the accepted compatibility path to it. Preserve file/realpath/hash/architecture/loader Evidence, non-generative preload activation, and positive relevant-process maps. Do not mutate the host, derive an image, change service config, research a second method, or reopen jemalloc as a separate Task.

## Gate F0 — frozen runtime, model, cache, and service admission

Prove and save:

- Frozen source/tree and exact Accepted wheel filename/SHA-256;
- exact Accepted image tag/digests/ID and runtime tuple;
- `torch_npu` import, `torch.npu.is_available() == True`, device count exactly matching the two mapped devices, and device identities;
- site-packages FL wheel ownership with no editable/source-tree shortcut;
- no installed/importable `vllm-ascend` / `vllm_ascend`; no FlagGems; `USE_FLAGGEMS=0`;
- PlatformFL / WorkerFL / ModelRunnerFL / GraphWrapper and A3 `_C_ascend`/OPP ownership;
- model root `/data/tiankuan/zyg/FL/workspace/Qwen3.6-35B-A3B`, 26/26 shards, 1045 BF16 tensors, non-quantized identity;
- fresh initially empty Task-owned vLLM/Triton/cache roots and exact container/device/work/Evidence identities.

Use one service instance and the same fresh cache roots for the complete O8 warm-up and, only if allowed, the O1024 matrix. Do not clear caches or restart between cells.

The frozen service contract remains:

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

Keep all other original Stage 6 environment variables and additional config unchanged, including `VLLM_PLUGINS=fl`, `USE_FLAGGEMS=0`, `SOC_VERSION=ascend910_93`, prefix align, chunked prefill, async scheduling, graph mode, CPU-binding configuration, sampling configuration, and request order.

F0 passes only after health/models are HTTP 200, both TP workers are healthy, automatic capture through 64 completes on both workers, the accepted jemalloc object is mapped in relevant processes, and no forbidden fallback or drift exists.

## Gate F1 — complete O8 warm-up from the beginning

Run all 16 O8 warm-up cells in this frozen order:

```text
I1024:  C1, C8, C32, C64
I4096:  C1, C8, C32, C64
I16384: C1, C8, C32, C64
I65536: C1, C8, C32, C64
```

Use the original workload generation and sampling contract: exact Frozen tokenizer; random dataset; `random_input_len=INPUT`; `random_output_len=8`; `random_range_ratio=0`; `random_prefix_len=0`; `num_prompts=max_concurrency=CONCURRENCY`; `request_rate=inf`; `temperature=1`; `top_p=1`; `top_k=0`; `ignore_eos=true`; dataset seed `INPUT + CONCURRENCY`; original request ID/order convention.

All functional validators, including D-034 below, apply to O8. If any O8 cell triggers a true functional failure, immediately STOP and do not enter O1024. A `TOKENIZER_NATIVE_UFFFD` classification alone is not a corruption failure, but any other failed readability/output-quality/functional gate remains a failure.

## Gate F2 — 16-cell O1024 formal matrix

Only after all 16 O8 cells pass, run all 16 O1024 cells once in the same frozen order and with the same workload/sampling contract except `random_output_len=1024`.

For every request, retain and enforce all original Stage 6 requirements:

- HTTP/API correctness and request success/error semantics;
- exact `usage.prompt_tokens == INPUT`;
- exact completion token count `== OUTPUT`;
- expected length finish reason under `ignore_eos`;
- nonempty output and readability/final-output quality;
- finite logprobs and no NaN/Inf in numeric fields or logs;
- no all-identical-token/pathological loop;
- deterministic repetition statistics and no pathological repetition/repeated 8-gram behavior;
- no cross-request contamination, stale output, or stale state;
- no graph/runtime ownership drift, worker failure, CPU fallback, `vllm_ascend`, or FlagGems.

Do not treat `TOKENIZER_NATIVE_UFFFD` as unconditional PASS: it exempts only the corruption-attribution predicate. The response must still pass every other functional/output-quality gate.

## D-034 — mandatory provenance-aware U+FFFD validator

For ordinary responses with no U+FFFD, do not expand instrumentation without need. For every response whose final text contains U+FFFD, capture at minimum:

```text
request ID
actual generated token IDs
independent exact-Frozen-tokenizer decode
serving response text
SSE/JSON text
raw HTTP decoded representation
client parser/accumulator text
saved result text
validator input text
text/codepoint equality across layers
classification: TOKENIZER_NATIVE_UFFFD or POST_TOKENIZER_CORRUPTION
earliest changed layer, if any
```

The independent decode must use the exact Frozen tokenizer and the actual generated IDs for that request.

Classify `TOKENIZER_NATIVE_UFFFD` only when independent native decode itself contains U+FFFD and its text/codepoints exactly equal every downstream representation through validator input, with no post-tokenizer mutation. Record it, but do not fail the corruption-attribution gate solely because U+FFFD exists.

Classify `POST_TOKENIZER_CORRUPTION`, fail the corruption gate, preserve earliest changed-layer Evidence, and immediately STOP if:

- independent native decode does not contain U+FFFD but a downstream layer first introduces it;
- serving/SSE/HTTP/client/saved/validator text or codepoints differ from independent native decode; or
- any post-tokenizer text mutation is observed.

Instrumentation must be capture-only, light, Task/Evidence-local, fully identified and impact-audited. It must not alter generated tokens, tokenizer return, response content, sampling, scheduling, request order/concurrency, graph, prefix, chunked prefill, async scheduling, or error semantics.

## Gate F3 — runtime-path ownership and shutdown

Prove from server/worker Evidence:

- FL-local GraphWrapper and Ascend NPUGraph ownership on both TP workers;
- automatic capture through 64 on both workers;
- eager/NONE prefill and real FULL decode replay with no silent eager fallback;
- 16K/64K chunked prefill at the frozen boundary;
- async scheduling is enabled and effectively used;
- prefix align remains enabled while independent prompts prevent reliance on prefix reuse;
- all requested cells complete without illegal address, OOM, CPU fallback, stale state, unrelated-workload impact, `vllm_ascend`, or FlagGems.

On PASS or STOP, stop accepting new work, finish only in-flight Task work as appropriate, cleanly shut down Task EngineCore/APIServer, close the port, release Task NPU processes, and preserve required Task Evidence/artifacts/cache. Do not alter unrelated resources.

## PASS and STOP

PASS requires F0-F3, all 16 O8 cells, and all 16 formal O1024 cells to pass:

```text
Execution PASS — Stage 6 Tokenizer-Native-UFFFD-Aware Functional Matrix (16/16)
```

This is Codex2 execution status only. Codex1 Acceptance remains `PENDING`; do not claim Formal Acceptance.

STOP at the first attributable blocker, including frozen identity/runtime/model/workload drift, service/capture failure, a true O8 or O1024 functional failure, `POST_TOKENIZER_CORRUPTION`, non-finite/pathological output, graph fallback, state contamination, illegal address/OOM, or dirty production source. Return the completed-cell set, last successful gate/cell, first blocker, earliest changed layer when relevant, root-cause confidence, and minimum follow-up.

## Required Result and final boundary

Preserve exact identities, commands/config, safe inventory, fresh roots, jemalloc reconstruction Evidence, workload manifests, prompt/request/token counts, raw/detailed per-request results, validator implementation/version, all U+FFFD provenance bundles, graph/chunked/async summaries, negative scans, shutdown state, checksums, deviations/corrections, and Code/Control/Evidence three pointers.

Publish one new immutable Result and sync `results/INDEX.md`/Control. Set Codex1 Acceptance to `PENDING`. After Control sync, STOP. Do not create or execute performance/capacity, prefix lifecycle, EP2, tokenizer/source repair, sampling change, new-model, or moving-upstream work.
