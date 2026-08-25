# 项目状态

更新时间：2026-08-25

总体状态：Stage 0 **COMPLETE**；A3 **UNVERIFIED / NOT STARTED**；首个 Codex2 Task `QWEN36-A3-S1S2-ENV-BUILD-RUNTIME` 为 **Waiting User input / Not Ready**。

## 当前快照

| Work item | Status | Evidence boundary |
| --- | --- | --- |
| Control repo | Established | 本仓库 `main`；首次 Control commit |
| Tracked implementation | Current GitHub snapshot recorded | `feature/qwen3.6-35b-a3b-ascend-graph-migration@7beda84...` / tree `a81eea...`；dispatch前重查 |
| Official base | Current GitHub snapshot recorded | `release/0.2@53adefb...` / tree `9ddfd0...` |
| PR #404 | OPEN / DRAFT / MERGEABLE / BLOCKED / REVIEW_REQUIRED | GitHub snapshot 2026-08-25 17:02 CST；状态会变化 |
| A2 implementation evidence | **A2 REFERENCE ONLY** | User资料中的 2×910B1结果，不是 A3 Acceptance |
| A3 environment/build/runtime | **UNVERIFIED** | 尚未访问 A3 server |
| A3 model/graph/serve/function/performance | **UNVERIFIED** | 没有 A3 execution Evidence |
| First Codex2 task | Waiting User input / Not Ready | Environment + source identity + A3 wheel + standalone runtime smoke |
| Validation Code repo/fork | **Not needed yet** | 尚无 attributable A3 blocker |
| GLM project | PAUSED by User Decision | 独立 Control；旧 Evidence/history保留，不写入本仓库 |

## 当前 implementation identity

核验时间：2026-08-25 17:02 CST。

- Tracked repo/branch：`xiemingda-1002/vllm-plugin-FL` / `feature/qwen3.6-35b-a3b-ascend-graph-migration`。
- Current head：`7beda84f59d7b25f49cdf03bdf6efecd771067ed`。
- Current tree：`a81eea55c1de548a0a1f182f51089eca0b088c82`。
- Official base/ref：`flagos-ai/vllm-plugin-FL:release/0.2`。
- Current release HEAD/tree：`53adefb269571684d83a51e997d3ba9be5f88235` / `9ddfd080953ad39b39772e108ff921d2973b0299`。
- PR compare：ahead 6 / behind 0；base SHA与当前 release HEAD一致。
- Branch movement：PR timeline显示此前 head `f9281f...` 被 force-push/rebase 为 `7beda84...`。User资料没有冻结可比较的旧 full SHA；不能声称“相对资料某 exact SHA”的普通 fast-forward。

以上只是 Control创建时的 moving GitHub snapshot。正式 Task在 User dispatch 前必须重新查询、冻结当次 exact HEAD/tree；若已变化，先做 diff review。

## 当前 Stage — Stage 1/2 dispatch preparation

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

首任务不运行完整 Qwen模型、graph、prefix、EP、64K 或 benchmark。Stage 1/2只有在 Codex2 `Execution PASS` 后经 Codex1独立审查，才可能写 `ACCEPTED` 并解锁独立 Stage 3 eager task。

## Ready 尚缺的 User 输入

1. A3/910C服务器或 Codex2可用执行入口，以及明确 dispatch。
2. 本任务获授权的安全 logical device范围；Stage 1/2只需最小 NPU smoke，不默认占用 TP2。
3. 已批准的 container/base-image route，或授权 Codex2先在给定候选中确认 matched vLLM 0.20.2 / CANN / torch-npu环境；major tuple变化仍需 User决定。
4. 服务器上允许写入的 project/work、Evidence、artifact/cache根目录，以及容器/镜像创建权限。
5. 依赖获取方式：GitHub/CATLASS/package index/registry可达，或离线 artifact位置。CATLASS如离线必须绑定 `41bf90da655bba3c66d0acd7e00abe33960ecfd6`。
6. 模型路径可在 Stage 1仅做 presence/identity inventory，但不是本 Task PASS前置；Stage 3 dispatch前必须确认完整 BF16模型路径。

## 当前已确认的高影响事实

- vLLM baseline是 `0.20.2`；vLLM-Ascend `0.20.2rc1`只作 matched implementation/oracle reference。
- 当前 head静态存在 `PlatformFL / WorkerFL / ModelRunnerFL`、FL-local Qwen/GDN/Mamba/Attention/MoE、`FULL_DECODE_ONLY` 和 Ascend build/package路径；静态存在不证明 A3执行。
- 当前 exact source列出 **8 个 OPP、9 个 `_C_ascend` schemas**；PR正文旧的 7/8计数已过时，Stage 2以 exact-head source为准并保存 wheel inventory。
- A3 build/package在 `SOC_VERSION`未设置时默认 `ascend910_93`；runtime custom-op选择未设置时却静态默认 `ascend910b1`。这只是 Stage 1/2风险，必须保存实际 effective `SOC_VERSION`、selected prebuilt root、extension/OPP origin；未在 A3复现前不是 blocker。
- A3 binary必须在 A3/compatible CANN环境重新构建；A2 wheel不可复用。
- 正式 runtime必须 `VLLM_PLUGINS=fl`、`USE_FLAGGEMS=0`，不得依赖 installed `vllm-ascend`，不得从源码树或 `PYTHONPATH`加载 `vllm_fl`。

## 不允许的状态外推

- A2 PASS不等于 A3 PASS。
- wheel build/文件存在不等于 NPU kernel正确执行。
- schema注册不等于 model path执行。
- capture成功不等于 replay/state正确。
- Execution PASS不等于 formal Acceptance。
- 当前 head的 Acceptance不自动覆盖 future branch head。
