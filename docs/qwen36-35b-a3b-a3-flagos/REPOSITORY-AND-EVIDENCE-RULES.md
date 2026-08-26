# Repository, Evidence, Result and Acceptance Rules

## Repository roles

- Control：本仓库，保存 contracts、pointers、immutable Result、Acceptance和 handoff。
- Implementation：Frozen source `e610a990d785356bf51a3cad50219d4c03310a31` / tree `609ff1ad0f08239f353cb4d8774e504b4deba03b`及Accepted wheel是Stage 6+ source of truth；feature branch/PR #404仅作历史reference。
- Validation Code：当前 N/A；只有 blocker-backed Code Decision后创建。
- Server Evidence：原始 logs/manifests/checksums/wheels/runtime facts；不等于长期 source working tree。

## Frozen baseline and run freeze

Stage 6及以后正式执行固定使用：

```text
source: e610a990d785356bf51a3cad50219d4c03310a31
tree: 609ff1ad0f08239f353cb4d8774e504b4deba03b
wheel: vllm_plugin_fl-0.2.0+ge610a990d-cp311-cp311-linux_aarch64.whl
wheel SHA-256: 2fcf788660f3fe42b364bc60d593ee1b9b634fc0632de58c444d961bff4aa1bd
last pre-change tracked reference: 032fddc91b6d013b98aed8e64ff05b54d1435648
```

正式run只核对Frozen source/artifact与实际installed/runtime identity。Feature branch、PR #404或official base的future变化不再查询为entry gate，不触发STOP、diff、moving-head review或rebuild。Result只对Frozen baseline及该次actual runtime/environment/workload identity有效。

Dirty working tree如果影响 source/build/runtime，只能作为 exploratory诊断，不能形成正式 PASS/Acceptance。若 dirty仅为明确列出的 non-source Evidence output，必须保存 diff/status并由 Codex1裁定。

## One task, three pointers

每个正式 run必须保留：

1. **Code/source pointer**：repo、Frozen SHA/tree、wheel filename/hash/origin、last pre-change reference、Code PR或 `N/A`。
2. **Control pointer**：task、immutable Result路径/commit、results index/Acceptance commit。
3. **Evidence pointer**：server absolute path、manifest、checksum/index、raw log入口。

## Server Evidence minimum

每个正式 Result至少能定位：

- Task ID、run ID、UTC/local timestamps；
- Frozen source/tree、Accepted wheel filename/hash/origin及last pre-change tracked reference；
- environment、image/container、device、model path/identity；
- command/effective config和关键环境变量；
- per-gate exit code、last successful gate、first attributable blocker；
- raw stdout/stderr/traceback；
- wheel/artifact/package/cache manifest与 checksums；
- import/module/distribution/library/provider origins；
- Code/source、Control、Evidence三指针。

如果 Task不运行模型，model字段可写 `N/A / inventory only`，但不能伪造 model验证。

## Evidence directory shape

推荐服务器结构：

```text
<EVIDENCE_ROOT>/<TASK_ID>/<RUN_ID>/
  manifest.md or manifest.json
  commands/
  logs/
  environment/
  source/
  artifacts/
  checksums.txt
  result-draft.md
```

实际根目录由 User/服务器约束决定；Control只保存稳定绝对 pointer和 checksum，不上传大 wheel/log/model。

## Immutable Result

Codex2首次 push的 run snapshot不可修改。错误或补证使用新的 supplement/follow-up Result，并在 `results/INDEX.md`追加 pointer；不得重写原 run事实。

Result必须区分：

- `completed / partial / blocked / failed / cancelled / superseded`；
- Execution `PASS / STOP / PARTIAL`；
- Control Sync `SYNCED / PENDING`；
- Codex1 Acceptance `PENDING / ACCEPTED / REJECTED / NEEDS-FOLLOWUP`。

## Formal Acceptance

Codex1独立审查：task contract、exact identities、Evidence completeness、first blocker、Code diff/PR或 N/A、claim boundary与未覆盖项。Acceptance只更新 `results/INDEX.md`或 `STATUS.md`，不修改 immutable Result。

`ACCEPTED`必须写明环境、版本、run、SHA/tree、覆盖 scope和重验条件。没有 A3 field execution不得写 A3 Acceptance。

## Upstream updates and new-baseline rule

Stage 6起upstream branch/PR/base更新一律不进入本项目execution planning。Codex1不得审查future diff，Codex2不得切换或rebuild future HEAD。若User未来要求验证新HEAD，必须先建立新的validation baseline/project evidence，并保留本项目Frozen Results不变；不得用新HEAD覆盖、继承或改写本项目Acceptance。

## Code fixes

