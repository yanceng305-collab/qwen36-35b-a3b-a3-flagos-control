# Codex1 Formal Acceptance — Qwen3.6 A3 Stage 5 Serve Correctness

Review date：2026-08-26

Acceptance：**ACCEPTED — bounded service correctness**

## Accepted execution identity

| Field | Accepted identity |
| --- | --- |
| Implementation repo / tracked branch | `https://github.com/xiemingda-1002/vllm-plugin-FL` / `feature/qwen3.6-35b-a3b-ascend-graph-migration` |
| Dispatch-time tracked HEAD/tree | `032fddc91b6d013b98aed8e64ff05b54d1435648` / `463806ef18e5e31006cd4f59e6a5261fc65cea4a` |
| Moving-head disposition | `e610a990... -> 032fddc9...` is docs/tests-only; no runtime rebuild required |
| Runtime artifact source/tree | `e610a990d785356bf51a3cad50219d4c03310a31` / `609ff1ad0f08239f353cb4d8774e504b4deba03b` |
| Installed wheel SHA-256 | `2fcf788660f3fe42b364bc60d593ee1b9b634fc0632de58c444d961bff4aa1bd` |
| Control dispatch parent | `371c37cb0b4c4fa1364cc2a29646818c6053b544` / tree `058f537bf09e6a474f4791b4194f75cd02dfaf86` |
| Immutable Result Control commit | `64840415679f67125e1660ae82f1fab53748ef8f` |
| Image | `quay.io/ascend/vllm-ascend:v0.20.2rc1-a3-openeuler` |
| Image manifest/platform digest | `sha256:442363921166771eb82baeec9c1ac0381f46fb830ead8d0e072df6e925f2a958` / `sha256:a55b2b0597f9fdd1882de9bf3b7ebc395dd77c1ca49f251d0cb759d7b2c1a807` |
| Local image ID | `sha256:8e7823878349b37d9900984555e28381c25681f88ff678cb4a86f7d1674a67c1` |
| Container | `qw36-a3-s2-gatec-priv-20260826T092617p0800` / `32562c7139600c25e570ec07841713737b0407c7d1bbdc563be41b87ea105f0a` |
| Runtime tuple | Python 3.11.15; CANN 9.0.0; torch `2.10.0+cpu`; torch_npu `2.10.0`; vLLM `0.20.2`; imported `triton.__version__=3.2.0`; triton-ascend distribution `3.2.1`; Transformers `5.5.3` |
| Device scope | 2 x `Ascend910_9382`; `ASCEND_VISIBLE_DEVICES=0,1`; `ASCEND_RT_VISIBLE_DEVICES=0,1`; `SOC_VERSION=ascend910_93` |
| Model | `/data/tiankuan/zyg/FL/workspace/Qwen3.6-35B-A3B`; 26/26 shards, 1045 tensors, BF16 non-quantized |
| Parallelism / graph | DP1 / TP2; `FULL_DECODE_ONLY`; capture `[1,2,4,8]` |
| Code PR | `N/A` |

The Result's `triton 3.2.0` field is the imported module version. Earlier Accepted diagnostics separately recorded community `triton` distribution `3.5.0`, imported `triton.__version__=3.2.0`, triton-ascend distribution `3.2.1`, and the Ascend provider. The Stage 5 notation therefore does not establish a runtime drift.

Formal Review live queries confirmed Control `main@64840415679f67125e1660ae82f1fab53748ef8f`, tracked HEAD/tree unchanged at `032fddc9...` / `463806ef...`, and official `release/0.2@ef78dec6...` / tree `7414bac4...`. PR #404 remains open/draft with the same head, but its current GitHub mergeability is `CONFLICTING / DIRTY`; this moving review state does not alter the exact accepted runtime artifact.

Codex1 did not operate the A3 server and did not rerun Stage 5. This review independently checked the immutable Result against the frozen Task, Stage 4 Acceptance, accepted reconstruction/runtime identity, live GitHub identities, and the preserved Evidence pointers.

## Reviewed Result and Evidence

- Immutable Result：[`RESULT-QWEN36-A3-S5-SERVE-CORRECTNESS-20260826T153354+0800.md`](../results/RESULT-QWEN36-A3-S5-SERVE-CORRECTNESS-20260826T153354+0800.md)
- Evidence root：`/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S5-SERVE-CORRECTNESS/evidence/20260826T153354p0800`
- Main server log：`runtime/server.log`
- API artifacts：`api/`
- Graph event summary：`runtime/graph_event_summary.json`
- Checksums：`checksums.txt` and `checksums.full.txt`
- Preserved runtime artifact/container/cache identities：recorded in the immutable Result and Evidence inventory

