# Control Note — unsynced device/provenance follow-up execution

Status: **SERVER EVIDENCE EXISTS / CONTROL SYNC PENDING**

This is a Control note, not an immutable Result and not a Codex1 Formal Acceptance. Codex1 did not import or reconstruct the server Result because the execution environment had no `GITHUB_TOKEN` / `GIT_SYNC_TOKEN` and this Control-only session does not operate A3.

## User-confirmed local execution pointer

```text
/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S6-DEVICE-MAPPING-AND-PROVENANCE-CORRELATION-FOLLOWUP/results/RESULT-QWEN36-A3-S6-DEVICE-MAPPING-AND-PROVENANCE-CORRELATION-FOLLOWUP-20260829T111455+0800.md
```

Reported first blocker under the then-current Task contract:

```text
F0 proof-gap: direct PID -> host logical NPU placement could not be obtained
npu-smi info proc = unsupported
no useful device-FD placement evidence
```

Disposition: the STOP was contract-compliant at the time. The local Result remains server-local and unsynced; no repository immutable Result is claimed.

## User-authorized supplemental mapping pointer

```text
/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S6-DEVICE-MAPPING-AND-PROVENANCE-CORRELATION-FOLLOWUP/supplemental/20260829T113000+0800/evidence
```

Summary file: `SUPPLEMENTAL-REPORT.md`.

User-confirmed composite proof for that supplemental run:

```text
selected host scope: physical NPU 14/15; host logical 14/15
logical 13 excluded because Alarm
container torch index 0 -> /dev/davinci14 -> host physical/logical 14
container torch index 1 -> /dev/davinci15 -> host physical/logical 15
```

Evidence basis included host physical/logical inventory, device-node major/minor identity, container device configuration, `ASCEND_VISIBLE_DEVICES=14,15`, `ASCEND_RT_VISIBLE_DEVICES=14,15`, `torch_npu` availability, device count 2, and two `Ascend910_9382` devices.

This closes the practical mapping diagnosis for that supplemental run only. It does not modify the prior immutable STOP, does not prove all future runs remap identically, and is not formal Stage 6 matrix progress.

## Remaining supplemental blocker

The same supplemental continuation reported no O8 formal cell and no O1024. It left the remaining blocker as `UNRESOLVED_PROVENANCE`: bare request IDs, vLLM IDs and `cmpl-<request-id>-0` were not connected by a source-backed deterministic, reversible or uniquely attributable, collision-free transform. `TOKENIZER_NATIVE_UFFFD` and `POST_TOKENIZER_CORRUPTION` were not established. No model/FL/NPUGraph/NPU corruption claim is supported.

The exact supplemental Evidence root and all raw IDs must be preserved for the next Task. No fuzzy matching is authorized.
