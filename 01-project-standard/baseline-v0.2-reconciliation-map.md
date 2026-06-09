# Kaizen Baseline to v0.2 Reconciliation Map

Status: active evidence-backed reconciliation
Date: 2026-06-09

## Purpose

Map the original 591-line Kaizen Project Standard to the accepted and implemented v0.2 foundation.

The original baseline is preserved at:

```text
01-project-standard/kaizen-project-standard.md
```

This map identifies what v0.2 should preserve, replace, defer, or reject. It prevents the rewrite from accidentally losing the original intent while also preventing outdated assumptions from surviving by inertia.

## Reconciliation rule

The original baseline remains historical source material.

Accepted Decisions 0001 through 0012 and reviewed implementation evidence override conflicting baseline details.

The v0.2 draft should preserve the baseline's goals and operating philosophy while replacing superseded schemas, permissions, plugin assumptions, workflow mechanics, and one-way handoff semantics.

## Section map

| Baseline section | v0.2 direction | Reason |
|---|---|---|
| Purpose and reusable project system | Preserve | Still the core Kaizen goal |
| Project intelligence pipeline | Preserve and close the loop | Six-stage enum is accepted; implementation return is required |
| Two agent tiers | Preserve concept, replace authority details | Agent authority is narrower and tool-enforced |
| Required Plugins | Replace | Canonical operation proved plugin-independent |
| Standard Project Folder Structure | Replace selectively | Nine types and earned-folder placement were proven in Milestone 4 |
| Standard Frontmatter | Replace completely | Superseded by field registry and lifecycle model |
| Command Center - LLM Entry Point | Preserve and strengthen | Plain-text entry point remains foundational |
| Command Center - Dataview status | Defer/optional | Human convenience only, never canonical |
| Command Center - Templater/Commander actions | Defer | Must be earned after manual workflow proves need |
| Canonical source links | Preserve concept, normalize paths | Keep source references; avoid local paths as portable identity |
| Vault-level dashboard | Defer | Useful only after enough canonical notes exist |
| Templater templates | Replace with static note contracts | Templates may later generate accepted schemas |
| Hermes configuration | Replace completely | Typed tools, staging, validation, approval, and operator controls govern agents |
| Build order | Replace | Tools and hammer gates precede live agent or database systems |
| Adding a new project | Preserve concept, rewrite details | Keep minimal project bootstrap and earned folders |
| What Never Goes in Kaizen | Preserve and expand | Confirmed by source-of-truth research and implementation |
| Source-of-Truth Rules | Preserve and expand | Add operational databases, staging, agents, and event-log boundaries |

## Preserve unchanged in spirit

### One governed path for every project

Every project should move through a standard path from idea to implementation-ready handoff and back through implementation evidence.

### One command center per project

The command center remains the human and agent entry point. Important orientation must remain plain text and usable without plugins.

### Earned structure

Do not pre-create empty folders, note types, databases, dashboards, or integrations. Structure appears when real content and repeated friction justify it.

### Separation from source and operations repositories

Kaizen owns project intelligence, synthesis, decisions, planning specs, audits, handoffs, and reviewed implementation-return evidence.

Code, tests, implementation details, operational procedures, generated artifacts, and raw/private data remain in their proper systems.

### Implementation-ready task packets

The principal implementation output remains a task packet a coding agent can execute without guessing. Completion evidence returns through governed amendment rather than unreviewed editing.

## Replace in v0.2

### Plugin-first setup

Replace the baseline requirement to install Dataview, Templater, and Commander first with:

- no plugin is required for canonical Kaizen operation;
- raw Markdown and validated YAML work first;
- Dataview may be added as a human read model after enough notes exist;
- Templater and Commander are deferred until repeated manual work proves need;
- Canvas, Kanban, Tasks, Excalidraw, AI plugins, and broad Obsidian REST/MCP access are deferred or non-canonical.

### Baseline frontmatter

Replace the baseline schema with the accepted universal core:

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

Replace mixed status values with three orthogonal axes:

```yaml
status: draft | active | blocked | complete | archived
review_status: not-required | pending | approved | rejected
authority: none | proposed | accepted
```

### Hermes direct vault work

Replace broad direct-vault actions with:

- read/search access to approved roots;
- writes only inside sibling staging through constrained tools;
- deterministic validation;
- human-controlled immutable planning and execution approval;
- no direct Postgres or Qdrant mutation;
- no commit/push, authority transition, source-repo mutation, or external publication;
- local human operator fallback when connector mutation is unavailable.

