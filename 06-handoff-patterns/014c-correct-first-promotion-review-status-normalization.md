---
id: kz-tp-01KX4C70000000000000000000
type: task-packet
status: proposed
project: kaizen-platform
created: 2026-06-14T20:35:00Z
updated: 2026-06-14T20:35:00Z
review_status: pending
authority: proposed
summary: "Correct first-promotion normalization so not-required note types remain canonically valid."
---

# Task Packet 014C - Correct First-Promotion Review-Status Normalization

## Objective

Correct the live first-promotion planner so note types whose accepted contract requires `review_status: not-required` remain `not-required` during canonical candidate normalization instead of being forced to `approved`.

## Triggering evidence

Packet 014A Phase 4 revalidated both Northstar candidates successfully in staging mode, then the first live promotion plan failed before immutable plan creation:

```text
code: validation_failed
message: Canonical candidate did not pass validation
```

The accepted note-type contracts state:

```text
overview.review_status_values = ["not-required"]
current-state.review_status_values = ["not-required"]
command-center.review_status_values = ["not-required"]
```

The current planner unconditionally executes:

```text
meta["review_status"] = "approved"
```

Canonical validation therefore rejects those note types with `review_status_not_allowed_for_type`.

The failed operation ID `kz-prom-01KX4B70000000000000000001` created no immutable plan and was cleaned by the existing planner failure path.

## Read First

```text
src/kaizen/promotion_plan.py
src/kaizen/validation.py
schemas/note-types.json
tests/test_promotion_workflow.py
tests/test_live_promotion.py
06-handoff-patterns/014a-controlled-implementation-return-pilot-execution.md
03-research-results/207-phase-3-completion.md
```

## Scope

Allowed changed paths:

```text
src/kaizen/promotion_plan.py
tests/test_promotion_workflow.py
tests/test_live_promotion.py
```

A test file may remain unchanged when the required coverage fits completely in the other approved test file.

## Non-Scope

- no change to `schemas/note-types.json`;
- no weakening of canonical validation;
- no direct vault write;
- no change to amendment normalization;
- no change to approval, execution, recovery, exactly-once, or Git-cleanliness controls;
- no Northstar candidate rewrite unless a separate defect is proven;
- no canonical promotion during implementation of this packet.

## Implementation Requirements

1. `normalize_candidate()` must derive the note type from parsed metadata.
2. For note types whose contract allows only `not-required`, preserve or set `review_status: not-required`.
3. For review-bearing note types, retain the existing transition to `review_status: approved`.
4. Preserve the existing `authority: proposed -> accepted` transition.
5. Reject or fail closed if a note type has no resolvable contract rather than guessing.
6. Keep normalized output deterministic and LF-only.
7. Do not special-case Northstar project names or paths.

A small contract-driven helper is preferred over a hardcoded list when it can reuse the authoritative machine-readable note-type contract without widening scope.

## Validation

Required tests:

- first promotion of an `overview` candidate succeeds canonical validation and retains `review_status: not-required`;
- first promotion of a `current-state` candidate succeeds and retains `not-required`;
- existing decision/spec/task-packet promotion still normalizes review status to `approved` and proposed authority to `accepted`;
- unsupported or malformed note types continue to fail closed;
- validation failure leaves no operation directory when no plan was created;
- live clean-state, branch, fixed-root, and plan-binding tests remain green.

Run:

```text
focused promotion tests
full platform test suite
compile checks
Ruff or the repository's accepted lint command
```

## Acceptance Criteria

- the two unchanged Northstar candidates can create immutable live promotion plans from a clean checkpoint;
- no contract or schema is weakened;
- exact changed paths remain within this packet;
- platform repository remains local-only, clean, and without remotes;
- one local commit records the correction;
- Packet 014A Phase 4 resumes only after separate owner approval, implementation, audit, and Kaizen MCP restart if required.

## Completion Report

Return:

```text
starting commit
ending commit
exact changed paths
root cause
focused tests
full tests
compile/lint results
normalization behavior by note type
clean-state and remote verification
known limitations
```

## Authority Boundary

Drafting and auditing Packet 014C do not authorize platform source changes. Separate owner approval bound to the exact packet SHA-256 is required.
