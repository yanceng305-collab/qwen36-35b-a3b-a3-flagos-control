# Codex2 Prompt — QWEN36-A3-S6-JEMALLOC-RECONSTRUCTION-AND-UFFFD-ROOT-CAUSE-DIAGNOSTIC

User formal dispatch：**execute only `QWEN36-A3-S6-JEMALLOC-RECONSTRUCTION-AND-UFFFD-ROOT-CAUSE-DIAGNOSTIC` in this new Codex2 session. Complete its immutable Result and Control sync, then stop. Do not execute any other Task.**

```text
Control repo:
https://github.com/yanceng305-collab/qwen36-35b-a3b-a3-flagos-control

Implementation repo:
https://github.com/xiemingda-1002/vllm-plugin-FL

Historical reference branch (not an execution gate):
feature/qwen3.6-35b-a3b-ascend-graph-migration

Task ID:
QWEN36-A3-S6-JEMALLOC-RECONSTRUCTION-AND-UFFFD-ROOT-CAUSE-DIAGNOSTIC

Frozen source:
e610a990d785356bf51a3cad50219d4c03310a31

Frozen tree:
609ff1ad0f08239f353cb4d8774e504b4deba03b

Frozen wheel:
vllm_plugin_fl-0.2.0+ge610a990d-cp311-cp311-linux_aarch64.whl

Wheel SHA-256:
2fcf788660f3fe42b364bc60d593ee1b9b634fc0632de58c444d961bff4aa1bd

Required context skill:
https://github.com/yanceng305-collab/long-context-orchestrator@0bb8a5eda9c46f1b170552ba41b871ba141e04b6
```

This is a new execution session. Do not reconstruct facts or authorization from prior chat or agent memory.

Before A3 work:

1. Live-query and sync latest Control `main`.
2. Read `AGENTS.md`, root `README.md`, `STATUS.md`, the exact current Task, its Formal Review and parent Results, `reconstruction/A3-STAGE1-2-ACCEPTED-RUNTIME.md`, `A3-RUNTIME-HANDOFF.md`, and `REPOSITORY-AND-EVIDENCE-RULES.md` in Codex2 order.
3. Confirm this Task remains `READY / ONLY NEXT TASK`; otherwise STOP.
4. Install/load the exact pinned `long-context-orchestrator` commit in a Task-owned/authorized path and record commit/path/`SKILL.md` hash. Maintain Task `WORKPLAN.md` and `INDEX.md` in the project work/Evidence hierarchy.

The skill manages context only. This formal Task, explicit User dispatch, STOP rules and Evidence contract take precedence. Skill/subagents cannot expand authority, and durable notes do not replace formal Evidence or immutable Result.

Strictly execute the current Task's three dependent phases:

```text
R0 Task-owned-container jemalloc reconstruction
-> R1 frozen service admission/readiness
-> R2 prospective U+FFFD output-chain diagnosis
```

If R0/R1 pass, continue directly into R2. Do not stop merely to report jemalloc success.

R0 keeps the frozen configuration unchanged:

```text
LD_PRELOAD=/usr/lib/aarch64-linux-gnu/libjemalloc.so.2
```

In one fresh Task-owned container, first verify `/usr/lib64/libjemalloc.so.2`: file type, realpath, ownership if available, SHA-256, AArch64 ELF/dependency/loader compatibility. If and only if the frozen target is absent and the source object is compatible, create the minimum container-local compatibility directory/symlink to the verified object. Do not change `LD_PRELOAD`, host, image, source, wheel, model or publish a derived image.

Before service launch, require target existence/realpath match, non-generative loader activation with no ignored-preload/ABI error, then rerun the staged two-device NPU invariant with the frozen preload effective. If this cannot be achieved with one verified filesystem method, STOP D.

R1 launches the exact frozen Stage 6 BF16 DP1/TP2 service with prefix align, chunked prefill, async scheduling, `FULL_DECODE_ONLY`, automatic capture through 64, `USE_FLAGGEMS=0` and all original env/additional config. Prove the configured path resolves, jemalloc is actually loaded in relevant processes, readiness succeeds and both TP workers are healthy. A real service/runtime failure is STOP; do not workaround.

R2 captures the request-linked chain:

```text
generated token IDs / token representation
-> tokenizer decode boundary
-> serving response object
-> raw HTTP chunks/bytes before parsing
-> parsed JSON/events
-> benchmark client in-memory text
-> serialized result
-> validator input/result
```

Runtime/client instrumentation is capture-only and Task/Evidence-local. It must not alter tokens, text, request order, sampling, graph, prefix, chunked/async or error semantics.

Keep the full newly frozen budget because the parent new-server run generated zero requests:

```text
service launches <= 3
C64 targets <= 4
prehistory sequences <= 2
workload cells <= 10
generation requests <= 338
all outputs = O8
runtime filesystem reconstruction = 1 verified method
output-chain instrumentation correction <= 1, separately accounted
```

Workload remains `I1024/O8`, `temperature=1`, `top_p=1`, `top_k=0`, `ignore_eos=true`, `enable_global_stream_random_sample=true`. S1 permits at most two C64 target cells. If needed, S2/S3 permit at most two same-service `C1→C8→C32→C64` sequences. These are faithful configuration/workload reproductions, not exact sampled-token replay.

Immediately stop scheduling new workload when the first complete U+FFFD chain locates the earliest changed layer, an invariant/service failure occurs, a budget is exhausted, or another reconstruction/instrumentation correction, production change or broader workaround would be required. Do not continue to collect more samples.

Return exactly A tokenizer/decoder-native, B benchmark/client/reconstruction, C service/model/runtime narrowed to the evidenced layer, or D unresolved. Never claim FL/NPUGraph bug without layer Evidence. Diagnostic A/B/C is not Stage 6 PASS.

Publish one immutable Result, sync `results/INDEX.md`, leave Codex1 Acceptance `PENDING`, preserve full reconstruction/output-chain Evidence and three pointers, shut down only Task-owned resources, and stop. Do not create a next Task or resume formal Stage 6.
