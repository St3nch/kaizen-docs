---
id: kz-tp-01KTMMKZPEY5R0C7NPBAMVNC3W
type: task-packet
status: active
project: kaizen-platform
summary: Generate and validate the six-note Milestone 4 planning bundle for governed amendment support without canonical promotion.
created: 2026-06-08T22:10:00Z
updated: 2026-06-08T22:10:00Z
review_status: pending
authority: proposed
primary_spec: 05-specs/staging-and-promotion-workflow.md
related_specs:
  - 05-specs/staging-and-promotion-workflow.md
  - 05-specs/promotion-event-schema.md
  - 05-specs/kaizen-note-type-registry.md
  - 05-specs/kaizen-field-registry.md
---

# Task Packet 009A - Generate Milestone 4 Governed Planning Bundle

> Security status: pending Result 041. This packet authorizes create-only staging writes and validation only. It does not authorize canonical promotion, accepted authority, platform implementation, amendment execution, or canonical note updates.

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
- contains every required task-packet body section, including an initially unpopulated `## Completion Report` that states implementation evidence is pending.

## Non-Scope

Packet 009A does not authorize:

- canonical promotion of any of the six notes;
- changing `review_status` to `approved`;
- changing authority to `accepted`;
- platform source-code changes;
- live or disposable amendment execution;
- edits to canonical command-center, overview, current-state, or promoted task packets;
- creation of promotion plans or approval evidence;
- append to the live promotion log;
- Hermes, LangGraph, Postgres, Qdrant, providers, UI, website, or deployment work.

## Implementation Requirements

1. Preflight the bound repository commits and clean states.
2. Confirm all six target paths are absent.
3. Confirm reserved IDs do not exist anywhere in canonical or staging Markdown.
4. Build six request JSON files in a packet-owned temporary directory outside canonical and staging note destinations.
5. Compute and record each content SHA-256 before invoking the writer.
6. Invoke `kaizen-stage-create` exactly once per note in dependency order.
7. Stop immediately on the first nonzero result.
8. Preserve created notes and attempt-log evidence on failure; do not delete or regenerate automatically.
9. Re-run staging validation on every created note after all six exist.
10. Verify stable-ID relationships and required body sections.
11. Record request IDs, intent/terminal staging events, file hashes, validation run IDs, and final attempt-log hash.
12. Remove packet-owned request JSON files only after their contents and hashes are recorded in the completion report.

## Constraints

- Agent provenance:

  ```text
  agent: kaizen-steward
  model: gpt-5.5-thinking
  session: packet-009a
  ```

- Staged notes use `status: draft` and `validation_status: pending` before the writer validates them.
- Governed notes use `review_status: pending`.
- Source summary and audit use `authority: none`.
- Claim, decision, spec, and task packet use at most `authority: proposed`.
- No note may claim human approval or accepted authority.
- Request IDs must be unique and begin `packet-009a-`.
- Search evidence IDs must be non-empty and begin `packet-009a-search-`.
- The create-only wrapper must remain the only writer for note files.

## Validation

Packet 009A passes only if:

- all six writes return `created` or valid idempotent success for the exact same request/content;
- all six files exist at the exact paths;
- each file hash matches its request;
- staging validation passes with zero errors;
- required body sections pass;
- all stable-ID relationships resolve within the six-note bundle or existing canonical vault;
- no duplicate IDs, summaries, or initial destinations exist;
- the live vault and promotion log are unchanged;
- platform and docs repositories remain unchanged;
- no promotion operation directory is created for the six notes.

## Acceptance Criteria

- The six-note bundle exists in staging with exact reserved IDs and paths.
- The evidence chain is internally consistent and validates.
- The proposed task packet is implementation-ready after later human review/promotion.
- No canonical mutation, authority transition, platform change, or live event occurs.
- A completion audit presents exact hashes and recommends whether to proceed to the separate promotion gate.

## Completion Report

Pending execution. The report must later record:

```text
result
preflight heads and hashes
request-file hashes
request IDs
created paths and note IDs
content hashes
staging intent/terminal event IDs
validation run IDs and counts
relationship-resolution results
final staging-write log hash
vault/platform/docs preservation checks
deviations and failures
promotion-gate recommendation
```
