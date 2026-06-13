---
id: kz-tp-01KTZ6OPSTATSEM013C
type: task-packet
status: draft
project: kaizen-platform
created: 2026-06-14T00:45:00Z
updated: 2026-06-14T00:45:00Z
review_status: pending
authority: proposed
summary: "Correct operation-status terminal semantics without weakening pre-action safety or mutation gates."
---

# Task Packet 013C - Operation-Status Terminal-Semantics Correction

## Purpose

Correct the accepted operation-status contract and platform implementation so trustworthy committed, recovered, or failed terminal history is preserved while current-world Git and integrity findings remain separately visible.

This packet does not authorize implementation until separately owner-approved at its exact audited SHA-256.

## Evidence basis

```text
Result 174 - Packet 013B Milestone 9 readiness result
Result 175 - Packet 013B completion audit
Result 176 - Packet 013B owner completion acceptance
Result 177 - operation-status terminal-semantics contract reconciliation
```

Verified source evidence:

```text
05-specs/milestone-6-governed-operator-surface.md
SHA-256:
0474fe139c4daa24260113b008fd9d16012367788faf1e4733b711fc7062d77c

C:\dev\kaizen\platform\src\kaizen\operation_status.py
SHA-256:
5f75fb64635b3783a3fe6a629da398bbc4c985856fd5628248d88ffdd6346a74

C:\dev\kaizen\platform\tests\test_operation_status.py
SHA-256:
f34d786de06ab0ed1a0013a92ac837847a14ba7e225fe76058d6e661877759a8
```

Any source-hash mismatch before editing stops execution.

## Repository scope

### Kaizen docs

```text
C:\dev\kaizen-docs
```

Allowed changes:

- `05-specs/milestone-6-governed-operator-surface.md` operation-status sections only;
- Packet 013C implementation return;
- Packet 013C completion security/steward audit;
- later owner completion record only after owner acceptance.

### Kaizen platform

```text
C:\dev\kaizen\platform
```

Allowed changes are limited to the smallest files required for status calculation, CLI/operator presentation, and focused/full tests.

Expected candidate paths:

```text
src/kaizen/operation_status.py
tests/test_operation_status.py
tests/test_operation_status_cli.py
tests/test_operator_console.py
```

Additional platform paths require exact evidence in the implementation return and must be necessary for the accepted contract. No schema, event-log, promotion-workflow, amendment-workflow, or public API expansion is presumed.

## Required contract behavior

### Terminal lifecycle truth

When one trustworthy terminal event and its installed/result evidence verify:

```text
state: committed | recovered | failed
```

must remain the lifecycle state even if later current-world findings exist.

### Findings remain visible

All current mismatches remain in the existing structured `mismatches` list and Git block.

Terminal-state preservation must not hide:

- dirty repository state;
- branch or HEAD drift;
- unrelated later changes;
- later current-state conditions requiring review.

### Invalid terminal evidence

`state: invalid` remains required when the terminal claim itself cannot be trusted, including:

- duplicate successful terminal events;
- contradictory event sequences;
- terminal binding corruption;
- installed/result hash mismatch;
- missing destination for a claimed committed result;
- malformed or unsupported terminal evidence;
- tampered plan, approval, packet, operation, or event evidence preventing trustworthy attribution.

### Nonterminal safety

No pre-action safety gate may weaken.

Before terminal completion, any Git, plan, approval, packet, candidate, prior, diff, destination, root, or validation mismatch must continue to prevent execution and produce non-actionable status.

### Eligibility

For `committed`, `recovered`, and `failed`:

```text
eligibility.execute: false
eligibility.recover: false
```

always.

No terminal-with-findings state may become executable or recoverable.

### CLI and operator behavior

Structured output must preserve terminal state and mismatch details.

Human-facing and exit behavior must distinguish a completed operation requiring attention from corrupt or incomplete evidence.

The implementation may retain a nonzero status-command exit for terminal-with-findings if documented and tested. Mutation commands must continue to reject any pre-action mismatch.

## Required implementation method

