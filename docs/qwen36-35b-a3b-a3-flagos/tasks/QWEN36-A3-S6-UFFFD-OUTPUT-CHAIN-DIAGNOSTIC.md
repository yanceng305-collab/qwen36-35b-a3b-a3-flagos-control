# QWEN36-A3-S6-UFFFD-OUTPUT-CHAIN-DIAGNOSTIC

状态：**ENDED / DIAGNOSTIC STOP D / FORMALLY REVIEWED NEEDS-FOLLOWUP — DO NOT RESUME**

执行代理：Codex2

Formal Review：[`REVIEW-QWEN36-A3-S6-UFFFD-OUTPUT-CHAIN-DIAGNOSTIC-20260827.md`](../reviews/REVIEW-QWEN36-A3-S6-UFFFD-OUTPUT-CHAIN-DIAGNOSTIC-20260827.md)

Immutable Result：[`RESULT-QWEN36-A3-S6-UFFFD-OUTPUT-CHAIN-DIAGNOSTIC-20260827T101217+0800.md`](../results/RESULT-QWEN36-A3-S6-UFFFD-OUTPUT-CHAIN-DIAGNOSTIC-20260827T101217+0800.md)

This Task ended after Phase A with `D / UNRESOLVED`; Phase B was not run and this contract must not be resumed. Later diagnostics completed the prospective chain；current routing is governed by `STATUS.md` and D-034.

Parent immutable Result：[`RESULT-QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX-20260826T180105+0800.md`](../results/RESULT-QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX-20260826T180105+0800.md)

This was an Evidence-first diagnostic, not a Stage 6 resume, rerun or Acceptance Task. Its historical contract is preserved below.

User formal dispatch statement：execute only `QWEN36-A3-S6-UFFFD-OUTPUT-CHAIN-DIAGNOSTIC` after the User explicitly sends this Task to Codex2. A Control link, Ready status, Codex1 review or prior Stage 6 dispatch is not execution authorization.

## Unified identity

```text
Control repo:
https://github.com/yanceng305-collab/qwen36-35b-a3b-a3-flagos-control

Implementation repo:
https://github.com/xiemingda-1002/vllm-plugin-FL

Historical reference branch (not an execution gate):
feature/qwen3.6-35b-a3b-ascend-graph-migration

Task ID:
QWEN36-A3-S6-UFFFD-OUTPUT-CHAIN-DIAGNOSTIC

Frozen source:
e610a990d785356bf51a3cad50219d4c03310a31

Frozen tree:
609ff1ad0f08239f353cb4d8774e504b4deba03b

Frozen wheel:
vllm_plugin_fl-0.2.0+ge610a990d-cp311-cp311-linux_aarch64.whl

Wheel SHA-256:
2fcf788660f3fe42b364bc60d593ee1b9b634fc0632de58c444d961bff4aa1bd
```

Moving upstream remains `OUT OF SCOPE / IGNORE FOR EXECUTION`. Do not query a future feature-branch HEAD, PR #404 HEAD/mergeability or official-base movement as an execution gate. Do not rebuild or change the frozen source/wheel.

## Objective

For the parent run's first blocker only — `I1024 / C64 / O8`, literal request index `34`, decoded output containing `29` U+FFFD characters — determine the earliest evidenced layer at which U+FFFD or its invalid precursor appears:

```text
raw generated token/output boundary
-> raw HTTP response bytes
-> parsed API response
-> benchmark client in-memory text
-> benchmark saved result
-> tokenizer decode / encode / re-decode
-> validator input and classification
```

Return exactly one evidence-backed classification:

- **A — tokenizer/decoder-native behavior**;
- **B — benchmark/harness/reconstruction behavior**;
- **C — actual service/model/runtime anomaly**;
- **D — existing/targeted Evidence insufficient; unresolved**.

Do not use the label “FL/NPUGraph Unicode corruption bug” unless raw Evidence actually isolates that layer.

## Phase A — mandatory read-only audit of existing Evidence

Before any service launch or workload rerun, read the original Evidence root:

```text
/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX/evidence/20260826T180105p0800
```

Create one new Task-specific diagnostic Evidence root. Copy or extract only bounded diagnostic material; preserve original files unchanged and record source/destination hashes.

Focus on `functional/i1024_c64_o8`, request index `34`, and its exact saved request ID. Preserve and compare every available layer:

1. original manifest/checksum status, failing `result.json`, prompt manifest, detailed/raw files and client stdout/stderr;
2. exact request ID, prompt hash/token IDs, actual prompt/output token counts, response order and index convention;
3. raw HTTP response bytes and parsed OpenAI response JSON, if present;
4. benchmark in-memory response text versus serialized/saved text and the exact reconstruction/save path;
5. generated output token IDs/logprob token representation, if present;
6. exact frozen tokenizer files/hash/config, `convert_ids_to_tokens`, decode, encode and re-decode results without silently normalizing U+FFFD;
7. validator source/path/version/hash, exact input, settings and byte-identical re-execution result;
8. every Evidence-local runtime/client instrumentation file, hash, load path, behavior, reason and impact;
9. a byte/code-point equality/mismatch table locating the earliest changed layer.

