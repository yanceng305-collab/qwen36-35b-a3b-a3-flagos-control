# Codex2 Prompt — QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG

状态：**DO NOT DISPATCH — historical Stage 1/2 follow-up prompt。Result chain已ACCEPTED，不得续跑。**

下一执行入口：`QWEN36-A3-S3-TP2-BF16-EAGER`。

```text
Control repo:
https://github.com/yanceng305-collab/qwen36-35b-a3b-a3-flagos-control

Implementation repo:
https://github.com/xiemingda-1002/vllm-plugin-FL

Tracked branch:
feature/qwen3.6-35b-a3b-ascend-graph-migration

Task ID:
QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG
```

同步 latest Control并读取 `AGENTS.md`、`STATUS.md`、本 Task、parent immutable Result、Codex1 Review、`BASELINE.md`和 Evidence规则。不要把旧聊天或服务器目录当 Control source of truth。

## Exact diagnostic identity

```text
Parent execution HEAD:
7beda84f59d7b25f49cdf03bdf6efecd771067ed

Parent execution tree:
a81eea55c1de548a0a1f182f51089eca0b088c82

Official PR base:
flagos-ai/vllm-plugin-FL:release/0.2

Official base SHA/tree:
53adefb269571684d83a51e997d3ba9be5f88235
9ddfd080953ad39b39772e108ff921d2973b0299

Parent Result commit:
beee47c91778b2b55b1ae7ae33a91aa6898ee797

Target host:
bm-jn-zs-zone1-910C-64G-10-108

Official A3 image:
quay.io/ascend/vllm-ascend:v0.20.2rc1-a3-openeuler

Manifest digest:
sha256:442363921166771eb82baeec9c1ac0381f46fb830ead8d0e072df6e925f2a958

Arm64 platform digest:
sha256:a55b2b0597f9fdd1882de9bf3b7ebc395dd77c1ca49f251d0cb759d7b2c1a807
```

诊断绑定上述 exact source。开始时记录 current tracked HEAD，但不得用 future moving HEAD改写 parent run或替换本次 reproduction source。

## Objective

最小定位并尽可能闭合 Gate B first blocker：

```text
OpFileNotExistsError: File aic-*-ops-info.ini does not exist
```

回答：为什么 `ASCEND_COMPUTE_UNIT=ascend910_93`的 OPP build没有在 `ascendc_impl_build.py`收到的目录中生成/找到预期 `aic-*-ops-info.ini`。

不要假设 source bug。区分并取证：具体 op definition、CMake/opbuild、CANN behavior、compute-unit/path naming、selected-op generation flow、wrong lookup directory或其他原因。

## Keep the path simple

- 使用 parent相同 official A3 openEuler image route和 exact CATLASS `41bf90da655bba3c66d0acd7e00abe33960ecfd6`。
- 若 target不可用、owner边界变化或 image digest不匹配，在 mutation前 STOP请求 User决定。
- 一个 Task专属 container、一个 clean source/build workspace；不拆 builder/runtime container。
- 先 static/read-only review，再做一次最小 Gate B reproduction。
- 不修改 source，不创建 fork/Code repo/branch/PR。只有 confirmed non-source correction闭合 Gate B后，才在同一 container继续 Gate C/D；任何情况下都不进入 Stage 3。

## Gate A evidence supplement

在 build前用新 Python process保存：

- `triton-ascend` distribution version；
- community `triton` distribution version；
- distribution file ownership/direct_url/editable信息；
- `triton.__version__`和`triton.__file__`；
- Ascend backend/provider identity和相关 module/library origin。

`triton=3.5.0`可能是 ARM上 `triton-ascend=3.2.1`的正常依赖，但必须由 actual evidence证明。

## Minimal Gate B reproduction

保存：

- exact command/config/numeric exit/full traceback；
- `SOC_VERSION`、`ASCEND_COMPUTE_UNIT`、CMake cache和 opbuild/prepare arguments；
- `--opsinfo-dir`实际目录参数；
- failure前后 generated directory tree、全部 relevant file names和 checksums；
- expected与 actual `aic-*-ops-info.ini` path/name；
- selected 8-op/current exact source set及每个 op进入 metadata flow的状态；
- matching-version official source对照；
- source HEAD/tree/status/diff。

