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

- Current phase: Decision 0013 accepted; first-slice specifications reconciled; Result 066 passed; Kaizen Project Standard v0.2 candidate awaits explicit owner acceptance
- Planning roadmap: historical planning source plus current live-operator implementation gate at `ROADMAP.md`
- Active implementation roadmap: `IMPLEMENTATION_ROADMAP.md`
- Checkpoint audit evidence: `03-research-results/024-implementation-checkpoint-red-team-audit-claude-summary.md`
- Governing remediation ledger: `03-research-results/025-implementation-checkpoint-audit-steward-reconciliation.md`
- Canonical vault: exists at `C:\dev\kaizen\vault`, commit `e5e4eec`; task-packet completion report and current-state return amendments are complete; no remote exists and no push is authorized
- Sibling staging root: exists at `C:\dev\kaizen\staging`; create-only staging write implemented and audited
- Platform repository: exists at `C:\dev\kaizen\platform`, commit `845d65f`; governed amendment support passes 230 full-suite tests and Result 057; no remote exists
- Task Packet 005: complete and audited pass-with-notes
- Task Packet 006: retired combined draft preserved as source material for the required 006A/006B split; not eligible for approval
- Task Packet 006A: complete at platform commit `26271ce`; steward-audited in Result 028
- Task Packet 006B: complete at `703d532`; final steward audit Result 031 pass-with-documented-limitations; no live promotion authority
- Task Packet 007: complete at vault commit `248b26a`; Result 033 pass; no promotion occurred
- Task Packet 008A: complete at platform commit `1a890dd`; Result 035 pass; no live plan or promotion occurred
- Task Packet 008B: complete at vault commit `80bc093`; Result 040 pass; first real promotion committed
- Task Packet 009A: complete; Result 042 pass; six staged notes validate with all relationships resolved; no canonical or platform mutation occurred
- Task Packet 009B and Milestone 4: complete; Milestone 5 retrospective and contract reconciliation are complete; Result 066 passes the v0.2 candidate for owner acceptance
- Operational Postgres database and Observatory domain: not implemented
- Qdrant index: not implemented
- Hermes Desktop / Hermes Agent: deferred beyond the first slice; no write access or live integration is authorized at the live-operator gate
- Decisions 0001 through 0007: accepted
- Decision 0008 operating conventions: accepted after end-to-end dry-run simulation
- Decision 0009 Operational Postgres/Observatory boundary: accepted
- Decision 0010 dedicated Internet Marketing Intelligence database boundary: accepted; no schema work authorized
- Decision 0011 progressive hybrid human interface direction: proposed; architecture only, no UI implementation authorized
- Decision 0012 first-slice contract and implementation boundary: accepted
- Decision 0013 v0.2 first-slice contract reconciliation: accepted on 2026-06-09; supporting specifications reconciled
- Field, note-type, ID, validation, hammer, Observatory, promotion, and staging specs: active drafts
- Document-contract research: completed externally and reconciled in `03-research-results/006-document-contract-standards-reconciliation.md`
- Historical research prompt: `02-research-prompts/002-document-contract-standards.md`; provenance only, do not execute as live doctrine
- Research Batch B: completed and reconciled in `03-research-results/015-internet-marketing-intelligence-providers-source-rights-claude-summary.md`; paid provider capture remains deferred and separately governed
- Stewardship principle: structure and automation must earn their existence

## Read-first sequence

This numbered list is a task-dependent reference map, not a requirement to load every file. Always read this entrypoint and `IMPLEMENTATION_ROADMAP.md`; then read only the decisions, specs, audits, and task packets relevant to the requested work unless the user explicitly requests a full-repository audit.