### Raw source notes

Replace pasted raw content in canonical Markdown with:

- raw or bulk content outside the canonical vault;
- source summaries in Markdown;
- direct URLs or stable source IDs for provenance;
- a `raw-source` note type only if future ingestion workflows prove it necessary.

### Machine-specific source paths

Replace absolute Windows paths as canonical identity with:

- repository URL or stable repository identifier;
- repo-relative path;
- optional commit/ref;
- local path only as non-canonical environment guidance.

### One-way handoff pipeline

Replace the baseline pipeline ending at coding-agent handoff with:

```text
task packet
-> implementation
-> tests and completion evidence
-> governed completion-report amendment
-> governed current-state update
-> retrospective and contract improvement
```

## Initial v0.2 note types

The accepted and proven set is:

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

## Accepted project structure for v0.2

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

No earned folder exists before its first real note.

## Command center changes

### Preserve

- short plain-text LLM entry point;
- current stage;
- blockers;
- next action;
- source-of-truth boundaries;
- read-first references;
- do-not-touch boundaries.

### Replace

- plugin queries are optional derived views below canonical prose;
- quick-action buttons are deferred;
- hard-coded absolute local paths are not canonical identity;
- command-center metadata follows the accepted field registry.

## Template changes

Do not port baseline Templater syntax into v0.2.

Static Markdown contracts for the nine note types are canonical. Later templates may generate those contracts, but template execution is not part of canonical meaning.

## Evidence-backed build order

1. Preserve the baseline as historical source material.
2. Accept foundation decisions and registries.
3. Build and hammer deterministic ID, parsing, validation, staging, promotion, and amendment tools.
4. Create canonical and staging roots with clean Git boundaries.
5. Run one complete governed project pipeline.
6. Return implementation evidence through governed amendments.
7. Retrospect and consolidate the standard from observed evidence.
8. Improve operator ergonomics before adding new intelligence infrastructure.
9. Add plugins, databases, retrieval, providers, and live agent integrations only after a concrete workload earns them.

## Research and implementation evidence status

Document-contract research and one complete governed implementation loop are complete.

No broad new research pass is required before drafting v0.2. Targeted research remains appropriate for:

- durable actor identity and authentication;
- backup and remote policy;
- future operational databases;
- Qdrant retrieval behavior;
- provider and source-rights constraints;
- live agent integration.

## Remaining design decisions

- durable human actor identity and authentication;
- amendment and recovery event terminology and compatibility rules;
- spec maturity labels after implementation;
- backup and remote checkpoint policy;
- connector mutation versus local operator expectations;
- private/raw-data threshold calibration;
- whether `operate` earns a future pipeline stage;
- future Qdrant treatment of current-state notes.

## Milestone 4 evidence reconciliation

The following directions are now proven:

- six-stage pipeline enum;
- nine note types;
- exact canonical folder placement;
- sibling staging;
- append-only promotion and amendment events;
- immutable plan, validation, candidate, diff, and approval evidence;
- separate human plan and execution gates;
- governed task-packet completion and current-state amendments;
- no plugin, database, retrieval layer, or live agent integration required for canonical operation.

Remaining gaps are recorded in Result 063 and must be reconciled without rewriting historical event evidence or mass-editing canonical notes.

## Compatibility rules

1. Preserve existing canonical IDs and operation IDs.
2. Preserve historical JSONL lines exactly.
3. Extend schemas backward-compatibly.
4. Keep first-time promotion behavior unchanged.
5. Do not mass-rewrite canonical notes for formatting only.
6. Preserve the current vault and sibling staging layout.
7. Keep the vault without a remote until the owner makes a separate decision.
8. Preserve the imported baseline in Git history after v0.2 acceptance.

## Related files

- `01-project-standard/kaizen-project-standard.md`
- `01-project-standard/standard-revision-plan.md`
- `03-research-results/063-milestone-5-first-slice-retrospective.md`
- `04-design-decisions/0001-two-zone-agent-write-model.md`
- `04-design-decisions/0007-foundation-resolution-for-v0.2.md`
- `04-design-decisions/0008-v0.2-operating-conventions.md`
- `04-design-decisions/0012-first-slice-contract-and-implementation-boundary.md`
- `05-specs/kaizen-field-registry.md`
- `05-specs/kaizen-note-type-registry.md`
