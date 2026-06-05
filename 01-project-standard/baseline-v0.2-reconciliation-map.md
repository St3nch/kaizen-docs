# Kaizen Baseline to v0.2 Reconciliation Map

Status: active draft
Date: 2026-06-04

## Purpose

Map the original 591-line Kaizen Project Standard to the accepted v0.2 foundation.

The original baseline is preserved at:

```text
01-project-standard/kaizen-project-standard.md
```

This map identifies what v0.2 should preserve, replace, defer, or reject. It prevents the rewrite from accidentally losing the original intent while also preventing outdated assumptions from surviving by inertia.

## Reconciliation rule

The original baseline remains historical source material.

Accepted Decisions 0001 through 0007 override conflicting baseline details.

The v0.2 draft should preserve the baseline's goals and operating philosophy while replacing superseded schemas, permissions, plugin assumptions, and workflow mechanics.

## Section map

| Baseline section | v0.2 direction | Reason |
|---|---|---|
| Purpose and reusable project system | Preserve | Still the core Kaizen goal |
| Project intelligence pipeline | Preserve and refine | Core flow remains valid; exact stage enum needs final decision |
| Two agent tiers | Preserve concept, replace authority details | Hermes remains clerk; accepted boundaries are stricter and tool-enforced |
| Required Plugins | Replace | Research rejected plugin-first architecture |
| Standard Project Folder Structure | Replace selectively | Preserve earned folders; align to nine initial note types |
| Standard Frontmatter | Replace completely | Superseded by accepted field registry and lifecycle model |
| Command Center - LLM Entry Point | Preserve and strengthen | Plain-text entry point remains foundational |
| Command Center - Dataview status | Defer/optional | Human convenience only, never canonical |
| Command Center - Templater/Commander actions | Defer | Must be earned after manual workflow proves need |
| Canonical source links | Preserve concept, normalize paths | Keep source-of-truth references; avoid machine-specific absolute paths as canonical format |
| Vault-level dashboard | Defer | Useful only after enough canonical notes exist |
| Templater templates | Replace with static note contracts | Templates may be built later from accepted registries |
| Hermes configuration | Replace completely | Accepted permission matrix, staging root, typed tools, and promotion rules govern Hermes |
| Build order | Replace | Foundation and validation now precede vault/plugin construction |
| Adding a new project | Preserve concept, rewrite details | Keep minimal project bootstrap and earned folders |
| What Never Goes in Kaizen | Preserve and expand | Strongly confirmed by Postgres/Qdrant/source-of-truth research |
| Source-of-Truth Rules | Preserve and expand | Add Postgres Observatory, Qdrant, staging, and agent boundaries |

## Preserve unchanged in spirit

### One pipeline for every project

Every project should still move through a standard path from idea to implementation-ready handoff.

### One command center per project

The command center remains the human and agent entry point. Important orientation must remain plain text and usable without plugins.

### Earned structure

Do not pre-create empty folders or speculative note types. Structure appears when real content and repeated friction justify it.

### Separation from source and operations repositories

Kaizen owns project intelligence, synthesis, decisions, planning specs, audits, and handoffs.

Code, tests, implementation contracts, operational procedures, generated artifacts, and raw/private data remain in their proper systems.

### Implementation-ready task packets

The principal output remains a task packet a coding agent can execute without guessing.

## Replace in v0.2

### Plugin-first setup

The baseline says to install Dataview, Templater, and Commander first.

Replace with:

- no plugin is required for canonical Kaizen operation
- raw Markdown and validated YAML work first
- Dataview may be added as a human read model after enough notes exist
- Templater and Commander are deferred until manual workflow repetition proves need
- Canvas, Kanban, Tasks, Excalidraw, AI plugins, and broad Obsidian REST/MCP access are deferred or non-canonical

### Baseline frontmatter

Replace the baseline schema:

```yaml
type:
status:
project:
pipeline_stage:
phase:
summary:
created:
updated:
source_repo:
source_docs:
```

