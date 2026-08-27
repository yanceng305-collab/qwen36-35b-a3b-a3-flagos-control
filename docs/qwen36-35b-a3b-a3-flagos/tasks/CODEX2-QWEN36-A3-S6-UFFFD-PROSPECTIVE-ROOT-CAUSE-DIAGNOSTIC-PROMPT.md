# Codex2 Prompt — QWEN36-A3-S6-UFFFD-PROSPECTIVE-ROOT-CAUSE-DIAGNOSTIC

User formal dispatch：**execute only `QWEN36-A3-S6-UFFFD-PROSPECTIVE-ROOT-CAUSE-DIAGNOSTIC` in this new Codex2 session. Complete its immutable diagnostic Result and Control sync, then stop. Do not execute any other Task.**

```text
Control repo:
https://github.com/yanceng305-collab/qwen36-35b-a3b-a3-flagos-control

Implementation repo:
https://github.com/xiemingda-1002/vllm-plugin-FL

Historical reference branch (not an execution gate):
feature/qwen3.6-35b-a3b-ascend-graph-migration

Task ID:
QWEN36-A3-S6-UFFFD-PROSPECTIVE-ROOT-CAUSE-DIAGNOSTIC

Frozen source:
e610a990d785356bf51a3cad50219d4c03310a31

Frozen tree:
609ff1ad0f08239f353cb4d8774e504b4deba03b

Frozen wheel:
vllm_plugin_fl-0.2.0+ge610a990d-cp311-cp311-linux_aarch64.whl

Wheel SHA-256:
2fcf788660f3fe42b364bc60d593ee1b9b634fc0632de58c444d961bff4aa1bd

Required long-context skill repo:
https://github.com/yanceng305-collab/long-context-orchestrator

Required long-context skill commit:
0bb8a5eda9c46f1b170552ba41b871ba141e04b6
```

This is a new execution session. Do not reconstruct facts or authorization from prior chat or agent memory.

Before A3 work:

1. Live-query and sync latest Control `main`.
2. Read `AGENTS.md`, root `README.md`, `STATUS.md`, the exact current Task, its Formal Review and prior immutable diagnostic Result, `A3-RUNTIME-HANDOFF.md`, and `REPOSITORY-AND-EVIDENCE-RULES.md` in the Codex2 reading order.
3. Confirm the Task is still `READY / ONLY NEXT TASK`; otherwise STOP.
4. Install/load and use `long-context-orchestrator` from the exact pinned commit above in a Task-owned/authorized path. Record commit, load path and `SKILL.md` hash. Do not follow floating `main` or modify an existing installed skill.
5. Create durable `WORKPLAN.md` and `INDEX.md` in the project Task work/Evidence hierarchy. Preserve current phase, used/remaining attempt budget, instrumentation version, completed cells, U+FFFD/chain state and active STOP state across compaction.

The skill manages context only. The formal Task, this explicit User dispatch, PASS/STOP gates and Evidence contract take precedence. The skill and subagents cannot expand authorization; durable notes do not replace Server Evidence, immutable Result or Control sync.

Strictly execute the current Ready Task. The User authorizes safe A3 service startup, capture-only prospective runtime/client instrumentation and the Task's bounded O8 reproduction. Do not redesign or broaden the contract.

Hard global workload cap:

```text
service launches:           <= 3
target I1024/C64/O8 cells:  <= 4
same-service prehistory:    <= 2 sequences of C1 -> C8 -> C32 -> C64
total workload cells:       <= 10
total generation requests:  <= 338
all outputs:                exactly O8
instrumentation correction: <= 1 bundle correction total
```

Every generation request counts. No activation probe may sit outside the budget. Do not run O1024, a full matrix, other input lengths, performance, prefix lifecycle or EP2. Do not change sampling, concurrency, graph, prefix, chunked-prefill, async scheduling, source, wheel, image, runtime or model.

Capture and correlate by attempt-aware request ID:

```text
earliest generated token IDs / token representation
-> tokenizer decode boundary
-> serving response object
-> raw HTTP response chunks/bytes before parsing
-> parsed OpenAI JSON/events
-> benchmark client in-memory text
-> serialized/saved result
-> validator input/result
```

Prefer Task/Evidence-local monkey patches, wrappers and hooks. Do not edit or shadow production packages. Preserve every instrumentation file/hash/load path/patched symbol/behavior/impact/rollback and exact benchmark/validator source. Instrumentation must observe only and must not alter token, text, request, sampling, scheduling or error semantics.

Parent sampling boundary：`temperature=1`; dataset seed freezes prompts, not sampled tokens. Parent C64 followed same-service C1/C8/C32 with `enable_global_stream_random_sample=true`. New runs are faithful configuration/workload reproductions, never exact sampled-token replay. Non-reproduction remains D and cannot deny the parent blocker; reproduction still requires complete chain Evidence and is not automatically an FL/NPUGraph bug.

Immediately stop scheduling new workload when the first complete U+FFFD chain can locate the earliest changed layer, any budget is exhausted, an invariant/cell failure occurs, or capture would require forbidden changes. If U+FFFD appears with an incomplete chain, pause and use the one instrumentation correction only if still available; otherwise STOP D. Do not run further cells merely to collect more occurrences.

Return exactly A/B/C/D as frozen by the Task. If C, narrow to the actual evidenced layer. If sampled-token versus decoder/server behavior cannot be separated, return D. A/B/C is diagnostic only and cannot produce Stage 6 PASS.

Publish one immutable diagnostic Result, sync `results/INDEX.md`, leave Codex1 Acceptance `PENDING`, preserve the three pointers and full reconstruction Evidence, shut down only Task-owned resources, and stop. Do not create a next Task or resume Stage 6.
