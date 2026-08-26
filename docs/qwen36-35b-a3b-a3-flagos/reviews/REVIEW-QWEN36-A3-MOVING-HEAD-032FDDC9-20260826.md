# Codex1 Bounded Moving-Head Review — `e610a990` to `032fddc9`

Review date：2026-08-26

Disposition：**DOCS/TESTS-ONLY — NO BUILD OR RUNTIME REGRESSION REQUIRED**

## Live GitHub identity

| Field | Live value |
| --- | --- |
| Tracked branch HEAD | `032fddc91b6d013b98aed8e64ff05b54d1435648` |
| Tree | `463806ef18e5e31006cd4f59e6a5261fc65cea4a` |
| Parent | `e610a990d785356bf51a3cad50219d4c03310a31` |
| Commit count from Accepted SHA | 1 |
| Subject | `build: document and test Ascend targets` |
| Official PR #404 | open / draft / mergeable；`mergeable_state=unstable` |
| Official base | `release/0.2@53adefb269571684d83a51e997d3ba9be5f88235` / tree `9ddfd080953ad39b39772e108ff921d2973b0299` |

GitHub tracked branch ref、official PR head和 fetched commit三者一致。

## Exact diff

`e610a990d785356bf51a3cad50219d4c03310a31..032fddc91b6d013b98aed8e64ff05b54d1435648`：

| Path | Change | Classification |
| --- | --- | --- |
| `README.md` | +11 / -0 | Ascend build usage documentation only；记录`VLLM_VENDOR=ascend`、A3 default `ascend910_93`、A2 `SOC_VERSION=ascend910b` |
| `tests/unit_tests/test_build_config.py` | new file，+66 / -0 | Characterization unit tests for existing `setup.py` / `build_opp.sh` behavior |

无其他path、rename、mode、binary或submodule变化。`setup.py`、`csrc/ascend/build_opp.sh`、`pyproject.toml`、CMake和整个`vllm_fl/`树在该range内对象级未变。因此PlatformFL、WorkerFL、ModelRunnerFL、Qwen/GDN/Mamba/attention/MoE、graph、communicator、C/CANN operators、packaging/build implementation与production runtime均无semantic delta。

`pyproject.toml` already references `README.md` as package metadata，因此若重新build `032fddc9...`，wheel的`dist-info/METADATA`文本和整体hash可能改变。本Review不声称`032fddc9...`可产生与Accepted wheel字节相同的artifact；结论是该差异不改运行/build-selection语义，Stage 4也无需为此重build。

## Test review

新测试仅对已有逻辑做可观测性固化：

- A3未设`SOC_VERSION`时prebuilt dir为`ascend910_93`；
- A2 `SOC_VERSION=ascend910b`时prebuilt dir为`ascend910b1`；
- `build_opp.sh`默认SoC为`ascend910_93`。

Host上使用`--confcutdir=tests/unit_tests`隔离与本测试无关的device conftest后，`3/3` tests PASS。本Review没有操作A3；该host unit test不作为A3 runtime Evidence，只用于确认新commit的test-only intent与已有build selection一致。

## Formal disposition

1. Stage 3 Formal Acceptance仍严格绑定`e610a990...` / tree `609ff1ad...`及wheel SHA-256 `2fcf788...`；历史Acceptance不改写。
2. `032fddc9...`不改production/runtime/build implementation语义，不要求wheel rebuild、custom-op regression、TP2 eager或完整Stage 3重跑。README-derived package metadata的潜在字节差异不是runtime regression要求。
3. Stage 4实际runtime artifact继续为`e610a990...` Accepted wheel；dispatch/result同时记录current tracked `032fddc9...`及本bounded disposition，不将`032fddc9...`伪写为wheel build SHA。
4. Dispatch时若tracked HEAD/tree仍精确为`032fddc9...` / `463806ef...`，Codex2可直接进入Stage 4 Gate G0；若再次移动，在graph/model mutation前STOP并交回新diff。
5. Stage 4 scope仍只是`FULL_DECODE_ONLY` graph correctness；不解锁serve、performance、prefix、EP2、long context或functional matrix。

Final verdict：**Stage 4 remains READY; no build/runtime regression prerequisite.**
