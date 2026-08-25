# QWEN36-A3-S1S2-ENV-BUILD-RUNTIME

状态：**ENDED / STOP at Gate B / Formal Review NEEDS-FOLLOWUP — historical parent contract; DO NOT RESUME**

执行代理：Codex2

目标 Stage：Stage 1 + Stage 2

本合同已经执行并结束。Immutable Result见 [`RESULT-QWEN36-A3-S1S2-ENV-BUILD-RUNTIME-20260825T205424+0800.md`](../results/RESULT-QWEN36-A3-S1S2-ENV-BUILD-RUNTIME-20260825T205424+0800.md)，Codex1 Review见 [`REVIEW-QWEN36-A3-S1S2-ENV-BUILD-RUNTIME-20260825.md`](../reviews/REVIEW-QWEN36-A3-S1S2-ENV-BUILD-RUNTIME-20260825.md)。不得通过重新发送旧 prompt续跑；follow-up使用新 Task。

## User-confirmed facts and bounded authorization

### A3 target and device

- 允许 Codex2在当前项目可访问的 Ascend A3/910C server执行本 Task。
- 开始时只读检查 physical model、logical mapping、health、occupancy、process/owner和当前冲突。
- 在不干扰其他任务的前提下，Codex2自主选择最小安全 device scope；Stage 1/2 custom-op smoke不得为了未来 TP2提前占更多 device。
- 禁止 kill、pause、reset、preempt、抢占或修改其他任务。若 target不唯一、access无效或 owner边界不清，在任何 mutation前 STOP并请求 User澄清。

### Official A3 base image route

只允许在以下 official vLLM-Ascend `v0.20.2rc1` A3 candidates中 bounded selection：

```text
quay.io/ascend/vllm-ascend:v0.20.2rc1-a3
quay.io/ascend/vllm-ascend:v0.20.2rc1-a3-openeuler
```

- Codex2先做 safe host/device/driver/container inventory，再按 actual compatibility选择 A3 Ubuntu或 A3 openEuler。
- Official matched tuple：vLLM 0.20.2；Python `>=3.10,<3.12`（两条 A3 Dockerfile均为 py3.11 base）；CANN 9.0.0；torch/torch_npu 2.10.0/2.10.0；Triton Ascend 3.2.1。
- Ordinary `quay.io/ascend/vllm-ascend:v0.20.2rc1`是 official A2 Ubuntu route，明确排除。A2/openEuler、nightly、其他 vLLM/CANN/version或 unpinned substitute也不允许作为 fallback。
- 如果两个 official A3 candidates均存在实质不兼容或不可用，STOP并返回 `Decision requested`；不得自行扩大 route。
- 必须记录 selected tag、resolved image digest、platform digest（如可区分）、local image ID、OS、Python、CANN、torch、torch_npu、vLLM、Triton和选择理由。

Base image只作为 A3 software environment/build carrier。其名称或初始 package/source状态不改变 final runtime ownership。

### Default single-container execution closure

本 Task默认复用同事已验证思路，在**一个 Task专属 container**内完成闭环：

```text
selected official A3 v0.20.2rc1 image
  -> one Task-specific container
  -> checkout tracked exact HEAD
  -> clean build A3 ascend910_93 wheel in that container
  -> leave the FL source tree
  -> uninstall vllm-ascend / old vllm-plugin-fl
  -> install the newly built wheel
  -> verify standalone FL from site-packages in a new Python process
  -> load _C_ascend / OPP
  -> run the real A3 NPU custom-op smoke
```

不要求独立 Builder Container + Runtime Container。本文中的 clean/isolated build只表示在该 Task container内使用 Task专属 clean source/build workspace，不表示另建 builder container。只有实际 Evidence证明单容器无法满足当前某个 gate时，才允许 STOP后提出额外 container层，并明确被阻塞的 gate、原因、新变量和验证成本；不得在本 Task内自行扩层。

### Server roots and container

