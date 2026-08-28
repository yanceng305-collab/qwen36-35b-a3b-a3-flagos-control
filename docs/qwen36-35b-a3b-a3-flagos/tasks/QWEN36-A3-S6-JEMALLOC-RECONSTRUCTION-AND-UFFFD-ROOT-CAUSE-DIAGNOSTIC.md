# QWEN36-A3-S6-JEMALLOC-RECONSTRUCTION-AND-UFFFD-ROOT-CAUSE-DIAGNOSTIC

状态：**ENDED / DIAGNOSTIC A ACCEPTED / STAGE 6 NOT ACCEPTED — DO NOT RESUME**

执行代理：Codex2

Formal Acceptance：[`REVIEW-QWEN36-A3-S6-JEMALLOC-RECONSTRUCTION-AND-UFFFD-ROOT-CAUSE-DIAGNOSTIC-20260828.md`](../reviews/REVIEW-QWEN36-A3-S6-JEMALLOC-RECONSTRUCTION-AND-UFFFD-ROOT-CAUSE-DIAGNOSTIC-20260828.md)

Immutable Result：[`RESULT-QWEN36-A3-S6-JEMALLOC-RECONSTRUCTION-AND-UFFFD-ROOT-CAUSE-DIAGNOSTIC-20260828T093900+0800.md`](../results/RESULT-QWEN36-A3-S6-JEMALLOC-RECONSTRUCTION-AND-UFFFD-ROOT-CAUSE-DIAGNOSTIC-20260828T093900+0800.md)

This Task ended after one complete classifiable chain and must not resume. Diagnostic A is Accepted within scope；Stage 6 remains STOP / NOT ACCEPTED. Any functional rerun awaits User Decision D-034.

New-server parent Result：[`RESULT-QWEN36-A3-S6-UFFFD-PROSPECTIVE-ROOT-CAUSE-DIAGNOSTIC-NEW-SERVER-20260827T173022+0800.md`](../results/RESULT-QWEN36-A3-S6-UFFFD-PROSPECTIVE-ROOT-CAUSE-DIAGNOSTIC-NEW-SERVER-20260827T173022+0800.md)

This was one combined runtime-reconstruction and U+FFFD root-cause diagnostic. Its historical contract is preserved below.

## Unified identity

```text
Control repo:
https://github.com/yanceng305-collab/qwen36-35b-a3b-a3-flagos-control

Implementation repo:
https://github.com/xiemingda-1002/vllm-plugin-FL

Historical reference branch (not an execution gate):
feature/qwen3.6-35b-a3b-ascend-graph-migration

Task ID:
QWEN36-A3-S6-JEMALLOC-RECONSTRUCTION-AND-UFFFD-ROOT-CAUSE-DIAGNOSTIC

Frozen source:
e610a990d785356bf51a3cad50219d4c03310a31

Frozen tree:
609ff1ad0f08239f353cb4d8774e504b4deba03b

Frozen wheel:
vllm_plugin_fl-0.2.0+ge610a990d-cp311-cp311-linux_aarch64.whl

Wheel SHA-256:
2fcf788660f3fe42b364bc60d593ee1b9b634fc0632de58c444d961bff4aa1bd
```

Moving upstream remains `OUT OF SCOPE / IGNORE FOR EXECUTION`. Do not rebuild or change source/wheel/model/image/runtime, and do not query future branch/PR/base movement as an execution gate.

## Pinned long-context skill

Before A3 work, install/load and use exactly:

```text
skill repo: https://github.com/yanceng305-collab/long-context-orchestrator
skill commit: 0bb8a5eda9c46f1b170552ba41b871ba141e04b6
```

Use a Task-owned/authorized path, record repo/commit/load path/`SKILL.md` hash, and preserve `WORKPLAN.md`/`INDEX.md` in the project Task work/Evidence hierarchy. The skill manages context only. Formal Task/User dispatch/STOP/Evidence rules take precedence; skill/subagents cannot expand authority.

## Objective and phase order

In one Task:

```text
R0: reconstruct the frozen jemalloc preload path in one Task-owned container
-> R1: prove frozen service admission/readiness with jemalloc actually loaded
-> R2: immediately continue the bounded prospective U+FFFD output-chain diagnosis
```

Do not stop after successful R0/R1 merely to report jemalloc correction. If R0 and R1 pass, continue directly to R2.

## Execution authorization

After explicit User dispatch, Codex2 may:

- perform safe read-only new-server A3 preflight and select an authorized free two-device scope;
- create one fresh Task-owned container from the exact Accepted image and runtime-access pattern;
- apply the one path-specific Task-owned-container runtime reconstruction below;
- launch the exact frozen Stage 6 service and run only the bounded O8 diagnostic workload;
- use capture-only Task/Evidence-local runtime/client instrumentation;
- publish one immutable diagnostic Result and Control sync.

