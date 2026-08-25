# QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG Follow-up Result

Task: `QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG`
Run timestamp: `2026-08-25T23:46:07+08:00`
Control sync timestamp: `2026-08-26T00:06:54+08:00`
Executor: `root` on `bm-jn-zs-zone1-910C-64G-10-108`
Dispatch: User formal dispatch to continue the bounded Gate B follow-up for the remaining `gitcode.com` network blocker.

## Status

- Lifecycle status: `blocked`
- Experiment Result: `STOP`
- Control Sync: `SYNCED` by the Control commit that first adds this immutable Result
- Codex1 Acceptance: `PENDING`
- Gate A supplement: `PASS`
- Gate B: `PASS`
- Gate C: `STOP`
- Gate D: `NOT RUN`
- Stage 1/2 Execution PASS: `NO`
- Stage 3: `NOT ENTERED / LOCKED`
- Code PR: `N/A`

This Result records a new follow-up run. It does not modify the previous immutable Result, patch implementation source, create a Code repo/branch/PR, run Gate D, load the model, or enter Stage 3.

## Task Roots

| Root | Absolute path |
| --- | --- |
| Base | `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG` |
| Work | `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/work` |
| Evidence | `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T234607p0800` |
| Artifacts | `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/artifacts/20260825T234607p0800` |
| Cache | `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/cache` |
| Source | `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/work/src-7beda84-20260825T234607p0800` |

The source path deliberately avoided `+` and other regex-significant timestamp characters.

## Source Identity

| Field | Value |
| --- | --- |
| Implementation repo | `https://github.com/xiemingda-1002/vllm-plugin-FL.git` |
| Tracked branch | `feature/qwen3.6-35b-a3b-ascend-graph-migration` |
| Current tracked HEAD observed at follow-up start | `e610a990d785356bf51a3cad50219d4c03310a31` |
| Reproduction source HEAD | `7beda84f59d7b25f49cdf03bdf6efecd771067ed` |
| Reproduction source tree | `a81eea55c1de548a0a1f182f51089eca0b088c82` |
| Parent execution HEAD/tree | `7beda84f59d7b25f49cdf03bdf6efecd771067ed` / `a81eea55c1de548a0a1f182f51089eca0b088c82` |
| Official PR base | `flagos-ai/vllm-plugin-FL:release/0.2` |
| Official base SHA/tree | `53adefb269571684d83a51e997d3ba9be5f88235` / `9ddfd080953ad39b39772e108ff921d2973b0299` |
| Source state | clean tracked files before and after build/install audit |
| CATLASS | `41bf90da655bba3c66d0acd7e00abe33960ecfd6` |

The moving tracked branch was recorded but was not used to replace the parent diagnostic identity, per dispatch.

Evidence:

- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T234607p0800/source/current_tracked_ls_remote.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T234607p0800/source/source_identity_initial.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T234607p0800/source/source_status_final.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T234607p0800/source/catlass_identity.txt`

## Image, Container And Device

| Field | Value |
| --- | --- |
| Selected official A3 image | `quay.io/ascend/vllm-ascend:v0.20.2rc1-a3-openeuler` |
| Manifest digest | `sha256:442363921166771eb82baeec9c1ac0381f46fb830ead8d0e072df6e925f2a958` |
| Arm64 platform digest | `sha256:a55b2b0597f9fdd1882de9bf3b7ebc395dd77c1ca49f251d0cb759d7b2c1a807` |
| Local image ID | `sha256:8e7823878349b37d9900984555e28381c25681f88ff678cb4a86f7d1674a67c1` |
| Container name | `qw36-a3-s2-gateb-net-followup-20260825T234607p0800` |
| Container ID | `82a815b4b7576230f3f786b95583f9d1b8400a1622b5a82752c62fcd444444f6` |
| Container lifecycle | removed after Gate C STOP; no PASS environment existed to preserve |
| Mount | `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG:/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG` |
| Device mapping | `/dev/davinci0`, `/dev/davinci1`, `/dev/davinci_manager`, `/dev/devmm_svm`, `/dev/hisi_hdc` |
| Selected safe scope | physical NPU 0, chip logical IDs 0/1 |
| Post-STOP scope | NPU 0/1 released; unrelated VLLMWorker_TP jobs remained on NPU 2-7 |

Evidence:

- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T234607p0800/environment/host_identity.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T234607p0800/environment/npu_smi_preflight.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T234607p0800/environment/docker_image_inspect.json`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T234607p0800/environment/container_create_command.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T234607p0800/environment/container_cleanup.log`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T234607p0800/environment/npu_smi_post_stop.txt`

## Environment Tuple

