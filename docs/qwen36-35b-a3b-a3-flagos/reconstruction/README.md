# Reconstruction Index

当前 Accepted reconstruction：

- [A3-STAGE1-2-ACCEPTED-RUNTIME.md](A3-STAGE1-2-ACCEPTED-RUNTIME.md)：exact `7beda84...` A3 environment、wheel、container/runtime access、standalone FL与 custom-op smoke。

最终项目收尾时将基于 Accepted Results、Formal Reviews、[`A2-TO-A3-VALIDATION-DELTA.md`](../A2-TO-A3-VALIDATION-DELTA.md)、本目录记录和 preserved artifacts/Evidence生成 `A3-END-TO-END-REPRODUCTION.md`。在 functional matrix、performance、prefix、EP2等最终合同冻结前不提前编写该文档；从 Stage 6开始的每个 Result必须遵守[`REPOSITORY-AND-EVIDENCE-RULES.md`](../REPOSITORY-AND-EVIDENCE-RULES.md)的 reproduction minimum，保证最终文档不依赖任何聊天或执行者记忆。

最终文档必须明确绑定且只复现：

```text
Frozen source: e610a990d785356bf51a3cad50219d4c03310a31
Frozen tree: 609ff1ad0f08239f353cb4d8774e504b4deba03b
Accepted wheel: vllm_plugin_fl-0.2.0+ge610a990d-cp311-cp311-linux_aarch64.whl
Wheel SHA-256: 2fcf788660f3fe42b364bc60d593ee1b9b634fc0632de58c444d961bff4aa1bd
```

PR #404、historical feature branch和official base不得被解析为“自动选择future latest HEAD”。若未来验证新HEAD，必须另立validation baseline/project evidence，不能覆盖本项目`A3-END-TO-END-REPRODUCTION.md`或Accepted Results。

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
