# Packet 023H — Resume View and Context-Pack Assembly

Status: planning packet draft — not owner accepted
Date: 2026-06-29
Milestone: 15
Packet: 023H

## Purpose

Define the next roadmap-consistent M15 packet after Packet 023G completed review-item note and derived review queue projection proof.

Packet 023H should prove fresh-context resume information and basic governed context-pack assembly from canonical Markdown records and typed projections. The goal is to make project continuation reliable for humans and LLMs without treating generated bundles, plugin output, Qdrant, Tauri state, or agent memory as canonical truth.

This packet is narrow on purpose: resume view and context-pack assembly only. Integrated M15 proof and closure audit remain Packet 023I.

## Authority basis

```text
05-specs/milestone-15-dynamic-obsidian-experience-and-typed-read-model.md
03-research-results/384-m15-research-synthesis-and-tauri-aligned-direction.md
03-research-results/386-m15-definition-spec-owner-acceptance.md
03-research-results/387-packet-023d-m15-note-schema-and-parser-direction.md
03-research-results/388-packet-023d-owner-acceptance.md
03-research-results/389-packet-023e-m15-schema-contract-and-parser-prototype.md
03-research-results/390-packet-023e-implementation-return.md
03-research-results/391-packet-023f-current-state-and-command-center-contracts.md
03-research-results/392-packet-023f-implementation-return.md
03-research-results/393-packet-023g-review-item-notes-and-review-queue-views.md
03-research-results/394-packet-023g-implementation-return.md
04-design-decisions/0003-raw-markdown-is-canonical.md
04-design-decisions/0011-progressive-hybrid-human-interface-direction.md
ROADMAP_V0.4.md
```

## Starting repository posture

Before implementation, verify:

```text
docs repository is clean and synced;
platform repository is clean;
platform starting commit is 1fb78418cbefb526e05eaf851bec5bff536371dc or explicitly updated in the approval;
vault repository is clean and not mutated;
staging root is not mutated unless explicitly added later.
```

## Implementation objective

Create a narrow platform demonstration that proves resume notes and context packs can be derived from parsed M15 Markdown records.

The implementation should:

```text
validate resume note fixtures with the existing M15 read model;
derive a resume view from valid typed records;
derive a context-pack manifest from valid typed records;
include current-state, command-center, active review items, and newest implementation return when present;
exclude invalid records by default;
keep derived output JSON-compatible;
write no generated context-pack files unless separately approved;
avoid mutating canonical vault notes.
```

## Resume view contract direction

A resume view is the fresh-context continuation surface for a governed project.

It should answer:

```text
What is the current mission?
What critical context must be preserved?
Which accepted decisions are still in force?
What active work is underway?
What blockers exist?
What exact path should a human or LLM read next?
```

It is not:

```text
a chat transcript;
a full evidence archive;
a generated source of truth;
a memory replacement;
a plugin-only view;
a Qdrant result;
a Tauri state file;
a substitute for current-state.md, command-center.md, accepted decisions, specs, task packets, or implementation returns.
```

## Context-pack contract direction

A context pack is a governed, derived bundle for a specific project and task.

It should include only enough approved project context to safely continue work.

Recommended manifest fields:

```text
project
purpose
source_count
included_paths
excluded_paths
records_with_errors
current_state_path
command_center_path
resume_path
active_review_items
latest_implementation_return_path
```

A context pack may include:

```text
project boundary;
current-state note;
command-center note;
resume note;
accepted decisions still in force;
active relevant specs or task packets;
open review items when relevant;
freshest implementation return when relevant.
```

A context pack must exclude:

```text
raw evidence dumps;
unrelated projects;
superseded material unless explicitly requested;
speculative drafts without authority unless relevant to the task and clearly marked;
private notes outside boundary;
plugin-render-only artifacts;
records with validation errors unless explicitly requested and clearly marked.
```

## Frontmatter contract

Resume notes should use the M15 common frontmatter:

```text
type
project
status
authority_level
created
updated
```

Optional fields may include:

```text
owner
related
source_docs
tags
last_reviewed
```

Rules:

```text
frontmatter stays flat;
body sections carry continuation context;
resume content must remain useful in raw Markdown;
no nested YAML;
no plugin-specific serialized state;
no generated truth marker that replaces human review.
```

## Required body headings

Use these exact H2 headings for resume notes:

```text
Current mission
Critical context
Accepted decisions in force
Active work
Blockers
Read-next path
```

No aliases should be accepted in the first 023H implementation. Aliases can be proposed later only if real authoring friction proves the need.

## Derived assembly direction

023H should add small pure functions in the existing M15 read-model module if the implementation remains simple.

Recommended functions:

```text
build_resume_view(records, project)
build_context_pack_manifest(records, project, purpose)
```

Recommended behavior:

```text
include only records for the requested project;
exclude records with validation errors by default;
select current-state, command-center, and resume records by type;
include active review items using the existing review queue projection;
select latest implementation-return by updated timestamp, then path;
return JSON-compatible dictionaries only;
do not write generated files;
do not create a CLI, service, database table, or Qdrant integration.
```

If implementation needs more than a small pure projection, stop and return for a narrower packet rather than growing a premature context-pack subsystem.

## Recommended implementation scope

### Platform paths to update

```text
src/kaizen/m15_read_model.py
tests/test_m15_read_model.py
```

Allowed only if needed:

```text
src/kaizen/__init__.py
pyproject.toml
```

`src/kaizen/__init__.py` may be updated only if exporting the read-model module becomes necessary.

`pyproject.toml` should not need changes. Add no dependency unless the implementer proves an existing approved parser cannot support the contract.

### Platform paths to create

```text
tests/fixtures/m15-read-model/valid-resume.md
tests/fixtures/m15-read-model/invalid-resume-missing-read-next-path.md
tests/fixtures/m15-read-model/valid-implementation-return.md
```

Optional only if needed for clearer proof:

```text
tests/fixtures/m15-read-model/valid-decision.md
tests/fixtures/m15-read-model/valid-task-packet.md
```

Existing current-state, command-center, and review-item fixtures may be reused.

## Explicitly excluded paths

Implementation must not mutate:

```text
vault/*
staging/*
database state
src/kaizen/ops_db/*
src/kaizen/ops_service/*
src/kaizen/ops_recovery/*
existing amendment/promotion/project-bootstrap workflow modules
Obsidian .base files
Obsidian .obsidian config
Tauri files
Qdrant files
Hermes / agent integration files
Neon Ronin repository
SearchClarity repository
```

## Validation behavior to add or prove

023H implementation should prove:

```text
valid resume fixture satisfies the contract;
missing required resume heading fails deterministically;
context-pack manifest includes only requested project records;
context-pack manifest excludes invalid records by default;
resume view and context-pack manifest remain JSON-compatible;
latest implementation-return selection is deterministic;
active review items reuse the derived review queue projection;
existing 023E, 023F, and 023G tests remain passing;
input fixtures are not mutated.
```

If implementing full context-pack file writing, search, retrieval, token budgeting, Qdrant, or source bundling becomes necessary, stop and defer to later milestone work rather than inflating 023H.

## Test requirements

Minimum focused command:

```text
python -m pytest tests/test_m15_read_model.py
```

Recommended collateral regression:

```text
python -m pytest tests/test_markdown.py tests/test_validation.py tests/test_m15_read_model.py
```

Full platform test suite is optional unless shared modules beyond `src/kaizen/m15_read_model.py` are changed.

## Acceptance criteria

Packet 023H implementation is complete only if:

```text
approved path scope is respected;
resume note contract is demonstrated;
context-pack manifest is derived from parsed Markdown records;
manifest output is deterministic and JSON-compatible;
fixtures exist only in approved fixture paths;
no generated context-pack files are written;
no canonical vault files are mutated;
no Obsidian plugin files are created;
all required tests pass;
implementation return records exact changed paths, tests run, residuals, and next recommendation.
```

## Stop conditions

Stop immediately if:

```text
platform working tree is dirty before implementation;
implementation requires vault mutation;
implementation requires staging mutation;
implementation requires database mutation;
implementation requires broad service or CLI creation;
implementation requires new heavy dependencies;
implementation treats generated context-pack output as canonical truth;
implementation drifts into Qdrant or semantic retrieval;
implementation drifts into Tauri;
implementation grants Hermes or agents write authority;
implementation creates Obsidian Bases, DataviewJS, Canvas, or .obsidian config;
context packs include unrelated/private/superseded material without explicit request;
fixtures contain secrets, credentials, or private customer data;
validation semantics conflict with Decision 0003 raw-Markdown authority.
```

## Non-authorization

This draft packet does not authorize implementation until explicitly owner accepted.

Even if accepted, it does not authorize:

```text
vault mutation;
staging mutation;
database mutation;
production database mutation;
Tauri implementation;
Qdrant implementation;
Hermes write authority;
Obsidian MCP write authority;
Obsidian REST write authority;
Obsidian Bases implementation;
DataviewJS dependency;
file-writing context-pack generation;
semantic retrieval;
source bundling;
broad platform architecture changes;
provider purchase;
customer-data reuse;
logged-in scraping;
Observatory / IMI implementation;
commercial operations;
production deployment;
full Neon Ronin implementation;
full SearchClarity implementation;
backup deletion;
restore work.
```

## Owner acceptance recommendation

Recommended owner action after review:

```text
Accept Packet 023H for narrow platform implementation at platform starting commit 1fb78418cbefb526e05eaf851bec5bff536371dc, with exact path scope and tests as listed.
```

After implementation return, the next roadmap packet should be:

```text
023I — M15 Integrated Proof and Closure Audit
```
