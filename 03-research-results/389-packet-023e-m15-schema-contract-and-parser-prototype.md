# Packet 023E — M15 Schema Contract and Parser Prototype

Status: implementation packet draft — not owner accepted
Date: 2026-06-29
Milestone: 15
Packet: 023E

## Purpose

Define the first narrow implementation packet for Milestone 15.

Packet 023E should create the first M15 schema contract and parser/validator prototype without mutating the canonical vault, building Tauri, adding Qdrant, or granting agent write authority.

This packet is intentionally small: prove the typed read-model direction with fixtures before broader Obsidian views, command centers, review queues, resume views, or context packs are implemented.

## Authority basis

```text
05-specs/milestone-15-dynamic-obsidian-experience-and-typed-read-model.md
03-research-results/384-m15-research-synthesis-and-tauri-aligned-direction.md
03-research-results/385-m15-definition-spec-audit.md
03-research-results/386-m15-definition-spec-owner-acceptance.md
03-research-results/387-packet-023d-m15-note-schema-and-parser-direction.md
03-research-results/388-packet-023d-owner-acceptance.md
04-design-decisions/0003-raw-markdown-is-canonical.md
04-design-decisions/0011-progressive-hybrid-human-interface-direction.md
```

## Starting repository posture

Before implementation, verify:

```text
docs repository is clean and synced;
platform repository is clean;
platform starting commit is 7daabf3eff0b3b0768e88512ca7d596c94e41140 or explicitly updated in the approval;
platform has no remote;
vault repository is not mutated;
staging repository is not mutated unless explicitly added later.
```

## Implementation objective

Create a Python-first M15 parser/validator prototype in the platform repository that can:

```text
read Markdown files with flat YAML frontmatter;
parse frontmatter into a JSON-compatible typed record;
extract top-level / required body headings;
validate note type, status, review_state, authority_level, and required fields;
validate required headings for selected first-slice note types;
report deterministic validation errors without mutating input files;
return output that a future TypeScript/Tauri client can consume.
```

## Exact proposed path scope

### Platform paths to create

```text
src/kaizen/m15_read_model.py
tests/test_m15_read_model.py
tests/fixtures/m15-read-model/valid-current-state.md
tests/fixtures/m15-read-model/valid-review-item.md
tests/fixtures/m15-read-model/invalid-nested-frontmatter.md
tests/fixtures/m15-read-model/invalid-missing-heading.md
tests/fixtures/m15-read-model/invalid-enum.md
```

### Platform paths allowed to update if necessary

```text
src/kaizen/__init__.py
pyproject.toml
```

`src/kaizen/__init__.py` may be updated only if exporting the new module is needed.

`pyproject.toml` may be updated only if a required parser dependency is already not available and the dependency is small, stable, and justified. Prefer no new dependency if existing parsing helpers are sufficient.

### Docs paths to update after implementation return

