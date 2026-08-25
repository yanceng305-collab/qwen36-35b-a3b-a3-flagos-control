# QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG Result

Task: `QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG`
Run timestamp: `2026-08-25T22:45:28+08:00`
Control sync timestamp: `2026-08-25T23:18:00+08:00`
Executor: `root` on `bm-jn-zs-zone1-910C-64G-10-108`
Dispatch: User formal dispatch for this Ready Task in the active thread.

## Status

- Lifecycle status: `blocked`
- Experiment Result: `STOP / DIAGNOSTIC PASS`
- Control Sync: `SYNCED` by the Control commit that first adds this immutable Result
- Codex1 Acceptance: `PENDING`
- Gate A supplement: `PASS`
- Gate B: `STOP`
- Gate C: `NOT RUN`
- Gate D: `NOT RUN`
- Stage 1/2 Execution PASS: `NO`
- Stage 3: `NOT ENTERED / LOCKED`
- Wheel: no wheel produced
- Code PR: `N/A`

This Result records the ended diagnostic run only. It does not patch implementation source, create a Code repo/branch/PR, advance any Stage gate, or authorize Stage 3.

## Task Roots

| Root | Absolute path |
| --- | --- |
| Base | `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG` |
| Work | `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/work` |
| Evidence | `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T224528+0800` |
| Artifacts | `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/artifacts/20260825T224528+0800` |
| Cache | `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/cache` |
| Original reproduction source | `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/work/source-7beda84f59d7b25f49cdf03bdf6efecd771067ed-20260825T224528+0800` |
| Corrected no-plus source | `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/work/source-7beda84f59d7b25f49cdf03bdf6efecd771067ed-20260825T224528p0800-corrected` |

## Source Identity

| Field | Value |
| --- | --- |
| Implementation repo | `https://github.com/xiemingda-1002/vllm-plugin-FL.git` |
| Tracked branch | `feature/qwen3.6-35b-a3b-ascend-graph-migration` |
| Current tracked HEAD at dispatch check | `7beda84f59d7b25f49cdf03bdf6efecd771067ed` |
| Parent execution HEAD | `7beda84f59d7b25f49cdf03bdf6efecd771067ed` |
| Parent execution tree | `a81eea55c1de548a0a1f182f51089eca0b088c82` |
| Official PR base | `flagos-ai/vllm-plugin-FL:release/0.2` |
| Official base SHA | `53adefb269571684d83a51e997d3ba9be5f88235` |
| Official base tree | `9ddfd080953ad39b39772e108ff921d2973b0299` |
| Source state | tracked files clean at the frozen SHA/tree for original reproduction and corrected no-plus attempt |

The corrected no-plus attempt used a fresh local clone of the same exact source SHA/tree. The only untracked preparation was dependency/build output under task-owned work paths; no implementation source patch was applied.

Evidence:

- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T224528+0800/source/current_tracked_ls_remote.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T224528+0800/source/source_clone_identity.log`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T224528+0800/source/corrected_no_plus_source_identity.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T224528+0800/source/source_status_final.txt`

## Image, Container And Device

| Field | Value |
| --- | --- |
| Selected official A3 image | `quay.io/ascend/vllm-ascend:v0.20.2rc1-a3-openeuler` |
| Manifest digest | `sha256:442363921166771eb82baeec9c1ac0381f46fb830ead8d0e072df6e925f2a958` |
| Arm64 platform digest | `sha256:a55b2b0597f9fdd1882de9bf3b7ebc395dd77c1ca49f251d0cb759d7b2c1a807` |
| Local image ID | `sha256:8e7823878349b37d9900984555e28381c25681f88ff678cb4a86f7d1674a67c1` |
| Image platform | `linux/arm64` |
| Container name | `qw36-a3-s2-gateb-opp-diag-20260825T224528p0800` |
| Container ID | `da11e8d0139824d72db50b6660a3818999202ea98e3688984d082fb964287497` |
| Container lifecycle | removed after STOP; no PASS environment existed to preserve |
| Mount | `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG:/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG` |
| Device mapping | `/dev/davinci0`, `/dev/davinci1`, `/dev/davinci_manager`, `/dev/devmm_svm`, `/dev/hisi_hdc` |
| Selected safe scope | physical NPU 0, chip logical IDs 0/1 |
| Post-STOP scope | NPU 0/1 released; unrelated VLLMWorker_TP processes remained on NPU 2-7 |

