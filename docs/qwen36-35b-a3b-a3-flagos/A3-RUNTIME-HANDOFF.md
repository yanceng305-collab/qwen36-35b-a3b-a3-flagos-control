# A3 Runtime Handoff — Qwen Validation to Future GLM Work

更新时间：2026-08-26

当前状态：**EXECUTION-PROVEN CANDIDATE / CODEX1 ACCEPTANCE PENDING**。Stage 1/2 已由真实 A3/910C execution 报告 `Execution PASS`，其中 container/runtime access pattern、standalone FL wheel 与 real NPU custom-op smoke 已通过；在 Codex1 Formal Acceptance 前，这些事实仍是 candidate，不得写成 `Validated handoff`。

## Handoff admission rule

每个可继承事实至少记录：

- source task/run和 Acceptance；
- source SHA/tree/clean state；
- A3 device/driver/CANN/environment tuple；
- image/container/wheel digest或 checksum；
- command/effective config与 Evidence pointer；
- 验证范围、未覆盖边界、失效/重验条件；
- GLM侧仍需独立验证的 contract。

仅 static source、A2 evidence或 Codex2 `Execution PASS`而未 Acceptance的内容不得写成 validated handoff。

## Candidate reusable foundations

| Candidate | Current state | Required Qwen A3 evidence before handoff |
| --- | --- | --- |
| Base image / container pattern | **EXECUTION PASS / ACCEPTANCE PENDING** | Gate C/D run已证明 privileged + host Ascend runtime mounts 可使 `torch.npu` 正确暴露；待 Codex1 Acceptance |
| CANN / Python / torch / torch-npu / vLLM 0.20.2 / Triton tuple | **EXECUTION PASS / ACCEPTANCE PENDING** | Stage 1/2 tuple和Gate D smoke已记录；待 Codex1 Acceptance |
| A3 wheel build flow / `ascend910_93` detection | **EXECUTION PASS / ACCEPTANCE PENDING** | A3 wheel、family inventory、hash已记录；待 Codex1 Acceptance |
| `_C_ascend` build/package/load infrastructure | **EXECUTION PASS / ACCEPTANCE PENDING** | wheel origin、ABI、A3 load、real NPU op已通过；待 Codex1 Acceptance |
| CANN OPP build/package/expose infrastructure | **EXECUTION PASS / ACCEPTANCE PENDING** | OPP inventory、A3 family、runtime registration/execution已通过；待 Codex1 Acceptance |
| HCCL / multiprocessing / device mapping | PARTIAL | 当前只证明2-device可见性与单NPU custom-op；TP2 eager/HCCL仍需后续 Accepted evidence |
| PlatformFL / WorkerFL / ModelRunnerFL lifecycle | PARTIAL | PlatformFL standalone import已通过；完整 Worker/ModelRunner model path仍待 Stage 3 |
| Standalone FL formal installation | **EXECUTION PASS / ACCEPTANCE PENDING** | site-packages wheel origin、no source PYTHONPATH、no installed `vllm-ascend`已通过；待 Codex1 Acceptance |
| Cache isolation / compiler identity | PARTIAL | 当前仅基础provider/build identity；model/graph cache contract待后续 Stage |
| Evidence / immutable Result / reconstruction discipline | EXECUTION-PROVEN | 已有完整三指针、checksum、immutable Result；formal Acceptance仍待 Codex1 |
| Runtime image/wheel/startup handoff | PARTIAL | wheel已保留，PASS container已保留；最终 Stage 8 freeze/reconstruction尚未完成 |

## Execution-proven A3 container runtime baseline — Acceptance pending

Source execution:

- Task/run: `QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG` Gate C/D follow-up / `20260826T092617p0800`
- Immutable Result: `results/RESULT-QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG-GATEC-FOLLOWUP-20260826T092617+0800.md`
- Execution source: `xiemingda-1002/vllm-plugin-FL@7beda84f59d7b25f49cdf03bdf6efecd771067ed`, tree `a81eea55c1de548a0a1f182f51089eca0b088c82`
- Official A3 carrier: `quay.io/ascend/vllm-ascend:v0.20.2rc1-a3-openeuler`
- Manifest digest: `sha256:442363921166771eb82baeec9c1ac0381f46fb830ead8d0e072df6e925f2a958`
- Arm64 platform digest: `sha256:a55b2b0597f9fdd1882de9bf3b7ebc395dd77c1ca49f251d0cb759d7b2c1a807`
- Evidence: `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-C-FLAGGEMS-DIAG/evidence/20260826T092617p0800`

### Why this baseline is required

Earlier Gate C used an insufficient/non-privileged carrier runtime mapping. In that environment:

```text
import torch_npu       -> succeeded
torch.npu.is_available -> False
torch.npu.device_count -> 0
```

`DeviceInfo()` therefore could not take its Ascend fast-path and fell through to the generic FlagGems detector, producing the misleading visible blocker:

```text
ModuleNotFoundError: No module named 'flag_gems'
```

After recreating the container with the User-provided privileged runtime access pattern, adapted to the formal A3 image and safe NPU scope:

```text
import torch_npu       -> succeeded
torch.npu.is_available -> True
torch.npu.device_count -> 2
device names           -> Ascend910_9382
DeviceInfo             -> vendor=ascend, type=npu, dispatch=PrivateUse1
```

`import vllm_fl.platform` then passed with `USE_FLAGGEMS=0` and without installing FlagGems. Gate D subsequently passed a real NPU `_C_ascend.npu_add_rms_norm_bias` smoke from the wheel-packaged `_C_ascend` and A3 OPP. Therefore the previous `flag_gems` error is classified as a **container/runtime mapping failure**, not evidence that Qwen runtime requires FlagGems.

### Required runtime-access pattern

For subsequent A3 tasks, reuse this pattern unless a later Accepted Result supersedes it. The image identity, task container name, and selected `davinciN` devices remain task-specific; do not blindly copy nightly tags or expose all cards.

```bash
# Example template only. Resolve IMAGE, CONTAINER and free NPU device IDs from
# the current authorized task/preflight before launch.
docker run -itd \
  --name="${CONTAINER}" \
  --privileged=true \
  --net=host \
  --device=/dev/davinci${NPU_A} \
  --device=/dev/davinci${NPU_B} \
  --device=/dev/davinci_manager \
  --device=/dev/devmm_svm \
  --device=/dev/hisi_hdc \
  -v /usr/local/dcmi:/usr/local/dcmi \
  -v /usr/local/Ascend/driver/tools/hccn_tool:/usr/local/Ascend/driver/tools/hccn_tool \
  -v /usr/local/bin/npu-smi:/usr/local/bin/npu-smi \
  -v /usr/local/Ascend/driver/lib64/:/usr/local/Ascend/driver/lib64/ \
  -v /usr/local/Ascend/driver/version.info:/usr/local/Ascend/driver/version.info \
  -v /etc/ascend_install.info:/etc/ascend_install.info \
  -v /etc/hccn.conf:/etc/hccn.conf \
  -v /data:/data \
  "${IMAGE}" \
  /bin/bash
```

For the successful Gate C/D run, only idle NPU 0/1 were exposed; NPU 2-7 had unrelated workloads and were intentionally not mapped. Future tasks must perform the same safe preflight and map only authorized free devices.

### Mandatory post-launch invariant

Before FL import/build/runtime diagnosis, verify the NPU runtime itself:

```bash
python - <<'PY'
import torch
import torch_npu  # noqa: F401

print("torch.npu.is_available =", torch.npu.is_available())
print("torch.npu.device_count =", torch.npu.device_count())
for i in range(torch.npu.device_count()):
    print(i, torch.npu.get_device_name(i))

assert torch.npu.is_available()
assert torch.npu.device_count() > 0
PY
```

For a task that intentionally maps two devices, the expected count is two. If `torch_npu` imports but `torch.npu.is_available()==False` or `device_count()==0`, treat the container/runtime access pattern as the first blocker. **Do not continue into FL/FlagGems diagnosis and do not install FlagGems to mask this condition.**

### What is fixed vs task-specific

Must preserve unless superseded by later Accepted evidence:

- privileged A3 runtime access (`--privileged=true`);
- host networking for the current server pattern;
- selected `davinciN` plus `/dev/davinci_manager`, `/dev/devmm_svm`, `/dev/hisi_hdc`;
- host Ascend driver/runtime mounts listed above;
- `/data:/data` for formal workspace/evidence/artifact access;
- explicit post-launch `torch.npu` availability/count validation.

Must be resolved per task rather than copied literally:

- image tag/digest (must match the task's frozen official baseline);
- container name;
- physical `davinciN` selection and visible-device environment;
- NPU count/topology;
- task workspace/cache mounts beyond `/data`;
- model/TP/graph/serve-specific environment variables.

The User example that led to the fix used `nightly-main-a3` and all eight cards, but those values are **not** part of this project baseline. The successful formal run retained the official `v0.20.2rc1-a3-openeuler` image and narrowed device scope to idle NPU 0/1.

## Explicitly non-transferable Qwen claims

即使 Qwen A3验证通过，也不得直接外推到 GLM：

- Qwen3.6 model patch / architecture / weight loader correctness；
- GDN、Mamba state/cache、Qwen full attention；
- Qwen MoE / expert map / EP2 correctness；
- Qwen-specific `FULL_DECODE_ONLY` capture/replay/state behavior；
- Qwen-specific OPP或 `_C_ascend`算子语义正确性；
- Qwen BF16 output correctness、prefix/64K/cache semantics；
- Qwen capacity、latency、throughput、HCCL tuning或 CPU binding性能；
- 任何 GLM-5.2-W8A8、MLA、DSA/SFA、Indexer、W8A8 Linear/MoE结论。

未来 GLM恢复时必须先做 vLLM 0.20.2 GLM contract review，再逐项引用本文件中已经 Accepted且语义通用的 A3基础。

## Validated handoff entries

当前无。新增 entry必须引用 immutable Result与 Acceptance commit，不得改写历史 entry来覆盖新 environment/source。