No host mutation, derived image, image/source/wheel/model change, upstream switch, production edit, O1024, full matrix, performance, prefix lifecycle or EP2 is authorized.

## Entry identity and safety gate

- Sync live Control and follow `AGENTS.md` Codex2 reading order.
- Target the new-server route `bm-jn-zs-zone1-910C-64G-10-111`; physical 0/1 are prior positive Evidence only and may be reused only if current safe preflight again proves healthy, process-free and authorized. Do not use an alarmed/occupied device or disturb unrelated work.
- Verify exact Accepted image tag/digests/ID, Frozen source/tree/wheel filename/hash/origin, standalone site-packages ownership, full runtime tuple and accepted model identity.
- Use one new Task-owned container and isolated Task work/Evidence/cache roots. Preserve all parent Results/Evidence unchanged.
- STOP before mutation if ownership, device scope or any Frozen identity is unclear or drifted.

## R0 — jemalloc runtime reconstruction correction

The frozen service configuration remains:

```text
LD_PRELOAD=/usr/lib/aarch64-linux-gnu/libjemalloc.so.2
```

Before correction, record for both `/usr/lib64/libjemalloc.so.2` and the frozen target path:

- existence, `lstat`/file type, link target, `realpath`, directory state and permissions;
- package/library ownership if available without installing packages;
- SHA-256 of the resolved object and applicable link/path metadata;
- ELF class/architecture, shared-object type, dependencies and dynamic-loader compatibility.

Only if the frozen target is absent and `/usr/lib64/libjemalloc.so.2` resolves to a verified compatible AArch64 shared object, create the minimum Task-owned-container directory/symlink compatibility path to that verified object.

Requirements:

- keep the frozen `LD_PRELOAD` string unchanged;
- do not overwrite an existing mismatched target;
- do not modify the source library, host, image tag/digest, Frozen source/wheel/model or publish a derived image;
- preserve exact correction command/exit code, before/after state, target/realpath, hashes, ownership/permissions, rollback/removal and semantic-impact record.

Before service launch, prove:

```text
test -e /usr/lib/aarch64-linux-gnu/libjemalloc.so.2 == PASS
realpath frozen target == verified /usr/lib64 object realpath
non-generative loader activation under frozen LD_PRELOAD == PASS
no cannot-preload / ignored / ABI-loader error
```

Then, with the frozen preload effective, run the staged invariant:

```text
python startup == PASS
import torch == PASS
import torch_npu == PASS
torch.npu.is_available() == True
torch.npu.device_count() == 2
both mapped device names == Ascend910_9382
```

This is one separately authorized **runtime filesystem reconstruction correction**. It does not consume the output-chain instrumentation correction. If the verified source object/path is unavailable or incompatible, the frozen target cannot be restored safely, loader/NPU verification fails, or a second/different reconstruction method is required, STOP D.

Do not change `LD_PRELOAD` to `/usr/lib64/libjemalloc.so.2` as a workaround.

## R1 — frozen service admission

Launch the exact frozen Stage 6 service contract:

```text
BF16 non-quantized
DP1 / TP2 / executor mp
max_model_len=66560
max_num_seqs=64
max_num_batched_tokens=16384
gpu_memory_utilization=0.90
prefix caching align
chunked prefill enabled
async scheduling enabled
FULL_DECODE_ONLY
automatic capture through 64
USE_FLAGGEMS=0
LD_PRELOAD=/usr/lib/aarch64-linux-gnu/libjemalloc.so.2
all original Stage 6 env/additional config
```

Admission requires:

- frozen preload path still exists and resolves to the verified object;
- no loader-ignore, missing-object or ABI error;
- positive process-level evidence that jemalloc is loaded for the front end and relevant workers where observable, with PID-to-role mapping;
- service reaches readiness and both TP workers remain healthy;
- Frozen package/model/runtime/config identities remain exact.

If R1 passes, continue directly to R2. If a real non-U+FFFD service/runtime failure becomes the first blocker, STOP D without another workaround.

## R2 — prospective U+FFFD diagnosis

Capture and correlate by exact attempt-aware request ID:

```text
earliest generated token IDs / token representation
-> tokenizer decode boundary
-> serving response object
-> raw HTTP response chunks/bytes before parsing
-> parsed OpenAI JSON/events
-> benchmark client accumulator/in-memory text
-> serialized/saved result
-> validator input/result
```

