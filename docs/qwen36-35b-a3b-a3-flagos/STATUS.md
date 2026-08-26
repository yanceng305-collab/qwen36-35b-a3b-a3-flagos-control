# 项目状态

更新时间：2026-08-26

总体状态：A3 Stage 1/2、Stage 3 TP2 BF16 eager、Stage 4 bounded `FULL_DECODE_ONLY [1,2,4,8]` graph均已 **ACCEPTED**；Stage 5 service correctness已 **Execution PASS**。Runtime artifact identity保持`e610a990...` Accepted wheel；tracked `032fddc9...`只作为docs/tests-only moving head记录。

## 当前快照

| Work item | Status | Evidence boundary |
| --- | --- | --- |
| Control repo | Established | 本仓库 `main`；首次 Control commit |
| Tracked implementation | Current GitHub snapshot reverified | `feature/qwen3.6-35b-a3b-ascend-graph-migration@032fddc9...` / tree `463806ef...`；Stage 3 Acceptance仍绑定`e610a990...` / accepted wheel |
| Official base | Current GitHub snapshot recorded | `release/0.2@ef78dec...` / tree `7414bac...`；moving fact |
| PR #404 | OPEN / DRAFT；head `032fddc9...` | Stage 4 Formal Review live recheck 2026-08-26；mergeability/base是moving facts，dispatch前重查 |
| A2 implementation evidence | **A2 REFERENCE ONLY** | User资料中的 2×910B1结果，不是 A3 Acceptance |
| A3 environment/build/runtime | **ACCEPTED — Stage 1/2 scope** | A3 environment、native wheel、standalone FL、PlatformFL import和 one real custom-op smoke Accepted；[Formal Review](reviews/REVIEW-QWEN36-A3-STAGE1-2-ACCEPTANCE-20260826.md) |
| A3 model TP2 BF16 eager | **ACCEPTED — Stage 3 scope** | exact `e610a990...`完成current-head regression、模型identity、TP2/HCCL、完整BF16权重加载、prefill/decode和两次generation；[Formal Review](reviews/REVIEW-QWEN36-A3-STAGE3-TP2-BF16-EAGER-ACCEPTANCE-20260826.md) |
| A3 graph | **ACCEPTED — Stage 4 bounded graph correctness** | `FULL_DECODE_ONLY [1,2,4,8]`两TP rank capture/replay/state gate通过；[Formal Review](reviews/REVIEW-QWEN36-A3-STAGE4-FULL-DECODE-ONLY-GRAPH-ACCEPTANCE-20260826.md) |
| A3 serve/function/performance | **Stage 5 serve correctness PASS / Acceptance pending** | Stage 5在真实 OpenAI-compatible vLLM service/API 路径中通过 health/models/completion/chat/repeat/bounded concurrency/replay/state/shutdown gates；尚未进入 functional matrix 或 performance |
| Official A3 base route | Bounded selection authorized | `v0.20.2rc1-a3` Ubuntu或`v0.20.2rc1-a3-openeuler`；ordinary unsuffixed A2 image excluded |
| Model artifact | **Gate M PASS** | `/data/tiankuan/zyg/FL/workspace/Qwen3.6-35B-A3B`；root 26/26 shards present，1045/1045 safetensors tensors BF16，no quantization，no download markers；checksum manifest saved |
| First Codex2 task | **STOP / initial NEEDS-FOLLOWUP closed by Accepted chain** | [Immutable Result](results/RESULT-QWEN36-A3-S1S2-ENV-BUILD-RUNTIME-20260825T205424+0800.md)；[Initial Review](reviews/REVIEW-QWEN36-A3-S1S2-ENV-BUILD-RUNTIME-20260825.md) |
| Latest bounded Task | **Execution PASS - Stage 5 Serve Correctness / Review pending** | [`QWEN36-A3-S5-SERVE-CORRECTNESS`](tasks/QWEN36-A3-S5-SERVE-CORRECTNESS.md)；Stage 4 Accepted FULL_DECODE_ONLY graph在真实 vLLM service/API 路径中通过 health/models/completion/chat/repeat/bounded concurrency/replay/state/shutdown gates；不进入 matrix 或 performance |
| Next Task | **READY / Awaiting explicit User dispatch** | Post-serve functional matrix/performance follow-up尚未创建；当前仅保留 Stage 5 service correctness 结果 |
| Validation Code repo/fork | **Not needed** | Stage 1/2 blockers均以 non-source route闭合；implementation source未修改 |
| GLM project | PAUSED by User Decision | 独立 Control；旧 Evidence/history保留，不写入本仓库 |

