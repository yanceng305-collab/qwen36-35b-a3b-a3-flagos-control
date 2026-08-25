# Agent Guide

## 项目一句话

本项目在 Ascend A3/910C 上验证同事当前 PR #404 / Qwen3.6-35B-A3B Ascend adaptation，形成可审计的 A3 build、standalone FL runtime、模型正确性、graph、serve 与交接证据；默认先验证，只有真实 blocker 才触发有边界的小修复。

面向人的项目地图见 [README.md](README.md)。动态状态以 [STATUS.md](docs/qwen36-35b-a3b-a3-flagos/STATUS.md) 和 [results/INDEX.md](docs/qwen36-35b-a3b-a3-flagos/results/INDEX.md) 为准。

## 角色与权力边界

- **User / Project Owner**：决定 dispatch、A3 NPU 资源、模型路径、major route/version/CANN/baseline change、风险接受和 Stage gate。
- **ChatGPT**：日常技术讨论、GitHub/官方源码核查、Codex1/Codex2独立复核、日志分析、task splitting、prompt 与项目协调；可建议 `PASS / REJECT / FOLLOW-UP`，但不是正式 Control Acceptance authority。
- **Codex1**：维护本 Control repo、baseline/current target、PLAN、DECISIONS、Task contract、Result review、formal Acceptance 与 Stage gate；原则上不直接操作 A3 服务器。
- **Codex2**：在 User 正式 dispatch 后负责 A3 environment、build、install、run、debug、Evidence、immutable Result、必要的小范围 Code fix task 和 Control sync。

User 的明确决定高于代理建议；服务器 `Execution PASS` 不自动等于 Codex1 `ACCEPTED`。

## 仓库边界

