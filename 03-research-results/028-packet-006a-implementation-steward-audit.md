---
id: kz-aud-01KTHTBF8WZS6PCSSHD8RR1CQB
type: audit
status: complete
project: kaizen-platform
summary: Steward audit of the Packet 006A first-time atomic-install primitive implementation.
created: 2026-06-07T19:53:28Z
updated: 2026-06-07T19:53:28Z
review_status: approved
related_specs:
  - 05-specs/staging-write-wrapper-and-promotion-recovery.md
  - 05-specs/staging-and-promotion-workflow.md
  - 05-specs/kaizen-hammer-test-strategy.md
---

# Research Result 028 - Packet 006A Implementation Steward Audit

## Scope

Audit platform commit `26271ce` against the owner-approved Packet 006A boundary.

The implementation was limited to a disposable-root Windows primitive for installing one owned temporary file into one absent sibling destination. It did not implement promotion plans, approval persistence, promotion events, recovery scanning, a promotion CLI, live canonical writes, Hermes, or MCP access.

## Files reviewed

```text
README.md
src/kaizen/windows_fs.py
src/kaizen/atomic_install.py
tests/test_atomic_install.py
```

## Native primitive selected

The implementation uses:

```text
NtSetInformationFile
FILE_INFORMATION_CLASS = FileRenameInformation (10)
FILE_RENAME_INFORMATION.Flags = 0
RootDirectory = verified destination-parent handle
```

The source file handle is opened with DELETE, read, attribute, and synchronize rights. The rename information uses the verified parent directory handle and a relative destination leaf. Replacement flags are absent.

Primary Microsoft documentation supports these core properties:

- rename requires DELETE access;
- RootDirectory may identify the destination directory for a relative name;
- source and destination must be on the same volume;
- when replacement is disabled and the destination exists, the operation fails.

The implementation does not use `MoveFileEx`, copy-and-delete fallback, or a check-then-rename safety claim.

## Proven behavior

### Successful first-time install

A prepared temporary file transitions from:

```text
source: present
destination: absent
```

to:

```text
source: absent
destination: present
```

The installed file preserves source file identity and exact SHA-256 bytes.

### Existing destination refusal

The native call returns `STATUS_OBJECT_NAME_COLLISION (0xC0000035)` when the destination exists. Tests prove:

```text
source bytes: unchanged
destination bytes: unchanged
replacement: none
```

### Destination-appearance race

A test creates the destination immediately before the native call. The operation fails as `destination_exists`; the racing destination remains byte-for-byte unchanged and the source remains recoverable.

### Independent-process contention

Two independent Python processes race different owned sources to one destination. Exactly one succeeds and the other receives `destination_exists`.

The repeated-contention test performs twenty races in one suite run. No loser replaced the winner.

### Handle-bound identity and hash verification

Source hash verification occurs through the already-open source handle, not by reopening a pathname. Post-install hash and file identity verification occur through a handle reopened relative to the still-held verified parent handle.

This avoids treating pathname re-resolution as authority during the critical operation.

### Parent replacement resistance

The verified parent handle is opened without delete sharing and held through install and verification. A test attempts to rename the parent after handle acquisition; Windows blocks the replacement and the install remains confined to the verified directory.

### Reparse/junction refusal

A junction used as the destination parent is rejected as `destination_reparse_forbidden`. No target file is installed through the junction.

### Lock/share interference

A separate process holds the source with incompatible sharing. The primitive fails deterministically as `source_lock_interference`; no destination appears.

### Interruption classification

A subprocess exits with `os._exit(91)`:

- immediately before the native call: source remains, destination absent;
- immediately after the native call: source absent, destination present with approved bytes.

The after-install state is stable across repeated classification. Packet 006A does not implement promotion recovery; it proves the namespace state is deterministic enough for Packet 006B recovery design.

## Test evidence

```text
Focused Packet 006A suite: 15 passed
Repeated focused runs: 3 consecutive runs, 15 passed each
Full platform suite: 145 passed
Failed: 0
Skipped: 0 reported
Diff check: clean
Control-character scan: clean
Backup/temp sweep: clean
```

The focused suite includes twenty independent-process contention races per run, for at least sixty repeated races across the final three focused runs.

## Live-root preservation

The live vault remained clean at commit `8567ff3` during implementation.

The live staging inventory remained unchanged:

```text
README.md
  bytes: 330
  sha256: 52e5f276e3b0259d31babbc09138e09e0f3d41133b563333113dcdd3ab9ff216

staging-write-attempts.jsonl
  bytes: 1486
  sha256: 35e2e1308a32015493881c25ede6d3ddefbcf527de7a09e39ddce77fd825af80

projects/kaizen-platform/claims/create-only-wrapper-smoke.md
  bytes: 1027
  sha256: d6033bacf6325abf40082fdca9bf7376888e6d7d1380bca966d3fdbc8e60d42e
```

No Packet 006A test targeted the live vault or live staging root.

## Deviations and limitations

### One-parent-handle design

The implementation deliberately narrows the conceptual packet API to one verified parent directory and two sibling leaf names. This structurally prevents cross-directory and cross-volume fallback through the exposed primitive.

### Physical cross-volume test unavailable

The workstation exposes one writable NTFS volume (`C:`). Therefore a real second-volume rename was unavailable.

Cross-volume refusal is supported by:

- the one-parent-handle API shape;
- handle-derived volume identity checks;
- a deterministic injected expected-volume mismatch test that fails before mutation;
- the selected native primitive's documented same-volume requirement.

A future environment with a second disposable writable volume should add a real cross-volume integration test before broad deployment. This limitation does not permit copy-and-delete fallback.

### Source reparse coverage

The implementation opens the source with `FILE_OPEN_REPARSE_POINT` and rejects a reparse source handle. The final suite directly proves destination-parent junction refusal. A future environment with reliable unprivileged file-symlink creation should add a direct source-symlink test.

### No power-loss claim

The audit proves namespace transition and process-interruption classification. It does not claim storage-device power-loss durability.

## Contract assessment

The implementation stays inside Packet 006A:

- no public CLI;
- no live canonical or staging writes;
- no promotion request or approval model;
- no promotion-event log;
- no plan persistence;
- no recovery scanner;
- no agent or MCP exposure;
- no overwrite-capable API.

## Result 025 disposition

The Packet 006A implementation portion of **F-12 Gate B** is complete.

The following remain open for Packet 006B or later:

```text
F-09  canonical_candidate_sha256 in the promotion plan contract
F-13  immutable promotion-plan storage and invalidation
F-14  separate _governance bootstrap before first promotion
F-15  promotion-event ID prefix
F-18  normalized promotion-event paths
F-19  promotion-event schema versioning
F-27  explicit Windows reparse-constant assertion before promotion implementation
```

Packet 006B is not approved or implemented by this result.

## Steward verdict

**pass with documented limitations**

Packet 006A is complete. The proven primitive is suitable as an implementation dependency for drafting Packet 006B.

## Next action

Draft Packet 006B for human-operated promotion planning, approval binding, event logging, execution, and recovery. Security-audit the packet before seeking owner approval. Do not begin Packet 006B implementation or any live canonical promotion.
