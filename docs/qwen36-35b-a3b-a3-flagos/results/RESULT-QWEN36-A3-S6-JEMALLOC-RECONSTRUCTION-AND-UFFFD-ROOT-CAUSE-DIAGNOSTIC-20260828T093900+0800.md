# RESULT-QWEN36-A3-S6-JEMALLOC-RECONSTRUCTION-AND-UFFFD-ROOT-CAUSE-DIAGNOSTIC

This document reproduces the frozen validation baseline:
`e610a990d785356bf51a3cad50219d4c03310a31`
tree: `609ff1ad0f08239f353cb4d8774e504b4deba03b`
wheel SHA-256: `2fcf788660f3fe42b364bc60d593ee1b9b634fc0632de58c444d961bff4aa1bd`

- Task/run: `QWEN36-A3-S6-JEMALLOC-RECONSTRUCTION-AND-UFFFD-ROOT-CAUSE-DIAGNOSTIC` / `20260828T093900+0800`
- Lifecycle: `completed`
- Experiment Result: **DIAGNOSTIC STOP — A / tokenizer-decoder-native**
- Stage 6: **STOP / NOT ACCEPTED**
- Control Sync: **SYNCED by the Control commit that first adds this immutable Result**
- Codex1 Acceptance: **PENDING**
- Code PR: `N/A`

## Dispatch and admission

The User formally dispatched only this Task and required immutable Result/Control sync followed by STOP. GitHub live `main` was queried before execution and remained `3efdf6014ee89e265fd7940bed8645587bb118a7`, tree `d8059c2d304a4360fdb1442f9aebdb4a31085958`; the exact Task was still `READY / Awaiting explicit User dispatch — ONLY NEXT TASK`.

The pinned context skill was installed from `yanceng305-collab/long-context-orchestrator@0bb8a5eda9c46f1b170552ba41b871ba141e04b6` into a Task-owned `work/skill` path. `SKILL.md` SHA-256 was `f4e15f8c5f097d6aec0fe42b58e1cb5386d3104a02b5ebb502e8f7b0041af1f2`. Task `WORKPLAN.md` and `INDEX.md` were maintained outside Control under the Task work hierarchy.

## Exact execution identity

- Host: `bm-jn-zs-zone1-910C-64G-10-111`, AArch64, driver/npu-smi `25.5.0`.
- Devices: physical/logical 0/1, both `Ascend910_9382`; healthy and process-free at admission.
- Image: `quay.io/ascend/vllm-ascend:v0.20.2rc1-a3-openeuler`; manifest `sha256:442363921166771eb82baeec9c1ac0381f46fb830ead8d0e072df6e925f2a958`; arm64 platform `sha256:a55b2b0597f9fdd1882de9bf3b7ebc395dd77c1ca49f251d0cb759d7b2c1a807`; image ID `sha256:8e7823878349b37d9900984555e28381c25681f88ff678cb4a86f7d1674a67c1`.
- Task container: `qw36-jemalloc-20260828t093900p0800` / `ed9253865319e0de4244f6a311143decd7e72305d8b9ada771a1607e5097e922`; exact Accepted composite runtime-access pattern; removed after STOP.
- Runtime: Python `3.11.15`; torch `2.10.0+cpu`; torch-npu `2.10.0`; vLLM `0.20.2`; Transformers `5.5.3`; triton-ascend distribution `3.2.1`; `USE_FLAGGEMS=0`; FlagGems absent; `vllm-ascend` distribution/module absent.
- Frozen wheel: exact filename/path and SHA-256 above, installed into site-packages; direct-url metadata matched the artifact; `vllm_fl` resolved from site-packages.
- Model: `/data/tiankuan/zyg/FL/workspace/Qwen3.6-35B-A3B`; architecture accepted; 26/26 indexed shards; 1045/1045 tensor headers BF16; no quantization.

## R0 — jemalloc reconstruction PASS

Before correction, `/usr/lib/aarch64-linux-gnu/libjemalloc.so.2` and its parent directory were absent. `/usr/lib64/libjemalloc.so.2` was a root-owned mode-0755 regular file owned by package `jemalloc-5.3.0-2.oe2403sp2.aarch64`, SHA-256:

```text
2059f0cb5c2b3da8b834f4a54c12a633295eadb01844cef298398f350a2df43e
```

`file`, `readelf`, `ldd`, and loader-trace Evidence proved an ELF64 little-endian AArch64 shared object with resolved dependencies and compatible loader initialization.

The single authorized filesystem method created the minimum container-local path:

```text
mkdir -p /usr/lib/aarch64-linux-gnu
ln -s /usr/lib64/libjemalloc.so.2 /usr/lib/aarch64-linux-gnu/libjemalloc.so.2
```

The frozen configuration remained unchanged:

```text
LD_PRELOAD=/usr/lib/aarch64-linux-gnu/libjemalloc.so.2
```

After correction, source and target realpaths and SHA-256 matched. A non-generative frozen-path loader trace explicitly initialized the target and contained no ignored-preload/ABI error. With the frozen preload effective, Python startup, torch import, torch_npu import, `is_available()==True`, device count 2 and both device-name checks passed; `/proc/self/maps` positively showed jemalloc. The correction changed only the Task container compatibility directory/symlink. Container removal was its rollback.

## R1 — service admission PASS

