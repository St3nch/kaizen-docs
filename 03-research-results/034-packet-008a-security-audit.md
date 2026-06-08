---
id: kz-aud-01KTMEKKXHKKT3JZR978Z3BC7D
type: audit
status: complete
project: kaizen-platform
summary: Security and proportionality audit of Packet 008A for a fixed-root human-only live promotion operator.
created: 2026-06-08T20:30:00Z
updated: 2026-06-08T20:30:00Z
review_status: approved
related_specs:
  - 05-specs/staging-write-wrapper-and-promotion-recovery.md
  - 05-specs/staging-and-promotion-workflow.md
  - 05-specs/promotion-event-schema.md
  - 05-specs/kaizen-hammer-test-strategy.md
---

# Research Result 034 - Packet 008A Security Audit

## Scope

Audit:

```text
06-handoff-patterns/008a-implement-owner-controlled-live-promotion-operator.md
```

The audit determines whether Packet 008A is sufficiently narrow and fail-closed to present for owner approval. It does not approve implementation or any live promotion action.

## Executive Verdict

**Pass for owner review.**

The discovered blocker is real: Packet 006B deliberately rejects the exact live roots, and the existing CLI is pinned to a disposable sandbox. A reviewed implementation gate is required before first-real-promotion planning can be honest.

Packet 008A keeps implementation enablement separate from live execution. That split is necessary and proportionate.

## Positive Security Findings

### Existing protections remain the default

The packet forbids weakening `kaizen-promote` or replacing the live-root refusal with a generic boolean. Default core calls remain disposable-only.

### Separate fixed-root entrypoint

`kaizen-promote-live` has no root arguments or environment override. This prevents a narrowly reviewed live command from becoming a general arbitrary-repository mutation tool.

### Live authority is explicit

A typed, verified live capability makes the transition from disposable to live behavior visible in code and tests. This is an accidental-misuse boundary inside a local process, not strong authentication, and the packet requires that limitation to be documented honestly.

### Plans bind environment state

The packet requires packet ID, exact roots, vault branch/head, platform head, and governance-log hash under the immutable plan hash. Approval therefore cannot silently float onto a different repository state.

### Initial execution and recovery are separated

A clean vault is required before intent. Recovery instead accepts only dirty paths explained by the approved operation. This avoids making recovery impossible while still rejecting unrelated dirty state.

### No Git mutation

The operator reads Git state but cannot stage, commit, push, reset, clean, restore, or stash. Human Git review remains outside the promotion engine.

### Human confirmation remains layered

Live approval requires an explicit basis, and execution requires an exact confirmation literal plus human actor and matching packet ID. These are accidental-invocation controls layered under the separate owner-approved Packet 008B gate.

### Candidate is held

The source-summary candidate is hash-bound and explicitly out of scope. Packet 008A cannot use its existence as implied permission to plan or promote it.

## Threat Review

### Generic live bypass

**Blocked by packet design.** No `allow_live=True`, caller root, environment override, or sandbox CLI flag is permitted.

### Stale approval after repository drift

**Blocked.** Live plan binding covers vault and platform heads, roots, packet ID, and log hash. Approve and execute verify the binding rather than refreshing it.

### Accidental CLI invocation

**Reduced.** A separate executable, fixed roots, packet ID, human actor, explicit basis, and confirmation literal make accidental execution materially harder. These controls do not replace human approval.

### Recovery over unrelated changes

**Blocked.** Recovery must classify operation-owned dirty paths and reject unrelated Git state without cleanup.

### Hidden Git automation

**Blocked.** The packet prohibits every mutating Git operation. Commit and push remain explicit later steps.

### Live-test leakage

**Blocked.** Tests must use disposable mirrors and must prove the real candidate, staging log, governance log, and vault remain unchanged.

### Parent-directory scope creep

**Blocked.** The live operator does not create destination parents. Packet 008B must authorize the one exact parent needed for the chosen candidate.

## Proportionality Assessment

A one-off script that bypasses `ensure_disposable_roots` would defeat the safety boundary proven by Packet 006B. Adding live behavior to the existing sandbox CLI would make accidental invocation easier. A separate fixed-root operator with immutable Git binding is the smallest durable implementation that preserves existing defaults and can support future human promotions.

The packet adds no network services, databases, agent orchestration, generalized repository tooling, or live mutation.

## Required Owner Understanding

Approval of Packet 008A means only:

```text
implement and test the fixed-root live operator
commit the platform implementation
prove the live vault and candidate were untouched
```

It does not approve:

```text
live plan generation
human approval evidence for the candidate
creation of the live research directory
promotion execution or recovery
promotion events
vault commit or push
```

## Residual Risks

- Python capability objects are not a hostile-process security boundary.
- Human identity remains a local actor string until a later authenticated operator layer exists.
- Git clean/head checks protect reviewed state but do not provide storage-device power-loss guarantees.
- The live operator increases the consequence of local shell access; Windows account and filesystem permissions remain part of the trust boundary.

These are acceptable for a local human-only first-slice operator and must be documented rather than obscured.

## Steward Verdict

**security-audited pass; explicit owner approval required before implementation**

## Next Gate After Successful Implementation

Draft and security-audit Packet 008B using the held source-summary. Packet 008B must generate and present the exact immutable plan and reviewed diff, authorize creation of only the required `research` parent, require explicit owner approval, execute one promotion, verify events and hashes, and commit the resulting vault diff locally.
