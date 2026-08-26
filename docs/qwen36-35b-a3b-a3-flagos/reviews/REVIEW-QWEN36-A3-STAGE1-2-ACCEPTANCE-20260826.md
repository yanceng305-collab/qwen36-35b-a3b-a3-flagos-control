# Codex1 Formal Acceptance — Qwen3.6 A3 Stage 1/2

Review date：2026-08-26

Acceptance：**ACCEPTED**

## Accepted execution identity

| Field | Accepted identity |
| --- | --- |
| Implementation repo | `https://github.com/xiemingda-1002/vllm-plugin-FL` |
| Tracked branch at execution | `feature/qwen3.6-35b-a3b-ascend-graph-migration` |
| Execution HEAD/tree | `7beda84f59d7b25f49cdf03bdf6efecd771067ed` / `a81eea55c1de548a0a1f182f51089eca0b088c82` |
| Official PR base | `flagos-ai/vllm-plugin-FL:release/0.2@53adefb269571684d83a51e997d3ba9be5f88235` |
| Official base tree | `9ddfd080953ad39b39772e108ff921d2973b0299` |
| A3 image | `quay.io/ascend/vllm-ascend:v0.20.2rc1-a3-openeuler` |
| Manifest/platform digest | `sha256:442363921166771eb82baeec9c1ac0381f46fb830ead8d0e072df6e925f2a958` / `sha256:a55b2b0597f9fdd1882de9bf3b7ebc395dd77c1ca49f251d0cb759d7b2c1a807` |
| Wheel | `vllm_plugin_fl-0.2.0+g7beda84f5-cp311-cp311-linux_aarch64.whl` |
| Wheel SHA-256 | `fa33f586b2e56e78f671989e6dc3dc2ee23f5005c5f0cc4800a6cf6e4b2e98c1` |
| A3 family | `ascend910_93` |
| Code PR | `N/A`；implementation source未修改 |

## Reviewed Result chain

1. [`RESULT-QWEN36-A3-S1S2-ENV-BUILD-RUNTIME-20260825T205424+0800.md`](../results/RESULT-QWEN36-A3-S1S2-ENV-BUILD-RUNTIME-20260825T205424+0800.md)：Gate A core PASS；Gate B initial STOP。
2. [`RESULT-QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG-20260825T224528+0800.md`](../results/RESULT-QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG-20260825T224528+0800.md)：Triton/provider补证；确认 `+0800` source path触发 CMake regex classification问题；无 source patch。
3. [`RESULT-QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG-FOLLOWUP-20260825T234607+0800.md`](../results/RESULT-QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG-FOLLOWUP-20260825T234607+0800.md)：non-source path/network closure；Gate B A3 wheel PASS；Gate C初次STOP。
4. [`RESULT-QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG-GATEC-FOLLOWUP-20260826T092617+0800.md`](../results/RESULT-QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG-GATEC-FOLLOWUP-20260826T092617+0800.md)：corrected A3 runtime access；Gate C/D PASS；Stage 1/2 Execution PASS。

Codex1本轮未操作 A3 server；Acceptance依据 immutable Results、manifest/checksum/raw-log pointers、Control contracts、GitHub source identities和静态 diff review。Acceptance不补猜 Result未记录的现场事实。

## Gate review

### Gate A — ACCEPTED

- actual A3/910C device与safe scope；
- official A3 image/digests、Python 3.11.15、CANN 9.0.0、torch/torch_npu 2.10.0/2.10.0、vLLM 0.20.2+empty；
- `triton-ascend 3.2.1`、community `triton 3.5.0`、imported module origin和 Ascend backend/provider补证；
- exact source、official base、CATLASS和 environment identity。

### Gate B — ACCEPTED

- parent metadata blocker root cause以 HIGH confidence确认：source path中的 `+`被 CMake未转义 regex解释，导致 selected op metadata flow为空；no-plus path清除 blocker，不需要 source patch；
- network dependency以现有可审计代理闭合；
- clean exact source生成 A3 `ascend910_93` wheel；
- wheel hash/ABI、8 OPP/9 schemas、A3 prebuilt root完整；严格 A2 residue搜索为0。

### Gate C — ACCEPTED