| Component | Observed value |
| --- | --- |
| OS/container | openEuler A3 image, `linux/arm64` |
| Python | `3.11.15` at `/usr/local/python3.11.15/bin/python` |
| CANN | `/usr/local/Ascend/cann-9.0.0` |
| torch | `2.10.0+cpu` |
| torch-npu / torch_npu | `2.10.0` |
| vLLM base package | `0.20.2+empty`, editable from `/vllm-workspace/vllm` in the carrier image |
| vLLM-Ascend before Gate C | `0.20.2rc1`, editable from `/vllm-workspace/vllm-ascend` in the carrier image |
| vLLM-Ascend after Gate C uninstall | distribution absent; `vllm_ascend` import failed as expected |
| transformers | `5.5.3` |
| triton-ascend distribution | `3.2.1` |
| triton distribution | `3.5.0` |
| imported `triton.__version__` | `3.2.0` |
| imported `triton.__file__` | `/usr/local/python3.11.15/lib/python3.11/site-packages/triton/__init__.py` |
| Ascend provider | `triton.backends.ascend.driver.NPUDriver`; compiler `triton.backends.ascend.compiler.AscendBackend` |
| Build controls | `VLLM_VENDOR=ascend`, `SOC_VERSION=ascend910_93`, `MAX_JOBS=8`, `VERBOSE=1` |
| Runtime controls | `VLLM_PLUGINS=fl`, `USE_FLAGGEMS=0`, `SOC_VERSION=ascend910_93`, `ASCEND_VISIBLE_DEVICES=0,1` |

Evidence:

- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T234607p0800/environment/container_env_and_triton_supplement.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T234607p0800/environment/triton_provider_identity.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T234607p0800/runtime/gate_c_standalone_audit.log`

## Network Follow-up

The previous remaining blocker was `gitcode.com` DNS/network failure while downloading CANN third-party `json` and `abseil-cpp`. This run used the server's existing proxy configuration without committing proxy credentials to Control. Container GET probes for both dependency URLs returned HTTP 200 and downloaded nonzero bytes:

- `https://gitcode.com/cann-src-third-party/json/releases/download/v3.11.3/include.zip`
- `https://gitcode.com/cann-src-third-party/abseil-cpp/releases/download/20230802.1/abseil-cpp-20230802.1.tar.gz`

Evidence:

- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T234607p0800/network/previous_gitcode_blocker_excerpt.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T234607p0800/network/gitcode_container_get_probe.log`

## Gate B PASS

Corrected build command:

```bash
cd /workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/work/src-7beda84-20260825T234607p0800
VLLM_VENDOR=ascend SOC_VERSION=ascend910_93 MAX_JOBS=8 VERBOSE=1 \
  CATLASS_PATH=/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/cache/catlass-41bf90da655bba3c66d0acd7e00abe33960ecfd6 \
  python -m pip wheel --no-build-isolation --no-deps \
  -w /workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/artifacts/20260825T234607p0800/wheels .
```

Result:

- exit code: `0`
- wheel: `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/artifacts/20260825T234607p0800/wheels/vllm_plugin_fl-0.2.0+g7beda84f5-cp311-cp311-linux_aarch64.whl`
- wheel sha256: `fa33f586b2e56e78f671989e6dc3dc2ee23f5005c5f0cc4800a6cf6e4b2e98c1`
- Python ABI: `cp311-cp311-linux_aarch64`
- effective family: `ascend910_93`
- generated expected A3 metadata, including `aic-ascend910_93-ops-info.ini` and `aic-ascend910_93-ops-info.json`
- packaged FL prebuilt root: `vllm_fl/dispatch/backends/vendor/ascend/prebuilt/ascend910_93`
- strict wheel search for `ascend910b` / `ascend910b1`: `0` matches

Selected exact-source OPP set remains:

```text
add_rms_norm_bias
causal_conv1d
recurrent_gated_delta_rule
chunk_fwd_o
chunk_gated_delta_rule_fwd_h
moe_gating_top_k
moe_init_routing_custom
apply_top_k_top_p_custom
```

Wheel `_C_ascend` schema set includes the nine current schemas, including `npu_apply_top_k_top_p`.

Evidence:

- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T234607p0800/commands/gate_b_corrected_proxy_build_command.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T234607p0800/logs/gate_b_corrected_proxy_build_wheel.log`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T234607p0800/logs/gate_b_corrected_proxy_build_wheel.exit_code.observed`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T234607p0800/artifacts/wheel_files_and_hashes.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T234607p0800/artifacts/wheel_inventory.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T234607p0800/artifacts/wheel_a3_a2_residue_audit.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T234607p0800/artifacts/wheel_strict_a2_residue_search.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T234607p0800/source/source_status_and_generated_metadata_after_build.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T234607p0800/static/torch_binding_schemas.txt`

## Gate C STOP

