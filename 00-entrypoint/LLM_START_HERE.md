# LLM Start Here

This is the first file every LLM should read when working in the `kaizen-docs` repository.

## What this repo is

`kaizen-docs` is the planning and stewardship repository for Kaizen.

Kaizen is a portable Markdown-first project intelligence system designed to move projects through this flow:

```text
idea capture -> research -> source summaries -> claims -> specs -> audit -> task packet -> coding agent build
```

This repository is **not** the future Obsidian vault. It is the design workspace used to research, plan, govern, and audit that future system.

## Accepted architecture

```text
Markdown / Obsidian = canonical project intelligence
Operational Postgres database = operational source of truth for governed structured records; Observatory is one bounded intelligence domain
Qdrant = rebuildable semantic retrieval index over approved Markdown
Hermes / agents = constrained clerks through approved typed tools
Humans = authority-bearing review, approval, and promotion
```

## Current project posture

- Current phase: implementation checkpoint remediation gate active; canonical-promotion approval is blocked pending Result 025 closure
- Planning roadmap: historical planning source plus current remediation gate at `ROADMAP.md`
- Active implementation roadmap: `IMPLEMENTATION_ROADMAP.md`
- Checkpoint audit evidence: `03-research-results/024-implementation-checkpoint-red-team-audit-claude-summary.md`
- Governing remediation ledger: `03-research-results/025-implementation-checkpoint-audit-steward-reconciliation.md`
- Canonical vault: exists at `C:\dev\kaizen\vault`, commit `3de6042`; remote backup not yet configured
- Sibling staging root: exists at `C:\dev\kaizen\staging`; create-only staging write implemented and audited
- Platform repository: exists at `C:\dev\kaizen\platform`, commit `36fa419`; remote backup not yet configured
- Task Packet 005: complete and audited pass-with-notes
- Task Packet 006: retired combined draft preserved as source material for the required 006A/006B split; not eligible for approval
- Operational Postgres database and Observatory domain: not implemented
- Qdrant index: not implemented
- Hermes Desktop / Hermes Agent: deferred beyond the first slice; no write access or live integration is authorized during the remediation gate
- Decisions 0001 through 0007: accepted
- Decision 0008 operating conventions: accepted after end-to-end dry-run simulation
- Decision 0009 Operational Postgres/Observatory boundary: accepted
- Decision 0010 dedicated Internet Marketing Intelligence database boundary: accepted; no schema work authorized
- Decision 0011 progressive hybrid human interface direction: proposed; architecture only, no UI implementation authorized
- Decision 0012 first-slice contract and implementation boundary: accepted
- Field, note-type, ID, validation, hammer, Observatory, promotion, and staging specs: active drafts
- Document-contract research: completed externally and reconciled in `03-research-results/006-document-contract-standards-reconciliation.md`
- Historical research prompt: `02-research-prompts/002-document-contract-standards.md`; provenance only, do not execute as live doctrine
- Research Batch B: completed and reconciled in `03-research-results/015-internet-marketing-intelligence-providers-source-rights-claude-summary.md`; paid provider capture remains deferred and separately governed
- Stewardship principle: structure and automation must earn their existence

## Read-first sequence

Read in this order unless the user gives a narrower task:

