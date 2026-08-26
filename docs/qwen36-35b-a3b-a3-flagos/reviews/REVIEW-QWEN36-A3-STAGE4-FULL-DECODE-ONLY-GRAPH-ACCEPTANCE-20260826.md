# Codex1 Formal Acceptance — Qwen3.6 A3 Stage 4 FULL_DECODE_ONLY Graph

Review date：2026-08-26

Acceptance：**ACCEPTED — bounded graph correctness**

## Accepted execution identity

| Field | Accepted identity |
| --- | --- |
| Implementation repo / tracked branch | `https://github.com/xiemingda-1002/vllm-plugin-FL` / `feature/qwen3.6-35b-a3b-ascend-graph-migration` |
| Dispatch-time tracked HEAD/tree | `032fddc91b6d013b98aed8e64ff05b54d1435648` / `463806ef18e5e31006cd4f59e6a5261fc65cea4a` |
| Moving-head disposition | `e610a990... → 032fddc9...` is docs/tests-only; no runtime rebuild required |
| Runtime artifact source/tree | `e610a990d785356bf51a3cad50219d4c03310a31` / `609ff1ad0f08239f353cb4d8774e504b4deba03b` |
| Installed wheel SHA-256 | `2fcf788660f3fe42b364bc60d593ee1b9b634fc0632de58c444d961bff4aa1bd` |
| Image | `quay.io/ascend/vllm-ascend:v0.20.2rc1-a3-openeuler` |
| Image manifest/platform digest | `sha256:442363921166771eb82baeec9c1ac0381f46fb830ead8d0e072df6e925f2a958` / `sha256:a55b2b0597f9fdd1882de9bf3b7ebc395dd77c1ca49f251d0cb759d7b2c1a807` |
| Container | `qw36-a3-s2-gatec-priv-20260826T092617p0800` / `32562c7139600c25e570ec07841713737b0407c7d1bbdc563be41b87ea105f0a` |
| Runtime tuple | Python 3.11.15; CANN 9.0.0; torch `2.10.0+cpu`; torch_npu `2.10.0`; vLLM `0.20.2+empty`; triton-ascend 3.2.1 / triton 3.5.0 |
| Device scope | 2 × `Ascend910_9382`; visible logical devices 0/1 |
| Model | `/data/tiankuan/zyg/FL/workspace/Qwen3.6-35B-A3B`; BF16 non-quantized |
| Parallelism / graph | DP1 / TP2 / HCCL; `FULL_DECODE_ONLY`; capture `[1,2,4,8]` |
| Code PR | `N/A` |

Formal Review live re-query found Control `main@0b3d796a0f387ca70e74df74bb33f9e80dadfc93`, tracked HEAD/tree unchanged at `032fddc9...` / `463806ef...`, and PR #404 still open/draft with that head. The official `release/0.2` branch has moved to `ef78dec66fea1ae858ef414584be1478929ee9b2` / tree `7414bac41c39bc445b0cc05dbdaecc0f08231aeb`; this moving base fact does not replace the already Accepted Stage 3 wheel identity.

Codex1 did not operate the A3 server. This review independently checked the immutable Result against the frozen Task, Stage 3 Acceptance, moving-head Review, runtime reconstruction/handoff and the recorded Evidence pointers. Raw Evidence remains at the server paths below.

## Reviewed result and evidence

- Immutable Result：[`RESULT-QWEN36-A3-S4-FULL-DECODE-ONLY-GRAPH-20260826T141740+0800.md`](../results/RESULT-QWEN36-A3-S4-FULL-DECODE-ONLY-GRAPH-20260826T141740+0800.md)
- Evidence root：`/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S4-FULL-DECODE-ONLY-GRAPH/evidence/20260826T141740p0800`
- Main graph log：`runtime/gate_g1_g2_full_decode_only_graph.log`
- Event summary：`runtime/gate_g1_g2_graph_event_summary.json`
- Key graph trace：`runtime/gate_g1_g2_key_graph_trace.txt`
- Output/exit trace：`runtime/gate_g2_outputs_and_exit_trace.txt`
- Evidence checksum manifest：`checksums.txt`

## Gate G0 — ACCEPTED