1. Reconcile the operation-status specification before or in the same reviewed commit chain as platform code.
2. Classify mismatches by whether they invalidate terminal history or describe later/current-world posture.
3. Preserve existing `OperationStatus` shape unless evidence proves an additive field is essential.
4. Avoid a new public state unless the existing `state + mismatches + git + eligibility` contract is demonstrably insufficient.
5. Preserve historical event compatibility.
6. Preserve idempotent read-only inspection.
7. Preserve exactly-once mutation semantics and all existing typed gates.

## Focused tests

Required minimum:

1. committed promotion with expected dirty Git keeps `committed` and retains `git_not_clean`;
2. committed amendment with expected dirty Git keeps `committed` and retains `git_not_clean`;
3. recovered operation with later dirty Git keeps `recovered`;
4. failed operation with later Git finding keeps `failed`;
5. terminal destination/result tamper produces `invalid`;
6. duplicate successful terminal events produce `invalid`;
7. approved nonterminal dirty state remains `invalid` and non-executable;
8. ready nonterminal branch/HEAD drift remains `invalid`;
9. inspect is repeatable and side-effect free;
10. terminal inspection remains terminal after the canonical repository is committed clean;
11. JSON output preserves terminal state, Git status, mismatches, events, and disabled eligibility;
12. CLI exit behavior is exact and tested;
13. operator execute and recover reject pre-action mismatches;
14. the Packet 012E amendment scenario is reproduced in a disposable Git repository;
15. promotion and amendment paths have equivalent semantics.

## Hammer proof

Add or extend a focused platform hammer test proving:

- terminal event plus expected dirty repository;
- terminal event plus unrelated later drift;
- installed-result tamper;
- duplicate terminal evidence;
- nonterminal dirty-state rejection;
- repeat inspections return identical structured status;
- inspection causes no file or Git mutation.

Do not introduce a generalized new hammer framework.

## Full verification

Required after focused proof:

```text
focused operation-status, CLI, operator, promotion, and amendment tests
relevant hammer tests
complete platform pytest suite
python compileall over platform source/tests
repository-specific Ruff profile when available under accepted policy
static search for forbidden public-state or event-schema changes
exact changed-path verification
staged-diff inspection
```

If the monolithic suite exceeds the accepted tool boundary, use complete non-overlapping partitions and record every test file and count.

## Git requirements

### Platform

- one local platform commit only;
- no remote creation or push;
- exact changed paths verified before commit;
- final platform working tree clean.

### Docs

- one docs commit containing the accepted contract amendment and Packet 013C evidence;
- push only to existing `origin/main`;
- exact staged and commit path verification;
- final docs tree clean and synchronized.

The platform and docs commits remain separate.

## Explicit exclusions

- canonical current-state amendment preparation, planning, approval, execution, or vault commit;
- vault or staging mutation;
- Kaizen MCP or Go8 production changes;
- connector, tunnel, process, or lifecycle action;
- Milestone 9 artifact or repository creation;
- fallback-runbook implementation;
- backup Generation 3;
- event-schema version change;
- new event phase;
- new mutation route;
- generalized rollback, correction, cleanup, delete, or move semantics;
- governance-compression doctrine;
- Observatory, Postgres, Qdrant, LangGraph, Hermes, UI, provider, dependency, or deferred-system work;
- platform remote or push;
- cleanup or deletion.

## Stop conditions

Stop and return to planning when:

- any bound source hash differs;
- trustworthy versus untrustworthy terminal findings cannot be separated without changing the event schema;
- a new public state or field is required;
- mutation gating must weaken;
- a non-authorized repository or path must change;
- the focused fix causes unrelated workflow failures;
- the full test suite reveals a broader contract conflict;
- exact Packet 012E behavior cannot be reproduced in disposable evidence.

## Completion evidence

Return:

- exact contract diff;
- before/after hashes;
- exact platform changed paths;
- focused and full test evidence;
- hammer evidence;
- compileall and lint evidence;
- static compatibility scans;
- platform commit hash and clean status;
- docs implementation return and completion audit hashes;
- docs commit/push evidence;
- exact owner completion-acceptance phrase.

## Authorization boundary

Drafting and auditing this packet are authorized.

Implementation requires separate owner approval bound to the exact audited Packet 013C SHA-256.
