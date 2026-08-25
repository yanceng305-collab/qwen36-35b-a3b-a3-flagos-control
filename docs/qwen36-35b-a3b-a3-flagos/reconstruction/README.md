# Reconstruction Index

当前没有 A3-validated runtime reconstruction。

从 Stage 1开始，每个 Accepted里程碑应按需记录：

- host/device/driver/firmware边界；
- base image digest/ID、container runtime、mount/device mapping；
- CANN/Python/torch/torch-npu/vLLM/Transformers/Triton/HCCL tuple；
- source repo/branch/SHA/tree/clean state；
- A3 build family、wheel filename/hash/inventory、CATLASS identity；
- package transaction、site-packages/module/library/OPP origin；
- cache roots与 invalidation contract；
- startup/effective config；
- Evidence absolute pointers、manifest/checksums、immutable Result与 Acceptance commit；
- host-local与 portable artifacts的明确区别；
- tested scope、unknowns、revalidation triggers。

仅有 image tag、container name或 shell history不足以形成 reconstruction。未 Accepted的候选记录留在 Result/Evidence，不升级为正式 handoff。