## 当前 implementation identity

核验时间：2026-08-26 bounded moving-head Formal Review live recheck。

- Tracked repo/branch：`xiemingda-1002/vllm-plugin-FL` / `feature/qwen3.6-35b-a3b-ascend-graph-migration`。
- Current head：`032fddc91b6d013b98aed8e64ff05b54d1435648`。
- Current tree：`463806ef18e5e31006cd4f59e6a5261fc65cea4a`。
- Stage 1/2 Accepted execution source：`7beda84f59d7b25f49cdf03bdf6efecd771067ed` / tree `a81eea55c1de548a0a1f182f51089eca0b088c82`。
- Official base/ref：`flagos-ai/vllm-plugin-FL:release/0.2`。
- Current release HEAD/tree（Stage 4 Formal Review live recheck）：`ef78dec66fea1ae858ef414584be1478929ee9b2` / `7414bac41c39bc445b0cc05dbdaecc0f08231aeb`。
- PR #404仍以`032fddc9...`为head；PR snapshot base SHA和moving `release/0.2` branch HEAD可短暂不同，dispatch前均重新查询。
- Branch movement：Stage 3 Accepted `e610a990...`后以1个commit前进到`032fddc9...`。Exact diff只有`README.md` +11和新增`tests/unit_tests/test_build_config.py` +66；[`Formal moving-head Review`](reviews/REVIEW-QWEN36-A3-MOVING-HEAD-032FDDC9-20260826.md)确认为docs/tests-only。

上述snapshot已对`032fddc9...`完成bounded disposition。Stage 5 dispatch前仍必须重新查询；若HEAD/tree继续为`032fddc9...` / `463806ef...`，复用`e610a990...` Accepted wheel；若再次变化，先STOP并返回新diff。

## Historical initial Stage 1/2 execution result

Task：[tasks/QWEN36-A3-S1S2-ENV-BUILD-RUNTIME.md](tasks/QWEN36-A3-S1S2-ENV-BUILD-RUNTIME.md)

目标是一次有边界的 Stage 1/2 gate：

```text
A3 physical/environment identity
  -> current tracked source exact identity
  -> clean A3-family wheel build
  -> formal standalone FL install
  -> A3 _C_ascend / OPP identity
  -> minimal A3 NPU custom-op/runtime smoke
```

首任务未运行完整 Qwen模型、graph、prefix、EP、64K 或 benchmark。该 initial run 未达到 `Execution PASS`；Codex1 Formal Review当时为 `NEEDS-FOLLOWUP`。后续 diagnostic/follow-up chain已闭合 Stage 1/2，并由 Codex1 Formal Acceptance解锁 Stage 3。

本次 run 事实：

