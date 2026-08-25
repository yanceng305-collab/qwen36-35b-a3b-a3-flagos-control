# Qwen3.6-35B-A3B × FlagOS × Ascend A3/910C Validation Control

本仓库是 Qwen3.6-35B-A3B A3 Validation Control Project 的正式 truth source。它管理目标、moving implementation、exact execution identity、Stage gate、Task、Evidence、immutable Result、Codex1 Acceptance 与 A3 Runtime Handoff。

## Project Goal

> 在 Ascend A3/910C 上验证当前 PR #404 / Qwen3.6-35B-A3B Ascend adaptation，在 vLLM 0.20.2 + FL release/0.2 系上完成 A3-native wheel build、standalone FL runtime、TP2 eager、FULL_DECODE_ONLY graph、serve 及必要功能验证，并形成可供未来 GLM-5.2-W8A8 复用的 A3 runtime / container / build / reconstruction 基础。

本项目不是重新开发一个 Qwen3.6 fork；当前路线是 `VERIFY FIRST / FIX ONLY WHEN EVIDENCE REQUIRES IT`。

## Current State

更新时间：2026-08-25

- Stage 0 Control / baseline establishment：**COMPLETE**。
- A3 execution：**NOT STARTED / A3 UNVERIFIED**。
- 首个 Codex2 Task：`QWEN36-A3-S1S2-ENV-BUILD-RUNTIME`，**READY / Awaiting explicit User dispatch**。
- Validation Code repo/fork：**Not needed yet**。
- GLM-5.2-W8A8项目：由 User Decision 暂停；本仓库不接收 GLM Result。

## Tracked Implementation

GitHub状态核验时间：2026-08-25 17:42 CST；PR/head/base未变化。移动状态在 dispatch 前必须重新查询。

