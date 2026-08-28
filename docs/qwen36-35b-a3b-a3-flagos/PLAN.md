# Qwen3.6-35B-A3B × FlagOS × Ascend A3/910C 项目计划

状态：Stage 1/2、Stage 3、Stage 4和Stage 5均已 **ACCEPTED**；Stage 6仍 **STOP / NOT ACCEPTED**。Combined diagnostic已 **ACCEPTED — A / tokenizer-decoder-native, scope-limited**；R0/R1/control compliant。D-034 validator semantics等待User Decision；当前无Ready Task。

## 结果目标

在 A3/910C上对`e610a990...` Frozen Validation Baseline建立从native wheel到模型、graph、serve、功能扩展、性能和可重建handoff的完整证据链；每项结论绑定exact Frozen source/artifact/environment/run identity。

## 硬约束

- Stage 6+ implementation固定为Frozen `e610a990...` / tree `609ff1ad...` / Accepted wheel SHA-256 `2fcf788...`；upstream future movement不是execution gate。
- A2只作 reference oracle；A3 Acceptance只来自 A3 execution。
- vLLM 0.20.2、FL release/0.2系、BF16 DP1/TP2 eager是当前 baseline。
- 最终 runtime为 standalone FL：`VLLM_PLUGINS=fl`、`USE_FLAGGEMS=0`、无 installed/runtime `vllm-ascend`依赖、无 source-tree import。
- Frozen A3 wheel已在compatible A3/CANN环境以`ascend910_93` family构建并Accepted；Stage 6+核对exact SHA-256/inventory/origin，不重build、不复用A2 binary。
- Frozen carrier为Accepted `v0.20.2rc1-a3-openeuler` exact digest/ID；不重新做candidate selection，不silent fallback到A2、nightly或其他版本。
- 模型identity已在Stage 3 Accepted；26/26 shards、1045/1045 tensors BF16，后续Task继续核对identity continuity。
- MTP、quantization、initial CP/FlashComm/MC2/EPLB不在第一阶段范围。
- 没有 attributable blocker不得创建大型 Code adaptation task。

## 当前关键路径

```text
Stage 0-5 accepted foundation
  -> Stage 6 STOP at I1024/C64/O8
  -> evidence-only diagnostic D / unresolved
  -> prospective bounded output-chain capture
  -> new-server jemalloc reconstruction STOP
  -> combined runtime correction + readiness + output-chain capture
  -> diagnostic A accepted
  -> User Decision on tokenizer-native U+FFFD semantics
  -> User-authorized formal Stage 6 recovery, if later approved
  -> same-matrix A3 performance/capacity validation
  -> prefix / EP2 / other specialist capabilities as needed
  -> runtime freeze / reconstruction / handoff
```

任一 Stage在 first attributable blocker处 STOP，保存 Evidence，按 environment/build/runtime/model/graph/serve/functional/performance归因后再设计最小后续 Task。

Stage 5通过后不再为了流程本身拆分与A2 baseline无关的小Stage。主线直接恢复同事A2 DP1/TP2合同：`FULL_DECODE_ONLY`、chunked prefill、async scheduling、`max_num_seqs=64`自动capture、相同prompt/token与sampling/cache/warm-up合同，并先完成input `1K/4K/16K/64K` × concurrency `C1/C8/C32/C64`、output 1024的16-cell functional matrix。只有16/16 correctness通过后，才对同一矩阵测performance。

## Stage 计划

