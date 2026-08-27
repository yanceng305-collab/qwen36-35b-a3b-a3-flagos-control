# Codex1 Formal Review — Qwen3.6 A3 Prospective U+FFFD Diagnostic New-Server STOP

Review date：2026-08-27

Disposition：**FORMALLY REVIEWED — valid, preservable, auditable DIAGNOSTIC STOP D record；new-server staged NPU invariant PASS within exact scope；jemalloc runtime reconstruction gap confirmed；Codex1 NEEDS-FOLLOWUP；Stage 6 remains STOP / NOT ACCEPTED**

This Review accepts the new-server Result as a bounded execution record. It is not Stage 6 Acceptance, not a Stage 1/2 revalidation, and not a U+FFFD A/B/C root-cause conclusion.

## Live Control provenance

Codex1 live-queried GitHub before review and did not rely on the supplied SHA or chat history.

| Field | Live-reviewed identity |
| --- | --- |
| Control repo | `https://github.com/yanceng305-collab/qwen36-35b-a3b-a3-flagos-control` |
| Live `main` before review | `b6e38e686ccf0f9897e276e2735ea4ccfce98584` / tree `1fdd64e06b4e7d46256b1e1a6e20caf879b933ae` |
| Immutable Result commit | `58ff6f3c10aead1826b0c23756882a66c44473ae` / tree `e43dfa1a22434944e57dcbdbdd82671bfdc44294` |
| Result direct parent | `87412e6b02018295b42cbb62f3e78fa793846158` |
| Result blob | `286f292b5ae0de7bad7ae25901a84fd314382356` |
| Post-Result sync commit | `b6e38e686ccf0f9897e276e2735ea4ccfce98584` |
| Executed Task blob | `73c481eead07c89181665c1ab51f2070f527929c` |

The Result commit adds only the immutable Result and its INDEX row. The sync commit changes only that INDEX row. The Result blob remained unchanged.

Codex1 did not access or operate A3 and did not open or re-hash server-resident Evidence. Findings below are bounded to the immutable Control record and its stable Evidence pointers.

## Reviewed records

- New-server Result：[`RESULT-QWEN36-A3-S6-UFFFD-PROSPECTIVE-ROOT-CAUSE-DIAGNOSTIC-NEW-SERVER-20260827T173022+0800.md`](../results/RESULT-QWEN36-A3-S6-UFFFD-PROSPECTIVE-ROOT-CAUSE-DIAGNOSTIC-NEW-SERVER-20260827T173022+0800.md)
- Old-server Result：[`RESULT-QWEN36-A3-S6-UFFFD-PROSPECTIVE-ROOT-CAUSE-DIAGNOSTIC-20260827T113500+0800.md`](../results/RESULT-QWEN36-A3-S6-UFFFD-PROSPECTIVE-ROOT-CAUSE-DIAGNOSTIC-20260827T113500+0800.md)
- Executed Task：[`QWEN36-A3-S6-UFFFD-PROSPECTIVE-ROOT-CAUSE-DIAGNOSTIC`](../tasks/QWEN36-A3-S6-UFFFD-PROSPECTIVE-ROOT-CAUSE-DIAGNOSTIC.md)
- New-server Evidence root：`/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S6-UFFFD-PROSPECTIVE-ROOT-CAUSE-DIAGNOSTIC/runs/20260827T173022+0800/evidence`
- Main Evidence：`runtime/staged_npu_invariant.log`, `runtime/runtime_tuple.log`, `runtime/server_s1.log`, `runtime/server_chain_s1.jsonl`, `manifest.md`, `checksums.txt`

## New-server staged NPU invariant

The new-server Result records a complete staged invariant PASS on:

```text
host: bm-jn-zs-zone1-910C-64G-10-111
physical NPU: 0,1
logical NPU: 0,1
device names: Ascend910_9382, Ascend910_9382
python startup: PASS
import torch: PASS
import torch_npu: PASS
torch.npu.is_available(): True
torch.npu.device_count(): 2
```

Formal disposition：**PASS for the exact new-host/container/device staged NPU admission invariant.**

This proves that a fresh container from the exact Accepted image, using the Accepted composite runtime-access pattern, can establish a healthy two-device NPU runtime on this exact host/mapping.

It does not prove service readiness, EngineCore/APIServer or TP-worker health, jemalloc activation, generation, graph replay, chunked/async effective use, custom-op smoke, the complete Stage 1/2 scope, every host, or the old preserved container.

Stage 1/2 Acceptance remains unchanged and is neither reopened nor weakened.

## Old-server timeout claim boundary

The old-server `20260827T113500+0800` 20-second invariant timeout with exit `124` and no output remains valid historical Evidence for that exact run.

The new-server positive invariant neither explains nor disproves the old timeout and does not convert it to PASS. It does, however, disprove the broader claim that the timeout establishes a universal Frozen image/runtime regression.

