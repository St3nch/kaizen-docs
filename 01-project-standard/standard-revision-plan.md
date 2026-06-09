# Kaizen Project Standard Revision Plan

Status: v0.2 owner-acceptance gate active
Date: 2026-06-09

## Purpose

Define how the original Kaizen Project Standard will be revised into v0.2 without silently turning research or implementation assumptions into doctrine.

Project-wide execution is tracked in `IMPLEMENTATION_ROADMAP.md`. Milestone 4 has now supplied the governed-loop and implementation-return evidence required to begin a non-authoritative v0.2 draft. This plan authorizes consolidation work only; explicit owner acceptance is still required before v0.2 becomes authoritative.

## Current posture

The original serial revision sequence was superseded by the accepted acceleration amendment. Decisions 0008 through 0012, the implemented first slice, Result 062, and Result 063 now provide the evidence base for consolidation. The active work is to reconcile proven behavior, friction, compatibility requirements, and remaining assumptions into an auditable v0.2 draft.

## Baseline status

The full original 591-line Kaizen Project Standard is preserved exactly at:

```text
01-project-standard/kaizen-project-standard.md
```

Its SHA-256 at import was:

```text
2ddfa929d4c9c59bba1c823876cb8729024418ef1f1bc98c2bd9f00b6f14572c
```

The baseline remains historical source material. Accepted decisions and reviewed implementation evidence override conflicting baseline details.

The section-by-section reconciliation is documented in:

```text
01-project-standard/baseline-v0.2-reconciliation-map.md
```

## Accepted foundation decisions

The following decisions are accepted inputs to v0.2:

- `04-design-decisions/0001-two-zone-agent-write-model.md`
- `04-design-decisions/0002-search-before-create-and-diff-before-write.md`
- `04-design-decisions/0003-raw-markdown-is-canonical.md`
- `04-design-decisions/0004-system-of-record-boundaries.md`
- `04-design-decisions/0005-api-only-structured-data-access.md`
- `04-design-decisions/0006-hammer-tests-are-a-hard-gate.md`
- `04-design-decisions/0007-foundation-resolution-for-v0.2.md`
- `04-design-decisions/0008-v0.2-operating-conventions.md`
- `04-design-decisions/0009-operational-postgres-and-observatory-boundary.md`
- `04-design-decisions/0010-dedicated-internet-marketing-intelligence-database.md`
- `04-design-decisions/0011-progressive-hybrid-human-interface-direction.md`
- `04-design-decisions/0012-first-slice-contract-and-implementation-boundary.md`

Decision 0008 establishes:

- the six-stage project pipeline;
- exact project folder placement;
- portable root/path conventions;
- plugin install/defer policy;
- `.obsidian` version-control policy;
- source URL retention;
- initial raw/private-data validation posture;
- implementation-return requirements;
- future Qdrant treatment of current-state notes.

Decision 0008 was accepted on 2026-06-07 and proven through the Milestone 4 governed loop.

## Active v0.2 planning specs

- `05-specs/kaizen-field-registry.md`
- `05-specs/kaizen-note-type-registry.md`
- `05-specs/kaizen-id-and-prefix-registry.md`
- `05-specs/operational-postgres-authority.md`
- `05-specs/kaizen-validation-gate-spec.md`
- `05-specs/kaizen-hammer-test-strategy.md`
- `05-specs/staging-and-promotion-workflow.md`
- `05-specs/promotion-event-schema.md`
- `05-specs/staging-write-wrapper-and-promotion-recovery.md`

## Proven first-slice contracts

The following are resolved and proven for v0.2:

1. Lifecycle axes and enum values.
2. One immutable prefixed ULID note ID; no second UUID.
3. `project` required; `domain` optional; `subdomain` deferred.
4. Stable IDs in frontmatter relationships and relative Markdown links in bodies.
5. Sibling staging folder outside the canonical vault.
6. Append-only JSONL promotion and amendment events.
7. Nine initial note types.
8. Seven universal required frontmatter fields plus type-specific fields.
9. Durable agent provenance and transient `validation_status`.
10. Raw Markdown canonical, API-only structured-data access, and hammer gates.
11. ID and event-prefix registry direction.
12. Six-stage project pipeline: `capture`, `research`, `spec`, `audit`, `handoff`, `build`.
13. Exact canonical project folder placement and earned folders.
14. Immutable plan, validation, candidate, diff, and approval evidence.
15. Separate human approval for plan generation and execution.
16. Intent-before-mutation and exactly one successful terminal event.
17. Governed implementation return through task-packet and current-state amendments.
18. Git as supplementary evidence rather than the operational event record.
19. No plugin, database, retrieval layer, or live agent integration required for canonical operation.

