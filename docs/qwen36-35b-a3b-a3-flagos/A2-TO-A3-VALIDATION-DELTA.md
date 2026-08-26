# A2 to A3 Validation Delta Ledger

Last updated：2026-08-26

Current formal cutoff：A3 Stage 3 TP2 BF16 eager `ACCEPTED`；Stage 4 `FULL_DECODE_ONLY` graph `READY / not executed`。

## Purpose / comparison contract

本项目的目标是将同事已在A2 / 2×Ascend 910B1上完成的Qwen3.6 FL适配，在A3 / 910C上重新验证。三类记录的责任边界是：

- [`A2-REFERENCE.md`](A2-REFERENCE.md) = **colleague documented baseline**；只保存同事在A2上记录的环境、功能、容量和性能事实。
- A3 immutable Results + Codex1 Formal Acceptance = **our A3 execution facts**；只对各自的exact source/tree/wheel/environment和覆盖边界成立。
- 本文件 = **delta ledger**；只记录A2 baseline与A3 validation之间的差异、原因分类和复现状态。

```text
A2 DOCUMENTED PASS
!= A3 EXECUTION PASS
!= A3 FORMAL ACCEPTANCE

bounded A3 correctness ACCEPTED
!= full A2-equivalent reproduction
```

本文不创建新Stage、Gate、Result或Acceptance，也不修改已有immutable Result的事实边界。

## Difference classification

| Classification | Meaning |
| --- | --- |
| `PLATFORM REQUIRED` | A2 → A3的硬件、SoC、image、driver/runtime access或build target必然变化；不等于implementation source fix |
| `VALIDATION BOUNDED` | 为分阶段correctness验证主动缩小或关闭的参数/能力；不代表最终交付配置变化 |
| `IMPLEMENTATION CHANGE` | 因A3实际blocker而修改production/runtime/model/operator/build implementation source |
| `SAME` | 与同事A2 baseline保持相同；A3是否已复验由A3 status列另行表示 |
| `NOT YET REVALIDATED` | A2已证明，但A3还没有对应execution/Acceptance |

当一项是因当前Task主动关闭而尚未复验时，`Delta type`记`VALIDATION BOUNDED`，`A3 status`记`NOT YET REVALIDATED`；不创造组合分类。

## Current difference ledger

### 1. Hardware

| A2 baseline | A3 validation | Delta type | A3 status |
| --- | --- | --- | --- |
| 2×Ascend 910B1 | Ascend 910C / `ascend910_93` family；当前TP2使用实际授权的2-device scope | `PLATFORM REQUIRED` | Stage 1/2硬件/family识别Accepted；Stage 3 TP2 scope Accepted |

历史Accepted run的具体device号不是future task固定参数；每次必须按当前授权和占用情况解析。

### 2. Base image

| A2 baseline | A3 validation | Delta type | A3 status |
| --- | --- | --- | --- |
| `quay.io/ascend/vllm-ascend:v0.20.2rc1` | `quay.io/ascend/vllm-ascend:v0.20.2rc1-a3-openeuler` | `PLATFORM REQUIRED` | Stage 1/2 Accepted；Stage 3复用同exact image/runtime |

Image是matched CANN/torch-npu/vLLM/Triton carrier；final runtime ownership仍必须归FL，不能仅由image名称推断。

### 3. Build target

| A2 baseline | A3 validation | Delta type | A3 status |
| --- | --- | --- | --- |
| `SOC_VERSION=ascend910b`；`prebuilt/ascend910b1` | Accepted build effective family `SOC_VERSION=ascend910_93`；`prebuilt/ascend910_93` | `PLATFORM REQUIRED` | A3-native wheel build/install/custom-op Accepted；strict A2 binary residue search=0 |

A2 binary wheel不可用于A3。当前source也支持不显式设`SOC_VERSION`时选择A3 default `ascend910_93`；本表记录Accepted run的effective target，不把配置差异写成source fix。

### 4. Final runtime ownership

