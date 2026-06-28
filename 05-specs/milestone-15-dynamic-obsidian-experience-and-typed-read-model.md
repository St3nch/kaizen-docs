# Milestone 15 — Dynamic Obsidian Experience and Typed Read Model

Status: draft — not owner accepted
Date: 2026-06-28
Milestone: 15

## Purpose

Milestone 15 makes Kaizen's canonical Markdown intelligence easier to navigate, review, resume, validate, and package without creating a second source of truth.

M15 is the bridge between the accepted Kaizen v1 operating core and the later full-system interface/retrieval/agent roadmap.

The central M15 split is:

```text
Obsidian = immediate human cockpit over canonical Markdown
Bases / Dataview = derived human read models only
Kaizen parser = client-neutral typed read-model projector
Context packs = governed bundles assembled from canonical notes and typed projections
Tauri = later operator shell that consumes the typed read model
```

M15 prepares the read-model and workflow contract that a later Tauri operator shell can consume. M15 does not implement Tauri.

## Authority basis

```text
ROADMAP_V0.4.md
03-research-results/384-m15-research-synthesis-and-tauri-aligned-direction.md
Kaizen M15 Deep Research on Obsidian Dynamic Experience and Typed Read Model, user-supplied source material
04-design-decisions/0003-raw-markdown-is-canonical.md
04-design-decisions/0011-progressive-hybrid-human-interface-direction.md
01-project-standard/kaizen-project-standard-v0.2.md
```

## Objective

M15 must define and prove a minimal dynamic operating layer over canonical project intelligence:

```text
canonical Markdown notes
-> flat frontmatter and body-heading contracts
-> parser-backed typed read model
-> validation errors and typed records
-> current-state / command-center / review / resume views
-> governed context-pack assembly
-> future Tauri-ready contract
```

The milestone is successful when a human can operate a governed Kaizen project through clearer Obsidian-facing views while agents and future clients can consume the same project state through a deterministic typed read model.

## Core design commitments

### 1. Raw Markdown remains canonical

M15 must not move project truth into rendered plugin output, `.base` files, Dataview results, Canvas layout, generated JSON snapshots, Tauri state, or agent memory.

### 2. Obsidian is the cockpit, not the authority

Obsidian may show dashboards, queues, links, bookmarks, searches, and embedded Bases views.

Obsidian does not become a second source of truth.

### 3. Tauri remains planned later

Tauri remains the likely future operator shell under the progressive hybrid interface roadmap.

M15 must shape the typed read model that Tauri later consumes, but M15 must not create or mutate a Tauri app.

### 4. Bases first, Dataview only if needed

Obsidian Bases may be used as a human read-model layer over note frontmatter and file metadata.

Dataview may be used only as a tactical optional supplement where Bases cannot express a materially useful view.

DataviewJS must not be required for M15 correctness.

### 5. Agent access stays conservative

Agents may read canonical Markdown, read typed projections, assemble context packs, and draft proposal/review artifacts in approved lanes.

Agents must not approve, reject, promote, mutate accepted decisions, overwrite accepted current state, or use broad Obsidian MCP/REST/CLI write authority.

## Required M15 deliverables

### Note schema contract

M15 must define a flat frontmatter schema and body-heading conventions for the minimum M15 note set.

Candidate note types:

```text
project
current-state
command-center
review-item
resume
decision
spec
audit
task-packet
implementation-return
claim
source-summary
```

Common frontmatter candidates:

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

Frontmatter must stay flat and boring. Prose, rationale, evidence reasoning, objections, implementation details, and long context belong in Markdown body sections.

### Current-state contract

M15 must define `current-state.md` as the short, curated, plain-Markdown truth surface for a governed project.

It should be readable without plugin rendering and useful to both humans and agents.

### Command-center contract

M15 must define `command-center.md` as the human cockpit note.

The top section must be plain Markdown and agent-readable. Human-only dynamic sections may embed Bases or other allowed views below the durable text.

### Review queue contract

M15 must define review queue items as first-class Markdown notes, not loose task checkboxes.

The minimal review state machine is:

```text
proposed -> ready -> in-review -> approved | rejected | superseded
```

Agents may prepare `proposed` or `ready` review artifacts. Humans own approval, rejection, and promotion.

### Resume view contract

