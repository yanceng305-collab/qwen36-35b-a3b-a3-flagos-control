# QWEN36-A3-S1S2-ENV-BUILD-RUNTIME Result

Task: `QWEN36-A3-S1S2-ENV-BUILD-RUNTIME`
Run timestamp: `2026-08-25T20:54:24+08:00`
Control sync timestamp: `2026-08-25T21:42:11+08:00`
Executor: `root` on `bm-jn-zs-zone1-910C-64G-10-108`
Dispatch: User formal dispatch for the Ready Task in this thread.

## Status

- Lifecycle status: `blocked`
- Experiment Result: `STOP`
- Control Sync: `SYNCED` by the Control commit that first adds this immutable Result
- Codex1 Acceptance: `PENDING`
- Last successful gate: Gate A, environment/image/source/device identity
- Stopped gate: Gate B, clean A3-native `ascend910_93` wheel build
- First blocker: `OpFileNotExistsError: File aic-*-ops-info.ini does not exist`
- Wheel: no wheel produced
- Gate C: `NOT RUN`
- Gate D: `NOT RUN`
- Code PR: `N/A`

This Result records the ended run only. It does not advance Stage gate status, create a follow-up task, or authorize Stage 3.

## Task Roots

| Root | Absolute path |
| --- | --- |
| Base | `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S1S2-ENV-BUILD-RUNTIME` |
| Work | `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S1S2-ENV-BUILD-RUNTIME/work` |
| Evidence | `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S1S2-ENV-BUILD-RUNTIME/evidence` |
| Artifacts | `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S1S2-ENV-BUILD-RUNTIME/artifacts` |
| Cache | `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S1S2-ENV-BUILD-RUNTIME/cache` |
| Local preliminary results | `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S1S2-ENV-BUILD-RUNTIME/results` |

## Source Identity

| Field | Value |
| --- | --- |
| Implementation repo | `https://github.com/xiemingda-1002/vllm-plugin-FL.git` |
| Tracked branch | `feature/qwen3.6-35b-a3b-ascend-graph-migration` |
| Frozen HEAD | `7beda84f59d7b25f49cdf03bdf6efecd771067ed` |
| Frozen tree | `a81eea55c1de548a0a1f182f51089eca0b088c82` |
| Compare base observed | `main@38e7dbc20197e2db742c4e4c9687d36ea4df9900` |
| Clean clone | `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S1S2-ENV-BUILD-RUNTIME/work/source-git-shallow-7beda84-20260825T184948+0800` |
| Clean build source | `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S1S2-ENV-BUILD-RUNTIME/work/build-source-7beda84-20260825T205051+0800` |
| Source state | clean at frozen SHA/tree for the formal build attempt |

The requested Control files were not part of the implementation repo exact tree; the governing Control rules for this sync are from this Control repo plus the User dispatch.

## Image And Environment

| Field | Value |
| --- | --- |
| Selected official A3 image | `quay.io/ascend/vllm-ascend:v0.20.2rc1-a3-openeuler` |
| Image digest | `sha256:442363921166771eb82baeec9c1ac0381f46fb830ead8d0e072df6e925f2a958` |
| Arm64 platform digest | `sha256:a55b2b0597f9fdd1882de9bf3b7ebc395dd77c1ca49f251d0cb759d7b2c1a807` |
| Local image ID | `sha256:8e7823878349b37d9900984555e28381c25681f88ff678cb4a86f7d1674a67c1` |
| Host OS / arch | openEuler 22.03 LTS-SP4 / `aarch64` |
| Host driver | `25.5.0` |
| Host CANN | `9.0.1` |
| Container OS | openEuler 24.03 LTS-SP2 |
| Python | `3.11.15` |
| CANN in container | `9.0.0` |
| torch | `2.10.0+cpu` |
| torch_npu | `2.10.0` |
| vLLM | `0.20.2+empty` |
| vLLM-Ascend base image package | `0.20.2rc1`, present before Gate C uninstall step |
| Transformers | `5.5.3` |
| Triton | `3.5.0` |
| Build tools | GCC/G++ `12.3.1`, CMake `4.3.2`, Ninja `1.13.0` |

