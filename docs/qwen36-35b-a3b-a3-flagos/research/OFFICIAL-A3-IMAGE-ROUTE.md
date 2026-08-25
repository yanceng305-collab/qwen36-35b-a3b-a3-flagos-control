# Official vLLM-Ascend v0.20.2rc1 A3 Image Route

核验时间：2026-08-25 17:42 CST。

本文件是 Stage 1/2 base image bounded-selection决策的静态官方证据。它不证明任何 image在本项目 A3 server可用，也不构成 A3 runtime Acceptance；selected image identity和 compatibility必须由 Codex2在 dispatch后现场验证。

## Official source identities

- vLLM-Ascend release/tag：[`v0.20.2rc1`](https://github.com/vllm-project/vllm-ascend/releases/tag/v0.20.2rc1)。
- Tag commit/tree：`367b8e62da799870a7476ce34f5f7658589a8aad` / `2fcd970131dbcab1be01f0980836418449149775`。
- Current compatibility-document snapshot：vLLM-Ascend `main@d1523852b416be34cfea126a9134bb3519771834`，核验时其 versioning matrix仍把 `v0.20.2rc1`映射到 vLLM `v0.20.2`。

## Official image matrix

Tag-specific [`installation.md`](https://github.com/vllm-project/vllm-ascend/blob/367b8e62da799870a7476ce34f5f7658589a8aad/docs/source/installation.md) 将 image family区分为：

| Tag form | Hardware | OS | Project use |
| --- | --- | --- | --- |
| `v0.20.2rc1` | Atlas A2 | Ubuntu | **Excluded from formal A3 baseline** |
| `v0.20.2rc1-openeuler` | Atlas A2 | openEuler | Excluded |
| `v0.20.2rc1-a3` | Atlas A3 | Ubuntu | Allowed candidate |
| `v0.20.2rc1-a3-openeuler` | Atlas A3 | openEuler | Allowed candidate |

Tag-specific image workflow maps:

- A3 Ubuntu → [`Dockerfile.a3`](https://github.com/vllm-project/vllm-ascend/blob/367b8e62da799870a7476ce34f5f7658589a8aad/Dockerfile.a3) → suffix `a3`；
- A3 openEuler → [`Dockerfile.a3.openEuler`](https://github.com/vllm-project/vllm-ascend/blob/367b8e62da799870a7476ce34f5f7658589a8aad/Dockerfile.a3.openEuler) → suffix `a3-openeuler`。

The publish workflow appends those suffixes to the release tag. Therefore the formal candidates are exactly:

```text
quay.io/ascend/vllm-ascend:v0.20.2rc1-a3
quay.io/ascend/vllm-ascend:v0.20.2rc1-a3-openeuler
```

## Official build definitions and compatibility tuple

| Field | A3 Ubuntu definition | A3 openEuler definition |
| --- | --- | --- |
| CANN base | `quay.io/ascend/cann:9.0.0-a3-ubuntu22.04-py3.11` | `quay.io/ascend/cann:9.0.0-a3-openeuler24.03-py3.11` |
| OS | Ubuntu 22.04 | openEuler 24.03 |
| Python | 3.11 base | 3.11 base |
| vLLM | `VLLM_TAG=v0.20.2` | `VLLM_TAG=v0.20.2` |
| torch / torch_npu | `2.10.0 / 2.10.0` through tag package contract | Same |
| Triton Ascend | `3.2.1` | `3.2.1` |
| Default Docker build SOC | `ascend910_9391` | `ascend910_9391` |

The current official [versioning policy](https://github.com/vllm-project/vllm-ascend/blob/d1523852b416be34cfea126a9134bb3519771834/docs/source/community/versioning_policy.md) records the full `v0.20.2rc1` compatibility line as Python `>=3.10,<3.12`, CANN `9.0.0`, PyTorch/torch_npu `2.10.0/2.10.0`, Triton Ascend `3.2.1`, matched vLLM `v0.20.2`.

The Dockerfile default `ascend910_9391` is an A3 variant, not permission to assume the target server's exact SKU. The Task must record physical SoC, effective `SOC_VERSION`, detection source and normalized `ascend910_93` family from actual field evidence.

## Registry snapshot

Quay public API reported these active multi-arch manifest-list digests at verification time:

| Tag | Manifest-list digest |
| --- | --- |
| `v0.20.2rc1` | `sha256:a926b560b81785a9f820f975748a39807f0b1753675ab40ec30cb4ae3dabb71e` |
| `v0.20.2rc1-a3` | `sha256:5cf8a2b6db8b06eb1bc7fc7d191d667aebf2b197351bdba13f776918c11ec7a7` |
| `v0.20.2rc1-a3-openeuler` | `sha256:442363921166771eb82baeec9c1ac0381f46fb830ead8d0e072df6e925f2a958` |

These values are a Control-time registry snapshot, not a substitute for pull-time platform-specific digest, local image ID and inspection. Codex2 must record the selected tag, resolved digest, image ID and actual tuple in Evidence.

## Project decision boundary

1. Codex2 may select only between the two official A3 candidates after safe host/device/driver/container inventory.
2. Control does not preselect Ubuntu or openEuler; the choice must be justified by actual host, driver, CANN/build/runtime compatibility.
3. The ordinary unsuffixed `v0.20.2rc1` is the official A2 Ubuntu route and is not a formal A3 candidate.
4. If neither A3 candidate is materially compatible or available, Codex2 must STOP and return `Decision requested`; it may not fall back to the A2 image, nightly, another vLLM/CANN line or an unpinned substitute.
5. The selected vLLM-Ascend image is an environment carrier/builder only. Final Stage 2 runtime must remove the `vllm-ascend` distribution/module dependency and prove FL wheel/site-packages ownership; the carrier name does not define runtime ownership.
