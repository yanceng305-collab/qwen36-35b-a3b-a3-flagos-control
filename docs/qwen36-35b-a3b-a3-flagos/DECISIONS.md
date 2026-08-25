# 技术与治理决策

更新时间：2026-08-25

| ID | Decision | Status | Rationale / boundary | Revisit trigger |
| --- | --- | --- | --- | --- |
| D-001 | 本项目是 A3 Validation Control Project，不重新开发 Qwen3.6 fork | Required | 同事 PR #404 implementation已基本完成；当前主要未知是 A3真实构建/运行 | 真实 Evidence证明当前路线根本不可行 |
| D-002 | implementation source of truth持续跟踪同事 feature branch及官方 PR #404 | Required | 验证当前同事主线，避免无理由长期分叉 | PR合并/关闭、branch替换或 User改变 source of truth |
| D-003 | moving branch与 execution identity分离；每次正式结果绑定 exact SHA/tree/clean state | Required | branch会继续更新，旧 PASS不能覆盖新 commit | 不取消；只按 diff选择重验范围 |
| D-004 | baseline为 vLLM 0.20.2 + FL release/0.2系；vLLM-Ascend 0.20.2rc1仅作 reference | Required | current source/PR与资料一致；最终 runtime必须 standalone FL | current source或 User批准 major baseline change |
| D-005 | A2全部结果统一标记 `A2 REFERENCE ONLY — NOT A3 ACCEPTANCE` | Required | 资料中真实执行是 2×910B1，不是 910C | 不能由推断取消；A3必须另行执行 |
| D-006 | 默认 `VERIFY FIRST / FIX ONLY WHEN EVIDENCE REQUIRES IT` | Required | 避免在没有 A3 blocker时重构/重新适配 | attributable blocker + root cause + bounded scope |
| D-007 | 当前不创建 validation Code repo/fork | Required / current | 暂无 A3 blocker或 patch需求 | 需要自有 small A3-specific fix且同事未直接修复 |
| D-008 | A3先走 environment/build/runtime，再 eager、graph、serve、functional expansion、performance | Required | correctness-first，隔离 A3 family/ABI/runtime风险 | Stage gate Evidence支持调整 |
| D-009 | A3 wheel必须在 A3/compatible CANN构建，family为 `ascend910_93`；禁止复用 A2 wheel | Required | A2=`ascend910b`，A3=`ascend910_93`，binary/OPP不可外推 | current source/CANN合同改变且获批准 |
| D-010 | standalone runtime要求 `USE_FLAGGEMS=0`、无 installed/runtime `vllm-ascend`依赖、正式 wheel/site-packages origin | Required | 证明 FL-local ownership和可重建安装，不接受 source-tree shortcut | User批准改变正式 runtime ownership（当前无） |
| D-011 | 首任务合并 Stage 1/2，只做 environment/source identity、A3 wheel、install、custom-op smoke | Waiting User input / Not Ready | 最小高信息量 gate；不把昂贵模型/graph/benchmark混入 | User inputs齐备并 dispatch |
| D-012 | current exact-head source的 8 OPP / 9 schemas优先于 PR正文旧的 7/8 | Required | exact source是当前可执行事实；PR prose已过时 | tracked head改变后重新盘点 |
| D-013 | build/runtime `SOC_VERSION`默认不对称只登记为风险，不提前写 blocker或 patch | Required | 静态 build默认 A3、loader默认 A2；实际环境可能显式提供变量 | A3执行复现 family选择/加载失败 |
| D-014 | Qwen A3 handoff只向 GLM提供 A3真实验证过的通用 runtime/build/evidence事实 | Required | Qwen model/GDN/Mamba/graph/performance不能证明 GLM/W8A8 | 对应通用事实有 A3 Acceptance并进入 handoff |
| D-015 | GLM项目 PAUSED；恢复时先做 vLLM 0.20.2 GLM contract review，不在本项目开始 GLM port | User Decision | 当前优先级转为 Qwen A3验证；保留 GLM历史 | User明确恢复 GLM |

## D-002 / D-003 — tracking 与正式验证身份

Control维护两个不同字段：

```text
Project tracking target:
  xiemingda-1002/vllm-plugin-FL
  feature/qwen3.6-35b-a3b-ascend-graph-migration

Formal run identity:
  branch + exact HEAD SHA + exact tree + worktree clean/dirty
```

同事 push新 commit后，Codex1必须先审查 old-tested-SHA → new-HEAD diff，决定是否只需 packaging/custom-op regression、eager、graph或完整 functional matrix；不得把旧 Result的 `PASS/ACCEPTED`文本改写成覆盖新 SHA。

## D-006 / D-007 — blocker到 Code fix

允许普通诊断、build、install、runtime检查和 first-blocker缩小。需要修改源码时：

1. 保存 first attributable blocker与 root-cause Evidence；
2. 写明 bounded fix scope、风险、回归范围和 Code owner；
3. 同事修复时跟踪其新 commit并重新冻结 execution identity；
4. 我方修复时再决定 User-controlled fork/task branch或独立 validation Code repo；
5. 修改、验证、commit/PR和 Control Result必须形成闭环。

如果范围明显超过 minor validation fix，STOP并请求 User重定义 Code task。服务器 working tree不得长期保留 undocumented patch。

## D-008 — Stage gate原则

- Stage 2 formal Acceptance前不运行完整模型。
- Stage 3 eager Acceptance前不进入 graph。
- Graph正确性后再做 serve。
- prefix/long context/EP/concurrency等从基础 A3结果逐项扩展，不机械复制 A2矩阵。
- correctness、graph、serve稳定前不做 performance/capacity。
- A3 performance必须使用 A3自己的工作负载、环境、原始数据和重复运行。

## 明确拒绝的路线

- 没有 blocker时复制或重写大段 Qwen/vLLM-Ascend实现；
- silent downgrade vLLM/CANN/torch-npu、替换模型、绕过 PR #404；
- 把 A2 wheel直接装到 A3；
- 通过源码 `PYTHONPATH`、editable import或遗留 build目录伪装 wheel成功；
- 因 image名称含 vLLM-Ascend而自动判违规，或因静态无 import就自动判运行独立；最终结论必须来自 installed/runtime Evidence；
- schema/import smoke直接外推 model correctness；
- 基础 gate前运行完整 A2 1K/4K/16K/64K × C1/C8/C32/C64性能矩阵；
- 在 Qwen项目中启动 GLM model适配或把 Qwen-specific结论外推给 GLM。
