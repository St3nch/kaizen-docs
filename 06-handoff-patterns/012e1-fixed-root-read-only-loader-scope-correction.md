---
id: kz-tp-01KTW4B4CDEFGHIJKLMNOPQ
type: task-packet
status: draft
project: kaizen-platform
created: 2026-06-11T22:10:00Z
updated: 2026-06-11T22:10:00Z
review_status: pending-review
authority: proposed
summary: "Packet 012E.1 fixed-root read-only prepared-candidate loader scope correction."
primary_spec: kz-spec-01KTW2M8N4Q6S8W0Y2Z4ABCDEF
---

# Task Packet 012E.1 - Fixed-Root Read-Only Loader Scope Correction

> Narrow correction packet. No implementation is authorized until the owner approves this packet's exact audited SHA-256. Packet 012E remains paused.

## Objective

Correct the platform live-scope gate so the read-only prepared-candidate loader can verify existing evidence under the fixed live roots when optional packet/live binding is omitted, while preserving verified live-scope requirements for all mutation-capable paths.

## Trigger and root cause

Packet 012E observed:

```text
live_root_forbidden: Packet 006B implementation is restricted to disposable roots
```

`enforce_promotion_scope(config, scope=None)` detects live roots and calls `ensure_disposable_roots(...)`, which rejects them before read-only loader verification can proceed. This violates frozen C1 only on the real fixed-root integration path.

## Implementation boundary

```text
repository: C:\dev\kaizen\platform
starting HEAD: 0ebb3f74ed06c8bbe8b22ef9699f0abaf7cc90bf
branch: main
working tree: clean
remotes: none
```

No Go8, connector, lifecycle, canonical, live staging, dependency, or production-MCP changes are authorized.

## Required design

Do not globally weaken `enforce_promotion_scope`.

Use one explicit read-only mode or dedicated loader helper so that:

- fixed roots with omitted scope are allowed only for read-only prepared-candidate loading;
- operation binding and every evidence hash remain mandatory;
- supplied packet binding still resolves and verifies live scope;
- preparation, planning, approval, execution, and recovery still require verified live scope on fixed roots;
- disposable-root behavior remains unchanged;
- no broad public API expansion occurs.

A blanket `scope=None` allowance is forbidden.

## Required tests

1. Fixed roots plus omitted packet/live binding reach loader verification without `live_root_forbidden`.
2. Missing preparation returns `preparation_missing`.
3. Sound existing evidence verifies operation binding and every hash.
4. Supplied wrong packet returns `live_packet_mismatch`.
5. Tampered evidence retains frozen C1 errors.
6. Loader creates no mutex, temporary evidence, plan, approval, event, Git, canonical, or staging mutation.
7. Mutation-capable paths still reject fixed roots without verified live scope.
8. Adapter plus real platform boundary is tested without mocking away fixed-root enforcement.
9. The Packet 012E failing call is reproduced before correction and returns the expected structured loader result after correction.

Use disposable mirrors for tamper cases. Never alter live evidence.

## Allowed candidate paths

```text
src/kaizen/promotion_scope.py
src/kaizen/amendment_candidate.py
tests/test_amendment_candidate.py
tests/test_live_promotion.py
```

Kaizen MCP test-only path if strictly necessary:

```text
tests/test_amendment_adapters.py
```

## Verification

- focused live-scope and loader tests;
- adapter integration test if needed;
- full platform pytest suite;
- compileall;
- capability and text-integrity scans;
- exact changed-path and staged-diff verification;
- one local platform commit with no remote or push.

Restart and connector refresh occur only after correction completion acceptance and Packet 012E resume authorization.

## Acceptance criteria

The omitted optional binding works on fixed roots for the read-only loader only; frozen C1 errors remain deterministic; supplied live binding remains strict; no mutation path gains scope-free live-root access; no live evidence changes; all tests pass; completion audit passes; owner accepts exact correction evidence before Packet 012E resumes.

## Explicit non-scope

Broad live-scope redesign, connector/lifecycle work, Go8 changes, canonical mutation, live staging mutation, dependencies, production migration, cleanup, and Milestone 9.

## Owner implementation gate

Implementation must not begin until the owner approves the exact audited Packet 012E.1 SHA-256.
