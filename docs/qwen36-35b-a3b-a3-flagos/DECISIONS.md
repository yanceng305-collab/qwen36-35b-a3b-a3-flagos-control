# 技术与治理决策

更新时间：2026-08-26

| ID | Decision | Status | Rationale / boundary | Revisit trigger |
| --- | --- | --- | --- | --- |
| D-001 | 本项目是 A3 Validation Control Project，不重新开发 Qwen3.6 fork | Required | 同事 PR #404 implementation已基本完成；当前主要未知是 A3真实构建/运行 | 真实 Evidence证明当前路线根本不可行 |
| D-002 | implementation source of truth持续跟踪同事 feature branch及官方 PR #404 | **Historical / superseded for Stage 6+ by D-030** | Stage 1-5用于验证当时同事主线 | 保留历史；不再作为future execution rule |
| D-003 | moving branch与 execution identity分离；每次正式结果绑定 exact SHA/tree/clean state | **Historical / superseded for Stage 6+ by D-030** | Stage 1-5保证旧PASS不错误覆盖新commit | 保留历史Result语义；Stage 6+改为Frozen artifact identity |
| D-004 | baseline为 vLLM 0.20.2 + FL release/0.2系；vLLM-Ascend 0.20.2rc1仅作 reference | Required | current source/PR与资料一致；最终 runtime必须 standalone FL | current source或 User批准 major baseline change |
| D-005 | A2全部结果统一标记 `A2 REFERENCE ONLY — NOT A3 ACCEPTANCE` | Required | 资料中真实执行是 2×910B1，不是 910C | 不能由推断取消；A3必须另行执行 |
| D-006 | 默认 `VERIFY FIRST / FIX ONLY WHEN EVIDENCE REQUIRES IT` | Required | 避免在没有 A3 blocker时重构/重新适配 | attributable blocker + root cause + bounded scope |
| D-007 | 当前不创建 validation Code repo/fork | Required / current | Stage 1/2 blockers均以 non-source path/network/container route闭合，无source patch需求 | Future confirmed blocker证明需要我方 bounded source fix |
| D-008 | A3先走 environment/build/runtime，再 eager、graph、serve、functional expansion、performance | Required | correctness-first，隔离 A3 family/ABI/runtime风险 | Stage gate Evidence支持调整 |
| D-009 | A3 wheel必须在 A3/compatible CANN构建，family为 `ascend910_93`；禁止复用 A2 wheel | Required | A2=`ascend910b`，A3=`ascend910_93`，binary/OPP不可外推 | current source/CANN合同改变且获批准 |
| D-010 | standalone runtime要求 `USE_FLAGGEMS=0`、无 installed/runtime `vllm-ascend`依赖、正式 wheel/site-packages origin | Required | 证明 FL-local ownership和可重建安装，不接受 source-tree shortcut | User批准改变正式 runtime ownership（当前无） |
| D-011 | 首任务合并 Stage 1/2，只做 environment/source identity、A3 wheel、install、custom-op smoke | **Ended / accepted through D-020 chain** | Parent STOP由后续diagnostic/wheel/C-D Results闭合；历史Result不改写 | 保留历史task boundary |
| D-012 | current exact-head source的 8 OPP / 9 schemas优先于 PR正文旧的 7/8 | **Historical / frozen at e610 by D-030** | exact tested source优先于旧PR prose | 只有User建立new baseline后重新盘点 |
| D-013 | build/runtime `SOC_VERSION`默认不对称只登记为风险，不提前写 blocker或 patch | Required | 静态 build默认 A3、loader默认 A2；实际环境可能显式提供变量 | A3执行复现 family选择/加载失败 |
| D-014 | Qwen A3 handoff只向 GLM提供 A3真实验证过的通用 runtime/build/evidence事实 | Required | Qwen model/GDN/Mamba/graph/performance不能证明 GLM/W8A8 | 对应通用事实有 A3 Acceptance并进入 handoff |
| D-015 | GLM项目 PAUSED；恢复时先做 vLLM 0.20.2 GLM contract review，不在本项目开始 GLM port | User Decision | 当前优先级转为 Qwen A3验证；保留 GLM历史 | User明确恢复 GLM |
| D-016 | Stage 1/2 base image只允许在 official `v0.20.2rc1-a3`与`v0.20.2rc1-a3-openeuler`中 bounded selection；无后缀 `v0.20.2rc1`明确排除 | Required / User-authorized | Official matrix把无后缀route映射到A2，把两个suffix映射到A3 Ubuntu/openEuler；final runtime仍须 standalone FL | Official source出现明确反证或两候选均不兼容并由User重决策 |
| D-017 | Codex2可在不干扰其他任务的前提下只读盘点并选择最小安全device scope、创建隔离的`/data` roots和Task container、使用可审计依赖访问 | Required / User-authorized | 用 bounded authorization替代逐项目录/设备设计；减少无价值Ready占位符 | 现场目标/owner/权限不明确或需要越界动作 |
| D-018 | 模型路径固定为`/data/tiankuan/zyg/FL/workspace/Qwen3.6-35B-A3B`；模型完成状态由Stage 3 Gate M独立验证 | **Satisfied / Gate M ACCEPTED** | initial root缺4个shards时正确STOP；resume证明26/26 shards、1045/1045 BF16 tensors和no quantization/download markers | model path/content/revision变化 |
| D-019 | `QWEN36-A3-S1S2-ENV-BUILD-RUNTIME` Formal Review为`NEEDS-FOLLOWUP`；只创建 Gate B OPP metadata diagnostic，不直接创建 source fix task | **Satisfied / closed by D-020** | Diagnostic/follow-up chain确认non-source path/network/runtime causes并闭合B/C/D，无source patch | 保留历史Review边界 |
| D-020 | Exact `7beda84...` A3 Stage 1/2 environment/build/standalone/custom-op foundation正式ACCEPTED；execution-proven composite container pattern成为默认runtime baseline | **Required / Codex1 ACCEPTED** | 4个immutable Results联合证明Gate A-D；无FlagGems、无vllm-ascend runtime、无source change；单项flag必要性未ablation | 新Accepted Evidence supersede或环境/source变化 |
| D-021 | Stage 3使用 current tracked head；`7beda84...→e610a990...`无需撤销Stage1/2 Acceptance，但需在preserved container内先rebuild+bounded C/D regression | **Satisfied / closed by D-022** | 2个新增commits只改communicator/platform/model runner，和TP2 live path相关，未改OPP/build packaging | Future HEAD再次变化或diff扩大 |
| D-022 | Exact `e610a990...` Stage 3 TP2 BF16 eager正式ACCEPTED；Stage 4只验证`FULL_DECODE_ONLY` capture/replay/fixed-address/state correctness | **Required / Codex1 ACCEPTED** | initial Gate M STOP由resume 26/26 shards、1045/1045 BF16 tensors闭合；TP2/HCCL、完整load、prefill/decode/repeat和standalone ownership通过；两个pre-construction harness/scope失败不要求机械重跑 | source/wheel/model/runtime变化或Stage 4 Evidence形成新结论 |
| D-023 | `e610a990... → 032fddc9...`为docs/tests-only bounded movement；Stage 4复用Stage 3 Accepted wheel，不rebuild、不重跑Stage 3 | **Historical / Codex1 reviewed** | exact diff仅`README.md` +11与新build-config unit test +66；production/runtime/build implementation对象未变；3/3新tests PASS | D-030将`032fddc9...`固定为last pre-change reference；future HEAD不再触发review |
| D-024 | Exact `e610a990...` wheel的Stage 4 `FULL_DECODE_ONLY [1,2,4,8]`正式ACCEPTED | **Required / Codex1 ACCEPTED** | G0 continuity、both-rank capture、batch-1/2 real replay、repeat/state freshness、finite outputs和clean exit闭环；pre-model错误probe均由corrected exit-0 probes闭合 | source/wheel/model/runtime变化，或扩大到service/automatic capture through 64 |
| D-025 | Stage 5只做serve correctness；通过后直接进入A2-equivalent DP1/TP2 16-cell functional reproduction，再测同矩阵performance | Required | 项目目标是A2→A3复现，不为流程本身无限拆小Stage；prefix/EP2等专项按价值补齐 | service或主矩阵出现需要独立隔离的真实blocker |
| D-026 | A2 vs A3只作cross-platform reproduction reference；FL相对性能优先A3 FL vs A3 matched native | Required | 910B1→910C硬件代际不同，绝对TPS差不能直接归因于FL；matched A3 comparison应统一cards/model/workload/sampling/cache/graph/warm-up | 无法获得matched native时明确记录comparison limitation |
| D-027 | Exact `e610a990...` wheel的Stage 5 bounded serve correctness正式ACCEPTED | **Required / Codex1 ACCEPTED** | S0 runtime continuity、health/models/completion/chat/repeat/C2、both-rank NPUGraph replay/state isolation和clean shutdown闭环；`<think>`格式不属于本bounded product-format contract | source/wheel/model/runtime变化，或扩大到automatic capture through 64 / chunked prefill / async / functional matrix |
| D-028 | Stage 6及以后每个Result必须足够支持最终`A3-END-TO-END-REPRODUCTION.md`，不得依赖聊天或执行者记忆 | Required | 最终新执行者必须仅凭Control、exact source、preserved artifacts/Evidence从环境准备复现到Accepted结果；大raw不进Git但identity/command/workload/result/Evidence/deviation必须可恢复 | 仅可由User改变最终交付合同；不得因单次Result体积取消 |
| D-029 | 下一主线为单一Stage 6 A2-equivalent DP1/TP2 16-cell functional Task；O8矩阵先warm-up，O1024做16/16 strict functional gate，performance另后置 | Required / Ready | A2正式资料冻结BF16、DP1/TP2、FULL_DECODE_ONLY、66560/64/16384、automatic capture、chunked/async、temperature=1、independent random prompts和seed公式；不再添加baseline外小correctness Stage | Frozen artifact/runtime/workload变化或首个真实functional blocker |
| D-030 / FROZEN-UPSTREAM-VALIDATION-BASELINE | Stage 6及后续functional、performance/capacity、prefix、EP2和handoff统一冻结在`e610a990...` / tree `609ff1ad...` / wheel SHA-256 `2fcf788...`；停止跟踪upstream moving HEAD | **Required / User Decision** | 项目目标改为验证固定、可复现的同事实现快照；避免debug清理、rebase/squash/history rewrite或继续开发污染A3数据，保证A2-to-A3比较和最终复现文档有单一代码基准 | 只有User新的正式Decision可建立new validation baseline；必须作为新baseline/project evidence，不能覆盖本项目结果 |

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
- Stage 5只验证service/API、graph replay和state isolation，不做performance。
- Stage 5通过后直接恢复A2 DP1/TP2 16-cell functional contract；prefix/EP2等专项不必全部挡住主矩阵。
- 16/16 functional correctness通过前不做对应performance/capacity。
- A3 performance必须使用 A3自己的工作负载、环境、原始数据和重复运行。

