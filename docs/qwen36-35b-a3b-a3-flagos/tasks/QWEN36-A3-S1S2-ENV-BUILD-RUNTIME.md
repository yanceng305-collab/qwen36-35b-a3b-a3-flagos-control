# QWEN36-A3-S1S2-ENV-BUILD-RUNTIME

状态：**Waiting User input / Not Ready**

执行代理：Codex2

目标 Stage：Stage 1 + Stage 2

## User-confirmed facts（Ready前必须填写）

- A3/910C execution target / access context：`<USER MUST CONFIRM>`
- Authorized logical device scope：`<USER MUST CONFIRM>`
- Approved base image/container route：`<USER MUST CONFIRM OR AUTHORIZE BOUNDED SELECTION>`
- Server project/work root：`<USER MUST CONFIRM>`
- Server Evidence root：`<USER MUST CONFIRM>`
- Artifact/cache roots and container/image creation permission：`<USER MUST CONFIRM>`
- Dependency access or offline artifact roots：`<USER MUST CONFIRM>`
- Optional model path for presence/identity inventory only：`<OPTIONAL; REQUIRED BEFORE STAGE 3>`
- User dispatch：`<NOT DISPATCHED>`

未填写关键字段并由 User明确下发前，本合同存在不等于 `Ready`，Codex2不得执行。

## Objective

在真实 Ascend A3/910C 上建立可审计的 environment/source identity，使用 dispatch时同事 tracked branch的 exact clean HEAD构建 A3-native FL wheel，完成 standalone wheel/site-packages安装，并用最小 NPU custom-op/runtime smoke证明 A3 family `_C_ascend`与 OPP可以在该 environment实际加载和执行。

本任务是进入完整 Qwen TP2 eager前的 environment/build/runtime gate。

## Project contribution

- 隔离 A2→A3最大未知：SoC family、CANN/ABI、wheel/OPP packaging与 standalone runtime。
- 为下一独立 Stage 3 eager Task提供 Accepted environment、wheel和 reconstruction输入。
- 为未来 GLM handoff产生候选通用 A3 build/runtime事实，但只有 Codex1 Acceptance后才能进入 validated handoff。

## Frozen project boundaries

- Implementation repo：`xiemingda-1002/vllm-plugin-FL`。
- Tracked branch：`feature/qwen3.6-35b-a3b-ascend-graph-migration`。
- Control创建时 snapshot：`7beda84f...` / tree `a81eea...`；**不是永久 run SHA**。
- Dispatch前必须从 GitHub重新查询并冻结 actual HEAD SHA/tree/clean state；branch变化时先由 Codex1审查 diff。
- Official base：`flagos-ai/vllm-plugin-FL:release/0.2`；Control snapshot `53adefb...` / tree `9ddfd0...`。
- vLLM baseline：`0.20.2`。
- vLLM-Ascend `0.20.2rc1`：reference only；最终 runtime dependency必须 absent。
- A3 build family：`ascend910_93`；不得复用 `ascend910b` A2 binary。
- Runtime：`VLLM_PLUGINS=fl`、`USE_FLAGGEMS=0`。

## Scope and permissions

允许：

- 在 User授权 server/device/container/work/Evidence范围内做 safe read-only inventory、创建本 Task专属 run/work/Evidence、checkout exact source、clean build、wheel inspection、package transaction、minimal NPU smoke与必要诊断。
- 卸载/替换 Task专属 container/environment中的旧 `vllm-plugin-fl`和 `vllm-ascend` distribution，以建立 final standalone runtime；不得改变 Host全局环境或其他项目容器。
- 记录 traceback、library/import/provider origin、build output、artifact checksum、device/CANN日志并缩小 first blocker。

禁止：

- 运行完整 Qwen model construction/weight load/generation、TP2 model HCCL、graph、serve、prefix、EP、64K、benchmark或性能调优。
- 修改同事 source、创建长期 undocumented server patch、创建 Code repo/fork/PR。
- 更换 vLLM major/minor baseline、CANN路线、模型、quantization或引入 full vLLM-Ascend runtime。
- 使用 A2 wheel、旧 build tree、source `PYTHONPATH`、editable/source-tree import绕过 formal install。
- 将静态 schema/import成功写成 A3 model PASS。

若 source修改被证明必需，保存 first blocker/root cause并 STOP，提交 `Decision requested`；后续由独立 bounded Code task授权。

## Ready gate

1. User-confirmed facts齐备并明确 dispatch。
2. A3 server/device可安全使用，无未知冲突 owner；write/device/container权限清楚。
3. Approved base/container route足以核对 vLLM 0.20.2与 matched CANN/torch-npu/Triton，或 User明确授权在候选中做 bounded selection；major tuple变化不得自决。
4. Server work/Evidence/artifact/cache位置已批准；不会污染 GLM或其他项目状态。
5. GitHub/current implementation已重新查询；run source SHA/tree已冻结，worktree可验证 clean。
6. 依赖获取路径已知；离线 CATLASS若使用，必须绑定 `41bf90da655bba3c66d0acd7e00abe33960ecfd6`。

## Sequential gates

### Gate A — A3 environment and source identity

- 证明 physical device是 A3/910C并记录 logical mapping、count、health/owner边界。
- 记录 driver、firmware、CANN toolkit/runtime/OPP、Python/arch、torch、torch-npu、vLLM、Transformers、Triton/provider、HCCL、compiler/build tools、base image digest/ID、mount/device mapping。
- 记录 tracked repo/branch、remote、exact SHA、tree、clean/dirty和 compare/base identity。
- 记录 build/runtime effective `SOC_VERSION`来源和值；不能机械照搬 A2 `ascend910b`。

