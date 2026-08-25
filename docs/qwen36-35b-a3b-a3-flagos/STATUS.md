# 项目状态

更新时间：2026-08-25

总体状态：Stage 0 **COMPLETE**；`QWEN36-A3-S1S2-ENV-BUILD-RUNTIME` **STOP at Gate B**，Codex1 Formal Review为 **NEEDS-FOLLOWUP**；Stage gate未推进，Stage 3 **LOCKED**。

## 当前快照

| Work item | Status | Evidence boundary |
| --- | --- | --- |
| Control repo | Established | 本仓库 `main`；首次 Control commit |
| Tracked implementation | Current GitHub snapshot reverified | `feature/qwen3.6-35b-a3b-ascend-graph-migration@7beda84...` / tree `a81eea...`；dispatch前重查 |
| Official base | Current GitHub snapshot recorded | `release/0.2@53adefb...` / tree `9ddfd0...` |
| PR #404 | OPEN / DRAFT / MERGEABLE / BLOCKED / REVIEW_REQUIRED | GitHub snapshot 2026-08-25 17:42 CST；状态会变化 |
| A2 implementation evidence | **A2 REFERENCE ONLY** | User资料中的 2×910B1结果，不是 A3 Acceptance |
| A3 environment/build/runtime | **PARTIAL / REVIEWED** | Gate A core ACCEPT WITH EVIDENCE GAP；Gate B STOP ACCEPTED；Gate C/D NOT RUN；无 wheel |
| A3 model/graph/serve/function/performance | **UNVERIFIED** | 没有 A3 execution Evidence |
| Official A3 base route | Bounded selection authorized | `v0.20.2rc1-a3` Ubuntu或`v0.20.2rc1-a3-openeuler`；ordinary unsuffixed A2 image excluded |
| Model artifact | `DOWNLOADING / NOT YET READY FOR STAGE 3` | `/data/tiankuan/zyg/FL/workspace/Qwen3.6-35B-A3B`；不阻塞Stage 1/2 |
| First Codex2 task | **STOP / Codex1 Review NEEDS-FOLLOWUP** | [Immutable Result](results/RESULT-QWEN36-A3-S1S2-ENV-BUILD-RUNTIME-20260825T205424+0800.md)；[Formal Review](reviews/REVIEW-QWEN36-A3-S1S2-ENV-BUILD-RUNTIME-20260825.md) |
| Next bounded Task | **READY / Awaiting explicit User dispatch** | [`QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG`](tasks/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG.md)；先确认 root cause；非源码闭合B后同container继续C/D |
| Validation Code repo/fork | **Not needed yet** | 已有 A3 execution blocker，但尚未证明 attributable to implementation source或需要 source change |
| GLM project | PAUSED by User Decision | 独立 Control；旧 Evidence/history保留，不写入本仓库 |

## 当前 implementation identity

核验时间：2026-08-25 22:14 CST；PR/head/base相对本次 execution identity未变化。

- Tracked repo/branch：`xiemingda-1002/vllm-plugin-FL` / `feature/qwen3.6-35b-a3b-ascend-graph-migration`。
- Current head：`7beda84f59d7b25f49cdf03bdf6efecd771067ed`。
- Current tree：`a81eea55c1de548a0a1f182f51089eca0b088c82`。
- Official base/ref：`flagos-ai/vllm-plugin-FL:release/0.2`。
- Current release HEAD/tree：`53adefb269571684d83a51e997d3ba9be5f88235` / `9ddfd080953ad39b39772e108ff921d2973b0299`。
- PR compare：ahead 6 / behind 0；base SHA与当前 release HEAD一致。
- Branch movement：PR timeline显示此前 head `f9281f...` 被 force-push/rebase 为 `7beda84...`。User资料没有冻结可比较的旧 full SHA；不能声称“相对资料某 exact SHA”的普通 fast-forward。