Host and device preflight confirmed target host `bm-jn-zs-zone1-910C-64G-10-108`, openEuler 22.03 LTS-SP4/aarch64 host, Ascend driver/npu-smi `25.5.0`, 8 physical NPUs with two chips each, and no running processes on selected NPU 0/1 before mutation. Other tasks on NPU 2-7 were not touched.

Evidence:

- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T224528+0800/environment/host_identity.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T224528+0800/environment/npu_smi_preflight.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T224528+0800/environment/docker_image_inspect.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T224528+0800/environment/container_create_command.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T224528+0800/environment/container_inspect.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T224528+0800/environment/container_cleanup.log`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T224528+0800/environment/npu_smi_post_stop.txt`

## Environment Tuple And Triton Supplement

| Component | Observed value |
| --- | --- |
| Python | `3.11.15` at `/usr/local/python3.11.15/bin/python` |
| CANN path | `/usr/local/Ascend/cann-9.0.0` |
| torch | `2.10.0+cpu` |
| torch-npu / torch_npu | `2.10.0` |
| vLLM base package | `0.20.2+empty`, editable from `/vllm-workspace/vllm` in the base image before any Gate C transaction |
| vLLM-Ascend base package | `0.20.2rc1`, editable from `/vllm-workspace/vllm-ascend` in the base image before any Gate C transaction |
| triton-ascend distribution | `3.2.1`, top-level owner `triton` |
| triton distribution | `3.5.0` |
| imported `triton.__version__` | `3.2.0` |
| imported `triton.__file__` | `/usr/local/python3.11.15/lib/python3.11/site-packages/triton/__init__.py` |
| `triton.backends` children | `amd`, `ascend`, `compiler`, `driver`, `nvidia` |
| Ascend backend/provider | `triton.backends.ascend.driver.NPUDriver`; compiler module `triton.backends.ascend.compiler.AscendBackend` |
| top-level `triton_ascend` import | absent, expected package entry is the `triton` namespace |
| Required env controls | `ASCEND_VISIBLE_DEVICES=0,1`, `VLLM_PLUGINS=fl`, `USE_FLAGGEMS=0` |
| CANN op_build | `/usr/local/Ascend/cann-9.0.0/tools/opbuild/op_build`, sha256 `a3cda5b79539564f1e5688a16ac05cda042e9d707c0bc917ee8cb770e4cf0977` |

Gate A supplement result: `PASS`. The parent Triton/provider evidence gap is closed for this diagnostic scope.

Evidence:

- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T224528+0800/environment/triton_provider_identity.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T224528+0800/environment/triton_backend_provider_details.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T224528+0800/environment/pip_show_f_triton_ascend.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T224528+0800/environment/pip_show_f_triton.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T224528+0800/environment/op_build_tool_identity_container.txt`

## Gate B Reproduction

Original reproduction command:

```bash
VLLM_VENDOR=ascend SOC_VERSION=ascend910_93 MAX_JOBS=8 VERBOSE=1 CATLASS_PATH=/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/cache/catlass-41bf90da655bba3c66d0acd7e00abe33960ecfd6 python -m pip wheel --no-build-isolation --no-deps -w /workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/artifacts/20260825T224528+0800/wheels .
```

Initial exact reproduction first stopped earlier on `makeself` FetchContent download. A non-source dependency-cache correction restored the verified `makeself-release-2.5.0-patch1.tar.gz` from the parent run; checksum `bfa730a5763cdb267904a130e02b2e48e464986909c0733ff1c96495f620369a` matched the CMake URL hash. Tracked source remained clean. This restored parent-equivalent dependency availability and the rerun reproduced the parent blocker exactly.

Corrected makeself rerun:

- exit code: `1`
- blocker: `OpFileNotExistsError: File aic-*-ops-info.ini does not exist`
- `ascendc_impl_build.py --opsinfo-dir` actual arguments:
  - `/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/work/source-7beda84f59d7b25f49cdf03bdf6efecd771067ed-20260825T224528+0800/csrc/ascend/build/autogen`
  - `/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/work/source-7beda84f59d7b25f49cdf03bdf6efecd771067ed-20260825T224528+0800/csrc/ascend/build/autogen/inner`
  - `/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/work/source-7beda84f59d7b25f49cdf03bdf6efecd771067ed-20260825T224528+0800/csrc/ascend/build/autogen/exc`
- generated metadata in those directories: none; only empty custom option ini files were present.
- generated Makefiles: `opbuild_gen_default`, `opbuild_gen_inner`, and `opbuild_gen_exc` existed but contained no `/tools/opbuild/op_build` command and depended only on `libop_host_aclnn*.so`.

Main reproduction log:

```text
/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T224528+0800/logs/gate_b_corrected_makeself_build_wheel.log
```

Evidence:

- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T224528+0800/commands/gate_b_reproduction_command.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T224528+0800/commands/gate_b_corrected_makeself_command.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T224528+0800/logs/gate_b_reproduction_build_wheel.log`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T224528+0800/logs/gate_b_corrected_makeself_build_wheel.log`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T224528+0800/logs/gate_b_corrected_makeself_build_wheel.exit_code.observed`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T224528+0800/generated/gate_b_corrected_makeself_after_tree_and_checksums.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T224528+0800/generated/makefile_targets_relevant.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T224528+0800/generated/generated_op_build_command_search.txt`

## Metadata Diagnosis

Root-cause category: `path naming / CMake selected-op metadata flow`.

Root-cause confidence: `HIGH`.

Confirmed facts:

- The exact source path used for the parent-blocker reproduction contained `+0800`.
- `custom_build.cmake` uses `string(REGEX MATCH "^${CMAKE_CURRENT_SOURCE_DIR}" is_match "${_src}")` while classifying `op_host_aclnn*` source files into generated `aclnn_*.cpp/h` and `*_proto.*` outputs.
- `${CMAKE_CURRENT_SOURCE_DIR}` is used as a regular expression, not as an escaped literal. In the failed reproduction path, `+` is regex syntax.
- CMake trace shows `if(is_match)` was skipped for the selected op definition files even though `get_target_property(base_aclnn_srcs op_host_aclnn SOURCES)` and `base_aclnn_exclude_srcs` contained same-tree absolute source paths.
- Because `generate_aclnn_srcs`, `generate_aclnn_inner_srcs`, and `generate_exclude_proto_srcs` were empty at the `custom_build.cmake` opbuild section, the generated `opbuild_gen_default`, `opbuild_gen_inner`, and `opbuild_gen_exc` targets carried no generated outputs and no `op_build` command.
- `generate_transformer_adapt_py` still ran `ascendc_impl_build.py` and looked in `build/autogen`, `build/autogen/inner`, and `build/autogen/exc`; with no `op_build` metadata generation attached, no `aic-*-ops-info.ini` existed and the exception was deterministic.
- All eight selected op definition files contain `AddConfig("ascend910_93")`, so the parent blocker is not explained by a missing A3 config in a specific op definition.
- The matched-version helper comparison showed FL's `ascendc_impl_build.py` hash matches the base image's vLLM-Ascend helper; the observed failure occurs before the helper receives metadata files.

Corrected non-source validation:

- A fresh same-SHA clean clone was created at a no-plus path: `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/work/source-7beda84f59d7b25f49cdf03bdf6efecd771067ed-20260825T224528p0800-corrected`.
- Running the same build command from that source path generated expected metadata:
  - `csrc/ascend/build/autogen/aic-ascend910_93-ops-info.ini`
  - `csrc/ascend/build/autogen/aic-ascend910_93-ops-info.json`
  - `csrc/ascend/build/autogen/exc/aic-ascend910_93-ops-info.ini`
  - generated default `aclnn_*.cpp/h` and excluded-op `*_proto.*` files.
- The corrected generated Makefiles contained explicit `OPS_PROTO_SEPARATE`, `OPS_ACLNN_GEN`, `OPS_PROJECT_NAME`, `OPS_PRODUCT_NAME="ascend910_93;"`, and `/usr/local/Ascend/cann-9.0.0/tools/opbuild/op_build` commands.

This confirms the parent `OpFileNotExistsError` as a path naming / CMake regex classification failure in the execution workspace. A source patch may be useful for robustness, but is not required to clear this exact blocker within the existing route; therefore no implementation source modification was made in this Task.

Evidence:

- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T224528+0800/logs/diagnostic_cmake_trace_prepare_build.log`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T224528+0800/generated/diagnostic_cmake_trace_aclnn_flow.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T224528+0800/generated/root_cause_diagnosis_summary.md`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T224528+0800/generated/corrected_no_plus_metadata_inventory_and_checksums.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T224528+0800/generated/corrected_no_plus_opbuild_command_search.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T224528+0800/static/ascendc_impl_build_opsinfo_lookup.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T224528+0800/static/official_helper_comparison.txt`

