# QWEN36-A3-S3-TP2-BF16-EAGER

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
QWEN36-A3-S3-TP2-BF16-EAGER
```

Control snapshot current target：

```text
HEAD: e610a990d785356bf51a3cad50219d4c03310a31
tree: 609ff1ad0f08239f353cb4d8774e504b4deba03b
Official PR base: release/0.2@53adefb269571684d83a51e997d3ba9be5f88235
```

Stage 1/2 Accepted source：`7beda84f...` / tree `a81eea55...`。Current target比accepted source多2个 communicator/platform/model-runner commits，不修改 A3 OPP/build packaging；因此本 Task先在 Accepted container中完成 current-head wheel rebuild与 bounded C/D regression，再进入模型。

## Objective

在 Accepted A3 runtime foundation上，对 dispatch exact current tracked HEAD完成：

1. source/wheel/standalone/custom-op bounded regression；
2. `Qwen/Qwen3.6-35B-A3B`完整 BF16 non-quantized model identity gate；
3. DP1/TP2 eager model construction、完整权重load、prefill、多token decode和 repeated generation；
4. 证明 live path落入 PlatformFL → WorkerFL → ModelRunnerFL → FL-local Qwen/GDN/Mamba/Attention/MoE → HCCL/A3 NPU。

## Accepted runtime prerequisite

- 复用 preserved Accepted container `qw36-a3-s2-gatec-priv-20260826T092617p0800`及 [Accepted reconstruction](../reconstruction/A3-STAGE1-2-ACCEPTED-RUNTIME.md)。
- 开始前只读确认 container仍存在、running且 image/runtime mounts未漂移；NPU scope仍获授权、无其他owner冲突。
- 必须先验证 `torch_npu` import、`torch.npu.is_available()==True`、`device_count==实际映射数量`和 device names。
- 如果 preserved environment缺失、被修改或 NPU invariant失败，STOP请求 Decision；不得安装FlagGems或自行重建另一套环境掩盖。

## Moving branch gate

- Dispatch时重新查询 tracked branch HEAD/tree和 official PR base。
- 若仍为 Control snapshot `e610a990...`，继续。
- 若已变化，在任何 build/model mutation前 STOP并返回 exact new identity/diff summary给 Codex1；不得自行把旧 Acceptance转移到新 HEAD。

## Gate R — Current-head Stage 1/2 regression

在同一 Accepted container中：

1. 使用无 regex-special timestamp的 clean source/build path checkout dispatch exact HEAD。
2. 使用 official A3 openEuler image环境、`SOC_VERSION=ascend910_93`、exact CATLASS `41bf90da655bba3c66d0acd7e00abe33960ecfd6`和既有可审计 dependency route构建 current-head wheel。
3. 保存 wheel hash/ABI/inventory、OPP/schema reconciliation和 A2-residue audit。
4. 离开 source tree，uninstall old FL/current `vllm-ascend`，install current-head wheel；new process验证 standalone FL、no source shortcut、no `vllm_ascend`、`USE_FLAGGEMS=0`、PlatformFL和 A3 `_C_ascend`/OPP origin。
5. 重跑最小 real A3 NPU custom-op smoke及 reference/synchronize/no-fallback assertions。

Gate R未通过不得访问模型。

## Gate M — Model identity

Model path：`/data/tiankuan/zyg/FL/workspace/Qwen3.6-35B-A3B`。

只读核验并保存：

- path/realpath、revision或可用 provenance；
- `config.json`和 architecture=`Qwen3_5MoeForConditionalGeneration`；
- tokenizer/config files；
- `model.safetensors.index.json`；
- index引用的全部 shards存在且无额外替代；
- shard size/inventory/checksum manifest；
- BF16 non-quantized contract；
- 40 layers、30 GDN/linear attention + 10 full attention、256 experts/top-8等关键config与预期一致。

模型不完整、仍在下载、quantized、identity冲突或需要download/repair/convert时 STOP；Codex2不得自行补模型。

## Gate E — TP2 BF16 eager

保持：

```text
VLLM_PLUGINS=fl
USE_FLAGGEMS=0
DP=1
TP=2
dtype=BF16
enforce eager / no graph
MTP=off
quantization=off
prefix caching=off for first eager
```

执行并证明：

- 两个 authorized A3 logical devices可见，TP2/HCCL初始化成功；
- model recognition/construction成功；
- complete BF16 weight load成功，无missing/unexpected shard/weight contract错误；
- Qwen wrapper、GDN、Mamba state、full attention、MoE路径进入 live forward；
- prefill和多token decode完成；
- output非空、可读，selected scores/logprobs finite（如接口提供）；
- repeated generation至少2次，无stale state、NaN/Inf、非法CPU fallback或病态重复；
- runtime不import/depend `vllm_ascend`，不激活 FlagGems；
- 保存 PlatformFL/WorkerFL/ModelRunnerFL、backend/device、HCCL和关键operator ownership Evidence。

## PASS

只有 Gate R、M、E全部PASS才允许报告 `Execution PASS — Stage 3 TP2 BF16 Eager`。Execution PASS后保留 container/current-head wheel/standalone FL/model-run evidence，等待 Codex1 formal Acceptance；不得自行进入 graph或serve。

## STOP

在 first attributable blocker处 STOP，包括：

- current branch identity漂移；
- Accepted container/runtime/NPU invariant失效；
- current-head wheel build/C-D regression失败；
- model identity/shards/BF16 contract不完整；
- TP2/HCCL、construction、weight load、GDN/Mamba/attention/MoE、prefill/decode失败；
- `vllm_ascend`依赖、FlagGems activation、CPU fallback、NaN/Inf；
- 需要source patch、模型修改、version/CANN/image/route变化。

保存 last successful gate、first blocker、raw Evidence、root-cause confidence、最小复现和 bounded follow-up；不得直接修改source或模型继续。

## Prohibited

- `FULL_DECODE_ONLY`或任何 graph capture/replay；
- serve/API/concurrency、prefix、EP、64K；
- benchmark、profiling/performance；
- MTP、quantization、CP/FlashComm/MC2/EPLB；
- GLM；
- source patch、Code repo/fork/PR；
- 新container/environment层或无Evidence重构。

## Required Evidence and return

- dispatch/header、current source/base/diff identity；
- Accepted container/image/device/post-launch invariant；
- current-head wheel build/hash/ABI/inventory/A2-residue；
- standalone FL/package/module/provider/custom-op regression；
- model config/tokenizer/index/shards/size/checksum/BF16 manifest；
- exact effective model command/config/env；
- TP2/HCCL init、model construction、full weight load；
- GDN/Mamba/full attention/MoE/prefill/decode ownership/device trace；
- prompts/outputs/repeated-generation/finite/no-fallback assertions；
- exit codes、last successful gate/first blocker；
- preserved container/wheel/Evidence paths；
- Code/source、Control、Evidence三指针；Code PR=`N/A`。

Result不得自行声明 Codex1 Acceptance或进入 Stage 4。