以上只是 Control创建时的 moving GitHub snapshot。正式 Task在 User dispatch 前必须重新查询、冻结当次 exact HEAD/tree；若已变化，先做 diff review。

## 当前 Stage — Stage 1/2 execution result

Task：[tasks/QWEN36-A3-S1S2-ENV-BUILD-RUNTIME.md](tasks/QWEN36-A3-S1S2-ENV-BUILD-RUNTIME.md)

目标是一次有边界的 Stage 1/2 gate：

```text
A3 physical/environment identity
  -> current tracked source exact identity
  -> clean A3-family wheel build
  -> formal standalone FL install
  -> A3 _C_ascend / OPP identity
  -> minimal A3 NPU custom-op/runtime smoke
```

首任务未运行完整 Qwen模型、graph、prefix、EP、64K 或 benchmark。Stage 1/2本次未达到 `Execution PASS`；Codex1 Formal Review为 `NEEDS-FOLLOWUP`，Stage 3未解锁。

本次 run 事实：

- Gate A：`PASS`。
- Gate A Codex1 Review：**ACCEPT WITH EVIDENCE GAP**；core device/image/environment/source identity接受，Triton/provider子项待补证。
- Gate B：`STOP`。
- Gate B Codex1 Review：**STOP ACCEPTED**；不等于 Gate B PASS。
- First blocker：`OpFileNotExistsError: File aic-*-ops-info.ini does not exist`。
- Source：`7beda84f59d7b25f49cdf03bdf6efecd771067ed` / tree `a81eea55c1de548a0a1f182f51089eca0b088c82`。
- Official PR base correction：`flagos-ai/vllm-plugin-FL:release/0.2@53adefb269571684d83a51e997d3ba9be5f88235` / tree `9ddfd080953ad39b39772e108ff921d2973b0299`；immutable Result中的 fork `main@38e7dbc...`不是 official PR compare base，但不影响 execution HEAD/tree或 STOP事实。
- Selected image：`quay.io/ascend/vllm-ascend:v0.20.2rc1-a3-openeuler`，image digest `sha256:442363921166771eb82baeec9c1ac0381f46fb830ead8d0e072df6e925f2a958`，arm64 platform digest `sha256:a55b2b0597f9fdd1882de9bf3b7ebc395dd77c1ca49f251d0cb759d7b2c1a807`，local image ID `sha256:8e7823878349b37d9900984555e28381c25681f88ff678cb4a86f7d1674a67c1`。
- No wheel produced；Gate C/D `NOT RUN`。
- Task container `qw36-a3-s1s2-env-pass-20260825190917` / `9f03ddc88115aec2865ae099596f8cd383a2647493397b72ce1f8f82d6c66adb` removed；NPU 0/1 released。
- Code PR：`N/A`。
- Evidence root：`/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S1S2-ENV-BUILD-RUNTIME/evidence`；main build log `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S1S2-ENV-BUILD-RUNTIME/evidence/20260825T205100+0800/build_wheel.log`。
- Immediate failure confidence：**HIGH**；metadata lookup未找到 `aic-*-ops-info.ini`，exception未恢复、exit 1、no wheel。
- Underlying root-cause confidence：**LOW / NOT CONFIRMED**；尚不能区分具体 op、CMake/opbuild、CANN behavior、SoC/path naming、selected-op flow或 lookup-directory问题，也未证明 source change必要。
- Gate A Evidence gap：Result只记录 `Triton=3.5.0`，未记录 `triton-ascend` distribution、module origin和 active provider。ARM上 `triton-ascend 3.2.1`可以正常依赖 community `triton 3.5.0`，故不判 wrong tuple，只要求 targeted补证。
- Immutable Result contract gap：缺显式 `Root-cause confidence`字段；只在 Review/STATUS/INDEX记录，不修改 immutable Result。

## Current authorization / dispatch gate

User已确认 bounded authorization：

