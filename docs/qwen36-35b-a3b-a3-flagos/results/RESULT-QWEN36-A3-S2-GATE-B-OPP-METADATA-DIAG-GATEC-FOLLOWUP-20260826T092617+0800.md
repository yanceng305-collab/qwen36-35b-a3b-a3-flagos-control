# QWEN36-A3-S2 Gate C/D Follow-up Result

Task: `QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG`
Run timestamp: `2026-08-26T09:26:17+08:00`
Control sync timestamp: `2026-08-26T09:47:00+08:00`
Executor: `root` on `bm-jn-zs-zone1-910C-64G-10-108`
Dispatch: User formal dispatch to continue bounded Gate C diagnosis, reuse the already built A3 wheel, avoid FlagGems installation, and continue Gate D only after Gate C PASS.

## Status

- Lifecycle status: `completed`
- Experiment Result: `PASS`
- Execution: `Execution PASS - Stage 1/2`
- Control Sync: `SYNCED` by the Control commit that first adds this immutable Result
- Codex1 Acceptance: `PENDING`
- Gate A supplement: `PASS`
- Gate B: `PASS` from the exact prior follow-up wheel; not rerun in this Gate C/D follow-up
- Gate C: `PASS`
- Gate D: `PASS`
- Stage 3: `NOT ENTERED / LOCKED`
- Code PR: `N/A`

This Result records a new bounded follow-up run. It does not modify the previous immutable Result, rebuild the wheel, change implementation source, create a Code repo/branch/PR, install FlagGems, load the model, run TP2, run graph, serve, benchmark, profile, or enter Stage 3.

## Task Roots

| Root | Absolute path |
| --- | --- |
| Gate C/D base | `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-C-FLAGGEMS-DIAG` |
| Gate C/D Evidence | `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-C-FLAGGEMS-DIAG/evidence/20260826T092617p0800` |
| Reused Gate B base | `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG` |
| Reused Gate B artifacts | `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/artifacts/20260825T234607p0800` |
| Reused Gate B source | `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/work/src-7beda84-20260825T234607p0800` |

## Source And Wheel Identity

| Field | Value |
| --- | --- |
| Implementation repo | `https://github.com/xiemingda-1002/vllm-plugin-FL.git` |
| Tracked branch | `feature/qwen3.6-35b-a3b-ascend-graph-migration` |
| Execution source HEAD | `7beda84f59d7b25f49cdf03bdf6efecd771067ed` |
| Execution source tree | `a81eea55c1de548a0a1f182f51089eca0b088c82` |
| Official PR base | `flagos-ai/vllm-plugin-FL:release/0.2` |
| Official base SHA/tree | `53adefb269571684d83a51e997d3ba9be5f88235` / `9ddfd080953ad39b39772e108ff921d2973b0299` |
| Source state | exact prior clean source reused; no source patch in this run |
| Wheel | `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/artifacts/20260825T234607p0800/wheels/vllm_plugin_fl-0.2.0+g7beda84f5-cp311-cp311-linux_aarch64.whl` |
| Wheel sha256 | `fa33f586b2e56e78f671989e6dc3dc2ee23f5005c5f0cc4800a6cf6e4b2e98c1` |
| Python ABI | `cp311-cp311-linux_aarch64` |
| A3 family | `ascend910_93` |
| A2 residue audit | prior Gate B strict wheel search for `ascend910b` / `ascend910b1` returned `0` matches |

Gate B was not rerun by this follow-up. The wheel identity, inventory, `_C_ascend`/OPP packaging, and A2-residue audit are inherited from the immutable follow-up Result that first produced the wheel.

## Image, Container And Device

| Field | Value |
| --- | --- |
| Selected official A3 image | `quay.io/ascend/vllm-ascend:v0.20.2rc1-a3-openeuler` |
| Manifest digest | `sha256:442363921166771eb82baeec9c1ac0381f46fb830ead8d0e072df6e925f2a958` |
| Arm64 platform digest | `sha256:a55b2b0597f9fdd1882de9bf3b7ebc395dd77c1ca49f251d0cb759d7b2c1a807` |
| Local image ID | `sha256:8e7823878349b37d9900984555e28381c25681f88ff678cb4a86f7d1674a67c1` |
| Container name | `qw36-a3-s2-gatec-priv-20260826T092617p0800` |
| Container ID | `32562c7139600c25e570ec07841713737b0407c7d1bbdc563be41b87ea105f0a` |
| Container lifecycle | preserved after Stage 1/2 Execution PASS |
| Device mapping | privileged carrier with `/dev/davinci0`, `/dev/davinci1`, `/dev/davinci_manager`, `/dev/devmm_svm`, `/dev/hisi_hdc` plus host Ascend runtime mounts |
| Selected safe scope | physical NPU 0/1, both idle at preflight; unrelated jobs remained on NPU 2-7 |
| PASS preservation | final standalone FL site-packages environment remains installed in the preserved container; wheel/artifacts remain on `/data` |