```text
00-entrypoint/LLM_START_HERE.md
ROADMAP_V0.4.md
03-research-results/<next-result>-packet-023e-implementation-return.md
```

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
Obsidian `.base` files
Obsidian `.obsidian` config
Tauri files
Qdrant files
Hermes / agent integration files
Neon Ronin repository
SearchClarity repository
```

## First-slice note types

The prototype should support these note types first:

```text
current-state
command-center
review-item
resume
implementation-return
```

The prototype may define enum placeholders for the broader first-slice set below, but tests should stay focused:

```text
project
decision
spec
audit
task-packet
```

Deferred:

```text
claim
source-summary
raw-source
context-pack
observatory-signal
experiment
opportunity
```

## Required common frontmatter fields

For typed M15 notes:

```text
type
project
status
authority_level
created
updated
```

Optional common fields:

```text
review_state
effective_date
supersedes
superseded_by
owner
related
source_docs
tags
last_reviewed
```

Frontmatter rules:

```text
flat mapping only;
no nested maps;
no long prose fields;
no plugin-rendered canonical fields;
no hidden approval fields;
unknown fields produce warnings or errors as defined by the prototype, but must be deterministic.
```

## Enum values

### `status`

```text
draft
proposed
ready
in-review
active
accepted
rejected
superseded
archived
```

### `review_state`

```text
none
proposed
ready
in-review
approved
rejected
superseded
```

### `authority_level`

Use this reduced set to avoid ambiguity between `human-approved` and `accepted`:

```text
draft
proposed
accepted
superseded
archived
```

Rationale:

```text
Human approval is expressed by governance events and accepted notes, not by a second frontmatter authority value.
```

## Required headings for prototype tests

### `current-state`

```text
Summary
Current objective
Current constraints
Active decisions
Blockers
Next actions
Read next
```

### `review-item`

```text
Proposal
Evidence
Checks run
Decision requested
Approval record
```

### `implementation-return`

```text
Summary
Scope completed
Evidence
Tests / checks
Changed paths
Residuals
Recommended next action
```

The prototype may define heading contracts for `command-center` and `resume`, but tests can focus on `current-state`, `review-item`, and negative cases.

## Typed record shape

The parser should produce a simple JSON-compatible structure equivalent to:

```text
{
  "path": "relative/path.md",
  "frontmatter": { ...flat key/value pairs... },
  "type": "current-state",
  "project": "example-project",
  "status": "active",
  "authority_level": "accepted",
  "headings": ["Summary", "Current objective", ...],
  "errors": [],
  "warnings": []
}
```

The Python implementation may use dataclasses or typed dictionaries internally, but the public output must be serializable to plain JSON-compatible dictionaries/lists/scalars.

## Validation behavior

The implementation must deterministically report at least these failures:

```text
missing required frontmatter field;
invalid enum value;
nested frontmatter mapping;
missing required heading;
missing closing frontmatter fence;
unsupported note type;
review_state misuse if review_state is required or forbidden for a given type.
```

No validation error should mutate the file being read.

## Test requirements

Minimum tests:

```text
valid current-state note parses with zero errors;
valid review-item note parses with zero errors;
nested frontmatter fixture reports nested frontmatter error;
missing required heading fixture reports missing heading error;
invalid enum fixture reports invalid enum error;
parser output is JSON-serializable;
input files are not mutated;
unknown fields produce deterministic warning/error behavior.
```

Recommended command:

```text
uv run pytest tests/test_m15_read_model.py
```

Also run the existing focused regression most likely to catch markdown/parser collateral damage:

```text
uv run pytest tests/test_markdown.py tests/test_validation.py tests/test_m15_read_model.py
```

Full platform test suite is desirable if runtime is acceptable, but not required for the first prototype unless the implementer touches shared validation or markdown modules.

## Acceptance criteria

Packet 023E implementation is complete only if:

```text
all approved paths are respected;
new parser/validator prototype exists in the exact approved platform path;
fixtures exist only in the approved fixture path;
required enum and heading behavior is tested;
validation output is deterministic;
output is JSON-compatible;
no canonical vault files are mutated;
no generated snapshot is treated as canonical;
all required tests pass;
implementation return records exact changed paths, tests run, residuals, and next recommendation.
```

## Stop conditions

Stop immediately if:

```text
platform working tree is dirty before implementation;
implementation requires vault mutation;
implementation requires database mutation;
implementation requires staging mutation not listed in approval;
implementation requires broad rewrites of existing platform modules;
implementation requires new heavy dependencies;
implementation drifts into Tauri;
implementation drifts into Qdrant;
implementation grants Hermes or agents write authority;
implementation creates Obsidian Bases or `.obsidian` config;
fixtures contain secrets or private customer data;
validation semantics conflict with Decision 0003 raw-Markdown authority.
```

## Non-authorization

This draft packet does not authorize implementation until explicitly owner accepted.

Even if accepted, it does not authorize:

```text
vault mutation;
staging mutation unless separately added;
database mutation;
production database mutation;
Tauri implementation;
Qdrant implementation;
Hermes write authority;
Obsidian MCP write authority;
Obsidian REST write authority;
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
Accept Packet 023E for narrow platform implementation at platform starting commit 7daabf3eff0b3b0768e88512ca7d596c94e41140, with exact path scope and tests as listed.
```
