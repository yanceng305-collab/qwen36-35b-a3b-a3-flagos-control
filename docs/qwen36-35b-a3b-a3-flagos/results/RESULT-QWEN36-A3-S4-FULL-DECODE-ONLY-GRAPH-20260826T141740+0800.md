# QWEN36-A3-S4-FULL-DECODE-ONLY-GRAPH Result

Task: `QWEN36-A3-S4-FULL-DECODE-ONLY-GRAPH`
Run timestamp: `2026-08-26T14:17:40+08:00`
Control sync timestamp: `2026-08-26T14:50:00+08:00`
Executor: `root` on `bm-jn-zs-zone1-910C-64G-10-108`
Dispatch: User formal dispatch to execute Stage 4 `FULL_DECODE_ONLY` graph validation.

## Status

- Lifecycle status: `completed`
- Experiment Result: `PASS`
- Execution PASS: `Execution PASS - Stage 4 FULL_DECODE_ONLY Graph`
- Control Sync: `SYNCED` by the Control commit that first adds this immutable Result
- Codex1 Acceptance: `PENDING`
- Gate G0 runtime continuity: `PASS`
- Gate G1 FULL_DECODE_ONLY capture: `PASS`
- Gate G2 replay/state correctness: `PASS`
- Stage 5: `NOT ENTERED / LOCKED`
- Code PR: `N/A`

This run did not rebuild the wheel, did not modify implementation source, did not modify model files, did not install FlagGems, did not import or depend on `vllm_ascend`, did not run serve/API, prefix caching, EP2, long context, 64K, functional matrix, benchmark, profiling, performance, MTP, quantization, `npugraph_ex`, Code fork/PR, GLM, or Stage 5.

## Control And Source Identity

| Field | Value |
| --- | --- |
| Control repo | `https://github.com/yanceng305-collab/qwen36-35b-a3b-a3-flagos-control` |
| Control parent at dispatch | `c0318036046d0109d06885461337c32b82c8ea09` |
| Implementation repo | `https://github.com/xiemingda-1002/vllm-plugin-FL` |
| Tracked branch | `feature/qwen3.6-35b-a3b-ascend-graph-migration` |
| Dispatch-time tracked HEAD/tree | `032fddc91b6d013b98aed8e64ff05b54d1435648` / `463806ef18e5e31006cd4f59e6a5261fc65cea4a` |
| Bounded moving-head disposition | `032fddc9...` is one commit ahead of Stage 3 Accepted `e610a990...`; diff is `README.md` modified and `tests/unit_tests/test_build_config.py` added; docs/tests-only; reuse accepted wheel/runtime |
| Runtime artifact source | Stage 3 Accepted `e610a990d785356bf51a3cad50219d4c03310a31` / tree `609ff1ad0f08239f353cb4d8774e504b4deba03b` |
| Accepted wheel | `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S3-TP2-BF16-EAGER/artifacts/20260826T112011p0800/wheels/vllm_plugin_fl-0.2.0+ge610a990d-cp311-cp311-linux_aarch64.whl` |
| Accepted wheel SHA-256 | `2fcf788660f3fe42b364bc60d593ee1b9b634fc0632de58c444d961bff4aa1bd` |
| Code PR | `N/A` |

Dispatch re-query found the tracked branch still matched the reviewed Stage 4 identity. Official base was re-queried and had moved to `flagos-ai/vllm-plugin-FL:release/0.2@ef78dec66fea1ae858ef414584be1478929ee9b2` / tree `7414bac41c39bc445b0cc05dbdaecc0f08231aeb`; this moving base fact was recorded and was not used to replace the already accepted Stage 3 runtime artifact identity.

Evidence:

- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S4-FULL-DECODE-ONLY-GRAPH/evidence/20260826T141740p0800/source/implementation_commit_032fddc9.json`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S4-FULL-DECODE-ONLY-GRAPH/evidence/20260826T141740p0800/source/compare_e610a990_to_032fddc9_summary.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S4-FULL-DECODE-ONLY-GRAPH/evidence/20260826T141740p0800/source/official_base_commit_dispatch.json`

## Runtime, Container And Device

