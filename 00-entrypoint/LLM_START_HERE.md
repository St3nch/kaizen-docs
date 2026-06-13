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

- Current phase: Milestones 1 through 8 are closed; Result 162 records exact owner acceptance of Milestone 8 closure
- Active planning roadmap: `ROADMAP_V0.3.md`; its current accepted planning state includes the corrective pre-Milestone-9 sequence and the parallel Observatory research track
- Current corrective planning evidence: `03-research-results/166-independent-milestones-6-8-audit-owner-acceptance.md` and `03-research-results/167-roadmap-lineage-and-corrective-sequence-reconciliation.md`
- Current task packet: `06-handoff-patterns/013a-documentation-and-authority-repair.md`; Packet 013A is documentation-only and separately approval-gated
- Milestone 9 controlled implementation-return pilot: accepted planning direction at `05-specs/controlled-implementation-return-pilot.md`; implementation remains unauthorized
- Canonical vault: local-only at commit `12d4ff849b6ca1f015b748cc23201b7f992139f6`; no remote exists and no push is authorized
- Platform repository: local-only at commit `75a269834b27ac016e7b5a973ae514d39cd8a1b8`; no remote exists
- Go8 repository checkpoint: `312704a8b8505bdb64f28cc557171c10de8bd5bc`
- Kaizen MCP remains a temporary non-Git proving ground; it is not production infrastructure
- Packet 012E and Packet 012E.1 are complete; the reliability and known-defect milestone is closed
- Immediate work is corrective planning and documentation repair before Milestone 9 readiness verification
- Observatory research is a parallel evidence track only; no provider purchase, raw capture, crawler deployment, client-data reuse, physical schema, Postgres, Qdrant, LangGraph, MCP-tool implementation, or hammer execution is authorized
- Operational Postgres database and Observatory domain: not implemented
- Qdrant index: not implemented
- Hermes Desktop / Hermes Agent: deferred; no canonical write authority or live integration is authorized
- Decisions 0001 through 0014 retain their recorded authority states; accepted decisions remain governing inputs
- Stewardship principle: structure and automation must earn their existence

## Read-first sequence

This numbered list is a task-dependent reference map, not a requirement to load every file. Always read this entrypoint and accepted `ROADMAP_V0.3.md`; read `IMPLEMENTATION_ROADMAP_V0.2.md` and `IMPLEMENTATION_ROADMAP.md` for completed implementation history, then load only the decisions, specs, audits, and task packets relevant to the requested work unless the user explicitly requests a full-repository audit.

