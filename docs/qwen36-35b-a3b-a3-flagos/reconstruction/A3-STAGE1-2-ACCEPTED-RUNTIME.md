# A3 Stage 1/2 Accepted Runtime Reconstruction

Acceptance date：2026-08-26

Status：**ACCEPTED — scope-limited Stage 1/2 foundation**

Formal Review：[`REVIEW-QWEN36-A3-STAGE1-2-ACCEPTANCE-20260826.md`](../reviews/REVIEW-QWEN36-A3-STAGE1-2-ACCEPTANCE-20260826.md)

## Accepted identities

| Field | Accepted value |
| --- | --- |
| Source HEAD/tree | `7beda84f59d7b25f49cdf03bdf6efecd771067ed` / `a81eea55c1de548a0a1f182f51089eca0b088c82` |
| Official base | `release/0.2@53adefb269571684d83a51e997d3ba9be5f88235` / tree `9ddfd080953ad39b39772e108ff921d2973b0299` |
| Image | `quay.io/ascend/vllm-ascend:v0.20.2rc1-a3-openeuler` |
| Manifest/platform digest | `sha256:442363921166771eb82baeec9c1ac0381f46fb830ead8d0e072df6e925f2a958` / `sha256:a55b2b0597f9fdd1882de9bf3b7ebc395dd77c1ca49f251d0cb759d7b2c1a807` |
| Python/CANN/torch/torch_npu | `3.11.15` / `9.0.0` / `2.10.0+cpu` / `2.10.0` |
| vLLM / Triton-Ascend / Triton | `0.20.2+empty` / `3.2.1` / `3.5.0` |
| Wheel | `vllm_plugin_fl-0.2.0+g7beda84f5-cp311-cp311-linux_aarch64.whl` |
| Wheel SHA-256 | `fa33f586b2e56e78f671989e6dc3dc2ee23f5005c5f0cc4800a6cf6e4b2e98c1` |
| Family | `ascend910_93`；strict A2 residue search=0 |

## Exact accepted execution instance — historical record only

- Host：`bm-jn-zs-zone1-910C-64G-10-108`。
- Preserved container：`qw36-a3-s2-gatec-priv-20260826T092617p0800` / `32562c7139600c25e570ec07841713737b0407c7d1bbdc563be41b87ea105f0a`。
- Historical mapped devices：`/dev/davinci0`、`/dev/davinci1`；actual NPU names `Ascend910_9382`。
- Wheel：`/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-B-OPP-METADATA-DIAG/artifacts/20260825T234607p0800/wheels/vllm_plugin_fl-0.2.0+g7beda84f5-cp311-cp311-linux_aarch64.whl`。
- Gate C/D Evidence：`/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S2-GATE-C-FLAGGEMS-DIAG/evidence/20260826T092617p0800`。

这些具体 host/device/container值只用于重建该次 Accepted execution，不是 future Task固定参数。

## Accepted composite container-access template

```bash
# Resolve IMAGE, CONTAINER, task roots and authorized idle devices from the
# current Task/preflight. Do not copy historical device numbers literally.
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

This composite pattern is execution-proven and accepted. No per-flag ablation was run; therefore retain the whole pattern by default, but do not claim every flag/mount is independently necessary. Any future narrowing needs its own successful Evidence.

## Mandatory post-launch invariant

Before FL、FlagGems、model或operator diagnosis：

```python
import torch
import torch_npu  # noqa: F401

assert torch.npu.is_available()
assert torch.npu.device_count() == EXPECTED_MAPPED_DEVICE_COUNT
for i in range(torch.npu.device_count()):
    print(i, torch.npu.get_device_name(i))
```

If `torch_npu` imports but NPU unavailable/count mismatch：

- first blocker=`container/runtime access`；
- 不继续 FL/FlagGems诊断；
- 不安装 FlagGems掩盖问题。

## Clean build reconstruction

- 使用一个 Task container完成 build/install/runtime闭环。
- Source/build path不得包含会进入 CMake unescaped regex的特殊字符；accepted workaround使用 no-plus timestamp path。
- `VLLM_VENDOR=ascend`、`SOC_VERSION=ascend910_93`、exact CATLASS `41bf90da655bba3c66d0acd7e00abe33960ecfd6`。
- Dependency downloads必须有可审计 network/proxy或 verified offline cache；不记录 credentials。
- Build success后保存 wheel hash/ABI/inventory、8 OPP/9 schemas和 A2-residue audit。

## Standalone FL transaction

在 source tree之外：

1. uninstall `vllm-ascend`和 old `vllm-plugin-fl`；
2. install accepted wheel；
3. new Python process验证 site-packages origin；
4. verify `vllm_ascend` absent；
5. set `VLLM_PLUGINS=fl`、`USE_FLAGGEMS=0`；
6. verify PlatformFL、wheel `_C_ascend`、A3 OPP origin。

## Accepted smoke and limits

Accepted Gate D smoke：wheel-packaged `_C_ascend.npu_add_rms_norm_bias`，BF16 `(2,1024)`，NPU input/output、synchronize、CPU reference、finite/correct、no CPU fallback。

未覆盖：full model、TP2/HCCL、GDN/Mamba/attention/MoE end-to-end、graph、serve、prefix、64K和performance。

## Preservation and revalidation

- Accepted container/wheel/Evidence须保留到后续明确 Decision。
- Image/source/wheel/driver/CANN/device topology变化时重新记录 identity。
- Future Task map only authorized idle devices；expected count必须等于实际映射数量。
- Current tracked branch不自动继承 `7beda84...` Acceptance；按 diff决定最小 regression。