- Gate A：`PASS`。
- Gate A Codex1 Review：**ACCEPT WITH EVIDENCE GAP**；core device/image/environment/source identity接受，Triton/provider子项待补证。
- Gate B：`STOP`。
- Gate B Codex1 Review：**STOP ACCEPTED**；不等于 Gate B PASS。
- First blocker：`OpFileNotExistsError: File aic-*-ops-info.ini does not exist`。
- Source：`7beda84f59d7b25f49cdf03bdf6efecd771067ed` / tree `a81eea55c1de548a0a1f182f51089eca0b088c82`。
- Official PR base correction：`flagos-ai/vllm-plugin-FL:release/0.2@53adefb269571684d83a51e997d3ba9be5f88235` / tree `9ddfd080953ad39b39772e108ff921d2973b0299`；immutable Result中的 fork `main@38e7dbc...`不是 official PR compare base，但不影响 execution HEAD/tree或 STOP事实。
- Selected image：`quay.io/ascend/vllm-ascend:v0.20.2rc1-a3-openeuler`，image digest `sha256:442363921166771eb82baeec9c1ac0381f46fb830ead8d0e072df6e925f2a958`，arm64 platform digest `sha256:a55b2b0597f9fdd1882de9bf3b7ebc395dd77c1ca49f251d0cb759d7b2c1a807`，local image ID `sha256:8e7823878349b37d9900984555e28381c25681f88ff678cb4a86f7d1674a67c1`。
- No wheel produced；Gate C/D `NOT RUN`。
- Task container `qw36-a3-s1s2-env-pass-20260825190917` / `9f03ddc88115aec2865ae099596f8cd383a2647493397b72ce1f8f82d6c66adb` removed；NPU 0/1 released。
- Code PR：`N/A`。
- Evidence root：`/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S1S2-ENV-BUILD-RUNTIME/evidence`；main build log `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S1S2-ENV-BUILD-RUNTIME/evidence/20260825T205100+0800/build_wheel.log`。
- Immediate failure confidence：**HIGH**；metadata lookup未找到 `aic-*-ops-info.ini`，exception未恢复、exit 1、no wheel。
- Underlying root-cause confidence：**LOW / NOT CONFIRMED**；尚不能区分具体 op、CMake/opbuild、CANN behavior、SoC/path naming、selected-op flow或 lookup-directory问题，也未证明 source change必要。
- Gate A Evidence gap：Result只记录 `Triton=3.5.0`，未记录 `triton-ascend` distribution、module origin和 active provider。ARM上 `triton-ascend 3.2.1`可以正常依赖 community `triton 3.5.0`，故不判 wrong tuple，只要求 targeted补证。
- Immutable Result contract gap：缺显式 `Root-cause confidence`字段；只在 Review/STATUS/INDEX记录，不修改 immutable Result。

## Current authorization / dispatch gate

User已确认 bounded authorization：

- Codex2可在当前项目可访问的 A3/910C server上执行；若实际可见 target不唯一或 access无效，任何 mutation前 STOP并请求澄清。
- 先只读盘点 NPU型号、logical mapping、health、occupancy和 owner，再选择不干扰其他任务的最小 safe scope；不得 kill/pause/reset/preempt。
- 只在 official `v0.20.2rc1-a3`与`v0.20.2rc1-a3-openeuler`中做 base selection；可 pull/inspect、创建和清理本 Task自己的 container。两者均不兼容时 STOP / Decision requested。
- 可在现有 `/data`创建新的 Qwen Validation专属 work/Evidence/artifacts/cache目录，参考 `/data/tiankuan/zyg/FL/`，但不得覆盖既有目录或写入模型目录；返回 exact paths。
- 可使用现有 GitHub/package index/container registry/CATLASS访问；离线 artifact必须可核验，CATLASS绑定 exact `41bf90da655bba3c66d0acd7e00abe33960ecfd6`。

Stage 1/2、Stage 3和Stage 4 Formal Acceptance均已完成。Stage 5 serve correctness已执行并形成 Execution PASS；只有User显式dispatch后Codex2才可进入后续 functional matrix / performance follow-up。

## Current Stage 3 run — QWEN36-A3-S3-TP2-BF16-EAGER

Result：[`RESULT-QWEN36-A3-S3-TP2-BF16-EAGER-20260826T112011+0800.md`](results/RESULT-QWEN36-A3-S3-TP2-BF16-EAGER-20260826T112011+0800.md)

Resume Result：[`RESULT-QWEN36-A3-S3-TP2-BF16-EAGER-RESUME-20260826T115234+0800.md`](results/RESULT-QWEN36-A3-S3-TP2-BF16-EAGER-RESUME-20260826T115234+0800.md)

本次 run 事实：