| Stage | Objective | Ready / entry gate | PASS evidence boundary | Current status |
| --- | --- | --- | --- | --- |
| 0 — Project / Baseline | Control、GitHub current state、baseline、A2 reference、workflow、first task | User授权初始化 | 正式 docs + exact snapshot + publish | **COMPLETE** |
| 1 — A3 Environment / Build Readiness | physical 910C、safe device、official A3 image selection、driver/CANN/Python/torch/torch-npu/vLLM/Triton/build tools/source identity | Accepted Result chain | selected tag/ID/digest/OS/tuple/reason + environment/source manifest | **ACCEPTED on `7beda84...`** |
| 2 — A3 Wheel / Standalone Runtime | clean A3 build、OPP/schema inventory、`_C_ascend`、wheel、formal install、FL origin、no vllm-ascend、real NPU op | Accepted Result chain | A3 artifact + hash/inventory + load/register + actual A3 NPU custom-op smoke | **ACCEPTED on `7beda84...`** |
| 3 — TP2 Eager | current-head regression、model identity、BF16完整权重、TP2/HCCL、Qwen/GDN/Mamba/full attention/MoE、prefill/decode | Stage 1/2 Accepted | complete load + finite/readable output + ownership/no CPU fallback | **ACCEPTED on `e610a990...` wheel** |
| 4 — FULL_DECODE_ONLY | bounded capture `[1,2,4,8]`、persistent state、replay、repeat/state freshness | Stage 3 Accepted | both ranks capture/replay；无 stale state/507011/NaN/Inf；bounded outputs正常 | **ACCEPTED — bounded graph correctness** |
| 5 — Serve correctness | standalone FL startup、models/completion/chat、repeat、small concurrency、graph ownership/replay、clean shutdown | Stage 4 Accepted | API correctness + actual FULL graph replay + state isolation + no forbidden fallback | **ACCEPTED — bounded service correctness** |
| 6 — A2-equivalent functional reproduction | 恢复A2 DP1/TP2 service合同并完成`1K/4K/16K/64K x C1/C8/C32/C64, O1024` | Stage 5 Accepted；historical [Task](tasks/QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX.md) | 16/16 A3 functional correctness；A2只作reference；不评价performance | **STOP / NOT ACCEPTED**；first blocker `I1024/C64/O8`；[Formal Review](reviews/REVIEW-QWEN36-A3-STAGE6-STOP-20260826.md) |
| 6D — Evidence-first U+FFFD diagnostic | read-only审计parent output-chain | [Historical Task](tasks/QWEN36-A3-S6-UFFFD-OUTPUT-CHAIN-DIAGNOSTIC.md) | 证明existing Evidence是否足够归因 | **FORMALLY REVIEWED — D / UNRESOLVED / NEEDS-FOLLOWUP** |
| 6R — Prospective root-cause diagnostic | instrument并捕获generated-token→validator完整chain | [Historical Task](tasks/QWEN36-A3-S6-UFFFD-PROSPECTIVE-ROOT-CAUSE-DIAGNOSTIC.md) | A/B/C earliest-layer attribution，或bounded D unresolved | **ENDED — old-server timeout + new-server jemalloc STOP；D unresolved** |
| 6J — Jemalloc reconstruction + U+FFFD | R0 compatibility path→R1 readiness→R2 complete output chain | [Historical Task](tasks/QWEN36-A3-S6-JEMALLOC-RECONSTRUCTION-AND-UFFFD-ROOT-CAUSE-DIAGNOSTIC.md) | loader/NPU/service admission + earliest-layer classification | **ACCEPTED — diagnostic A / tokenizer-decoder-native；Stage 6 still STOP** |
| 6V — Validator semantics decision | decide provenance-aware corruption gate vs absolute zero-U+FFFD quality rule | [D-034 proposed](DECISIONS.md#d-034--stage-6-tokenizer-native-ufffd-semantics) | User-selected contract branch | **AWAITING USER DECISION；no Ready Task** |
| 6M — Functional matrix recovery proposal | rerun full Stage 6 from beginning if D-034 permits | [Proposed Task](tasks/QWEN36-A3-S6-TOKENIZER-NATIVE-UFFFD-AWARE-FUNCTIONAL-MATRIX-RERUN.md) | revised-oracle 16/16 functional correctness | **NOT READY / DO NOT DISPATCH** |
| Performance / Capacity | 对同一16-cell合同记录A3 FL结果；条件允许时做A3 matched native | functional 16/16 PASS | comparable raw measurements、cache/warm-up口径、capacity/variance | Locked |
| Specialist capabilities | aligned prefix lifecycle、EP2 eager/graph、cold/persistent startup、更宽eager覆盖等 | 主TP2路线稳定；按价值解锁 | 每项独立A3 Evidence；不挡主矩阵除非成为真实依赖 | Locked |
| Runtime Freeze / Handoff | validated image/wheel/source/environment/device/cache/HCCL/startup/Evidence/reconstruction | 所需前序范围Accepted | 可重建 manifest、hash、pointer、handoff边界 | Locked |

## Performance comparison contract

记录两类比较，禁止混淆：

1. `A2 colleague result vs A3 FL result`仅是cross-platform reproduction reference，包含910B1→910C硬件代际差异。
2. 判断FL相对性能时优先`A3 FL vs A3 matched native`，尽可能保持same cards/model/input/output/concurrency/prompts/order/sampling/cache/graph/warm-up。

不得用A3绝对TPS与A2绝对TPS的差异直接归因于FL实现。

## Stage 1/2 首任务拆分边界

合并执行是为了减少环境重复进入，但验收仍分 gate保存：

1. `Environment identity PASS` 后才允许 build；
2. `Source identity + clean build inputs PASS` 后才产生 wheel；
3. `Wheel inventory/family/ABI PASS` 后才 formal install；
4. `Standalone import/origin/dependency PASS` 后才运行 minimal A3 NPU custom-op；
5. 任一步失败都在 first attributable blocker处 STOP，不继续完整模型。

## Frozen baseline change control

Stage 6及后续不再处理source branch更新：不live-query future HEAD、不获取diff、不做moving-head review、不rebuild、不重跑Stage 3/4/5。Feature branch、PR #404和official base只作历史reference。

当前唯一execution source/artifact是：

```text
e610a990d785356bf51a3cad50219d4c03310a31
tree 609ff1ad0f08239f353cb4d8774e504b4deba03b
wheel SHA-256 2fcf788660f3fe42b364bc60d593ee1b9b634fc0632de58c444d961bff4aa1bd
```

只有User发布new baseline Decision后，才可为新HEAD建立新的计划、Evidence和Acceptance；不得覆盖本项目Frozen results。

## Risk register

| Risk | Current evidence state | Control |
| --- | --- | --- |
| A3 `SOC_VERSION` build/runtime family | **ACCEPTED / Frozen artifact continuity required** | source/wheel family/origin漂移即STOP；不切换upstream或rebuild |
| A2 binary/build residue混入 A3 wheel | **Closed for Frozen accepted wheel** | future run只核对exact wheel SHA-256/inventory/origin；不rebuild |
| CANN/torch-npu/vLLM ABI不匹配 | **Closed for Stage 4 bounded model/graph scope** | tuple变化时重验 |
| A3 Ubuntu/openEuler route与actual host不兼容 | **Closed for accepted openEuler route** | image/driver/host变化重做 preflight |
| runtime实际依赖 vllm-ascend或 source tree | **Closed for accepted standalone FL** | every wheel reinstall保留negative import/origin audit |
| upstream变化污染固定A3数据 | User已冻结baseline | future branch/PR/base movement ignore for execution；只核对Frozen artifact/runtime identity |
| graph stale pointer/state | **Closed for Stage 5 `[1,2,4,8]` bounded service scope** | Stage 6验证automatic capture through 64、chunked prefill、async和long-context/concurrency shapes |
| cache污染导致假失败/假成功 | A2有真实先例 | cache identity/隔离纳入每个 run manifest |
| HCCL/CPU topology在 A3不同 | TP2/HCCL correctness Accepted；performance topology unknown | functional稳定后用A3 Evidence冻结performance合同 |
