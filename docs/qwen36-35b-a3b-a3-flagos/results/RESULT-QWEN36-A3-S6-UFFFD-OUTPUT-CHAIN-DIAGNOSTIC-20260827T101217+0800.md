# RESULT-QWEN36-A3-S6-UFFFD-OUTPUT-CHAIN-DIAGNOSTIC

Dispatch: User formal dispatch to execute only `QWEN36-A3-S6-UFFFD-OUTPUT-CHAIN-DIAGNOSTIC`, complete immutable diagnostic Result and Control sync, then stop.

Run ID: `20260827T101217+0800`

## Status

- Lifecycle status: `completed`
- Experiment Result: `DIAGNOSTIC STOP — D / UNRESOLVED`
- Classification: **D — existing/targeted Evidence insufficient; unresolved**
- Root-cause confidence: `LOW / NOT CONFIRMED / UNRESOLVED`
- Control Sync: `SYNCED` by the Control commit that first adds this immutable Result
- Codex1 Acceptance: `PENDING`
- Phase A: `COMPLETE — bounded read-only parent Evidence audit`
- Phase B: `NOT RUN`
- Stage 6 PASS claim: `NO`
- Code PR: `N/A`

This Result does not resume Stage 6, does not make Stage 6 PASS, does not run O1024, performance/capacity, prefix lifecycle, EP2 or later stages, and does not modify production source, rebuild the Accepted wheel, inspect future upstream/PR/base movement or create a Code branch/PR.

## Identity

| Field | Value |
| --- | --- |
| Control repo | `yanceng305-collab/qwen36-35b-a3b-a3-flagos-control` |
| Live Control `main` before execution | `09830051a442eb648bc665a3e03a450b21223ce2` / tree `2b270e97c019abfa077fcf1eaded36fb7c4c3d48` |
| Task | `docs/qwen36-35b-a3b-a3-flagos/tasks/QWEN36-A3-S6-UFFFD-OUTPUT-CHAIN-DIAGNOSTIC.md` |
| Live Task status | `READY / Awaiting explicit User dispatch — ONLY NEXT TASK` |
| Frozen source | `xiemingda-1002/vllm-plugin-FL@e610a990d785356bf51a3cad50219d4c03310a31` |
| Frozen tree | `609ff1ad0f08239f353cb4d8774e504b4deba03b` |
| Accepted wheel | `vllm_plugin_fl-0.2.0+ge610a990d-cp311-cp311-linux_aarch64.whl` |
| Accepted wheel SHA-256 | `2fcf788660f3fe42b364bc60d593ee1b9b634fc0632de58c444d961bff4aa1bd` |
| Accepted wheel path | `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S3-TP2-BF16-EAGER/artifacts/20260826T112011p0800/wheels/vllm_plugin_fl-0.2.0+ge610a990d-cp311-cp311-linux_aarch64.whl` |
| Selected image | `quay.io/ascend/vllm-ascend:v0.20.2rc1-a3-openeuler` |
| Manifest digest | `sha256:442363921166771eb82baeec9c1ac0381f46fb830ead8d0e072df6e925f2a958` |
| Local image ID | `sha256:8e7823878349b37d9900984555e28381c25681f88ff678cb4a86f7d1674a67c1` |
| Accepted container | `qw36-a3-s2-gatec-priv-20260826T092617p0800` / `32562c7139600c25e570ec07841713737b0407c7d1bbdc563be41b87ea105f0a` |
| Parent Evidence root | `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX/evidence/20260826T180105p0800` |
| Diagnostic Evidence root | `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S6-UFFFD-OUTPUT-CHAIN-DIAGNOSTIC/evidence/20260827T101217p0800` |

Runtime tuple from parent Evidence: `Python 3.11.15`, `torch 2.10.0+cpu`, `torch-npu 2.10.0`, `transformers 5.5.3`, `triton 3.5.0`, `triton-ascend 3.2.1`, `vLLM 0.20.2+empty`, `vllm-plugin-fl 0.2.0+ge610a990d`.

## Phase A audit

Phase A audited the preserved parent Evidence root without modifying it. The audited first blocker is exactly:

```text
Cell: I1024 / C64 / O8
Dataset seed: 1088
Request index: 34
Request ID: s6-i1024-c64-o8-34
Saved output U+FFFD count: 29
Validator reason: unicode_replacement
```

Available parent layers:

- `functional/i1024_c64_o8/result.json` contains saved `generated_texts[34]`, `input_lens`, `output_lens`, empty `errors`, aggregate timing and throughput fields.
- `functional/i1024_c64_o8/prompt_manifest.json` contains request ID, prompt/token hashes, request count, seed and length checks.
- `functional/summary_o8.json` records validator failure for request index `34`, request ID `s6-i1024-c64-o8-34`, `completion_tokens=8`, `prompt_tokens=1024`, and `replacement_char_count=29`.
- `runtime/server.log` and `runtime/graph_event_summary.json` record request/scheduling identity and runtime-path traces, but not output bytes or generated token IDs.
- `commands/validate_matrix_results.py` shows the validator reads saved `result.json`, encodes the saved text with the tokenizer, counts `"\ufffd"`, and classifies any nonzero count as `unicode_replacement`.

Copied bounded diagnostic artifacts and hashes are preserved in the new Evidence root with `manifest.md`, `layer_audit.md`, `tokenizer_audit.txt`, `current_status.md` and `checksums.txt`.

## Missing output-chain layers

Phase A did not find the required earlier layers:

| Required layer | Status | Impact |
| --- | --- | --- |
| Raw generated token IDs at earliest server/model boundary | Missing | Cannot prove tokenizer/decoder-native behavior or service/model/runtime anomaly. |
| Raw HTTP response bytes before parsing | Missing | Cannot distinguish wire output from client parsing/stream aggregation. |
| Parsed OpenAI response JSON artifact | Missing | Cannot locate whether U+FFFD first appears in parsed API JSON. |
| Benchmark client in-memory text before serialization | Missing | Cannot distinguish client memory text from saved JSON. |
| Benchmark reconstruction/save intermediate artifacts | Missing | Only final saved JSON and stdout/stderr are present. |
| Generated output token IDs or logprob token representation | Missing | `logprobs` was not requested; token layer is absent. |
| Byte-identical validator input chain before saved result | Incomplete | Validator source and summary are present, but earlier layers are already absent. |

The earliest evidenced layer containing U+FFFD is the benchmark saved result. That is not necessarily the earliest actual layer.

## Tokenizer saved-text audit

Tokenizer files were hashed:

| File | SHA-256 |
| --- | --- |
| `tokenizer.json` | `5f9e4d4901a92b997e463c1f46055088b6cca5ca61a6522d1b9f64c4bb81cb42` |
| `tokenizer_config.json` | `5186f0defcd7f232382c7f0aebcd2252d073bb921ab240e407b7ae8745d2b29b` |
| `chat_template.jinja` | `e84f32a23fdda27689f868aa4a1a5621f41133e51a48d7f3efcbea2839574259` |
| `vocab.json` | `ce99b4cb2983d118806ce0a8b777a35b093e2000a503ebde25853284c9dfa003` |
| `merges.txt` | `a9d356d7bdf1ef4949e3e748e95b8e10ad9d4e2e838eddc38a0a7b6b94d1db8d` |

Container tokenizer audit used `CachedTokenizersBackend`, vocab size `248044`. Encoding the saved text produced `1032` derived token IDs, decode/re-encode was stable, and the decoded text remained byte/code-point equal to saved text with `29` U+FFFD. This proves only that the saved text is tokenizer-stable; it does **not** prove the original generated output token IDs or the layer where U+FFFD first appeared.

## Runtime/client instrumentation record

Parent runtime-only instrumentation:

- File: `runtime/instrument/sitecustomize.py`
- SHA-256: `5a4cd69f5af9ac1e24992c307a27038f3ada68597196e8c19c2d4e2575e852bd`
- Load path: Evidence-local `PYTHONPATH`.
- Behavior:
  - patched `vllm_fl.compilation.graph.GraphWrapper` and graph helper functions to emit graph capture/replay/task/workspace events;
  - patched `vllm.v1.core.sched.scheduler.Scheduler` to emit scheduler init and per-schedule request metadata;
  - patched `OpenAIServingCompletion.create_completion` to emit request metadata, header request ID and sampling settings.