- **Control repo（本仓库）**：项目目标、状态、决策、计划、task/prompt、research、Evidence pointer、immutable result、Acceptance 与 reconstruction/handoff。
- **Implementation repo**：[`xiemingda-1002/vllm-plugin-FL`](https://github.com/xiemingda-1002/vllm-plugin-FL)，持续跟踪 `feature/qwen3.6-35b-a3b-ascend-graph-migration`。
- **Official review/base**：[`flagos-ai/vllm-plugin-FL#404`](https://github.com/flagos-ai/vllm-plugin-FL/pull/404) / `release/0.2`。
- **Validation Code repo/fork**：当前 `Not needed yet`。只有真实 A3 blocker、root-cause Evidence 和 bounded fix scope 成立后，由 User/Codex1决定。
- **Server Evidence**：原始日志、环境 manifest、命令与 exit code、checksum、wheel、runtime facts。大文件留在服务器 Evidence 目录；Control 保存稳定指针和 immutable result。

不得把 FL 源码或未归档服务器 patch 放入 Control repo。不得把本项目 Qwen Result 写入 GLM Control。

## 新会话读取顺序

### Codex1 / ChatGPT

1. `AGENTS.md`
2. `README.md`
3. `docs/qwen36-35b-a3b-a3-flagos/STATUS.md`
4. `docs/qwen36-35b-a3b-a3-flagos/DECISIONS.md`
5. `docs/qwen36-35b-a3b-a3-flagos/results/INDEX.md`
6. 当前 task、对应 immutable result 与 Evidence pointer
7. `BASELINE.md`、必要的 research/reconstruction/handoff；移动 GitHub 状态必须重新查询

### Codex2

1. `AGENTS.md`
2. `README.md`
3. `docs/qwen36-35b-a3b-a3-flagos/STATUS.md`
4. User 已下发的当前 task 与 prompt
5. `REPOSITORY-AND-EVIDENCE-RULES.md`
6. 当前 run/work/container/Evidence 状态
7. implementation tracked branch 的实时 HEAD/tree/clean state

没有 User 下发的 `Ready` task 时，Codex2不得把计划、历史 prompt 或聊天摘录当作执行授权。

## Moving branch 与 execution identity

项目持续跟踪：

```text
xiemingda-1002/vllm-plugin-FL
  feature/qwen3.6-35b-a3b-ascend-graph-migration
```

每次正式执行在 dispatch 时必须记录：

```text
tracked branch
exact HEAD SHA
exact tree
working tree clean/dirty
```

Evidence、Result、PASS、Acceptance 只对该次 exact SHA/tree 有效。分支更新后，Codex1先审查旧验证 SHA到新 HEAD 的 diff，再决定 tiny regression、eager、graph 或完整 functional matrix 的重验范围；不得把旧 PASS 自动转移给新 commit。

## Prompt / Task 约定

- Task只冻结 objective、boundaries、User-confirmed facts、Ready、PASS、STOP、required Evidence 与 Acceptance边界。
- 不无必要写死普通 shell command、命令顺序或排障过程；Codex2在合同内自主检查和定位。
- 重大路线、版本、CANN、baseline、模型、quantization 或大范围架构变化必须 `Decision requested`。
- 当前分支/PR/SHA等移动事实必须在 dispatch 前从 GitHub重新查询，不能使用旧 prompt 或聊天 SHA。

## 简单优先 / 不无故复杂化

本项目默认采用**满足当前目标与 Evidence gate 的最简单可行方案**。当已有经过验证的参考流程时，优先复用其整体执行方式，只修改当前硬件、版本或真实 blocker要求变化的部分。

不得仅为了“更规范”“更隔离”或假设未来可能需要，在没有实际 Evidence/blocker时增加：

- 额外 container/environment层；
- 中间 artifact handoff；
- 新 repo/fork；
- 新 abstraction/wrapper；
- 新 build/runtime分层；
- 多余 Stage/Task；
- 当前问题未要求的重构。

任何新增复杂度必须明确回答：

1. 它解决了哪个已经观察到的真实问题？
2. 不增加它会导致哪个当前 Task gate无法满足？
3. 增加后的新变量和验证成本是否值得？

无法明确回答时，保持现有更简单方案。

> Prefer the smallest change and shortest execution path that can satisfy the current gate. Do not solve hypothetical future problems before they become real blockers.

## VERIFY FIRST / FIX ONLY WHEN EVIDENCE REQUIRES IT

Codex2可以检查、build、install、run、debug、收集 traceback/log、缩小 first blocker。任何源码修改必须遵循：

```text
first attributable blocker
  -> root-cause Evidence
  -> bounded fix scope
  -> Control Decision / Code task
  -> task branch
  -> modification
  -> verification
  -> commit / PR
```

如果需要的修改明显超过 minor validation fix，立即 STOP；不得 silent downgrade、换模型、更新整套 vLLM-Ascend、扩大复制代码或无边界重构。

## Task → Evidence → Result → Acceptance

```text
Codex1 task contract + User dispatch
  -> Codex2 A3 execution
  -> raw Server Evidence
  -> immutable Result + results/INDEX.md sync
  -> Codex1 independent Code/Evidence/Result review
  -> ACCEPTED / REJECTED / NEEDS-FOLLOWUP
```

每个 run 必须保留 Code/source、Control、Evidence 三指针。Immutable Result首次 push 后不得修改；Acceptance只更新 `results/INDEX.md` 或 `STATUS.md`。完整规则见 [REPOSITORY-AND-EVIDENCE-RULES.md](docs/qwen36-35b-a3b-a3-flagos/REPOSITORY-AND-EVIDENCE-RULES.md)。

## 证据词汇

- `A2 DOCUMENTED PASS / A2 REFERENCE ONLY`：来自 User资料的 910B1 执行事实，只能当 A3 oracle。
- `A3 UNVERIFIED`：尚无真实 A3 execution evidence。
- `Execution PASS`：Codex2按 task 报告执行通过，尚未 formal Acceptance。
- `ACCEPTED`：Codex1完成独立审查后，只在明确环境、版本、SHA/tree 和覆盖边界内接受。

任何 A3 PASS/Acceptance 必须来自真实 A3/910C execution evidence。静态源码、配置、CI/YAML、A2结果或 wheel内容检查都不能单独证明 A3 模型正确性。

## 禁止事项

- 不 force push、不改写 immutable result、不删除历史 Evidence、不把 dirty source 的正式结果伪装成 clean PASS。
- 不复用 A2 binary wheel 到 A3；不通过源码 `PYTHONPATH` 或 editable/source-tree import 绕过正式 wheel安装。
- 正式 Qwen runtime必须 `USE_FLAGGEMS=0`，不得依赖已安装 `vllm-ascend` Python package。
- 基础 gate 未完成前不运行 graph、prefix、EP、64K 或 benchmark；correctness/graph/serve 稳定前不做性能优化。
- 不把 A2 `104.82%`、KV capacity、cold start 或功能矩阵写成 A3 结果。
- 不在本项目启动 GLM port；A3 Runtime Handoff只记录本项目真实验证后可复用的通用基础。