- Gate R：`PASS`；复用 Accepted runtime container `qw36-a3-s2-gatec-priv-20260826T092617p0800` / `32562c7139600c25e570ec07841713737b0407c7d1bbdc563be41b87ea105f0a`，current-head `e610a990d785356bf51a3cad50219d4c03310a31` / tree `609ff1ad0f08239f353cb4d8774e504b4deba03b` clean rebuild成功。
- Current-head wheel：`/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S3-TP2-BF16-EAGER/artifacts/20260826T112011p0800/wheels/vllm_plugin_fl-0.2.0+ge610a990d-cp311-cp311-linux_aarch64.whl`，sha256 `2fcf788660f3fe42b364bc60d593ee1b9b634fc0632de58c444d961bff4aa1bd`。
- Gate R standalone：`PASS`；`vllm-ascend` absent、`vllm_ascend`不可import、`vllm_fl`来自 site-packages wheel、`USE_FLAGGEMS=0`，A3 `_C_ascend`/OPP来自 `ascend910_93` wheel。
- Gate R custom-op：`PASS`；`torch.ops._C_ascend.npu_add_rms_norm_bias` 在 A3 NPU执行，BF16 input/output在 `npu:0`，reference/synchronize/no-fallback checks通过。
- Gate M：`STOP`；模型路径 `/data/tiankuan/zyg/FL/workspace/Qwen3.6-35B-A3B`存在，config/tokenizer/index存在，index引用26个shards，但 root缺少4个required shards：`model-00016-of-00026.safetensors`、`model-00018-of-00026.safetensors`、`model-00022-of-00026.safetensors`、`model-00025-of-00026.safetensors`；对应文件仅在 `._____temp/`。
- Gate E：`NOT RUN`；未执行TP2/HCCL初始化、model construction、weight load、prefill/decode、generation、graph、serve、benchmark或profiling。
- Code PR：`N/A`；implementation source未修改。
- Evidence root：`/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S3-TP2-BF16-EAGER/evidence/20260826T112011p0800`；main build log `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S3-TP2-BF16-EAGER/evidence/20260826T112011p0800/logs/gate_r_build_wheel.log`。

Resume run事实：

- Execution：`Execution PASS - Stage 3 TP2 BF16 Eager`；Codex1 Formal Acceptance现为 **ACCEPTED**。
- Gate R：`PASS`；复用同一 source/current-head wheel evidence，wheel sha256 `2fcf788660f3fe42b364bc60d593ee1b9b634fc0632de58c444d961bff4aa1bd`。
- Gate M：`PASS`；模型root 26/26 index shards存在，`._____temp/`为空，无download markers；config为 `Qwen3_5MoeForConditionalGeneration`，40 layers、30 linear_attention + 10 full_attention、256 experts/top-8；1045 tensors header-only审计全部 `BF16`，无quantization。
- Gate E：`PASS`；`tensor_parallel_size=2`、DP1、BF16、`enforce_eager=True`、`cudagraph_mode=NONE`、`enable_prefix_caching=False`、`enable_chunked_prefill=False`、MTP/quantization off；HCCL `world_size=2` backend `hccl`；26/26 safetensors权重加载完成；两次generation输出非空且不同，logprobs finite，最终 `torch.npu.synchronize()`成功。
- Output samples：`Hello, my name is` -> ` John. I am a 30`；`The capital of France is` -> ` Paris, a city renowned for its rich`。
- Platform/runtime：`vllm-ascend` absent，`vllm_ascend`不可import，FlagGems absent且 `USE_FLAGGEMS=0`；PlatformFL device `npu` / dispatch `PrivateUse1`，WorkerFL/ModelRunnerFL来自 site-packages `vllm_fl`。
- Preserved container：`qw36-a3-s2-gatec-priv-20260826T092617p0800` / `32562c7139600c25e570ec07841713737b0407c7d1bbdc563be41b87ea105f0a`，保留current-head standalone FL环境和wheel。
- Evidence root：`/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S3-TP2-BF16-EAGER/evidence/20260826T115234p0800`；Gate E log `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S3-TP2-BF16-EAGER/evidence/20260826T115234p0800/runtime/gate_e_tp2_bf16_eager_file.log`。

Formal Review：[`REVIEW-QWEN36-A3-STAGE3-TP2-BF16-EAGER-ACCEPTANCE-20260826.md`](reviews/REVIEW-QWEN36-A3-STAGE3-TP2-BF16-EAGER-ACCEPTANCE-20260826.md)。Stage 3 Acceptance绑定exact `e610a990...` / tree `609ff1ad...`和current-head wheel sha256 `2fcf788...`；Stage 4随后完成bounded graph Acceptance。

## Current Stage 4 run — QWEN36-A3-S4-FULL-DECODE-ONLY-GRAPH