- Codex2可在当前项目可访问的 A3/910C server上执行；若实际可见 target不唯一或 access无效，任何 mutation前 STOP并请求澄清。
- 先只读盘点 NPU型号、logical mapping、health、occupancy和 owner，再选择不干扰其他任务的最小 safe scope；不得 kill/pause/reset/preempt。
- 只在 official `v0.20.2rc1-a3`与`v0.20.2rc1-a3-openeuler`中做 base selection；可 pull/inspect、创建和清理本 Task自己的 container。两者均不兼容时 STOP / Decision requested。
- 可在现有 `/data`创建新的 Qwen Validation专属 work/Evidence/artifacts/cache目录，参考 `/data/tiankuan/zyg/FL/`，但不得覆盖既有目录或写入模型目录；返回 exact paths。
- 可使用现有 GitHub/package index/container registry/CATLASS访问；离线 artifact必须可核验，CATLASS绑定 exact `41bf90da655bba3c66d0acd7e00abe33960ecfd6`。

当前正常下一步是 User决定是否 dispatch Ready Task `QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG`。它先补 Triton/provider Evidence并定位 Gate B root cause：需要 source change则 STOP/Decision；non-source修正闭合 Gate B则沿用同一 container直接执行 parent Gate C/D。Codex2当前未运行，不得自动续跑；即使 Stage 1/2 Execution PASS也必须等待 Codex1 Acceptance，Stage 3仍锁定。

## 当前已确认的高影响事实

- vLLM baseline是 `0.20.2`；vLLM-Ascend `0.20.2rc1`只作 matched implementation/oracle reference。
- Official image matrix把 ordinary `v0.20.2rc1`映射为 A2 Ubuntu；正式 A3候选只有 `v0.20.2rc1-a3`与`v0.20.2rc1-a3-openeuler`。两条 build definition均使用 CANN 9.0.0 A3/Python 3.11、vLLM 0.20.2、torch/torch_npu 2.10.0和 Triton Ascend 3.2.1。
- Base image是 carrier/builder，不改变 final ownership；Stage 2仍要求 absent `vllm-ascend` distribution/module/runtime dependency和正式 FL wheel/site-packages origin。
- 当前 head静态存在 `PlatformFL / WorkerFL / ModelRunnerFL`、FL-local Qwen/GDN/Mamba/Attention/MoE、`FULL_DECODE_ONLY` 和 Ascend build/package路径；静态存在不证明 A3执行。
- 当前 exact source列出 **8 个 OPP、9 个 `_C_ascend` schemas**；PR正文旧的 7/8计数已过时，Stage 2以 exact-head source为准并保存 wheel inventory。
- A3 build/package在 `SOC_VERSION`未设置时默认 `ascend910_93`；runtime custom-op选择未设置时却静态默认 `ascend910b1`。这只是 Stage 1/2风险，必须保存实际 effective `SOC_VERSION`、selected prebuilt root、extension/OPP origin；未在 A3复现前不是 blocker。
- A3 binary必须在 A3/compatible CANN环境重新构建；A2 wheel不可复用。
- 正式 runtime必须 `VLLM_PLUGINS=fl`、`USE_FLAGGEMS=0`，不得依赖 installed `vllm-ascend`，不得从源码树或 `PYTHONPATH`加载 `vllm_fl`。
- User确认模型为 `Qwen/Qwen3.6-35B-A3B` BF16 non-quantized，路径 `/data/tiankuan/zyg/FL/workspace/Qwen3.6-35B-A3B`，当前仍在下载。Stage 1/2只允许只读 presence/download-state inventory；不等待、不加载、不以完整权重作为 PASS条件。

## 不允许的状态外推

- A2 PASS不等于 A3 PASS。
- wheel build/文件存在不等于 NPU kernel正确执行。
- schema注册不等于 model path执行。
- capture成功不等于 replay/state正确。
- Execution PASS不等于 formal Acceptance。
- 当前 head的 Acceptance不自动覆盖 future branch head。
