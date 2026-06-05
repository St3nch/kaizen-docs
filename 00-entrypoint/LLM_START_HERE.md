# LLM Start Here

This is the first file every LLM should read when working in the `kaizen-docs` repository.

## What this repo is

`kaizen-docs` is the planning and stewardship repository for Kaizen.

Kaizen is a portable Markdown-first project intelligence system designed to move projects through this flow:

```text
idea capture -> research -> source summaries -> claims -> specs -> audit -> task packet -> coding agent build
```

This repository is **not** the future Obsidian vault. It is the design workspace used to research, plan, govern, and audit that future system.

## Accepted architecture

```text
Markdown / Obsidian = canonical project intelligence
Postgres Observatory = operational source of truth for observations, jobs, runs, costs, and logs
Qdrant = rebuildable semantic retrieval index over approved Markdown
Hermes / agents = constrained clerks through approved typed tools
Humans = authority-bearing review, approval, and promotion
```

## Current project posture

- Current phase: Kaizen Project Standard v0.2 planning
- Future canonical vault: not created yet
- Sibling staging root: design accepted, not created for production use yet
- Postgres Observatory: not implemented
- Qdrant index: not implemented
- Hermes Desktop / Hermes Agent: installed by user, not approved for Kaizen write access
- Decisions 0001 through 0007: accepted
- Field, note-type, validation, hammer, Observatory, and promotion specs: active drafts
- Primary immediate need: import the original baseline standard, resolve the remaining v0.2 questions, and draft the complete v0.2 standard
- Stewardship principle: structure and automation must earn their existence

## Read-first sequence

Read in this order unless the user gives a narrower task:

1. `00-entrypoint/LLM_START_HERE.md` - this file
2. `01-project-standard/kaizen-project-standard.md` - baseline import placeholder
3. `01-project-standard/standard-revision-plan.md` - current v0.2 revision path
4. `04-design-decisions/0007-foundation-resolution-for-v0.2.md`
5. `04-design-decisions/0001-two-zone-agent-write-model.md`
6. `04-design-decisions/0002-search-before-create-and-diff-before-write.md`
7. `04-design-decisions/0003-raw-markdown-is-canonical.md`
8. `04-design-decisions/0004-system-of-record-boundaries.md`
9. `04-design-decisions/0005-api-only-structured-data-access.md`
10. `04-design-decisions/0006-hammer-tests-are-a-hard-gate.md`
11. `05-specs/kaizen-field-registry.md`
12. `05-specs/kaizen-note-type-registry.md`
13. `05-specs/staging-and-promotion-workflow.md`
14. `05-specs/kaizen-validation-gate-spec.md`
15. `05-specs/postgres-observatory-authority.md`
16. `05-specs/kaizen-hammer-test-strategy.md`
17. `07-hermes/hermes-permission-matrix.md`
18. `07-hermes/hermes-write-access-preconditions.md`
19. `03-research-results/` for supporting evidence

## Folder map

| Folder | Purpose | Doctrine status |
|---|---|---|
| `00-entrypoint/` | LLM and human orientation | Current guidance |
| `01-project-standard/` | Baseline and v0.2 standard revision planning | Candidate doctrine until accepted |
| `02-research-prompts/` | Prompts used for Claude/GPT/Hermes research | Inputs only |
| `03-research-results/` | Research outputs and summaries | Evidence, not doctrine |
| `04-design-decisions/` | Explicit architecture and governance decisions | Doctrine when status is accepted |
| `05-specs/` | Draft authority docs, registries, workflows, and implementation specs | Active drafts until reviewed |
| `06-handoff-patterns/` | Agent task-packet patterns | Draft until reviewed |
| `07-hermes/` | Hermes boundaries, tests, and skill planning | Draft until validated |
| `08-obsidian/` | Obsidian/plugin/MCP constraints | Evidence and draft constraints |
| `99-archive/` | Retired or superseded docs | Not current |

## Accepted v0.2 foundation

### Lifecycle

```yaml
status: draft | active | blocked | complete | archived
review_status: not-required | pending | approved | rejected
authority: none | proposed | accepted
```

- `active` means usable at the note's current authority level; it does not mean approved.
- Only humans may set `review_status: approved`, `review_status: rejected`, or `authority: accepted`.
- Supersedence uses explicit relationships, not a status value.

### Identity

Every durable note uses one immutable tool-generated ID:

