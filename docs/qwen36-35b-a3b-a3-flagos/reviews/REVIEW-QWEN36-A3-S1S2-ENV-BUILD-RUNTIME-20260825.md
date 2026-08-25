# Codex1 Formal Review — QWEN36-A3-S1S2-ENV-BUILD-RUNTIME

Review date：2026-08-25

Review state：**NEEDS-FOLLOWUP**

## Unified identity

| Field | Reviewed identity |
| --- | --- |
| Control repo | `https://github.com/yanceng305-collab/qwen36-35b-a3b-a3-flagos-control` |
| Result sync commit | `beee47c91778b2b55b1ae7ae33a91aa6898ee797` |
| Implementation repo | `https://github.com/xiemingda-1002/vllm-plugin-FL` |
| Tracked branch | `feature/qwen3.6-35b-a3b-ascend-graph-migration` |
| Execution HEAD/tree | `7beda84f59d7b25f49cdf03bdf6efecd771067ed` / `a81eea55c1de548a0a1f182f51089eca0b088c82` |
| Official PR/base | `flagos-ai/vllm-plugin-FL#404` / `release/0.2@53adefb269571684d83a51e997d3ba9be5f88235` |
| Official base tree | `9ddfd080953ad39b39772e108ff921d2973b0299` |
| Immutable Result | [`RESULT-QWEN36-A3-S1S2-ENV-BUILD-RUNTIME-20260825T205424+0800.md`](../results/RESULT-QWEN36-A3-S1S2-ENV-BUILD-RUNTIME-20260825T205424+0800.md) |
| Server Evidence pointer | `/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S1S2-ENV-BUILD-RUNTIME/evidence` |

本 Review只审查已经结束的 exact execution。Tracked branch后续变化不得改变本 Result的 source identity或把未来结果回填到本次 run。

## Review evidence and limitation

Codex1本轮未访问 A3 server，未直接打开 server-only raw log。Review依据：immutable Result及其 Evidence pointers、Control task/contract、GitHub current PR/ref API、exact source `7beda84...`静态审查，以及 official matching-version vLLM-Ascend/Triton-Ascend source/docs。

因此本 Review可以接受 Result中有明确身份、exit code和因果顺序的 STOP事实，但不会把未进入 Control的 raw generated-directory状态或未保存的 provider metadata补猜为事实。

## Gate A Review — ACCEPT WITH EVIDENCE GAP

### Accepted scope

以下 Gate A facts在 immutable Result中身份明确，且未发现静态反证：

- actual A3/910C target、logical mapping、safe NPU 0/1 scope和其他任务 owner边界；
- official A3 openEuler image tag、manifest/platform digest、local image ID与选择理由；
- container Python 3.11.15、CANN 9.0.0、torch/torch_npu 2.10.0/2.10.0、vLLM 0.20.2+empty、Transformers 5.5.3；
- tracked source exact HEAD/tree和 clean formal build source；
- physical runtime device `Ascend910_9382`、effective build `SOC_VERSION=ascend910_93`及 exact CATLASS commit；
- model只做 inventory，未被用作 Gate A/B PASS依据。

### Triton/provider Evidence gap

Result只记录 `Triton = 3.5.0`，没有显式记录：

- `triton-ascend` distribution version；
- community `triton` distribution version与二者同时存在的 package identity；
- imported `triton` module file/origin；
- active Ascend backend/provider identity。

