---
id: kz-tp-01KTMMKZPEY5R0C7NPBAMVNC3W
type: task-packet
status: complete
project: kaizen-platform
summary: Generate and validate the six-note Milestone 4 planning bundle for governed amendment support without canonical promotion.
created: 2026-06-08T22:10:00Z
updated: 2026-06-08T22:23:58Z
review_status: approved
authority: accepted
primary_spec: 05-specs/staging-and-promotion-workflow.md
related_specs:
  - 05-specs/staging-and-promotion-workflow.md
  - 05-specs/promotion-event-schema.md
  - 05-specs/kaizen-note-type-registry.md
  - 05-specs/kaizen-field-registry.md
---

# Task Packet 009A - Generate Milestone 4 Governed Planning Bundle

> Completion status: owner-approved and complete. Result 042 verifies all six staged notes, twelve staging events, zero validation errors/warnings, and full relationship resolution. Canonical promotion and implementation remain separately gated.

## Objective

Create one linked six-note planning bundle in governed staging that proves the evidence-to-handoff half of Milestone 4 and defines the smallest implementation slice needed to complete the implementation-return leg: human-operated governed amendment support.

## Read First

- `IMPLEMENTATION_ROADMAP.md`
- `04-design-decisions/0008-v0.2-operating-conventions.md`
- `05-specs/kaizen-note-type-registry.md`
- `05-specs/kaizen-field-registry.md`
- `05-specs/staging-and-promotion-workflow.md`
- `05-specs/promotion-event-schema.md`
- `03-research-results/040-packet-008b-completion-steward-audit.md`
- canonical `projects/kaizen-platform/command-center.md`
- canonical `projects/kaizen-platform/current-state.md`

## Bound State

```text
docs commit: d46a99af54398643877509c1e74329e02dd455b4
platform commit: 1a890dd80d022e711f385525c202264b95d5faba
vault commit: 80bc0932c505770a57dd8790f105cca1915f9a23
staging-write log sha256: 07b09195bd04852fd6d2f12dc6a3c878b85f964eec2cff850b496352bd4b8a52
```

All three repositories must be clean before execution. Do not reset, stash, restore, clean, or discard unexpected work.

## Reserved Artifact IDs and Paths

```text
source-summary
id: kz-ss-01KTMMKZPEY5R0C7NPBAMVNC3Y
path: projects/kaizen-platform/source-summaries/milestone-4-amendment-gap.md

claim
id: kz-clm-01KTMMKZPEY5R0C7NPBAMVNC3Z
path: projects/kaizen-platform/claims/governed-amendment-required.md

decision
id: kz-dec-01KTMMKZPF5PKS31DR8NP8PCQQ
path: projects/kaizen-platform/decisions/governed-amendment-slice.md

spec
id: kz-spec-01KTMMKZPF5PKS31DR8NP8PCQR
path: projects/kaizen-platform/specs/governed-amendment-support.md

audit
id: kz-aud-01KTMMKZPF5PKS31DR8NP8PCQS
path: projects/kaizen-platform/audits/governed-amendment-support-audit.md

task-packet
id: kz-tp-01KTMMKZPF5PKS31DR8NP8PCQT
path: projects/kaizen-platform/handoffs/implement-governed-amendment-support.md
```

No replacement IDs or alternate paths are authorized if a precondition fails.

## Scope

Generate the six notes above through `kaizen-stage-create --request <json>` using one request file per note.

Each request must bind:

- exact logical path;
- exact UTF-8 note content;
- note type and project;
- agent/model/session provenance;
- unique request ID;
- search-before-create evidence ID;
- staging validation mode;
- exact expected content SHA-256.

The six notes must form this stable-ID chain:

```text
source-summary
  -> claim.source_docs
  -> decision evidence/body reference
  -> spec.related_decisions
  -> audit.related_decisions + audit.related_specs
  -> task-packet.primary_spec + task-packet.related_specs
```

## Required Note Content

### Source summary

Must record verified evidence that:

- Decision 0008 requires completion reports and current-state changes to return through governed amendments when staged/promotion-controlled;
- the promotion event schema defines `action: amend` and requires prior content hash, new content hash, and change summary;
- the platform currently contains no amendment implementation or tests;
- canonical command-center, overview, and current-state notes are stale relative to Packet 008B completion;
- Packet 008B proved first-time promotion only.

It must distinguish extracted evidence from interpretation and retain precise repository/file locators.

### Claim

One core assertion only:

> Governed amendment support is the smallest missing platform capability required to complete Milestone 4's implementation-return leg for a staged task packet and existing `current-state.md`.

Use confidence `high`, source the source-summary ID, and include conflicting evidence even when none is known.

### Decision

Propose this bounded choice:

- implement human-operated first-amendment support before any Hermes, Postgres, Qdrant, UI, provider, or website work;
- preserve first-time promotion defaults;
- exclude supersedence, correction, rollback, deletion, generalized overwrite, and agent-triggered amendment;
- require separate owner approval before any live amendment.

### Spec

Define implementation-ready requirements for:

- immutable amendment planning and approval;
- binding prior canonical path/ID/hash and candidate hash;
- `action: amend` intent and terminal events;
- exact reviewed diff and change summary;
- stale canonical/source/candidate/log/Git refusal;
- same-path replacement only after approved prior-byte verification;
- interruption recovery and no silent overwrite;
- fixed-root human-only live command posture;
- disposable Git-mirror hammers;
- no live amendment during implementation packet execution.

### Audit

Audit the proposed decision and spec for safety, completeness, proportionality, contract alignment, and Milestone 4 suitability. Human verdict in the staged draft remains a recommendation only; do not set accepted authority.

### Task packet

Create an implementation-ready proposed packet that:

- names the spec as `primary_spec` and in `related_specs`;
- authorizes platform implementation and disposable tests only;
- preserves all first-time promotion behavior;
- requires completion evidence but does not authorize populating its own completion report yet;
- explicitly defers live amendment to a later packet;
- contains every required task-packet body section, including an initially unpopulated `## Completion Report

```text
result: pass
docs baseline: 2824353d8ac2d3bf4566ab35d67e77421ab32286
platform baseline: 1a890dd80d022e711f385525c202264b95d5faba
vault baseline: 80bc0932c505770a57dd8790f105cca1915f9a23
created notes: 6
staging events: 12 (6 intent, 6 committed)
validation errors: 0
validation warnings: 0
relationship checks: all resolved
staging log after: 3419bc43e1588c7e97d44626bb3e9e263cd3d109647f79c8bda150c422905059
live vault/platform/docs mutation during execution: none
```

Exact note, request, event, and validation evidence is recorded in Result 042. The temporary packet-owned request files may be removed after this completion record is committed. The next gate is a separate ordered promotion packet; no promotion or implementation authority is implied by Packet 009A completion.
