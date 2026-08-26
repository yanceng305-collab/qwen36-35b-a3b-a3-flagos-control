# RESULT-QWEN36-A3-S5-SERVE-CORRECTNESS

Dispatch：User formal dispatch to execute Stage 5 serve correctness gate only.

Run ID：`20260826T153354+0800`

## Identity

| Field | Value |
| --- | --- |
| Control repo | `yanceng305-collab/qwen36-35b-a3b-a3-flagos-control` |
| Control main sync | `371c37cb0b4c4fa1364cc2a29646818c6053b544` / tree `058f537bf09e6a474f4791b4194f75cd02dfaf86` |
| Code/source pointer | `xiemingda-1002/vllm-plugin-FL@032fddc91b6d013b98aed8e64ff05b54d1435648` / tree `463806ef18e5e31006cd4f59e6a5261fc65cea4a` |
| Official PR base | `flagos-ai/vllm-plugin-FL:release/0.2@ef78dec66fea1ae858ef414584be1478929ee9b2` / tree `7414bac41c39bc445b0cc05dbdaecc0f08231aeb` |
| Code PR | `N/A` |
| Selected image | `quay.io/ascend/vllm-ascend:v0.20.2rc1-a3-openeuler` |
| Manifest digest | `sha256:442363921166771eb82baeec9c1ac0381f46fb830ead8d0e072df6e925f2a958` |
| Arm64 platform digest | `sha256:a55b2b0597f9fdd1882de9bf3b7ebc395dd77c1ca49f251d0cb759d7b2c1a807` |
| Local image ID | `sha256:8e7823878349b37d9900984555e28381c25681f88ff678cb4a86f7d1674a67c1` |
| Accepted container | `qw36-a3-s2-gatec-priv-20260826T092617p0800` / `32562c7139600c25e570ec07841713737b0407c7d1bbdc563be41b87ea105f0a` |
| Server host | `bm-jn-zs-zone1-910C-64G-10-108` |
| Evidence root | `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S5-SERVE-CORRECTNESS/evidence/20260826T153354p0800` |
| Main server log | `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S5-SERVE-CORRECTNESS/evidence/20260826T153354p0800/runtime/server.log` |
| API artifacts | `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S5-SERVE-CORRECTNESS/evidence/20260826T153354p0800/api` |
| Graph summary | `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S5-SERVE-CORRECTNESS/evidence/20260826T153354p0800/runtime/graph_event_summary.json` |
| Checksums | `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S5-SERVE-CORRECTNESS/evidence/20260826T153354p0800/checksums.txt` |
| Cache root | `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S5-SERVE-CORRECTNESS/evidence/20260826T153354p0800/cache` |

Runtime tuple:
`Python 3.11.15` / `CANN 9.0.0` / `torch 2.10.0+cpu` / `torch_npu 2.10.0` / `vLLM 0.20.2` / `triton 3.2.0` / `triton-ascend 3.2.1` / `transformers 5.5.3` / `vllm-plugin-fl 0.2.0+ge610a990d`.

Visible device scope during service run:
`ASCEND_VISIBLE_DEVICES=0,1`, `ASCEND_RT_VISIBLE_DEVICES=0,1`, `SOC_VERSION=ascend910_93`.

## Gate S0 PASS

- `torch_npu` imported successfully.
- `torch.npu.is_available() == True`.
- `torch.npu.device_count() == 2`; device names were `Ascend910_9382` and `Ascend910_9382`.
- `vllm-plugin-fl` came from site-packages and the accepted wheel direct_url/hash, not from source checkout or editable mode.
- `vllm-ascend` distribution was absent; `vllm_ascend` was not importable.
- FlagGems was absent and `USE_FLAGGEMS=0`.
- `vllm_fl.platform` imported from site-packages; `current_platform` was `PlatformFL` with device `npu`.
- `vllm_fl.worker.model_runner` and `vllm_fl.compilation.graph` were loaded from site-packages.
- Model inventory was present and consistent: `config.json`, tokenizer files and `model.safetensors.index.json` existed; index referenced 26 shards / 1045 tensors and no shards were missing.

## Gate S1 PASS