- Codex2可在现有 `/data`空间选择并创建新的 Qwen A3 Validation专属 `work`、`Evidence`、`artifacts`、`cache` roots；可以参考 `/data/tiankuan/zyg/FL/`。
- 新 roots必须与 GLM和其他任务隔离、不覆盖已有目录、不写入模型目录；Result返回 exact paths。
- 允许 pull/inspect上述 official A3 images、创建一个 Task专属 container并在其中完成 build/install/smoke闭环。失败过程中产生的无价值临时 container可以停止/删除。
- 如果 Gate A–D全部通过，必须保留最终 PASS container、其中已安装的 standalone FL环境以及本次 wheel/必要 artifacts，等待 `Execution PASS → Codex1 formal Acceptance → User dispatch Stage 3`，并在该已验证环境基础上继续 TP2 BF16 eager。除非后续有明确 Decision，不得删除或重建该 PASS环境。
- 禁止修改其他项目 container、删除其他 image、修改 Host全局 Python/CANN环境。长期 runtime image snapshot不是 Stage 1/2默认产物，后置 Stage 8。

### Dependency access

- 允许使用服务器现有 GitHub、package index、container registry和 CATLASS获取能力。
- 网络不可用时可检查已有离线 artifacts，但每个离线依赖必须核验 identity/version/checksum或等价来源证据。
- CATLASS如需要，必须绑定 exact commit `41bf90da655bba3c66d0acd7e00abe33960ecfd6`；不得使用版本不明替代物。

### Model state — inventory only

- Model：`Qwen/Qwen3.6-35B-A3B`。
- dtype：BF16，non-quantized。
- User-confirmed path：`/data/tiankuan/zyg/FL/workspace/Qwen3.6-35B-A3B`。
- Current state：`DOWNLOADING / NOT YET READY FOR STAGE 3`。

模型不是本 Task执行依赖。Stage 1只允许只读检查 path存在性并记录 download/inventory状态；不得等待下载完成、加载模型、检查完整权重作为 PASS条件、运行 TP2或 generation。模型 downloading **不阻塞 Stage 1/2**。

Future Stage 3前必须用独立 model identity gate核对 config、architecture、tokenizer、`model.safetensors.index.json`、全部 shards、size、checksum/manifest和 BF16/no-quantization contract；本轮不执行。

### Dispatch record

- Original User dispatch：completed；run ended `STOP at Gate B`。

## Objective

在真实 Ascend A3/910C上建立可审计的 environment/source identity，使用 dispatch时同事 tracked branch的 exact clean HEAD构建 A3-native FL wheel，完成 standalone wheel/site-packages安装，并用最小真实 NPU custom-op/runtime smoke证明本次 A3 family `_C_ascend`与 OPP可以在 selected official A3 environment实际加载和执行。

## Frozen project boundaries

- Implementation repo：`xiemingda-1002/vllm-plugin-FL`。
- Tracked branch：`feature/qwen3.6-35b-a3b-ascend-graph-migration`。
- Control snapshot：`7beda84f...` / tree `a81eea...`；不是永久 run SHA。Dispatch前重新查询并冻结 actual HEAD SHA/tree/clean state；变化时先由 Codex1审查 diff。
- Official base：`flagos-ai/vllm-plugin-FL:release/0.2`；Control snapshot `53adefb...` / tree `9ddfd0...`。
- vLLM baseline：`0.20.2`；vLLM-Ascend `0.20.2rc1`提供 matched reference/carrier，final runtime dependency必须 absent。
- A3 build family：`ascend910_93`；不得复用 `ascend910b` A2 binary/build tree。
- Final runtime：`VLLM_PLUGINS=fl`、`USE_FLAGGEMS=0`、standalone FL wheel/site-packages。

## Scope and permissions

允许在上述 bounded范围内：safe inventory、创建 Task专属 roots/run/container、checkout exact source、clean build、wheel inspection、package transaction、minimal NPU smoke和必要诊断；记录 traceback、library/import/provider origin、build output、artifact checksum、device/CANN日志并缩小 first blocker。