| A2 baseline | A3 validation | Delta type | A3 status |
| --- | --- | --- | --- |
| `VLLM_PLUGINS=fl`；`vllm-ascend` uninstalled；`USE_FLAGGEMS=0`；PlatformFL → WorkerFL → ModelRunnerFL → FL-local Ascend | Same contract | `SAME` | **A3 REVALIDATED / Stage 3 ACCEPTED** |

A3 Evidence还证明`vllm_ascend`不可import、FlagGems absent、`vllm_fl`来自wheel/site-packages，并覆盖FL-local Qwen/GDN/Mamba/full-attention/MoE live forward。

### 5. Runtime/device mapping

| A2 baseline | A3 validation | Delta type | A3 status |
| --- | --- | --- | --- |
| A2 documented container/device access | Accepted `--privileged=true` + host network + authorized davinci devices + manager/devmm/HDC + required host Ascend runtime/driver mounts + `/data:/data`；显式visible-device scope | `PLATFORM REQUIRED` | Stage 1/2 and Stage 3 Accepted |

这是A3 execution environment requirement，不是同事production source implementation change。完整reconstruction见[`A3-STAGE1-2-ACCEPTED-RUNTIME.md`](reconstruction/A3-STAGE1-2-ACCEPTED-RUNTIME.md)。启动后必须硬验证`torch.npu.is_available() == True`且`device_count == actual mapped/visible count`。

### 6. Model / dtype / parallelism

| A2 baseline | A3 validation | Delta type | A3 status |
| --- | --- | --- | --- |
| Qwen3.6-35B-A3B；BF16 non-quantized；DP1/TP2 first | Same model contract；26/26 shards，1045/1045 tensors BF16；DP1/TP2 | `SAME` | **A3 REVALIDATED / Stage 3 ACCEPTED** |

### 7. Eager validation scope

| A2 baseline | A3 validation | Delta type | A3 status |
| --- | --- | --- | --- |
| 8个short prompts、2个repeat prompts、1个1,215-token prompt，以及更宽功能覆盖 | `max_model_len=2048`，`max_num_seqs=1`，2个不同short prompts，每个8 output tokens，2次generation；prefix/chunked prefill/MTP/quantization off | `VALIDATION BOUNDED` | Bounded TP2 BF16 eager **ACCEPTED** |

Stage 3 `ACCEPTED` **不等于** 完整复现A2 eager functional matrix。A3尚未执行A2的same-prompt repeat gate或1,215-token prompt覆盖。

### 8. `FULL_DECODE_ONLY` graph

| A2 baseline | A3 validation | Delta type | A3 status |
| --- | --- | --- | --- |
| Historical bounded capture `[1,2,4,8]` PASS；final `max_num_seqs=64` service自动capture `[1,2,4,8,16,24,32,40,48,56,64]` | Stage 4 first correctness Task只验证`[1,2,4,8]` capture/replay/state correctness | `VALIDATION BOUNDED` | **NOT YET REVALIDATED**；Stage 4 READY / not executed |

即使Stage 4 PASS，也只能声称bounded graph correctness，不能声称已复现A2 final service capture matrix。

### 9. Chunked prefill

| A2 baseline | A3 validation | Delta type | A3 status |
| --- | --- | --- | --- |
| vLLM 0.20.2自动启用，并在eager/graph/EP/concurrency多场景覆盖 | Stage 3和首个Stage 4 Task显式关闭 | `VALIDATION BOUNDED` | **NOT YET REVALIDATED** |

### 10. Prefix caching

| A2 baseline | A3 validation | Delta type | A3 status |
| --- | --- | --- | --- |
| aligned Mamba prefix caching有short lifecycle和65,536-token long graph Evidence | 尚未执行 | `NOT YET REVALIDATED` | A3 UNVERIFIED |

### 11. EP2

| A2 baseline | A3 validation | Delta type | A3 status |
| --- | --- | --- | --- |
| eager/graph correctness Evidence；256 experts按rank分片 | 尚未执行 | `NOT YET REVALIDATED` | A3 UNVERIFIED |

### 12. Long context / functional matrix