Allowed claim：**old-server timeout is host/run-scoped historical Evidence and remains unresolved；it must not be generalized to the Frozen runtime family.**

## First blocker and STOP correctness

The first new-server service launch retained the frozen Stage 6 configuration:

```text
LD_PRELOAD=/usr/lib/aarch64-linux-gnu/libjemalloc.so.2
```

The immutable Result records that the fresh exact-image container exposed:

```text
/usr/lib64/libjemalloc.so.2
```

but lacked the frozen configured path. The dynamic loader repeatedly reported that the configured object could not be preloaded and was ignored.

This blocker occurred during the first service launch, before readiness, worker-health proof or generation. Actual budget use was:

```text
service launches: 1/3
C64 targets: 0/4
prehistory: 0/2
workload cells: 0/10
generation requests: 0/338
instrumentation correction: 0/1
```

STOP was required by the executed Task. Creating a symlink, restarting and continuing in that run would have exceeded its authority.

## Formal jemalloc classification

The blocker is formally classified as:

> **new-server clean reconstruction exposed an undocumented Stage 6 jemalloc preload-path prerequisite / runtime reconstruction documentation gap**

It is not evidence of:

- an incorrect image tag/digest;
- Frozen source/tree/wheel/model drift;
- production source or wheel defect;
- Stage 1/2 failure or required revalidation;
- Stage 6 model/output failure;
- any FL/NPUGraph or U+FFFD root cause.

The launch did have an effective service-runtime mismatch because the frozen preload string did not resolve. Do not claim whole-effective-runtime no drift for that launch.

Current Control does not prove that the historical Accepted container used the same symlink or correction procedure. Historical service success and this newly discovered clean-container prerequisite are distinct facts. The Result wording suggesting an “accepted runtime's symlink” is therefore not adopted as a formal historical claim.

The file type, realpath, package ownership, SHA-256, ELF architecture/dependencies and actual loader activation of `/usr/lib64/libjemalloc.so.2` remain mandatory follow-up Evidence, not facts independently re-derived by this Control-only review.

## Reconstruction gap decision

[`A3-STAGE1-2-ACCEPTED-RUNTIME.md`](../reconstruction/A3-STAGE1-2-ACCEPTED-RUNTIME.md) and [`A3-RUNTIME-HANDOFF.md`](../A3-RUNTIME-HANDOFF.md) documented the Accepted composite container-access pattern but omitted the Stage 6 jemalloc compatibility-path prerequisite and verification procedure.

This omission is now recorded as a later-discovered Stage 6 clean-container reconstruction prerequisite. It does not alter the Accepted image/runtime identity or claim that the historical Stage 1/2 container used the same preparation step.

## U+FFFD disposition

No generation occurred. There is no new sampled-token, tokenizer-decode, serving-object, raw HTTP, parsed JSON, client, serialized-result or validator chain.

- Classification：**D — UNRESOLVED**.
- Historical saved-result/validator blocker：preserved and not denied.
- Underlying U+FFFD root-cause confidence：**LOW / NOT CONFIRMED**.
- A/B/C：not supported or excluded by this run.

## Exactly one next Task

The executed prospective Task is ended and must not resume. The only Ready follow-up is:

[`QWEN36-A3-S6-JEMALLOC-RECONSTRUCTION-AND-UFFFD-ROOT-CAUSE-DIAGNOSTIC`](../tasks/QWEN36-A3-S6-JEMALLOC-RECONSTRUCTION-AND-UFFFD-ROOT-CAUSE-DIAGNOSTIC.md)

It combines one execution contract:

```text
R0 runtime reconstruction correction
-> R1 frozen service admission/readiness
-> R2 prospective U+FFFD diagnosis
```

There is no jemalloc-only intermediate Task. If R0/R1 pass, Codex2 must continue directly into R2 in the same Task rather than returning only a jemalloc success.

The new Task resets the full diagnostic workload budget because this new-server run generated zero requests:

```text
service launches <= 3
C64 targets <= 4
prehistory sequences <= 2
workload cells <= 10
generation requests <= 338
all output lengths = O8
```

Runtime filesystem reconstruction is separately authorized and does not consume the one output-chain instrumentation-bundle correction. A second reconstruction method or second instrumentation correction is not authorized.

## Current project disposition

- New-server Result：**FORMALLY REVIEWED / NEEDS-FOLLOWUP**.
- Old-server timeout：historical, run-scoped, not a general Frozen regression.
- New-server NPU invariant：**PASS at exact admission scope**.
- Jemalloc blocker：**runtime reconstruction gap**.
- Stage 6：**STOP / NOT ACCEPTED**.
- U+FFFD：**D / UNRESOLVED**.
- Performance/capacity, prefix lifecycle, EP2, O1024, full matrix and later stages：**LOCKED**.

This Review does not dispatch Codex2 and does not modify any immutable Result.