### Gate B — clean A3-native wheel

- 使用 clean source/isolated builder，不混入 A2或旧 ABI build outputs。
- 构建输出必须属于 `ascend910_93` family，并保存 detection/build logs、wheel filename/hash、Python ABI和完整 inventory。
- 以 dispatch时 exact-head source为准核对 expected OPP/schema count；Control snapshot期望 8 OPP / 9 schemas。tracked head若变化必须保存差异，不可机械冻结旧计数。
- 证明 wheel不含 A2 `prebuilt/ascend910b1` binary residue，不依赖 whole external vLLM-Ascend runtime tree。

### Gate C — formal standalone install

- 在 source tree之外以 wheel完成 final install；记录 package transaction与新的 Python process结果。
- `vllm_fl.__file__`、distribution metadata、extension/OPP origin必须来自实际 wheel/site-packages。
- 不得通过 `PYTHONPATH`/editable/source checkout加载。
- installed `vllm-ascend` distribution与 `vllm_ascend` module/entrypoint必须 absent；runtime不得 import/call它。
- `VLLM_PLUGINS=fl`、`USE_FLAGGEMS=0`生效；记录实际 platform/dispatch/provider，不出现 FlagGems activation。
- 记录 selected `prebuilt/ascend910_93` root、loaded `_C_ascend` path、OPP kernel/config root和 `ASCEND_CUSTOM_OPP_PATH`。

### Gate D — minimal A3 NPU runtime/custom-op smoke

- 在授权 logical device上执行一个或多个与 packaged `_C_ascend`/OPP匹配的最小安全 custom-op smoke。
- 输入/output device、dtype/shape、reference/assertion、synchronize、exit code、loaded operator/library/OPP origin必须明确。
- PASS必须证明 op实际在 A3 NPU执行且输出 finite/correct，无 silent CPU fallback；schema可见或 import成功本身不够。

## PASS

只有 Gate A–D全部满足，Codex2才可报告 `Execution PASS`：

1. exact A3 environment、device、container和 source identity完整；
2. actual run source clean且与冻结 SHA/tree一致；
3. wheel为 A3-native `ascend910_93`，无 A2 binary residue，hash/inventory完整；
4. standalone site-packages install成立，无 source shortcut和 installed/runtime `vllm-ascend`依赖；
5. FL platform/runtime/dispatch identity成立，`USE_FLAGGEMS=0`；
6. A3 `_C_ascend`与 OPP实际加载，最小 NPU custom-op完成正确性/device/no-fallback assertion；
7. raw Evidence、checksum、immutable Result draft与三指针齐全。

`Execution PASS`不自动解锁 Stage 3；Codex1独立审查后才能写 formal Acceptance。

## STOP

在 first attributable blocker立即停止后续 gate并保存 Evidence，包括但不限于：

- wrong/unknown hardware、device owner或越权风险；
- wrong base image、vLLM、CANN、torch-npu、Triton/provider或不可批准的 tuple变化；
- tracked branch/source identity漂移、dirty source或无法冻结 SHA/tree；
- A3 family detection/build失败，或错误使用 `ascend910b`/A2 binary；
- wheel缺少 expected `_C_ascend`、A3 OPP、正确 ABI或包含错误 runtime payload；
- extension/OPP加载或 registration失败；
- final runtime依赖 installed/imported `vllm_ascend`；
- `vllm_fl`来自 source tree、editable或 `PYTHONPATH`；
- FlagGems意外激活；
- NPU custom-op失败、CPU fallback、NaN/Inf、device/ABI/CANN错误；
- 需要源码修改、major route/version/CANN/baseline change或大范围重构。

STOP Result必须写 last successful gate、first blocker、root-cause confidence、最小复现、raw logs和建议的 bounded follow-up；不得继续完整模型来“绕过”基础失败。

## Required Evidence

- Task ID / run ID / timestamps / executor；
- User-confirmed dispatch inputs与实际授权 scope；
- hardware/device/owner/container/image/environment manifest；
- package/version/provider/library/HCCL/build-tool inventory；
- repo/remote/branch/HEAD/tree/status/diff-base identity；
- effective `SOC_VERSION`、detected family、selected prebuilt root；
- build logs/exit codes/wheel path/hash/inventory/ABI/8-OPP-or-current-head reconciliation；
- uninstall/install logs、new-process distribution/module/entrypoint checks；
- `vllm_fl`/`_C_ascend`/OPP/library origin、`PYTHONPATH`、plugin/platform/dispatch/provider trace；
- custom-op command/config/input/output/reference/device/sync/exit/no-fallback Evidence；
- cache roots/identity与是否 clean/reused；
- per-gate last success / first blocker；
- Code/source、Control、Evidence三指针；Code PR正常应为 `N/A`；
- Evidence manifest与 checksums。

## Result return contract

Codex2返回：

- actual status：`completed / partial / blocked / failed`；
- Execution：`PASS / STOP / PARTIAL`；
- actual artifacts/changes与未产生的预期产物；
- 每个 PASS标准到 Evidence pointer映射；
- exact source SHA/tree/clean state与 environment tuple；
- deviations/new facts/invalid assumptions；
- first blocker/root cause/scope if any；
- Code change/PR=`N/A`或明确 Decision request；
- immutable Result path/commit、results index sync与三指针；
- 对下一 Stage/plan的建议，但不得自行进入 Stage 3。