1. `00-entrypoint/LLM_START_HERE.md` - this file
2. `01-project-standard/kaizen-vision-and-architecture-alignment.md` - central intended system vision and missing capability alignment
3. `01-project-standard/internet-marketing-intelligence-vision.md` - portfolio-wide longitudinal marketing intelligence vision
4. `ROADMAP.md` - historical planning provenance with a maintained current-gate summary
5. `01-project-standard/kaizen-project-standard.md` - imported original baseline
6. `01-project-standard/baseline-v0.2-reconciliation-map.md` - preserve/replace/defer map
7. `01-project-standard/standard-revision-plan.md` - deferred v0.2 consolidation path; resume after Milestone 4 evidence
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
46. `06-handoff-patterns/006-implement-human-operated-canonical-promotion.md` - retired combined draft; do not approve
47. `06-handoff-patterns/006a-prove-windows-first-time-atomic-install.md` - complete; Result 028
48. `06-handoff-patterns/006b-implement-human-operated-first-promotion.md` - complete at platform commit `703d532`; Result 031 pass-with-documented-limitations
49. `06-handoff-patterns/004-implement-staging-root-and-path-confinement-foundations.md`
50. `06-handoff-patterns/003-bootstrap-canonical-kaizen-vault.md`
51. `06-handoff-patterns/002-implement-deterministic-note-validation.md`
52. `03-research-results/006-document-contract-standards-reconciliation.md`
53. Other files in `03-research-results/` for supporting evidence
54. `06-handoff-patterns/007-bootstrap-live-promotion-governance.md` - complete at vault commit `248b26a`
55. `03-research-results/032-packet-007-security-audit.md` - Packet 007 security audit
56. `03-research-results/033-packet-007-completion-steward-audit.md` - Packet 007 completion audit
57. `06-handoff-patterns/008a-implement-owner-controlled-live-promotion-operator.md` - security-audited pass; awaiting explicit owner approval
58. `03-research-results/034-packet-008a-security-audit.md` - Packet 008A security audit
59. `03-research-results/035-packet-008a-completion-steward-audit.md` - Packet 008A completion audit
60. `06-handoff-patterns/008b-first-real-promotion-two-gate-execution.md` - Gate A security-audited pass; Gate B prohibited
61. `03-research-results/036-packet-008b-gate-a-security-audit.md` - Packet 008B Gate A security audit
62. `03-research-results/037-packet-008b-gate-a-command-correction-audit.md` - Gate A command correction and zero-side-effect audit
63. `03-research-results/038-packet-008b-gate-a-docs-binding-correction-audit.md` - Gate A docs-binding correction audit
64. `03-research-results/039-packet-008b-gate-a-evidence-and-gate-b-security-audit.md` - Gate A evidence and Gate B security audit
65. `03-research-results/040-packet-008b-completion-steward-audit.md` - Packet 008B completion audit
66. `06-handoff-patterns/009a-generate-milestone-4-governed-planning-bundle.md` - staging-only Milestone 4 bundle packet
67. `03-research-results/041-packet-009a-security-audit.md` - Packet 009A security audit
68. `03-research-results/042-packet-009a-completion-steward-audit.md` - Packet 009A completion audit
69. `06-handoff-patterns/009b-finalize-review-and-run-ordered-bundle-promotion.md` - ordered multi-gate bundle promotion packet
70. `03-research-results/043-packet-009b-security-audit.md` - Packet 009B security audit
71. `03-research-results/044-packet-009b-lifecycle-activation-correction-audit.md` - lifecycle activation correction audit
72. `03-research-results/045-packet-009b-wave-1-plan-security-audit.md` - Wave 1 immutable plan audit
73. `03-research-results/046-packet-009b-wave-1-completion-steward-audit.md` - Wave 1 completion audit
74. `03-research-results/047-packet-009b-wave-2-plan-security-audit.md` - Wave 2 immutable plan security audit
75. `03-research-results/048-packet-009b-wave-2-completion-steward-audit.md` - Wave 2 completion steward audit
76. `03-research-results/049-packet-009b-wave-3-plan-security-audit.md` - Wave 3 immutable plan security audit
77. `03-research-results/050-packet-009b-wave-3-completion-steward-audit.md` - Wave 3 completion steward audit
78. `03-research-results/051-packet-009b-wave-4-plan-security-audit.md` - Wave 4 immutable plan security audit
79. `03-research-results/052-packet-009b-wave-4-completion-steward-audit.md` - Wave 4 completion steward audit
80. `03-research-results/053-packet-009b-wave-5-plan-security-audit.md` - Wave 5 immutable plan security audit
81. `03-research-results/054-packet-009b-wave-5-completion-steward-audit.md` - Wave 5 completion steward audit
82. `03-research-results/055-packet-009b-wave-6-plan-security-audit.md` - Wave 6 immutable plan security audit
83. `03-research-results/056-packet-009b-wave-6-completion-steward-audit.md` - Wave 6 completion and Packet 009B closure audit
84. `03-research-results/057-governed-amendment-implementation-security-steward-audit.md` - governed amendment implementation security and steward audit
85. `03-research-results/058-task-packet-completion-amendment-plan-security-audit.md` - task-packet completion amendment plan security audit
86. `03-research-results/059-task-packet-completion-amendment-steward-audit.md` - task-packet completion amendment steward audit
87. `03-research-results/060-current-state-amendment-plan-security-audit.md` - current-state amendment plan security audit
88. `03-research-results/061-current-state-amendment-completion-steward-audit.md` - current-state amendment completion steward audit
89. `03-research-results/062-milestone-4-implementation-return-closure-audit.md` - Milestone 4 implementation-return closure audit
90. `03-research-results/063-milestone-5-first-slice-retrospective.md` - Milestone 5 first-slice retrospective
91. `03-research-results/064-v0.2-draft-contradiction-audit.md` - v0.2 draft contradiction audit
92. `04-design-decisions/0013-v0.2-first-slice-contract-reconciliation.md` - proposed v0.2 reconciliation decision
93. `03-research-results/065-decision-0013-security-steward-audit.md` - Decision 0013 security and steward audit
94. `03-research-results/066-v0.2-post-reconciliation-acceptance-readiness-audit.md` - v0.2 post-reconciliation acceptance-readiness audit
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

### Current next gate

- Packet 006A and Packet 006B are complete and steward-audited;
- Packet 007 is complete and steward-audited pass in Result 033;
- a clean low-risk source-summary candidate is staged at SHA-256 `119788565b83876d833917e6b7c7fdd4f35e9620c0c468202e7628825ad58f1a`;
- Packet 008A is complete and steward-audited pass in Result 035;
- Packet 008B is complete and Result 040 verifies the first real promotion;
- Packet 009A is complete and Result 042 verifies all six staged notes;
- Packet 009B Wave 1 is complete;
- Wave 2 plan `13fef2deec96f89f388a997cc91a89f41efa69fc9120b52684e4492f35f0f601` is security-audited pass in Result 047;
- require exact owner approval naming Wave 2, operation `kz-prom-01KTMNHB6NZ5YBJACWZM92HGJH`, and the plan hash before execution;
- Wave 3 planning and Waves 3 through 6 execution remain prohibited;
- keep Postgres, Qdrant, Hermes, providers, UI, websites, and other deferred systems out of scope until earned;
- use the first real promotion and Milestone 4 governed loop as evidence for the later v0.2 consolidation;
- preserve the owner-deferred platform and vault remote gap until the working-project checkpoint.

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