Result：[`RESULT-QWEN36-A3-S4-FULL-DECODE-ONLY-GRAPH-20260826T141740+0800.md`](results/RESULT-QWEN36-A3-S4-FULL-DECODE-ONLY-GRAPH-20260826T141740+0800.md)

本次 run 事实：

- Execution：`Execution PASS - Stage 4 FULL_DECODE_ONLY Graph`；Codex1 Formal Acceptance为 **ACCEPTED — bounded graph correctness**。
- Current tracked source：`032fddc91b6d013b98aed8e64ff05b54d1435648` / tree `463806ef18e5e31006cd4f59e6a5261fc65cea4a`；dispatch recheck匹配已审查docs/tests-only disposition。
- Runtime artifact：复用Stage 3 Accepted `e610a990d785356bf51a3cad50219d4c03310a31` / tree `609ff1ad0f08239f353cb4d8774e504b4deba03b` wheel，sha256 `2fcf788660f3fe42b364bc60d593ee1b9b634fc0632de58c444d961bff4aa1bd`；未重build。
- Gate G0：`PASS`；preserved container `qw36-a3-s2-gatec-priv-20260826T092617p0800` / `32562c7139600c25e570ec07841713737b0407c7d1bbdc563be41b87ea105f0a`继续运行，`torch_npu`可用，visible device count 2，standalone FL/site-packages、A3 `_C_ascend`/OPP、`vllm-ascend` absent、`vllm_ascend`不可import、FlagGems absent、model BF16 identity均通过。
- Gate G1：`PASS`；`enforce_eager=False`、`compilation_config.mode=VLLM_COMPILE`、`backend=eager`、`cudagraph_mode=FULL_DECODE_ONLY`、capture sizes `[1,2,4,8]`；两个TP worker均完成四个size capture，graph type为Ascend `NPUGraph`，GraphWrapper来自 `vllm_fl`。
- Gate G2：`PASS`；prefill/non-uniform路径为 `forward_mode=NONE` / eager passthrough，decode replay为 `forward_mode=FULL` / `phase=replay`；batch size 1和2均有多token replay，同一greedy prompt重复两次token/text一致，不同prompt输出非空且不同，logprobs finite，final `torch.npu.synchronize()`成功，process exit 0。
- NPU 0/1 final released；NPU 2-7 unrelated workloads未触碰。
- Formal Review：[`REVIEW-QWEN36-A3-STAGE4-FULL-DECODE-ONLY-GRAPH-ACCEPTANCE-20260826.md`](reviews/REVIEW-QWEN36-A3-STAGE4-FULL-DECODE-ONLY-GRAPH-ACCEPTANCE-20260826.md)。
- Stage 5：`Execution PASS`；serve已完成 health/models/completion/chat/repeat/bounded concurrency/replay/state/shutdown gates；prefix/EP2/64K/matrix/performance仍未执行。
- Code PR：`N/A`。
- Evidence root：`/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S4-FULL-DECODE-ONLY-GRAPH/evidence/20260826T141740p0800`；main graph log `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S4-FULL-DECODE-ONLY-GRAPH/evidence/20260826T141740p0800/runtime/gate_g1_g2_full_decode_only_graph.log`。

## Current diagnostic run — QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG

Result：[`RESULT-QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG-20260825T224528+0800.md`](results/RESULT-QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG-20260825T224528+0800.md)

本次 run 事实：

- Gate A supplement：`PASS`；补齐 `triton-ascend 3.2.1`、community `triton 3.5.0`、imported `triton` module origin和 Ascend backend/provider identity。
- Parent Gate B blocker：exact source/image/SoC上可复现，`OpFileNotExistsError: File aic-*-ops-info.ini does not exist`。
- Root-cause confidence：`HIGH` for parent metadata blocker。源路径中的 `+0800`进入 `custom_build.cmake`未转义的 `string(REGEX MATCH "^${CMAKE_CURRENT_SOURCE_DIR}" ...)`正则，导致 selected op def分类失败、`opbuild_gen_default/inner/exc`不包含 `op_build`命令，进而 `ascendc_impl_build.py`在 lookup dirs找不到 `aic-*-ops-info.ini`。
- Corrected no-plus path attempt：同一 exact SHA/tree、同一 official A3 openEuler image、无 source patch；生成了 `aic-ascend910_93-ops-info.ini`和相关 `aclnn_*.cpp/h`、`*_proto.*`文件，证明 parent metadata blocker被非源码路径修正清除。
- Remaining Gate B blocker：corrected attempt随后因 `gitcode.com` third-party `json`/`abseil-cpp` DNS下载失败停止；无 wheel产出。
- Gate C/D：`NOT RUN`。
- Task container：`qw36-a3-s2-gateb-opp-diag-20260825T224528p0800` / `da11e8d0139824d72db50b6660a3818999202ea98e3688984d082fb964287497`已删除；NPU 0/1释放。
- Code PR：`N/A`；implementation source未修改。
- Evidence root：`/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T224528+0800`；main build log `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T224528+0800/logs/gate_b_corrected_no_plus_path_build_wheel.log`。