Selection reason: the host is openEuler/aarch64 with Ascend driver 25.5.0, so the official A3 openEuler candidate best matched the host OS family and required vLLM/CANN/torch_npu tuple. The ordinary unsuffixed `quay.io/ascend/vllm-ascend:v0.20.2rc1` tag was excluded as the A2 route.

## Device Scope

| Field | Value |
| --- | --- |
| Physical target | A3/910C host `bm-jn-zs-zone1-910C-64G-10-108` |
| Logical mapping | `npu-smi info -m` reported 8 physical NPU IDs, each with 2 chips, logical chip IDs 0-15 |
| Selected minimal working scope | Physical NPU 0, chip logical IDs 0 and 1 |
| Device mapping | `/dev/davinci0` and `/dev/davinci1` |
| Runtime NPU observation | `torch.npu.is_available=True`, `device_count=2`, device name `Ascend910_9382` |
| Owner/occupancy boundary | NPU 0/1 had no running task process for this run's safe scope; NPU 2-7 had active unrelated VLLMWorker_TP processes |
| Post-STOP state | Task container removed; NPU 0/1 released |

## Container Lifecycle

| Field | Value |
| --- | --- |
| Task container name | `qw36-a3-s1s2-env-pass-20260825190917` |
| Task container ID | `9f03ddc88115aec2865ae099596f8cd383a2647493397b72ce1f8f82d6c66adb` |
| Lifecycle result | removed after STOP because Gate A-D did not all pass and no PASS environment existed to preserve |
| Cleanup evidence | `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S1S2-ENV-BUILD-RUNTIME/evidence/20260825T211000+0800/container_cleanup.log` |

## Gate A

Gate A: `PASS`.

Evidence captured actual A3/910C target identity, logical mapping, safe device scope, official A3 image selection, environment tuple, source SHA/tree, task roots, cache identity, and model inventory-only state. The selected image and source identities are listed above.

Key Evidence:

- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S1S2-ENV-BUILD-RUNTIME/evidence/20260825T184300+0800/npu_smi_info.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S1S2-ENV-BUILD-RUNTIME/evidence/20260825T184300+0800/github_ref.json`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S1S2-ENV-BUILD-RUNTIME/evidence/20260825T184300+0800/github_commit.json`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S1S2-ENV-BUILD-RUNTIME/evidence/20260825T184300+0800/source_archive_identity.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S1S2-ENV-BUILD-RUNTIME/evidence/20260825T190400+0800/container_gate_a.txt`

## Gate B

Gate B: `STOP`.

Exact build command/config:

```bash
VLLM_VENDOR=ascend \
SOC_VERSION=ascend910_93 \
MAX_JOBS=8 \
VERBOSE=1 \
CATLASS_PATH=/workspace/QWEN36-A3-S1S2-ENV-BUILD-RUNTIME/cache/catlass-41bf90da655bba3c66d0acd7e00abe33960ecfd6 \
python -m pip wheel --no-build-isolation --no-deps \
  -w /workspace/QWEN36-A3-S1S2-ENV-BUILD-RUNTIME/artifacts/wheels .