- Preserved Accepted container/runtime continuity, A3 image identity and two-device visible scope are recorded; `torch_npu` imported, `torch.npu.is_available()==True`, device count was 2, and both devices were `Ascend910_9382`.
- Installed `vllm-plugin-fl 0.2.0+ge610a990d` came from site-packages and the Accepted wheel hash. `032fddc9...` is correctly recorded only as dispatch-time tracked source, not as wheel build identity.
- `vllm-ascend` distribution was absent, `vllm_ascend` was not importable, FlagGems was absent, and `USE_FLAGGEMS=0`.
- PlatformFL, WorkerFL, ModelRunnerFL and GraphWrapper came from standalone `vllm_fl`; `_C_ascend` and OPP came from the wheel's `prebuilt/ascend910_93` tree.
- Model identity remained 26/26 shards, 1045/1045 BF16 tensors, 40 layers with 30 linear-attention and 10 full-attention layers, 256 experts/top-8, no quantization.
- TP2/HCCL initialized with world size 2; no runtime scope drift is reported.

### Pre-model probe failures — accepted as harness corrections

Two runtime probes used incorrect guessed module paths (`vllm_fl.v1...` and `vllm_fl._C_ascend`), and one model probe incorrectly expected nested `text_config` fields at the top level. All occurred before model/graph execution. Corrected probes used the installed wheel's actual loader/config structure and exited 0; the later formal graph process completed capture, replay, synchronization and shutdown.

These failures do not show runtime, model or device-state corruption and do not justify a mechanical rerun of the successful bounded graph execution.

## Gate G1 — ACCEPTED

- Effective configuration was DP1/TP2 BF16, `enforce_eager=False`, compilation mode `VLLM_COMPILE`, backend `eager`, `cudagraph_mode=FULL_DECODE_ONLY`, capture sizes `[1,2,4,8]`, prefix/chunked prefill/MTP/quantization off, and `npugraph_ex` disabled.
- The selected path was FL-local GraphWrapper + eager FX + Ascend NPUGraph.
- Both TP workers captured sizes 1, 2, 4 and 8. Entries recorded `graph_type=NPUGraph`, `forward_mode=FULL`, `phase=capture`.
- For every size and rank, task-order evidence recorded 30 conv1d/GDN tasks and 10 attention tasks, consistent with the model layer structure.
- Persistent Ascend graph workspace/state evidence was recorded on both devices for every capture size.
- The negative scan reported no silent eager fallback, `507011`, invalid address, OOM, NaN/Inf, CPU fallback, `vllm_ascend` activation or FlagGems activation.

## Gate G2 — ACCEPTED

- Prefill/non-uniform paths recorded `forward_mode=NONE` / `phase=eager_passthrough`; decode recorded `forward_mode=FULL` / `phase=replay` on existing NPUGraph entries.
- Both TP workers replayed batch size 1 and 2: per worker, size-1 replay count 15 and size-2 replay count 6.
- Every recorded generation produced 8 decode tokens, proving consecutive multi-token replay.
- Independent greedy runs of `Hello, my name is` were token/text identical: ` John. I am a 30`, token IDs `[3629, 13, 353, 1044, 264, 220, 18, 15]`.
- The batch-2 prompts produced nonempty distinct outputs: France → ` Paris, a city renowned for its iconic`; tea → ` the following steps:\n\n1. Bo`.
- All recorded logprobs were finite. Repeated replay across independent requests and a batch-size transition showed no observed GDN/Mamba recurrent-state, attention KV/slot-mapping or request-metadata contamination.
- Final `torch.npu.synchronize()` succeeded, engine shutdown completed, process exited 0 and NPU 0/1 were released.

## Acceptance boundary

Stage 4 Acceptance proves only bounded A3 graph correctness for this exact source/wheel/model/runtime identity and capture sizes `[1,2,4,8]`.

It does **not** prove:

- the A2 final service automatic capture matrix through `max_num_seqs=64`;
- serve/API correctness, chunked prefill, async scheduling or prefix caching;
- EP2, long context, the 16-cell `1K/4K/16K/64K × C1/C8/C32/C64, O1024` functional matrix;
- capacity, latency, throughput, profiling or FL/native performance parity.

## Final conclusion

Stage 4 `FULL_DECODE_ONLY` `[1,2,4,8]`：**ACCEPTED — bounded graph correctness**.

Implementation conclusion remains：**IMPLEMENTATION CHANGE: NONE SO FAR**.

Stage 5 serve correctness is unlocked as a Ready Task, but requires explicit User dispatch. No A3 execution is authorized by this Acceptance alone.
