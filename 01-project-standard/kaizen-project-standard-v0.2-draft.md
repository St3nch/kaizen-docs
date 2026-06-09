# Kaizen Project Standard v0.2 Draft

Status: non-authoritative Milestone 5 consolidation draft
Date: 2026-06-09
Owner: Kaizen project steward
Acceptance: explicit owner review required

## Purpose

Kaizen is a continuously improving, agent-connected project intelligence system.

It gathers, organizes, evaluates, and compounds project knowledge; connects external and operational evidence to human-reviewed decisions; produces audited implementation-ready task packets; and returns implementation outcomes through governed updates.

This standard defines the proven first-slice operating model and the boundaries that future Kaizen systems must preserve.

This draft is not authoritative until explicitly accepted.

## Governing principle

```text
Smart and Optimal
```

That means:

- verify before claiming;
- read before editing;
- search before creating;
- diff before writing;
- never infer owner approval;
- never widen approved scope;
- preserve evidence;
- distinguish facts, project records, inferences, and recommendations;
- stop safely when a hash, path, Git state, dependency, operation, or authorization differs;
- keep governance strict where mistakes are consequential;
- avoid ceremony that does not protect a real boundary.

## Authority order

When sources conflict, apply this order:

1. accepted design decisions and amendments;
2. accepted specifications and registries;
3. verified implementation evidence and completion audits;
4. this accepted project standard, once accepted;
5. active roadmaps and task packets within their approved scope;
6. research results and recommendations;
7. the historical baseline standard.

The imported v0.1 baseline remains historical source material. It does not override later accepted decisions or proven implementation contracts.

## System identity

Kaizen is not only a documentation pipeline.

It is intended to become a living project intelligence environment that:

- moves projects from idea to implementation-ready instructions;
- lets humans and agents resume work without reconstructing context;
- preserves evidence, decisions, constraints, and implementation outcomes;
- supports technical, product, business, marketing, and strategic work;
- compounds reusable knowledge across projects;
- improves its own contracts and tooling through observed evidence.

The current proven slice is deliberately smaller than the full vision. Future systems remain deferred until a concrete workload earns them.

## Repository and root boundaries

### Planning and governance repository

```text
C:\dev\kaizen-docs
```

Owns:

- project doctrine;
- accepted decisions;
- specifications and registries;
- research results;
- task-packet source material;
- implementation roadmaps;
- audit and closure evidence.

It does not own platform runtime code or canonical project intelligence.

### Platform repository

```text
C:\dev\kaizen\platform
```

Owns:

- deterministic Kaizen tools;
- validation;
- staging-write controls;
- promotion and amendment workflows;
- fixed-root local operators;
- tests and implementation documentation.

The platform performs no Git push, reset, clean, stash, or remote creation as part of governed mutation workflows.

### Canonical vault

```text
C:\dev\kaizen\vault
```

Owns canonical governed Markdown and the append-only governance event log.

The vault is a separate Git repository. A remote is not required during an explicitly owner-accepted local-only period. Clean Git state and exact local commits remain mandatory.

### Sibling staging root

```text
C:\dev\kaizen\staging
```

Owns agent- or human-prepared candidates and immutable operation evidence before canonical mutation.

Staging is not canonical truth.

### Temporary MCP proving ground

```text
C:\dev\kaizen-mcp
```

Owns temporary project-only MCP experiments.

It is not doctrine and must not become a Git repository unless explicitly authorized. Proven patterns may later move into a production Kaizen MCP.

## System-of-record boundaries

### Canonical Markdown

Canonical Markdown owns:

- project orientation;
- source summaries;
- claims;
- decisions;
- specifications;
- human audit narratives and verdicts;
- task packets;
- reviewed implementation-return evidence;
- current-state snapshots.

### Source repositories

Source repositories own:

- code;
- tests;
- implementation details;
- operational runbooks;
- build artifacts;
- technical interfaces that are authoritative only in the source repository.

### Structured operational databases

Future Operational Postgres systems may own:

- ingestion state;
- automation and job state;
- governance and authorization events where later accepted;
- audit and security records;
- structured observations;
- provider requests and results;
- reference data.

The Observatory is one future bounded intelligence domain, not the name of the entire operational database.

A dedicated Internet Marketing Intelligence database remains a future boundary for longitudinal search, ranking, visibility, marketplace, and related evidence workloads.

No database implementation is authorized by this standard alone.

### Qdrant

Qdrant may later serve as a rebuildable semantic index.

It owns no unique truth and must be reconstructable from authoritative systems.

