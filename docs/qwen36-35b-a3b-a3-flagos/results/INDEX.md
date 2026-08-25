# Execution Results Index

Codex2生成的 run snapshot首次 push后不可修改。Codex1 Acceptance只更新本 INDEX或 `../STATUS.md`，不修改 immutable Result。

当前尚无 A3 execution Result。

| Task | Run | Experiment Result | Code/source pointer | Immutable Result | Server Evidence | Control Sync | Codex1 Acceptance |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `QWEN36-A3-S1S2-ENV-BUILD-RUNTIME` | N/A | **Waiting User input / Not Ready** | Tracked branch；run SHA/tree在 dispatch时冻结 | N/A | N/A | Task created | PENDING / no execution |

## Status semantics

- Experiment Result：服务器执行的 `PASS / STOP / PARTIAL`。
- Control Sync：`SYNCED / PENDING`；同步失败不改变 execution事实。
- Codex1 Acceptance：`PENDING / ACCEPTED / REJECTED / NEEDS-FOLLOWUP`。
- `Waiting User input / Not Ready`不是 run，也不产生 immutable Result或 Acceptance。

每个 future row必须保留 Code/source、Control、Server Evidence三指针，并明确 exact source SHA/tree/clean state、environment和 claim boundary。
