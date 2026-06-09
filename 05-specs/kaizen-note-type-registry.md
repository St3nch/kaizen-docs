# Spec - Kaizen Note Type Registry

Status: implemented baseline
Date: 2026-06-04
Updated: 2026-06-09
Related decision: `04-design-decisions/0007-foundation-resolution-for-v0.2.md`
Related decision: `04-design-decisions/0013-v0.2-first-slice-contract-reconciliation.md`

## Purpose

Define the initial durable note types for Kaizen v0.2, their required metadata and body structure, and their authority and agent boundaries.

Adding a new note type is a governed change, not an ad hoc convenience.

## Initial v0.2 registry

| Type | Prefix | Purpose | Hermes role | Review/authority | Qdrant direction |
|---|---|---|---|---|---|
| `command-center` | `cc` | curated project entrypoint and LLM orientation | read; propose edits only | review not required; no authority | exclude |
| `overview` | `ov` | durable project purpose, scope, and orientation | draft | review not required unless promoted as governing guidance | index |
| `current-state` | `cs` | human-readable snapshot of project position, blockers, and next move | draft | review not required by default | index selectively |
| `source-summary` | `ss` | evidence and interpretation from one independently governed source unit | draft | pending review; no authority | index |
| `claim` | `clm` | discrete, testable assertion supported by sources | draft | pending review; proposed or accepted authority | index |
| `decision` | `dec` | explicit governing choice with alternatives and consequences | draft proposal only | pending review; proposed or accepted authority | index |
| `spec` | `spec` | implementation definition and acceptance boundaries | draft proposal only | pending review; proposed or accepted authority | index |
| `audit` | `aud` | structured review of a claim, decision, spec, or task packet | draft evidence/findings only; never human verdict | pending review; authority remains none | index |
| `task-packet` | `tp` | implementation-ready handoff to a coding agent | draft from approved spec | pending review; proposed or accepted authority | index |

## Universal fields

Every type requires the seven universal fields defined in `05-specs/kaizen-field-registry.md`:

```yaml
id:
type:
status:
project:
summary:
created:
updated:
```

## Type-specific field requirements

### `command-center`

Required:

```yaml
pipeline_stage:
review_status: not-required
```

Authority is omitted or `none`.

### `overview`

Required:

```yaml
review_status: not-required
```

`pipeline_stage` is optional.

### `current-state`

Required:

```yaml
pipeline_stage:
review_status: not-required
```

This note is a narrative planning snapshot. It must not become a substitute for live jobs, runs, measurements, queues, or automation state in Postgres.

### `source-summary`

Required:

```yaml
review_status: pending | approved | rejected
authority: none
```

And at least one of:

```yaml
source_docs:
source_urls:
```

### `claim`

Required:

```yaml
review_status: pending | approved | rejected
authority: none | proposed | accepted
confidence: low | medium | high
source_docs:
```

`source_urls` may temporarily supplement `source_docs` before a source registry exists, but accepted claims require stable source references.

### `decision`

Required:

```yaml
review_status: pending | approved | rejected
authority: proposed | accepted
```

### `spec`

Required:

```yaml
review_status: pending | approved | rejected
authority: proposed | accepted
related_decisions:
```

A spec cannot become accepted if its governing decisions are not approved and accepted.

### `audit`

Required:

```yaml
review_status: pending | approved | rejected
```

And at least one of:

```yaml
related_claims:
related_decisions:
related_specs:
```

The human verdict is recorded in the body and promotion event. Hermes may draft findings and recommendations but not the verdict.

### `task-packet`

Required:

```yaml
review_status: pending | approved | rejected
authority: proposed | accepted
related_specs:
```

A task packet is implementation-ready only when:

- `status: active`
- `review_status: approved`
- `authority: accepted`
- every related spec is approved and accepted
- required body sections pass validation
- a promotion event exists

## Required body sections

### `command-center`

- `## LLM entry point`
- `## Current stage`
- `## Read first`
- `## Current blockers`
- `## Next action`
- `## Source-of-truth boundaries`

### `overview`

- `## Purpose`
- `## Scope`
- `## Non-goals`
- `## Current posture`

### `current-state`

- `## Current stage`
- `## What is true now`
- `## Blockers`
- `## Next move`
- `## Operational-state boundary`

### `source-summary`

Required:

- `## Summary`
- `## Extracted Evidence`
- `## Interpretation`
- `## Provenance`
- `## Caveats`

Conditional:

- `## Project Relevance` when the source's current project use is not obvious from the summary