## Corrected Gate B Attempt And Remaining Blocker

Corrected no-plus command:

```bash
VLLM_VENDOR=ascend SOC_VERSION=ascend910_93 MAX_JOBS=8 VERBOSE=1 CATLASS_PATH=/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/cache/catlass-41bf90da655bba3c66d0acd7e00abe33960ecfd6 python -m pip wheel --no-build-isolation --no-deps -w /workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/artifacts/20260825T224528+0800/wheels-corrected-no-plus .
```

Corrected no-plus attempt result:

- exit code: `1`
- parent metadata blocker: cleared; `aic-ascend910_93-ops-info.ini` was generated in the expected directories.
- Gate B wheel result: `STOP`; no wheel produced.
- remaining blocker: CMake ExternalProject downloads for `json` and `abseil-cpp` from `gitcode.com` failed with DNS resolution errors (`Could not resolve host: gitcode.com`).

Because the corrected Gate B attempt did not produce a wheel, the Task stopped at Gate B and did not enter Gate C or Gate D.

Main corrected build log:

```text
/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T224528+0800/logs/gate_b_corrected_no_plus_path_build_wheel.log
```

Evidence:

- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T224528+0800/commands/gate_b_corrected_no_plus_path_command.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T224528+0800/logs/gate_b_corrected_no_plus_path_build_wheel.log`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T224528+0800/logs/gate_b_corrected_no_plus_path_build_wheel.exit_code.observed`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T224528+0800/generated/corrected_no_plus_metadata_inventory_and_checksums.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T224528+0800/generated/corrected_no_plus_opbuild_command_search.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T224528+0800/artifacts/wheel_output_audit.txt`

## OPP And Schema Observations

Selected exact-source OPP op set:

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

Per-op status:

- `add_rms_norm_bias`, `causal_conv1d`, `moe_gating_top_k`, and `moe_init_routing_custom`: default `aclnn` op definitions, `AddConfig("ascend910_93")` present, entered `op_host_aclnn`, and generated `aclnn_*.cpp/h` plus default `aic-ascend910_93-ops-info.ini` in corrected no-plus attempt.
- `apply_top_k_top_p_custom`, `chunk_fwd_o`, `chunk_gated_delta_rule_fwd_h`, and `recurrent_gated_delta_rule`: `aclnn_exclude` op definitions, `AddConfig("ascend910_93")` present, entered `op_host_aclnnExc`, and generated `*_proto.*` plus `exc/aic-ascend910_93-ops-info.ini` in corrected no-plus attempt.
- `op_host_aclnnInner`: no selected inner op; stub was expected and not causal for this blocker.

Difference from older PR prose remains the same as the parent run: exact source has 8 OPP definitions and includes the newer `apply_top_k_top_p_custom`; `_C_ascend` schema inventory includes `npu_apply_top_k_top_p`. No wheel was produced in this Task, so wheel-level schema inventory and A2-residue audit remain not run.

Evidence:

- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T224528+0800/generated/per_op_build_observed_files.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T224528+0800/static/config_opbuild_selection_snippets.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T224528+0800/generated/corrected_no_plus_metadata_inventory_and_checksums.txt`

## Ranked Hypotheses