## D-016 — Official A3 image bounded selection

Official vLLM-Ascend `v0.20.2rc1@367b8e62...` installation matrix和 image workflow将 routes明确分开：

```text
quay.io/ascend/vllm-ascend:v0.20.2rc1             -> A2 Ubuntu / excluded
quay.io/ascend/vllm-ascend:v0.20.2rc1-a3          -> A3 Ubuntu candidate
quay.io/ascend/vllm-ascend:v0.20.2rc1-a3-openeuler -> A3 openEuler candidate
```

两条 A3 Dockerfile都固定 vLLM `v0.20.2`并使用 CANN 9.0.0 A3/Python 3.11 base；official compatibility tuple是 Python `>=3.10,<3.12`、torch/torch_npu `2.10.0/2.10.0`、Triton Ascend `3.2.1`。详细证据见 [OFFICIAL-A3-IMAGE-ROUTE.md](research/OFFICIAL-A3-IMAGE-ROUTE.md)。

Codex2先做安全只读 inventory，再按 actual host/driver/CANN/build/runtime compatibility选择 Ubuntu或 openEuler。必须记录 selected tag、resolved digest、image ID、OS、Python、CANN、torch、torch_npu、vLLM、Triton和选择理由。两条 route均实质不兼容时 STOP / `Decision requested`；不得退回 A2 image、nightly或其他版本。

