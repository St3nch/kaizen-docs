# Packet 023D — M15 Note Schema and Parser Direction

Status: planning packet draft — not owner accepted
Date: 2026-06-29
Milestone: 15
Packet: 023D

## Purpose

Define the first implementation-bearing M15 packet before any implementation begins.

Packet 023D should establish the schema and parser direction that later M15 implementation packets will use for dynamic Obsidian views, typed read-model projection, review queues, resume views, and governed context-pack assembly.

This packet exists to prevent M15 from starting with UI decoration or plugin sprawl before the canonical note contract exists.

## Authority basis

```text
05-specs/milestone-15-dynamic-obsidian-experience-and-typed-read-model.md
03-research-results/384-m15-research-synthesis-and-tauri-aligned-direction.md
03-research-results/385-m15-definition-spec-audit.md
03-research-results/386-m15-definition-spec-owner-acceptance.md
04-design-decisions/0003-raw-markdown-is-canonical.md
04-design-decisions/0011-progressive-hybrid-human-interface-direction.md
ROADMAP_V0.4.md
```

## Recommended implementation objective

Create a bounded M15 schema-and-parser-direction package that defines:

```text
exact M15 note types
flat frontmatter fields
allowed enum values
required body headings
validation rules
pilot corpus / fixture target
generated snapshot posture
parser language direction
first-pass typed record shape
non-authorization boundaries
```

The output should be enough for a later implementation packet to build a parser/validator without arguing about note semantics.

## Recommended implementation scope

### 1. Note-type registry slice

Define the minimum M15 note-type set for the first parser contract.

Recommended first slice:

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
```

Recommended deferred note types for later M15 or later milestones:

```text
claim
source-summary
raw-source
context-pack
observatory-signal
experiment
opportunity
```

Reason:

```text
The first M15 parser should prove operational navigation, review, resume, and implementation-return continuity before expanding into research/intelligence ontology.
```

### 2. Common frontmatter contract

Define required common fields for M15-typed notes:

```text
type
project
status
authority_level
created
updated
```

Define optional common fields:

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

Rules:

```text
frontmatter must be flat;
frontmatter must not contain long prose;
frontmatter must not contain nested objects;
frontmatter must not contain plugin-specific serialized data;
frontmatter relationship fields must use stable relative Markdown paths or accepted Kaizen IDs as defined by the packet;
body sections carry narrative and reasoning.
```

### 3. Enum contract

Define exact allowed enum values for:

```text
type
status
review_state
authority_level
```

Recommended initial values:

```text
status:
  draft
  proposed
  ready
  in-review
  active
  accepted
  rejected
  superseded
  archived

review_state:
  none
  proposed
  ready
  in-review
  approved
  rejected
  superseded

authority_level:
  draft
  proposed
  human-approved
  accepted
  superseded
  archived
```

Implementation note:

```text
The packet should reconcile whether `accepted` and `human-approved` are both needed, or whether one should be removed to reduce ambiguity.
```

### 4. Required body-heading conventions

Define body-heading contracts for each first-slice note type.

Recommended minimum:

```text
current-state.md:
  Summary
  Current objective
  Current constraints
  Active decisions
  Blockers
  Next actions
  Read next

command-center.md:
  LLM entry point
  Current state
  Next actions
  Blockers
  Resume path
  Human dynamic panels

review-item:
  Proposal
  Evidence
  Checks run
  Decision requested
  Approval record

resume:
  Current mission
  Critical context
  Accepted decisions in force
  Active work
  Blockers
  Read-next path

implementation-return:
  Summary
  Scope completed
  Evidence
  Tests / checks
  Changed paths
  Residuals
  Recommended next action
```

The implementation packet should define exact heading names and whether aliases are allowed.

### 5. Validation rules

Define validation rules before code implementation.

Recommended rules:

```text
required frontmatter present for typed notes;
type is one accepted enum value;
status is one accepted enum value;
authority_level is one accepted enum value;
review_state is required only for review-item notes unless explicitly allowed elsewhere;
required headings are present for each note type;
frontmatter is flat;
no nested maps in frontmatter;
no plugin-only canonical fields;
relative links or accepted IDs are valid where required;
superseded notes point forward when superseded_by is present;
accepted notes cannot depend on draft-only authority without explicit exception;
generated snapshots are marked derived if produced.
```

### 6. Pilot corpus / fixture target

Choose one safe pilot target for schema validation.

Recommended starting target:

```text
docs-only synthetic fixture under platform tests or staging evidence, not canonical vault mutation.
```

Reason:

```text
M15 must prove schema/parser behavior without mutating the canonical vault before the contract is stable.
```

Acceptable alternatives:

```text
small docs fixture in the platform repo;
small staging fixture under C:\dev\kaizen\staging;
read-only scan of selected docs files with no mutation.
```

Not recommended for 023D implementation:

```text
canonical vault mutation;
full vault migration;
creating broad Obsidian Bases files;
rewriting project command centers;
rewriting historical decisions/specs.
```

### 7. Generated snapshot posture

Define whether typed read-model output is:

```text
ephemeral command output only;
JSON snapshot files in a generated/artifacts area;
both, with generated files clearly non-canonical.
```

Recommended first posture:

```text
ephemeral output first;
optional JSON fixture output under a clearly generated, non-canonical test-artifact path.
```

Generated snapshots must never be treated as canonical truth.

### 8. Parser language direction

Recommended first direction:

```text
Python-first parser/validator in platform, because Kaizen's existing operational foundation and tests are Python-heavy.
```

Tauri alignment requirement:

```text
The output contract must remain TypeScript/Tauri-friendly by emitting simple JSON-compatible typed records with stable field names and no Python-only object semantics.
```

This keeps Tauri supported later without prematurely building a Tauri app.

## Recommended deliverables for 023D implementation

```text
one schema contract document or registry update;
one parser direction / typed record contract document;
one validation-rule checklist;
one pilot fixture plan;
no implementation unless separately included and accepted;
no vault mutation;
no Tauri implementation.
```

## Recommended files to create or update

Recommended docs-only planning output:

```text
05-specs/m15-note-schema-and-parser-direction.md
00-entrypoint/LLM_START_HERE.md
ROADMAP_V0.4.md
```

If the owner approves implementation in the same packet later, recommended platform work should be separately scoped and may include:

```text
platform parser module path to be determined;
platform tests for fixture parsing;
generated fixture output only if explicitly approved.
```

## Stop conditions

Stop if:

```text
scope expands into Tauri implementation;
scope expands into Qdrant;
scope grants Hermes or other agents write authority;
scope grants Obsidian MCP / REST / CLI write authority;
scope mutates canonical vault notes;
scope rewrites historical docs broadly;
scope creates broad `.base` files before schema/parser proof;
frontmatter schema becomes nested or prose-heavy;
enum values remain ambiguous after review;
parser language decision is used to block future Tauri compatibility;
repository state is dirty before mutation;
secrets or private credentials appear in generated artifacts.
```

## Non-authorization

This 023D planning packet draft does not authorize:

```text
implementation;
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
restore work.
```

## Owner acceptance recommendation

Recommended owner action after review:

```text
Accept Packet 023D as the first M15 implementation-packet planning direction, with implementation still requiring a separate exact approval.
```

The next accepted work should either:

```text
1. turn this draft into an accepted schema/parser-direction spec without code; or
2. separately authorize a narrow implementation packet that creates the schema contract and first parser/validator prototype.
```
