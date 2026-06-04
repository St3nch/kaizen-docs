# LLM Start Here

This is the first file every LLM should read when working in the `kaizen-docs` repository.

## What this repo is

`kaizen-docs` is the planning and stewardship repository for Kaizen.

Kaizen is a portable Markdown-first project intelligence system. It is being designed to move projects through this flow:

```text
idea capture -> research -> source summaries -> claims -> specs -> audit -> handoff packet -> coding agent build
```

This repository is **not** the future Obsidian vault. It is the design workspace used to research, plan, govern, and audit that future system.

## Current architecture direction

```text
Markdown / Obsidian = canonical project intelligence
Postgres Observatory = operational source of truth for observations, jobs, runs, costs, and logs
Qdrant = rebuildable semantic retrieval index over approved Markdown
Hermes / agents = constrained clerks through approved typed tools
Humans = authority-bearing approvals and promotion
```

## Current project posture

- Current phase: foundation design and governance drafting
- Future vault: not created yet
- Postgres Observatory: not implemented
- Qdrant index: not implemented
- Hermes Desktop / Hermes Agent: installed by user, not approved for Kaizen write access
- Existing architectural decisions are still proposed unless explicitly marked accepted
- Primary immediate need: review the new decisions/registries, resolve lifecycle and ID questions, then revise the project standard
- Stewardship principle: keep the system lean until structure earns its existence

## Read-first sequence

Read in this order unless the user gives a narrower task:

1. `00-entrypoint/LLM_START_HERE.md` - this file
2. `01-project-standard/kaizen-project-standard.md` - original baseline and current standard status
3. `04-design-decisions/0003-raw-markdown-is-canonical.md`
4. `04-design-decisions/0004-system-of-record-boundaries.md`
5. `04-design-decisions/0005-api-only-structured-data-access.md`
6. `04-design-decisions/0006-hammer-tests-are-a-hard-gate.md`
7. `05-specs/kaizen-field-registry.md`
8. `05-specs/kaizen-note-type-registry.md`
9. `05-specs/postgres-observatory-authority.md`
10. `05-specs/kaizen-validation-gate-spec.md`
11. `05-specs/kaizen-hammer-test-strategy.md`
12. `07-hermes/hermes-permission-matrix.md`
13. `07-hermes/hermes-write-access-preconditions.md`
14. `03-research-results/` for supporting evidence

## Folder map

| Folder | Purpose | Doctrine status |
|---|---|---|
| `00-entrypoint/` | LLM and human orientation | Current guidance |
| `01-project-standard/` | Baseline Kaizen standard drafts | Candidate doctrine |
| `02-research-prompts/` | Prompts used for Claude/GPT/Hermes research | Inputs only |
| `03-research-results/` | Research outputs and summaries | Evidence, not doctrine |
| `04-design-decisions/` | Explicit decisions about Kaizen design | Doctrine only when status is accepted |
| `05-specs/` | Draft authority docs, registries, and implementation/planning specs | Draft until reviewed |
| `06-handoff-patterns/` | Agent task packet patterns | Draft until reviewed |
| `07-hermes/` | Hermes capability, boundaries, tests, and skill planning | Draft until validated |
| `08-obsidian/` | Obsidian/plugin/MCP constraints | Evidence and draft constraints |
| `99-archive/` | Retired or superseded docs | Not current |

## Current strongest constraints

1. Raw Markdown plus flat validated YAML is the canonical project-intelligence layer.
2. Obsidian is the current human interface, not an independent source of truth.
3. Dataview, Templater, Commander, Canvas, Kanban, Tasks, and other plugins are non-canonical convenience layers unless explicitly promoted through a decision.
4. Postgres owns structured observations and operational records; it does not own canonical project narrative.
5. Qdrant owns no truth and must remain rebuildable from Markdown.
6. Hermes may search, draft, lint, package, and propose within approved boundaries.
7. Hermes may not approve, promote, run arbitrary SQL, mutate Qdrant, touch source repos, publish externally, or mark work implementation-ready.
8. Agent writes begin in staging and require validation plus human promotion.
9. Research and observations are evidence, not doctrine.
10. Declared invariants should receive validation and eventually real-execution hammer coverage.

## Rules for LLMs

1. Never assume this repo is the future Obsidian vault.
2. Never create vault folders here unless the user explicitly asks for a simulated or draft structure.
3. Do not promote research to doctrine without an accepted decision.
4. Do not change a proposed decision to accepted without explicit human approval.
5. Do not mark Hermes as approved for write access until all preconditions and tests are satisfied.
6. Prefer small, durable Markdown files over huge monolithic docs.
7. Use explicit file paths when referencing docs.
8. Keep documents readable as raw Markdown; do not rely on rendered-only features.
9. Preserve system-of-record boundaries across Markdown, Postgres, Qdrant, source repos, operational repos, and private/raw storage.
10. When uncertain, create an open question, research item, or decision proposal rather than inventing policy.
11. Search before creating a new document.
12. Produce a reviewable diff before modifying existing governed content.

## Current unresolved decisions

- Final lifecycle enums for `status`, `review_status`, and `authority`
- Whether all notes need both a human-readable ID and UUID
- Whether `domain` is universal or conditional
- Final internal link convention
- Staging location: in-vault, sibling repo/folder, or shadow vault
- Promotion event storage: Markdown, Postgres, or both
- Exact Observatory result object and retention model
- Qdrant chunking/embedding choices after hands-on evaluation
- Where Kaizen validation and hammer code will live
- Which `.obsidian` configuration files should be version-controlled

## Required output style for future docs

Prefer this shape:

```markdown
# Clear Title

Status: draft | proposed | accepted | rejected | superseded | archived
Date: YYYY-MM-DD

## Purpose or Context

Why this file exists.

## Content or Decision

The actual useful material.

## Consequences or Validation

What follows from it.

## Open questions

What remains unresolved.

## Related files

Explicit paths.
```

## Stewardship stance

Kaizen should grow by evidence, not enthusiasm.

The default answer to new structure, automation, plugins, folders, note types, database tables, and agent powers is:

```text
not yet - earn it
```

When a feature proves useful through real friction, governed need, or research-backed evidence, document it, decide it, validate it, and only then add it to the standard.
