# RESULT-QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX

Dispatch: User formal dispatch to execute only Stage 6 A2-equivalent DP1/TP2 functional matrix, then stop before performance, prefix lifecycle, EP2 or later stages.

Run ID: `20260826T180105+0800`

## Status

- Lifecycle status: `failed`
- Experiment Result: `STOP`
- Control Sync: `SYNCED` by the Control commit that first adds this immutable Result
- Codex1 Acceptance: `PENDING`
- Gate F0: `PASS`
- Gate F1: `STOP`
- Gate F2: `RUN AFTER F1 STOP / NOT ACCEPTED`
- Gate F3: `PARTIAL EVIDENCE COLLECTED / NOT ACCEPTED`
- Stage 6 Execution PASS: `NO`
- Code PR: `N/A`

This Result records the completed Stage 6 execution evidence and the first attributable blocker. It does not modify implementation source, rebuild the accepted wheel, inspect later upstream movement, run performance, run prefix lifecycle, run EP2, create a Code repo/fork/PR, or enter any later stage.

## Identity

| Field | Value |
| --- | --- |
| Control repo | `yanceng305-collab/qwen36-35b-a3b-a3-flagos-control` |
| Control parent/main sync | `228891e57b4050a9d68d842216a858eaeec3e006` / tree `710615ea913b05573ec204ff2b5dd86ddd6946b4` |
| Task | `docs/qwen36-35b-a3b-a3-flagos/tasks/QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX.md` |
| Frozen source | `xiemingda-1002/vllm-plugin-FL@e610a990d785356bf51a3cad50219d4c03310a31` |
| Frozen tree | `609ff1ad0f08239f353cb4d8774e504b4deba03b` |
| Last pre-change tracked reference | `032fddc91b6d013b98aed8e64ff05b54d1435648` / tree `463806ef18e5e31006cd4f59e6a5261fc65cea4a` |
| Later upstream movement | `OUT OF SCOPE / IGNORE FOR EXECUTION` |
| Accepted wheel | `vllm_plugin_fl-0.2.0+ge610a990d-cp311-cp311-linux_aarch64.whl` |
| Accepted wheel SHA-256 | `2fcf788660f3fe42b364bc60d593ee1b9b634fc0632de58c444d961bff4aa1bd` |
| Accepted wheel path | `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S3-TP2-BF16-EAGER/artifacts/20260826T112011p0800/wheels/vllm_plugin_fl-0.2.0+ge610a990d-cp311-cp311-linux_aarch64.whl` |
| Selected image | `quay.io/ascend/vllm-ascend:v0.20.2rc1-a3-openeuler` |
| Manifest digest | `sha256:442363921166771eb82baeec9c1ac0381f46fb830ead8d0e072df6e925f2a958` |
| Local image ID | `sha256:8e7823878349b37d9900984555e28381c25681f88ff678cb4a86f7d1674a67c1` |
| OS/platform | `linux/arm64`, openEuler carrier image |
| Container | `qw36-a3-s2-gatec-priv-20260826T092617p0800` / `32562c7139600c25e570ec07841713737b0407c7d1bbdc563be41b87ea105f0a` |
| Server host | `bm-jn-zs-zone1-910C-64G-10-108` |
| Visible device scope | `ASCEND_VISIBLE_DEVICES=0,1`, `ASCEND_RT_VISIBLE_DEVICES=0,1` |
| Model | `/data/tiankuan/zyg/FL/workspace/Qwen3.6-35B-A3B`, BF16 non-quantized |
| Evidence root | `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX/evidence/20260826T180105p0800` |
| Main server log | `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX/evidence/20260826T180105p0800/runtime/server.log` |
| Functional summary | `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX/evidence/20260826T180105p0800/functional/cell_summary.csv` |
| O8 summary | `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX/evidence/20260826T180105p0800/functional/summary_o8.json` |
| O1024 summary | `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX/evidence/20260826T180105p0800/functional/summary_o1024.json` |
| Graph summary | `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX/evidence/20260826T180105p0800/runtime/graph_event_summary.json` |
| Checksums | `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX/evidence/20260826T180105p0800/checksums.txt` |

Runtime tuple:
`Python 3.11.15` / `CANN 9.0.0` / `torch 2.10.0+cpu` / `torch_npu 2.10.0` / `vLLM 0.20.2+empty` / `triton 3.5.0` / `triton-ascend 3.2.1` / `transformers 5.5.3` / `vllm-plugin-fl 0.2.0+ge610a990d`.

## Gate F0 PASS

F0 runtime continuity and service readiness passed before the functional cells:

