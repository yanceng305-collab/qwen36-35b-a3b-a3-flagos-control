# QWEN36-A3-S6-UFFFD-PROSPECTIVE-ROOT-CAUSE-DIAGNOSTIC

状态：**ENDED / TWO DIAGNOSTIC STOPS / FORMALLY REVIEWED NEEDS-FOLLOWUP — DO NOT RESUME**

执行代理：Codex2

Formal Review：[`REVIEW-QWEN36-A3-S6-UFFFD-PROSPECTIVE-ROOT-CAUSE-DIAGNOSTIC-NEW-SERVER-20260827.md`](../reviews/REVIEW-QWEN36-A3-S6-UFFFD-PROSPECTIVE-ROOT-CAUSE-DIAGNOSTIC-NEW-SERVER-20260827.md)

Old-server Result：[`RESULT-QWEN36-A3-S6-UFFFD-PROSPECTIVE-ROOT-CAUSE-DIAGNOSTIC-20260827T113500+0800.md`](../results/RESULT-QWEN36-A3-S6-UFFFD-PROSPECTIVE-ROOT-CAUSE-DIAGNOSTIC-20260827T113500+0800.md)

New-server Result：[`RESULT-QWEN36-A3-S6-UFFFD-PROSPECTIVE-ROOT-CAUSE-DIAGNOSTIC-NEW-SERVER-20260827T173022+0800.md`](../results/RESULT-QWEN36-A3-S6-UFFFD-PROSPECTIVE-ROOT-CAUSE-DIAGNOSTIC-NEW-SERVER-20260827T173022+0800.md)

This Task ended after two bounded STOP Results and must not resume. The only Ready next Task is [`QWEN36-A3-S6-JEMALLOC-RECONSTRUCTION-AND-UFFFD-ROOT-CAUSE-DIAGNOSTIC`](QWEN36-A3-S6-JEMALLOC-RECONSTRUCTION-AND-UFFFD-ROOT-CAUSE-DIAGNOSTIC.md).

Prior immutable diagnostic Result：[`RESULT-QWEN36-A3-S6-UFFFD-OUTPUT-CHAIN-DIAGNOSTIC-20260827T101217+0800.md`](../results/RESULT-QWEN36-A3-S6-UFFFD-OUTPUT-CHAIN-DIAGNOSTIC-20260827T101217+0800.md)

This was one prospective root-cause diagnostic, not a Stage 6 resume or Acceptance Task. Its historical contract is preserved below.

## Unified identity

```text
Control repo:
https://github.com/yanceng305-collab/qwen36-35b-a3b-a3-flagos-control

Implementation repo:
https://github.com/xiemingda-1002/vllm-plugin-FL

Historical reference branch (not an execution gate):
feature/qwen3.6-35b-a3b-ascend-graph-migration

Task ID:
QWEN36-A3-S6-UFFFD-PROSPECTIVE-ROOT-CAUSE-DIAGNOSTIC

Frozen source:
e610a990d785356bf51a3cad50219d4c03310a31

Frozen tree:
609ff1ad0f08239f353cb4d8774e504b4deba03b

Frozen wheel:
vllm_plugin_fl-0.2.0+ge610a990d-cp311-cp311-linux_aarch64.whl

Wheel SHA-256:
2fcf788660f3fe42b364bc60d593ee1b9b634fc0632de58c444d961bff4aa1bd
```

Moving upstream is `OUT OF SCOPE / IGNORE FOR EXECUTION`. Do not query future feature-branch, PR #404 or official-base movement as an execution gate. Do not rebuild or change the frozen source/wheel.

## Pinned long-context skill

Before A3 work, install/load and use exactly:

```text
skill repo: https://github.com/yanceng305-collab/long-context-orchestrator
skill commit: 0bb8a5eda9c46f1b170552ba41b871ba141e04b6
SKILL.md git blob: 41373033056391e3c965120baf7c5861e88fddef
```

