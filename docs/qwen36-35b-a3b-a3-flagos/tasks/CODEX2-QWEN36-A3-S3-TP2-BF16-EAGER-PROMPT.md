# Codex2 Prompt — QWEN36-A3-S3-TP2-BF16-EAGER

User formal dispatch：**Execute this Ready Task now.**

```text
Control repo:
https://github.com/yanceng305-collab/qwen36-35b-a3b-a3-flagos-control

Implementation repo:
https://github.com/xiemingda-1002/vllm-plugin-FL

Tracked branch:
feature/qwen3.6-35b-a3b-ascend-graph-migration

Task ID:
QWEN36-A3-S3-TP2-BF16-EAGER
```

同步 latest Control并读取 `AGENTS.md`、`STATUS.md`、本 Task、Stage 1/2 Formal Acceptance、`A3-RUNTIME-HANDOFF.md`、Accepted reconstruction、`BASELINE.md`和 Evidence规则。

## Control identity

```text
Current tracked snapshot:
e610a990d785356bf51a3cad50219d4c03310a31
tree 609ff1ad0f08239f353cb4d8774e504b4deba03b

Stage 1/2 Accepted source:
7beda84f59d7b25f49cdf03bdf6efecd771067ed
tree a81eea55c1de548a0a1f182f51089eca0b088c82

Official PR base:
release/0.2@53adefb269571684d83a51e997d3ba9be5f88235

Model:
/data/tiankuan/zyg/FL/workspace/Qwen3.6-35B-A3B
BF16 non-quantized
```

Dispatch时重新查询 current HEAD/tree。若不再是 Control snapshot，在任何 mutation前 STOP并返回 new identity/diff给 Codex1。

## Reuse Accepted environment

复用 preserved container：

```text
qw36-a3-s2-gatec-priv-20260826T092617p0800
```

只读确认 container/image/runtime mounts未漂移、authorized NPU无owner冲突，并先验证：

```text
import torch_npu succeeds
torch.npu.is_available() == True
torch.npu.device_count() == actual mapped device count
```

如果 container缺失/漂移或NPU invariant失败，STOP请求Decision。不要安装FlagGems、创建新container或重建另一套环境掩盖。

## Gate R — Current-head regression

在同一 Accepted container内：

1. 以无regex-special字符的clean path checkout dispatch exact HEAD；
2. 使用official A3 openEuler环境、`SOC_VERSION=ascend910_93`、exact CATLASS `41bf90da655bba3c66d0acd7e00abe33960ecfd6`和既有dependency route构建wheel；
3. 保存wheel hash/ABI/inventory、OPP/schema reconciliation、A2-residue audit；
4. 离开source tree，uninstall old FL和`vllm-ascend`，install current-head wheel；
5. new process验证site-packages standalone FL、no source shortcut、no `vllm_ascend`、`USE_FLAGGEMS=0`、PlatformFL、A3 `_C_ascend`/OPP origin；
6. 重跑real A3 custom-op smoke/reference/synchronize/no-fallback assertions。

Gate R不PASS不得访问模型。

## Gate M — Model identity

对 `/data/tiankuan/zyg/FL/workspace/Qwen3.6-35B-A3B`只读核验：

- config与architecture；
- tokenizer files；
- `model.safetensors.index.json`；
- index引用的全部shards；
- size/inventory/checksum manifest；
- BF16 non-quantized；
- 40 layers、30 GDN + 10 full attention、256 experts/top-8等关键config。

模型不完整、仍下载、identity冲突、quantized或需要download/repair/convert时STOP；不得自行处理模型。

## Gate E — TP2 BF16 eager

保持：

```text
VLLM_PLUGINS=fl
USE_FLAGGEMS=0
DP=1
TP=2
BF16
eager only / no graph
MTP off
quantization off
prefix caching off for first eager
```

必须证明：

- TP2/HCCL初始化；
- model recognition/construction；
- complete BF16 weight load；
- Qwen/GDN/Mamba/full attention/MoE live forward；
- prefill + multi-token decode；
- readable nonempty output、finite scores/logprobs（如可用）；
- repeated generation至少2次，无stale state、NaN/Inf、病态重复；
- no CPU fallback、no `vllm_ascend` dependency、no FlagGems activation；
- PlatformFL/WorkerFL/ModelRunnerFL、HCCL、backend/device/operator ownership Evidence。

只有 R+M+E全部PASS才报告：

```text
Execution PASS — Stage 3 TP2 BF16 Eager
```

保留container、current-head wheel、standalone FL和Evidence，等待Codex1 Acceptance。不得进入graph或serve。

## STOP / prohibited

遇到 current-head drift、runtime regression、model identity、TP2/HCCL、construction/load、GDN/Mamba/attention/MoE、prefill/decode、external runtime、CPU fallback或数值问题时，在first blocker STOP并保存Evidence。需要source/model/version/CANN/image修改时只返回Decision requested，不直接修改。

禁止graph、serve、prefix、EP、64K、benchmark/performance、MTP、quantization、GLM、source patch、Code repo/fork和额外container层。

生成immutable Result并同步INDEX，返回完整三指针、exit codes、last successful gate/first blocker和preserved paths。不得自行进入 Stage 4。