Image只提供匹配 environment/build toolchain。Stage 2 final runtime仍必须证明 `vllm-ascend` distribution absent、`vllm_ascend`不可 import且无 runtime dependency，`vllm_fl`来自正式 wheel/site-packages、无 FL source `PYTHONPATH`/editable shortcut，`VLLM_PLUGINS=fl`、`USE_FLAGGEMS=0`，并加载本次 A3-native wheel的 `_C_ascend`/OPP。

## D-017 / D-018 — Bounded authorization and model state

- A3 target是当前项目可访问的 A3/910C server；若 Codex2面对多个无法区分的 target或没有有效 access，必须在任何 mutation前请求 User澄清。
- Device先只读检查型号、mapping、health、occupancy和 owner，再自主选择本 Task最小安全 scope；不得 kill/pause/reset/preempt或修改其他任务。
- Codex2可在现有 `/data`空间创建新的 Qwen A3 Validation专属 work/Evidence/artifacts/cache roots，参考 `/data/tiankuan/zyg/FL/`但不得写入模型目录或覆盖既有目录；必须返回 exact paths。
- 允许 pull/inspect两条 official A3 candidates，创建/停止/删除本 Task自己的临时 container，并在其内做 package transaction；不得改其他 container/image或 Host全局 Python/CANN。长期 image snapshot后置 Stage 8。
- 允许使用现有 GitHub/package index/registry/CATLASS访问；离线 artifact必须可核验。CATLASS继续绑定 `41bf90da655bba3c66d0acd7e00abe33960ecfd6`。
- 已确认模型路径当前仅作 inventory。模型下载完成不是 Stage 1/2 Ready/PASS条件；Stage 3前另行验证 config、architecture、tokenizer、index、全部 shards、size、checksum/manifest和 BF16/no-quantization contract。

