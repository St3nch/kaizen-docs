# Packet 023F — Current-State and Command-Center Contracts

Status: planning packet draft — not owner accepted
Date: 2026-06-29
Milestone: 15
Packet: 023F

## Purpose

Define the next roadmap-consistent M15 packet after the completed Packet 023E parser/validator prototype.

Packet 023F should establish the `current-state.md` and `command-center.md` contracts that humans, agents, Obsidian-derived views, and later Tauri clients can rely on without creating a second source of truth.

This packet is intentionally narrow: make the two primary operating notes boring, readable, validated, and useful before building review queues, resume views, context packs, Bases, Dataview panels, or Tauri surfaces.

## Authority basis

```text
05-specs/milestone-15-dynamic-obsidian-experience-and-typed-read-model.md
03-research-results/384-m15-research-synthesis-and-tauri-aligned-direction.md
03-research-results/386-m15-definition-spec-owner-acceptance.md
03-research-results/387-packet-023d-m15-note-schema-and-parser-direction.md
03-research-results/388-packet-023d-owner-acceptance.md
03-research-results/389-packet-023e-m15-schema-contract-and-parser-prototype.md
03-research-results/390-packet-023e-implementation-return.md
04-design-decisions/0003-raw-markdown-is-canonical.md
04-design-decisions/0011-progressive-hybrid-human-interface-direction.md
ROADMAP_V0.4.md
```

## Starting repository posture

Before implementation, verify:

```text
docs repository is clean and synced;
platform repository is clean;
platform starting commit is 2820bdb94a8eb580ae79ad797c61bd6422649659 or explicitly updated in the approval;
vault repository is clean and not mutated;
staging root is not mutated unless explicitly added later.
```

## Implementation objective

Create a narrow platform demonstration that proves the M15 read-model prototype can validate current-state and command-center contracts from Markdown fixtures.

The implementation should:

```text
define or encode the current-state and command-center heading contracts;
add representative valid fixtures for current-state and command-center notes;
add negative fixtures for stale/missing operational sections where useful;
assert parser output stays JSON-compatible;
assert the notes remain readable as raw Markdown;
assert human-only dynamic sections remain explicitly derived/non-canonical;
avoid mutating canonical vault notes.
```

## Contract direction

### current-state.md role

`current-state.md` is the short, curated, plain-Markdown truth surface for one governed project.

It should answer:

```text
What is true now?
What is the current objective?
What constraints and decisions are active?
What is blocked?
What should happen next?
What should a fresh LLM or human read next?
```

It is not:

```text
a full governance history;
a raw implementation-return archive;
a backup hash ledger;
a generated status dump;
a live operational queue;
a rendered plugin view;
a substitute for Git history, staging evidence, or operational records.
```

### command-center.md role

`command-center.md` is the human cockpit and agent entrypoint for one governed project.

Its top section must be useful in raw Markdown before any Obsidian rendering.

It should answer:

```text
Where does this project start?
What is the current state pointer?
What action is next?
What is blocked?
How does a human or LLM resume safely?
Which dynamic panels are human convenience only?
```

It is not:

```text
a second current-state note;
a hidden approval surface;
a plugin-owned source of truth;
a broad automation dashboard;
a place to bury full audit chains or raw evidence.
```

## Frontmatter contract

Both note types should use the M15 common frontmatter established by 023D / 023E:

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
frontmatter carries routing and lifecycle facts only;
body sections carry narrative context;
no nested YAML;
no plugin-specific serialized state;
no hidden approval field;
no generated truth marker that replaces human review.
```

## Required body headings

### current-state

Use these exact H2 headings:

```text
Summary
Current objective
Current constraints
Active decisions
Blockers
Next actions
Read next
```

### command-center

Use these exact H2 headings:

```text
LLM entry point
Current state
Next actions
Blockers
Resume path
Human dynamic panels
```

No aliases should be accepted in the first implementation. Aliases can be proposed later only if real authoring friction proves the need.

## Human dynamic panels rule

The `Human dynamic panels` section may describe or embed human convenience views, but it must clearly preserve this rule:

```text
Dynamic panels are derived convenience views only. They are not canonical truth, not approval records, and not a substitute for current-state.md or governed records.
```

023F should not create broad `.base` files, DataviewJS, Canvas layout, Obsidian config, or plugin-dependent meaning.

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
tests/fixtures/m15-read-model/valid-command-center.md
tests/fixtures/m15-read-model/invalid-command-center-missing-human-dynamic-panels.md
```

Optional only if needed for clearer proof:

```text
tests/fixtures/m15-read-model/invalid-current-state-dynamic-truth.md
tests/fixtures/m15-read-model/invalid-command-center-hidden-approval.md
```

The implementation should avoid generating snapshots unless separately approved.

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

023F implementation should prove:

```text
valid current-state fixture satisfies the contract;
valid command-center fixture satisfies the contract;
missing command-center required heading fails deterministically;
command-center dynamic panels section is represented in heading output;
parser output remains JSON-compatible;
input fixtures are not mutated;
unknown fields continue deterministic behavior;
existing 023E current-state and review-item tests remain passing.
```

If implementing checks for hidden approval or generated-truth language would require brittle prose policing, defer that to a later lint/audit packet rather than overfitting the parser.

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

Packet 023F implementation is complete only if:

```text
approved path scope is respected;
current-state and command-center contracts are encoded or demonstrated;
fixtures exist only in approved fixture paths;
required heading behavior is tested;
validation output is deterministic;
output remains JSON-compatible;
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
implementation requires broad rewrites of existing platform modules;
implementation requires new heavy dependencies;
implementation drifts into Tauri;
implementation drifts into Qdrant;
implementation grants Hermes or agents write authority;
implementation creates Obsidian Bases, DataviewJS, Canvas, or .obsidian config;
current-state truth becomes unreviewed auto-generated output;
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
Accept Packet 023F for narrow platform implementation at platform starting commit 2820bdb94a8eb580ae79ad797c61bd6422649659, with exact path scope and tests as listed.
```

After implementation return, the next roadmap packet should be:

```text
023G — Review-Item Notes and Review Queue Views
```