If Phase A proves A, B or C, **STOP without running workload**. Publish one immutable diagnostic Result and sync Control. If any required layer is unavailable and classification remains D, record the missing layer before considering Phase B.

## Phase B — one conditional faithful cell only

Phase B is allowed only if Phase A demonstrates that a missing raw wire and/or generated-token layer prevents classification. It permits **one attempt of exactly one cell**:

```text
Input: 1024
Concurrency: 64
Output: 8
dataset seed: 1088
request-id prefix: s6-i1024-c64-o8-
```

Reuse the exact parent Stage 6 service/workload contract:

- Accepted image/runtime/container/model identity and two-device scope;
- BF16 non-quantized, DP1/TP2, distributed executor `mp`;
- `max_model_len=66560`, `max_num_seqs=64`, `max_num_batched_tokens=16384`, `gpu_memory_utilization=0.90`;
- prefix caching enabled with `mamba_cache_mode=align`;
- chunked prefill and async scheduling enabled;
- `FULL_DECODE_ONLY`, no manual capture sizes, automatic capture through 64;
- same random-dataset generation, prompt order/hash manifest, temperature 1, top-p 1, top-k 0 and `ignore_eos`;
- same applicable environment controls from the parent Task.

Use one independent new Evidence root and new isolated cache roots. Do not reuse parent result output paths. Runtime-only diagnostic instrumentation may be used only if production source/wheel remain unchanged, installed `vllm_fl` continues to come from the Accepted wheel/site-packages rather than a source-tree/editable shortcut, and every instrumentation file/hash/load path/behavior/why/impact is recorded.

For all 64 requests preserve raw HTTP response bytes before parsing, parsed JSON, request-to-output token IDs at the earliest available server boundary, tokenizer token/decode/re-encode data, client in-memory text, saved text/JSON and validator input/result. Correlate every layer by exact request ID.

After this one cell, shut down the Task service, record task-scoped process/port/device release, classify A/B/C/D, publish one immutable diagnostic Result, and STOP. Do not repeat the cell in this Task.

Retaining C64 is mandatory because reducing concurrency would change the first-failure condition. This diagnostic cell is not a formal Stage 6 cell and cannot make Stage 6 PASS.

## Diagnostic disposition

- `DIAGNOSTIC PASS — A/B/C` requires the earliest changed layer to be supported by byte/token/text Evidence and competing earlier-layer hypotheses to be excluded within the preserved chain.
- `DIAGNOSTIC STOP — D / UNRESOLVED` applies if the necessary layer is absent, the symptom does not reproduce, identities drift, instrumentation cannot capture the layer without source change, or competing hypotheses remain.

In either case, Codex1 must review the diagnostic Result before deciding whether the formal Stage 6 matrix can resume. Codex2 must not resume Stage 6 or create a new task.

## Immediate STOP conditions

STOP and return the preserved facts if:

- frozen source/tree/wheel, accepted image/runtime/container/model or workload identity drifts;
- a production source patch, rebuild, different upstream commit, different image/runtime/model or parameter change would be required;
- existing Evidence proves A/B/C during Phase A;
- the one permitted Phase B cell completes or fails;
- raw layer capture would require lowering concurrency, disabling graph/prefix/chunked/async, entering O1024, or running another matrix cell;
- target/device ownership is unclear or an unrelated workload would be affected.

## Prohibited

- full O8 or O1024 matrix rerun;
- any O1024 request;
- performance/capacity, prefix lifecycle, EP2 or later-stage work;
- source modification, source rebuild, new validation baseline, Code fork/branch/PR;
- graph/prefix/chunked-prefill/async disable or concurrency/input/output reduction;
- using diagnostic output as Stage 6 PASS or Acceptance;
- modifying the parent immutable Result or original Evidence.

## Required diagnostic Result and Evidence

Preserve:

- explicit User dispatch, Task/run/timestamps, live Control parent/current identity;
- frozen source/tree/wheel and full accepted image/runtime/container/device/model identities;
- original and new Evidence roots, manifests and checksums;
- Phase A inventory and missing-layer decision;
- exact failing request ID/index convention and all available raw byte/token/text layers;
- tokenizer files/hash/config and decode/encode/re-decode commands/results;
- benchmark client source/version/hash and save/reconstruction path;
- validator source/version/hash/input/per-check output;
- runtime/client instrumentation identity/hash/behavior/why/impact;
- if Phase B runs: exact one-cell command/config/env/cache roots/exit codes and task-scoped shutdown;
- layer comparison table, classification A/B/C/D, confirmed facts, competing hypotheses and root-cause confidence;
- last successful diagnostic step, first blocker and one Codex1 decision request;
- Code/source, Control and Evidence three pointers; Code PR=`N/A`.

The Result must not claim Stage 6 PASS, performance, capacity, prefix lifecycle, EP2, production-source root cause or any broader runtime defect beyond the layer actually proven.