禁止：

- 完整 Qwen model construction/weight load/generation、TP2 model/HCCL execution、`FULL_DECODE_ONLY` graph、serve、prefix、EP、64K、benchmark、profiling/performance或 GLM。
- 修改同事 source、创建长期 undocumented patch、fork、Code repo/branch/PR。
- silent downgrade、切换 vLLM、随意切换 CANN、使用 nightly或扩大 official A3 image候选。
- 使用 A2 wheel、`prebuilt/ascend910b1` binary、旧 A2 build tree或让 A2 residue进入 A3 wheel。
- 使用 FL source `PYTHONPATH`、editable/source-tree import绕过正式 wheel；官方 carrier中 vLLM自身安装方式必须记录，但 final `vllm_fl`必须是 non-editable wheel/site-packages origin。
- 把 import/schema可见写成 Gate D或 model PASS。

如源码修改被证明必需：保存 first blocker/root-cause Evidence，STOP并提交 bounded fix proposal / `Decision requested`；不得直接 patch后继续。

## Ready gate

Task contract已 Ready。执行开始仍要求：

1. User明确 dispatch；Codex2已同步 latest Control和 GitHub moving state。
2. 当前可访问 A3 target可唯一识别；只读 inventory不显示 owner冲突或越权风险。
3. 所有 mutation保持在 Codex2现场选择的新 Task专属 roots/container/device scope内。
4. Base选择不越过两个 official A3 candidates；major tuple或 fallback变化必须 User决定。

## Sequential gates

### Gate A — Environment, image and source identity

- 证明 physical device是 A3/910C并记录 logical mapping、count、health/owner和 selected minimal scope。
- 在两个 official A3 candidates内选择 compatible route；记录 selected tag、image ID/digest、OS、Python、CANN toolkit/runtime/OPP、torch、torch_npu、vLLM、Transformers、Triton/provider、HCCL、compiler/build tools和选择理由。
- 记录 container、mount/device mapping、Task exact roots和 cache identity。
- 记录 tracked repo/branch/remote、exact SHA、tree、clean/dirty和 compare/base identity。
- 记录 physical SoC、build/runtime effective `SOC_VERSION`、detection source、normalized family和 selected prebuilt root；不得机械设置 A2 `ascend910b`。
- 模型只记录 path存在性与 `DOWNLOADING`/inventory状态，不等待或验证完整性。

### Gate B — Clean A3-native wheel in the same Task container

- 在同一个 Task container内使用 Task专属 clean source/build workspace，不混入 A2或旧 ABI outputs；`isolated`不表示另建 builder container。
- 构建输出属于 `ascend910_93` family；保存 detection/build logs、wheel filename/hash、Python ABI和完整 inventory。
- Control snapshot的 8 OPP / 9 schemas仅供参考。必须按 dispatch exact HEAD source重新盘点 actual expected set/count，记录与 PR prose旧 7/8的差异；当前已知新增包括 `apply_top_k_top_p_custom` / `npu_apply_top_k_top_p`。
- 证明 wheel没有 `prebuilt/ascend910b1`或其他 A2 binary residue，不依赖 whole external vLLM-Ascend runtime tree。

### Gate C — Formal standalone FL install in the same Task container

- 在完成 build的同一 Task container内离开 FL source tree，卸载 `vllm-ascend`和 old `vllm-plugin-fl`，安装本次 wheel，并在新的 Python process验证。
- `vllm_fl.__file__`、distribution metadata、`_C_ascend`/OPP origin来自实际 wheel/site-packages；无 FL `PYTHONPATH`、editable或 source checkout shortcut。
- `vllm-ascend` distribution absent；`vllm_ascend` module/entrypoint不可 import；runtime无 `vllm_ascend` import/call/dependency。
- `VLLM_PLUGINS=fl`、`USE_FLAGGEMS=0`生效；记录 actual PlatformFL/dispatch/provider，无 FlagGems activation。
- 记录 selected `prebuilt/ascend910_93` root、loaded `_C_ascend` path、OPP kernel/config root和 `ASCEND_CUSTOM_OPP_PATH`。