```

Additional execution controls: `VLLM_PLUGINS=fl`, `USE_FLAGGEMS=0`. CATLASS was bound to exact commit `41bf90da655bba3c66d0acd7e00abe33960ecfd6`.

Last successful step: Gate A completed and CATLASS exact commit was prepared; the second Gate B build reached CANN OPP prepare/build with `ASCEND_COMPUTE_UNIT=ascend910_93`.

First blocker:

```text
OpFileNotExistsError: File aic-*-ops-info.ini does not exist
```

Observed build exit code: `1`.

Main build log:

```text
/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S1S2-ENV-BUILD-RUNTIME/evidence/20260825T205100+0800/build_wheel.log
```

The build log records `ASCEND_COMPUTE_UNIT=ascend910_93` and the configured OPP op set. It then fails during CANN OPP prepare/build before wheel creation. The wheel output directory produced no formal wheel; therefore no wheel filename, wheel hash, Python ABI wheel inventory, installed `_C_ascend` origin, or packaged OPP origin exists for this run.

Follow-up hypothesis, not confirmed root cause: the A3 OPP definition/build path may need a bounded source change so CANN emits the required `aic-*-ops-info.ini` files for `ascend910_93`.

## Gate C

Gate C: `NOT RUN`.

Reason: Gate B produced no A3-native wheel. The standalone FL site-packages install, `vllm-ascend` uninstall audit, `vllm_ascend` negative import/entrypoint audit, FL `PYTHONPATH`/editable audit, PlatformFL/provider trace, and `_C_ascend`/OPP load checks were not run.

## Gate D

Gate D: `NOT RUN`.

Reason: there was no installed standalone wheel and no packaged A3 `_C_ascend`/OPP origin to smoke. No custom-op execution, NPU input/output assertion, synchronization, reference check, finite check, or CPU-fallback audit was run.

## OPP And Schema Reconciliation

Exact HEAD source facts recorded for this run:

- OPP build op set count: 8
- OPP build ops: `add_rms_norm_bias`, `causal_conv1d`, `recurrent_gated_delta_rule`, `chunk_fwd_o`, `chunk_gated_delta_rule_fwd_h`, `moe_gating_top_k`, `moe_init_routing_custom`, `apply_top_k_top_p_custom`
- `_C_ascend` torch schema count: 9
- Schemas: `npu_moe_init_routing_custom`, `npu_apply_top_k_top_p`, `moe_gating_top_k`, `npu_gemma_rms_norm`, `npu_add_rms_norm_bias`, `npu_recurrent_gated_delta_rule`, `npu_causal_conv1d_custom`, `chunk_gated_delta_rule_fwd_h`, `chunk_fwd_o`

Difference from older PR prose: current exact HEAD includes `apply_top_k_top_p_custom` in OPP sources and `npu_apply_top_k_top_p` in schemas, so the old fixed 7 OPP / 8 schema wording is stale for this run.

Because no wheel was produced, A2 residue could not be accepted or rejected at final wheel-inventory level. Source-level A2 residue risk was noted only as build evidence context, not as a completed Gate B wheel audit.

## Model Inventory

Model: `Qwen/Qwen3.6-35B-A3B`
dtype: BF16, non-quantized
Path: `/data/tiankuan/zyg/FL/workspace/Qwen3.6-35B-A3B`
State: `DOWNLOADING / NOT YET READY FOR STAGE 3`

Only inventory/presence state was recorded. No model load, shard completeness verification, TP2, HCCL model execution, generation, serving, graph, prefix, EP, 64K, benchmark, or profiling was run.

## Evidence Pointers

Evidence root:

```text
/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S1S2-ENV-BUILD-RUNTIME/evidence
```

Evidence manifest:

```text
/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S1S2-ENV-BUILD-RUNTIME/evidence/evidence_manifest.sha256
```

Key logs:

- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S1S2-ENV-BUILD-RUNTIME/evidence/20260825T184300+0800/host_identity.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S1S2-ENV-BUILD-RUNTIME/evidence/20260825T184300+0800/host_cann_driver.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S1S2-ENV-BUILD-RUNTIME/evidence/20260825T184300+0800/docker_manifest_a3_openeuler.json`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S1S2-ENV-BUILD-RUNTIME/evidence/20260825T190400+0800/container_gate_a.txt`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S1S2-ENV-BUILD-RUNTIME/evidence/20260825T191400+0800/build_wheel.log`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S1S2-ENV-BUILD-RUNTIME/evidence/20260825T192200+0800/catlass_retry.log`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S1S2-ENV-BUILD-RUNTIME/evidence/20260825T205100+0800/build_wheel.log`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S1S2-ENV-BUILD-RUNTIME/evidence/20260825T205100+0800/build_wheel.exit_code.observed`
- `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S1S2-ENV-BUILD-RUNTIME/evidence/20260825T211000+0800/container_cleanup.log`

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
path: docs/qwen36-35b-a3b-a3-flagos/results/RESULT-QWEN36-A3-S1S2-ENV-BUILD-RUNTIME-20260825T205424+0800.md
commit: the Control commit that first adds this immutable Result
```

Evidence pointer:

```text
/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S1S2-ENV-BUILD-RUNTIME/evidence
main build log: /data/tiankuan/zyg/FL/workspace/QWEN36-A3-S1S2-ENV-BUILD-RUNTIME/evidence/20260825T205100+0800/build_wheel.log
manifest: /data/tiankuan/zyg/FL/workspace/QWEN36-A3-S1S2-ENV-BUILD-RUNTIME/evidence/evidence_manifest.sha256
```
