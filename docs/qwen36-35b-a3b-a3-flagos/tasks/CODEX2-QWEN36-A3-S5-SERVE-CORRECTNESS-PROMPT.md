# Codex2 Execution Prompt — QWEN36-A3-S5-SERVE-CORRECTNESS

User dispatch：**执行 `QWEN36-A3-S5-SERVE-CORRECTNESS`。这是正式授权，只执行Stage 5 service correctness gate；不要进入functional matrix或performance。**

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

你是Codex2 execution代理。先同步latest Control main，严格读取`AGENTS.md`、`README.md`、`STATUS.md`、`results/INDEX.md`、Stage 4 Formal Acceptance、`A3-RUNTIME-HANDOFF.md`、Accepted reconstruction和本Task。不要依赖旧聊天记忆或服务器目录猜测task identity。

Stage 4 Accepted runtime artifact：

```text
source e610a990d785356bf51a3cad50219d4c03310a31
tree 609ff1ad0f08239f353cb4d8774e504b4deba03b
wheel sha256 2fcf788660f3fe42b364bc60d593ee1b9b634fc0632de58c444d961bff4aa1bd
model /data/tiankuan/zyg/FL/workspace/Qwen3.6-35B-A3B
```

Task创建时reviewed tracked source：

```text
HEAD 032fddc91b6d013b98aed8e64ff05b54d1435648
tree 463806ef18e5e31006cd4f59e6a5261fc65cea4a
disposition docs/tests-only; reuse Accepted wheel/runtime
official release/0.2 HEAD ef78dec66fea1ae858ef414584be1478929ee9b2
official release/0.2 tree 7414bac41c39bc445b0cc05dbdaecc0f08231aeb
```

首先live query tracked branch HEAD/tree、official PR #404和official base。若tracked identity仍精确匹配`032fddc9...` / `463806ef...`，复用Accepted `e610a990...` wheel/runtime，绝不能把`032fddc9...`写成wheel build identity。若branch已移动，在任何model/service mutation前STOP，返回`032fddc9...`到新HEAD的exact diff。

只读盘点当前授权NPU；不得kill/pause/reset/preempt或触碰其他任务。优先复用Accepted container。若容器缺失，只能按Accepted reconstruction恢复同一image/version/runtime/wheel；任何image、CANN、torch/torch-npu、vLLM、Triton、wheel或device mapping漂移都STOP。

Stage 5的唯一目标是证明Stage 4 Accepted `FULL_DECODE_ONLY` graph在真实vLLM service/API路径中正确工作。不要做性能。

进入service前验证并保存Evidence：`torch_npu` import；`torch.npu.is_available()==True`；visible device count 2；Accepted wheel SHA/site-packages origin；model identity；`vllm-ascend` absent；`vllm_ascend`不可import；FlagGems absent且`USE_FLAGGEMS=0`；PlatformFL/WorkerFL/ModelRunnerFL/GraphWrapper来自standalone `vllm_fl`；A3 `_C_ascend`/OPP origin；DP1/TP2/HCCL continuity。

保持Stage 4 bounded配置：BF16 non-quantized、DP1/TP2、prefix off、chunked prefill off、MTP off、`enforce_eager=False`、compilation mode `VLLM_COMPILE`、backend `eager`、`cudagraph_mode=FULL_DECODE_ONLY`、capture sizes `[1,2,4,8]`、`npugraph_ex` disabled。启动standalone FL OpenAI-compatible service，保存完整server stdout/stderr、effective config、PID/process tree、readiness和exit status。

至少完成并保存以下API请求/响应：

1. readiness/health；
2. `GET /v1/models`，HTTP 200且model identity正确；
3. `POST /v1/completions`，单请求、greedy、bounded多token输出，HTTP 200、非空可读；
4. `POST /v1/chat/completions`，HTTP 200、非空可读；
5. 同一greedy completion独立重复两次，text一致；
6. bounded concurrency=2，两个不同prompt，均成功、非空且输出不同；
7. completion logprobs可用时请求并检查全部finite；如当前API/版本不提供所请求字段，保存实际响应和等价finite output/logit Evidence，不能静默略过。

必须从server/worker trace或counter证明，而不是从HTTP 200推断：effective mode仍为`FULL_DECODE_ONLY`；GraphWrapper/graph type归属FL-local Ascend `NPUGraph`；两个TP worker capture `[1,2,4,8]`或安全复用本Task对应capture并有可审计Evidence；prefill为`forward_mode=NONE`/eager passthrough；decode为`forward_mode=FULL`/real replay；batch size 1和2在两个TP worker均有非零实际replay。允许不修改production source/artifact的runtime-only instrumentation。

通过repeat、different-prompt concurrency、batch-size切换、连续多token decode和final synchronization检查GDN/Mamba recurrent state、attention KV/slot mapping及request metadata无stale/cross-request contamination。扫描并确认无CPU fallback、`vllm_ascend`、FlagGems activation、507011、invalid address、OOM、NaN/Inf或pathological output。

完成请求后停止接收新请求，等待in-flight完成，执行clean engine/service shutdown；保存退出码、shutdown log、最终NPU synchronization或等价shutdown synchronization Evidence，并确认本Task NPU进程已释放、设备健康。保留Accepted container/wheel/cache/Evidence，不清理其他任务资源。

只有全部通过才报告：

```text
Execution PASS — Stage 5 Serve Correctness
```

任一gate失败就在first attributable blocker处STOP，保存last successful gate、first blocker、root-cause confidence和唯一最小follow-up；不得修改implementation source或扩大范围。

本Task明确禁止：1K/4K/16K/64K完整矩阵、C32/C64、O1024完整测试、benchmark/profiling/capacity/performance、prefix、EP2、MTP、quantization、A3 matched native、Code fork/PR和GLM。

最后创建immutable Result并同步Control `results/INDEX.md`，返回exact Evidence root、main server log、API artifacts、graph event summary、checksums、container/cache paths、Code/source-Control-Evidence三指针和Control commit。Codex2不得声明Codex1 Acceptance，也不得自行进入下一阶段。