发生 A3 blocker时先保存 root-cause Evidence。需要修改Frozen production source或采用upstream新commit时必须先由User发布new baseline Decision，再建立bounded Code task。任何Code change都必须有branch、diff、tests、commit/PR、rollback与source pointer；Codex1/Codex2不得自行改变Frozen baseline。

## Reconstruction discipline

从 Stage 1开始记录 environment/container/build/install/cache/device/Evidence方法，但只有 Accepted事实进入 [reconstruction](reconstruction/README.md)和 [A3-RUNTIME-HANDOFF.md](A3-RUNTIME-HANDOFF.md)。Host-local image/tag不是 portable artifact；必须同时记录 image ID/digest、build inputs、mount/device、source和 external dependency。

## Final A3 end-to-end reproduction rule

从 Stage 6开始及其后的每个 execution Result，都必须保存足够原材料，使项目完成后不依赖 ChatGPT、Codex1、Codex2聊天记录或某个执行者的个人记忆，仅依靠：

1. Control repo；
2. implementation exact source identity；
3. preserved artifacts；
4. preserved Server Evidence；

即可编写并执行最终 [`reconstruction/A3-END-TO-END-REPRODUCTION.md`](reconstruction/README.md)。现在不提前冻结尚未完成的 functional/performance/prefix/EP2合同；项目收尾时再从 Accepted Results、Formal Reviews、A2-to-A3 delta、reconstruction和 preserved Evidence生成完整文档。

最终文档必须在开头明确：

```text
This document reproduces the frozen validation baseline:
e610a990d785356bf51a3cad50219d4c03310a31
tree: 609ff1ad0f08239f353cb4d8774e504b4deba03b
wheel SHA-256: 2fcf788660f3fe42b364bc60d593ee1b9b634fc0632de58c444d961bff4aa1bd
```

不得自动替换为PR #404 future HEAD。Future HEAD validation必须建立新的baseline/project evidence，不能覆盖本项目文档或结果。

最终文档必须能让一个新执行者从环境准备一路复现到最终Accepted功能/性能范围，至少覆盖：hardware/device family、image exact tag/digest/ID、Driver/CANN/Python/torch/torch_npu/vLLM/Triton版本、implementation repo/branch/SHA/tree、official base/PR、build target/prerequisites、wheel identity/inventory、standalone FL install、no vllm-ascend/no FlagGems、container invocation/mount/device mapping、NPU invariant、model identity、env、cache isolation、eager/graph/serve/API、functional matrix、performance workload、prompt/token generation、sampling、warm-up/cache/cold-start、concurrency/input/output、prefix/EP2、success criteria、known pitfalls、A2-to-A3 delta、A3-specific source changes和 final accepted limitations。

## Stage 6+ Result minimum for reproduction

大体积 raw log、wheel、model或cache不要求commit进Control，但 Result和 preserved Evidence至少必须满足以下合同。不能只写“按Stage 5配置运行”或依赖聊天补充。

### Identity

- exact Task ID、run ID、timestamps、Control parent/current commit；
- Frozen source/tree、last pre-change tracked reference、actual runtime artifact/wheel identity；
- wheel filename/SHA-256/inventory；image tag/manifest/platform digest/image ID；
- runtime tuple、host/device family和 exact visible-device scope。

### Commands and configuration

- actual docker/container invocation，或Accepted reconstruction pointer加全部task-specific substitutions；
- exact env artifact、model/serve/start/benchmark command、config JSON、port和所有关键non-default参数；
- TP/DP/EP、dtype/quantization、graph mode/capture sizes、max-model-len/max-num-seqs/max-num-batched-tokens；
- prefix/chunked-prefill/async、sampling、concurrency、input/output、warm-up和cache state。

### Workload generation

Functional/performance Result必须保存prompt生成方式、random seed、tokenizer identity、exact token-length检查方式、salted/independent prompt规则、num prompts、concurrency、request order、output length和sampling contract，使新执行者能生成同类型workload。

### Result

- pass/fail、per-cell/per-request结果和HTTP/API状态；
- token/output correctness、finite/corruption/repetition checks；
- applicable throughput/latency/capacity实际指标及timing boundary；
- warm/cold/persistent cache口径、failure/error摘要；
- functional-only Task产生的incidental timing必须明确标记为非Acceptance数据。

### Evidence pointers

至少记录 Evidence root、main log、command/config artifact、workload manifest、raw result table/JSON/CSV、checksums/full checksums、preserved container/wheel/cache/artifact paths、last successful gate/cell和first blocker。

### Deviations and corrections

任何 harness correction、retry、rerun、workaround、parameter deviation、source change或environment change，都必须记录 `what / why / impact / Evidence`。未记录的修正不能靠聊天解释后再进入Acceptance。