| A2 baseline | A3 validation | Delta type | A3 status |
| --- | --- | --- | --- |
| `1K/4K/16K/64K × C1/C8/C32/C64`，O1024，16/16 strict pass | 尚未复现 | `NOT YET REVALIDATED` | A3 UNVERIFIED |

### 13. Performance / capacity

| A2 baseline | A3 validation | Delta type | A3 status |
| --- | --- | --- | --- |
| FL/native A/B、KV capacity、clean/persistent startup、warm-up和throughput数据已记录 | 尚未测试 | `NOT YET REVALIDATED` | A3 UNVERIFIED；无A3 performance/capacity结论 |

详细A2数字只在[`A2-REFERENCE.md`](A2-REFERENCE.md)保存。本文不重复粘贴，也不将A2 `104.824%`、KV token capacity或startup/warm-up数字用作A3结论。

### 14. Implementation source changes

Current conclusion：**IMPLEMENTATION CHANGE: NONE SO FAR**

精确边界：

- 截至Stage 3 Formal Acceptance，本A3 validation没有修改production runtime/model/operator/build implementation source，没有创建Code fork/patch/PR。
- Stage 3在detached clean `e610a990d785356bf51a3cad50219d4c03310a31` / tree `609ff1ad0f08239f353cb4d8774e504b4deba03b`构建Accepted wheel；Code PR=`N/A`，wheel SHA-256 `2fcf788660f3fe42b364bc60d593ee1b9b634fc0632de58c444d961bff4aa1bd`。
- Stage 1/2 Accepted source `7beda84...`到Stage 3 source `e610a990...`的变化是tracked implementation branch的既有moving-head commits；Control对diff定义了bounded regression并重建/执行，不将它写成A3 execution发现后由本项目产生的source fix。
- 后续`e610a990... → 032fddc9...`已由[`moving-head Review`](reviews/REVIEW-QWEN36-A3-MOVING-HEAD-032FDDC9-20260826.md)确认为docs/tests-only，不改该结论。

Evidence：[`Stage 3 Formal Acceptance`](reviews/REVIEW-QWEN36-A3-STAGE3-TP2-BF16-EAGER-ACCEPTANCE-20260826.md)、[`initial Result`](results/RESULT-QWEN36-A3-S3-TP2-BF16-EAGER-20260826T112011+0800.md)、[`resume Result`](results/RESULT-QWEN36-A3-S3-TP2-BF16-EAGER-RESUME-20260826T115234+0800.md)。

若未来出现真实A3-specific source change，必须在本节按下表增加，不能只写“A3 fix”：

| File / function | Old behavior | A3 problem | Change | Why required | Evidence | Resulting commit / PR | A2 behavior affected? |
| --- | --- | --- | --- | --- | --- | --- | --- |
| _None so far_ | - | - | - | - | - | - | - |

## Reproduction status

