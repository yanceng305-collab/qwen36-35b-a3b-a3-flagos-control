# A3 Runtime Handoff — Qwen Validation to Future GLM Work

更新时间：2026-08-28

当前状态：**STAGE 1-5 ACCEPTED, SCOPE-LIMITED**。真实A3/910C execution已完成A3-native wheel、standalone FL、TP2/HCCL BF16 eager、bounded `FULL_DECODE_ONLY [1,2,4,8]` graph和service/API correctness；A2-equivalent matrix和performance仍未验证。Qwen-specific模型/graph/service结论不得直接外推到GLM。

Stage 6及后续handoff统一绑定User Decision `D-030` Frozen Validation Baseline：source `e610a990d785356bf51a3cad50219d4c03310a31`、tree `609ff1ad0f08239f353cb4d8774e504b4deba03b`、Accepted wheel SHA-256 `2fcf788660f3fe42b364bc60d593ee1b9b634fc0632de58c444d961bff4aa1bd`。PR/branch future HEAD不得自动替换该identity。

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
| Base image / container pattern | **ACCEPTED — scope-limited** | Accepted composite runtime-access pattern；具体image/device/container按task解析 |
| CANN / Python / torch / torch-npu / vLLM 0.20.2 / Triton tuple | **ACCEPTED — exact tuple** | Accepted exact Stage 1/2 environment；version变化需重验 |
| A3 wheel build flow / `ascend910_93` detection | **ACCEPTED** | A3 wheel、family inventory、hash和A2-residue audit已审查 |
| `_C_ascend` build/package/load infrastructure | **ACCEPTED — smoke scope** | wheel origin、ABI、A3 load、one real NPU op accepted |
| CANN OPP build/package/expose infrastructure | **ACCEPTED — smoke scope** | OPP inventory、A3 family、runtime registration/execution accepted |
| HCCL / multiprocessing / device mapping | **ACCEPTED — Qwen DP1/TP2 scope** | Stage 3/4证明world size 2、both workers和bounded model/graph execution；新模型/拓扑仍重验 |
| PlatformFL / WorkerFL / ModelRunnerFL lifecycle | **ACCEPTED — Qwen Stage 3-5 scope** | standalone site-packages ownership、full model eager、bounded graph与service/API path通过 |
| Standalone FL formal installation | **ACCEPTED** | site-packages wheel origin、no source PYTHONPATH、no installed `vllm-ascend` accepted |
| Cache isolation / compiler identity | **ACCEPTED — bounded Qwen graph scope** | FL-local GraphWrapper + eager FX + NPUGraph、task cache root和`[1,2,4,8]` capture/replay通过；service/other shapes待验 |
| Evidence / immutable Result / reconstruction discipline | **ACCEPTED for Stage 1-5** | 完整三指针、checksum、immutable Results与Formal Reviews；Stage 6+适用final reproduction minimum |
| Runtime image/wheel/startup handoff | PARTIAL | wheel/PASS container已保留；new-server Result补充Stage 6 clean-container jemalloc compatibility prerequisite；最终 Stage 8 freeze/reconstruction尚未完成 |

## Accepted A3 container runtime baseline

Source execution:

- Task/run: `QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG` Gate C/D follow-up / `20260826T092617p0800`
- Immutable Result: `results/RESULT-QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG-GATEC-FOLLOWUP-20260826T092617+0800.md`
- Execution source: `xiemingda-1002/vllm-plugin-FL@7beda84f59d7b25f49cdf03bdf6efecd771067ed`, tree `a81eea55c1de548a0a1f182f51089eca0b088c82`
- Official A3 carrier: `quay.io/ascend/vllm-ascend:v0.20.2rc1-a3-openeuler`
- Manifest digest: `sha256:442363921166771eb82baeec9c1ac0381f46fb830ead8d0e072df6e925f2a958`
- Arm64 platform digest: `sha256:a55b2b0597f9fdd1882de9bf3b7ebc395dd77c1ca49f251d0cb759d7b2c1a807`
- Evidence: `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-C-FLAGGEMS-DIAG/evidence/20260826T092617p0800`
- Formal Acceptance: [`reviews/REVIEW-QWEN36-A3-STAGE1-2-ACCEPTANCE-20260826.md`](reviews/REVIEW-QWEN36-A3-STAGE1-2-ACCEPTANCE-20260826.md)
- Exact reconstruction: [`reconstruction/A3-STAGE1-2-ACCEPTED-RUNTIME.md`](reconstruction/A3-STAGE1-2-ACCEPTED-RUNTIME.md)

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

The successful run changed the composite runtime-access pattern as a group. No individual ablation proved that each flag or mount is independently necessary. Control therefore accepts the complete known-good pattern as the default operational baseline while keeping individual-causality claims out of scope.

### Required runtime-access pattern