The User-provided privileged container pattern was adapted only for runtime access. The official A3 openEuler image remained unchanged; `nightly-main-a3` was not used.

Evidence:

- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-C-FLAGGEMS-DIAG/evidence/20260826T092617p0800/environment/host_npu_preflight.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-C-FLAGGEMS-DIAG/evidence/20260826T092617p0800/environment/docker_image_inspect.json`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-C-FLAGGEMS-DIAG/evidence/20260826T092617p0800/environment/container_privileged_create_command.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-C-FLAGGEMS-DIAG/evidence/20260826T092617p0800/environment/container_privileged_final_inspect.json`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-C-FLAGGEMS-DIAG/evidence/20260826T092617p0800/environment/container_preserved_status.txt`

## Environment Tuple

| Component | Observed value |
| --- | --- |
| OS/container | openEuler A3 image, `linux/arm64` |
| Python | `3.11.15` at `/usr/local/python3.11.15/bin/python` |
| CANN | `/usr/local/Ascend/cann-9.0.0` plus host driver runtime mounts |
| torch | `2.10.0+cpu` |
| torch-npu / torch_npu | `2.10.0` |
| vLLM base package | `0.20.2+empty`, editable from `/vllm-workspace/vllm` in the carrier image |
| vLLM-Ascend after Gate C uninstall | distribution absent; `vllm_ascend` import failed as expected |
| vllm-plugin-fl | `0.2.0+g7beda84f5`, installed from the reused wheel |
| triton-ascend distribution | `3.2.1` |
| triton distribution | `3.5.0` |
| Runtime controls | `VLLM_PLUGINS=fl`, `USE_FLAGGEMS=0`, `SOC_VERSION=ascend910_93`, `ASCEND_VISIBLE_DEVICES=0,1`, `ASCEND_RT_VISIBLE_DEVICES=0,1` |
| torch.npu visibility | `is_available=True`, `device_count=2`, device names `Ascend910_9382` |

## Gate C Diagnosis And PASS

Previous visible Gate C blocker:

```text
ModuleNotFoundError: No module named 'flag_gems'
```

Confirmed diagnosis in this run:

- In the earlier non-privileged carrier, `torch_npu` import succeeded but `torch.npu.is_available()` was `False` and `torch.npu.device_count()` was `0`.
- With host Ascend runtime mounts and privileged device access, `torch_npu` import succeeded, `torch.npu.is_available()` became `True`, `torch.npu.device_count()` became `2`, and `DeviceInfo()` selected the Ascend fast-path.
- `DeviceInfo vendor=ascend type=npu dispatch=PrivateUse1 backend=None`.
- `import vllm_fl.platform` succeeded without installing or importing FlagGems.
- `flag_gems`, `flag-gems`, and `flaggems` distributions remained absent, and `USE_FLAGGEMS=0` remained set.

Root-cause confidence: `HIGH` for the previous Gate C failure being an environment/container runtime mapping problem, not a confirmed FL source bug and not a requirement to install FlagGems.

Gate C standalone audit result:

- exit code: `0`
- `vllm_fl.__file__` was `/usr/local/python3.11.15/lib/python3.11/site-packages/vllm_fl/__init__.py`
- no FL source checkout path appeared in `sys.path`
- `vllm-plugin-fl` distribution direct URL hash matched `sha256=fa33f586b2e56e78f671989e6dc3dc2ee23f5005c5f0cc4800a6cf6e4b2e98c1`
- `vllm-ascend` / `vllm_ascend` distributions were absent
- `import vllm_ascend` failed with `ModuleNotFoundError`, as required
- `VLLM_PLUGINS=fl`, `USE_FLAGGEMS=0`
- `vllm_fl.ascend_custom_ops` loaded packaged `_C_ascend` and OPP from site-packages
- selected prebuilt root: `/usr/local/python3.11.15/lib/python3.11/site-packages/vllm_fl/dispatch/backends/vendor/ascend/prebuilt/ascend910_93`
- `ASCEND_CUSTOM_OPP_PATH`: `/usr/local/python3.11.15/lib/python3.11/site-packages/vllm_fl/dispatch/backends/vendor/ascend/prebuilt/ascend910_93/opp/vendors/custom_transformer`
- `PlatformFL.device_type=npu`, `PlatformFL.device_name=npu`, `PlatformFL.dispatch_key=PrivateUse1`

Evidence:

- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-C-FLAGGEMS-DIAG/evidence/20260826T092617p0800/runtime/gate_c_torch_npu_pre_platform_probe.log`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-C-FLAGGEMS-DIAG/evidence/20260826T092617p0800/runtime/gate_c_privileged_fastpath_probe.log`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-C-FLAGGEMS-DIAG/evidence/20260826T092617p0800/runtime/gate_c_privileged_full_standalone_audit.log`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-C-FLAGGEMS-DIAG/evidence/20260826T092617p0800/logs/gate_c_privileged_reused_wheel_install.log`

## Gate D PASS

Custom-op smoke:

```text
torch.ops._C_ascend.npu_add_rms_norm_bias
```

Configuration:

- process cwd: `/tmp`
- environment: `VLLM_PLUGINS=fl`, `USE_FLAGGEMS=0`, `SOC_VERSION=ascend910_93`, `ASCEND_VISIBLE_DEVICES=0,1`, `ASCEND_RT_VISIBLE_DEVICES=0,1`
- input device: `npu:0`
- output device: `npu:0`
- input dtype: `torch.bfloat16`
- output dtype: `torch.bfloat16` for `y` and `x`, `torch.float32` for `rstd`
- input shape: `x1=(2, 1024)`, `x2=(2, 1024)`, `gamma=(1024,)`, `beta=(1024,)`
- output shape: `y=(2, 1024)`, `rstd=(2, 1)`, `x=(2, 1024)`
- synchronization: `torch.npu.synchronize()` completed
- operator origin: `_C_ascend` torch op from wheel-packaged site-packages extension
- OPP origin: wheel-packaged `ascend910_93/opp/vendors/custom_transformer`
- reference: CPU formula `x=x1+x2; rstd=rsqrt(mean(x^2)+eps); y=x*rstd*gamma+beta`
- max absolute difference: `x=0.001953125`, `rstd=0.00013947486877441406`, `y=0.019088029861450195`
- finite/correct: `True`
- no silent CPU fallback: asserted by input/output NPU devices, custom-op package origin, NPU synchronize, and numeric reference check
- numeric exit code: `0`

Evidence:

- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-C-FLAGGEMS-DIAG/evidence/20260826T092617p0800/commands/gate_d_custom_op_smoke_command.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-C-FLAGGEMS-DIAG/evidence/20260826T092617p0800/runtime/gate_d_custom_op_smoke.log`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-C-FLAGGEMS-DIAG/evidence/20260826T092617p0800/runtime/gate_d_custom_op_smoke.exit_code`

## Model Inventory

Model loading, shard validation, TP2, graph, serve, generation, and benchmark were not run. The model artifact remains outside this Stage 1/2 PASS claim.

Expected model path for later Stage 3 identity gate remains:

```text
/data/tiankuan/zyg/FL/workspace/Qwen3.6-35B-A3B
```

## Last Successful Step And First Blocker

- Last successful gate: `Gate D PASS`
- Last successful step: minimal real A3 NPU custom-op smoke using wheel-packaged `_C_ascend`/OPP
- First blocker in this corrected Gate C/D follow-up: `None`
- Previous Gate C blocker disposition: resolved as container runtime mapping issue; no implementation source change required for this Gate C/D follow-up
- Next Decision request: Codex1 formal Acceptance review of Stage 1/2 Execution PASS before any Stage 3 dispatch

## Evidence Manifest

- Evidence manifest: `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-C-FLAGGEMS-DIAG/evidence/20260826T092617p0800/manifest.md`
- Evidence checksums: `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-C-FLAGGEMS-DIAG/evidence/20260826T092617p0800/checksums.txt`
- Main build log from reused Gate B PASS: `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T234607p0800/logs/gate_b_corrected_proxy_build_wheel.log`
- Gate C standalone audit log: `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-C-FLAGGEMS-DIAG/evidence/20260826T092617p0800/runtime/gate_c_privileged_full_standalone_audit.log`
- Gate D smoke log: `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-C-FLAGGEMS-DIAG/evidence/20260826T092617p0800/runtime/gate_d_custom_op_smoke.log`

## Preserved PASS Environment

- Container: `qw36-a3-s2-gatec-priv-20260826T092617p0800`
- Container ID: `32562c7139600c25e570ec07841713737b0407c7d1bbdc563be41b87ea105f0a`
- Container state at Result drafting: `running`
- Standalone FL environment: installed in `/usr/local/python3.11.15/lib/python3.11/site-packages` inside the preserved container
- Wheel: `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/artifacts/20260825T234607p0800/wheels/vllm_plugin_fl-0.2.0+g7beda84f5-cp311-cp311-linux_aarch64.whl`
- Required artifacts: Gate B artifacts under `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/artifacts/20260825T234607p0800`; Gate C/D evidence under `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-C-FLAGGEMS-DIAG/evidence/20260826T092617p0800`

Do not delete or rebuild this PASS container without a later explicit Decision.

## Three Pointers

- Code/source pointer: `https://github.com/xiemingda-1002/vllm-plugin-FL.git`, branch `feature/qwen3.6-35b-a3b-ascend-graph-migration`, run source `7beda84f59d7b25f49cdf03bdf6efecd771067ed`, tree `a81eea55c1de548a0a1f182f51089eca0b088c82`, source clean, Code PR=`N/A`.
- Control pointer: `docs/qwen36-35b-a3b-a3-flagos/tasks/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG.md`, this immutable Result, `docs/qwen36-35b-a3b-a3-flagos/results/INDEX.md`, sync commit recorded in the index after push.
- Evidence pointer: `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-C-FLAGGEMS-DIAG/evidence/20260826T092617p0800`, manifest `manifest.md`, checksums `checksums.txt`.