1. `HIGH / CONFIRMED`: path naming plus CMake regex classification caused the parent `aic-*-ops-info.ini` absence. Evidence: trace and no-plus rerun.
2. `HIGH / CONFIRMED as remaining blocker`: corrected Gate B did not finish because `gitcode.com` third-party dependency downloads for `json`/`abseil-cpp` failed DNS resolution. Evidence: corrected no-plus build log.
3. `LOW / NOT SUPPORTED`: a specific selected op lacks `ascend910_93` definition. Evidence instead shows all selected op defs contain `AddConfig("ascend910_93")`.
4. `LOW / NOT SUPPORTED`: CANN `op_build` cannot generate A3 metadata for this op set. Evidence instead shows corrected no-plus path generated `aic-ascend910_93-ops-info.ini`.
5. `LOW / NOT SUPPORTED`: `ascendc_impl_build.py` looked in the wrong directory for the parent reproduction. Evidence instead shows the expected lookup dirs were empty because upstream metadata generation was not attached.

## Gate C

Gate C: `NOT RUN`.

Reason: Gate B did not produce an A3-native wheel. No standalone FL site-packages install, `vllm-ascend` uninstall, `vllm_ascend` negative import/entrypoint audit, FL `PYTHONPATH`/editable audit, PlatformFL/provider trace, or `_C_ascend`/OPP origin check was run.

## Gate D

Gate D: `NOT RUN`.

Reason: there was no installed standalone FL wheel and no packaged A3 `_C_ascend`/OPP origin to smoke. No custom-op execution, NPU input/output assertion, synchronization, reference check, finite check, or CPU fallback audit was run.

## Model Inventory

Model: `Qwen/Qwen3.6-35B-A3B`
dtype: BF16, non-quantized
Path: `/data/tiankuan/zyg/FL/workspace/Qwen3.6-35B-A3B`
State: User-declared `DOWNLOADING / NOT YET READY FOR STAGE 3`

Only inventory/presence state was recorded. No model load, shard completeness verification as a PASS condition, TP2, HCCL model execution, generation, serving, graph, prefix, EP, 64K, benchmark, or profiling was run.

Evidence:

- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T224528+0800/model/model_inventory_only.txt`

## Evidence Manifest

Evidence root:

```text
/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T224528+0800
```

Evidence manifest:

```text
/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T224528+0800/manifest.md
```

Evidence checksums:

```text
/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T224528+0800/checksums.txt
```

Main build log:

```text
/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T224528+0800/logs/gate_b_corrected_no_plus_path_build_wheel.log
```

Parent-blocker reproduction log:

```text
/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T224528+0800/logs/gate_b_corrected_makeself_build_wheel.log
```

## Last Successful Step And Blocker

Last successful step: Gate A supplement passed; parent Gate B metadata blocker was reproduced and then explained by CMake trace and a no-plus same-SHA rerun that generated the expected A3 metadata files.

First blocker for the parent Gate B reproduction:

```text
OpFileNotExistsError: File aic-*-ops-info.ini does not exist
```

First remaining blocker after the non-source no-plus correction:

```text
CMake ExternalProject download failed: Could not resolve host: gitcode.com
```

Next Decision request: authorize a bounded follow-up Gate B closure run that uses a no-regex-metacharacter source path and either working network/proxy access for `gitcode.com` third-party dependencies or verified offline artifacts for `json` and `abseil-cpp`. No implementation source patch is proposed by this Result for the parent metadata blocker.

## Three Pointers

Code/source pointer:

```text
https://github.com/xiemingda-1002/vllm-plugin-FL.git
branch: feature/qwen3.6-35b-a3b-ascend-graph-migration
HEAD: 7beda84f59d7b25f49cdf03bdf6efecd771067ed
tree: a81eea55c1de548a0a1f182f51089eca0b088c82
Code PR: N/A
```

Control pointer:

```text
https://github.com/yanceng305-collab/qwen36-35b-a3b-a3-flagos-control
path: docs/qwen36-35b-a3b-a3-flagos/results/RESULT-QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG-20260825T224528+0800.md
commit: the Control commit that first adds this immutable Result
```

Evidence pointer:

```text
/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T224528+0800
main build log: /data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T224528+0800/logs/gate_b_corrected_no_plus_path_build_wheel.log
parent-blocker reproduction log: /data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T224528+0800/logs/gate_b_corrected_makeself_build_wheel.log
manifest: /data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T224528+0800/manifest.md
checksums: /data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T224528+0800/checksums.txt
```