该 diagnostic run当时 Stage 1/2仍未 PASS；后续 follow-up已闭合。

## Current follow-up run — QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG

Result：[`RESULT-QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG-FOLLOWUP-20260825T234607+0800.md`](results/RESULT-QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG-FOLLOWUP-20260825T234607+0800.md)

本次 run 事实：

- Gate A supplement：`PASS`；复核 official A3 openEuler image、Triton/provider identity和 NPU 0/1 safe scope。
- Source：继续绑定 parent exact `7beda84f59d7b25f49cdf03bdf6efecd771067ed` / tree `a81eea55c1de548a0a1f182f51089eca0b088c82`；follow-up开始时记录 moving tracked branch已到 `e610a990d785356bf51a3cad50219d4c03310a31`，但不替换本次 reproduction source。
- Network correction：使用服务器现有代理后，container内两个 `gitcode.com`依赖 GET probe均返回 HTTP 200。
- Gate B：`PASS`；产出 `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/artifacts/20260825T234607p0800/wheels/vllm_plugin_fl-0.2.0+g7beda84f5-cp311-cp311-linux_aarch64.whl`，sha256 `fa33f586b2e56e78f671989e6dc3dc2ee23f5005c5f0cc4800a6cf6e4b2e98c1`；wheel包含 `ascend910_93` prebuilt `_C_ascend`/OPP，严格 `ascend910b`/`ascend910b1`搜索为0。
- Gate C：`STOP`；required standalone uninstall/install transaction成功，`vllm-ascend` distribution absent、`vllm_ascend` import不可用、`vllm_fl`来自 site-packages wheel，`_C_ascend`和 OPP来自本次 wheel；但 `import vllm_fl.platform` 触发 `ModuleNotFoundError: No module named 'flag_gems'`，无法完成 PlatformFL identity gate。
- Gate D：`NOT RUN`。
- Task container：`qw36-a3-s2-gateb-net-followup-20260825T234607p0800` / `82a815b4b7576230f3f786b95583f9d1b8400a1622b5a82752c62fcd444444f6`已删除；NPU 0/1释放。
- Code PR：`N/A`；implementation source未修改。
- Evidence root：`/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T234607p0800`；main build log `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T234607p0800/logs/gate_b_corrected_proxy_build_wheel.log`。

该 follow-up run当时 Stage 1/2仍未 PASS；后续 Gate C/D follow-up已闭合。

## Current Gate C/D follow-up run — QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG

Result：[`RESULT-QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG-GATEC-FOLLOWUP-20260826T092617+0800.md`](results/RESULT-QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG-GATEC-FOLLOWUP-20260826T092617+0800.md)

本次 run 事实：