with the accepted universal core:

```yaml
id:
type:
status:
project:
summary:
created:
updated:
```

and type-specific fields defined in:

- `05-specs/kaizen-field-registry.md`
- `05-specs/kaizen-note-type-registry.md`

### Baseline lifecycle

Replace the mixed baseline status values with three orthogonal axes:

```yaml
status: draft | active | blocked | complete | archived
review_status: not-required | pending | approved | rejected
authority: none | proposed | accepted
```

### Hermes direct vault work

Replace broad direct-vault actions with:

- read/search access to approved roots
- writes only inside the sibling staging root after boundary tests pass
- deterministic validation
- human-controlled promotion
- no direct Postgres or Qdrant mutation
- no commit/push, approval, authority transition, source-repo mutation, or external publication

### Raw source notes

The baseline stores pasted raw content inside Markdown.

Replace with:

- raw/bulk content outside the canonical vault
- source summaries in Markdown
- direct URLs or stable source IDs for provenance
- a `raw-source` note type only if future ingestion workflows prove it necessary

### Machine-specific source paths

The baseline embeds absolute Windows paths in command centers and task packets.

Replace canonical representation with:

- repository URL or stable repository identifier
- repo-relative path
- optional commit/ref
- local path only as non-canonical environment guidance

## Initial v0.2 note types

The accepted initial set is:

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

Baseline types deferred from v0.2:

```text
roadmap
raw-source
source-import-map
```

The concepts may still appear as sections or ordinary prose until separate types earn their existence.

## Candidate project structure for v0.2

This remains a draft until folder placement is accepted:

```text
projects/
  {project-slug}/
    command-center.md
    overview.md
    current-state.md
    source-summaries/   # earned
    claims/             # earned
    decisions/          # earned
    specs/              # earned
    audits/              # earned
    handoffs/            # earned
```

No empty earned folder is created before its first real note.

## Command center changes

### Preserve

- short plain-text LLM entry point
- current stage
- blockers
- next action
- source-of-truth boundaries
- read-first references
- do-not-touch boundaries

### Replace

- plugin queries are optional derived views below canonical prose
- quick-action buttons are deferred
- hard-coded absolute local paths are not canonical
- command-center metadata must follow the accepted field registry

## Template changes

Do not port baseline Templater syntax into v0.2.

First define static Markdown contracts for the nine note types. Later templates may generate those contracts, but template execution is not part of canonical meaning.

## Build-order changes

Recommended v0.2 planning order:

1. Import and preserve the baseline.
2. Accept foundation decisions and registries.
3. Resolve remaining v0.2 design choices.
4. Complete document-contract research for claims, decisions, specs, audits, and task packets.
5. Draft and audit the complete v0.2 standard.
6. Build static validation and ID tooling.
7. Create canonical and staging roots.
8. Run one full project pipeline manually.
9. Add human convenience plugins only after measured friction.
10. Consider constrained Hermes writes only after boundary tests pass.

## Remaining research dependency

One major research pass still matters before finalizing v0.2 document contracts:

- claim/evidence records
- decision records
- implementation specs
- audit and review gates
- task packets and coding-agent handoffs
- objective acceptance criteria

This can be completed tomorrow with Claude and independently reviewed by GPT.

## Remaining design decisions

The following can mostly be resolved without deep research:

- final pipeline stage enum
- exact folder placement
- portable root/path convention
- promotion JSONL event schema
- ULID command and prefix registry
- plugin install/defer policy
- `.obsidian` Git policy
- source URL retention rule
- initial raw-data and private-data validation posture
- future Qdrant treatment of current-state notes

## Related files

- `01-project-standard/kaizen-project-standard.md`
- `01-project-standard/standard-revision-plan.md`
- `04-design-decisions/0001-two-zone-agent-write-model.md`
- `04-design-decisions/0007-foundation-resolution-for-v0.2.md`
- `05-specs/kaizen-field-registry.md`
- `05-specs/kaizen-note-type-registry.md`
