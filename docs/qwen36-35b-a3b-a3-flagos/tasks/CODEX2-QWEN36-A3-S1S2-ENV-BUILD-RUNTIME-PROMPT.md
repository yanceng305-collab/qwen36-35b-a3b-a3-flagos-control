# Codex2 Prompt — QWEN36-A3-S1S2-ENV-BUILD-RUNTIME

状态：**DO NOT DISPATCH — historical parent prompt。Run已在 Gate B STOP并完成 Formal Review；不得用本 prompt续跑。**

Follow-up只能使用 `QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG`及其 self-contained prompt。

同步并读取 latest `AGENTS.md`、`README.md`、`STATUS.md`、本 Task合同、`BASELINE.md`、`OFFICIAL-A3-IMAGE-ROUTE.md`和 `REPOSITORY-AND-EVIDENCE-RULES.md`。GitHub moving state优先于旧 prompt/chat；dispatch时重新冻结 tracked branch exact HEAD SHA/tree/clean state。

## User-authorized execution boundary

- Target：当前项目可访问的 A3/910C server。若 target不唯一或 access无效，在 mutation前 STOP请求澄清。
- Device：先只读检查型号、mapping、health、occupancy和 owner，再选择不干扰其他任务的最小安全 scope。不得 kill/pause/reset/preempt；不要为未来 TP2提前占卡。
- Base image只允许二选一：
  - `quay.io/ascend/vllm-ascend:v0.20.2rc1-a3`
  - `quay.io/ascend/vllm-ascend:v0.20.2rc1-a3-openeuler`
- 先按 actual host/driver/CANN/build/runtime compatibility选择 Ubuntu或 openEuler，记录 selected tag、digest、image ID、OS、Python、CANN、torch、torch_npu、vLLM、Triton和理由。普通无后缀 `v0.20.2rc1`是 A2 route，明确排除；两候选均不兼容则 STOP/Decision requested，不得改用 A2、nightly或其他版本。
- 默认使用一个 Task专属 container完成最短闭环：selected official A3 image → checkout tracked exact HEAD → container内 clean build `ascend910_93` wheel → 离开 FL source tree → uninstall `vllm-ascend`/old `vllm-plugin-fl` → install本次 wheel → new Python process验证 site-packages standalone FL → `_C_ascend`/OPP → real A3 NPU custom-op smoke。不要默认拆成 Builder Container + Runtime Container；clean/isolated只表示同一 container内的 Task专属 clean source/build workspace。只有真实 Evidence证明单容器无法满足当前 gate时，才 STOP并提出额外 container层与理由。
- Roots：在现有 `/data`创建不覆盖既有内容、与 GLM/其他任务隔离的 Qwen Validation专属 work/Evidence/artifacts/cache roots，返回 exact paths；不要写入模型目录。
- Container：可 pull/inspect上述 images并创建一个 Task专属 container。失败过程中的无价值临时 container可清理；Gate A–D全部 PASS后必须保留最终 container、standalone FL环境、wheel和必要 artifacts，等待 Codex1 Acceptance及 User dispatch Stage 3后在该环境继续 TP2 BF16 eager。无明确 Decision不得删除/重建 PASS环境；不得改其他 container/image或 Host全局 Python/CANN。长期 image snapshot后置 Stage 8。
- Dependencies：允许现有 GitHub/package index/registry/CATLASS；离线 artifact必须可核验。CATLASS绑定 `41bf90da655bba3c66d0acd7e00abe33960ecfd6`。

## Model fact — non-blocking inventory only

```text
Model: Qwen/Qwen3.6-35B-A3B
dtype: BF16 non-quantized
path: /data/tiankuan/zyg/FL/workspace/Qwen3.6-35B-A3B
state: DOWNLOADING / NOT YET READY FOR STAGE 3
```

Stage 1/2只可只读记录 path存在/download状态；不要等待、加载、核验完整 shards、运行 TP2或 generation。Downloading不阻塞本 Task。Stage 3前另建 model identity gate。

## Objective

只完成：A3 environment/device identity；dispatch exact PR404 source identity；clean `ascend910_93` wheel build；standalone FL wheel/site-packages install；absent installed/runtime `vllm-ascend`；无 FL source `PYTHONPATH`/editable shortcut；本次 A3 `_C_ascend`/OPP family/origin/load；至少一个最小真实 A3 NPU custom-op smoke。

保持 `VLLM_PLUGINS=fl`、`USE_FLAGGEMS=0`。Control的 8 OPP / 9 schemas只是 snapshot；按 dispatch exact HEAD重新盘点 actual set/count并记录旧 PR prose差异，包含当前已知 `apply_top_k_top_p_custom` / `npu_apply_top_k_top_p`。记录 physical SoC、effective `SOC_VERSION`、detection source、selected family/prebuilt root和 A2-residue audit。

## Boundaries

不要运行完整模型、TP2 model/HCCL、graph、serve、prefix、EP、64K、benchmark/profiling/performance或 GLM；不要修改同事源码、创建 fork/Code repo/branch/PR、silent downgrade或切换 vLLM/CANN；不要在无 blocker时增加额外 container/environment、artifact handoff或 build/runtime分层。需要源码修改或额外 container层时保存 first blocker/root-cause Evidence并 STOP，返回 bounded proposal / Decision requested。

## PASS / STOP / Return

只有 Gate A environment/image/source、Gate B A3-native wheel、Gate C standalone FL install和 Gate D实际 A3 NPU custom-op全部通过，且 Evidence/exit/checksum/三指针完整，才报告 `Execution PASS`。Import/schema可见不能替代 Gate D；模型状态不参与 PASS。

遇到 owner/权限风险、两个 A3 image都不兼容、wrong tuple/source/family、A2 residue、wheel/ABI/`_C_ascend`/OPP失败、final `vllm_ascend`依赖、FL source import、FlagGems activation、NPU failure/CPU fallback或需要源码/major route修改时，在 first attributable blocker STOP。保存 last successful gate、raw Evidence、root-cause confidence、最小复现和 bounded follow-up；不得跑完整模型绕过。

生成 immutable Result并同步 `results/INDEX.md`，返回 selected image与environment、single Task container name/ID、exact source/wheel/SoC identities、Task exact roots、per-gate Evidence、model download-state inventory、last success/first blocker、Code PR=`N/A`或 Decision request。PASS时明确 container/standalone FL/wheel已保留供 Acceptance和 Stage 3续用。Execution PASS不等于 formal Acceptance，不得自行进入 Stage 3。