Instrumentation remains capture-only and Task/Evidence-local. It may observe/copy but cannot alter tokens, text, request order, concurrency, sampling, graph, prefix, chunked prefill, async scheduling, parsing/error semantics, production packages, wheel or model.

Preserve every instrumentation file/hash/load path/patched symbol/behavior/impact/rollback, exact benchmark/validator source and a request-linked byte/token/text/code-point comparison table.

The **output-chain instrumentation correction** remains at most one bundle correction for activation/capture failure. It is independent of and is not consumed by R0 filesystem reconstruction. A second instrumentation correction is STOP.

## Sampling boundary and workload

All attempts are faithful configuration/workload reproductions, not exact sampled-token replay. Dataset seed freezes prompts, not sampled output tokens. Keep:

```text
I1024 / O8
temperature=1
top_p=1
top_k=0
ignore_eos=true
enable_global_stream_random_sample=true
parent RandomDataset/tokenizer/seed/prompt contract
```

The parent new-server run generated zero requests, so this new Task receives the full newly frozen budget:

```text
service launches:                         <= 3
target I1024/C64/O8 cells:                <= 4
same-service prehistory sequences:        <= 2
total workload cells:                     <= 10
total generation requests:                <= 338
all outputs:                              exactly O8
output-chain instrumentation corrections: <= 1
runtime filesystem reconstruction:        1 verified method, separately accounted
```

Strategy S1：on one ready service, run `I1024/C64/O8` at most twice.

Strategies S2/S3 only if needed：at most two same-service `C1 -> C8 -> C32 -> C64` sequences, one fresh service launch/attempt-cache namespace per sequence.

No generation request or activation probe may sit outside the budget. Non-reproduction remains D and cannot deny the historical blocker.

## Immediate STOP

Stop scheduling new work and enter analysis/Result when:

1. the first complete U+FFFD-bearing chain is captured and the earliest changed layer is locatable;
2. any Frozen identity/service/runtime/device/safety invariant drifts;
3. R0 cannot safely restore frozen preload semantics;
4. a real service/runtime/HTTP/cell failure becomes the first blocker;
5. any service/target/prehistory/cell/request budget is exhausted;
6. another reconstruction method, second instrumentation correction, production edit/rebuild, parameter/feature change or broader workaround would be required.

Do not continue to collect additional samples after one complete classifiable chain. If U+FFFD appears with an incomplete chain, pause workload; use the one remaining instrumentation correction only if safe, otherwise STOP D.

On STOP, shut down only Task-owned service/process/port/device resources and preserve all Evidence.

## Classification

Return exactly one:

- **A — tokenizer/decoder-native**;
- **B — benchmark/client/reconstruction**;
- **C — service/model/runtime**, narrowed to the actual evidenced layer;
- **D — unresolved**.

Never label an FL/NPUGraph bug without layer-specific Evidence. Diagnostic A/B/C is not Stage 6 PASS and cannot unlock later stages.

## Required Evidence and Result

Publish one immutable diagnostic Result and sync `results/INDEX.md`, leaving Codex1 Acceptance `PENDING`. Preserve at least:

- explicit dispatch, live Control identity, Task/run/timestamps and pinned skill provenance;
- exact host/device/container/image/runtime/source/tree/wheel/model identity;
- Task-owned container invocation and R0 correction commands/exit codes;
- before/after path state, file/realpath/ownership/hashes/ELF/loader Evidence and rollback;
- non-generative loader proof, corrected staged NPU invariant and process-level jemalloc load Evidence;
- exact service config/env/readiness/worker-health and cache roots;
- separate runtime-reconstruction versus instrumentation-correction accounting;
- exact workload commands/manifests, attempt/prehistory order and actual/remaining budgets;
- complete request-linked output-chain Evidence and comparison tables;
- A/B/C/D, earliest evidenced layer, confirmed/excluded/remaining hypotheses and confidence;
- last successful step, first blocker/STOP trigger and one Codex1 decision request;
- Code/source, Control and Evidence three pointers; Code PR=`N/A`.

Every correction/deviation records `what / why / impact / Evidence`. Codex2 must not create a next Task or resume formal Stage 6.

## Prohibited

- jemalloc-only completion that stops after R0/R1 success;
- changing frozen `LD_PRELOAD` to `/usr/lib64/...` as the preferred route;
- host mutation, derived image, production source/model/package edit, wheel rebuild, upstream/new baseline;
- full matrix, any O1024, other input lengths, performance/capacity, prefix lifecycle or EP2;
- lower parameters, changed sampling/seeds or graph/prefix/chunked/async disablement;
- unbounded retry, second reconstruction method, second instrumentation correction or continued workload after a complete chain;
- treating diagnostic output as Stage 6 PASS or Acceptance.