```text
kz-<type-prefix>-<ulid>
```

No second UUID is required in v0.2.

### Metadata and links

- `project` is required.
- `domain` is optional and lowercase kebab-case when present.
- Frontmatter relationships use stable IDs.
- Canonical body links use relative Markdown links.
- Wikilinks may appear in staging but must be normalized or rejected before promotion.

### Staging and promotion

Recommended sibling roots:

```text
C:\dev\kaizen-vault
C:\dev\kaizen-staging
```

- Hermes may eventually write only to the staging root.
- Canonical content remains read-only to Hermes.
- Promotion is human-controlled and validated.
- Promotion events are append-only JSONL at `_governance/promotion-log.jsonl` in the canonical vault repository.
- Durable agent provenance remains after promotion; `validation_status` does not.

### Initial note types

```text
command-center
overview
current-state
source-summary
claim
decision
spec
audit
task-packet
```

Deferred until earned:

```text
roadmap
raw-source
source-import-map
observatory-insight
opportunity
domain-note
promotion-record
conflict
```

### Universal frontmatter

```yaml
id:
type:
status:
project:
summary:
created:
updated:
```

Type-specific fields are defined in `05-specs/kaizen-note-type-registry.md`.

## Current strongest constraints

1. Raw Markdown plus flat validated YAML is canonical for project intelligence.
2. Obsidian is the current human interface, not an independent source of truth.
3. Plugins are non-canonical convenience layers unless an accepted decision says otherwise.
4. Postgres owns structured operational records, not canonical project narrative.
5. Qdrant owns no truth and must remain rebuildable from Markdown.
6. Agents access Postgres and Qdrant only through approved typed APIs/tools.
7. Hermes may search, draft, lint, package, and propose within approved boundaries.
8. Hermes may not approve, promote, run arbitrary SQL, mutate Qdrant, touch source repos, publish externally, or mark work implementation-ready.
9. Agent writes begin in the sibling staging root and require deterministic validation plus human promotion.
10. Research and observations are evidence, not doctrine.
11. Search-before-create and diff-before-write are mandatory agent rules.
12. Declared invariants require validation and eventually real-execution hammer coverage.

## Rules for LLMs

1. Never assume this repo is the future Obsidian vault.
2. Never create production vault folders here unless the user explicitly asks for a simulation or draft.
3. Treat accepted decisions as governing inputs; do not silently contradict them.
4. Do not change an accepted decision without an explicit amendment or superseding decision and human approval.
5. Do not promote research directly into doctrine.
6. Do not mark Hermes as approved for write access until all preconditions and boundary tests pass.
7. Prefer small, durable Markdown files over monolithic documents.
8. Use explicit file paths when referencing repo docs.
9. Keep documents readable as raw Markdown and avoid rendered-only meaning.
10. Preserve boundaries across Markdown, Postgres, Qdrant, source repos, operational repos, staging, and private/raw storage.
11. Search before creating a new document.
12. Produce a reviewable diff before modifying existing governed content.
13. When uncertain, record an open question or propose a decision rather than inventing policy.
14. Never treat a mutable frontmatter field as an access-control mechanism.

## Remaining v0.2 decisions

- Final `pipeline_stage` enum
- Exact canonical folder placement for the nine initial note types
- Portable vault-root naming/path convention
- Promotion-log JSON Schema and event action vocabulary
- ULID generator command and machine-readable type-prefix registry
- Plugin install/defer policy
- Intentional `.obsidian` version-control policy
- Whether source URLs remain after stable source IDs are assigned
- Initial private-data scanning and raw-data size thresholds
- Whether current-state notes are indexed in future Qdrant

## Required output shape for future docs

Prefer:

```markdown
# Clear Title

Status: draft | proposed | accepted | rejected | superseded | archived
Date: YYYY-MM-DD

## Purpose or Context

Why this file exists.

## Content or Decision

The actual material.

## Consequences or Validation

What follows from it.

## Open questions

What remains unresolved.

## Related files

Explicit repository paths.
```

## Stewardship stance

Kaizen grows by evidence, not enthusiasm.

The default answer to new structure, automation, plugins, folders, note types, database tables, retrieval fields, and agent powers is:

```text
not yet - earn it
```

When a capability proves useful through real friction, governed need, or research-backed evidence, document it, decide it, validate it, hammer-test the boundary when appropriate, and only then add it to the standard.
