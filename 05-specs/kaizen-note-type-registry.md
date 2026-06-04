# Spec - Kaizen Note Type Registry

Status: draft
Date: 2026-06-04

## Purpose

Define the candidate durable note types in Kaizen, their roles, and their authority/agent boundaries. Adding a new note type is a governed change, not an ad hoc convenience.

## Candidate registry

| Type | Purpose | Hermes role | Authority potential | Qdrant direction |
|---|---|---|---|---|
| `command-center` | curated project entrypoint | read only / propose edits | none | exclude |
| `overview` | durable project orientation | draft | low | index |
| `current-state` | human-readable project snapshot | draft | low | index selectively |
| `roadmap` | narrative now/next/later plan | draft | proposed | index |
| `raw-source` | pointer and provenance for external raw source | create pointer | none | metadata only/exclude body |
| `source-summary` | distilled source meaning | draft | evidence only | index |
| `claim` | discrete testable assertion | draft | accepted/doctrine after human review | index |
| `observatory-insight` | interpretation of structured result IDs | draft | never doctrine | index |
| `opportunity` | proposed action/bet from evidence | draft | proposed only | index |
| `decision` | governed choice with consequences | draft proposal only | accepted/doctrine after human review | index |
| `spec` | governed implementation definition | draft proposal only | accepted/implementation-ready by human | index |
| `audit` | review findings and verdict | draft findings only | human verdict | index |
| `task-packet` | agent-ready implementation handoff | draft from approved spec | human-approved readiness | index |
| `source-import-map` | source/repo migration mapping | update within rules | none | exclude |
| `domain-note` | stable governed domain knowledge | draft | doctrine after review | index |

## Required body structure direction

Each type should eventually define required H2 headings. Candidate examples:

### Claim

- `## Statement`
- `## Evidence`
- `## Confidence and caveats`
- `## Sources`
- `## Conflicts` when applicable

### Decision

- `## Context`
- `## Decision`
- `## Alternatives`
- `## Consequences`
- `## Evidence`
- `## Supersedence rationale` when applicable

### Spec

- `## Goal`
- `## Context`
- `## Scope`
- `## Non-goals`
- `## Requirements`
- `## Constraints`
- `## Acceptance criteria`
- `## Open questions`

### Audit

- `## Scope`
- `## Evidence reviewed`
- `## Findings`
- `## Contradictions and gaps`
- `## Recommendation`
- `## Human verdict`

### Task packet

- `## Objective`
- `## Read first`
- `## Context pack`
- `## Constraints`
- `## Output location`
- `## Acceptance criteria`
- `## Do not touch`

## Authority rules

- Hermes-created content begins staged and non-authoritative.
- Hermes cannot create a human verdict, approve a spec, or mark a task packet implementation-ready.
- Observatory insights cannot become doctrine directly.
- Claims, decisions, specs, and domain notes may carry higher authority only after explicit human review.
- Superseding an accepted record requires the same approval posture as the original.

## Raw-source boundary

A raw-source note is a pointer/provenance artifact, not a raw content warehouse.

Bulk crawled content, screenshots, datasets, transcripts, and large payloads live outside the vault. The note stores location, identity, capture metadata, and why the source matters.

## Registry change rule

Adding a note type requires:

1. purpose and ownership definition
2. required/optional fields
3. required body sections
4. authority and agent rules
5. validation updates
6. Qdrant indexing decision
7. review and decision record

## Open questions

- Whether `promotion-record` deserves its own type or should remain an audit/event concern.
- Whether `conflict` deserves its own type later.
- Whether current-state belongs in semantic retrieval.
- Exact boundaries between claim, domain-note, and decision authority.

## Related files

- `05-specs/kaizen-field-registry.md`
- `05-specs/kaizen-validation-gate-spec.md`
- `05-specs/kaizen-hammer-test-strategy.md`
