# QWEN36-A3-S3-TP2-BF16-EAGER Resume Result

Task: `QWEN36-A3-S3-TP2-BF16-EAGER`
Run timestamp: `2026-08-26T11:52:34+08:00`
Control sync timestamp: `2026-08-26T12:12:00+08:00`
Executor: `root` on `bm-jn-zs-zone1-910C-64G-10-108`
Dispatch: User requested continuing the bounded Stage 3 task after completing the model download.

## Status

- Lifecycle status: `completed`
- Experiment Result: `PASS`
- Execution PASS: `Execution PASS - Stage 3 TP2 BF16 Eager`
- Control Sync: `SYNCED` by the Control commit that first adds this immutable Result
- Codex1 Acceptance: `PENDING`
- Gate R current-head regression: `PASS` by reused prior same-task current-head evidence
- Gate M model identity: `PASS`
- Gate E TP2 BF16 eager: `PASS`
- Stage 4: `NOT ENTERED / LOCKED`
- Code PR: `N/A`

This resume did not modify implementation source, did not modify model files, did not rebuild the wheel, did not create a new container, did not install FlagGems, and did not run graph, serve, prefix caching, MTP, quantization, benchmark, profiling, GLM, EP or 64K.

## Control And Source Identity

| Field | Value |
| --- | --- |
| Control repo | `https://github.com/yanceng305-collab/qwen36-35b-a3b-a3-flagos-control` |
| Control parent at resume | `076d83b622615be948da9202a36520a52270f94d` |
| Implementation repo | `https://github.com/xiemingda-1002/vllm-plugin-FL` |
| Tracked branch | `feature/qwen3.6-35b-a3b-ascend-graph-migration` |
| Resume dispatch-time tracked HEAD | `e610a990d785356bf51a3cad50219d4c03310a31` |
| Resume source tree | `609ff1ad0f08239f353cb4d8774e504b4deba03b` |
| Source path | `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S3-TP2-BF16-EAGER/work/src-e610a990-20260826T112011p0800` |
| Source state | detached checkout, clean; no implementation source patch |
| Official PR base | `flagos-ai/vllm-plugin-FL:release/0.2@53adefb269571684d83a51e997d3ba9be5f88235` |

Evidence:

- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S3-TP2-BF16-EAGER/evidence/20260826T115234p0800/source/source_identity_resume.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S3-TP2-BF16-EAGER/evidence/20260826T115234p0800/source/source_status_final.txt`

## Runtime, Container And Device

| Field | Value |
| --- | --- |
| Reused preserved container | `qw36-a3-s2-gatec-priv-20260826T092617p0800` |
| Container ID | `32562c7139600c25e570ec07841713737b0407c7d1bbdc563be41b87ea105f0a` |
| Image | `quay.io/ascend/vllm-ascend:v0.20.2rc1-a3-openeuler` |
| Manifest/platform digest | `sha256:442363921166771eb82baeec9c1ac0381f46fb830ead8d0e072df6e925f2a958` / `sha256:a55b2b0597f9fdd1882de9bf3b7ebc395dd77c1ca49f251d0cb759d7b2c1a807` |
| Local image ID | `sha256:8e7823878349b37d9900984555e28381c25681f88ff678cb4a86f7d1674a67c1` |
| Runtime scope | `ASCEND_RT_VISIBLE_DEVICES=0,1`, `ASCEND_VISIBLE_DEVICES=0,1` |
| Device invariant | `torch_npu` import succeeded; `torch.npu.is_available()==True`; `torch.npu.device_count()==2`; device names `Ascend910_9382` |
| Final device state | NPU 0/1 released; unrelated NPU 2-7 workloads unchanged |
| Final container state | preserved and running |

A first invariant script without `ASCEND_RT_VISIBLE_DEVICES` observed all 16 logical devices in the privileged container and exited nonzero. The accepted runtime path was then used explicitly with `ASCEND_RT_VISIBLE_DEVICES=0,1`; the rerun passed and all Gate E commands used that exact runtime-visible scope.

Evidence:

- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S3-TP2-BF16-EAGER/evidence/20260826T115234p0800/runtime/resume_runtime_invariant_rerun.log`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S3-TP2-BF16-EAGER/evidence/20260826T115234p0800/environment/npu_smi_pre_resume.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S3-TP2-BF16-EAGER/evidence/20260826T115234p0800/environment/npu_smi_final.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S3-TP2-BF16-EAGER/evidence/20260826T115234p0800/environment/container_final_status.txt`

## Gate R PASS

Gate R was not rebuilt in this resume because the same task already produced and installed the current-head wheel at the unchanged source identity. The resume reused:

- Prior Gate R evidence root: `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S3-TP2-BF16-EAGER/evidence/20260826T112011p0800`
- Wheel: `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S3-TP2-BF16-EAGER/artifacts/20260826T112011p0800/wheels/vllm_plugin_fl-0.2.0+ge610a990d-cp311-cp311-linux_aarch64.whl`
- Wheel SHA-256: `2fcf788660f3fe42b364bc60d593ee1b9b634fc0632de58c444d961bff4aa1bd`
- Python ABI: `cp311-cp311-linux_aarch64`
- Family: `ascend910_93`
- Main build log: `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S3-TP2-BF16-EAGER/evidence/20260826T112011p0800/logs/gate_r_build_wheel.log`

Resume runtime invariant confirmed the installed package remained the current-head wheel from that path, `vllm-ascend` remained absent, `vllm_ascend` remained non-importable, `flag_gems` remained absent, `VLLM_PLUGINS=fl`, and `USE_FLAGGEMS=0`.

## Gate M PASS

Model path: `/data/tiankuan/zyg/FL/workspace/Qwen3.6-35B-A3B`.

Observed model identity:

- model root exists; observed size about `67G`
- required config/tokenizer/index files present: `config.json`, `tokenizer.json`, `tokenizer_config.json`, `generation_config.json`, `model.safetensors.index.json`, `merges.txt`, `vocab.json`
- architecture: `Qwen3_5MoeForConditionalGeneration`
- text config dtype: `bfloat16`; no `quantization_config`
- key structure: 40 layers, 30 `linear_attention` and 10 `full_attention`, 256 experts, top-8 experts per token, hidden size 2048, 16 attention heads, 2 KV heads, max position embeddings 262144
- index references 26 shards and 1045 weight entries; all 26 referenced shards are present in the model root
- no extra root-level `.safetensors` alternatives
- `._____temp/` exists but is empty; no `.lock`, `.tmp`, `.incomplete`, `.aria2` or `.part` download markers found
- safetensors header-only audit in the container read all 26 shards and found `BF16` for all 1045 tensors
- model root file checksum manifest saved

Evidence:

- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S3-TP2-BF16-EAGER/evidence/20260826T115234p0800/model/model_file_inventory_resume.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S3-TP2-BF16-EAGER/evidence/20260826T115234p0800/model/model_identity_resume_rerun.json`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S3-TP2-BF16-EAGER/evidence/20260826T115234p0800/model/model_safetensors_header_audit.json`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S3-TP2-BF16-EAGER/evidence/20260826T115234p0800/model/model_root_file_checksums_resume.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S3-TP2-BF16-EAGER/evidence/20260826T115234p0800/model/gate_m_overall_summary.txt`

## Gate E PASS

Effective Gate E command/config:

- command file: `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S3-TP2-BF16-EAGER/evidence/20260826T115234p0800/commands/gate_e_tp2_bf16_eager_file_command.txt`
- model: `/data/tiankuan/zyg/FL/workspace/Qwen3.6-35B-A3B`
- `VLLM_PLUGINS=fl`
- `USE_FLAGGEMS=0`
- `ASCEND_RT_VISIBLE_DEVICES=0,1`
- `SOC_VERSION=ascend910_93`
- `HCCL_OP_EXPANSION_MODE=AIV`
- `tensor_parallel_size=2`
- `pipeline_parallel_size=1`
- `data_parallel_size=1`
- `dtype=torch.bfloat16`
- `quantization=None`, `quantization_config=None`
- `speculative_config=None`
- `enforce_eager=True`
- `compilation_config.mode=NONE`, `cudagraph_mode=NONE`, `max_cudagraph_capture_size=0`
- `enable_prefix_caching=False`
- `enable_chunked_prefill=False`
- `max_model_len=2048`, `max_num_seqs=1`, `max_num_batched_tokens=2048`

Execution evidence:

- file-script Gate E exit code: `0`
- initial stdin-based Gate E attempt exit code: `1`; root cause was Python multiprocessing spawn attempting to reopen `/tmp/<stdin>`. It happened before model construction. The corrected file-script rerun is the valid Gate E execution.
- model recognition: `Resolved architecture: Qwen3_5MoeForConditionalGeneration`
- TP2/HCCL: `world_size=2`, ranks 0/1, backend `hccl`, local ranks 0/1
- workers: `Worker_TP0` and `Worker_TP1` started; CPU binding logged `visible_npus=[0, 1]`
- full weight load: 26/26 safetensors checkpoint shards loaded; `Loading weights took 18.35 seconds`
- FL local path: `PlatformFL` device type `npu`, dispatch key `PrivateUse1`; `ModelRunnerFL_module` and `WorkerFL_module` loaded from site-packages `vllm_fl`
- GDN/Mamba/attention/MoE path evidence: logs show `Patched HybridAttentionMambaModelConfig`, `Patched Mamba batch memcpy`, `Enabled FL-local Ascend C GDN operators and metadata builder`, `Using Triton/FLA GDN prefill kernel`, and `topk_softmax` using `vendor.ascend`
- graph disabled: eager mode disabled torch.compile and CUDAGraphs; config recorded `cudagraph_mode=NONE`
- no `vllm_ascend`: distribution absent and module import failed as required
- no FlagGems: distribution absent, import failed, `USE_FLAGGEMS=0`
- output 1: prompt `Hello, my name is`, text ` John. I am a 30`, 8 output tokens, finite logprobs
- output 2: prompt `The capital of France is`, text ` Paris, a city renowned for its rich`, 8 output tokens, finite logprobs
- repeated generation: two different prompts produced two nonempty different outputs; no stale identical state
- `torch.npu.synchronize()` succeeded after generation
- final NPU status recorded; NPU 0/1 released after run