| Field | Value |
| --- | --- |
| Reused preserved container | `qw36-a3-s2-gatec-priv-20260826T092617p0800` |
| Container ID | `32562c7139600c25e570ec07841713737b0407c7d1bbdc563be41b87ea105f0a` |
| Container state | preserved and running |
| Image | `quay.io/ascend/vllm-ascend:v0.20.2rc1-a3-openeuler` |
| Manifest/platform digest | `sha256:442363921166771eb82baeec9c1ac0381f46fb830ead8d0e072df6e925f2a958` / `sha256:a55b2b0597f9fdd1882de9bf3b7ebc395dd77c1ca49f251d0cb759d7b2c1a807` |
| Local image ID | `sha256:8e7823878349b37d9900984555e28381c25681f88ff678cb4a86f7d1674a67c1` |
| Runtime visible scope | `ASCEND_RT_VISIBLE_DEVICES=0,1`, `ASCEND_VISIBLE_DEVICES=0,1` |
| Device invariant | `torch_npu` import succeeded; `torch.npu.is_available()==True`; `torch.npu.device_count()==2`; device names `Ascend910_9382` |
| Safe scope | NPU 0/1 had no running processes at preflight; unrelated NPU 2-7 workloads were not touched |
| Final device state | NPU 0/1 released; unrelated NPU 2-7 workloads unchanged |

Gate G0 corrected runtime probe exit code: `0`. Two earlier G0 evidence probes exited nonzero due to harness-only incorrect module path assumptions (`vllm_fl.v1...` and `vllm_fl._C_ascend` import path); they happened before model/graph execution. The corrected probe used the installed wheel's actual `vllm_fl.ascend_custom_ops.enable_custom_op()` loader and passed.

Runtime evidence:

- `vllm-plugin-fl 0.2.0+ge610a990d` from site-packages and direct URL pointing to the accepted wheel hash.
- `vllm-ascend` distribution absent.
- `vllm_ascend` import failed with `ModuleNotFoundError`.
- `flag_gems` distribution absent and import failed with `ModuleNotFoundError`.
- `VLLM_PLUGINS=fl`, `USE_FLAGGEMS=0`.
- `PlatformFL` device `npu`, dispatch key `PrivateUse1`.
- `WorkerFL`, `ModelRunnerFL`, `GraphWrapper` loaded from site-packages `vllm_fl`.
- `_C_ascend` extension file and OPP roots came from `site-packages/vllm_fl/dispatch/backends/vendor/ascend/prebuilt/ascend910_93`.
- `ASCEND_CUSTOM_OPP_PATH` after custom-op loader pointed to the accepted wheel's `ascend910_93/opp/vendors/custom_transformer`.

Evidence:

- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S4-FULL-DECODE-ONLY-GRAPH/evidence/20260826T141740p0800/runtime/gate_g0_runtime_probe_corrected.log`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S4-FULL-DECODE-ONLY-GRAPH/evidence/20260826T141740p0800/environment/npu_smi_preflight.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S4-FULL-DECODE-ONLY-GRAPH/evidence/20260826T141740p0800/environment/npu_smi_final_rerun.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S4-FULL-DECODE-ONLY-GRAPH/evidence/20260826T141740p0800/environment/container_final_status.txt`

## Model Identity

Model path: `/data/tiankuan/zyg/FL/workspace/Qwen3.6-35B-A3B`.

Gate G0 model identity corrected probe exit code: `0`.

Observed model identity:

- architecture: `Qwen3_5MoeForConditionalGeneration`
- model type: `qwen3_5_moe`; text model type `qwen3_5_moe_text`
- text dtype: `bfloat16`
- no `quantization_config`
- 40 layers
- layer type counts: 30 `linear_attention`, 10 `full_attention`
- 256 experts, top-8 experts per token
- index references 26 shards and 1045 weight entries
- all 26 referenced shards present in the model root
- safetensors header audit found 1045/1045 tensors as `torch.bfloat16`
- tokenizer files present: `merges.txt`, `tokenizer.json`, `tokenizer_config.json`, `vocab.json`
- no `.lock`, `.tmp`, `.incomplete`, `.aria2`, or `.part` download markers

An earlier model probe exited nonzero because it asserted top-level config fields that are actually nested under `text_config`; it had already observed the same 26/26 shards and BF16 tensor count. The corrected probe used the actual config structure and passed.

Evidence:

- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S4-FULL-DECODE-ONLY-GRAPH/evidence/20260826T141740p0800/model/gate_g0_model_identity_corrected.log`

## Gate G1 PASS — FULL_DECODE_ONLY Capture

Main graph script exit code: `0`.

Effective config:

- model `/data/tiankuan/zyg/FL/workspace/Qwen3.6-35B-A3B`
- `VLLM_PLUGINS=fl`
- `USE_FLAGGEMS=0`
- `ASCEND_RT_VISIBLE_DEVICES=0,1`
- `SOC_VERSION=ascend910_93`
- `HCCL_OP_EXPANSION_MODE=AIV`
- DP1 / TP2 / BF16
- `enforce_eager=False`
- `quantization=None`, `quantization_config=None`
- `enable_prefix_caching=False`
- `enable_chunked_prefill=False`
- `compilation_config.mode=VLLM_COMPILE`
- `compilation_config.backend=eager`
- `compilation_config.cudagraph_mode=FULL_DECODE_ONLY`
- `cudagraph_capture_sizes=[1,2,4,8]`
- `max_cudagraph_capture_size=8`
- `npugraph_ex` opt-in not enabled
- `max_model_len=2048`, `max_num_seqs=8`, `max_num_batched_tokens=2048`

Execution evidence:

- Engine config recorded `enforce_eager=False`, `device_config=npu`, `tensor_parallel_size=2`, `pipeline_parallel_size=1`, `data_parallel_size=1`, `dtype=torch.bfloat16`, and `cudagraph_mode=FULL_DECODE_ONLY`.
- Platform log selected the Ascend FULL_DECODE_ONLY eager-FX + NPUGraph route; effective config showed backend `eager`, `use_inductor_graph_partition=False`, and `fuse_norm_quant/fuse_act_quant/fuse_attn_quant=False`.
- TP2/HCCL initialized with `world_size=2`, rank 0/1, backend `hccl`.
- 26/26 safetensors checkpoint shards loaded.
- Runtime-only instrumentation recorded `GraphWrapper` from `vllm_fl.compilation.graph` and graph class `torch_npu.npu.graphs.NPUGraph`.
- Both TP worker processes captured all requested decode sizes:
  - worker pid `23965`: `[1,2,4,8]`
  - worker pid `23966`: `[1,2,4,8]`
- Captured graph entries used `graph_type=NPUGraph`, `forward_mode=FULL`, `phase=capture`.
- Per-size task-order instrumentation recorded 10 `attention` tasks and 30 `conv1d`/GDN tasks for each captured size on each TP worker, matching the model's 10 full-attention and 30 linear-attention/GDN layer structure.
- Persistent Ascend graph workspace evidence was recorded for sizes 1, 2, 4, and 8 on both `npu:0` and `npu:1`.
- Negative keyword scan found no `507011`, invalid address, OOM, NaN/Inf, CPU fallback, `vllm_ascend` activation, or FlagGems activation. It contains only the expected negative import evidence for absent `vllm_ascend`/`flag_gems` and accepted carrier warnings about absent upstream `vllm._C`.

Evidence:

- Main graph log: `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S4-FULL-DECODE-ONLY-GRAPH/evidence/20260826T141740p0800/runtime/gate_g1_g2_full_decode_only_graph.log`
- Event summary: `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S4-FULL-DECODE-ONLY-GRAPH/evidence/20260826T141740p0800/runtime/gate_g1_g2_graph_event_summary.json`
- Key graph trace: `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S4-FULL-DECODE-ONLY-GRAPH/evidence/20260826T141740p0800/runtime/gate_g1_g2_key_graph_trace.txt`

## Gate G2 PASS — Replay And State Correctness

Replay/state evidence:

- Prefill/non-uniform warmup and prompt paths recorded `forward_mode=NONE`, `phase=eager_passthrough`, with no graph entry created, demonstrating prefill stayed non-graph.
- Decode used captured FULL graph replay with `forward_mode=FULL`, `phase=replay`, `graph_created_or_present=true`, `graph_type=NPUGraph`.
- Replay counts covered batch size 1 and 2 on both TP workers:
  - `23965:1` replay count 15; `23965:2` replay count 6
  - `23966:1` replay count 15; `23966:2` replay count 6
- Same greedy prompt repeated twice independently:
  - prompt `Hello, my name is`
  - output text both runs: ` John. I am a 30`
  - token IDs both runs: `[3629, 13, 353, 1044, 264, 220, 18, 15]`
- Batch size 2 different prompts produced nonempty different outputs:
  - `The capital of France is` -> ` Paris, a city renowned for its iconic`
  - `A short recipe for tea starts with` -> ` the following steps:\n\n1. Bo`
- Each generation produced 8 output tokens, demonstrating multi-token decode.
- Logprobs were checked and finite for all recorded outputs (`40` logprob entries per output record).
- GDN/Mamba recurrent and attention state freshness is supported by per-size task order of 30 conv1d/GDN + 10 attention tasks during capture, repeated replay across independent requests and batch-size changes, identical repeat output for the same greedy prompt, different nonempty outputs for different prompts, and final `torch.npu.synchronize()`.
- Process exited `0`; engine shutdown completed.

Evidence:

- Output/exit trace: `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S4-FULL-DECODE-ONLY-GRAPH/evidence/20260826T141740p0800/runtime/gate_g2_outputs_and_exit_trace.txt`
- Parsed graph event summary: `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S4-FULL-DECODE-ONLY-GRAPH/evidence/20260826T141740p0800/runtime/gate_g1_g2_graph_event_summary.json`

## Last Successful Gate And First Blocker

- Last successful gate: `Gate G2 PASS`
- Last successful step: batch size 2 replay generation completed, logprobs finite, outputs nonempty/different, final `torch.npu.synchronize()` succeeded, process exited 0, NPU 0/1 released.
- First blocker: `N/A`
- Root-cause confidence: `N/A` because no final blocker remained.
- Next Decision request: Codex1 formal Acceptance review for Stage 4 FULL_DECODE_ONLY graph. Do not enter Stage 5 without User dispatch.

## Evidence Manifest

- Evidence root: `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S4-FULL-DECODE-ONLY-GRAPH/evidence/20260826T141740p0800`
- Evidence checksums: `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S4-FULL-DECODE-ONLY-GRAPH/evidence/20260826T141740p0800/checksums.txt`
- Evidence file inventory: `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S4-FULL-DECODE-ONLY-GRAPH/evidence/20260826T141740p0800/artifacts/evidence_file_inventory.txt`
- Main graph log: `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S4-FULL-DECODE-ONLY-GRAPH/evidence/20260826T141740p0800/runtime/gate_g1_g2_full_decode_only_graph.log`

## Preserved Runtime And Artifacts

- Container: `qw36-a3-s2-gatec-priv-20260826T092617p0800`
- Container ID: `32562c7139600c25e570ec07841713737b0407c7d1bbdc563be41b87ea105f0a`
- Container state at Result drafting: running
- Installed runtime: accepted standalone `vllm-plugin-fl 0.2.0+ge610a990d`; `vllm-ascend` absent; `USE_FLAGGEMS=0`
- Accepted wheel: `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S3-TP2-BF16-EAGER/artifacts/20260826T112011p0800/wheels/vllm_plugin_fl-0.2.0+ge610a990d-cp311-cp311-linux_aarch64.whl`
- Wheel SHA-256: `2fcf788660f3fe42b364bc60d593ee1b9b634fc0632de58c444d961bff4aa1bd`
- Graph/Triton cache root: `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S4-FULL-DECODE-ONLY-GRAPH/cache`
- Evidence/artifacts remain under `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S4-FULL-DECODE-ONLY-GRAPH`

## Three Pointers

- Code/source pointer: current tracked `https://github.com/xiemingda-1002/vllm-plugin-FL`, branch `feature/qwen3.6-35b-a3b-ascend-graph-migration`, dispatch HEAD `032fddc91b6d013b98aed8e64ff05b54d1435648`, tree `463806ef18e5e31006cd4f59e6a5261fc65cea4a`, bounded docs/tests-only disposition; runtime artifact source `e610a990d785356bf51a3cad50219d4c03310a31`, tree `609ff1ad0f08239f353cb4d8774e504b4deba03b`, accepted wheel SHA-256 `2fcf788660f3fe42b364bc60d593ee1b9b634fc0632de58c444d961bff4aa1bd`; Code PR=`N/A`.
- Control pointer: `docs/qwen36-35b-a3b-a3-flagos/tasks/QWEN36-A3-S4-FULL-DECODE-ONLY-GRAPH.md`, this immutable Result, `docs/qwen36-35b-a3b-a3-flagos/results/INDEX.md`, sync commit recorded in the index after push.
- Evidence pointer: `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S4-FULL-DECODE-ONLY-GRAPH/evidence/20260826T141740p0800`, checksums `checksums.txt`, main graph log `runtime/gate_g1_g2_full_decode_only_graph.log`.
