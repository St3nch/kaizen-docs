---
id: kz-aud-01KTHWEY0VT7PT3NESJTA0BTCN
type: audit
status: complete
project: kaizen-platform
summary: Steward checkpoint audit of the Packet 006B first-time promotion workflow implementation.
created: 2026-06-07T20:30:19Z
updated: 2026-06-07T20:30:19Z
review_status: approved
related_specs:
  - 05-specs/staging-write-wrapper-and-promotion-recovery.md
  - 05-specs/staging-and-promotion-workflow.md
  - 05-specs/promotion-event-schema.md
  - 05-specs/kaizen-validation-gate-spec.md
---

# Research Result 030 - Packet 006B Implementation Checkpoint Audit

## Scope

Audit platform commits:

```text
032fa63 Implement Packet 006B promotion core
667f304 Harden Packet 006B promotion evidence
```

against the owner-approved Packet 006B boundary.

This is a checkpoint audit, not final closure. Packet 006B remains active until the remaining filesystem and lock-interference hammers are implemented and reviewed.

## What is implemented

The platform now contains:

```text
schemas/promotion-event.schema.json
src/kaizen/promotion_contracts.py
src/kaizen/promotion_plan.py
src/kaizen/promotion_events.py
src/kaizen/promotion_workflow.py
src/kaizen/promotion_cli.py
```

The implementation provides:

- canonical JSON hashing for plans, approvals, validation evidence, and events;
- fixed disposable promotion sandbox configuration;
- exact source and canonical-candidate SHA-256 binding;
- `kz-val-<ULID>` validation-run evidence;
- immutable create-new candidate, validation, plan, and approval records;
- human-actor and approval chronology checks;
- reviewed-diff hash binding and execution-time revalidation;
- canonical destination-path normalization and `_governance` destination rejection;
- global note-ID and relationship-resolution checks;
- body-link and staging-backlink checks;
- strict schema-versioned promotion-event validation;
- verified governance-path and reparse checks;
- canonical-root named mutex serialization;
- durable intent append before canonical temporary-file creation;
- handle-relative temporary-file creation;
- Packet 006A no-replacement installation;
- canonical identity, size, and hash verification;
- append-only committed and recovered events;
- retained staged source;
- idempotent repeated execute and recovery outcomes;
- exact live-root refusal.

## Test evidence

Final checkpoint evidence:

```text
Focused Packet 006B suite: 32 passed
Repeated focused runs: 3 consecutive runs, 32 passed each
Full platform suite: 177 passed
Failed: 0
Skipped: 0 reported
Diff check: clean
Control-character scan: clean
Backup/temp sweep: clean
```

Subprocess evidence includes:

- two independent executors for one operation converging to one physical commit and one idempotent result;
- two different approved operations racing one destination with exactly one committed winner;
- abrupt `os._exit(91)` after durable intent;
- abrupt `os._exit(91)` after canonical temporary-file creation;
- abrupt `os._exit(91)` after canonical installation before terminal event;
- recovery convergence for each interruption point.

## Proven invariants

### Immutable evidence

Candidate, validation, plan, and approval tampering are detected before canonical mutation.

### Approval freshness

Execution rejects:

- changed staged source;
- changed candidate;
- changed validation evidence;
- changed plan;
- changed reviewed diff;
- approval timestamp before validation or plan creation;
- executing actor different from approving actor;
- invalid or agent-like human actor IDs.

### Intent before mutation

Tests prove the intent event exists before the canonical temporary file is created.

### No replacement

Destination collision causes failure without replacement. Competing operations cannot replace the winner.

### Event integrity

Malformed JSONL, incomplete events, duplicate IDs, invalid schema/action/phase data, and terminal events lacking intent binding fail closed.

### Recovery

Recovery distinguishes:

- intent with no temp or canonical file;
- intent with owned approved temp;
- canonical file present with approved hash and terminal missing;
- canonical conflict;
- unknown or mismatched temp;
- terminal event with missing canonical file;
- repeated recovery.

Recovery appends evidence and does not rewrite prior events.

## Live-root preservation

The live vault remained clean.

The live staging inventory and hashes remained unchanged:

```text
README.md
  sha256: 52e5f276e3b0259d31babbc09138e09e0f3d41133b563333113dcdd3ab9ff216

staging-write-attempts.jsonl
  sha256: 35e2e1308a32015493881c25ede6d3ddefbcf527de7a09e39ddce77fd825af80

projects/kaizen-platform/claims/create-only-wrapper-smoke.md
  sha256: d6033bacf6325abf40082fdca9bf7376888e6d7d1380bca966d3fdbc8e60d42e
```

## Remaining required evidence

Packet 006B is not yet closed. The following high-value cases remain:

1. direct source-file symlink/reparse test where workstation permissions permit it;
2. governance-directory and promotion-log junction/reparse hammer tests;
3. destination-parent junction/reparse hammer test through the full promotion workflow;
4. promotion-log incompatible share/lock interference;
5. staged-source incompatible share/lock interference;
6. canonical destination-parent replacement race during full execute;
7. explicit temporary-file tamper between write and install;
8. explicit destination appearance after temp verification and before Packet 006A install;
9. explicit terminal-event append failure injection after canonical install;
10. event-schema file validation against representative intent, committed, failed, and recovered records;
11. repeated competing-operation races beyond the single focused run;
12. final native-handle and mutation-surface steward review after those tests pass.

Physical cross-volume integration remains unavailable because the workstation exposes one writable volume. The one-parent/same-volume design and injected identity mismatch remain the current evidence.

## Contract assessment

The implementation remains inside Packet 006B:

- no live `_governance` creation;
- no live canonical promotion;
- no amendment, supersedence, correction, overwrite, or general rollback surface;
- no staged-source deletion;
- no Hermes or MCP exposure;
- no network, database, Qdrant, UI, or Git mutation by the workflow;
- no caller-selected roots in the CLI;
- fixed disposable sandbox and exact live-root refusal.

## Steward verdict

**partial - core pass, remaining hammer gate open**

The implemented core is suitable for continued hardening. Packet 006B must remain active and must not be described as complete.

## Next action

Implement the remaining reparse, lock/share, race-window, terminal-append-failure, and schema-validation hammers in disposable roots. Re-run repeated focused and full-suite validation, then perform a final Packet 006B steward audit.
