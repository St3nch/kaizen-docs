# Kaizen Project Standard Revision Plan

Status: draft
Date: 2026-06-04

## Purpose

Define how the original Kaizen Project Standard will be revised without silently turning research and draft specs into accepted doctrine.

## Current baseline

The original standard remains the baseline concept document. It established:

- the project-intelligence pipeline
- one command center per project
- earned folder structure
- source-of-truth separation from source and operations repos
- Hermes for bounded volume work
- frontier models for reasoning-heavy audits and architecture
- implementation-ready task packets as the primary pipeline output

## Proposed revision inputs

The next standard revision should evaluate these proposed decisions:

- `04-design-decisions/0001-two-zone-agent-write-model.md`
- `04-design-decisions/0002-search-before-create-and-diff-before-write.md`
- `04-design-decisions/0003-raw-markdown-is-canonical.md`
- `04-design-decisions/0004-system-of-record-boundaries.md`
- `04-design-decisions/0005-api-only-structured-data-access.md`
- `04-design-decisions/0006-hammer-tests-are-a-hard-gate.md`

It should also evaluate these draft specs:

- `05-specs/kaizen-field-registry.md`
- `05-specs/kaizen-note-type-registry.md`
- `05-specs/postgres-observatory-authority.md`
- `05-specs/kaizen-validation-gate-spec.md`
- `05-specs/kaizen-hammer-test-strategy.md`

## Required decisions before v0.2

1. Final `status`, `review_status`, and `authority` lifecycle values and transitions.
2. Whether every note requires both a stable human ID and UUID.
3. Whether `domain` is universal or conditional.
4. Internal link convention.
5. Staging location and promotion flow.
6. Promotion event storage and audit evidence.
7. Initial note-type set.
8. Initial required field set by note type.
9. Plugin install/defer policy.
10. Whether proposed architecture decisions become accepted.

## Revision method

1. Review each proposed decision individually.
2. Accept, reject, or revise each decision explicitly.
3. Resolve conflicts between the original standard and accepted decisions.
4. Produce a complete Kaizen Project Standard v0.2 draft.
5. Audit v0.2 against research summaries and Veda-derived governance patterns.
6. Accept v0.2 only after human review.
7. Preserve the original standard in Git history or archive when superseded.

## Non-goal

Do not implement the vault, Postgres Observatory, Qdrant index, or Hermes write workflow as part of the standard revision itself.

The standard defines the governed plan. Implementation follows only after the plan is accepted.

## Related files

- `01-project-standard/kaizen-project-standard.md`
- `00-entrypoint/LLM_START_HERE.md`
- `03-research-results/`
- `04-design-decisions/`
- `05-specs/`
