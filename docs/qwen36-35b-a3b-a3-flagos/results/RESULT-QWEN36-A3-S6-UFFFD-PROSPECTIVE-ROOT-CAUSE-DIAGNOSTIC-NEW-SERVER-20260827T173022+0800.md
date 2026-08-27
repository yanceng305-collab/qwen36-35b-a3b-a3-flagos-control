# RESULT-QWEN36-A3-S6-UFFFD-PROSPECTIVE-ROOT-CAUSE-DIAGNOSTIC — NEW SERVER

This document reproduces the frozen validation baseline:
`e610a990d785356bf51a3cad50219d4c03310a31`
tree: `609ff1ad0f08239f353cb4d8774e504b4deba03b`
wheel SHA-256: `2fcf788660f3fe42b364bc60d593ee1b9b634fc0632de58c444d961bff4aa1bd`

- Task/run: `QWEN36-A3-S6-UFFFD-PROSPECTIVE-ROOT-CAUSE-DIAGNOSTIC` / `20260827T173022+0800`
- Execution: **DIAGNOSTIC STOP — D / UNRESOLVED**
- Codex1 Acceptance: **PENDING**
- Code PR: `N/A`

## New-server scope

This was a fresh run on new server `bm-jn-zs-zone1-910C-64G-10-111`. The old-server `20260827T113500+0800` invariant timeout was historical Evidence only and was not inherited. The new staged NPU invariant passed. This run stopped later, during the first service launch and before service readiness or generation, because the frozen jemalloc preload was not active.

## Admission and reconstruction identity

- Live Control `main`: `87412e6b02018295b42cbb62f3e78fa793846158`; no newer superseding decision; this Task remained `READY / ONLY NEXT TASK` under the explicit new-server dispatch.
- Pinned skill: `long-context-orchestrator@0bb8a5eda9c46f1b170552ba41b871ba141e04b6`; run-local load path under `work/skill`; `SKILL.md` SHA-256 `f4e15f8c5f097d6aec0fe42b58e1cb5386d3104a02b5ebb502e8f7b0041af1f2`.
- Host/driver: AArch64; driver and `npu-smi` `25.5.0`; selected physical NPU 0/1 after repeated healthy/process-free snapshots; physical 13 was excluded because it reported `Alarm`.
- Mapping: physical 0 -> logical 0; physical 1 -> logical 1; both names `Ascend910_9382`.
- Image: exact `quay.io/ascend/vllm-ascend:v0.20.2rc1-a3-openeuler`; manifest `sha256:442363921166771eb82baeec9c1ac0381f46fb830ead8d0e072df6e925f2a958`; arm64 platform `sha256:a55b2b0597f9fdd1882de9bf3b7ebc395dd77c1ca49f251d0cb759d7b2c1a807`; image ID `sha256:8e7823878349b37d9900984555e28381c25681f88ff678cb4a86f7d1674a67c1`.
- Fresh container: `qw36-ufffd-20260827t173022p0800` / `59f215a7eafa0b477df45229ed1c9fb110cf50c9180f9a97c4e5e16a2b4c5d9a`; accepted privileged/host-network/device/driver mount pattern; removed after STOP.
- Runtime tuple: Python `3.11.15`; torch `2.10.0+cpu`; torch-npu `2.10.0`; vLLM `0.20.2+empty`; Transformers `5.5.3`; triton-ascend `3.2.1`; `USE_FLAGGEMS=0`; FlagGems absent; runtime `vllm-ascend` uninstalled/absent.
- Frozen wheel: recovered from preserved Stage 3 artifacts, exact filename/hash; installed standalone at formal site-packages; `vllm_fl` resolved from site-packages; no FL source `PYTHONPATH` or editable FL install.
- Model: `/data/tiankuan/zyg/FL/workspace/Qwen3.6-35B-A3B`; accepted architecture/config; 26/26 indexed shards; 1045/1045 tensor headers BF16; no quantization; empty temporary directory; no incomplete file content.

