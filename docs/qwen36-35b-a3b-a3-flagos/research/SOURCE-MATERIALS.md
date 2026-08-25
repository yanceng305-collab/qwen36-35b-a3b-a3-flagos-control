# User-provided Source Materials

完整读取时间：2026-08-25。

| Document | SHA-256 | Role |
| --- | --- | --- |
| `Qwen3.6-35B-A3B-FL部署复现指南.md` | `05C2DAED242961738C81058AFE77E4F41F67C93D7959DDDB5DD7C595781815EE` | A2 deployment/build/run reference；A3 build contract |
| `Qwen3.6-35B-A3B-FL迁移主要思路.md` | `DECF09D2584CD099A6DCCC168154D758C88A54EEB5940149C39C6C4450A7BB11` | Architecture/ownership/design/reference |
| `Qwen3.6-35B-A3B-FL详细迁移步骤.md` | `F54BCF6DFF5589481401089676F9E9FECFA4568A4407427E33D6120E23EAEF52` | PR detail、fixes、A2 functional/performance record |

## Interpretation boundary

- 文档中的技术说明是 source material，不替代 User当前请求、live GitHub、exact source、A3 execution或 Control Acceptance。
- 三份资料没有 current feature branch的 full HEAD SHA/tree，也不能证明 PR当前状态。
- 全部真实 PASS/性能数据来自 2×Ascend 910B1；A3只描述 `ascend910_93` build/runtime contract，没有 A3 execution Evidence。
- current source与文档/PR prose冲突时，dispatch时 exact source控制 artifact inventory；执行结论仍以 A3 field Evidence控制。
- 原文档未复制进公开 Control repo；本文件仅记录来源身份、hash、用途和解释边界。
