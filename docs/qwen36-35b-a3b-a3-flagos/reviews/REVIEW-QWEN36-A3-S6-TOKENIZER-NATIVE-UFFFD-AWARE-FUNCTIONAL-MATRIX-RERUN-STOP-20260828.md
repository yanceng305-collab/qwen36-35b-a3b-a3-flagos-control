# Codex1 Formal Review — Stage 6 tokenizer-native U+FFFD-aware rerun STOP

Review date: 2026-08-29

Result reviewed: [`RESULT-QWEN36-A3-S6-TOKENIZER-NATIVE-UFFFD-AWARE-FUNCTIONAL-MATRIX-RERUN-20260828T161700+0800.md`](../results/RESULT-QWEN36-A3-S6-TOKENIZER-NATIVE-UFFFD-AWARE-FUNCTIONAL-MATRIX-RERUN-20260828T161700+0800.md)

Disposition: **ACCEPTED — valid auditable STOP, F0 device-scope mapping drift**

Stage 6: **STOP / NOT ACCEPTED**

## Scope and identity

The immutable Result is reviewed against live Control `main` at `da5f8c1f2f59cbd4abefb7d2cb41e091c952bbca`. It records the exact D-034 Task, Frozen source/tree/wheel, Accepted image, model, container, and Evidence pointers. No immutable historical Result is modified by this Review.

## Findings

1. The formal Result correctly reports `F0 STOP — DEVICE-SCOPE MAPPING DRIFT BEFORE READINESS`; F1 O8 and F2 O1024 were not run, with zero workload requests/cells. The shutdown and Task-resource cleanup evidence is sufficient for an auditable STOP.
2. Admission selected physical NPU 7 with host logical devices 14/15. The service start script then set `ASCEND_VISIBLE_DEVICES=0,1` and `ASCEND_RT_VISIBLE_DEVICES=0,1`. In the privileged/direct-device container, the Result evidence shows these values were not proven container-local renumbering: Task APIServer/TP worker PIDs landed on host logical 0/1. This is a real scope mismatch before readiness and is the first blocker.
3. The precise attribution is: **the service launch overwrote the already-resolved host logical scope with an unproven `0,1` visibility convention in this exact privileged/direct-device runtime**.
4. This Review does not generalize the finding to Accepted image incompatibility, universal Ascend remapping behavior, physical NPU 7, logical 14/15, or Frozen runtime failure. A future Task must resolve and prove its actual mapping dynamically and preserve it through service launch.
5. The Result's `/data:/data` failure and User-authorized `/data/tiankuan:/data/tiankuan` narrowing are accepted as a Task/runtime mount-scope correction. All required model, wheel, work, Evidence, artifact, and cache paths are under `/data/tiankuan`; this does not alter Frozen source/model/runtime/service/workload, does not mutate the host, and does not rewrite the historical `/data:/data` template. The narrowed bind may be used by the follow-up Task.

## Acceptance boundary

Accepted claims are limited to the exact STOP scope and evidence listed above. This Review does not accept F0 readiness, any O8/O1024 cell, U+FFFD classification, or Stage 6 PASS. The parent Result remains immutable and its first blocker is not rewritten.

The original Task is ended by this STOP and must not be resumed. The only next route is the new bounded device-mapping and provenance-correlation follow-up, pending explicit User dispatch.

## Concurrent-run handling

The later same-Task session ending around `20260828T180824+0800` is not a second formal execution Result. Its cleanup and no-publish behavior are accepted as Control-race handling. Its artifacts are registered separately as late concurrent-run supplemental diagnostic Evidence; they do not alter this Result, its first blocker, formal progress, or historical acceptance boundary.