## Runtime invariant

The flush-visible staged probe completed in about 5.7 seconds:

```text
python startup -> PASS
import torch -> PASS
import torch_npu -> PASS
torch.npu.is_available() -> True
torch.npu.device_count() -> 2
get_device_name(0) -> Ascend910_9382
get_device_name(1) -> Ascend910_9382
```

The old server's timeout is therefore excluded as a runtime fact for this host. A tool-yield ambiguity caused a duplicate bounded non-generative invariant process; both exited before service work. The saved first timeline is the formal invariant record and no workload budget was consumed.

## Instrumentation and service STOP

Instrumentation v1 was Task-local and capture-only. It wrapped `vllm.outputs.RequestOutput.__init__/add` and `OpenAIServingCompletion.completion_stream_generator` to copy token IDs, decoded text and serialized SSE events. The Task-local client reused vLLM `RandomDataset` and was prepared to capture raw chunks, parsed events, accumulator, serialization and validation. Activation and hashes were preserved. No production source/package file was edited, no token/text/request/scheduler behavior was changed, and correction budget remained unused.

The first frozen Stage 6 service launch parsed the required model, TP2, BF16, length/capacity, prefix align, chunked-prefill, async, graph and additional configuration. Its loader then reported repeatedly:

```text
ERROR: ld.so: object '/usr/lib/aarch64-linux-gnu/libjemalloc.so.2' from LD_PRELOAD cannot be preloaded (cannot open shared object file): ignored.
```

The fresh image contained `/usr/lib64/libjemalloc.so.2` but lacked the accepted runtime's `/usr/lib/aarch64-linux-gnu/libjemalloc.so.2` symlink. Thus the configured `LD_PRELOAD` string was present but its semantic effect drifted. Under the immediate STOP rules, this real non-U+FFFD service-configuration blocker prohibited a post-launch symlink correction, restart, strategy substitution or generation workload.

## Budget and classification

- Service launches: `1/3`.
- Target C64 cells: `0/4`.
- Prehistory sequences: `0/2`.
- Workload cells: `0/10`.
- Generation requests: `0/338`.
- Instrumentation correction: `0/1`.
- U+FFFD reproduction: not attempted/observed in this run.
- Output chain: no generated chain.

Classification is **D — unresolved**. There is no newly evidenced U+FFFD layer. The historical saved-result/validator blocker remains valid and none of tokenizer-native, client reconstruction, serving decode/object, model/runtime or graph/state hypotheses is confirmed or excluded by generation Evidence here.

- Earliest evidenced changed layer: `N/A` for U+FFFD; no generation. Earliest launch drift was dynamic-loader preload resolution.
- Root-cause confidence: **HIGH** for the service-launch configuration drift; **LOW / UNRESOLVED** for historical U+FFFD root cause.
- Last successful diagnostic step: new-server staged two-device NPU invariant PASS, followed by service parsing of the frozen configuration.
- First blocker / STOP trigger: frozen jemalloc preload target absent and ignored during launch.

## Pointers and cleanup

- Code/source: `xiemingda-1002/vllm-plugin-FL@e610a990d785356bf51a3cad50219d4c03310a31`; tree `609ff1ad0f08239f353cb4d8774e504b4deba03b`; Code PR `N/A`.
- Control: this immutable Result and `results/INDEX.md`.
- Server Evidence: `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S6-UFFFD-PROSPECTIVE-ROOT-CAUSE-DIAGNOSTIC/runs/20260827T173022+0800/evidence`; main logs `runtime/staged_npu_invariant.log`, `runtime/runtime_tuple.log`, `runtime/server_s1.log`, `runtime/server_chain_s1.jsonl`; manifest `manifest.md`; checksums `checksums.txt`.

Only the Task-owned service/container was stopped and removed. Physical NPU 0/1 were process-free after cleanup. This diagnostic does not produce Stage 6 PASS, does not create a next Task, and does not authorize a retry.
