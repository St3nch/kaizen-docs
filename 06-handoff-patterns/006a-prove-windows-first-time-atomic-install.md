---
id: kz-tp-01KTHS7HBYG02WFC12C95QMVHF
type: task-packet
status: active
project: kaizen-platform
summary: Prove a Windows first-time atomic canonical-install primitive in disposable roots without implementing promotion.
created: 2026-06-07T19:33:51Z
updated: 2026-06-07T19:40:21Z
review_status: approved
authority: accepted
primary_spec: 05-specs/staging-write-wrapper-and-promotion-recovery.md
related_specs:
  - 05-specs/staging-write-wrapper-and-promotion-recovery.md
  - 05-specs/staging-and-promotion-workflow.md
  - 05-specs/kaizen-hammer-test-strategy.md
---

# Task Packet 006A - Prove Windows First-Time Atomic Install

> Security status: security-audited pass and explicitly owner-approved on 2026-06-07. This packet authorizes only the bounded disposable-root implementation and tests below. It still authorizes no live canonical-vault mutation, no promotion workflow, and no agent-facing write surface.

## Objective

Implement and hammer-test one narrowly scoped Windows-native primitive in `C:\dev\kaizen\platform` that installs a prepared temporary file into a previously absent destination on the same volume.

The primitive must provide one indivisible destination transition from absent to present and must fail without replacing, truncating, deleting, or modifying an existing destination.

The purpose of this packet is to prove the filesystem primitive required by later Packet 006B. It is not a promotion implementation.

## Read First

1. `C:\dev\kaizen-docs\03-research-results\025-implementation-checkpoint-audit-steward-reconciliation.md`
2. `C:\dev\kaizen-docs\03-research-results\026-staging-boundary-evidence-hardening-steward-audit.md`
3. `C:\dev\kaizen-docs\05-specs\staging-write-wrapper-and-promotion-recovery.md`
4. `C:\dev\kaizen-docs\05-specs\staging-and-promotion-workflow.md`
5. `C:\dev\kaizen-docs\05-specs\kaizen-hammer-test-strategy.md`
6. `C:\dev\kaizen-docs\06-handoff-patterns\006-implement-human-operated-canonical-promotion.md` - retired source material only
7. `C:\dev\kaizen\platform\AGENTS.md`
8. `C:\dev\kaizen\platform\src\kaizen\windows_fs.py`

Primary Microsoft documentation to verify before selecting the primitive:

- `SetFileInformationByHandle`
- `FILE_INFO_BY_HANDLE_CLASS`, especially `FileRenameInfo` and `FileRenameInfoEx`
- `FILE_RENAME_INFO` and any supported extended rename structure/flags
- `CreateFileW` access and sharing rules
- `FlushFileBuffers`
- handle-based file and volume identity APIs used for same-volume proof

Do not rely on memory, secondary articles, or a check-then-rename pattern.

## Context Pack

Current proven state:

```text
platform: f377f53
platform suite: 130 passed, zero skipped reported
R2 evidence hardening: complete and steward-audited in Result 026
vault: fc0135c
canonical promotion: not implemented or authorized
combined Packet 006: archived and not eligible for approval
```

## Scope

Implement only:

1. a platform-internal Windows primitive for installing one already-created temporary file into one absent final destination;
2. trusted parent-directory handles for the source and destination names;
3. explicit same-volume proof before the install call;
4. exact destination-absent semantics enforced by the selected native operation rather than by preflight alone;
5. stable typed results and failure codes for the primitive;
6. cleanup ownership rules for packet-created temporary files;
7. unit, integration, subprocess, race, lock/share-violation, reparse/junction, interruption, and repeated-recovery tests using disposable roots;
8. platform-local documentation of the proven primitive and its limits.

The leading candidate is a handle-based rename through `SetFileInformationByHandle` using a rename information class and replacement disabled. The implementation must verify exact structure, flag, access, sharing, root-directory-handle, and operating-system behavior from primary Microsoft documentation and live tests before adopting it.

## Non-Scope

Do not implement:

- promotion requests, plans, approvals, or approval persistence;
- canonical-content normalization;
- promotion event models or `_governance/promotion-log.jsonl`;
- intent, committed, failed, or recovered promotion events;
- recovery scanning across promotion operations;
- staging-source retention or deletion policy;
- global note-ID uniqueness or relationship resolution;
- a `kaizen-promote` CLI;
- any write to `C:\dev\kaizen\vault`;
- any write to `C:\dev\kaizen\staging`;
- overwrite, amendment, supersedence, correction, rollback, delete, or general rename APIs;
- Hermes, MCP, Postgres, Qdrant, providers, Tauri, Obsidian plugins, or UI work;
- ACL, registry, Developer Mode, service, or global system changes;
- Git remote creation or push.

## Allowed Changes

Implementation changes are limited to:

```text
C:\dev\kaizen\platform\src\kaizen\windows_fs.py
C:\dev\kaizen\platform\src\kaizen\<one narrowly named primitive module if separation is justified>
C:\dev\kaizen\platform\tests\
C:\dev\kaizen\platform\README.md
```

All runtime mutation must occur beneath pytest or explicitly created disposable roots outside the live vault and staging roots.

## Do Not Touch

- `C:\dev\kaizen\vault` contents or Git history;
- `C:\dev\kaizen\staging` contents, smoke artifact, or attempt log;
- `C:\dev\kaizen-docs` during implementation;
- existing accepted decisions;
- production promotion contracts;
- global Windows, Git, or Python configuration.

## Implementation Requirements

### Primitive contract

The primitive must accept only trusted handles and validated leaf names. It must not accept caller-selected absolute paths or a general filesystem command.

Conceptual contract:

```text
install_first_time_atomic(
    source_parent_handle,
    source_leaf_name,
    destination_parent_handle,
    destination_leaf_name,
) -> result
```

The exact Python API may differ, but it must remain narrower than a general rename surface.

### Required preconditions

Before the native install call:

- the source temporary file exists and is owned by the current test/operation;
- source and destination parent handles are verified non-reparse directories beneath the disposable trusted root;
- source and destination are proved to be on the same volume by handle-derived identity;
- the destination leaf name is valid and relative;
- no caller-controlled replacement or overwrite flag exists;
- source content has been flushed and its expected hash recorded by the test fixture.

Destination absence may be checked for diagnostics, but safety must not depend on that check remaining true.

### Required native behavior

The selected operation must prove through documentation and live tests that:

- the source is renamed within the same volume;
- the destination transition is atomic from observers' perspective at the filesystem namespace level;
- replacement is disabled;
- an existing destination causes a stable failure;
- the existing destination remains byte-for-byte unchanged;
- the source remains recoverable or its resulting state is deterministically classifiable after failure;
- no check-then-rename race can silently replace an unrelated file.

If these properties cannot be proved, stop as `blocked`.

### Handle and sharing behavior

The implementation must explicitly define and test:

- access rights required on the source file handle;
- access and sharing modes used when creating and reopening the source temporary file;
- destination-parent handle lifetime;
- delete-sharing implications for the source handle;
- behavior when another process holds the source or destination with incompatible sharing;
- behavior when the destination parent is replaced or becomes a reparse point;
- handle cleanup on every success and failure path.

### Failure codes

Provide stable codes at minimum for:

```text
invalid_install_request
source_missing
source_not_owned
destination_exists
destination_reparse_forbidden
source_reparse_forbidden
cross_volume_unsupported
source_lock_interference
destination_lock_interference
atomic_install_failed
post_install_verification_failed
ambiguous_install_state
internal_failure
```

Reuse existing equivalent native failure codes where doing so remains unambiguous.

### Result contract

The result must provide enough evidence for tests and later Packet 006B without exposing a broad API:

```text
result
source_state
destination_state
source_volume_identity
destination_volume_identity
installed_file_identity
installed_size
installed_sha256
failure_code
message
```

Do not add approval, promotion-event, approver, plan, or staging-finalization fields.

## Constraints

- Windows 11 / current owner workstation is the first-slice execution environment.
- Python remains 3.11.15.
- Use only primary Microsoft documentation to justify native semantics.
- Do not claim power-loss durability from namespace atomicity.
- Do not call a check-then-rename sequence race-resistant.
- Do not use `MoveFileEx` with replacement enabled.
- Do not use copy-and-delete or cross-volume fallback.
- Do not weaken destination-collision refusal for test convenience.
- Do not install into the live canonical vault even for a smoke test.
- Do not expose the primitive through Hermes, MCP, or a public CLI.