A source summary covers one independently governed source unit. Evidence items identify their type and a precise locator when available. Interpretations must not be presented as extracted evidence.

### `claim`

- `## Statement`
- `## Supporting Evidence`
- `## Conflicting Evidence`
- `## Confidence and Caveats`

A claim contains one core evidence-checkable assertion. `## Conflicting Evidence` remains present even when no conflicting evidence is currently known. Confidence must include a rationale and must not be calculated from source count alone.

### `decision`

- `## Context`
- `## Decision`
- `## Alternatives`
- `## Consequences`
- `## Evidence`
- `## Supersedence rationale` when applicable

### `spec`

Required:

- `## Goal`
- `## Scope`
- `## Non-Goals`
- `## Requirements`
- `## Constraints`
- `## Acceptance Criteria`
- `## Open Questions`

Conditional when applicable:

- `## Context`
- `## Interfaces and Data`
- `## Failure Behavior`
- `## Migrations`
- `## Test Strategy`
- `## Dependencies`
- `## Examples`

A spec must not introduce hidden governing decisions. Accepted authority does not by itself make a spec task-packet-ready. Unresolved correctness questions block readiness.

### `audit`

- `## Scope`
- `## Evidence Reviewed`
- `## Findings`
- `## Contradictions and Gaps`
- `## Recommendation`
- `## Human Verdict`

Finding severities are `blocker`, `major`, `minor`, and `info`.

Human verdicts are `pass`, `pass-with-notes`, `fail`, and `stale`. `pass-with-notes` may contain only non-blocking observations. Any unmet condition required before promotion means `fail`.

### `task-packet`

Required:

- `## Objective`
- `## Read First`
- `## Scope`
- `## Non-Scope`
- `## Implementation Requirements`
- `## Constraints`
- `## Validation`
- `## Acceptance Criteria`
- `## Completion Report`

Conditional when applicable:

- `## Context Pack`
- `## Allowed Changes`
- `## Do Not Touch`
- `## Output Location`
- `## Interfaces and Data`
- `## Migrations`
- `## Documentation Updates`
- `## Rollback and Recovery`
- `## Dependencies and Blockers`

A task packet identifies exactly one primary governing spec. Additional related specs may provide supporting constraints. Conditional sections become required when the implementation touches their concern.

## Authority and review rules

- Hermes-created content starts with `status: draft`.
- Hermes may set `review_status: not-required` only where the registry explicitly permits it.
- Hermes may set `review_status: pending` but never `approved` or `rejected`.
- Hermes may set `authority: none` or `proposed` where the type permits it.
- Hermes may never set `authority: accepted`.
- An active note is usable at its current authority level; active does not imply approved.
- Superseding an accepted claim, decision, or spec requires the same human approval posture as the record being superseded.

## Deferred note types

These remain deferred until concrete workflow friction proves need:

| Type | Reason deferred |
|---|---|
| `roadmap` | current-state can carry now/next/later until a separate roadmap earns itself |
| `raw-source` | no governed ingestion system exists yet |
| `source-import-map` | add when the first real migration/import workflow exists |
| `observatory-insight` | Operational Postgres database and Observatory domain do not exist yet |
| `opportunity` | can initially be represented through claims and proposed decisions |
| `domain-note` | cross-project domain governance has not yet been proven |
| `promotion-record` | promotion events use append-only JSONL |
| `conflict` | use `conflict_with` plus claim/audit body sections until a separate type is justified |

## Registry change rule

Adding a note type requires:

1. purpose and ownership definition
2. stable type prefix
3. required and optional fields
4. required body sections
5. lifecycle, review, authority, and agent rules
6. validation updates
7. Qdrant indexing decision
8. an explicit accepted decision

## Open questions

- Final `pipeline_stage` enum.
- Exact machine representation for the independently governed source unit and its reviewed version.
- Whether audits need a structured verdict field later.
- When `raw-source` becomes necessary.
- Whether current-state notes should be indexed once Qdrant exists.

## Implemented-baseline evidence

This contract was exercised by the Milestones 1 through 4 platform and canonical-vault work, including deterministic validation, six ordered promotions, two governed amendments, hammer tests, and Result 062 closure. Remaining open questions are future refinements rather than blockers to the implemented first-slice baseline.

## Related files

- `04-design-decisions/0007-foundation-resolution-for-v0.2.md`
- `05-specs/kaizen-field-registry.md`
- `05-specs/kaizen-validation-gate-spec.md`
- `05-specs/staging-and-promotion-workflow.md`
