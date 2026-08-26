# QWEN36-A3-S4-FULL-DECODE-ONLY-GRAPH

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
QWEN36-A3-S4-FULL-DECODE-ONLY-GRAPH
```

Stage 3 Accepted execution identity：

```text
HEAD: e610a990d785356bf51a3cad50219d4c03310a31
tree: 609ff1ad0f08239f353cb4d8774e504b4deba03b
wheel sha256: 2fcf788660f3fe42b364bc60d593ee1b9b634fc0632de58c444d961bff4aa1bd
model: /data/tiankuan/zyg/FL/workspace/Qwen3.6-35B-A3B
```

Reviewed tracked-head identity：

```text
HEAD: 032fddc91b6d013b98aed8e64ff05b54d1435648
tree: 463806ef18e5e31006cd4f59e6a5261fc65cea4a
parent: e610a990d785356bf51a3cad50219d4c03310a31
disposition: docs/tests-only; reuse Stage 3 Accepted wheel
```

Formal prerequisite：[`Stage 3 Acceptance`](../reviews/REVIEW-QWEN36-A3-STAGE3-TP2-BF16-EAGER-ACCEPTANCE-20260826.md)。

Moving-head prerequisite：[`032fddc9 bounded Review`](../reviews/REVIEW-QWEN36-A3-MOVING-HEAD-032FDDC9-20260826.md)。

## Objective

在Stage 3 Accepted standalone runtime上验证Qwen3.6-35B-A3B DP1/TP2 BF16 `FULL_DECODE_ONLY`：

1. graph配置和FL-local graph ownership；
2. bounded capture sizes `[1,2,4,8]`成功capture；
3. decode实际replay而非eager fallback；
4. GDN/Mamba recurrent state与attention KV在多次replay、独立request和不同batch size间保持fresh；
5. repeated generation输出可读、finite，无`507011`、invalid address、NaN/Inf或stale state。

## Entry and moving identity gate

- Dispatch时重新查询tracked branch HEAD/tree和official PR base。
- 若HEAD/tree等于已审查的`032fddc9...` / `463806ef...`，记录current tracked identity后直接复用`e610a990...` Stage 3 Accepted wheel/runtime；不重build，不重复Stage 3 eager。
- 运行时Code/source pointer必须同时记录：当前tracked `032fddc9...`及其bounded disposition，以及实际installed runtime artifact所绑定的`e610a990...` / accepted wheel SHA-256。
- 若tracked HEAD/tree不再等于`032fddc9...` / `463806ef...`，在任何graph/model mutation前STOP，返回`032fddc9...`到新HEAD的diff给Codex1决定regression范围。
- 优先复用preserved container；若单纯缺失，可严格按Accepted reconstruction重建同一runtime并安装Stage 3 Accepted wheel。任何image/version/CANN/runtime mapping变化或reconstruction冲突均STOP。
- 开始前必须验证`torch_npu` import、availability true、device count等于实际visible scope、device names，以及source/wheel/model identities。

## Gate G0 — Accepted runtime continuity

证明：

- wheel hash和site-packages origin仍为Stage 3 Accepted current-head wheel；
- `vllm-ascend` absent、`vllm_ascend`不可import；
- `VLLM_PLUGINS=fl`、`USE_FLAGGEMS=0`、FlagGems absent；
- PlatformFL/WorkerFL/ModelRunnerFL、A3 `_C_ascend`/OPP origin未漂移；
- TP2 visible scope安全且HCCL可初始化。

G0未通过不得进入graph。

## Gate G1 — FULL_DECODE_ONLY capture

保持Stage 3的model、BF16、DP1/TP2、MTP/quantization/prefix off，并设置：

```text
enforce_eager=False
compilation mode=VLLM_COMPILE
cudagraph mode=FULL_DECODE_ONLY
capture sizes=[1,2,4,8]
npugraph_ex opt-in disabled
```

使用current default compatibility route：FL-local `GraphWrapper` + eager FX backend + Ascend `NPUGraph`；不得启用未经本Task授权的compiler/performance路线。

保存并证明：

- effective config未回退到`NONE`或其他graph mode；
- both TP ranks完成每个bounded size的capture；
- capture count/entries、graph/backend ownership和persistent input/state buffers可定位；
- capture过程中无OOM、`507011`、invalid address、NaN/Inf或silent eager fallback。

## Gate G2 — Replay and state correctness

- 证明prefill保持非graph路径、decode实际使用captured FULL graph replay；
- 至少对batch size 1和2执行实际replay；每个generation包含多token decode，从而发生连续replay；
- 同一greedy prompt跨独立generation重复至少两次，token/text一致；不同prompt输出必须非空且不同；
- 保存outputs、token IDs、finite logprobs和replay trace/counters；
- 证明GDN/Mamba recurrent state、attention KV/slot mapping和request metadata在request完成、复用与batch size变化后没有stale/cross-request contamination；
- 最终`torch.npu.synchronize()`成功，process exit 0；无非法CPU fallback、`vllm_ascend`或FlagGems activation。

## PASS

只有G0、G1、G2全部PASS才报告：

```text
Execution PASS — Stage 4 FULL_DECODE_ONLY Graph
```

保留container、wheel、graph caches和Evidence，等待Codex1 Acceptance；不得自行进入serve或performance。

## STOP

在first attributable blocker处STOP，包括：

- source/wheel/model/runtime identity漂移；
- Accepted runtime无法复用/原样重建或NPU invariant失败；
- graph mode fallback、capture/replay失败、OOM、`507011`、invalid address；
- fixed-address/persistent metadata不成立；
- stale GDN/Mamba/attention state、cross-request contamination；
- unreadable/empty/pathological output、non-finite values/logprobs、CPU fallback；
- 需要source patch、model修改、version/CANN/image/runtime route变化。

保存last successful gate、first blocker、raw Evidence、root-cause confidence和最小复现；不得直接修改source继续。

## Prohibited

- serve/API/concurrency service；
- prefix、EP2、long context、64K或functional matrix；
- benchmark、profiling、capacity/performance tuning；
- MTP、quantization、CP/FlashComm/MC2/EPLB；
- `npugraph_ex`或其他新compiler route；
- source patch、Code repo/fork/PR、GLM。

## Required Evidence and return

- dispatch、source/base/wheel/model/runtime exact identity；
- NPU invariant和TP2/HCCL identity；
- exact graph config、capture sizes、backend/GraphWrapper/NPUGraph ownership；
- per-rank capture completion、capture entries/count和replay proof；
- persistent address/slot/state metadata与GDN/Mamba/attention freshness；
- prompts、token IDs、outputs、finite logprobs、repeat comparisons；
- exit codes、last successful gate/first blocker；
- cache/container/artifact/Evidence paths和checksums；
- Code/source、Control、Evidence三指针；Code PR=`N/A`。

Result不得自行声明Codex1 Acceptance或进入Stage 5。