这不是已确认的 wrong tuple。Official Triton-Ascend installation guide at `triton-lang/triton-ascend@a4aecc3292d8bdf7ae80165f113a27ea1e89803a`说明：`triton-ascend 3.2.1`增加对 community Triton的依赖，并明确示例冲突文本为 `triton-ascend 3.2.1 requires triton==3.5.0`；ARM使用 community Triton 3.5系列。见 [`installation_guide.md`](https://github.com/triton-lang/triton-ascend/blob/a4aecc3292d8bdf7ae80165f113a27ea1e89803a/docs/en/installation_guide.md#L300-L335)。

因此 `triton=3.5.0`与正确的 `triton-ascend=3.2.1`组合是相容候选，但当前 Result没有证明后半部分或实际 provider。Gate A核心环境事实接受；Triton/provider子项保持 **EVIDENCE GAP / NOT ACCEPTED YET**，由 bounded follow-up补证，不重跑整个 Gate A。

## Gate B Review — STOP ACCEPTED

### One-sentence conclusion

Formal build进入 `ASCEND_COMPUTE_UNIT=ascend910_93`的 CANN OPP prepare/build后，在 metadata input查找阶段因没有发现任何 `aic-*-ops-info.ini`而抛出 `OpFileNotExistsError`，exit code 1且没有 wheel；按 Task合同在 Gate B停止是正确行为。

### Precise failure location

- Immutable Result：Gate B / main build log pointer，first blocker `OpFileNotExistsError: File aic-*-ops-info.ini does not exist`。
- Exact source：`csrc/ascend/cmake/scripts/util/ascendc_impl_build.py`中 `get_ops_info_files()` glob `aic-*-ops-info.ini`；结果为空时抛出 `OpFileNotExistsError`。
- Consequence：no wheel；Gate C/D没有合法输入，因此 `NOT RUN`正确；没有 model/graph/serve/performance执行。

### Root-cause boundary

**Confirmed immediate failure mechanism：HIGH confidence。** Metadata stage期望的 `aic-*-ops-info.ini`没有出现在被检查目录，exception未恢复并使 wheel build exit 1。

**Underlying root cause：LOW confidence / NOT CONFIRMED。** 当前 Evidence不能区分：

- 哪个具体 op未进入或未完成 metadata generation；
- CMake/opbuild/prepare/build infrastructure问题；
- CANN opbuild行为；
- compute-unit与 generated path/name转换问题；
- ops-info生成成功但传入/查找目录错误；
- 其他 A3-specific build原因。

Exact FL source已明确把 `ascend910_93`映射到 A3 family/prebuilt root；`build.sh`和 CMake支持该 compute unit；8个当前 OPP的 OpDef均存在 A3 config，包含 `apply_top_k_top_p_custom::AddConfig("ascend910_93")`。Matching-version official vLLM-Ascend `v0.20.2rc1@367b8e62...`也包含相同 `ascendc_impl_build.py`查找逻辑和多个 `ascend910_93` OpDef。静态事实不证明 FL build一定正确，但足以拒绝“同事未适配 A3 OPP”或“必须修改 source”的提前结论。

## Six issue disposition

| Issue | Disposition | Evidence / Control handling |
| --- | --- | --- |
| 1. Gate B STOP成立 | **CONFIRMED** | first blocker、exit 1、no wheel、C/D未运行符合 Task STOP合同；接受 STOP事实，不等于 Gate B PASS |
| 2. 不得把 source defect写成 confirmed root cause | **CONFIRMED** | Result保持 hypothesis边界；Review明确 underlying root cause LOW/unknown，不授权 source change |
| 3. Gate A Triton/provider identity gap | **CONFIRMED** | 3.5.0可能是 ARM community Triton依赖；缺 `triton-ascend` distribution/module/provider证据；Gate A ACCEPT WITH EVIDENCE GAP |
| 4. Source compare base错误 | **CONFIRMED** | Result的 `main@38e7dbc...`不是 official PR base；正确 base是 `flagos-ai:release/0.2@53adefb...`。Execution HEAD/tree仍正确，故不推翻 STOP |
| 5. 缺明确 root-cause confidence | **CONFIRMED** | immutable Result没有 `Root-cause confidence`字段；不修改 Result，在本 Review/INDEX/STATUS记录：immediate failure HIGH，underlying cause LOW |
| 6. STATUS fork逻辑措辞 | **CONFIRMED** | 已有 A3 execution blocker，但尚未归因到 implementation source；继续 `Code repo/fork = Not needed yet`，理由改为 source attribution未确认 |

## Contract gaps

1. Gate A缺 Triton-Ascend distribution/module/provider identity。
2. Result compare-base字段引用 fork `main`而非 official PR base。
3. Result缺显式 root-cause confidence字段。

Immutable Result保持不变。上述缺口只通过 Review、STATUS、INDEX和 follow-up Evidence补充。

## Formal claim boundary

### Accepted

- Gate A core A3 device/image/environment/source identity，**不含 Triton/provider子项**；
- Gate B发生并按合同 STOP；
- first blocker字符串、build exit 1、no wheel；
- Gate C/D NOT RUN；
- no model/TP2/graph/serve/benchmark；
- Code PR=`N/A`且没有 source修改。

### Not accepted / not established

- Stage 1/2 Execution PASS或 formal Acceptance；
- Gate B PASS、A3-native wheel、standalone FL install或 custom-op smoke；
- underlying root cause、任何具体 OPP defect或 implementation-source attribution；
- source fix必要性；
- Stage 3 Ready或任何 model correctness。

## Formal conclusion and next gate

Formal Review：**NEEDS-FOLLOWUP**。

Stage 3：**LOCKED**。

Next Task：`QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG`。它只补 Gate A Triton/provider identity并定位/闭合 Gate B metadata first blocker；不得修改 source或进入 Gate C/D。只有 confirmed root cause证明 source change不可避免时，才返回 bounded Code Decision request。