## D-019 — Gate B STOP review boundary

- Gate A core device/image/environment/source identity接受，但 `triton-ascend` distribution/module/provider identity未被 Result证明，保持 targeted Evidence gap。
- Gate B STOP、first blocker字符串、exit 1、no wheel及 C/D未运行接受；这不是 Gate B或 Stage 1/2 PASS。
- Exact source和 matching-version official source都静态支持 `ascend910_93`并使用相同 metadata lookup infrastructure，故不把 failure提前归因成“未适配A3 OPP”或 source defect。
- Underlying root-cause confidence记录为 `LOW`；immutable Result缺 explicit confidence与 official PR compare-base字段错误只在 Review/STATUS/INDEX补充，不修改 Result。
- Next Task只定位/闭合 `aic-*-ops-info.ini` first blocker并补 Triton/provider Evidence；确认 source change必要前，Code repo/fork仍 `Not needed yet`。

## D-020 / D-021 / D-022 / D-023 / D-024 — Stage Acceptance and moving-head handoff

- Stage 1/2 Acceptance绑定 `7beda84f...` / tree `a81eea55...`、wheel SHA-256 `fa33f586...`、official A3 openEuler image和 accepted composite runtime pattern。
- Accepted scope为 A3-native wheel、standalone FL、PlatformFL import和 one real NPU custom-op smoke；不覆盖 full model、TP2/HCCL、graph、serve或performance。
- Runtime pattern作为已验证组合整体复用；未做逐项ablation，不声称每个flag/mount独立必要。Post-launch必须验证 `torch.npu.is_available()`和实际mapped device count；失败时不得安装FlagGems掩盖。
- Current tracked `e610a990...`比accepted source多 communicator/platform/model-runner changes，不自动继承 Acceptance。Stage 3同一Task先build/install current-head wheel并做bounded C/D regression，PASS后才加载模型。
- Stage 3仍需 explicit User dispatch和model identity gate；Codex2不得自动进入。
- Stage 3 Acceptance绑定`e610a990...` / tree `609ff1ad...`、wheel sha256 `2fcf788...`和完成后的26-shard BF16 model root；不自动覆盖future HEAD或变化后的model/runtime。
- Resume首次未带visible scope的invariant probe和stdin multiprocessing attempt均发生在正式model execution前；corrected scope/file-script完成完整exit 0 Gate E，因此不构成重复执行要求。
- Stage 4以FL-local GraphWrapper + eager FX + Ascend NPUGraph完成`[1,2,4,8]` both-rank capture与batch-1/2 replay，正式ACCEPTED；`npugraph_ex`、serve、automatic capture through 64和performance不在该Acceptance范围。
- Current tracked head已从Stage 3 Accepted `e610a990...` single-parent前进到`032fddc9...` / tree `463806ef...`。Bounded review证明只有README文档和新增build-config characterization tests，不改build implementation、Python/native runtime、graph、model或communicator语义。README由`pyproject.toml`引用为package metadata，故不声称rebuilt wheel字节级相同；Stage 4不rebuild也不使用该假设。
- Stage 3/4 Acceptance仍只绑定`e610a990...` wheel sha256 `2fcf788...`。`032fddc9...`只作为dispatch-time tracked identity记录；若Stage 5 dispatch时tracked HEAD/tree再次变化，在model/service mutation前STOP交回Codex1。