1. `00-entrypoint/LLM_START_HERE.md` - this file
2. `01-project-standard/kaizen-vision-and-architecture-alignment.md` - central intended system vision and missing capability alignment
3. `01-project-standard/internet-marketing-intelligence-vision.md` - portfolio-wide longitudinal marketing intelligence vision
4. `ROADMAP.md` - active planning roadmap and phase tracker
5. `01-project-standard/kaizen-project-standard.md` - imported original baseline
6. `01-project-standard/baseline-v0.2-reconciliation-map.md` - preserve/replace/defer map
7. `01-project-standard/standard-revision-plan.md` - current v0.2 revision path
8. `04-design-decisions/0007-foundation-resolution-for-v0.2.md`
9. `04-design-decisions/0001-two-zone-agent-write-model.md`
10. `04-design-decisions/0002-search-before-create-and-diff-before-write.md`
11. `04-design-decisions/0003-raw-markdown-is-canonical.md`
12. `04-design-decisions/0004-system-of-record-boundaries.md`
13. `04-design-decisions/0005-api-only-structured-data-access.md`
14. `04-design-decisions/0006-hammer-tests-are-a-hard-gate.md`
15. `04-design-decisions/0008-v0.2-operating-conventions.md` - accepted
16. `04-design-decisions/0009-operational-postgres-and-observatory-boundary.md` - accepted
17. `04-design-decisions/0010-dedicated-internet-marketing-intelligence-database.md` - accepted; dedicated boundary and pre-schema design gate
18. `04-design-decisions/0011-progressive-hybrid-human-interface-direction.md` - proposed; architecture direction only
19. `04-design-decisions/0012-first-slice-contract-and-implementation-boundary.md` - accepted; first-slice code and repository boundary
20. `IMPLEMENTATION_ROADMAP.md` - active first-slice implementation roadmap
21. `05-specs/kaizen-field-registry.md`
22. `05-specs/kaizen-note-type-registry.md`
23. `05-specs/kaizen-id-and-prefix-registry.md`
24. `05-specs/staging-and-promotion-workflow.md`
25. `05-specs/promotion-event-schema.md`
26. `05-specs/kaizen-validation-gate-spec.md`
27. `05-specs/operational-postgres-authority.md`
28. `05-specs/kaizen-hammer-test-strategy.md`
29. `07-hermes/hermes-permission-matrix.md`
30. `07-hermes/hermes-write-access-preconditions.md`
31. `03-research-results/013-kaizen-human-interface-architecture-claude-summary.md`
32. `03-research-results/014-human-interface-technical-verification-and-amendment.md`
33. `03-research-results/015-internet-marketing-intelligence-providers-source-rights-claude-summary.md`
34. `05-specs/deferred-dataforseo-llm-ranking-capture-packet.md` - deferred; execution not authorized
35. `03-research-results/016-kaizen-planning-acceleration-audit.md`
36. `03-research-results/017-decision-0008-end-to-end-dry-run-simulation.md`
37. `03-research-results/018-milestone-1-tools-foundation-steward-audit.md`
38. `03-research-results/019-deterministic-note-validation-steward-audit.md`
39. `03-research-results/020-canonical-vault-bootstrap-steward-audit.md`
40. `03-research-results/021-staging-path-confinement-steward-audit.md`
41. `03-research-results/022-create-only-staging-write-wrapper-packet-audit.md`
42. `03-research-results/023-create-only-staging-write-wrapper-steward-audit.md`
43. `03-research-results/024-implementation-checkpoint-red-team-audit-claude-summary.md`
44. `03-research-results/025-implementation-checkpoint-audit-steward-reconciliation.md`
45. `06-handoff-patterns/005-implement-create-only-staging-write-wrapper.md`
46. `06-handoff-patterns/006-implement-human-operated-canonical-promotion.md` - pending draft; do not approve as written
47. `06-handoff-patterns/004-implement-staging-root-and-path-confinement-foundations.md`
48. `06-handoff-patterns/003-bootstrap-canonical-kaizen-vault.md`
49. `06-handoff-patterns/002-implement-deterministic-note-validation.md`
50. `03-research-results/006-document-contract-standards-reconciliation.md`
51. Other files in `03-research-results/` for supporting evidence
## Folder map

| Folder | Purpose | Doctrine status |
|---|---|---|
| `00-entrypoint/` | LLM and human orientation | Current guidance |
| `01-project-standard/` | Baseline and v0.2 standard revision planning | Candidate doctrine until accepted |
| `02-research-prompts/` | Prompts used for Claude/GPT/Hermes research | Inputs only |
| `03-research-results/` | Research outputs and summaries | Evidence, not doctrine |
| `04-design-decisions/` | Explicit architecture and governance decisions | Doctrine when status is accepted |
| `05-specs/` | Draft authority docs, registries, workflows, and implementation specs | Active drafts until reviewed |
| `06-handoff-patterns/` | Agent task-packet patterns | Draft until reviewed |
| `07-hermes/` | Hermes boundaries, tests, and skill planning | Draft until validated |
| `08-obsidian/` | Obsidian/plugin/MCP constraints | Evidence and draft constraints |
| `99-archive/` | Retired or superseded docs | Not current |

## Accepted v0.2 foundation

### Lifecycle

```yaml
status: draft | active | blocked | complete | archived
review_status: not-required | pending | approved | rejected
authority: none | proposed | accepted
```

- `active` means usable at the note's current authority level; it does not mean approved.
- Only humans may set `review_status: approved`, `review_status: rejected`, or `authority: accepted`.
- Supersedence uses explicit relationships, not a status value.

### Identity

Every durable note uses one immutable tool-generated ID:

```text
kz-<type-prefix>-<ulid>
```

No second UUID is required in v0.2.

### Metadata and links

- `project` is required.
- `domain` is optional and lowercase kebab-case when present.
- Frontmatter relationships use stable IDs.
- Canonical body links use relative Markdown links.
- Wikilinks may appear in staging but must be normalized or rejected before promotion.

### Staging and promotion

Current sibling roots:

```text
C:\dev\kaizen\vault
C:\dev\kaizen\staging
```