- final `vllm-plugin-fl`来自 accepted wheel/site-packages；无 FL source `PYTHONPATH`/editable shortcut；
- `vllm-ascend` distribution absent，`vllm_ascend`不可 import；
- `VLLM_PLUGINS=fl`、`USE_FLAGGEMS=0`；PlatformFL走 Ascend fast-path；
- `_C_ascend`和 OPP来自 wheel `prebuilt/ascend910_93`；
- 不安装 FlagGems即可通过。之前 visible `flag_gems` error接受为 runtime/device mapping不完整后的 fallback表象，不是 FL source defect。

### Gate D — ACCEPTED

- wheel-packaged `torch.ops._C_ascend.npu_add_rms_norm_bias`在真实 A3 NPU执行；
- input/output为 `npu:0`，BF16 `(2,1024)`；NPU synchronize、CPU reference、finite/correct和 no CPU fallback Evidence完整；
- Acceptance只覆盖该最小 smoke及其记录的数值范围，不外推全部 OPP/model correctness。

## Accepted A3 container/runtime baseline

Evidence证明：先前 incomplete/non-privileged runtime mapping下，`torch_npu`可import但 `torch.npu.is_available()==False`、`device_count()==0`，随后 DeviceInfo落入 FlagGems detector。Corrected composite pattern下 NPU availability/count正确、PlatformFL与 real custom-op PASS。

因此接受**整套已执行成功的 composite runtime-access pattern**作为后续 A3任务默认 baseline：privileged container、host networking、task-selected davinci devices与管理设备、host Ascend driver/runtime mounts、`/data:/data`，以及 post-launch NPU invariant。

Claim boundary：本次没有逐项 ablation，故不声称每个 flag/mount单独都是必要根因；它们作为已验证组合应整体复用，直到后续 Accepted Evidence证明可安全收窄。具体 device编号、数量、container name、image tag/digest仍按 task/preflight解析，不写死。

Exact reconstruction见 [`A3-STAGE1-2-ACCEPTED-RUNTIME.md`](../reconstruction/A3-STAGE1-2-ACCEPTED-RUNTIME.md)，可迁移约束见 [`A3-RUNTIME-HANDOFF.md`](../A3-RUNTIME-HANDOFF.md)。

## ChatGPT commits disposition

### `8cafd7cf55f8c377301ace3efcd7b52a20c3e3e7`

**Retained and refined; not reverted.** 将 runtime pattern写入 `A3-RUNTIME-HANDOFF.md`方向正确。Codex1调整为 Accepted scope，增加 composite-vs-individual causality边界，并把 exact现场重建细节另存 reconstruction。

### `9fcc7e4391b47ba50329e4a9f612c801a62ba44e`

**Retained and refined; not reverted.** AGENTS长期规则正确：必须先验证 `torch.npu` availability/count，不能用安装FlagGems掩盖runtime mapping失败。Codex1将其指向正式 Accepted handoff/reconstruction。

## Acceptance boundary / not covered

Stage 1/2 `ACCEPTED`只证明 exact `7beda84...`：A3 environment、native wheel build、standalone FL install、PlatformFL import和一个 real A3 custom-op smoke。

仍未证明：

- Qwen模型完整构造与 BF16权重load；
- TP2/HCCL model execution；
- GDN/Mamba/full attention/MoE完整 forward；
- readable generation、repeatability；
- graph、serve、prefix、EP、64K、capacity或performance。

## Moving branch and Stage 3

Current tracked branch已前进到 `e610a990d785356bf51a3cad50219d4c03310a31` / tree `609ff1ad0f08239f353cb4d8774e504b4deba03b`，比 accepted source多2个 runtime commits，涉及 NPU communicator、PlatformFL和 ModelRunnerFL，未修改 A3 OPP/build packaging。

因此 Stage 1/2 Acceptance不被撤销，但不自动覆盖 `e610a...`。Stage 3可以解锁；其 Ready Task必须在 preserved Accepted container中对 dispatch HEAD执行 wheel rebuild + bounded C/D regression，再进入 TP2 BF16 eager。若 dispatch HEAD再次变化，STOP请求 Codex1 diff review。

## Final conclusion

Stage 1/2：**ACCEPTED**。

Stage 3：**UNLOCKED / Awaiting explicit User dispatch of Ready Task**。
