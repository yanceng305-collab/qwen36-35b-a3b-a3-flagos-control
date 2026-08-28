# RESULT-QWEN36-A3-S6-TOKENIZER-NATIVE-UFFFD-AWARE-FUNCTIONAL-MATRIX-RERUN

Dispatch: User formal dispatch to execute only `QWEN36-A3-S6-TOKENIZER-NATIVE-UFFFD-AWARE-FUNCTIONAL-MATRIX-RERUN`, publish its immutable Result and Control sync, then STOP.

Run ID: `20260828T161700+0800`

## Status

- Lifecycle status: `failed`
- Experiment Result: `STOP`
- Control Sync: `SYNCED` by the Control commit that first adds this immutable Result
- Codex1 Acceptance: `PENDING`
- Gate F0: `STOP — DEVICE-SCOPE MAPPING DRIFT BEFORE READINESS`
- Gate F1 O8: `NOT RUN`
- Gate F2 O1024: `NOT RUN`
- Gate F3: `SHUTDOWN/EVIDENCE COMPLETE; FUNCTIONAL PROOFS NOT RUN`
- Stage 6 Execution PASS: `NO`
- Code PR: `N/A`

No workload request or matrix cell was scheduled. This Result does not modify implementation source, rebuild the wheel, modify the tokenizer, change sampling, run performance, run prefix lifecycle, run EP2, or enter any later Task or Stage.

## Identity

| Field | Value |
| --- | --- |
| Control repo | `yanceng305-collab/qwen36-35b-a3b-a3-flagos-control` |
| Live Control parent | `551333d776d56b5accbddc2677669234802318ca` |
| Task | `docs/qwen36-35b-a3b-a3-flagos/tasks/QWEN36-A3-S6-TOKENIZER-NATIVE-UFFFD-AWARE-FUNCTIONAL-MATRIX-RERUN.md` |
| D-034 | `APPROVED / USER DECISION — provenance-aware branch` |
| Frozen source | `xiemingda-1002/vllm-plugin-FL@e610a990d785356bf51a3cad50219d4c03310a31` |
| Frozen tree | `609ff1ad0f08239f353cb4d8774e504b4deba03b` |
| Frozen wheel | `vllm_plugin_fl-0.2.0+ge610a990d-cp311-cp311-linux_aarch64.whl` |
| Frozen wheel SHA-256 | `2fcf788660f3fe42b364bc60d593ee1b9b634fc0632de58c444d961bff4aa1bd` |
| Accepted image | `quay.io/ascend/vllm-ascend:v0.20.2rc1-a3-openeuler` |
| Manifest digest | `sha256:442363921166771eb82baeec9c1ac0381f46fb830ead8d0e072df6e925f2a958` |
| Arm64 platform digest | `sha256:a55b2b0597f9fdd1882de9bf3b7ebc395dd77c1ca49f251d0cb759d7b2c1a807` |
| Image ID | `sha256:8e7823878349b37d9900984555e28381c25681f88ff678cb4a86f7d1674a67c1` |
| Host | `bm-jn-zs-zone1-910C-64G-10-105` |
| Selected scope | physical NPU 7; host logical 14/15; both healthy and idle at admission |
| Container | `qw36-s6-native-20260828t161700p0800` / `0721dfd8bc0915a42f78bff4b585f24f0a35e4a8ef5e770913fffe4399db4fda` |
| Model | `/data/tiankuan/zyg/FL/workspace/Qwen3.6-35B-A3B`, BF16, non-quantized |
| Evidence root | `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S6-TOKENIZER-NATIVE-UFFFD-AWARE-FUNCTIONAL-MATRIX-RERUN/runs/20260828T161700p0800/evidence` |
| Main server log | same root, `runtime/server.log` |
| Checksums | same root, `checksums.txt` |

## Long-context execution state

The mandatory context-management skill was installed and loaded before A3 mutation from:

- repo `https://github.com/yanceng305-collab/long-context-orchestrator`
- resolved commit `0bb8a5eda9c46f1b170552ba41b871ba141e04b6`
- load path `/data/tiankuan/zyg/FL/workspace/stage6-native-ufffd-task/skill/SKILL.md`
- `SKILL.md` SHA-256 `f4e15f8c5f097d6aec0fe42b58e1cb5386d3104a02b5ebb502e8f7b0041af1f2`

Durable Task-owned `WORKPLAN.md` and `INDEX.md` were maintained under the run `work/` directory. The skill was used only for context/state management and did not expand the Task or alter any gate, retry, runtime, or STOP rule.

## User-authorized F0 mount correction

The first container admission attempts using `/data:/data` failed with overlay `ENOSPC` despite ample block storage. Evidence showed recursive propagation of the shared `/data` mount, including Docker's own `/data/docker` tree, exhausted the affected container mount namespace ceiling.

The User explicitly authorized narrowing this project's bind to:

`/data/tiankuan:/data/tiankuan`

The successful container inspect recorded this bind as `rprivate`. This was a filesystem-visibility-only correction: every required model, wheel, work, Evidence, artifact, and cache path is under `/data/tiankuan`. It did not change source, model, runtime configuration, workload, sampling, scheduling, graph, or acceptance semantics.

Evidence:

- `/data/tiankuan/docker-overlay-enospc-diagnosis-20260828.md`
- `runtime/f0_stop_storage.typescript`
- `environment/container_inspect.typescript`

## F0 checks completed before the blocker

The following bounded admission checks passed:

- exact Accepted image ID/platform and narrowed Task mount;
- exact Frozen wheel file/hash and wheel-origin `direct_url`;
- standalone FL import ownership from site-packages;
- `vllm-ascend` distribution absent and `vllm_ascend` not importable outside the image source directory;
- FlagGems absent and `USE_FLAGGEMS=0`;
- PlatformFL, WorkerFL, ModelRunnerFL and GraphWrapper owned by installed `vllm_fl`;
- `torch_npu` import, NPU availability, count 2 and both device identities `Ascend910_9382` in the staged scope probe;
- model root with 26/26 safetensors shards, 1045 BF16 tensors and no quantization config;
- Accepted jemalloc object `/usr/lib64/libjemalloc.so.2`, SHA-256 `2059f0cb5c2b3da8b834f4a54c12a633295eadb01844cef298398f350a2df43e`, reconstructed only inside the container at the Frozen preload path and proven by loader trace/maps;
- capture-only D-034 RequestOutput/SSE/raw-HTTP/client hooks loaded from the Task Evidence path and passed import/load smoke.

The service started with the Frozen model/runtime tuple and logged automatic capture sizes `[1,2,4,8,16,24,32,40,48,56,64]` in effective EngineCore configuration, but never reached readiness and no graph capture/workload claim is made.

## First blocker and immediate STOP

The selected and authorized host scope was physical NPU 7, logical devices 14/15. The service start script, following the historical container-visible convention, exported:

`ASCEND_VISIBLE_DEVICES=0,1`

`ASCEND_RT_VISIBLE_DEVICES=0,1`

In this fresh privileged/direct-device container, those values were not a container-local renumbering. Host `npu-smi` showed the exact Task-owned PIDs on physical NPU 0 logical 0/1:

- APIServer host PID `2384132`
- TP worker host PID `2416275` on logical 0
- TP worker host PID `2416278` on logical 1

This differed from the selected logical 14/15 scope and triggered the formal unsafe-device/Frozen-environment drift STOP rule before readiness.

- Last successful cell: none
- Completed cells in frozen order: none
- Current cell at STOP: none
- O8 requests: zero
- O1024 requests: zero
- U+FFFD requests: zero
- `TOKENIZER_NATIVE_UFFFD`: zero
- `POST_TOKENIZER_CORRUPTION`: false

Root-cause confidence: `HIGH` for the immediate device-scope mismatch and the `0,1` interpretation in this exact privileged/direct-device container. This Result does not generalize that behavior to all Ascend container-runtime configurations.

Minimum follow-up: a new Control/User-authorized execution must explicitly resolve and prove host logical 14/15 visibility before any service mutation, and must determine whether to retain host numbering or use a truly remapped non-privileged runtime. This Result does not authorize a retry or parameter correction within the stopped run.

Evidence:

- `runtime/server.log`
- `runtime/first_blocker_process_identity.typescript`
- `runtime/first_blocker_npu_mapping.typescript`
- `commands/start_stage6_serve.sh`

## Shutdown

Workload scheduling was never started. SIGTERM and SIGINT did not unwind the process tree while workers were in HCCL initialization, so exact Task-owned PIDs only were terminated with SIGKILL. Post-shutdown Evidence confirms:

- no Task APIServer, EngineCore, TP worker, or resource tracker remained;
- port 8016 was closed;
- all host NPU process tables were empty;
- no unrelated process, container, device, cache, or workload was killed, reset, or modified;
- the Task-owned container remains preserved and running with only its primary shell.

Evidence:

- `runtime/post_shutdown_process_scan.typescript`
- `runtime/post_shutdown_port_8016.typescript`
- `runtime/post_shutdown_npu_smi.typescript`

## Evidence manifest

- Manifest: `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S6-TOKENIZER-NATIVE-UFFFD-AWARE-FUNCTIONAL-MATRIX-RERUN/runs/20260828T161700p0800/evidence/manifest.md`
- Checksums: same root, `checksums.txt`, SHA-256 `848778b6c0c7d9ea44b6637d37704902133e777bd826c50b13a96e3508613dcf`
- Durable state: same run root, `work/WORKPLAN.md` and `work/INDEX.md`
- D-034 impact audit: same Evidence root, `runtime/instrument/IMPACT_AUDIT.md`

## Three pointers

- Code/source pointer: `https://github.com/xiemingda-1002/vllm-plugin-FL`, frozen source `e610a990d785356bf51a3cad50219d4c03310a31`, tree `609ff1ad0f08239f353cb4d8774e504b4deba03b`, wheel SHA-256 `2fcf788660f3fe42b364bc60d593ee1b9b634fc0632de58c444d961bff4aa1bd`, Code PR=`N/A`.
- Control pointer: exact rerun Task, this immutable Result, `docs/qwen36-35b-a3b-a3-flagos/results/INDEX.md`, and the Control commit that first adds them.
- Evidence pointer: `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S6-TOKENIZER-NATIVE-UFFFD-AWARE-FUNCTIONAL-MATRIX-RERUN/runs/20260828T161700p0800/evidence`, `manifest.md`, and `checksums.txt`.