| Capability | A2 baseline | A3 status | Delta type | Evidence / Acceptance |
| --- | --- | --- | --- | --- |
| A3-family build / standalone custom-op foundation | A2 build/runtime PASS | **ACCEPTED** on A3-native wheel | `PLATFORM REQUIRED` | [`Stage 1/2 Review`](reviews/REVIEW-QWEN36-A3-STAGE1-2-ACCEPTANCE-20260826.md) |
| Standalone FL runtime ownership | PASS | **ACCEPTED / REVALIDATED** | `SAME` | Stage 1/2 + [`Stage 3 Review`](reviews/REVIEW-QWEN36-A3-STAGE3-TP2-BF16-EAGER-ACCEPTANCE-20260826.md) |
| Qwen3.6-35B-A3B BF16 DP1/TP2 eager | PASS | **ACCEPTED**, bounded short-prompt scope | `VALIDATION BOUNDED` | [`Stage 3 Review`](reviews/REVIEW-QWEN36-A3-STAGE3-TP2-BF16-EAGER-ACCEPTANCE-20260826.md) |
| Complete BF16 weight load | PASS | **ACCEPTED**；26/26 shards, 1045/1045 BF16 tensors | `SAME` | Stage 3 Review |
| TP2/HCCL | PASS | **ACCEPTED**；world size 2 | `SAME` | Stage 3 Review |
| GDN/Mamba/full-attention/MoE full-model live forward | PASS | **ACCEPTED**, bounded eager path | `VALIDATION BOUNDED` | Stage 3 Review |
| `FULL_DECODE_ONLY` `[1,2,4,8]` | PASS | Pending Stage 4 | `VALIDATION BOUNDED` | [`Stage 4 Task`](tasks/QWEN36-A3-S4-FULL-DECODE-ONLY-GRAPH.md) |
| Final service capture through 64 | PASS | Not yet | `NOT YET REVALIDATED` | - |
| Chunked prefill | PASS | Disabled in current bounded Tasks; not yet | `VALIDATION BOUNDED` | Stage 3 Review / Stage 4 Task |
| Prefix caching | PASS | Not yet | `NOT YET REVALIDATED` | - |
| EP2 eager/graph | PASS | Not yet | `NOT YET REVALIDATED` | - |
| Serve/API | PASS as part of A2 service results | Not yet | `NOT YET REVALIDATED` | - |
| 1K/4K/16K/64K × C1/C8/C32/C64 | PASS | Not yet | `NOT YET REVALIDATED` | - |
| Long aligned-prefix lifecycle | PASS | Not yet | `NOT YET REVALIDATED` | - |
| Functional matrix | PASS | Not yet | `NOT YET REVALIDATED` | - |
| Performance / capacity A/B | Recorded | Not yet | `NOT YET REVALIDATED` | - |

## What A3 has reproduced so far

- A3/910C family identification and the official A3 openEuler carrier environment;
- A3-native `ascend910_93` wheel build, standalone install, `_C_ascend`/OPP ownership and one real NPU custom-op smoke;
- no installed/runtime `vllm-ascend`, no `vllm_ascend` import, no FlagGems activation;
- Qwen3.6-35B-A3B BF16 non-quantized complete model identity and full 26-shard load;
- DP1/TP2 HCCL eager execution with two workers/ranks;
- bounded full-model prefill + multi-token decode covering Qwen hybrid GDN/Mamba/full-attention/MoE live paths;
- two different short-prompt generations with nonempty/readable/distinct output, finite logprobs and final NPU synchronize.

这些项只在各自Formal Acceptance的exact source/wheel/model/environment和bounded workload内成立。

## A2 capabilities not yet reproduced on A3

- `FULL_DECODE_ONLY` graph capture/replay/state correctness（Stage 4 pending）；
- final service automatic capture sizes through 64；
- serve health/models/completion/chat及bounded concurrency；
- chunked prefill and async scheduling；
- aligned Mamba prefix caching，包括short/long hit/reset lifecycle；
- EP2 eager/graph correctness and expert sharding behavior；
- A2更宽eager prompts，same-prompt repeat与1,215-token prompt；
- 1K/4K/16K/64K × C1/C8/C32/C64, O1024 functional matrix；
- A2 long-context and 65,536-token aligned-prefix scenarios；
- capacity, cold/persistent startup, warm-up, latency, throughput and FL/native performance A/B；
- A2其他未单独列入A3 Formal Acceptance的defaults/features，包括CPU binding、memory profiling及graph task-update等的A3-specific correctness/performance结论。

## Maintenance rules

- 每次新的Formal Acceptance后，Codex1检查本delta ledger是否需要更新。
- Immutable Result仍保持immutable；不因ledger状态变化修改历史Result。
- 本文只总结已正式存在的A2 documented facts和A3 Result/Acceptance，不制造新Acceptance。
- 不重复粘贴`A2-REFERENCE.md`的完整性能数据；使用链接引用。
- 当A3参数只因阶段性Task关闭时，记`VALIDATION BOUNDED`，不误写为最终配置差异。
- 当环境差异只是SoC/image/runtime access要求时，记`PLATFORM REQUIRED`，不误写为implementation change。
- 只有A3 blocker导致production source修改并形成commit/PR/Evidence时，才新增`IMPLEMENTATION CHANGE`。
- 本文不新增Stage、Gate或其他维护流程。
