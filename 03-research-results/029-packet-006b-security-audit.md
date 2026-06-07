---
id: kz-aud-01KTHV1A4B5C6D7E8F9GHJKMNP
type: audit
status: complete
project: kaizen-platform
summary: Security and contract audit of Packet 006B for a human-operated first-time promotion workflow.
created: 2026-06-07T20:12:00Z
updated: 2026-06-07T20:12:00Z
review_status: approved
related_specs:
  - 05-specs/staging-write-wrapper-and-promotion-recovery.md
  - 05-specs/staging-and-promotion-workflow.md
  - 05-specs/promotion-event-schema.md
  - 05-specs/kaizen-validation-gate-spec.md
---

# Research Result 029 - Packet 006B Security Audit

## Scope

Audit:

```text
06-handoff-patterns/006b-implement-human-operated-first-promotion.md
```

The audit determines whether Packet 006B is sufficiently narrow, explicit, testable, and fail-closed to present for owner approval. It does not approve implementation or live canonical promotion on the owner's behalf.

## Governing verdict

The packet is materially narrower than the retired combined Packet 006.

It implements only a first-time `promote` action in disposable roots. It does not authorize an existing-destination update, amendment, supersedence, correction, automatic rollback, governance bootstrap, agent trigger, live promotion, or broad filesystem surface.

## Result 025 findings

### F-09 - canonical candidate hash

**Satisfied in packet contract.**

`plan.json` and `approval.json` both require `canonical_candidate_sha256`, distinct from `source_sha256`. Execution recomputes both before mutation.

### F-10 - required packet sections

**Satisfied.**

Packet 006B contains explicit implementation requirements, constraints, validation, acceptance criteria, stop conditions, rollback/recovery, and completion-report sections.

### F-11 - editorial integrity

**Satisfied for draft.**

Test numbering is unique and sequential. Required terminology distinguishes source bytes, canonical-candidate bytes, event phases, and business action.

### F-13 - immutable plan storage

**Satisfied in packet contract.**

The exact staging-local operation directory and write order are defined. `plan.json` is written last as the ready marker and is immutable. Approval is a separate create-new immutable record. Hashing, invalidation, and retry rules are explicit.

### F-14 - governance bootstrap

**Preserved as separate Gate C work.**

The packet forbids implementation from creating `_governance`. Execution must fail as `governance_not_bootstrapped` until an owner-approved, Git-committed bootstrap exists.

### F-15 - promotion event IDs

**Satisfied in packet contract.**

Every event and the operation ID use `kz-prom-<ULID>`. Later terminal records use new event IDs while copying the intent ID as `operation_id`.

### F-18 - normalized event paths

**Satisfied in packet contract.**

Every stored canonical destination is vault-relative with forward slashes. Source paths remain local staging paths and are never portable canonical identity.

### F-19 - schema version

**Satisfied in packet contract.**

Plan, validation, approval, and promotion-event records require `schema_version: 1.0`. Packet implementation must create `schemas/promotion-event.schema.json`.

### F-27 - Windows reparse constant

**Satisfied in packet contract.**

Implementation must assert that `stat.FILE_ATTRIBUTE_REPARSE_POINT` exists and is nonzero on the supported runtime. Silent zero fallback is forbidden.

## Positive security findings

### Approval is hash-bound and human-only

The approval record repeats and binds:

```text
plan hash
source hash
canonical-candidate hash
destination
validation run
reviewed diff
human actor
timestamp
```

The CLI explicitly forbids auto-approval and agent-supplied approval paths.

### Plan readiness is unambiguous

`plan.json` is written last. Its existence means candidate and validation evidence are complete and immutable. Incomplete-planning cleanup is allowed only before readiness and only for operation-owned files.

### Governance bootstrap is not hidden

The packet preserves `_governance` creation as a separate owner-approved canonical-maintenance change. Promotion implementation cannot invent its own audit structure during execution.

### Intent precedes canonical mutation

The executor must append and flush intent before creating the canonical temporary file. Every canonical mutation therefore has durable pre-write evidence.

### Packet 006A is used without broadening

Canonical installation remains absent-to-present, same-parent, no-replacement, hash-verified, and internal. No overwrite option is introduced.

### Recovery is evidence-driven

The recovery table distinguishes absent, temp-only, canonical-matching, canonical-conflicting, and inconsistent-terminal states. Recovery appends events and never rewrites prior history.

### Staging retention avoids hidden destructive cleanup

The first slice retains the staged source. Duplicate promotion is prevented by canonical and event checks rather than automatic source deletion.

### Mutating surfaces are enumerated

Execution/recovery may mutate only:

```text
_governance/promotion-log.jsonl
one operation-owned canonical temporary file
one absent approved canonical destination
```

Planning and approval mutations are separately bounded under the operation-evidence directory.

## Required clarifications captured by the packet

- current validator output lacks validation-run IDs, so 006B must implement durable promotion-validation evidence;
- candidate normalization is deterministic and allowlisted, with a human-reviewed diff;
- source and canonical-candidate hashes are distinct approval bindings;
- plan, validation, approval, and event canonical-JSON hashing rules are required;
- first-slice action is `promote` only;
- automatic canonical rollback is not authorized;
- terminal-event failure after canonical installation is handled by recovery, not silent success or automatic overwrite;
- no test may target live roots.

## Remaining owner gates

Even after Packet 006B implementation passes, these remain separate:

1. owner-approved `_governance` bootstrap;
2. authoring of a clean first-promotion staged artifact;
3. a separate reviewed first-real-promotion execution packet or explicit go/no-go;
4. review of live Git and event evidence after execution.

Packet 006B approval must not be interpreted as approval of any of those actions.

## Risks intentionally left to implementation proof

- exact canonical JSON representation and package-version string;
- event append handle and sharing semantics;
- mutex interaction with recovery and competing operations;
- operation-evidence cleanup before readiness;
- relationship and link resolution across disposable canonical roots;
- terminal append failure injection and post-install recovery;
- antivirus-style file-lock behavior;
- source-symlink coverage where the workstation permits it;
- physical cross-volume integration when a second disposable volume exists.

The packet's stop conditions require a blocked result rather than weakening these boundaries.

## Steward verdict

**pass for owner approval**

Packet 006B is narrow enough to present for explicit owner approval. It authorizes no implementation until approved and no live canonical promotion under any interpretation.

## Next action

Present Packet 006B to the owner for approval. After approval, implementation remains confined to disposable roots and must stop if hash binding, event integrity, intent-before-mutation, recovery, or no-replacement guarantees cannot be proved.