For subsequent A3 tasks, reuse this pattern unless a later Accepted Result supersedes it. The image identity, task container name, and selected `davinciN` devices remain task-specific; do not blindly copy nightly tags or expose all cards.

```bash
# Example template only. Resolve IMAGE, CONTAINER and free NPU device IDs from
# the current authorized task/preflight before launch.
docker run -itd \
  --name="${CONTAINER}" \
  --privileged=true \
  --net=host \
  --device=/dev/davinci${NPU_0} \
  ${OPTIONAL_ADDITIONAL_DAVINCI_DEVICES} \
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
import os
import torch
import torch_npu  # noqa: F401

expected = int(os.environ["EXPECTED_MAPPED_DEVICE_COUNT"])
print("torch.npu.is_available =", torch.npu.is_available())
print("torch.npu.device_count =", torch.npu.device_count())
for i in range(torch.npu.device_count()):
    print(i, torch.npu.get_device_name(i))

assert torch.npu.is_available()
assert torch.npu.device_count() == expected
PY
```

For the accepted historical run, expected count was two. Future tasks set the expected count from their actual authorized mapping. If `torch_npu` imports but availability is false or count mismatches, treat the container/runtime access pattern as the first blocker. **Do not continue into FL/FlagGems diagnosis and do not install FlagGems to mask this condition.**

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

### Stage 6 clean-container jemalloc preload-path reconstruction

The frozen Stage 6 service config retains:

```text
LD_PRELOAD=/usr/lib/aarch64-linux-gnu/libjemalloc.so.2
```

The new-server run `20260827T173022+0800` used a fresh container from the exact Accepted image, observed `/usr/lib64/libjemalloc.so.2` but no frozen compatibility path, and stopped when the loader ignored the preload. The same run first passed the staged two-device NPU invariant on its exact host/container/device tuple.

Clean-container Stage 6 reconstruction must verify the image-provided library's file/realpath/ownership/hash/architecture/loader compatibility before an explicitly authorized Task restores the frozen path inside its Task-owned container. Keep the frozen preload string unchanged; do not modify the host, image, source, wheel or publish a derived image. Before service admission prove target resolution, non-generative loader activation, the NPU invariant and positive relevant-process jemalloc loading.

This is a [later-discovered Stage 6 reconstruction prerequisite](reconstruction/A3-STAGE1-2-ACCEPTED-RUNTIME.md#later-discovered-stage-6-jemalloc-compatibility-prerequisite), not a Stage 1/2 regression or new baseline. The [Formal Review](reviews/REVIEW-QWEN36-A3-S6-UFFFD-PROSPECTIVE-ROOT-CAUSE-DIAGNOSTIC-NEW-SERVER-20260827.md) does not claim that the historical successful container used the same symlink or preparation step.

The old-server invariant timeout remains historical and unresolved; the new-server positive invariant means it must not be generalized as a universal Frozen image/runtime regression.

Run `20260828T093900+0800` then proved the Task-owned compatibility method and frozen service admission on the exact new-host scope. The verified object SHA-256 was `2059f0cb5c2b3da8b834f4a54c12a633295eadb01844cef298398f350a2df43e`; the frozen preload string remained unchanged; loader and NPU checks passed. API, EngineCore and both TP workers mapped the verified jemalloc object, service readiness passed and automatic capture through 64 completed. See the [immutable Result](results/RESULT-QWEN36-A3-S6-JEMALLOC-RECONSTRUCTION-AND-UFFFD-ROOT-CAUSE-DIAGNOSTIC-20260828T093900+0800.md) and [Formal Acceptance](reviews/REVIEW-QWEN36-A3-S6-JEMALLOC-RECONSTRUCTION-AND-UFFFD-ROOT-CAUSE-DIAGNOSTIC-20260828.md).

This accepts the reconstruction/admission method only for the recorded Task-owned container. It does not prove the historical container used that symlink and does not change the Accepted image or Stage 1/2 baseline.

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

- **A3 Stage 1/2 runtime/build foundation**：ACCEPTED on 2026-08-26 for exact source `7beda84...`、official A3 openEuler carrier、`ascend910_93` wheel、standalone FL和 one real custom-op smoke。Evidence/claim boundary见 Formal Review和 reconstruction。
- **A3 Stage 3/4/5 Qwen execution layer**：ACCEPTED on 2026-08-26 for exact runtime source `e610a990...`、wheel SHA-256 `2fcf788...`、DP1/TP2 HCCL BF16 eager、`FULL_DECODE_ONLY [1,2,4,8]` bounded capture/replay/state correctness及bounded service/API lifecycle。可复用的是已验证的runtime ownership、two-worker lifecycle、graph infrastructure和clean service shutdown；Qwen model/GDN/Mamba/attention/MoE/service语义不向GLM外推。
- 新环境或 source不得覆盖本entry；使用新的 Result/Acceptance追加验证。