### Agents

Agents own no canonical domain.

Agents may operate only through approved tools and permissions. Agent output becomes canonical only through governed review and mutation.

## Canonical document format

Canonical project intelligence uses:

- UTF-8 Markdown;
- flat validated YAML frontmatter;
- ordinary headings;
- stable prefixed ULID IDs;
- explicit provenance;
- stable frontmatter relationships;
- relative Markdown body links where applicable.

Nested YAML objects are prohibited in canonical notes.

Obsidian plugins are not required for canonical operation.

## Universal frontmatter

Every durable Kaizen note requires:

```yaml
id:
type:
status:
project:
summary:
created:
updated:
```

### Identity

`id` is globally unique, immutable, tool-generated, and never reused.

Format:

```text
kz-<type-prefix>-<ULID>
```

Human readability comes from the title, filename, and links. A second UUID is not used.

### Status

```text
draft | active | blocked | complete | archived
```

`active` means usable at the note's current authority level. It does not mean approved.

### Review status

```text
not-required | pending | approved | rejected
```

Only a human may set `approved` or `rejected`.

### Authority

```text
none | proposed | accepted
```

Only a human may set `accepted`.

Evidence does not gain authority merely by being promoted.

### Pipeline stage

Project-level stage:

```text
capture | research | spec | audit | handoff | build
```

`pipeline_stage` belongs on command-center and current-state notes.

It describes the dominant project phase, not a monotonic state machine. A project may move backward when evidence or audit exposes gaps.

`operate` remains deferred until a real operational project proves it useful.

## Initial note types

The accepted and proven initial set is:

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

Adding a type requires a governed decision, prefix, fields, body contract, validation changes, lifecycle rules, agent rules, and future indexing direction.

Deferred types include:

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

## Canonical project structure

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

A governed workflow may create a missing earned folder only when:

- the note type maps to that folder;
- the destination remains inside the project root;
- traversal and reparse-point checks pass;
- creation occurs inside the reviewed operation.

## Required project notes

### Command center

Purpose: concise human and agent entry point.

Required sections:

- `## LLM entry point`
- `## Current stage`
- `## Read first`
- `## Current blockers`
- `## Next action`
- `## Source-of-truth boundaries`

Important orientation remains plain text. Plugin queries may appear only as optional derived views.

### Overview

Purpose: durable project purpose, scope, non-goals, and posture.

### Current state

Purpose: human-readable snapshot of project position, blockers, and next move.

It must not represent live jobs, queues, runs, or measurements that belong in a future operational database.

## Governed project loop

The proven loop is:

```text
project definition and current-state orientation
-> research questions and source collection
-> source summaries
-> claims, risks, conflicts, and opportunities
-> human-reviewed decisions
-> specifications
-> audit and implementation-readiness review
-> accepted task packet
-> implementation in the source repository
-> tests, completion report, deviations, and discoveries
-> governed completion-report amendment
-> governed current-state amendment
-> retrospective and contract improvement
```

This is a loop, not a one-way pipeline.

## Search-before-create and diff-before-write

Before creating a durable note:

1. search for an existing note covering the same subject;
2. identify whether the new content belongs as an amendment, supersedence, or separate note;
3. compare the intended content with existing canonical state;
4. preserve the reviewed difference.

Before canonical mutation:

- bind exact prior bytes;
- bind exact candidate bytes;
- bind the reviewed diff;
- bind the validation result;
- bind the plan and approval hashes.

## Validation layers

### Intrinsic note validation

Checks the note itself:

- UTF-8 and parseability;
- flat YAML;
- required fields;
- field types and enums;
- ID/type prefix consistency;
- required body sections;
- lifecycle and authority rules;
- prohibited private or structural content.

### Operation-context validation

Checks the proposed mutation in context:

- fixed roots;
- staging confinement;
- destination placement;
- canonical dependency existence;
- relationship and local-link resolution;
- source, prior, candidate, diff, and validation hashes;
- Git branch, HEAD, and clean-state binding;
- governance-log binding;
- packet and operation binding;
- approval freshness;
- filesystem safety.

Passing intrinsic validation alone does not authorize canonical mutation.

## Staging writes

Agent and tool writes occur only in the sibling staging root through constrained create-only or operation-specific tools.

Rules:

- fixed configured root;
- no arbitrary root override;
- no traversal, device path, UNC escape, symlink, junction, mount-point, or reparse escape;
- no silent overwrite;
- exact source and output hashes;
- immutable evidence after plan creation;
- staged evidence retained after canonical success.