| Field | Current verified value |
| --- | --- |
| Implementation repo | [`xiemingda-1002/vllm-plugin-FL`](https://github.com/xiemingda-1002/vllm-plugin-FL) |
| Tracked branch | `feature/qwen3.6-35b-a3b-ascend-graph-migration` |
| Current head | `7beda84f59d7b25f49cdf03bdf6efecd771067ed` |
| Current head tree | `a81eea55c1de548a0a1f182f51089eca0b088c82` |
| Official review | [`flagos-ai/vllm-plugin-FL#404`](https://github.com/flagos-ai/vllm-plugin-FL/pull/404) |
| PR state | `OPEN / DRAFT / MERGEABLE / BLOCKED / REVIEW_REQUIRED` |
| Official base | `flagos-ai/vllm-plugin-FL:release/0.2` |
| Base/release HEAD | `53adefb269571684d83a51e997d3ba9be5f88235` |
| Base/release tree | `9ddfd080953ad39b39772e108ff921d2973b0299` |

当前 feature head 比 `release/0.2` ahead 6 / behind 0。PR timeline显示分支曾从 `f9281f...` force-push/rebase 到当前 `7beda84...`；因此项目跟踪 moving branch，但任何正式执行/结果只绑定 dispatch 时的 exact SHA/tree。

## Technical Baseline

| Item | Baseline / boundary |
| --- | --- |
| vLLM | `0.20.2` |
| FL | `release/0.2` line；adaptation以 PR #404 current tracked head为准 |
| vLLM-Ascend | `0.20.2rc1`，matched-version implementation/oracle reference；不是最终 runtime dependency |
| Stage 1/2 base candidates | `quay.io/ascend/vllm-ascend:v0.20.2rc1-a3` / `...:v0.20.2rc1-a3-openeuler`；only bounded selection |
| Model | `Qwen/Qwen3.6-35B-A3B` |
| Model path/state | `/data/tiankuan/zyg/FL/workspace/Qwen3.6-35B-A3B`；`DOWNLOADING / NOT YET READY FOR STAGE 3`；不阻塞Stage 1/2 |
| dtype / DP / first TP | BF16 / DP1 / TP2 |
| First model execution | eager |
| Graph | `FULL_DECODE_ONLY`，仅 eager Acceptance 后 |
| Required runtime | `VLLM_PLUGINS=fl`、`USE_FLAGGEMS=0`、standalone FL wheel、无 installed `vllm-ascend` |
| Out of first scope | MTP、quantization、CP、FlashComm、MC2、EPLB |

## Required Live Path

```text
VLLM_PLUGINS=fl
  -> PlatformFL
  -> WorkerFL
  -> ModelRunnerFL
  -> FL-local Qwen3.6 patches
  -> GDN / Mamba / Attention / MoE
  -> FL-local Ascend runtime
  -> _C_ascend / CANN OPP / Triton / torch_npu
  -> HCCL
  -> Ascend A3/910C
```

正式 Evidence必须证明 `vllm_fl`来自 wheel/site-packages，不通过源码 `PYTHONPATH`，运行时不 import/依赖 `vllm_ascend`，A3 wheel确实包含并加载 `ascend910_93` family的 `_C_ascend` 与 OPP，且没有复用 A2 binary。

## Stages

| Stage | Goal | Gate |
| --- | --- | --- |
| 0 | Project / baseline establishment | **COMPLETE** |
| 1 | A3 environment + build readiness | **READY / Awaiting explicit User dispatch** |
| 2 | A3-native wheel + standalone FL runtime/custom-op smoke | 同一 Task内，Gate A通过后进入 |
| 3 | TP2 BF16 eager model correctness | Stage 2 `ACCEPTED` 后 |
| 4 | `FULL_DECODE_ONLY` capture/replay/state correctness | Stage 3 `ACCEPTED` 后 |
| 5 | Serve health/models/completion/chat/repeat/bounded concurrency | Stage 4 `ACCEPTED` 后 |
| 6 | Prefix、long context、EP2、concurrency、chunked prefill、async、64K | 按 A3 Evidence逐项解锁 |
| 7 | Performance / capacity | correctness + graph + serve稳定后 |
| 8 | Runtime freeze / reconstruction / handoff | 所需 A3 stages Accepted后 |

完整关键路径和每阶段证据见 [PLAN.md](docs/qwen36-35b-a3b-a3-flagos/PLAN.md)。

## A2 Evidence Boundary

User资料记录了 2×Ascend 910B1 上的 standalone FL、eager、`FULL_DECODE_ONLY`、prefix、EP2、64K、功能/性能矩阵等完整结果。这些内容统一保存在 [A2-REFERENCE.md](docs/qwen36-35b-a3b-a3-flagos/A2-REFERENCE.md)，状态始终是：

```text
A2 REFERENCE ONLY — NOT A3 ACCEPTANCE
```

任何 A3 PASS/Acceptance必须来自真实 A3/910C execution evidence。

## Current Task

- Task contract：[QWEN36-A3-S1S2-ENV-BUILD-RUNTIME.md](docs/qwen36-35b-a3b-a3-flagos/tasks/QWEN36-A3-S1S2-ENV-BUILD-RUNTIME.md)
- Codex2 prompt：[CODEX2-QWEN36-A3-S1S2-ENV-BUILD-RUNTIME-PROMPT.md](docs/qwen36-35b-a3b-a3-flagos/tasks/CODEX2-QWEN36-A3-S1S2-ENV-BUILD-RUNTIME-PROMPT.md)
- 状态：**READY / Awaiting explicit User dispatch**。
- 目标：A3 environment identity + current PR404 implementation identity + A3 wheel build + standalone FL installation/runtime/custom-op smoke。
- 明确不执行：完整模型、graph、prefix、EP、64K、benchmark。
- Base route：只允许在 official `v0.20.2rc1-a3`和`v0.20.2rc1-a3-openeuler`中根据现场 compatibility选择；普通无后缀 A2 image、nightly和其他版本均不是 fallback。
- 模型：已记录正在下载的正式路径；本 Task仅可做presence/download-state inventory，不等待、不加载、不验证完整权重。

## What Is Not Done

- 尚无 A3 wheel build、install、custom-op、TP2/HCCL、模型、graph、serve、prefix、EP2、64K、capacity 或性能 Evidence。
- 尚未证明 A3 runtime/container/build tuple。
- 尚未创建 validation Code repo/fork；没有 Evidence要求代码修改。
- 尚未建立可供 GLM继承的 A3-proven runtime事实；[A3-RUNTIME-HANDOFF.md](docs/qwen36-35b-a3b-a3-flagos/A3-RUNTIME-HANDOFF.md)当前全部为候选/未验证。

## Navigation

| Question | Source of truth |
| --- | --- |
| 当前状态、门禁、User待确认输入 | [STATUS.md](docs/qwen36-35b-a3b-a3-flagos/STATUS.md) |
| 技术/治理决策 | [DECISIONS.md](docs/qwen36-35b-a3b-a3-flagos/DECISIONS.md) |
| Stage与关键路径 | [PLAN.md](docs/qwen36-35b-a3b-a3-flagos/PLAN.md) |
| 版本、分支、SHA/tree、baseline | [BASELINE.md](docs/qwen36-35b-a3b-a3-flagos/BASELINE.md) |
| A2 oracle/reference | [A2-REFERENCE.md](docs/qwen36-35b-a3b-a3-flagos/A2-REFERENCE.md) |
| 未来 GLM可继承/不可外推边界 | [A3-RUNTIME-HANDOFF.md](docs/qwen36-35b-a3b-a3-flagos/A3-RUNTIME-HANDOFF.md) |
| Task/Evidence/Result/Acceptance规则 | [REPOSITORY-AND-EVIDENCE-RULES.md](docs/qwen36-35b-a3b-a3-flagos/REPOSITORY-AND-EVIDENCE-RULES.md) |
| 当前 GitHub/source核查 | [CURRENT-IMPLEMENTATION-STATE.md](docs/qwen36-35b-a3b-a3-flagos/research/CURRENT-IMPLEMENTATION-STATE.md) |
| Result与Acceptance索引 | [results/INDEX.md](docs/qwen36-35b-a3b-a3-flagos/results/INDEX.md) |
