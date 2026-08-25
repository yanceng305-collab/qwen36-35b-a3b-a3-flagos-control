# QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG

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
QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG
```

Diagnostic source identity：

```text
HEAD: 7beda84f59d7b25f49cdf03bdf6efecd771067ed
tree: a81eea55c1de548a0a1f182f51089eca0b088c82
Official PR base: flagos-ai/vllm-plugin-FL:release/0.2
Official base SHA: 53adefb269571684d83a51e997d3ba9be5f88235
Official base tree: 9ddfd080953ad39b39772e108ff921d2973b0299
```

Parent Result：[`RESULT-QWEN36-A3-S1S2-ENV-BUILD-RUNTIME-20260825T205424+0800.md`](../results/RESULT-QWEN36-A3-S1S2-ENV-BUILD-RUNTIME-20260825T205424+0800.md)

Formal Review：[`REVIEW-QWEN36-A3-S1S2-ENV-BUILD-RUNTIME-20260825.md`](../reviews/REVIEW-QWEN36-A3-S1S2-ENV-BUILD-RUNTIME-20260825.md)

## Objective

使用最短执行路径定位并尽可能闭合 Gate B first blocker：

```text
OpFileNotExistsError: File aic-*-ops-info.ini does not exist
```

必须回答：为什么 `ASCEND_COMPUTE_UNIT=ascend910_93`的 OPP build没有在传给 `ascendc_impl_build.py`的目录中产生/找到预期 `aic-*-ops-info.ini`，并将 underlying root cause从 LOW confidence提升为有 Evidence支持的结论。

同时补齐 parent Gate A唯一指定缺口：`triton-ascend` distribution、community `triton` distribution、imported module origin和 active Ascend provider identity。

## Fixed boundaries

- 复现绑定 parent exact source HEAD/tree，不因 moving branch后续变化替换诊断 source。开始时仍记录 current tracked HEAD用于项目事实，但不得把它套到 parent Result。
- Official base固定记录为 `flagos-ai/vllm-plugin-FL:release/0.2@53adefb...`；不得使用 fork `main`作为 PR compare base。
- 目标 host为 parent run的 `bm-jn-zs-zone1-910C-64G-10-108`；若该 target不可用或 owner边界变化，在 mutation前 STOP请求 User决定。
- 使用 parent run相同 official A3 openEuler image：`quay.io/ascend/vllm-ascend:v0.20.2rc1-a3-openeuler`；manifest digest `sha256:442363921166771eb82baeec9c1ac0381f46fb830ead8d0e072df6e925f2a958`，arm64 platform digest `sha256:a55b2b0597f9fdd1882de9bf3b7ebc395dd77c1ca49f251d0cb759d7b2c1a807`。现场重新记录 resolved digest和 local image ID。
- 使用 exact CATLASS `41bf90da655bba3c66d0acd7e00abe33960ecfd6`。
- 使用一个 Task专属 container和 clean source/build workspace；不要增加 builder/runtime分层。
- 不修改 implementation source，不创建 fork/Code repo/branch/PR，不进入 Gate C/D或 Stage 3。

## Allowed work

1. Read-only/static review exact FL source、matching-version official vLLM-Ascend/CANN build infrastructure和公开官方资料。
2. 在 A3上对 Gate B做最小复现；只占用最小安全 scope，不干扰其他任务。
3. 在 failure前后保存 generated directories、file names、CMake cache/variables、opbuild命令/arguments、selected op set和完整 traceback。
4. 确认 `ascendc_impl_build.py --opsinfo-dir`实际收到哪些目录，以及这些目录中实际生成了什么。
5. 检查 metadata generation是否未运行、失败、生成到其他路径/命名、被错误筛选，或只对某个 op/SoC组合缺失。
6. 如果 confirmed root cause是现有 route内的非源码操作/配置问题，可做一次最小、可审计、可逆的 corrected Gate B rerun；不得改变 vLLM/CANN/image/source baseline。

## Required preflight supplement — Triton/provider

在任何 build复现前，以新 Python process记录：

- `importlib.metadata.version("triton-ascend")`；
- `importlib.metadata.version("triton")`；
- distributions/file ownership与任何 editable/direct_url信息；
- `triton.__version__`、`triton.__file__`；
- `triton.backends` / active Ascend backend/provider可识别身份；
- relevant module/library origin。

Expected relation只是 oracle：ARM上的 `triton-ascend 3.2.1`可以依赖 community `triton 3.5.0`。必须记录 actual identity，不得根据版本号自动 PASS。

## Diagnostic PASS

只有以下全部成立才可报告 `DIAGNOSTIC PASS`：

1. first blocker在 exact source/image/SoC contract上最小可复现，或有充分 Evidence解释为何无法复现；
2. 完整 call/failure path和 `opsinfo-dir` inputs已保存；
3. expected与 actual generated metadata/file/path差异已保存；
4. root cause能够区分 op definition、CMake/opbuild、CANN behavior、SoC/path naming、selected-op flow或其他类别；
5. `Root-cause confidence: LOW / MEDIUM / HIGH`显式填写并说明依据；
6. Triton/provider identity gap已闭合；
7. source保持 exact clean，无 undocumented patch。

如果非源码修正使原 Gate B成功并产生 wheel，还必须保存 wheel path/hash/ABI、dispatch-head OPP/schema inventory和 A2-residue audit；此时可以另写 `Gate B PASS`。不得继续 Gate C/D。

## STOP / Decision requested

在以下边界 STOP：

- 需要修改 implementation source才能继续；
- 需要换 CANN、vLLM、image、nightly或其他 major route；
- exact parent source不能重建/复现且 Evidence不足；
- target/device owner不明确或需要干扰其他任务；
- 诊断开始扩大到 full model、Gate C/D、graph、serve、benchmark或性能。

如果 Evidence确认 source change必要，返回：confirmed root cause、最小 affected files/symbols、bounded fix scope、required regression和 `Decision requested`；不要在本 Task内修改。

## Required Evidence and return

- Task/run/timestamps/explicit dispatch；
- Control/implementation/tracked branch/Task ID完整 header；
- parent exact source HEAD/tree/clean state和 official PR base；
- selected image/container/device/root identity；
- Triton distributions/module/backend/provider supplement；
- exact reproduction command/config/exit code/full traceback；
- CMake/opbuild/prepare arguments与 environment；
- generated directory trees/file lists/checksums；
- expected vs actual `aic-*-ops-info.ini` path/name；
- per-op/SoC metadata observations；
- confirmed facts、ranked hypotheses、root-cause confidence；
- source status/diff证明无修改；
- if produced：wheel hash/inventory/A2-residue audit；
- last successful step、first blocker、next Decision request；
- Code/source、Control、Evidence三指针；Code PR=`N/A`。

Result不得把 hypothesis写成 confirmed root cause，也不得自行进入后续 Stage。
