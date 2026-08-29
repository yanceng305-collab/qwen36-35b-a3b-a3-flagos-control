# Supplemental Evidence Record — composite device mapping proof

Status: **SUPPLEMENTAL DIAGNOSTIC EVIDENCE — NOT AN IMMUTABLE RESULT / NOT FORMAL STAGE 6 PROGRESS**

This record registers the User-authorized continuation after the local immutable STOP Result that remains server-local and unsynced. It does not replace or modify that Result.

Evidence root:

```text
/data/tiankuan/zyg/FL/workspace/QWEN36-A3-S6-DEVICE-MAPPING-AND-PROVENANCE-CORRELATION-FOLLOWUP/supplemental/20260829T113000+0800/evidence
```

Summary: `SUPPLEMENTAL-REPORT.md`.

User-confirmed selected scope:

```text
physical NPU 14/15
host logical 14/15
logical 13 excluded because Alarm
```

User-confirmed composite mapping:

```text
container torch index 0 -> /dev/davinci14 -> host physical/logical NPU 14
container torch index 1 -> /dev/davinci15 -> host physical/logical NPU 15
```

Evidence basis: host physical/logical inventory; device-node major/minor identity; container device configuration; `ASCEND_VISIBLE_DEVICES=14,15`; `ASCEND_RT_VISIBLE_DEVICES=14,15`; `torch_npu` available; `torch.npu.device_count() == 2`; both devices `Ascend910_9382`.

Disposition: **AUTHORIZED MAPPING PROVEN FOR THIS SUPPLEMENTAL RUN**. No mapping correction was needed. This does not prove future runs automatically map 0/1 to 14/15, does not establish universal Ascend renumbering, and does not close the need for dynamic inventory/proof on each fresh run. It closes practical mapping diagnosis for this run only.

No O8 formal cell or O1024 cell was run. The remaining blocker is `UNRESOLVED_PROVENANCE`; `TOKENIZER_NATIVE_UFFFD` and `POST_TOKENIZER_CORRUPTION` were not established. No model/FL/NPUGraph/NPU corruption claim is supported.