Evidence:

- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S3-TP2-BF16-EAGER/evidence/20260826T115234p0800/runtime/gate_e_tp2_bf16_eager_file.log`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S3-TP2-BF16-EAGER/evidence/20260826T115234p0800/runtime/gate_e_key_evidence.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S3-TP2-BF16-EAGER/evidence/20260826T115234p0800/runtime/gate_e_tp2_bf16_eager_file.exit_code`

Residual warnings observed but not blocking this PASS claim:

- vLLM emitted warnings that `vllm._C` was absent. This matches the accepted carrier where `vllm` is `0.20.2+empty`; the run used FL `_C_ascend`/OPP and completed TP2 generation.
- vLLM warned that this model does not officially support disabling chunked prefill. The effective config kept `enable_chunked_prefill=False` to honor the bounded first eager task; the run completed successfully.
- torch emitted a shape-format warning during GDN forward. The run completed two generations with finite logprobs and final NPU sync.

## Last Successful Gate And First Blocker

- Last successful gate: `Gate E PASS`
- Last successful step: second generation completed, logprobs finite, `torch.npu.synchronize()` succeeded, process exited 0
- First blocker: `N/A`
- Root-cause confidence: `N/A` because no final blocker remained
- Next Decision request: Codex1 formal Acceptance review for Stage 3 TP2 BF16 eager; do not enter Stage 4 without User dispatch

## Evidence Manifest

- Evidence root: `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S3-TP2-BF16-EAGER/evidence/20260826T115234p0800`
- Evidence manifest: `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S3-TP2-BF16-EAGER/evidence/20260826T115234p0800/manifest.md`
- Evidence checksums: `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S3-TP2-BF16-EAGER/evidence/20260826T115234p0800/checksums.txt`
- Main Stage 3 build log: `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S3-TP2-BF16-EAGER/evidence/20260826T112011p0800/logs/gate_r_build_wheel.log`
- Main Gate E log: `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S3-TP2-BF16-EAGER/evidence/20260826T115234p0800/runtime/gate_e_tp2_bf16_eager_file.log`

## Preserved Runtime And Artifacts

- Container: `qw36-a3-s2-gatec-priv-20260826T092617p0800`
- Container ID: `32562c7139600c25e570ec07841713737b0407c7d1bbdc563be41b87ea105f0a`
- Container state at Result drafting: running
- Installed runtime: current-head standalone `vllm-plugin-fl 0.2.0+ge610a990d`; `vllm-ascend` absent; `USE_FLAGGEMS=0`
- Current-head wheel: `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S3-TP2-BF16-EAGER/artifacts/20260826T112011p0800/wheels/vllm_plugin_fl-0.2.0+ge610a990d-cp311-cp311-linux_aarch64.whl`
- Wheel SHA-256: `2fcf788660f3fe42b364bc60d593ee1b9b634fc0632de58c444d961bff4aa1bd`
- Evidence/artifacts remain under `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S3-TP2-BF16-EAGER`

## Three Pointers

- Code/source pointer: `https://github.com/xiemingda-1002/vllm-plugin-FL`, branch `feature/qwen3.6-35b-a3b-ascend-graph-migration`, run source `e610a990d785356bf51a3cad50219d4c03310a31`, tree `609ff1ad0f08239f353cb4d8774e504b4deba03b`, source clean, Code PR=`N/A`.
- Control pointer: `docs/qwen36-35b-a3b-a3-flagos/tasks/QWEN36-A3-S3-TP2-BF16-EAGER.md`, this immutable resume Result, `docs/qwen36-35b-a3b-a3-flagos/results/INDEX.md`, sync commit recorded in the index after push.
- Evidence pointer: `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S3-TP2-BF16-EAGER/evidence/20260826T115234p0800`, manifest `manifest.md`, checksums `checksums.txt`.
