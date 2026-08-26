# Qwen3.6-35B-A3B × FlagOS × Ascend A3/910C 项目计划

状态：Stage 1/2 **ACCEPTED** for exact `7beda84...`；Stage 3 `QWEN36-A3-S3-TP2-BF16-EAGER` **READY / Awaiting explicit User dispatch**。

## 结果目标

在 A3/910C上对当前 PR #404 implementation建立从 native wheel到模型、graph、serve、功能扩展、性能和可重建 handoff的完整证据链；每项结论绑定 exact source/environment/run identity。

## 硬约束

- implementation跟踪 moving feature branch；每次执行冻结 exact SHA/tree/clean state。
- A2只作 reference oracle；A3 Acceptance只来自 A3 execution。
- vLLM 0.20.2、FL release/0.2系、BF16 DP1/TP2 eager是当前 baseline。
- 最终 runtime为 standalone FL：`VLLM_PLUGINS=fl`、`USE_FLAGGEMS=0`、无 installed/runtime `vllm-ascend`依赖、无 source-tree import。
- A3 wheel使用 `ascend910_93` family并在 compatible A3/CANN环境重建；禁止 A2 binary复用。
- Base carrier只在 official `v0.20.2rc1-a3` / `v0.20.2rc1-a3-openeuler`中 bounded selection；ordinary A2 image、nightly或其他版本不得 silent fallback。
- 模型路径已确认但仍在下载；Stage 1/2只做 optional inventory且不等待，完整 model identity gate后置Stage 3。
- MTP、quantization、initial CP/FlashComm/MC2/EPLB不在第一阶段范围。
- 没有 attributable blocker不得创建大型 Code adaptation task。

## 当前关键路径

```text
Stage 0 Control / live baseline established
  -> parent Stage 1/2 execution: Gate A core PASS with Evidence gap / Gate B STOP
  -> Codex1 Review: NEEDS-FOLLOWUP
  -> bounded Gate B OPP metadata diagnostic
  -> if source change required: STOP / Decision requested
  -> if non-source correction closes Gate B: same-container Gate C/D
  -> Stage 1/2 Execution Result
  -> Codex1 Acceptance (ACCEPTED)
  -> current-head wheel rebuild + bounded C/D regression
  -> model identity gate
  -> Stage 3 TP2 BF16 eager
  -> Stage 4 FULL_DECODE_ONLY graph
  -> Stage 5 serve
  -> Stage 6 bounded functional expansion
  -> Stage 7 A3 performance/capacity
  -> Stage 8 runtime freeze/reconstruction/handoff
```

任一 Stage在 first attributable blocker处 STOP，保存 Evidence，按 environment/build/runtime/model/graph/serve/functional/performance归因后再设计最小后续 Task。

## Stage 计划

| Stage | Objective | Ready / entry gate | PASS evidence boundary | Current status |
| --- | --- | --- | --- | --- |
| 0 — Project / Baseline | Control、GitHub current state、baseline、A2 reference、workflow、first task | User授权初始化 | 正式 docs + exact snapshot + publish | **COMPLETE** |
| 1 — A3 Environment / Build Readiness | physical 910C、safe device、official A3 image selection、driver/CANN/Python/torch/torch-npu/vLLM/Triton/build tools/source identity | Accepted Result chain | selected tag/ID/digest/OS/tuple/reason + environment/source manifest | **ACCEPTED on `7beda84...`** |
| 2 — A3 Wheel / Standalone Runtime | clean A3 build、OPP/schema inventory、`_C_ascend`、wheel、formal install、FL origin、no vllm-ascend、real NPU op | Accepted Result chain | A3 artifact + hash/inventory + load/register + actual A3 NPU custom-op smoke | **ACCEPTED on `7beda84...`** |
| 3 — TP2 Eager | current-head regression、model identity、BF16完整权重、TP2/HCCL、Qwen/GDN/Mamba/full attention/MoE、prefill/decode/repeat | Stage 1/2 Accepted；Ready Task；explicit User dispatch | current-head wheel C/D regression + complete load + finite/readable repeat output + ownership/no CPU fallback | **READY / Awaiting dispatch** |
| 4 — FULL_DECODE_ONLY | capture sizes、fixed-address state、replay、GDN/Attention update、repeat/state freshness | Stage 3 Accepted | capture+replay多次；无 stale state/507011/NaN/Inf；输出正常 | Locked |
| 5 — Serve | health/models/completion/chat/repeat/bounded concurrency | Stage 4 Accepted | API/engine/model/run identity + repeated requests + bounded concurrency | Locked |
| 6 — Functional Expansion | prefix、long context、EP2、concurrency、chunked prefill、async、64K、functional matrix | Stage 5 Accepted；每项独立合同 | 每项真实 A3 raw logs/output/identity；未覆盖项不外推 | Locked |
| 7 — Performance / Capacity | 决定是否复现 1K/4K/16K/64K × C1/C8/C32/C64 | correctness+graph+serve稳定 | A3 raw measurements、配置、缓存、重复运行、capacity/variance | Locked |
| 8 — Runtime Freeze / Handoff | validated image/wheel/source/environment/device/cache/HCCL/startup/Evidence/reconstruction | 所需前序 Stage Accepted | 可重建 manifest、hash、pointer、handoff边界 | Locked |

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
| CANN/torch-npu/vLLM ABI不匹配 | **Closed for Stage 1/2 smoke scope** | model path在Stage3继续验证 |
| A3 Ubuntu/openEuler route与actual host不兼容 | **Closed for accepted openEuler route** | image/driver/host变化重做 preflight |
| runtime实际依赖 vllm-ascend或 source tree | **Closed for accepted standalone FL** | every wheel reinstall保留negative import/origin audit |
| moving branch使旧结果误标新 SHA | Branch已实际 force-push | exact run identity + diff-driven regression |
| graph stale pointer/state | A2曾修复；A3 unknown | Stage 4 repeated replay/state freshness，不提前外推 |
| cache污染导致假失败/假成功 | A2有真实先例 | cache identity/隔离纳入每个 run manifest |
| HCCL/CPU topology在 A3不同 | A2 defaults only | correctness-first；A3实测后再冻结 handoff/performance |
