# Current Implementation State — GitHub and Static Source Verification

核验时间：2026-08-25 17:02 CST / 09:02 UTC。

本文件记录 Control创建时的 live snapshot。它不是 A3 execution Evidence；dispatch前必须重新查询。

## GitHub state

| Field | Verified value |
| --- | --- |
| PR | <https://github.com/flagos-ai/vllm-plugin-FL/pull/404> |
| State | OPEN |
| Draft | true |
| Mergeability | MERGEABLE / no reported git conflict |
| Merge state / review | BLOCKED / REVIEW_REQUIRED |
| Head repo/ref | `xiemingda-1002/vllm-plugin-FL:feature/qwen3.6-35b-a3b-ascend-graph-migration` |
| Head SHA/tree | `7beda84f59d7b25f49cdf03bdf6efecd771067ed` / `a81eea55c1de548a0a1f182f51089eca0b088c82` |
| Base repo/ref | `flagos-ai/vllm-plugin-FL:release/0.2` |
| Base/release SHA/tree | `53adefb269571684d83a51e997d3ba9be5f88235` / `9ddfd080953ad39b39772e108ff921d2973b0299` |
| Compare | ahead 6 / behind 0；base也是 merge base |

Current 6-commit series：

1. `09851fe...` migrate Qwen3.6 eager and graph runtime；
2. `fa43a8b...` expert-parallel gates；
3. `ae75bad...` aligned Mamba prefix caching；
4. `1fd84a5...` HCCL AIV default；
5. `fb4800c...` CPU binding and shared-expert overlap；
6. `7beda84...` graph serving alignment。

PR timeline显示 branch从 `f9281f78...` force-push/rebase到 `7beda84...`，不是简单 fast-forward。当前 mergeable只说明 Git冲突状态；PR仍 Draft/Blocked/Review-required，且观察到的 labeler check不等于 adaptation CI PASS。

## Static source facts at `7beda84...`

- vLLM platform/general plugin entry points均指向 `vllm_fl`；test extra固定 `vllm[audio]==0.20.2`。
- PR/source comments把 vLLM-Ascend `0.20.2rc1`作为 matched reference；packaging和 executable `vllm_fl`中未发现 direct runtime import requirement。
- FL-local `PlatformFL`、`WorkerFL`、`ModelRunnerFL`及 Qwen/Mamba/GDN/Attention/MoE patch/runtime anchors存在。
- NPU registration设置 `USE_FLAGGEMS=0`；Ascend dispatch配置优先 FL-local vendor实现。
- `PlatformFL`在 NPU只接受 `NONE`或 `FULL_DECODE_ONLY`，其他 graph mode回退 `NONE`。
- `setup.py`与 OPP build在 unset `SOC_VERSION`时默认 A3 `ascend910_93`，CMake支持该 family，wheel package data包含 family-specific extension/OPP。
- current source list是 **8 OPP / 9 `_C_ascend` schemas**。PR正文的 7/8是 stale prose，Stage 2以 run source盘点为准。

## Static A3 risk requiring field verification

`setup.py` / `build_opp.sh`未设置 `SOC_VERSION`时默认 `ascend910_93`；`vllm_fl/ascend_custom_ops.py` runtime loader未设置时却默认 `ascend910b1`。当前 source中未找到 loader前统一赋值。

这可能由实际 A3 container/CANN显式环境变量解决，也可能造成 loader选择错误。当前只能登记为风险；只有 A3 execution记录 effective variable、selected prebuilt root、extension/OPP origin并复现失败后，才形成 blocker/Code Decision。

## Evidence links

- Head：<https://github.com/xiemingda-1002/vllm-plugin-FL/commit/7beda84f59d7b25f49cdf03bdf6efecd771067ed>
- Base：<https://github.com/flagos-ai/vllm-plugin-FL/commit/53adefb269571684d83a51e997d3ba9be5f88235>
- Compare：<https://github.com/flagos-ai/vllm-plugin-FL/compare/release/0.2...xiemingda-1002:feature/qwen3.6-35b-a3b-ascend-graph-migration>

Static anchors at exact head：

- `pyproject.toml` project/entry points/vLLM test pin：lines 28–71；
- `vllm_fl/__init__.py` NPU registration / `USE_FLAGGEMS=0`：lines 133–172；
- `platform.py` graph mode：lines 346–397；
- `setup.py` A3 family：lines 70–78, 127–130, 240–282；
- `csrc/ascend/build_opp.sh` A3 family与 8 OPP：lines 8–49；
- `torch_binding.cpp` 9 schemas：lines 479–526；
- `ascend_custom_ops.py` runtime family selection/loading：lines 71–132。