Server launch used the bounded Stage 4 graph configuration:
`vllm serve /data/tiankuan/zyg/FL/workspace/Qwen3.6-35B-A3B --host 127.0.0.1 --port 8005 --served-model-name qwen3.6-35b-a3b-fl --data-parallel-size 1 --tensor-parallel-size 2 --dtype bfloat16 --max-model-len 2048 --max-num-seqs 8 --max-num-batched-tokens 2048 --disable-log-stats --no-enable-prefix-caching --no-enable-chunked-prefill --trust-remote-code --compilation-config '{"mode":3,"backend":"eager","cudagraph_mode":"FULL_DECODE_ONLY","cudagraph_capture_sizes":[1,2,4,8],"max_cudagraph_capture_size":8,"cudagraph_num_of_warmups":1,"compile_sizes":[]}'`

- readiness/health returned HTTP 200.
- `GET /v1/models` returned HTTP 200 with served model `qwen3.6-35b-a3b-fl`.
- `POST /v1/completions` returned HTTP 200 twice with identical text and finite logprobs.
- `POST /v1/chat/completions` returned HTTP 200 with readable nonempty output and finite logprobs.
- bounded concurrency=2 used two different prompts and returned HTTP 200 for both; outputs were nonempty and different (`Paris` vs `Tokyo`).
- repeated greedy completion text matched exactly across independent requests.

## Gate S2 PASS

- runtime-only instrumentation was injected through `sitecustomize.py` under the Evidence root; implementation source was not patched.
- both TP workers recorded graph capture sizes `[1,2,4,8]` through `GRAPH_SET_ASCEND_PARAMS` (logged in reverse internal order `[8,4,2,1]`).
- both TP workers were `VLLMWorker_TP` pids `33556` and `33557`.
- `GRAPH_EVENT_BEFORE` showed `forward_mode=NONE` with `phase=eager_passthrough` for prefill/non-uniform paths.
- `GRAPH_EVENT_BEFORE/AFTER` showed `forward_mode=FULL` with `graph_type=NPUGraph` for capture/replay paths.
- both TP workers had nonzero replay counts for batch size 1 and 2: `97/14` replay events per worker for `num_reqs=1/2`.
- task ordering matched the model structure: per worker and per capture size, 10 attention tasks and 30 conv1d tasks were recorded.
- `GRAPH_WORKSPACE` showed A3 NPU workspaces on `npu:0` and `npu:1`.
- negative scan found zero hits for `507011`, `invalid address`, `OOM`, `NaN`, `CPU fallback`, `vllm_ascend`, `flag_gems`, or eager fallback.

## Gate S3 PASS

- service shutdown was clean after `SIGTERM`; wrapper exited after 9 seconds.
- EngineCore logged `Shutdown initiated` and `Shutdown complete`.
- APIServer logged `Shutting down`, `Waiting for application shutdown`, and `Application shutdown complete`.
- after shutdown, port 8005 refused connections.
- final `npu-smi info` showed no running processes on NPU 0/1.
- accepted container and runtime artifacts were preserved; only the Stage 5 service process was stopped.

## API and graph evidence

- `completion_1` text: `\n\n<think>\nHere's a thinking process`
- `completion_2` text: same as `completion_1`
- `chat` text: `Here's a thinking process:\n\n1`
- `concurrency_rerun_1` text: `\n\n<think>\n\n</think>\n\nParis`
- `concurrency_rerun_2` text: `\n\n<think>\n\n</think>\n\nTokyo`
- all recorded logprob values were finite.
- `graph_event_summary.json` records the replay counts, capture sizes, graph ownership and negative scan results.

## Result

Execution status: **Execution PASS — Stage 5 Serve Correctness**

Per-gate exit codes:
`S0=0`, `S1=0`, `S2=0`, `S3=0`.

Last successful gate: `Gate S3 PASS`.
First blocker: `N/A`.
Root-cause confidence: `N/A` because no blocker was observed in this run.

Control Sync: **SYNCED via this Result commit**.
Codex1 Acceptance: `PENDING`.
Working tree: `clean` after commit.

Claim boundary:
this run proves bounded Stage 5 service correctness only. It does not claim performance, capacity, long-context, prefix, EP2, MTP, quantization, matrix, or Stage 6+ behavior.
