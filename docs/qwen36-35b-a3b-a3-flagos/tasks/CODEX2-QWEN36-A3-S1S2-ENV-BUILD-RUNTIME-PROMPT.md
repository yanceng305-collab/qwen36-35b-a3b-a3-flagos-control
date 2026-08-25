# Codex2 Prompt — QWEN36-A3-S1S2-ENV-BUILD-RUNTIME

状态：**Waiting User input / Not Ready。只有下列 User-confirmed facts填写并由 User明确下发后才执行。**

先同步并读取最新 `AGENTS.md`、`STATUS.md`、本 Task合同和 `REPOSITORY-AND-EVIDENCE-RULES.md`。GitHub moving state优先于旧 prompt/chat；dispatch前重新冻结 tracked branch的 exact HEAD SHA/tree和 clean state。

## User-confirmed facts

- A3/910C target/access：`<USER MUST CONFIRM>`
- Authorized logical device scope：`<USER MUST CONFIRM>`
- Approved base image/container route：`<USER MUST CONFIRM OR AUTHORIZE BOUNDED SELECTION>`
- Server work root / Evidence root / artifact-cache roots：`<USER MUST CONFIRM>`
- Container/image creation permission：`<USER MUST CONFIRM>`
- Dependency network or offline roots：`<USER MUST CONFIRM>`
- Optional model path inventory：`<OPTIONAL>`
- User dispatch：`<USER MUST DISPATCH>`

## Objective

在真实 A3/910C上完成：environment/device identity；current PR #404 tracked implementation exact source identity；clean `ascend910_93` wheel build；standalone wheel/site-packages FL install；无 installed/runtime `vllm-ascend`与无 source `PYTHONPATH`；A3 `_C_ascend`/OPP origin；最小实际 NPU custom-op/runtime smoke。

保持 `VLLM_PLUGINS=fl`、`USE_FLAGGEMS=0`。Control snapshot的 source计数是 8 OPP / 9 schemas；若 branch已更新，以 dispatch时 exact-head source盘点并保存 diff。显式记录 build/runtime effective `SOC_VERSION`、selected family/prebuilt root，验证 A3不是 A2 binary复用。

## Boundaries

只执行 Stage 1/2。不要运行完整模型、TP2 model、graph、serve、prefix、EP、64K或 benchmark；不要修改同事源码、创建 fork/Code repo、silent downgrade或改变 major tuple。普通检查、build/install/debug方法由你自主决定，不需要照抄固定命令。

## PASS / STOP

只有 environment/source、A3 wheel、standalone install和实际 A3 NPU custom-op四个 gate全部通过，且 Evidence/exit/checksum/三指针完整，才报告 `Execution PASS`。遇到 wrong tuple/source/family、A2 residue、extension/OPP/load失败、vllm-ascend依赖、source import、FlagGems activation、NPU failure/CPU fallback或需要源码/major route修改时，在 first attributable blocker STOP并保存 root-cause Evidence；不得继续完整模型。

生成 immutable Result并同步 `results/INDEX.md`，返回 exact run identity、environment、artifact、per-gate Evidence、last success、first blocker、Code PR=`N/A`或 Decision request。Execution PASS不等于 formal Acceptance，不得自行进入 Stage 3。