## Required Tests

At minimum:

1. valid first-time install into an absent destination;
2. destination already exists before the call;
3. destination appears after preflight but before the native call;
4. two independent processes race to install different source files to the same destination;
5. exactly one install succeeds and the loser cannot replace the winner;
6. winner bytes and identity match the winning source;
7. losing source and temporary-state outcome are deterministically classified;
8. same-volume proof succeeds for source and destination handles;
9. cross-volume attempt is rejected before namespace mutation;
10. destination parent is a symbolic link, junction, mount point, or other reparse point;
11. destination parent replacement/race probe;
12. source file is held with incompatible sharing;
13. destination file appears and is held with incompatible sharing;
14. source temporary file disappears before install;
15. source content or identity changes after preflight;
16. process terminates immediately before the native call;
17. process terminates immediately after the native call returns success but before higher-level bookkeeping;
18. post-install destination hash and file identity verification;
19. no silent replacement under repeated contention;
20. orphan temporary-file ownership and cleanup rules;
21. repeated recovery/classification converges;
22. unrelated files beneath the disposable root remain byte-for-byte unchanged;
23. full platform suite remains green with zero unexplained skips.

Every test must assert relevant exit/result code, source state, destination state, bytes, hashes, identities, and unrelated-file state.

## Validation

Before completion:

1. run focused primitive tests repeatedly;
2. run independent-process contention tests repeatedly, not once;
3. run the full platform suite;
4. report passed, failed, skipped, and warning counts;
5. run `git diff --check`;
6. scan for backup, temporary, malformed, and suspicious-control-character files;
7. confirm live vault and staging Git/filesystem state did not change;
8. review every native declaration, structure size, access mask, share mode, flag, error mapping, and handle-close path;
9. preserve primary-source references in platform documentation or the completion report;
10. produce a steward-reviewable completion report.

## Acceptance Criteria

1. one narrow first-time atomic-install primitive exists;
2. destination replacement is impossible through the exposed contract;
3. an existing destination is never modified;
4. a destination-appearance race cannot silently replace the winner;
5. same-volume behavior is proved from handles;
6. cross-volume fallback does not exist;
7. source and destination reparse/junction attacks fail closed;
8. lock and sharing violations produce deterministic outcomes;
9. post-install identity, size, and hash are verified;
10. interruption states are deterministic and recoverable by later higher-level logic;
11. unrelated files remain unchanged;
12. all required tests pass with zero unexplained skips;
13. no live vault or staging mutation occurs;
14. no promotion model, event log, approval plan, CLI, or agent write surface is introduced;
15. implementation and completion evidence are committed in the platform repository;
16. a steward audit determines whether the primitive is suitable for Packet 006B.

## Stop Conditions

Stop and report `blocked` when:

- primary Microsoft documentation does not support the required collision-refusing semantics;
- the chosen structure or information class is unavailable or ambiguous on the supported runtime;
- live tests show silent replacement is possible;
- the destination-appearance race cannot be made safe;
- source/destination same-volume identity cannot be proved;
- the implementation requires a broad path-based rename API;
- the implementation requires touching the live canonical vault or staging root;
- test setup requires global ACL, registry, policy, service, or Developer Mode changes;
- failure leaves an ambiguous namespace state that cannot be classified from handles and filesystem evidence.

Do not weaken the contract to avoid a blocker.

## Rollback and Recovery

Code rollback is through Git revert.

Disposable runtime artifacts may be removed only when ownership is explicit and confinement is proved. Existing or unrelated files are never removed as cleanup.

This packet does not implement promotion recovery. It only records the source and destination states needed for later Packet 006B recovery design.

## Completion Report

Return:

```text
Status: complete | partial | blocked
Platform repository and commit:
Selected native primitive:
Primary Microsoft references:
Exact structures, flags, access masks, and sharing modes:
Files created:
Files modified:
Commands run:
Focused test results:
Repeated contention results:
Full-suite results:
Skipped tests and reasons:
Source state after success/failure:
Destination state after success/failure:
Cross-volume result:
Reparse/junction result:
Lock/share-violation result:
Interruption results:
Unrelated-file verification:
Live vault status and commit:
Live staging inventory/hash verification:
Deviations:
Unresolved risks:
Recommendation for Packet 006B:
```