- Hermes may eventually write only to the staging root.
- Canonical content remains read-only to Hermes.
- Promotion is human-controlled and validated.
- Promotion events are append-only JSONL at `_governance/promotion-log.jsonl` in the canonical vault repository.
- Durable agent provenance remains after promotion; `validation_status` does not.

### Initial note types

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

Deferred until earned:

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

### Universal frontmatter

```yaml
id:
type:
status:
project:
summary:
created:
updated:
```

Type-specific fields are defined in `05-specs/kaizen-note-type-registry.md`.

## Current strongest constraints

1. Raw Markdown plus flat validated YAML is canonical for project intelligence.
2. Obsidian is the current human interface, not an independent source of truth.
3. Plugins are non-canonical convenience layers unless an accepted decision says otherwise.
4. Postgres owns structured operational records, not canonical project narrative.
5. Qdrant owns no truth and must remain rebuildable from Markdown.
6. Agents access Postgres and Qdrant only through approved typed APIs/tools.
7. Hermes may search, draft, lint, package, and propose within approved boundaries.
8. Hermes may not approve, promote, run arbitrary SQL, mutate Qdrant, touch source repos, publish externally, or mark work implementation-ready.
9. Agent writes begin in the sibling staging root and require deterministic validation plus human promotion.
10. Research and observations are evidence, not doctrine.
11. Search-before-create and diff-before-write are mandatory agent rules.
12. Declared invariants require validation and eventually real-execution hammer coverage.
13. Kaizen is intended to compound lawful public-market observations across projects while preserving project-private, customer-restricted, and licensed-data boundaries.
14. Opportunity and strategy outputs remain reviewable candidates until humans accept them.
15. No Internet Marketing Intelligence schema work begins until the dedicated database decision is reviewed and a focused owner/steward design session is completed.
16. Kaizen will pursue a progressive hybrid human-interface direction only through reviewed decisions and later implementation gates; Obsidian remains the canonical document workspace.

## Rules for LLMs

1. Never assume this repo is the future Obsidian vault.
2. Never create production vault folders here unless the user explicitly asks for a simulation or draft.
3. Treat accepted decisions as governing inputs; do not silently contradict them.
4. Do not change an accepted decision without an explicit amendment or superseding decision and human approval.
5. Do not promote research directly into doctrine.
6. Do not mark Hermes as approved for write access until all preconditions and boundary tests pass.
7. Prefer small, durable Markdown files over monolithic documents.
8. Use explicit file paths when referencing repo docs.
9. Keep documents readable as raw Markdown and avoid rendered-only meaning.
10. Preserve boundaries across Markdown, Postgres, Qdrant, source repos, operational repos, staging, and private/raw storage.
11. Search before creating a new document.
12. Produce a reviewable diff before modifying existing governed content.
13. When uncertain, record an open question or propose a decision rather than inventing policy.
14. Never treat a mutable frontmatter field as an access-control mechanism.

## Remaining v0.2 work

### Active implementation-checkpoint remediation

- follow `03-research-results/025-implementation-checkpoint-audit-steward-reconciliation.md`;
- correct the two canonical bootstrap notes and revalidate them;
- configure approved private remotes for the platform and vault repositories;
- add the adopted subprocess, abrupt-termination, malformed-log, and no-side-effect hammer tests;
- retire the current combined Packet 006 from approval consideration;
- draft and audit Packet 006A for the Windows first-time atomic-install primitive;
- draft Packet 006B only after 006A passes;
- keep canonical promotion blocked until Result 025 Gate A and Gate B close.

### Non-blocking future work

- marketplace-ranking coverage before the Internet Marketing Intelligence database-design session;
- provider-rights completion before permanent retention or external-use policy;
- Postgres and Qdrant storage/recovery work in their implementation phases;
- knowledge-quality and context-assembly research after real promoted notes exist;
- Hermes, desktop-shell, bridge-plugin, and provider-adapter implementation through later roadmap gates.

Decision 0008 is accepted. The broad research lanes above do not block the first vertical slice.
## Required output shape for future docs

Prefer:

```markdown
# Clear Title

Status: draft | proposed | accepted | rejected | superseded | archived
Date: YYYY-MM-DD

## Purpose or Context

Why this file exists.

## Content or Decision

The actual material.

## Consequences or Validation

What follows from it.

## Open questions

What remains unresolved.

## Related files

Explicit repository paths.
```

## Stewardship stance

Kaizen grows by evidence, not enthusiasm.

The default answer to new structure, automation, plugins, folders, note types, database tables, retrieval fields, and agent powers is:

```text
not yet - earn it
```

When a capability proves useful through real friction, governed need, or research-backed evidence, document it, decide it, validate it, hammer-test the boundary when appropriate, and only then add it to the standard.