### Gate D — Minimal real A3 NPU custom-op smoke

- 执行至少一个与本次 wheel packaged `_C_ascend`/OPP匹配的最小安全 custom-op。
- 明确 input/output A3 NPU device、dtype/shape、reference/assertion、synchronize、numeric exit code、operator/library/OPP origin。
- PASS必须是 finite/correct且无 silent CPU fallback；import/schema可见本身不能算 Gate D PASS。

## PASS

只有 Gate A–D全部成立才允许报告 `Execution PASS`：

1. actual A3、safe device、selected official A3 image和完整 tuple/source identity；
2. exact clean run source和 `ascend910_93` A3-native wheel；hash/inventory完整且无 A2 residue；
3. standalone FL site-packages install，无 installed/runtime `vllm-ascend`和 FL source shortcut；
4. FL ownership、`USE_FLAGGEMS=0`、A3 `_C_ascend`/OPP origin成立；
5. 至少一个实际 A3 NPU custom-op通过 device/reference/sync/exit/no-fallback断言；
6. raw Evidence、checksums、immutable Result draft和三指针完整；最终 PASS container name/ID和 preserved状态已记录。

模型下载状态不属于 PASS判断。`Execution PASS`不自动解锁 Stage 3；等待 Codex1 formal Acceptance。

## STOP

在 first attributable blocker处停止，包括但不限于：

- hardware不是预期 A3、target/device owner不明确或需要干扰其他任务；
- 两个 official A3 images均不可用/不兼容，或需要 A2、nightly、其他 vLLM/CANN route；
- wrong vLLM/CANN/torch_npu/Triton/provider；
- tracked source异常、漂移、dirty或无法冻结 identity；
- A3 family build失败、wheel/ABI错误、A2 residue；
- `_C_ascend`/OPP load/registration失败；
- final runtime仍依赖/import `vllm_ascend`，或 `vllm_fl`来自 source/editable/PYTHONPATH；
- FlagGems activation；NPU op失败、CPU fallback、NaN/Inf、ABI/CANN错误；
- 需要源码修改、major route/version/CANN/baseline change或大范围重构。

STOP Result必须保存 last successful gate、first blocker、raw Evidence、root-cause confidence、最小复现和 bounded follow-up建议；不得继续完整模型绕过基础失败。

## Required Evidence

- Task/run/timestamps/executor和 explicit dispatch；
- actual target/device/owner/container/roots authorization scope；
- selected image tag/ID/digest/OS/tuple/selection reason，以及 single Task container name/ID/lifecycle；
- hardware/driver/firmware/CANN/package/provider/HCCL/build-tool manifest；
- repo/remote/branch/HEAD/tree/status/diff-base identity；
- physical SoC、effective `SOC_VERSION`、detection source、family/prebuilt root；
- build logs/exit codes/wheel path/hash/ABI/inventory和 exact-head OPP/schema reconciliation；
- install/uninstall logs和 new-process distribution/module/entrypoint checks；
- `vllm_fl`/`_C_ascend`/OPP/library origin、FL `PYTHONPATH`/editable audit、plugin/platform/dispatch/provider trace；
- custom-op config/input/output/reference/device/sync/numeric exit/no-fallback Evidence；
- model path existence/download-state inventory only；
- cache roots/identity、last successful gate/first blocker、Evidence manifest/checksums；PASS时记录 container与 standalone FL环境保留位置；
- Code/source、Control、Evidence三指针；Code PR正常应为 `N/A`。

## Result return contract

返回 actual status与 `PASS / STOP / PARTIAL`、exact identities/tuple/artifacts、每个 gate的 Evidence mapping、deviations/new facts/first blocker、Code PR=`N/A`或 Decision request、immutable Result/INDEX sync和三指针。可建议下一步，但不得自行进入 Stage 3。
