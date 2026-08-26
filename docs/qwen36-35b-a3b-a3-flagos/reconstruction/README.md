# Reconstruction Index

当前 Accepted reconstruction：

- [A3-STAGE1-2-ACCEPTED-RUNTIME.md](A3-STAGE1-2-ACCEPTED-RUNTIME.md)：exact `7beda84...` A3 environment、wheel、container/runtime access、standalone FL与 custom-op smoke。

最终项目收尾时将基于 Accepted Results、Formal Reviews、[`A2-TO-A3-VALIDATION-DELTA.md`](../A2-TO-A3-VALIDATION-DELTA.md)、本目录记录和 preserved artifacts/Evidence生成 `A3-END-TO-END-REPRODUCTION.md`。在 functional matrix、performance、prefix、EP2等最终合同冻结前不提前编写该文档；从 Stage 6开始的每个 Result必须遵守[`REPOSITORY-AND-EVIDENCE-RULES.md`](../REPOSITORY-AND-EVIDENCE-RULES.md)的 reproduction minimum，保证最终文档不依赖任何聊天或执行者记忆。

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