## D-030 — Frozen upstream validation baseline

User于2026-08-26主动将项目目标从“持续验证同事moving development branch”调整为“在A3/910C上完整验证一个固定、可复现的同事Qwen3.6 Ascend实现快照”。

Frozen Validation Baseline：

```text
source: e610a990d785356bf51a3cad50219d4c03310a31
tree: 609ff1ad0f08239f353cb4d8774e504b4deba03b
wheel: vllm_plugin_fl-0.2.0+ge610a990d-cp311-cp311-linux_aarch64.whl
wheel SHA-256: 2fcf788660f3fe42b364bc60d593ee1b9b634fc0632de58c444d961bff4aa1bd
last pre-change tracked reference: 032fddc91b6d013b98aed8e64ff05b54d1435648
last reference tree: 463806ef18e5e31006cd4f59e6a5261fc65cea4a
```

Stage 3/4/5 Acceptance正好绑定该runtime artifact和wheel，因此全部保持原样，无需因upstream变化重跑eager、graph或serve。`032fddc9...`已有Formal Review证明仅为README/docs+tests变化，只保留为冻结时最后的pre-change reference，不是wheel build source，不rebuild。

从Stage 6开始：

- feature branch、PR #404和official base只作历史/reference URL；
- 后续debug删除/增加、rebase、squash、history rewrite、PR冲突或branch移动一律`OUT OF SCOPE / IGNORE FOR EXECUTION`；
- 不live-query future tracked HEAD作为execution gate，不做moving-head diff/review，不因upstream变化STOP；
- 只对Frozen source/artifact、Accepted image/runtime、model、container/device mapping、cache、workload和config漂移STOP；
- Codex1/Codex2不得自行升级、rebuild或替换为新HEAD。

本项目最终结论明确适用于`e610a990... Frozen Validation Baseline`，不适用于“PR #404 future latest HEAD”。若未来需要验证新HEAD，必须由User另立new validation baseline/project evidence，不能改写或覆盖本项目Accepted Results。

## 明确拒绝的路线

- 没有 blocker时复制或重写大段 Qwen/vLLM-Ascend实现；
- silent downgrade vLLM/CANN/torch-npu、替换模型、绕过 PR #404；
- 把 A2 wheel直接装到 A3；
- 通过源码 `PYTHONPATH`、editable import或遗留 build目录伪装 wheel成功；
- 因 image名称含 vLLM-Ascend而自动判违规，或因静态无 import就自动判运行独立；最终结论必须来自 installed/runtime Evidence；
- schema/import smoke直接外推 model correctness；
- 基础 gate前运行完整 A2 1K/4K/16K/64K × C1/C8/C32/C64性能矩阵；
- 在 Qwen项目中启动 GLM model适配或把 Qwen-specific结论外推给 GLM。