Gate C install transaction:

```bash
cd /tmp
python -m pip uninstall -y vllm-ascend vllm-plugin-fl
python -m pip install --no-deps --force-reinstall /workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/artifacts/20260825T234607p0800/wheels/vllm_plugin_fl-0.2.0+g7beda84f5-cp311-cp311-linux_aarch64.whl
```

Install transaction result:

- exit code: `0`
- `vllm_ascend 0.20.2rc1` uninstalled
- `vllm-plugin-fl 0.2.0+g7beda84f5` installed from this run's wheel

Standalone new-process audit facts from `/tmp`:

- `vllm-plugin-fl` distribution came from the run wheel, with direct URL hash `sha256=fa33f586b2e56e78f671989e6dc3dc2ee23f5005c5f0cc4800a6cf6e4b2e98c1`
- `vllm_fl.__file__` was `/usr/local/python3.11.15/lib/python3.11/site-packages/vllm_fl/__init__.py`
- no FL source checkout path appeared in `sys.path`
- `vllm-ascend` / `vllm_ascend` distributions were absent
- `import vllm_ascend` failed with `ModuleNotFoundError`, as required
- `VLLM_PLUGINS=fl` and `USE_FLAGGEMS=0`
- `vllm_fl.ascend_custom_ops` loaded the packaged `_C_ascend` and OPP from `site-packages`
- selected prebuilt root was `/usr/local/python3.11.15/lib/python3.11/site-packages/vllm_fl/dispatch/backends/vendor/ascend/prebuilt/ascend910_93`
- `ASCEND_CUSTOM_OPP_PATH` was set to the wheel's `opp/vendors/custom_transformer`

First blocker:

```text
ModuleNotFoundError: No module named 'flag_gems'
```

Failure location:

```text
import vllm_fl.platform
  -> class PlatformFL(Platform)
  -> DeviceInfo()
  -> vllm_fl.utils._load_flaggems_device_runtime()
  -> import flag_gems
```

`flag_gems`, `flag-gems`, and `flaggems` distributions were absent after the required `vllm-ascend` uninstall. Because the task contract requires actual `PlatformFL` identity and no FlagGems activation under `USE_FLAGGEMS=0`, the run stopped at Gate C. Gate D was not run.

Root-cause confidence: `HIGH` for the immediate Gate C failure mechanism and missing distribution. Source-change necessity is `NOT DETERMINED`; this Result does not claim whether the bounded follow-up should install a validated `flag_gems` package, change packaging dependencies, or patch optional import behavior.

Evidence:

- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T234607p0800/commands/gate_c_install_command.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T234607p0800/logs/gate_c_install.log`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T234607p0800/logs/gate_c_install.exit_code.observed`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T234607p0800/runtime/gate_c_standalone_audit.log`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T234607p0800/runtime/gate_c_platform_import_traceback.log`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T234607p0800/runtime/gate_c_flaggems_distribution_check.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T234607p0800/runtime/gate_c_flaggems_platform_static_trace.txt`

## Gate D

Gate D: `NOT RUN`.

Reason: Gate C did not pass. No full model, TP2, graph, serve, benchmark, profiling, or Stage 3 execution was performed.

## Last Successful Step And First Blocker

- Last successful gate: `Gate B PASS`
- Last successful runtime step: Gate C uninstall/install transaction and partial standalone wheel/origin/_C_ascend/OPP audit
- First blocker: `ModuleNotFoundError: No module named 'flag_gems'` during `import vllm_fl.platform`
- Next Decision request: decide the bounded Gate C follow-up route for `flag_gems` absence after required `vllm-ascend` uninstall, without changing the already-passed Gate B facts.

## Evidence Manifest

- Evidence manifest: `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T234607p0800/manifest.md`
- Evidence checksums: `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T234607p0800/checksums.txt`
- Main build log: `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T234607p0800/logs/gate_b_corrected_proxy_build_wheel.log`
- Gate C blocker log: `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T234607p0800/runtime/gate_c_platform_import_traceback.log`

## Three Pointers

- Code/source pointer: `https://github.com/xiemingda-1002/vllm-plugin-FL.git`, branch `feature/qwen3.6-35b-a3b-ascend-graph-migration`, run source `7beda84f59d7b25f49cdf03bdf6efecd771067ed`, tree `a81eea55c1de548a0a1f182f51089eca0b088c82`, source clean, Code PR=`N/A`.
- Control pointer: `docs/qwen36-35b-a3b-a3-flagos/tasks/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG.md`, this immutable Result, `docs/qwen36-35b-a3b-a3-flagos/results/INDEX.md`, sync commit recorded in the index after push.
- Evidence pointer: `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T234607p0800`, manifest `manifest.md`, checksums `checksums.txt`.
