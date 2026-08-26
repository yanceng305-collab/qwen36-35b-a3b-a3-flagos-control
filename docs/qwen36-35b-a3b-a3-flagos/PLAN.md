# Qwen3.6-35B-A3B × FlagOS × Ascend A3/910C 项目计划

状态：Stage 1/2、Stage 3、Stage 4和Stage 5均已 **ACCEPTED**；Stage 6 `QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX` **READY / Awaiting explicit User dispatch**。

## 结果目标

在 A3/910C上对当前 PR #404 implementation建立从 native wheel到模型、graph、serve、功能扩展、性能和可重建 handoff的完整证据链；每项结论绑定 exact source/environment/run identity。

## 硬约束

- implementation跟踪 moving feature branch；每次执行冻结 exact SHA/tree/clean state。
- A2只作 reference oracle；A3 Acceptance只来自 A3 execution。
- vLLM 0.20.2、FL release/0.2系、BF16 DP1/TP2 eager是当前 baseline。
- 最终 runtime为 standalone FL：`VLLM_PLUGINS=fl`、`USE_FLAGGEMS=0`、无 installed/runtime `vllm-ascend`依赖、无 source-tree import。
- A3 wheel使用 `ascend910_93` family并在 compatible A3/CANN环境重建；禁止 A2 binary复用。
- Base carrier只在 official `v0.20.2rc1-a3` / `v0.20.2rc1-a3-openeuler`中 bounded selection；ordinary A2 image、nightly或其他版本不得 silent fallback。
- 模型identity已在Stage 3 Accepted；26/26 shards、1045/1045 tensors BF16，后续Task继续核对identity continuity。
- MTP、quantization、initial CP/FlashComm/MC2/EPLB不在第一阶段范围。
- 没有 attributable blocker不得创建大型 Code adaptation task。

## 当前关键路径

```text
Stage 0-5 accepted foundation
  -> Stage 6 A2-equivalent DP1/TP2 functional reproduction (16/16)
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
| 6 — A2-equivalent functional reproduction | 恢复A2 DP1/TP2 service合同并完成`1K/4K/16K/64K x C1/C8/C32/C64, O1024` | Stage 5 Accepted；[Ready Task](tasks/QWEN36-A3-S6-A2-EQUIVALENT-FUNCTIONAL-MATRIX.md)；explicit User dispatch | 16/16 A3 functional correctness；A2只作reference；不评价performance | **READY / Awaiting dispatch** |
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

## Source branch更新处理

如果 dispatch 前 tracked branch已从 Control snapshot更新：

1. 记录旧 snapshot和新 HEAD/tree；
2. 获取 diff与 commit说明；
3. Codex1判断 task contract是否仍成立；
4. 在 task/result中冻结 actual run SHA/tree；
5. future update从最后 accepted/tested SHA做 diff，不从任意旧聊天 SHA做比较。

Regression scope候选：

- 与 A3/build/runtime无关：identity检查或 no rerun decision；
- packaging/SoC/ABI微调：Stage 1/2 tiny regression；
- model/eager path变化：重跑 Stage 3；
- graph/state变化：重跑 Stage 4，必要时回 Stage 3；
- broad Qwen/runtime变化：重新定义 functional scope。

## Risk register

| Risk | Current evidence state | Control |
| --- | --- | --- |
| A3 `SOC_VERSION` build/runtime family | **ACCEPTED on `7beda84...`** | future source/wheel变化时重验 effective family/origin |
| A2 binary/build residue混入 A3 wheel | **Closed for accepted wheel** | current-head rebuild继续严格 inventory/search |
| CANN/torch-npu/vLLM ABI不匹配 | **Closed for Stage 4 bounded model/graph scope** | tuple变化时重验 |
| A3 Ubuntu/openEuler route与actual host不兼容 | **Closed for accepted openEuler route** | image/driver/host变化重做 preflight |
| runtime实际依赖 vllm-ascend或 source tree | **Closed for accepted standalone FL** | every wheel reinstall保留negative import/origin audit |
| moving branch使旧结果误标新 SHA | Branch已实际 force-push | exact run identity + diff-driven regression |
| graph stale pointer/state | **Closed for Stage 5 `[1,2,4,8]` bounded service scope** | Stage 6验证automatic capture through 64、chunked prefill、async和long-context/concurrency shapes |
| cache污染导致假失败/假成功 | A2有真实先例 | cache identity/隔离纳入每个 run manifest |
| HCCL/CPU topology在 A3不同 | TP2/HCCL correctness Accepted；performance topology unknown | functional稳定后用A3 Evidence冻结performance合同 |