M15 must define a resume view that supports fresh-context continuation.

The resume view should include:

```text
current objective
current constraints
critical accepted decisions
active review items
active task packets
freshest implementation return
blocked work
exact read-next path
```

It must remain useful in raw Markdown.

### Typed read-model contract

M15 must define and prove a parser-backed typed read model.

The parser should deterministically project canonical Markdown and frontmatter into typed records and validation errors.

The typed read model must be client-neutral and future Tauri-ready.

Generated snapshots, if any, are derived artifacts only.

### Governed context-pack contract

M15 must define basic governed context-pack assembly before Qdrant.

A context pack should include:

```text
manifest
project boundary
current-state note
resume path
accepted decisions still in force
active relevant specs or task packets
open review items when relevant
freshest implementation return when relevant
```

A context pack should exclude:

```text
raw evidence dumps
unrelated projects
superseded material unless requested
speculative drafts without authority
private notes outside boundary
plugin-render-only artifacts
```

## Entry criteria

M15 may begin implementation planning only when:

```text
Kaizen v1 is owner accepted;
post-v1 hardening lane is closed enough to proceed;
Result 384 M15 research synthesis is recorded;
M15 definition/spec is drafted and audited;
owner accepts the M15 definition/spec and first task packet;
current docs repository state is clean and synced before mutation.
```

## Exit criteria

M15 can close only when:

```text
M15 note schema contract is defined and validated;
current-state and command-center contracts are defined and demonstrated;
review-item note type and review-state flow are demonstrated;
resume view contract is demonstrated;
parser-backed typed read model is implemented or explicitly constrained to a proven prototype;
context-pack assembly contract is demonstrated;
all generated or dynamic views are proven derived from canonical Markdown;
raw Markdown remains readable without plugin rendering;
Tauri alignment is documented without implementing Tauri;
closure audit passes;
owner accepts M15 closure.
```

## Required proof

M15 implementation must produce evidence for:

```text
schema validation
parser output / typed records
validation failure behavior
current-state and command-center examples
review queue example
resume view example
context-pack manifest and bundle example
raw Markdown readability without plugin rendering
non-authority of Bases / Dataview / generated snapshots
Tauri-readiness of typed read-model contract
```

## Recommended packet sequence

```text
023C — M15 Definition Audit and Owner Acceptance Packet
023D — M15 Note Schema and Body-Heading Contracts
023E — Parser-Backed Typed Read Model Prototype
023F — Current-State and Command-Center Contracts
023G — Review-Item Notes and Review Queue Views
023H — Resume View and Context-Pack Assembly
023I — M15 Integrated Proof and Closure Audit
```

## Stop conditions

M15 must stop if:

```text
work drifts into Tauri implementation;
work drifts into Qdrant implementation;
work grants Hermes or other agents write authority;
work exposes broad Obsidian MCP/REST/CLI write access;
plugin-rendered output is treated as canonical truth;
generated JSON snapshots are treated as canonical truth;
frontmatter schema grows nested, prose-heavy, or plugin-specific;
review state can be approved by an agent;
current-state truth becomes unreviewed auto-generated output;
context packs include unrelated/private/superseded material without explicit request;
repository state is unexpectedly dirty before mutation;
secrets or private credentials appear in docs or generated context packs.
```

## Non-authorization

This M15 definition draft does not authorize:

```text
M15 implementation;
platform mutation;
vault mutation;
staging mutation;
database mutation;
production database mutation;
Tauri implementation;
Qdrant implementation;
Hermes write authority;
Obsidian MCP write authority;
Obsidian REST write authority;
provider purchase;
customer-data reuse;
logged-in scraping;
Observatory / IMI implementation;
commercial operations;
production deployment;
full Neon Ronin implementation;
full SearchClarity implementation;
backup deletion;
restore work;
Git push beyond the separately approved docs sync for this draft.
```

## Steward recommendation

Proceed to a focused M15 definition audit before owner acceptance.

The audit should specifically test whether this draft:

```text
preserves raw Markdown as canonical;
keeps Obsidian dynamic views derived and non-authoritative;
keeps Tauri in the roadmap without implementing it early;
defines a usable parser-backed typed read model;
keeps agent authority bounded;
sets clear enough exit criteria for implementation packets.
```