- Execution：`Execution PASS - Stage 1/2`；该immutable Result发布时Codex1 Acceptance为`PENDING`，后续Formal Acceptance已完成。
- Gate B：复用 exact prior A3 wheel，不重建；wheel sha256 `fa33f586b2e56e78f671989e6dc3dc2ee23f5005c5f0cc4800a6cf6e4b2e98c1`。
- Gate C：`PASS`；在同一 official A3 openEuler image中使用 privileged host Ascend runtime mapping后，`torch.npu.is_available()==True`、`device_count==2`，`DeviceInfo()`走 Ascend fast-path；`import vllm_fl.platform`成功，`vllm-ascend` absent，`vllm_ascend`不可import，`vllm_fl`来自 site-packages wheel，`USE_FLAGGEMS=0`且未安装FlagGems。
- Gate C root cause disposition：此前 `ModuleNotFoundError: No module named 'flag_gems'`为容器runtime mapping不完整导致 `torch.npu`不可用后的fallback表象；不是已确认source bug，也不需要安装FlagGems来满足PR #404 standalone contract。
- Gate D：`PASS`；`torch.ops._C_ascend.npu_add_rms_norm_bias` 在 A3 NPU上执行，输入/输出均为 `npu:0`，BF16形状 `(2,1024)`，`torch.npu.synchronize()`成功，CPU reference最大误差 `y=0.019088029861450195`、`rstd=0.00013947486877441406`、`x=0.001953125`，finite/correct且无silent CPU fallback。
- Preserved container：`qw36-a3-s2-gatec-priv-20260826T092617p0800` / `32562c7139600c25e570ec07841713737b0407c7d1bbdc563be41b87ea105f0a`，保留standalone FL环境和必要artifacts。
- Evidence root：`/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-C-FLAGGEMS-DIAG/evidence/20260826T092617p0800`。
- Main reused build log：`/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/evidence/20260825T234607p0800/logs/gate_b_corrected_proxy_build_wheel.log`。
- Gate D smoke log：`/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-C-FLAGGEMS-DIAG/evidence/20260826T092617p0800/runtime/gate_d_custom_op_smoke.log`。

Stage 1/2 Formal Acceptance：**ACCEPTED**。Acceptance绑定 exact source `7beda84...`和上述 wheel/container/Evidence；Stage 3随后已dispatch并完成Formal Acceptance。

## 当前已确认的高影响事实

- vLLM baseline是 `0.20.2`；vLLM-Ascend `0.20.2rc1`只作 matched implementation/oracle reference。
- Official image matrix把 ordinary `v0.20.2rc1`映射为 A2 Ubuntu；正式 A3候选只有 `v0.20.2rc1-a3`与`v0.20.2rc1-a3-openeuler`。两条 build definition均使用 CANN 9.0.0 A3/Python 3.11、vLLM 0.20.2、torch/torch_npu 2.10.0和 Triton Ascend 3.2.1。
- Base image是 carrier/builder，不改变 final ownership；Stage 2仍要求 absent `vllm-ascend` distribution/module/runtime dependency和正式 FL wheel/site-packages origin。
- 当前 head静态存在 `PlatformFL / WorkerFL / ModelRunnerFL`、FL-local Qwen/GDN/Mamba/Attention/MoE、`FULL_DECODE_ONLY` 和 Ascend build/package路径；静态存在不证明 A3执行。
- 当前 exact source列出 **8 个 OPP、9 个 `_C_ascend` schemas**；PR正文旧的 7/8计数已过时，Stage 2以 exact-head source为准并保存 wheel inventory。
- A3 build/package在 `SOC_VERSION`未设置时默认 `ascend910_93`；runtime custom-op选择未设置时却静态默认 `ascend910b1`。这只是 Stage 1/2风险，必须保存实际 effective `SOC_VERSION`、selected prebuilt root、extension/OPP origin；未在 A3复现前不是 blocker。
- A3 binary必须在 A3/compatible CANN环境重新构建；A2 wheel不可复用。
- 正式 runtime必须 `VLLM_PLUGINS=fl`、`USE_FLAGGEMS=0`，不得依赖 installed `vllm-ascend`，不得从源码树或 `PYTHONPATH`加载 `vllm_fl`。
- User确认模型为 `Qwen/Qwen3.6-35B-A3B` BF16 non-quantized，路径 `/data/tiankuan/zyg/FL/workspace/Qwen3.6-35B-A3B`，当前仍在下载。Stage 1/2只允许只读 presence/download-state inventory；不等待、不加载、不以完整权重作为 PASS条件。

## 不允许的状态外推

- A2 PASS不等于 A3 PASS。
- wheel build/文件存在不等于 NPU kernel正确执行。
- schema注册不等于 model path执行。
- capture成功不等于 replay/state正确。
- Execution PASS不等于 formal Acceptance。
- 当前 head的 Acceptance不自动覆盖 future branch head。