- Why: parent Stage 6 runtime-path evidence for automatic capture, graph replay, chunked prefill/scheduler and request identity.
- Impact: runtime logging/monkey patching only; no production source file or wheel rebuild. It did not capture raw generated tokens, raw HTTP bytes, parsed response JSON, client in-memory text or saved/validator transformation boundaries.

This diagnostic added no runtime instrumentation and ran no service workload.

## Phase B disposition

Phase B was not run. The formal Task permits one `I1024/C64/O8` cell only conditionally after Phase A. Phase A already establishes that the preserved/targeted Evidence remains insufficient to classify A/B/C and that any non-reproduction under `temperature=1` would not negate the parent blocker. Running a new sampled cell would be a faithful configuration/workload reproduction, not exact sampled-token replay, and would not repair the parent chain’s missing raw layers.

## Classification

Final classification: **D — existing/targeted Evidence insufficient; unresolved**.

Competing hypotheses that remain open:

- tokenizer/decoder-native behavior;
- benchmark/harness/stream aggregation/reconstruction/save behavior;
- service/model/runtime anomaly;
- parent runtime instrumentation side effect;
- temperature-1 sampling and prior same-service C1/C8/C32 history interacting with the observed sampled output.

Confirmed facts:

- Parent first blocker is valid at the saved-result/validator layer.
- Request index `34` / `s6-i1024-c64-o8-34` saved output contains `29` U+FFFD.
- Parent result had 64 completed requests, 0 failed requests, 65536 input tokens and 512 output tokens for the failing cell.
- The parent Evidence does not contain the raw output-chain layers required to attribute A/B/C.

This Result does not deny the parent blocker and does not label it an FL/NPUGraph Unicode corruption bug.

## Safety and side effects

- Parent immutable Result and original Evidence: unchanged.
- Frozen source/tree and Accepted wheel: unchanged; no rebuild.
- Production source: unchanged.
- Phase B service: not launched.
- Stage 6 task port `8016`: not listening during this diagnostic.
- Accepted container: remained running; not killed, paused, reset or restarted.
- Unrelated host workloads on other ports/devices were observed but not touched.

## Evidence Manifest

- Diagnostic Evidence root: `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S6-UFFFD-OUTPUT-CHAIN-DIAGNOSTIC/evidence/20260827T101217p0800`
- Evidence manifest: `manifest.md`
- Layer audit: `layer_audit.md`
- Tokenizer audit: `tokenizer_audit.txt`
- Current status/safety: `current_status.md`
- Checksums: `checksums.txt`
- Parent bounded copies: `parent/`

## Last successful diagnostic step and first blocker

- Last successful diagnostic step: Phase A bounded parent Evidence audit completed.
- First blocker: required earlier raw output-chain layers are absent; earliest actual U+FFFD layer cannot be located.
- Codex1 decision request: review this diagnostic Result and decide whether any later formal Stage 6 recovery or separate instrumentation task is authorized. Codex2 does not resume Stage 6 or create a next Task.

## Three pointers

- Code/source pointer: `https://github.com/xiemingda-1002/vllm-plugin-FL.git`, frozen source `e610a990d785356bf51a3cad50219d4c03310a31`, tree `609ff1ad0f08239f353cb4d8774e504b4deba03b`, Accepted wheel SHA-256 `2fcf788660f3fe42b364bc60d593ee1b9b634fc0632de58c444d961bff4aa1bd`, Code PR=`N/A`.
- Control pointer: `docs/qwen36-35b-a3b-a3-flagos/tasks/QWEN36-A3-S6-UFFFD-OUTPUT-CHAIN-DIAGNOSTIC.md`, this immutable Result, `docs/qwen36-35b-a3b-a3-flagos/results/INDEX.md`, sync commit recorded after push.
- Evidence pointer: `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S6-UFFFD-OUTPUT-CHAIN-DIAGNOSTIC/evidence/20260827T101217p0800`, with parent source root `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX/evidence/20260826T180105p0800`.