## Evidence base

The consolidation must use at least:

- `03-research-results/006-document-contract-standards-reconciliation.md`
- `03-research-results/017-decision-0008-end-to-end-dry-run-simulation.md`
- `03-research-results/025-implementation-checkpoint-audit-steward-reconciliation.md`
- `03-research-results/031-packet-006b-final-implementation-steward-audit.md`
- `03-research-results/040-packet-008b-completion-steward-audit.md`
- `03-research-results/057-governed-amendment-implementation-security-steward-audit.md`
- `03-research-results/062-milestone-4-implementation-return-closure-audit.md`
- `03-research-results/063-milestone-5-first-slice-retrospective.md`
- `01-project-standard/kaizen-vision-and-architecture-alignment.md`

The baseline itself remains historical evidence, not current doctrine.

## Remaining human and design decisions

1. Accept, revise, or reject the consolidated v0.2 draft.
2. Define the durable human actor-ID and authentication model beyond the trusted `owner.local` first-slice convention.
3. Reconcile amendment event fields, recovery terminology, and backward compatibility into the accepted specs.
4. Decide the owner-approved backup and remote posture for platform and vault repositories.
5. Calibrate private/raw-data warning thresholds after broader sample-note testing.
6. Decide whether `operate` earns a pipeline stage after a real operational project.
7. Decide when connector mutation may be relied upon versus the local operator fallback.
8. Choose the next implementation milestone only after v0.2 acceptance.

## Required compatibility rules

1. Preserve all canonical note IDs and operation IDs.
2. Preserve existing JSONL event lines exactly.
3. Extend schemas backward-compatibly for existing `promote` events.
4. Keep first-time promotion behavior unchanged while documenting amendment behavior.
5. Do not mass-rewrite canonical notes solely for formatting consistency.
6. Preserve the current canonical vault and sibling staging layout.
7. Keep the vault without a remote until the owner makes the separate backup/visibility decision.
8. Preserve historical baseline and audit evidence in Git history.

## Revision method

1. Preserve the imported baseline unchanged as historical source material.
2. Use `baseline-v0.2-reconciliation-map.md` to map old sections to accepted replacements.
3. Use Result 063 as the retrospective change ledger.
4. Draft `01-project-standard/kaizen-project-standard-v0.2-draft.md`.
5. Reconcile amendment support, event vocabulary, validation layers, spec maturity, actor identity, and remote policy.
6. Audit the draft against research summaries, Veda governance patterns, Decisions 0001 through 0012, and the implementation evidence listed above.
7. Correct contradictions, overengineering, stale assumptions, and missing boundaries.
8. Accept v0.2 only after explicit owner review.
9. Preserve the original baseline in Git history when v0.2 is accepted.
10. Select and authorize the next implementation milestone only after v0.2 acceptance.

## Non-goals

The v0.2 consolidation does not implement new runtime systems. It does not implement:

- Qdrant;
- the Operational Postgres database or Observatory domain;
- the Internet Marketing Intelligence database;
- Hermes live integration;
- paid-provider ingestion;
- website automation;
- multi-agent orchestration;
- a general desktop application shell.

Existing validation, promotion, and amendment tooling is implementation evidence for the standard. New implementation follows only after the next roadmap and task packets are accepted.

## Active consolidation sequence

1. Complete the Milestone 5 retrospective and change ledger.
2. Update this revision plan and the reconciliation map with Milestone 4 evidence.
3. Draft the non-authoritative v0.2 standard.
4. Reconcile amendment support, event vocabulary, validation layers, spec maturity, actor identity, and remote posture through accepted Decision 0013.
5. Audit the reconciled v0.2 candidate against accepted decisions and implementation evidence.
6. Obtain explicit owner acceptance of candidate SHA-256 `e5749b3a228dc2aaaed634395ed81edd0626511d30ce44ea6f6a4f80684fc95e` before declaring v0.2 authoritative.
7. Choose the next implementation milestone family.
8. Create reviewed task packets before any new implementation begins.

## Related files

- `ROADMAP.md`
- `IMPLEMENTATION_ROADMAP.md`
- `01-project-standard/kaizen-project-standard.md`
- `01-project-standard/baseline-v0.2-reconciliation-map.md`
- `01-project-standard/kaizen-vision-and-architecture-alignment.md`
- `03-research-results/063-milestone-5-first-slice-retrospective.md`
- `04-design-decisions/`
- `05-specs/`