Use a Task-owned or otherwise authorized Codex2 checkout/load path; do not follow floating `main` and do not modify an existing installed skill. Record repo URL, resolved commit, load path and `SKILL.md` SHA-256.

Use the project Task work/Evidence hierarchy, not a forced `.agent-work`, for durable `WORKPLAN.md`, `INDEX.md` and bounded research notes. At minimum preserve current phase, service-launch count, completed cells, remaining attempt budget, instrumentation bundle/version, whether U+FFFD appeared, chain completeness and active STOP state.

The formal Task, explicit User dispatch, frozen PASS/STOP gates and Evidence contract take precedence over the skill. The skill manages context only and cannot expand execution authority. Subagents inherit only explicitly assigned read/analysis scope and cannot create permission. Durable memory never replaces Server Evidence, immutable Result or Control rules.

## Objective

Prospectively capture one complete U+FFFD-bearing output chain and locate the earliest evidenced layer where U+FFFD or its invalid precursor first appears:

```text
earliest generated token IDs / token representation
-> tokenizer decode boundary
-> serving response object
-> raw HTTP response chunks/bytes before client parsing
-> parsed OpenAI JSON/events
-> benchmark client accumulator/in-memory text
-> serialized/saved result
-> validator input/result
```

Correlate every layer by exact request ID, service launch and attempt identity. Return exactly one A/B/C/D classification. If C, narrow to the actual evidenced service/model/runtime sublayer as far as the captured chain supports.

## Execution authorization

The User authorizes Codex2, after explicit dispatch, to:

- perform safe read-only A3 preflight;
- start the exact Accepted service on an authorized free two-device scope;
- run only the bounded O8 diagnostic workload below;
- add capture-only Task/Evidence-local runtime/client monkey patches, wrappers or hooks;
- analyze artifacts and publish one immutable diagnostic Result.

This does not authorize production source/model/package edits, wheel rebuild, upstream change, formal Stage 6 execution, performance/capacity, prefix lifecycle, EP2, O1024 or parameter changes.

## Entry and runtime gate

- Sync live Control and follow `AGENTS.md` Codex2 reading order.
- Verify the Frozen source/tree/wheel filename/hash/origin and the full Accepted image/runtime/container/model/device identity from current Control.
- Reuse the Accepted A3 openEuler runtime-access pattern in [`A3-RUNTIME-HANDOFF.md`](../A3-RUNTIME-HANDOFF.md). Resolve only authorized idle devices; do not kill, pause, reset or inspect unrelated workload content.
- After container/service launch, verify `torch_npu` import, `torch.npu.is_available()==True` and device count equals the actual mapped scope before FL/model diagnosis.
- Keep standalone `vllm_fl` resolution at the Accepted wheel/site-packages origin; no installed `vllm-ascend`, no `vllm_ascend`, no FlagGems, `USE_FLAGGEMS=0`.
- Create a new Task-owned work/Evidence/cache hierarchy. Preserve parent Evidence and immutable Results unchanged.
- STOP before mutation if target ownership, device scope or Accepted identity is unclear or drifted.

## Instrumentation boundary

Instrumentation is prospective, diagnostic-only and capture-only.

- Prefer Task/Evidence-local monkey patches, wrappers and diagnostic hooks. A wrapped/copy-derived benchmark client is allowed only if its preserved delta leaves request generation, order, concurrency, sampling, parsing and error semantics unchanged.
- Do not edit installed production files, Frozen source, model files or Accepted wheel. Do not rebuild. Do not use editable/source-tree imports or `PYTHONPATH` shadowing of `vllm_fl` or other production packages.
- Hooks may observe/copy token IDs, token pieces/byte-fallback representation, decoder state/text, serving objects, transport chunks/bytes, parsed JSON/events, client accumulation, serialization and validation. They must not alter token IDs, text, request order, sampling, scheduler, graph, prefix, chunked-prefill, async scheduling or error handling.
- Preserve every instrumentation file/hash, load path, patched symbol/signature, activation proof, behavior, reason, expected/observed side effects, rollback/removal and semantic-impact assessment. Preserve exact benchmark/validator source and hashes.
- Record overhead and possible scheduling perturbation. No performance claim is allowed.
- Permit **one instrumentation-bundle correction total for the entire Task** after a non-generative activation/dry check failure or one incomplete U+FFFD capture. Preserve both versions and `what / why / impact / Evidence`. A second correction, production edit or semantic change is a STOP.
- Do not send an unaccounted generative probe. Every generation request must belong to the frozen cells below and count against the global budget.