1. `00-entrypoint/LLM_START_HERE.md` - this file
2. `01-project-standard/kaizen-vision-and-architecture-alignment.md` - central intended system vision and missing capability alignment
3. `01-project-standard/internet-marketing-intelligence-vision.md` - portfolio-wide longitudinal marketing intelligence vision
4. `ROADMAP_V0.3.md` - accepted post-Milestone 6 planning direction; `ROADMAP.md` is historical planning provenance
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
20. `IMPLEMENTATION_ROADMAP_V0.2.md` - accepted implementation roadmap snapshot; preserve the exact accepted SHA-256 and bytes
21. `IMPLEMENTATION_ROADMAP.md` - completed historical v0.1 first-slice implementation roadmap
22. `05-specs/kaizen-field-registry.md`
23. `05-specs/kaizen-note-type-registry.md`
24. `05-specs/kaizen-id-and-prefix-registry.md`
25. `05-specs/staging-and-promotion-workflow.md`
26. `05-specs/promotion-event-schema.md`
27. `05-specs/kaizen-validation-gate-spec.md`
28. `05-specs/operational-postgres-authority.md`
29. `05-specs/kaizen-hammer-test-strategy.md`
30. `07-hermes/hermes-permission-matrix.md`
31. `07-hermes/hermes-write-access-preconditions.md`
32. `03-research-results/013-kaizen-human-interface-architecture-claude-summary.md`
33. `03-research-results/014-human-interface-technical-verification-and-amendment.md`
34. `03-research-results/015-internet-marketing-intelligence-providers-source-rights-claude-summary.md`
35. `05-specs/deferred-dataforseo-llm-ranking-capture-packet.md` - deferred; execution not authorized
36. `03-research-results/016-kaizen-planning-acceleration-audit.md`
37. `03-research-results/017-decision-0008-end-to-end-dry-run-simulation.md`
38. `03-research-results/018-milestone-1-tools-foundation-steward-audit.md`
39. `03-research-results/019-deterministic-note-validation-steward-audit.md`
40. `03-research-results/020-canonical-vault-bootstrap-steward-audit.md`
41. `03-research-results/021-staging-path-confinement-steward-audit.md`
42. `03-research-results/022-create-only-staging-write-wrapper-packet-audit.md`
43. `03-research-results/023-create-only-staging-write-wrapper-steward-audit.md`
44. `03-research-results/024-implementation-checkpoint-red-team-audit-claude-summary.md`
45. `03-research-results/025-implementation-checkpoint-audit-steward-reconciliation.md`
46. `06-handoff-patterns/005-implement-create-only-staging-write-wrapper.md`
47. `06-handoff-patterns/006-implement-human-operated-canonical-promotion.md` - retired combined draft; do not approve
48. `06-handoff-patterns/006a-prove-windows-first-time-atomic-install.md` - complete; Result 028
49. `06-handoff-patterns/006b-implement-human-operated-first-promotion.md` - complete at platform commit `703d532`; Result 031 pass-with-documented-limitations
50. `06-handoff-patterns/004-implement-staging-root-and-path-confinement-foundations.md`
51. `06-handoff-patterns/003-bootstrap-canonical-kaizen-vault.md`
52. `06-handoff-patterns/002-implement-deterministic-note-validation.md`
53. `03-research-results/006-document-contract-standards-reconciliation.md`
54. Other files in `03-research-results/` for supporting evidence
55. `06-handoff-patterns/007-bootstrap-live-promotion-governance.md` - complete at vault commit `248b26a`
56. `03-research-results/032-packet-007-security-audit.md` - Packet 007 security audit
57. `03-research-results/033-packet-007-completion-steward-audit.md` - Packet 007 completion audit
58. `06-handoff-patterns/008a-implement-owner-controlled-live-promotion-operator.md` - security-audited pass; awaiting explicit owner approval
59. `03-research-results/034-packet-008a-security-audit.md` - Packet 008A security audit
60. `03-research-results/035-packet-008a-completion-steward-audit.md` - Packet 008A completion audit
61. `06-handoff-patterns/008b-first-real-promotion-two-gate-execution.md` - Gate A security-audited pass; Gate B prohibited
62. `03-research-results/036-packet-008b-gate-a-security-audit.md` - Packet 008B Gate A security audit
63. `03-research-results/037-packet-008b-gate-a-command-correction-audit.md` - Gate A command correction and zero-side-effect audit
64. `03-research-results/038-packet-008b-gate-a-docs-binding-correction-audit.md` - Gate A docs-binding correction audit
65. `03-research-results/039-packet-008b-gate-a-evidence-and-gate-b-security-audit.md` - Gate A evidence and Gate B security audit
66. `03-research-results/040-packet-008b-completion-steward-audit.md` - Packet 008B completion audit
67. `06-handoff-patterns/009a-generate-milestone-4-governed-planning-bundle.md` - staging-only Milestone 4 bundle packet
68. `03-research-results/041-packet-009a-security-audit.md` - Packet 009A security audit
69. `03-research-results/042-packet-009a-completion-steward-audit.md` - Packet 009A completion audit
70. `06-handoff-patterns/009b-finalize-review-and-run-ordered-bundle-promotion.md` - ordered multi-gate bundle promotion packet
71. `03-research-results/043-packet-009b-security-audit.md` - Packet 009B security audit
72. `03-research-results/044-packet-009b-lifecycle-activation-correction-audit.md` - lifecycle activation correction audit
73. `03-research-results/045-packet-009b-wave-1-plan-security-audit.md` - Wave 1 immutable plan audit
74. `03-research-results/046-packet-009b-wave-1-completion-steward-audit.md` - Wave 1 completion audit
75. `03-research-results/047-packet-009b-wave-2-plan-security-audit.md` - Wave 2 immutable plan security audit
76. `03-research-results/048-packet-009b-wave-2-completion-steward-audit.md` - Wave 2 completion steward audit
77. `03-research-results/049-packet-009b-wave-3-plan-security-audit.md` - Wave 3 immutable plan security audit
78. `03-research-results/050-packet-009b-wave-3-completion-steward-audit.md` - Wave 3 completion steward audit
79. `03-research-results/051-packet-009b-wave-4-plan-security-audit.md` - Wave 4 immutable plan security audit
80. `03-research-results/052-packet-009b-wave-4-completion-steward-audit.md` - Wave 4 completion steward audit
81. `03-research-results/053-packet-009b-wave-5-plan-security-audit.md` - Wave 5 immutable plan security audit
82. `03-research-results/054-packet-009b-wave-5-completion-steward-audit.md` - Wave 5 completion steward audit
83. `03-research-results/055-packet-009b-wave-6-plan-security-audit.md` - Wave 6 immutable plan security audit
84. `03-research-results/056-packet-009b-wave-6-completion-steward-audit.md` - Wave 6 completion and Packet 009B closure audit
85. `03-research-results/057-governed-amendment-implementation-security-steward-audit.md` - governed amendment implementation security and steward audit
86. `03-research-results/058-task-packet-completion-amendment-plan-security-audit.md` - task-packet completion amendment plan security audit
87. `03-research-results/059-task-packet-completion-amendment-steward-audit.md` - task-packet completion amendment steward audit
88. `03-research-results/060-current-state-amendment-plan-security-audit.md` - current-state amendment plan security audit
89. `03-research-results/061-current-state-amendment-completion-steward-audit.md` - current-state amendment completion steward audit
90. `03-research-results/062-milestone-4-implementation-return-closure-audit.md` - Milestone 4 implementation-return closure audit
91. `03-research-results/063-milestone-5-first-slice-retrospective.md` - Milestone 5 first-slice retrospective
92. `03-research-results/064-v0.2-draft-contradiction-audit.md` - v0.2 draft contradiction audit
93. `04-design-decisions/0013-v0.2-first-slice-contract-reconciliation.md` - proposed v0.2 reconciliation decision
94. `03-research-results/065-decision-0013-security-steward-audit.md` - Decision 0013 security and steward audit
95. `03-research-results/066-v0.2-post-reconciliation-acceptance-readiness-audit.md` - v0.2 post-reconciliation acceptance-readiness audit
96. `01-project-standard/kaizen-project-standard-v0.2.md` - authoritative Kaizen Project Standard v0.2
97. `03-research-results/067-kaizen-project-standard-v0.2-acceptance-and-milestone-5-closure-audit.md` - v0.2 acceptance and Milestone 5 closure audit
98. `03-research-results/068-implementation-roadmap-v0.2-and-milestone-6-security-steward-audit.md` - audited roadmap proposal and formal Milestone 6 definition
99. `03-research-results/069-implementation-roadmap-v0.2-owner-acceptance-and-milestone-6-planning-authorization.md` - exact roadmap acceptance and planning-only authorization
100. `06-handoff-patterns/010a-define-milestone-6-contracts-and-repository-placement.md` - completed documentation-only Packet 010A
101. `03-research-results/070-packet-010a-security-steward-audit.md` - Packet 010A pre-approval security and steward audit
102. `03-research-results/071-packet-010a-owner-approval.md` - exact Packet 010A documentation-only approval
103. `04-design-decisions/0014-milestone-6-governed-operator-surface-boundary.md` - accepted Milestone 6 operator-surface boundary
104. `05-specs/milestone-6-governed-operator-surface.md` - accepted operation-status, tool, console, and evidence contract
105. `03-research-results/072-packet-010a-completion-and-contract-audit.md` - Packet 010A completion and contract audit
106. `03-research-results/073-decision-0014-and-operator-surface-specification-owner-acceptance.md` - exact Decision 0014 and operator-surface specification acceptance
107. `06-handoff-patterns/010b-implement-read-only-operation-status-and-evidence-integrity.md` - proposed read-only Packet 010B
108. `03-research-results/074-packet-010b-security-steward-audit.md` - Packet 010B security and steward audit
109. `03-research-results/075-packet-010b-owner-approval.md` - exact Packet 010B read-only implementation approval
110. `03-research-results/076-packet-010b-completion-security-steward-audit.md` - Packet 010B completion and security audit
111. `06-handoff-patterns/010c-implement-amendment-native-mcp-adapters.md` - blocked Packet 010C MCP adapter draft
112. `03-research-results/077-packet-010c-prerequisite-gap-and-security-steward-audit.md` - Packet 010C prerequisite-gap audit
113. `06-handoff-patterns/010b1-implement-constrained-amendment-candidate-preparation.md` - complete at platform commit `c56e1cc`
114. `03-research-results/078-packet-010b1-option-a-authorization-and-security-steward-audit.md` - Option A authorization and Packet 010B.1 pre-implementation audit
115. `03-research-results/079-packet-010b1-completion-security-steward-audit.md` - Packet 010B.1 completion and security audit
116. `03-research-results/080-packet-010c-revised-security-steward-audit.md` - revised Packet 010C security and steward audit
117. `03-research-results/081-packet-010c-owner-approval.md` - exact revised Packet 010C owner authorization
118. `03-research-results/082-packet-010c-completion-security-steward-audit.md` - Packet 010C completion and security audit
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

## Current next gate

The active corrective sequence is:

```text
Packet 013A documentation and authority repair
-> Packet 013B Milestone 9 readiness verification
-> Packet 013C operation-status terminal-semantics correction, if confirmed
-> Packet 013D operator fallback runbook correction
-> Packet 013E post-Milestone-8 backup Generation 3
-> separate Milestone 9 implementation packet and owner gate
```

Current authority:

- Result 162 closes Milestone 8;
- Result 166 records the owner's bounded acceptance of the independent Milestones 6-8 audit;
- Result 167 defines the proposed corrective sequence;
- Packet 013A is the current approved documentation-only task packet;
- Milestone 9 fixture creation, oracle creation, disposable-repository creation, and execution remain unauthorized;
- Observatory work remains research-only under the OBR-01 through OBR-15 track;
- Postgres, Qdrant, LangGraph, Hermes, UI, provider purchase, raw capture, and production MCP work remain deferred and separately gated.

## Non-blocking future work

- governance-compression Decision 0015 reconciliation;
- Observatory rights, retention, measurement, reuse, and hammer research;
- Postgres and Qdrant storage/recovery work only after accepted workload evidence;
- Hermes, interface, provider-adapter, and production MCP work through later roadmap gates.
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