The first service launch used the exact frozen Stage 6 contract: BF16 non-quantized DP1/TP2 mp, model length 66560, 64 sequences, 16384 batched tokens, memory utilization 0.90, prefix caching align, chunked prefill, async scheduling, `FULL_DECODE_ONLY`, automatic capture through 64, frozen additional configuration and `USE_FLAGGEMS=0`.

The resolved compilation configuration recorded capture sizes:

```text
[1,2,4,8,16,24,32,40,48,56,64]
```

Both TP workers loaded all 26 shards and completed graph capture. `/health` and `/v1/models` passed. Container PID roles were API 1166, EngineCore 1236, TP0 1276 and TP1 1277; all four mapped `/usr/lib64/libjemalloc.so.2`. The frozen target still resolved correctly and the loader-error scan passed.

A usage-reporting background thread logged a nonfatal `cpuinfo` JSON decode exception during loading. The engine, both workers, capture, readiness, workload and clean shutdown all completed, so it was not a service/runtime STOP.

## R2 — complete output chain and A classification

Initial instrumentation was Task/Evidence-local and capture-only. It recorded generated token IDs/representations, native decoder input/output, serving `RequestOutput`, response objects and serialized SSE, raw HTTP chunks, parsed JSON, benchmark memory, serialized result and validator input/result. All patches activated; there was no correction (`0/1`). It did not alter token selection, decoder return, text, request order, sampling, graph, prefix, chunked prefill, async scheduling, parsing or error semantics.

The first S1 `I1024/C64/O8` target used seed 1088, `temperature=1`, `top_p=1`, `top_k=0`, `ignore_eos=true` and global stream random sampling. All 64 requests returned HTTP 200, parsed without error and returned exactly eight token IDs. Two requests contained U+FFFD:

```text
ufffd-s1-c64a-034: 28 U+FFFD
ufffd-s1-c64a-035:  1 U+FFFD
```

The concise classifying chain is request 035:

```text
generated IDs:
[113814,100538,105211,99329,109,68,145113,95841]

token 109 representation "±" -> native decode "" (no U+FFFD)
token 68 representation "e" -> native decode "�e" (first U+FFFD)
-> serving object "�e"
-> serialized SSE / raw HTTP literal ef bf bd / parsed JSON "�e"
-> benchmark memory "本学期尤其独自扮�e英语四级出"
-> serialized result, identical
-> validator input, identical; U+FFFD count 1; FAIL
```

The exact tokenizer independently re-decoded the same eight IDs to the identical text and code points. Every continuity check passed: generated IDs matched serving and client IDs; concatenated decoder text matched the serving object, parsed wire events, benchmark memory, saved result and validator input. Request 034 independently showed the same boundary: its first token ID 11802 / representation `ÑĪ` natively decoded to a 2,805-code-point string containing 28 U+FFFD, then propagated unchanged.

Therefore the earliest evidenced changed layer is the **native tokenizer decode boundary**, and the required classification is:

> **A — tokenizer/decoder-native**

This excludes serving-object construction, JSON serialization, wire transport, client parsing/accumulation, save/reload and validator as the first changed layer. It does not label FL, NPUGraph, the model runtime or client as corrupt, and it does not decide whether tokenizer policy should suppress/repair such byte-token sequences.

## STOP, budget, and cleanup

The first complete classifiable U+FFFD chains triggered immediate STOP. No second target or prehistory sequence was scheduled.

```text
service launches:                  1 / 3
C64 targets:                       1 / 4
prehistory sequences:              0 / 2
workload cells:                    1 / 10
generation requests:              64 / 338
output length:                     all exactly O8
runtime filesystem reconstruction: 1 / 1 method
instrumentation corrections:       0 / 1
```

Last successful step: complete request-linked chain capture and exact-tokenizer independent redecode. First blocker/STOP trigger: U+FFFD first appeared at the native decoder boundary and propagated unchanged through validator failure.

The APIServer received SIGTERM; EngineCore and both TP workers reported clean shutdown. Only the Task-owned container was stopped and removed. Port 8016 was free and NPU 0/1 were process-free after cleanup.

## Evidence and three pointers

- Code/source: `https://github.com/xiemingda-1002/vllm-plugin-FL`, frozen source `e610a990d785356bf51a3cad50219d4c03310a31`, tree `609ff1ad0f08239f353cb4d8774e504b4deba03b`, wheel SHA-256 `2fcf788660f3fe42b364bc60d593ee1b9b634fc0632de58c444d961bff4aa1bd`; Code PR `N/A`.
- Control: dispatched Task, this immutable Result, and `results/INDEX.md` in the commit that first publishes them.
- Server Evidence: `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S6-JEMALLOC-RECONSTRUCTION-AND-UFFFD-ROOT-CAUSE-DIAGNOSTIC/runs/20260828T093900+0800/evidence`; main entries `manifest.md`, `checksums.txt`, `runtime/r0_reconstruct_and_invariant.typescript`, `runtime/r1_admission_proof.typescript`, `runtime/server_s1.typescript`, `runtime/server_chain_s1.jsonl`, `workload/s1_c64a/`, and `analysis/request_linked_chain.md`/`.json`.

Codex1 decision requested: independently review the A classification and its exact claim boundary. This Result leaves Acceptance `PENDING`, does not make Stage 6 PASS, does not unlock later stages, does not create a next Task and does not resume formal Stage 6.
