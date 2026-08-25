# Execution Results Index

Codex2生成的 run snapshot首次 push后不可修改。Codex1 Acceptance只更新本 INDEX或 `../STATUS.md`，不修改 immutable Result。

当前已有 1 个 A3 execution STOP Result；尚无 A3 Execution PASS或 Codex1 Acceptance。

| Task | Run | Experiment Result | Code/source pointer | Immutable Result | Server Evidence | Control Sync | Codex1 Acceptance |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `QWEN36-A3-S1S2-ENV-BUILD-RUNTIME` | `2026-08-25T20:54:24+08:00` | **STOP / Gate A PASS; Gate B STOP; Gate C/D NOT RUN** | `xiemingda-1002/vllm-plugin-FL@7beda84f59d7b25f49cdf03bdf6efecd771067ed` / tree `a81eea55c1de548a0a1f182f51089eca0b088c82`; Code PR=`N/A` | [RESULT-QWEN36-A3-S1S2-ENV-BUILD-RUNTIME-20260825T205424+0800.md](RESULT-QWEN36-A3-S1S2-ENV-BUILD-RUNTIME-20260825T205424+0800.md) | `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S1S2-ENV-BUILD-RUNTIME/evidence`; main log `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S1S2-ENV-BUILD-RUNTIME/evidence/20260825T205100+0800/build_wheel.log` | SYNCED in the commit that adds this row | PENDING |

## Status semantics

- Experiment Result：服务器执行的 `PASS / STOP / PARTIAL`。
- Control Sync：`SYNCED / PENDING`；同步失败不改变 execution事实。
- Codex1 Acceptance：`PENDING / ACCEPTED / REJECTED / NEEDS-FOLLOWUP`。
- `READY / Awaiting explicit User dispatch`不是 run，也不产生 immutable Result或 Acceptance。

每个 future row必须保留 Code/source、Control、Server Evidence三指针，并明确 exact source SHA/tree/clean state、environment和 claim boundary。
