# QWEN36-A3-S5-SERVE-CORRECTNESS

状态：**READY / Awaiting explicit User dispatch**

执行代理：Codex2

## Unified identity

```text
Control repo:
https://github.com/yanceng305-collab/qwen36-35b-a3b-a3-flagos-control

Implementation repo:
https://github.com/xiemingda-1002/vllm-plugin-FL

Tracked branch:
feature/qwen3.6-35b-a3b-ascend-graph-migration

Task ID:
QWEN36-A3-S5-SERVE-CORRECTNESS
```

Stage 4 Accepted runtime artifact identity：

```text
source: e610a990d785356bf51a3cad50219d4c03310a31
tree: 609ff1ad0f08239f353cb4d8774e504b4deba03b
wheel sha256: 2fcf788660f3fe42b364bc60d593ee1b9b634fc0632de58c444d961bff4aa1bd
model: /data/tiankuan/zyg/FL/workspace/Qwen3.6-35B-A3B
```

Reviewed tracked-head identity at Task creation：

```text
HEAD: 032fddc91b6d013b98aed8e64ff05b54d1435648
tree: 463806ef18e5e31006cd4f59e6a5261fc65cea4a
disposition: docs/tests-only; reuse Stage 4 Accepted wheel/runtime
official release/0.2 HEAD: ef78dec66fea1ae858ef414584be1478929ee9b2
official release/0.2 tree: 7414bac41c39bc445b0cc05dbdaecc0f08231aeb
```

Formal prerequisite：[`Stage 4 Acceptance`](../reviews/REVIEW-QWEN36-A3-STAGE4-FULL-DECODE-ONLY-GRAPH-ACCEPTANCE-20260826.md)。

## Objective

只证明：Stage 4 Accepted `FULL_DECODE_ONLY` graph在真实vLLM service/API路径中仍正确工作。

本Task不是performance、capacity、long-context或A2完整矩阵Task。

## Entry and identity gate

- 首先同步latest Control并按`AGENTS.md`导航恢复上下文。
- Dispatch时live query tracked branch HEAD/tree、official PR #404和official base。
- 若tracked HEAD/tree仍为`032fddc9...` / `463806ef...`，继续复用`e610a990...` Accepted wheel/runtime；不得将`032fddc9...`写成wheel build identity。
- 若tracked HEAD/tree变化，在任何model/service mutation前STOP，返回从`032fddc9...`到新HEAD的exact diff，由Codex1决定最小regression。
- 优先复用Accepted container/runtime；若不存在，只能按Accepted reconstruction原样重建。image/CANN/torch/torch-npu/vLLM/Triton/wheel/device mapping漂移时STOP。
- 只读确认当前授权的空闲NPU scope，不kill/pause/reset/preempt任何进程，不触碰非本Task设备。

## Gate S0 — runtime and service configuration continuity

开始service前证明：

- `torch_npu` import成功、NPU available、visible device count等于2；
- Accepted wheel hash和site-packages origin正确；model identity未漂移；
- `vllm-ascend` absent、`vllm_ascend`不可import；FlagGems absent、`USE_FLAGGEMS=0`；
- PlatformFL / WorkerFL / ModelRunnerFL / GraphWrapper来自standalone `vllm_fl`；A3 `_C_ascend` / OPP origin正确；
- DP1/TP2/HCCL、BF16 non-quantized、prefix off、chunked prefill off、MTP off；
- `enforce_eager=False`、mode=`VLLM_COMPILE`、backend=`eager`、`cudagraph_mode=FULL_DECODE_ONLY`、capture sizes=`[1,2,4,8]`、`npugraph_ex` disabled。

## Gate S1 — service startup and API correctness

启动standalone FL OpenAI-compatible service，并保存完整server stdout/stderr、effective config和exit identity。必须验证：

- service ready/health正常；
- `/v1/models`返回200且model identity正确；
- `/v1/completions`返回200、非空可读输出；
- `/v1/chat/completions`返回200、非空可读输出；
- 至少一个单请求、同一greedy请求独立重复两次、以及bounded concurrency=2的两个不同prompt；
- repeated greedy请求text一致；不同prompt输出非空且不同；
- completion logprobs可用时必须请求并验证全部finite；若API/版本不提供所请求字段，保存响应并用等价finite output/logit Evidence，不能静默跳过；
- 每个验证请求使用bounded短输出，足以产生连续多token decode。

## Gate S2 — graph replay and state isolation in serve path

必须从server/worker侧证明，而不是仅凭API 200推断：

- service effective graph mode仍为`FULL_DECODE_ONLY`，没有fallback到eager/NONE；
- 两个TP worker完成`[1,2,4,8]` capture或安全复用本Task对应cache，并记录Ascend `NPUGraph` ownership；
- prefill为`forward_mode=NONE` / eager passthrough，decode真实为`forward_mode=FULL` / graph replay；
- batch size 1和bounded concurrency batch size 2均出现实际replay；两个TP worker都有非零replay Evidence；
- repeated requests和batch-size切换后，无GDN/Mamba recurrent state、attention KV/slot mapping、request metadata stale/cross-request contamination；
- 无CPU fallback、`vllm_ascend`、FlagGems activation、507011、invalid address、OOM、NaN/Inf或pathological output。

若需要观测graph counter，可使用不改变production source/artifact的runtime-only instrumentation；不得patch implementation source。

## Gate S3 — shutdown and device release

- 停止接收请求后完成in-flight requests；
- service/engine clean shutdown，保存exit code和shutdown log；
- 最终NPU synchronize成功，或保存等价的engine shutdown/device synchronization Evidence；
- 确认本Task的NPU进程释放、设备健康；不触碰其他NPU workload；
- 保存container、cache和Evidence，等待Codex1 review，不自行清理Accepted foundation。

## PASS

S0-S3全部通过才报告：

```text
Execution PASS — Stage 5 Serve Correctness
```

该PASS只覆盖bounded service correctness，不得宣称performance或A2完整service/matrix复现。

## STOP

在first attributable blocker处STOP，包括identity/runtime drift、service不能ready、API错误/空输出、graph fallback、任一TP worker无replay、state contamination、non-finite/病态输出、CPU/forbidden backend activation、非法地址/OOM或clean shutdown失败。保存last successful gate、first blocker、root-cause confidence和唯一最小follow-up；不得直接修改source或扩展测试。

## Prohibited

- 1K/4K/16K/64K完整矩阵、C32/C64、O1024完整测试；
- benchmark、profiling、capacity、performance tuning；
- prefix caching、EP2、MTP、quantization、CP/FlashComm/MC2/EPLB；
- A3 matched native comparison；
- source patch、Code fork/PR、GLM；
- 操作或清理非本Task的NPU/container/process/cache。

## Required Evidence and return

- dispatch/current tracked/base/runtime artifact/wheel/model/container/device exact identity；
- server launch/effective config/readiness/clean shutdown logs；
- `/v1/models`、completion、chat、repeat、concurrency请求/响应/HTTP status；
- prompts、sampling params、token/text、logprobs或等价finite checks；
- both-rank capture/replay counters/traces和graph ownership；
- forbidden-path/error negative scan；
- final device state、exit codes、checksums、Evidence inventory；
- immutable Result + results/INDEX sync；
- Code/source、Control、Evidence三指针；Code PR=`N/A`。

Result不得自行声明Codex1 Acceptance，不得进入A2-equivalent functional matrix或performance。