For each request preserve earliest token representation, decoder boundary, serving object, raw ordered transport chunks and concatenated bytes before parsing, parsed API events/JSON, client accumulator/in-memory text, serialization input/output, saved result and validator input/result. Preserve token/text/byte/code-point hashes and a layer equality/mismatch table.

## Sampling interpretation boundary

Parent Stage 6 used `temperature=1`. Dataset seed freezes prompt/dataset generation, not sampled output token identity. Before parent `I1024/C64/O8`, the same service ran C1/C8/C32 and `enable_global_stream_random_sample=true` was active.

Every attempt below is therefore a **faithful configuration/workload reproduction**, never exact sampled-token replay. Non-reproduction cannot deny the parent blocker or produce Stage 6 PASS. Reproduction does not automatically prove the same FL/NPUGraph bug; attribution still requires the complete correlated chain.

## Frozen diagnostic workload

All workload is diagnostic-only and uses the exact parent Stage 6 service/workload contract:

- input exactly `1024`, output exactly `8`;
- BF16 non-quantized DP1/TP2, distributed executor `mp`;
- `max_model_len=66560`, `max_num_seqs=64`, `max_num_batched_tokens=16384`, `gpu_memory_utilization=0.90`;
- prefix caching `align`, chunked prefill and async scheduling enabled;
- `FULL_DECODE_ONLY`, no manual capture sizes, automatic capture through 64;
- parent random-dataset/tokenizer generation and seed formula `INPUT + CONCURRENCY`;
- `temperature=1`, `top_p=1`, `top_k=0`, `ignore_eos=true`;
- `enable_global_stream_random_sample=true` and all other frozen Stage 6 controls;
- unique attempt-aware request IDs with a manifest mapping to cell/index/prompt hash.

### Strategy S1 — direct target attempts

- At most one fresh service launch.
- Run `I1024/C64/O8` once.
- If no complete classifiable U+FFFD chain is captured and no STOP condition holds, the same service may run `I1024/C64/O8` once more.
- S1 maximum：2 cells / 128 requests / 2 target attempts.

### Strategies S2/S3 — parent-prehistory attempts

Only while no complete chain exists and same-service history remains a material hypothesis:

- At most two fresh service launches.
- In each launch run exactly `I1024/C1/O8 -> I1024/C8/O8 -> I1024/C32/O8 -> I1024/C64/O8` on the same service without restart or cache clearing between cells.
- Each sequence：4 cells / 105 requests / 1 target attempt.
- Instrument all requests. A complete classifiable U+FFFD chain in a prehistory cell triggers the same immediate STOP.

### Hard global cap

```text
service launches:           <= 3
target I1024/C64/O8 cells:  <= 4
prehistory sequences:       <= 2
total workload cells:       <= 10
total generation requests:  <= 338
all outputs:                exactly O8
instrumentation correction: <= 1 bundle correction total
```

Strategies/cells may be skipped but never substituted or exceeded. Each fresh launch uses a new Task-owned attempt/cache namespace while retaining the exact Accepted image/runtime/model/wheel/config.

## Immediate STOP rules

Stop scheduling new requests/cells and enter analysis/Result when:

1. one U+FFFD-bearing response has a complete chain sufficient to locate the earliest changed layer;
2. any launch/target/prehistory/cell/request budget is exhausted;
3. Frozen identity, service parameter, runtime mapping, device ownership or safety invariant drifts;
4. a non-U+FFFD service/HTTP/runtime/cell failure becomes the attributable blocker;
5. capture would require production edit/rebuild, parameter/feature change, O1024, another matrix cell or budget expansion;
6. instrumentation changes generation/request/error semantics or cannot expose a required layer safely.

If U+FFFD appears with an incomplete chain, preserve the attempt and pause new workload. Use the one correction only if still available and safe. If already consumed, or the corrected bundle still cannot capture a complete chain, STOP D. A real workload failure is not an instrumentation-correction retry.

Immediate STOP means no new scheduling. Already-issued concurrent requests may be safely drained or terminated only to preserve a coherent Task record. Then shut down only Task-owned service/process/port/device resources and preserve Evidence.

## Classification

Return exactly one:

- **A — tokenizer/decoder-native**：the captured sampled token sequence, decoded with the exact frozen tokenizer, natively and reproducibly explains U+FFFD without a downstream/server transformation. Record the token sequence and tokenizer semantics.
- **B — benchmark/client/reconstruction**：an earlier serving/wire layer is clean and U+FFFD first appears in transport parsing, event aggregation, client reconstruction, serialization/save or validator handling.
- **C — service/model/runtime**：the first evidenced invalid precursor/change is before a demonstrably clean client boundary. Narrow it to the supported layer, such as an anomalous model sampled-token sequence not explained by tokenizer-native semantics, serving-side decode, async output processing, OpenAI serving object/serialization, graph/state, instrumentation side effect or another evidenced server layer.
- **D — unresolved**：no U+FFFD within budget, incomplete chain, unsafe/unavailable capture, identity drift, exhausted budget or competing earliest-layer hypotheses.

If sampled-token versus decoder/server behavior cannot be separated, report D. Never write `FL/NPUGraph bug` without layer-specific Evidence.

`DIAGNOSTIC PASS — A/B/C` closes only the diagnostic classification. It is never Stage 6 PASS. `DIAGNOSTIC STOP — D / UNRESOLVED` preserves the parent blocker and keeps Stage 6 stopped.

## Required Evidence and Result

Publish one immutable diagnostic Result and sync `results/INDEX.md`, with Codex1 Acceptance `PENDING`. Preserve at least:

- explicit dispatch, live Control parent/current identity, Task/run/timestamps and pinned skill provenance;
- Frozen source/tree/wheel/origin and full Accepted image/runtime/container/device/model identities;
- safe preflight, actual service invocation/config/env/port/cache roots and exit codes;
- instrumentation bundles/hashes/activation/corrections/semantic-impact records;
- exact workload source/version/commands, request/prompt/token manifests, seeds, service/attempt/prehistory order;
- complete request-linked output-chain artifacts and byte/token/text/code-point comparison tables;
- server/worker/client/validator logs, graph/chunked/async continuity Evidence, bounded negative scans and Task-scoped shutdown;
- authorized versus actual launches/cells/requests, skipped budget and reason;
- A/B/C/D, earliest evidenced changed layer, confirmed/excluded/remaining hypotheses and root-cause confidence;
- last successful diagnostic step, first blocker/STOP trigger and one Codex1 decision request;
- Code/source, Control and Evidence three pointers; Code PR=`N/A`.

Every correction/deviation records `what / why / impact / Evidence`. Codex2 must not create the next Task or resume Stage 6.

## Prohibited

- full 16-cell O8 matrix, any O1024, other input lengths, performance/capacity, prefix lifecycle, EP2 or later stage;
- lower concurrency/input/output, greedy sampling, changed seeds/sampling or graph/prefix/chunked/async disablement;
- production source/model/package edit, wheel rebuild, upstream switch, new baseline or Code branch/PR;
- overwrite of parent Evidence or immutable Results;
- unbounded retry, unrecorded instrumentation correction or workload after a complete classifiable chain;
- treating diagnostic output as Stage 6 PASS or Acceptance.