## Promotion

Promotion creates a canonical note for the first time.

Required gates:

### Gate 1: plan generation approval

The owner explicitly approves one named operation for immutable plan generation.

Plan generation may create only reviewed operation evidence. It must not create approval evidence, canonical files, events, or Git commits.

### Gate 2: execution approval

The owner explicitly approves:

- operation ID;
- exact plan SHA-256;
- execution scope.

Execution requires the exact confirmation literal:

```text
PROMOTE-LIVE-EXACTLY-ONCE
```

### Promotion evidence

A first-time promotion binds:

- source bytes;
- canonical candidate bytes;
- validation result;
- destination;
- note identity and type;
- lifecycle and authority transition;
- required dependencies;
- Git and governance-log state;
- plan and approval hashes.

## Amendment

Amendment changes one existing canonical Markdown note without exposing generalized overwrite permission.

An amendment operation preserves and binds:

- exact prior canonical bytes;
- original staged candidate bytes;
- normalized canonical candidate bytes;
- reviewed unified diff;
- validation result;
- change summary;
- note continuity fields;
- plan hash;
- approval hash;
- operation-context state.

### Continuity

The first amendment slice preserves:

- note ID;
- note type;
- project;
- created timestamp;
- review status unless an approved transition is explicitly supported;
- authority unless an approved transition is explicitly supported.

Generalized overwrite, delete, move, correction, supersedence, rollback execution, and multi-note amendment transactions remain prohibited unless separately designed and accepted.

## Event log

Canonical event log:

```text
_governance/promotion-log.jsonl
```

The log is append-only.

Each consequential operation begins with one durable `intent` event before canonical mutation and ends with exactly one successful terminal event.

Proven action values:

```text
promote
amend
```

Future action values such as `supersede`, `correct`, and `rollback` remain unimplemented even if reserved in planning specifications.

### Required event binding

Consequential events bind at least:

- schema version;
- event ID;
- operation ID;
- phase;
- action;
- note ID and type;
- project;
- destination;
- approved human actor and timestamp;
- validation run;
- plan SHA-256;
- approval SHA-256;
- approved content SHA-256;
- concise basis.

Amendment events additionally bind:

- prior content SHA-256;
- new content SHA-256;
- reviewed change summary;
- reviewed diff evidence.

Historical JSONL lines are never rewritten.

### Recovery terminology

Existing event evidence must remain valid.

The accepted spec update must define one canonical mapping among:

```text
committed
recovered
recovered_committed
```

Until reconciled, implementation and readers must preserve backward compatibility and must not rename historical events.

## Filesystem and atomicity rules

Canonical mutation must use handle-relative, fixed-root, same-volume operations.

Required properties:

- parent paths opened and verified without following reparse escapes;
- temporary file created as an owned sibling;
- exact candidate hash verified before install;
- prior canonical hash verified immediately before amendment replacement;
- native no-replacement install for first promotion;
- native same-path atomic replacement for amendment;
- installed identity, size, and SHA-256 verified after mutation;
- failure and interruption leave deterministic recoverable evidence.

Hammer tests are a hard gate for write boundaries.

## Human authority

No agent, connector, or tool may infer owner approval.

Approval for one operation, plan, wave, note, or action does not authorize another.

The first slice uses a trusted local human actor string such as:

```text
owner.local
```

This is an explicit limitation, not a complete authentication system. Durable identity and authentication remain future security work.

## Agent roles and permissions

Agents may:

- read approved roots;
- search before creating;
- draft and validate staging content;
- generate immutable plans after owner approval;
- inspect operations and evidence;
- propose recommendations.

Agents may not:

- approve on the owner's behalf;
- set human review verdicts or accepted authority;
- mutate canonical Markdown outside an approved operation;
- commit, push, reset, clean, stash, or create remotes;
- use arbitrary SQL or direct database credentials;
- publish externally;
- widen task scope.

## MCP and operator surfaces

Typed tools should expose narrow operations rather than shell or generic filesystem access.

Expected tool families:

```text
health
validate
operation status
prepare constrained candidate
plan promotion
approve promotion
execute promotion
recover promotion
plan amendment
approve amendment
execute amendment
recover amendment
```

Connector mutation is opportunistic, not guaranteed. Upstream platform safety layers may block a call before Kaizen receives it.

The guaranteed fallback is a fixed-root local human operator using the same platform enforcement code.

A minimal local console should display:

- operation ID;
- packet ID;
- plan hash;
- prior hash;
- candidate hash;
- reviewed diff hash;
- approval state;
- execution state.

