# Kaizen Project Standard Revision Plan

Status: active
Date: 2026-06-04

## Purpose

Define how the original Kaizen Project Standard will be revised into v0.2 without silently turning research or implementation assumptions into doctrine.

## Current baseline

The original standard remains the baseline concept document. It established:

- the project-intelligence pipeline
- one command center per project
- earned folder structure
- source-of-truth separation from source and operations repos
- Hermes for bounded volume work
- frontier models for reasoning-heavy audits and architecture
- implementation-ready task packets as the primary pipeline output

## Accepted foundation decisions

The following decisions are now accepted inputs to v0.2:

- `04-design-decisions/0001-two-zone-agent-write-model.md`
- `04-design-decisions/0002-search-before-create-and-diff-before-write.md`
- `04-design-decisions/0003-raw-markdown-is-canonical.md`
- `04-design-decisions/0004-system-of-record-boundaries.md`
- `04-design-decisions/0005-api-only-structured-data-access.md`
- `04-design-decisions/0006-hammer-tests-are-a-hard-gate.md`
- `04-design-decisions/0007-foundation-resolution-for-v0.2.md`

## Active v0.2 planning specs

- `05-specs/kaizen-field-registry.md`
- `05-specs/kaizen-note-type-registry.md`
- `05-specs/postgres-observatory-authority.md`
- `05-specs/kaizen-validation-gate-spec.md`
- `05-specs/kaizen-hammer-test-strategy.md`
- `05-specs/staging-and-promotion-workflow.md`

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

## Remaining decisions before v0.2 draft

1. Final `pipeline_stage` enum.
2. Exact canonical project folder placement for the nine initial types.
3. Canonical vault root name/path convention independent of local Windows paths.
4. Promotion-log JSON Schema and event action vocabulary.
5. ULID generation command and machine-readable type-prefix registry.
6. Plugin install/defer policy in the rewritten standard.
7. Which `.obsidian` configuration files are intentionally version-controlled.
8. Whether source URLs remain after stable source IDs are assigned.
9. Initial private-data scanning and raw-data size thresholds.
10. Whether current-state notes are included in future Qdrant indexing.

## Revision method

1. Import the full original standard without reconstructing it from memory.
2. Reconcile it against accepted Decisions 0001-0007.
3. Resolve the remaining v0.2 decisions listed above.
4. Produce a complete Kaizen Project Standard v0.2 draft.
5. Audit v0.2 against the research summaries and Veda-derived governance patterns.
6. Correct contradictions and missing boundaries.
7. Accept v0.2 only after explicit human review.
8. Preserve the original standard in Git history or archive when superseded.

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

## Next recommended artifact

After importing the original baseline, create:

```text
01-project-standard/kaizen-project-standard-v0.2-draft.md
```

Do not overwrite or supersede the baseline placeholder until the v0.2 draft has passed audit and human approval.

## Related files

- `01-project-standard/kaizen-project-standard.md`
- `00-entrypoint/LLM_START_HERE.md`
- `03-research-results/`
- `04-design-decisions/`
- `05-specs/`
