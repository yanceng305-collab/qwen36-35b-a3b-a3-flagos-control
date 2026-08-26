# Repository, Evidence, Result and Acceptance Rules

## Repository roles

- Control：本仓库，保存 contracts、pointers、immutable Result、Acceptance和 handoff。
- Implementation：同事 feature branch / PR #404，当前 source of truth。
- Validation Code：当前 N/A；只有 blocker-backed Code Decision后创建。
- Server Evidence：原始 logs/manifests/checksums/wheels/runtime facts；不等于长期 source working tree。

## Moving branch and run freeze

正式执行前同步 GitHub并记录：tracked repo/branch、exact HEAD SHA、exact tree、worktree clean/dirty、remote URL。Result只对 actual run identity有效。

Dirty working tree如果影响 source/build/runtime，只能作为 exploratory诊断，不能形成正式 PASS/Acceptance。若 dirty仅为明确列出的 non-source Evidence output，必须保存 diff/status并由 Codex1裁定。

## One task, three pointers

每个正式 run必须保留：

1. **Code/source pointer**：repo、branch、SHA、tree、clean/dirty、Code PR或 `N/A`。
2. **Control pointer**：task、immutable Result路径/commit、results index/Acceptance commit。
3. **Evidence pointer**：server absolute path、manifest、checksum/index、raw log入口。

## Server Evidence minimum

每个正式 Result至少能定位：

- Task ID、run ID、UTC/local timestamps；
- tracked branch与 exact SHA/tree/clean state；
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

## Source update and regression

tracked branch更新后，保留 old Result不变。Codex1审查 `last-tested-SHA..new-HEAD`并新建 regression task；新 Result引用新 SHA/tree。不得用 branch name本身代替 execution identity。

## Code fixes

发生 A3 blocker时先保存 root-cause Evidence。需要源码修改则先建立 Control Decision和 bounded Code task，再选择同事新 commit、User fork/task branch或 validation Code repo。任何 Code change都必须有 branch、diff、tests、commit/PR、rollback与 source pointer。

## Reconstruction discipline

从 Stage 1开始记录 environment/container/build/install/cache/device/Evidence方法，但只有 Accepted事实进入 [reconstruction](reconstruction/README.md)和 [A3-RUNTIME-HANDOFF.md](A3-RUNTIME-HANDOFF.md)。Host-local image/tag不是 portable artifact；必须同时记录 image ID/digest、build inputs、mount/device、source和 external dependency。

## Final A3 end-to-end reproduction rule

从 Stage 6开始及其后的每个 execution Result，都必须保存足够原材料，使项目完成后不依赖 ChatGPT、Codex1、Codex2聊天记录或某个执行者的个人记忆，仅依靠：

1. Control repo；
2. implementation exact source identity；
3. preserved artifacts；
4. preserved Server Evidence；

即可编写并执行最终 [`reconstruction/A3-END-TO-END-REPRODUCTION.md`](reconstruction/README.md)。现在不提前冻结尚未完成的 functional/performance/prefix/EP2合同；项目收尾时再从 Accepted Results、Formal Reviews、A2-to-A3 delta、reconstruction和 preserved Evidence生成完整文档。

最终文档必须能让一个新执行者从环境准备一路复现到最终Accepted功能/性能范围，至少覆盖：hardware/device family、image exact tag/digest/ID、Driver/CANN/Python/torch/torch_npu/vLLM/Triton版本、implementation repo/branch/SHA/tree、official base/PR、build target/prerequisites、wheel identity/inventory、standalone FL install、no vllm-ascend/no FlagGems、container invocation/mount/device mapping、NPU invariant、model identity、env、cache isolation、eager/graph/serve/API、functional matrix、performance workload、prompt/token generation、sampling、warm-up/cache/cold-start、concurrency/input/output、prefix/EP2、success criteria、known pitfalls、A2-to-A3 delta、A3-specific source changes和 final accepted limitations。

## Stage 6+ Result minimum for reproduction

大体积 raw log、wheel、model或cache不要求commit进Control，但 Result和 preserved Evidence至少必须满足以下合同。不能只写“按Stage 5配置运行”或依赖聊天补充。

### Identity

- exact Task ID、run ID、timestamps、Control parent/current commit；
- implementation tracked HEAD/tree/clean state、actual runtime artifact source/tree；
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