- `torch_npu` imported successfully.
- `torch.npu.is_available() == True`.
- `torch.npu.device_count() == 2`; device names were `Ascend910_9382`, `Ascend910_9382`.
- accepted wheel SHA-256 matched `2fcf788660f3fe42b364bc60d593ee1b9b634fc0632de58c444d961bff4aa1bd`.
- `vllm-plugin-fl` was installed from site-packages; no editable/source-tree shortcut was recorded.
- `vllm-ascend` distribution was absent and `vllm_ascend` was not importable.
- `flag_gems` / `flag-gems` were absent and `USE_FLAGGEMS=0`.
- `vllm_fl.platform`, WorkerFL/ModelRunnerFL and GraphWrapper modules came from the standalone installed `vllm_fl`.
- model identity matched 26/26 shards, 1045 BF16 safetensors tensors and no quantization/download markers.
- `/health` and `/v1/models` returned HTTP 200.

Service command:
`/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX/evidence/20260826T180105p0800/commands/start_stage6_serve.sh`

Effective service contract included:
`DP1`, `TP2`, `distributed-executor-backend=mp`, BF16, `max_model_len=66560`, `max_num_seqs=64`, `max_num_batched_tokens=16384`, `gpu_memory_utilization=0.90`, prefix caching enabled with `mamba_cache_mode=align`, chunked prefill enabled, async scheduling enabled, `enforce_eager=False`, `cudagraph_mode=FULL_DECODE_ONLY`, no manual capture sizes, `VLLM_PLUGINS=fl`, `USE_FLAGGEMS=0`, `SOC_VERSION=ascend910_93`, `TASK_QUEUE_ENABLE=1`, `HCCL_OP_EXPANSION_MODE=AIV`, and `HCCL_BUFFSIZE=512`.

Runtime-only instrumentation was loaded from the Evidence-local path through `PYTHONPATH` and did not patch implementation source.

## Gate F1 STOP

All 16 O8 warm-up cells executed in the frozen order and every `vllm bench serve` process exited `0`. Prompt manifests recorded matching prompt lengths, unique prompts and no fixed shared prefix. However, the warm-up validator failed output-quality checks:

| Output | Cells with validator PASS | Total cells | First failing cell | First failure |
| --- | ---: | ---: | --- | --- |
| `O8` | 9 | 16 | `I1024 / C64 / O8` | request index `34` contained `29` Unicode replacement characters |

First blocker:
`Gate F1 output-quality validation failed: I1024 / C64 / O8 request index 34 contained 29 Unicode replacement characters.`

Evidence:

- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX/evidence/20260826T180105p0800/functional/o8_cell_exit_codes.csv`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX/evidence/20260826T180105p0800/functional/summary_o8.json`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX/evidence/20260826T180105p0800/functional/i1024_c64_o8/result.json`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX/evidence/20260826T180105p0800/functional/i1024_c64_o8/prompt_manifest.json`

Per contract, O1024 should not start after an O8 failure. Actual execution continued and the full O1024 matrix was run. That continuation is recorded below as raw evidence only and is not accepted as Gate F2 PASS.

## Gate F2 RUN AFTER F1 STOP / NOT ACCEPTED

All 16 O1024 cells executed once and every `vllm bench serve` process exited `0`. The validator nevertheless failed the main matrix:

| Output | Cells with validator PASS | Total cells | First failing cell | First observed failures |
| --- | ---: | ---: | --- | --- |
| `O1024` | 3 | 16 | `I1024 / C8 / O1024` | request index `3` had 1 Unicode replacement character; request index `5` had 4 Unicode replacement characters and `consecutive_duplicate_8gram_run=8` |

The O1024 run is useful for diagnosis because it shows the same output-quality class at larger output length and across multiple cells. It is not a Stage 6 PASS because Gate F1 already stopped and because only 3/16 O1024 cells passed the validator.

Evidence:

- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX/evidence/20260826T180105p0800/functional/o1024_cell_exit_codes.csv`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX/evidence/20260826T180105p0800/functional/summary_o1024.json`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX/evidence/20260826T180105p0800/functional/i1024_c8_o1024/result.json`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX/evidence/20260826T180105p0800/functional/i1024_c8_o1024/prompt_manifest.json`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX/evidence/20260826T180105p0800/functional/cell_summary.csv`

Timing fields emitted by `vllm bench serve` were preserved as incidental raw data only and were not evaluated for acceptance, throughput, latency, capacity or performance claims.

## Gate F3 PARTIAL EVIDENCE / NOT ACCEPTED

Server and worker trace evidence was collected during the same service instance:

- `GraphWrapper` instrumentation loaded from `/usr/local/python3.11.15/lib/python3.11/site-packages/vllm_fl/compilation/graph.py`.
- both TP workers recorded `S6_GRAPH_SET_ASCEND_PARAMS` with automatic capture sizes `[64,56,48,40,32,24,16,8,4,2,1]`, normalized to `[1,2,4,8,16,24,32,40,48,56,64]`.
- server effective config recorded `enable_chunked_prefill=True`, `async_scheduling=True`, `cudagraph_mode=FULL_DECODE_ONLY`, backend `eager`.
- `S6_GRAPH_EVENT` entries recorded `forward_mode=NONE` eager passthrough for prefill/non-uniform paths and `forward_mode=FULL` with `graph_type=NPUGraph` for graph capture/replay paths.
- observed FULL NPUGraph replay counts included batch/request sizes `1,2,4,8,16,24,32,40,48,56,64` on both TP worker pids `46333` and `46334`.
- chunked prefill scheduler evidence recorded `max_num_scheduled_tokens=16384` and request scheduling with `is_prefill_chunk=true`.
- negative substring scan of `server.log` found no `507011`, `invalid address`, `OOM`, `CPU fallback`, `vllm_ascend`, `flag_gems`, `FlagGems` or eager fallback.

This evidence is not sufficient for Stage 6 PASS because the functional output-quality gates failed first.

Evidence:

- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX/evidence/20260826T180105p0800/runtime/server.log`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX/evidence/20260826T180105p0800/runtime/graph_event_summary.json`

## Shutdown

After the User requested to stop and summarize, the Stage 6 service process was shut down with `SIGTERM`. It exited after approximately 30 seconds. Post-shutdown process and port scans for this task's service were empty.

Evidence:

- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX/evidence/20260826T180105p0800/runtime/shutdown_attempt.log`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX/evidence/20260826T180105p0800/runtime/shutdown_attempt.exit_code`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX/evidence/20260826T180105p0800/runtime/post_shutdown_process_scan.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX/evidence/20260826T180105p0800/runtime/post_shutdown_port_8016.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX/evidence/20260826T180105p0800/runtime/post_shutdown_npu_smi.txt`

The accepted container was preserved. This Result does not claim that all unrelated host NPU processes were absent; it only records that this task's service process and port were released.

## Last Successful Gate And First Blocker

- Last successful gate: `Gate F0 PASS`
- First blocker: `Gate F1 output-quality validation failed at I1024 / C64 / O8: request index 34 contained 29 Unicode replacement characters`
- Additional raw evidence: O1024 matrix later showed output-quality failures starting at `I1024 / C8 / O1024`, including Unicode replacement characters and an 8-gram duplicate run.
- Root-cause confidence: `HIGH` for the immediate observed output-quality blocker; `LOW / NOT CONFIRMED` for underlying cause. The evidence does not distinguish whether the source is graph/state behavior, tokenizer/output decoding, sampling behavior, workload stress, runtime instrumentation side effect, or another runtime/model issue.
- Minimum follow-up: Codex1 review of this STOP Result and raw outputs, then a bounded follow-up decision scoped to Stage 6 output corruption/repetition under the frozen runtime. No source change is authorized by this Result.

## Evidence Manifest

- Evidence manifest: `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX/evidence/20260826T180105p0800/manifest.md`
- Evidence checksums: `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX/evidence/20260826T180105p0800/checksums.txt`
- Main server log: `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX/evidence/20260826T180105p0800/runtime/server.log`
- API/result artifacts: `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX/evidence/20260826T180105p0800/functional`
- Graph summary: `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX/evidence/20260826T180105p0800/runtime/graph_event_summary.json`
- Command/config artifacts: `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX/evidence/20260826T180105p0800/commands`

## Three Pointers

- Code/source pointer: `https://github.com/xiemingda-1002/vllm-plugin-FL.git`, frozen source `e610a990d785356bf51a3cad50219d4c03310a31`, tree `609ff1ad0f08239f353cb4d8774e504b4deba03b`, accepted wheel SHA-256 `2fcf788660f3fe42b364bc60d593ee1b9b634fc0632de58c444d961bff4aa1bd`, Code PR=`N/A`.
- Control pointer: `docs/qwen36-35b-a3b-a3-flagos/tasks/QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX.md`, this immutable Result, `docs/qwen36-35b-a3b-a3-flagos/results/INDEX.md`, sync commit recorded after push.
- Evidence pointer: `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX/evidence/20260826T180105p0800`, manifest `manifest.md`, checksums `checksums.txt`.
