# Kaizen Project Standard Revision Plan

Status: active
Date: 2026-06-04

## Purpose

Define how the original Kaizen Project Standard will be revised into v0.2 without silently turning research or implementation assumptions into doctrine.

## Baseline status

The full original 591-line Kaizen Project Standard has now been imported exactly into:

```text
01-project-standard/kaizen-project-standard.md
```

Its SHA-256 at import was:

```text
2ddfa929d4c9c59bba1c823876cb8729024418ef1f1bc98c2bd9f00b6f14572c
```

The baseline remains historical source material. Accepted design decisions override conflicting baseline details.

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

## Proposed operating decision awaiting review

- `04-design-decisions/0008-v0.2-operating-conventions.md`

Decision 0008 proposes:

- final pipeline stages
- exact project folder placement
- portable root/path conventions
- plugin install/defer policy
- `.obsidian` version-control policy
- source URL retention
- initial raw/private-data validation thresholds
- future Qdrant treatment of current-state notes

It should remain proposed until human review and, ideally, Claude's independent document-contract report confirms no conflict.

## Active v0.2 planning specs

- `05-specs/kaizen-field-registry.md`
- `05-specs/kaizen-note-type-registry.md`
- `05-specs/kaizen-id-and-prefix-registry.md`
- `05-specs/postgres-observatory-authority.md`
- `05-specs/kaizen-validation-gate-spec.md`
- `05-specs/kaizen-hammer-test-strategy.md`
- `05-specs/staging-and-promotion-workflow.md`
- `05-specs/promotion-event-schema.md`

## Resolved foundation questions

The following are resolved for v0.2:

1. Lifecycle axes and enum values.
2. One immutable prefixed ULID note ID; no second UUID.
3. `project` required; `domain` optional; `subdomain` deferred.
4. Stable IDs in frontmatter relationships and relative Markdown links in bodies.
5. Sibling staging folder outside the canonical vault.
6. Append-only JSONL promotion events.
7. Nine initial note types.
8. Seven universal required frontmatter fields plus type-specific fields.
9. Durable agent provenance and transient `validation_status`.
10. Raw Markdown canonical, API-only structured-data access, and hammer gates.
11. Initial note/event prefix registry direction.
12. Initial promotion-event action vocabulary and event contract direction.

## Remaining research before v0.2 draft

One major research pass remains:

```text
source-summary, claim, decision, spec, audit, and task-packet document contracts
```

The prepared prompt is:

```text
02-research-prompts/002-document-contract-standards.md
```

Recommended workflow:

1. Claude clones/reads the repo and returns the report only.
2. GPT independently critiques the report against accepted Kaizen constraints.
3. Update the field registry, note-type registry, validation spec, and task-packet contract only where justified.

## Remaining human/design decisions

1. Accept, revise, or reject Decision 0008.
2. Finalize the `pipeline_stage` enum after one real-project dry run.
3. Confirm exact canonical folder placement after that dry run.
4. Approve the promotion-event action vocabulary and JSON Schema direction.
5. Choose implementation language/location for `kaizen-id` and validation tooling.
6. Define exact human actor-ID format.
7. Calibrate private/raw-data warning thresholds after sample-note testing.
8. Decide whether `operate` remains a pipeline stage.

## Revision method

1. Preserve the imported baseline unchanged as source material until v0.2 is accepted.
2. Use `baseline-v0.2-reconciliation-map.md` to map old sections to accepted replacements.
3. Complete the document-contract research.
4. Resolve Decision 0008 and remaining human choices.
5. Produce `01-project-standard/kaizen-project-standard-v0.2-draft.md`.
6. Audit the draft against research summaries, Veda governance patterns, and Decisions 0001-0008.
7. Correct contradictions, overengineering, and missing boundaries.
8. Accept v0.2 only after explicit human review.
9. Preserve the original baseline in Git history or archive when superseded.

## Non-goals

The v0.2 standard revision does not implement:

- the production Obsidian vault
- Qdrant
- the Postgres Observatory
- Hermes write access
- validation code
- promotion tooling
- source ingestion automation

The standard defines the governed plan. Implementation follows only after the plan is accepted and task-packeted.

## Next recommended actions

Tonight:

1. Review the imported baseline and reconciliation map.
2. Review Decision 0008 as a proposal, not accepted doctrine.
3. Commit and push the baseline/design batch.

Tomorrow:

1. Run `002-document-contract-standards.md` with Claude.
2. Paste the report back for independent critique.
3. Reconcile the document contracts.
4. Draft the complete v0.2 standard.

## Related files

- `01-project-standard/kaizen-project-standard.md`
- `01-project-standard/baseline-v0.2-reconciliation-map.md`
- `00-entrypoint/LLM_START_HERE.md`
- `02-research-prompts/002-document-contract-standards.md`
- `03-research-results/`
- `04-design-decisions/`
- `05-specs/`