It should expose only exact approve, execute-once, and recover actions.

## Git evidence

Before consequential mutation:

- required repositories are on the expected branch and HEAD;
- working trees are clean except for explicitly allowed recovery paths;
- repository state is bound into the immutable plan where required.

After mutation:

- verify exact changed paths;
- stage only authorized paths;
- create a local commit with reviewed message;
- do not push or create a vault remote unless separately authorized.

Git does not replace the append-only governance event log.

## Implementation return

A task packet is not complete merely because code exists.

Required return path:

1. implement in the source repository;
2. run focused, integration, regression, and hammer tests as applicable;
3. create security and steward evidence;
4. prepare a governed amendment candidate for the task-packet completion report;
5. plan, review, approve, and execute that amendment;
6. create a scoped local vault commit;
7. prepare and execute a separate current-state amendment;
8. audit the complete return loop;
9. close the milestone explicitly.

Completion reports are evidence, not authority.

## Spec and decision maturity

Planning files must distinguish:

```text
draft
accepted planning contract
implemented baseline
superseded or amended
```

A spec marked draft must not be described elsewhere as accepted without an explicit decision or amendment.

Milestone 5 must reconcile stale maturity labels in the first-slice registries and workflow specs.

## Governance proportionality

Strict controls remain required for:

- canonical mutation;
- authority transitions;
- immutable plans and approvals;
- exact hashes;
- event sequencing;
- filesystem confinement;
- Git scope;
- implementation-return updates.

Lower ceremony is appropriate for:

- read-only evidence gathering;
- local retrospective analysis;
- low-risk documentation cleanup that changes no accepted contract;
- concise status mirrors of one authoritative tracker.

Governance exists to protect consequential boundaries, not to create paperwork for sport.

## Plugins and human interface

No Obsidian community plugin is required for canonical operation.

Deferred or optional capabilities:

- Dataview as a human read model;
- Templater after repeated authoring friction;
- Commander after stable human workflows;
- Tasks, Canvas, Kanban, and Excalidraw as non-canonical aids;
- a progressive hybrid interface after workflow evidence;
- a minimal local operator console as the first justified interface slice.

## Deferred systems

The following remain deferred until a concrete blocker or approved workload earns them:

```text
Operational Postgres
Observatory domains
Internet Marketing Intelligence database
Qdrant
Hermes live integration
production Kaizen MCP
broad Tauri application shell
DataForSEO and other providers
website automation
LangGraph or multi-agent orchestration
```

A minimal operator console does not authorize a broad application shell.

## Backup and remote posture

A canonical or platform repository may operate locally without a remote only when the owner explicitly accepts that temporary risk.

Required during a local-only period:

- separate Git repository;
- clean working tree at checkpoints;
- exact local commits;
- no accidental remote creation;
- documented future backup/visibility checkpoint.

The owner must separately decide public/private visibility and remote timing.

## Project bootstrap

A new project begins with only:

```text
command-center.md
overview.md
current-state.md
```

Additional folders and note types are earned by real work.

The project must record:

- purpose and scope;
- current stage;
- source-of-truth boundaries;
- current blockers;
- next action;
- do-not-touch boundaries.

## Change control

Changes to governance-sensitive fields, note types, authority transitions, event schemas, mutation workflows, source-of-truth boundaries, or agent permissions require:

1. evidence or concrete friction;
2. a proposed decision or amendment;
3. specification updates;
4. validation and test updates;
5. human review;
6. explicit acceptance;
7. compatibility and migration treatment.

Historical evidence remains immutable.

## v0.2 acceptance gates

This draft may become authoritative only after:

1. Result 063 retrospective findings are reconciled;
2. amendment event and workflow specs are updated or explicitly deferred with rationale;
3. spec maturity labels are reconciled;
4. the draft is audited against Decisions 0001 through 0012 and implementation evidence;
5. contradictions and stale assumptions are corrected;
6. the owner explicitly accepts the standard;
7. the accepted file and supersedence posture for the v0.1 baseline are recorded.

## Recommended next milestone family

After v0.2 acceptance, the evidence-backed next implementation family is:

```text
governed operator surface and evidence ergonomics
```

Candidate scope:

- amendment-native MCP tools;
- minimal local approve/execute/recover console;
- operation status and evidence inspection;
- spec and schema reconciliation tests;
- status-drift and index-integrity checks;
- actor-identity design research;
- backup and remote decision checkpoint.

No next implementation milestone is authorized by this draft alone.
