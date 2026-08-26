# Codex2 Prompt — QWEN36-A3-S4-FULL-DECODE-ONLY-GRAPH

User formal dispatch：**Execute this Ready Task now.**

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

同步latest Control，按`AGENTS.md`读取`STATUS.md`、本Task、Stage 3 Formal Acceptance、`A3-RUNTIME-HANDOFF.md`、Accepted reconstruction、`BASELINE.md`和Evidence规则。

## Accepted identity

```text
Stage 3 Accepted HEAD:
e610a990d785356bf51a3cad50219d4c03310a31
tree 609ff1ad0f08239f353cb4d8774e504b4deba03b

Accepted wheel sha256:
2fcf788660f3fe42b364bc60d593ee1b9b634fc0632de58c444d961bff4aa1bd

Model:
/data/tiankuan/zyg/FL/workspace/Qwen3.6-35B-A3B
BF16 non-quantized
```

## Reviewed moving head

```text
Current reviewed tracked HEAD:
032fddc91b6d013b98aed8e64ff05b54d1435648
tree 463806ef18e5e31006cd4f59e6a5261fc65cea4a
parent e610a990d785356bf51a3cad50219d4c03310a31

Disposition:
docs/tests-only; reuse Stage 3 Accepted wheel/runtime
```

Dispatch时重新查询current HEAD/tree和official base。若仍为`032fddc9...` / `463806ef...`，记录tracked identity和bounded Review，直接复用`e610a990...` Accepted wheel；不要重build，不要重跑Stage 3。若HEAD/tree不再精确相等，在任何graph/model mutation前STOP，返回`032fddc9...`到新HEAD的diff给Codex1。

## Gate G0 — Runtime continuity

优先复用preserved Accepted container：

```text
qw36-a3-s2-gatec-priv-20260826T092617p0800
```

如果container单纯缺失，可严格按`docs/qwen36-35b-a3b-a3-flagos/reconstruction/A3-STAGE1-2-ACCEPTED-RUNTIME.md`原样重建，并安装Stage 3 Accepted wheel。不得改变image/version/CANN/runtime/device-mapping pattern或创建新环境路线。

开始前验证：

```text
import torch_npu succeeds
torch.npu.is_available() == True
torch.npu.device_count() == actual visible device count
```

同时验证wheel hash/site-packages origin、model identity、`vllm-ascend` absent、`vllm_ascend`不可import、`VLLM_PLUGINS=fl`、`USE_FLAGGEMS=0`、FlagGems absent，以及PlatformFL/WorkerFL/ModelRunnerFL和A3 `_C_ascend`/OPP origin。Code/source证据同时记录current tracked `032fddc9...`和actual runtime artifact `e610a990...` / Accepted wheel SHA-256，不得把未重build的`032fddc9...`写成wheel build identity。

## Gate G1 — Capture

保持Stage 3 model、BF16、DP1/TP2、MTP/quantization/prefix off，使用：

```text
enforce_eager=False
compilation mode=VLLM_COMPILE
cudagraph mode=FULL_DECODE_ONLY
capture sizes=[1,2,4,8]
npugraph_ex opt-in disabled
```

必须证明effective config没有fallback，current route为FL-local GraphWrapper + eager FX backend + Ascend NPUGraph，两个TP ranks成功capture `[1,2,4,8]`。保存capture entries/count、backend ownership和persistent input/state buffer Evidence。出现OOM、507011、invalid address、NaN/Inf或silent eager fallback立即STOP。

## Gate G2 — Replay/state

- 证明prefill保持非graph路径，decode实际进入FULL graph replay；
- batch size 1和2都实际replay，且每个generation包含多token连续decode；
- 同一greedy prompt跨独立generation至少重复两次，token/text一致；不同prompt输出非空且不同；
- 保存token IDs、outputs、finite logprobs和replay trace/counters；
- 证明GDN/Mamba recurrent state、attention KV/slot mapping和request metadata在request完成、复用及batch size变化后无stale/cross-request contamination；
- final `torch.npu.synchronize()`和process exit 0；无CPU fallback、`vllm_ascend`或FlagGems activation。

只有G0/G1/G2全部PASS才报告：

```text
Execution PASS — Stage 4 FULL_DECODE_ONLY Graph
```

保留container、wheel、graph caches和Evidence，等待Codex1 Acceptance。不得进入serve或performance。

## STOP / prohibited

遇到identity/runtime drift、无法原样重建、NPU invariant、capture/replay、fixed-address metadata、507011、invalid address、OOM、state freshness、output/finite/no-fallback问题，在first blocker STOP并保存Evidence。需要source/model/version/CANN/image/runtime route修改时只返回Decision requested，不直接修改。

禁止serve、prefix、EP2、long context、64K、functional matrix、benchmark/performance、MTP、quantization、npugraph_ex、新compiler route、source patch、Code fork/PR和GLM。

生成immutable Result并同步INDEX，返回完整三指针、per-gate exit codes、last successful gate/first blocker和preserved paths。不得自行进入Stage 5。