## Gate S0 — ACCEPTED

- `torch_npu` imported; `torch.npu.is_available()==True`; visible device count was 2; both devices were `Ascend910_9382`.
- Installed `vllm-plugin-fl 0.2.0+ge610a990d` came from site-packages and the Accepted wheel rather than source checkout or editable mode.
- `vllm-ascend` distribution was absent; `vllm_ascend` was not importable; FlagGems was absent; `USE_FLAGGEMS=0`.
- PlatformFL, WorkerFL, ModelRunnerFL and GraphWrapper came from standalone `vllm_fl`; accepted A3 `_C_ascend`/OPP and runtime ownership remained in force.
- Model identity remained 26/26 shards and 1045 tensors, BF16 non-quantized. No A3 runtime identity drift was reported.

## Gate S1 — ACCEPTED

The actual server command is preserved in the Result. Its effective contract was BF16 non-quantized, DP1/TP2, `max-model-len=2048`, `max-num-seqs=8`, `max-num-batched-tokens=2048`, prefix off, chunked prefill off, `enforce_eager=False`, compilation mode `VLLM_COMPILE`, backend `eager`, `FULL_DECODE_ONLY`, and capture sizes `[1,2,4,8]`.

- readiness/health and `GET /v1/models` returned HTTP 200.
- completion and chat completion returned HTTP 200 with nonempty readable output.
- two independent greedy completion calls were text-identical.
- bounded concurrency=2 used different prompts; both requests succeeded and returned distinct nonempty outputs (`Paris` and `Tokyo`).
- all recorded logprob values were finite.

Some short outputs contain `<think>` sections. Stage 5 is a bounded service-correctness contract, not final product-format acceptance. The preserved outputs contain no replacement-character corruption, non-finite values, pathological repetition, or observed cross-request state pollution, so this formatting is not a blocker for this gate.

## Gate S2 — ACCEPTED

- Runtime-only `sitecustomize.py` instrumentation was stored under Evidence; production source/artifact was not patched.
- Graph ownership was FL-local GraphWrapper with `graph_type=NPUGraph`, not inferred from HTTP status.
- Both TP workers captured `[1,2,4,8]`; per worker and per capture size, the recorded structure was 30 conv1d/GDN tasks plus 10 attention tasks.
- Prefill/non-uniform execution used `forward_mode=NONE` / eager passthrough; decode used `forward_mode=FULL` / real NPUGraph replay.
- Both workers recorded nonzero batch-size 1 and 2 replay counts: 97 and 14 per worker.
- A3 workspaces were present on `npu:0` and `npu:1`.
- The recorded negative scan found no `507011`, invalid address, OOM, NaN/Inf, CPU fallback, `vllm_ascend`, FlagGems activation, or silent eager fallback.

## Gate S3 — ACCEPTED

- After `SIGTERM`, the wrapper exited after 9 seconds; EngineCore recorded `Shutdown initiated` and `Shutdown complete`; APIServer recorded application shutdown completion.
- Port 8005 refused connections after shutdown.
- Final NPU state showed no Stage 5 process on NPU 0/1.
- Accepted container, wheel, cache and Evidence were preserved; only this Task's service was stopped and no unrelated NPU workload was reported affected.

## Acceptance boundary

Stage 5 Acceptance proves only bounded standalone-FL service correctness for this exact source/wheel/model/image/runtime identity and the Stage 4 graph subset `[1,2,4,8]`.

It proves health/models/completion/chat/repeat/bounded-concurrency API behavior, finite recorded logprobs, both-rank NPUGraph replay for batch sizes 1 and 2, no observed state contamination or forbidden fallback, and clean service shutdown.

It does **not** prove:

- automatic graph capture through 64;
- chunked prefill or async scheduling;
- `1K/4K/16K/64K x C1/C8/C32/C64, O1024` functional correctness;
- aligned prefix-cache lifecycle or EP2;
- capacity, startup/warm-up claims, latency, throughput, profiling, or FL/native performance parity.

## Final conclusion

Stage 5 Serve Correctness：**ACCEPTED — bounded service correctness**.

Implementation conclusion remains：**IMPLEMENTATION CHANGE: NONE SO FAR**.

The next Ready Task may enter A2-equivalent DP1/TP2 functional reproduction, but requires explicit User dispatch. This Acceptance does not authorize Codex2 to run it.
