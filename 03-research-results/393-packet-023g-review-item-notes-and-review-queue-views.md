# Packet 023G — Review-Item Notes and Review Queue Views

Status: planning packet draft — not owner accepted
Date: 2026-06-29
Milestone: 15
Packet: 023G

## Purpose

Define the next roadmap-consistent M15 packet after Packet 023F completed current-state and command-center contract proof.

Packet 023G should prove review-item notes and review queue views as first-class Markdown-backed project operating surfaces. The goal is to make review work visible, typed, resumable, and human-owned without turning Obsidian plugin output, generated snapshots, or agent memory into authority.

This packet is narrow on purpose: review-item notes and review queue behavior only. Resume view and context-pack assembly remain Packet 023H. Integrated closure remains Packet 023I.

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
04-design-decisions/0003-raw-markdown-is-canonical.md
04-design-decisions/0011-progressive-hybrid-human-interface-direction.md
ROADMAP_V0.4.md
```

## Starting repository posture

Before implementation, verify:

```text
docs repository is clean and synced;
platform repository is clean;
platform starting commit is 9ba18e075deaca368c0869b636c318855491bb8a or explicitly updated in the approval;
vault repository is clean and not mutated;
staging root is not mutated unless explicitly added later.
```

## Implementation objective

Create a narrow platform demonstration that proves review-item notes can be parsed, validated, and surfaced as a deterministic review queue from Markdown fixtures.

The implementation should:

```text
preserve review-item notes as canonical Markdown inputs;
validate required review-item headings;
validate review_state enum behavior;
prove agents may prepare proposed or ready review artifacts;
prove human terminal states remain represented but not agent-owned;
derive a queue-like projection from parsed records;
sort queue output deterministically;
keep output JSON-compatible;
avoid mutating canonical vault notes.
```

## Review-item contract direction

Review items are first-class Markdown notes used to hold proposed decisions, approvals, rejections, or supersedence requests that require human review.

They are not:

```text
loose task checkboxes;
plugin-only tasks;
agent approvals;
chat memory;
raw implementation chatter;
Git commits;
operational database rows;
a substitute for accepted decisions, specs, or implementation returns.
```

A review item should answer:

```text
What is proposed?
What evidence supports it?
What checks were run?
What decision is requested from the human?
What approval/rejection record exists, if any?
```

## Review-state contract

The M15 minimum review state machine remains:

```text
proposed -> ready -> in-review -> approved | rejected | superseded
```

Rules:

```text
agents may prepare proposed review items;
agents may prepare ready review items when evidence/checks are present;
agents may not approve, reject, or supersede review items;
humans own approved, rejected, and superseded states;
in-review means a human review is actively underway or explicitly staged for review;
none remains valid only where review_state is optional or not applicable.
```

Implementation should not attempt to prove actor identity or permissions through frontmatter alone. It should prove the note contract and queue projection. Human authority remains governed by process and accepted records, not mutable YAML magic.

## Frontmatter contract

Review-item notes should use the M15 common frontmatter:

```text
type
project
status
authority_level
created
updated
review_state
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
review_state is required for review-item notes;
review_state must be one accepted enum value;
review_state must not imply approval authority by itself;
body sections carry proposal, evidence, checks, and approval/rejection details;
no nested YAML;
no plugin-specific serialized task state;
no hidden approval field.
```

## Required body headings

Use these exact H2 headings for review-item notes:

```text
Proposal
Evidence
Checks run
Decision requested
Approval record
```

No aliases should be accepted in the first 023G implementation. Aliases can be proposed later only if real authoring friction proves the need.

## Review queue projection direction

023G should add a derived queue projection over parsed review-item records.

The projection should be plain JSON-compatible data derived from Markdown records, not a canonical artifact.

Recommended queue item shape:

```json
{
  "path": "valid-review-item.md",
  "project": "example-project",
  "status": "ready",
  "review_state": "ready",
  "authority_level": "proposed",
  "errors": [],
  "warnings": []
}
```

Recommended queue behavior:

```text
include review-item records only;
exclude records with validation errors by default, or mark them clearly if included;
sort by review_state priority, then updated, then path;
prioritize in-review, ready, proposed, rejected, superseded, approved for human queue visibility;
return only JSON-compatible primitives;
do not write generated queue files unless separately approved.
```

The queue projection may live as a pure function in the existing M15 read-model module if small. Avoid creating a broad service, CLI, database table, or generated artifact path.

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
tests/fixtures/m15-read-model/valid-review-item-proposed.md
tests/fixtures/m15-read-model/valid-review-item-in-review.md
tests/fixtures/m15-read-model/valid-review-item-approved.md
tests/fixtures/m15-read-model/invalid-review-item-missing-review-state.md
tests/fixtures/m15-read-model/invalid-review-item-missing-approval-record.md
```

Optional only if needed for clearer proof:

```text
tests/fixtures/m15-read-model/valid-review-item-rejected.md
tests/fixtures/m15-read-model/valid-review-item-superseded.md
```

Existing `valid-review-item.md` from Packet 023E may remain as a valid ready-state fixture.

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

023G implementation should prove:

```text
valid review-item fixtures satisfy the contract;
review_state is required for review-item notes;
invalid review_state still fails deterministically through existing enum validation;
missing required review-item heading fails deterministically;
review queue projection includes only review-item records;
review queue projection sorts deterministically;
review queue projection remains JSON-compatible;
existing 023E and 023F tests remain passing;
input fixtures are not mutated.
```

If implementing actor-permission checks for agent versus human states would require brittle metadata or fake identity enforcement, defer that to a later authority/audit packet rather than pretending YAML is access control.

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

Packet 023G implementation is complete only if:

```text
approved path scope is respected;
review-item contract is demonstrated;
review queue projection is derived from parsed Markdown records;
queue output is deterministic and JSON-compatible;
fixtures exist only in approved fixture paths;
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
implementation treats generated queue output as canonical truth;
implementation drifts into resume view or context-pack assembly;
implementation drifts into Tauri;
implementation drifts into Qdrant;
implementation grants Hermes or agents write authority;
implementation creates Obsidian Bases, DataviewJS, Canvas, or .obsidian config;
review_state can be approved by an agent;
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
resume view implementation;
context-pack assembly implementation;
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
Accept Packet 023G for narrow platform implementation at platform starting commit 9ba18e075deaca368c0869b636c318855491bb8a, with exact path scope and tests as listed.
```

After implementation return, the next roadmap packet should be:

```text
023H — Resume View and Context-Pack Assembly
```
