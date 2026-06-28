# M15 Research Synthesis and Tauri-Aligned Direction

Status: synthesis / planning direction
Date: 2026-06-28
Source material: user-supplied deep research report, `Kaizen M15 Deep Research on Obsidian Dynamic Experience and Typed Read Model`

## Purpose

This record distills the M15 deep research into Kaizen's next planning direction.

It does not authorize implementation, vault mutation, platform mutation, database mutation, Tauri implementation, Qdrant, Hermes write authority, Obsidian MCP writes, Observatory / IMI work, downstream project mutation, backup deletion, or restore work.

## Executive synthesis

M15 should not be treated as an Obsidian-beautification project.

M15 should define the operating/read-model layer that makes canonical Markdown easier to navigate, review, resume, validate, and package for humans and future clients.

The recommended split is:

```text
Obsidian = immediate human cockpit over canonical Markdown
Bases / Dataview = derived human read models only
Kaizen parser = client-neutral typed read-model projector
Context packs = governed bundles assembled from canonical notes and typed projections
Tauri = later operator shell that consumes the typed read model
```

Tauri remains part of the Kaizen roadmap. M15 prepares the contract that Tauri will later consume; M15 does not build the Tauri app.

## Research-supported design direction

### 1. Obsidian cockpit, not second source of truth

Obsidian should remain the human workspace for canonical Markdown. It can show dynamic views, links, bookmarks, searches, and dashboards, but it must not become a hidden database or authority layer.

Rendered plugin output is convenience, not truth.

### 2. Boring frontmatter and readable bodies

M15 should define a small, flat, safe frontmatter schema for routing and lifecycle facts only.

Good frontmatter candidates:

```text
type
project
status
review_state
authority_level
effective_date
supersedes
superseded_by
owner
tags
related
last_reviewed
```

Prose, rationale, claims, evidence reasoning, objections, implementation details, and context belong in Markdown body sections, not YAML blobs.

### 3. Bases first, Dataview only when needed

Obsidian Bases should be the preferred human dashboard/read-model layer for M15 because it is core, property-oriented, and suitable for tables, lists, cards, and operator views derived from notes.

Dataview may remain an optional tactical supplement where Bases cannot express a useful view, but DataviewJS must not become required infrastructure.

### 4. Command centers and current-state notes

Each governed project should converge on two primary operational notes:

```text
current-state.md
command-center.md
```

`current-state.md` is plain Markdown truth: short, curated, and readable by humans or agents without plugin rendering.

`command-center.md` is the operator cockpit: plain Markdown resume/current sections first, then optional embedded human-only dynamic views.

### 5. Review queue items are notes

Review queue items should be first-class Markdown notes with frontmatter, not loose task checkboxes.

Recommended review state machine:

```text
proposed -> ready -> in-review -> approved | rejected | superseded
```

Agents may prepare proposals and ready-for-review items. Humans approve, reject, or promote.

### 6. Parser-backed typed read model

A client-neutral typed read model should be a deterministic projection from canonical Markdown and frontmatter into typed records and validation errors.

The typed read model must not depend on Obsidian rendering, Bases serialization, Dataview runtime, Canvas layout, or Tauri.

A small Python or TypeScript parser is the likely M15 implementation path.

### 7. Context packs before Qdrant

Before Qdrant exists, context packs should be generated or curated from canonical notes and typed projections.

A good context pack should include:

```text
manifest
project boundary
current-state note
resume path
accepted decisions still in force
active specs or task packets relevant to the request
open review items when relevant
freshest implementation return when relevant
```

It should exclude raw dumps, unrelated projects, superseded material unless requested, speculative drafts without authority, private notes outside boundary, and plugin-render-only artifacts.

### 8. Agent access remains conservative

M15 should prefer filesystem read access and Kaizen-owned parsing over broad Obsidian MCP, REST, CLI, or plugin write authority.

Agents may read canonical Markdown, read typed projections, assemble context packs, and draft proposal/review notes in approved staging/proposal lanes.

Agents must not change approval state, overwrite accepted current state, mutate accepted decisions, or use unrestricted Obsidian write APIs.

## Tauri alignment

The M15 output should make later Tauri work easier, not compete with it.

The future Tauri app should consume the same typed read model M15 defines:

```text
canonical Markdown / vault
-> frontmatter and body-heading conventions
-> Kaizen parser / typed read model
-> review queues, resume views, context-pack manifests, project state
-> Obsidian cockpit now
-> Tauri operator shell later
```

This protects the roadmap from two bad outcomes:

```text
1. Obsidian plugin views become the only way to understand the system.
2. Tauri later invents a second truth layer because M15 failed to define a shared contract.
```

## Recommended M15 milestone definition direction

M15 should define and prove:

```text
flat note-type/frontmatter schema
canonical body-heading conventions
current-state.md template / contract
command-center.md template / contract
review-item note type and review-state model
human Bases views over canonical notes
resume note / resume path contract
client-neutral typed read-model parser contract
context-pack manifest and Markdown bundle contract
validation checks for schema, authority, freshness, and supersedence
```

M15 should explicitly defer:

```text
Tauri shell implementation
Qdrant semantic retrieval
Hermes write authority
Obsidian MCP write authority
DataviewJS dependency
Canvas as required infrastructure
Observatory / IMI implementation
production deployment
downstream project implementation
```

## Proposed next packet sequence

```text
023B: Draft M15 milestone definition / spec
023C: Audit M15 definition and resolve open questions
023D: Owner acceptance of M15 spec and first task packet
023E: Define M15 note schema and body-heading contracts
023F: Implement parser-backed typed read model / validator
023G: Define current-state and command-center templates
023H: Define review-item notes and human review queue Bases views
023I: Define resume view and context-pack assembly
```

## Open questions for M15 spec drafting

1. Should M15 parser be Python-first because Kaizen platform is Python-heavy, or TypeScript-first because Tauri/front-end work will later consume the model?
2. Should generated typed snapshots be JSON files on disk, ephemeral command output, or both?
3. What exact note types are in M15 scope versus later milestones?
4. Should Bases files be created during M15, or should M15 first define their contract and defer concrete `.base` files to implementation packets?
5. How much automatic updating should be allowed for `current-state.md` versus human-curated updates?

## Steward recommendation

Proceed to a bounded M15 definition/spec draft before implementation.

The spec should be Tauri-aware but not Tauri-implementing. It should make the typed read model the shared contract across Obsidian, future Tauri, agents, and context-pack assembly.