如果 confirmed root cause是现有 route内非源码、可逆的 build procedure/config问题，可做一次 corrected Gate B rerun。只有 corrected wheel满足原 Gate B全部要求，才继续下面的 Gate C/D。

## Conditional execution flow

A. 如果 confirmed root cause需要 source修改：

- 立即 STOP / Decision requested；
- 返回 confirmed root cause、confidence、最小 affected files/symbols、bounded fix和 regression scope；
- 不得 patch source。

B. 如果是 non-source procedure/config原因，但 corrected Gate B仍失败：

- STOP并保存 Evidence；
- 不进入 Gate C/D。

C. 如果 non-source correction闭合 Gate B：

先证明：

- exact clean source不变；
- `ascend910_93` A3-native wheel产生；
- wheel path/hash/ABI/inventory完整；
- exact-source OPP/schema reconciliation完整；
- `_C_ascend`/OPP属于 A3；
- 无 A2 residue；
- 不依赖 whole external vLLM-Ascend runtime tree。

然后在同一个 Task container直接继续 Gate C/D，不创建新 Task、不重建环境。

### Gate C — Standalone FL install

离开 FL source tree：

1. uninstall `vllm-ascend`和 old `vllm-plugin-fl`；
2. install本次 corrected wheel；
3. new Python process验证。

必须证明：

- `vllm_fl`来自正式 wheel/site-packages；
- 无 FL source `PYTHONPATH`/editable/source shortcut；
- `vllm-ascend` distribution absent；
- `vllm_ascend` module/entrypoint不可 import，runtime无其 dependency/call；
- `VLLM_PLUGINS=fl`；
- `USE_FLAGGEMS=0`；
- PlatformFL/dispatch/provider identity正确；
- `_C_ascend`、`prebuilt/ascend910_93`和 OPP来自本次 wheel。

### Gate D — Minimal real A3 NPU custom-op smoke

执行至少一个与本次 wheel匹配的真实 A3 NPU custom-op，保存：

- input/output A3 device；
- dtype/shape；
- reference/assertion；
- synchronize；
- numeric exit code；
- operator/library/OPP origin；
- finite/correct；
- no CPU fallback。

Import/schema可见不能替代 Gate D。

D. 如果 Gate C或D出现新 first blocker：

- 在对应 gate STOP；
- 保存 last successful gate、first blocker、raw Evidence、confidence和最小复现；
- 不绕过、不进入 Stage 3。

E. 如果 Gate A supplement + B + C + D全部通过：

允许报告：

```text
Execution PASS — Stage 1/2
```

保留最终 Task container、standalone FL环境、wheel和必要 artifacts，记录 container name/ID与 exact paths，等待 Codex1 formal Acceptance。不得运行模型或进入 Stage 3。

## PASS / STOP

`DIAGNOSTIC PASS`要求：first blocker/call path/generated metadata差异已闭合；root cause可区分类别；显式填写 `Root-cause confidence: LOW / MEDIUM / HIGH`；Triton/provider gap已补；source clean。只有 Branch E满足时才能额外报告 `Execution PASS — Stage 1/2`。

如果必须修改 source或改变 CANN/vLLM/image/nightly/major route，立即 STOP，返回 confirmed root cause、最小 affected files/symbols、bounded fix proposal、regression scope和 `Decision requested`。不要直接修改。

禁止完整模型、TP2、Stage 3、graph、serve、prefix、EP、64K、benchmark、profiling、GLM、source patch、Code repo/fork和未来问题重构。Gate C/D只允许在 Branch C的 Gate B PASS后执行。

生成 immutable Result并同步 INDEX，返回 Code/source、Control、Evidence三指针；Code PR=`N/A`。如果执行 C/D，必须返回 standalone install/provider/custom-op Evidence；如果 Stage 1/2 PASS，必须返回并保留 final container/FL environment/wheel/artifacts。不得自行进入 Stage 3。
