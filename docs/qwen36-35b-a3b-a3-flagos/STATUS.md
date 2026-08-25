# 项目状态

更新时间：2026-08-25

总体状态：Stage 0 **COMPLETE**；A3 **UNVERIFIED / NOT STARTED**；首个 Codex2 Task `QWEN36-A3-S1S2-ENV-BUILD-RUNTIME` 为 **READY / Awaiting explicit User dispatch**。

## 当前快照

| Work item | Status | Evidence boundary |
| --- | --- | --- |
| Control repo | Established | 本仓库 `main`；首次 Control commit |
| Tracked implementation | Current GitHub snapshot reverified | `feature/qwen3.6-35b-a3b-ascend-graph-migration@7beda84...` / tree `a81eea...`；dispatch前重查 |
| Official base | Current GitHub snapshot recorded | `release/0.2@53adefb...` / tree `9ddfd0...` |
| PR #404 | OPEN / DRAFT / MERGEABLE / BLOCKED / REVIEW_REQUIRED | GitHub snapshot 2026-08-25 17:42 CST；状态会变化 |
| A2 implementation evidence | **A2 REFERENCE ONLY** | User资料中的 2×910B1结果，不是 A3 Acceptance |
| A3 environment/build/runtime | **UNVERIFIED** | 尚未访问 A3 server |
| A3 model/graph/serve/function/performance | **UNVERIFIED** | 没有 A3 execution Evidence |
| Official A3 base route | Bounded selection authorized | `v0.20.2rc1-a3` Ubuntu或`v0.20.2rc1-a3-openeuler`；ordinary unsuffixed A2 image excluded |
| Model artifact | `DOWNLOADING / NOT YET READY FOR STAGE 3` | `/data/tiankuan/zyg/FL/workspace/Qwen3.6-35B-A3B`；不阻塞Stage 1/2 |
| First Codex2 task | **READY / Awaiting explicit User dispatch** | Environment + source identity + A3 wheel + standalone runtime smoke |
| Validation Code repo/fork | **Not needed yet** | 尚无 attributable A3 blocker |
| GLM project | PAUSED by User Decision | 独立 Control；旧 Evidence/history保留，不写入本仓库 |

## 当前 implementation identity

核验时间：2026-08-25 17:42 CST；相对首次 Control snapshot未变化。

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

## Current authorization / dispatch gate

User已确认 bounded authorization：

- Codex2可在当前项目可访问的 A3/910C server上执行；若实际可见 target不唯一或 access无效，任何 mutation前 STOP并请求澄清。
- 先只读盘点 NPU型号、logical mapping、health、occupancy和 owner，再选择不干扰其他任务的最小 safe scope；不得 kill/pause/reset/preempt。
- 只在 official `v0.20.2rc1-a3`与`v0.20.2rc1-a3-openeuler`中做 base selection；可 pull/inspect、创建和清理本 Task自己的 container。两者均不兼容时 STOP / Decision requested。
- 可在现有 `/data`创建新的 Qwen Validation专属 work/Evidence/artifacts/cache目录，参考 `/data/tiankuan/zyg/FL/`，但不得覆盖既有目录或写入模型目录；返回 exact paths。
- 可使用现有 GitHub/package index/container registry/CATLASS访问；离线 artifact必须可核验，CATLASS绑定 exact `41bf90da655bba3c66d0acd7e00abe33960ecfd6`。

当前唯一正常 dispatch gate是 **User明确把 Ready Task下发给 Codex2**。无需 User预先手工选择 device、OS image或服务器目录；现场若目标歧义、权限不足或需要扩大版本/route，再请求 User决定。

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
