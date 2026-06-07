---
id: kz-aud-01KTHD01T3DGECJ14W8VDN3G6Q
type: audit
status: complete
project: kaizen-platform
summary: Steward audit of staging-root configuration and dry-run Windows path-confinement foundations.
created: 2026-06-07T00:00:00Z
updated: 2026-06-07T00:00:00Z
review_status: approved
related_specs:
  - 05-specs/staging-write-wrapper-and-promotion-recovery.md
  - 05-specs/kaizen-hammer-test-strategy.md
---

# Research Result 021 - Staging Path Confinement Steward Audit

## Scope

Audit Task Packet 004, platform commit `b39b3a6`, and the sibling staging-root bootstrap.

## Evidence Reviewed

- trusted root configuration and dry-run path-check implementation;
- CLI human and JSON behavior;
- hostile logical-path tests;
- parent-chain, reparse-point, and junction tests;
- real staging-root contents;
- platform, vault, and documentation Git states;
- full post-commit test execution.

## Findings

### Pass - boundary separation

The implementation does not write staged notes, canonical files, promotion events, or grant agent access. `kaizen-path-check` is dry-run only.

### Pass - trusted roots

Canonical and staging roots are fixed trusted configuration, distinct siblings, and cannot be caller-selected. Invalid, overlapping, missing, UNC, device, and drive-relative root forms fail closed.

### Pass - hostile path rejection

Traversal, absolute, drive-relative, UNC, device, extended-length, null-byte, reserved-name, invalid-character, extension, trailing-space/period, and path-length cases use stable failure codes.

### Pass - existing parent confinement

Existing parents are resolved beneath the staging root. Reparse points and junctions are refused. Path checks create no target directories or files.

### Pass - staging bootstrap

`C:\dev\kaizen\staging` contains only `README.md`, is not a Git repository, is explicitly non-canonical, and grants no agent write authority.

### Pass - regression gate

Platform commit `b39b3a6` is clean. The vault remains clean at `3de6042`. The full suite reports 105 passed and 2 skipped.

### Note - symlink privilege

Two directory-symlink fixtures were skipped because the current Windows account lacks symlink-creation privilege. The Windows junction/reparse-point escape test passed. The skips are explicit and do not authorize writes.

## Contradictions and Gaps

No accepted-doctrine contradiction was found.

Python path inspection does not itself prove a race-resistant create operation. A later create-only wrapper must use a verified native handle-relative or equivalent fail-closed primitive before agent staging writes can be approved.

## Recommendation

Pass Task Packet 004.

Draft the next packet for a create-only staging-write wrapper with create-new semantics, exact validation, provenance, idempotency, and append-only attempt evidence. Keep canonical promotion separate.

## Human Verdict

**pass-with-notes**
