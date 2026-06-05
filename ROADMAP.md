# Kaizen Planning Roadmap

Status: active
Date: 2026-06-05
Owner: Kaizen project steward

## Purpose

This roadmap is the project-wide planning guide for Kaizen.

It exists to keep the planning, governance, research, and specification work moving in a deliberate order until Kaizen is ready for a separate implementation roadmap.

This document is not the implementation roadmap. It does not authorize coding, production vault creation, database schema work, Hermes write access, MCP deployment, or Qdrant implementation.

The planning outcome is:

> An accepted Kaizen Project Standard, audited implementation boundaries, and a complete set of planning inputs from which a coding and implementation roadmap can be written without guessing.

## How to use this roadmap

- Read phases in order.
- Use the linked document as the detailed source for each phase.
- Do not start a later phase merely because part of it looks interesting.
- A phase is complete only when its exit criteria are met.
- Proposed decisions remain proposed until explicitly accepted.
- Research remains evidence until reconciled and approved.
- Implementation work begins only after the planning roadmap reaches its final handoff phase.

## Status vocabulary

| Status | Meaning |
|---|---|
| `complete` | Phase exit criteria are satisfied |
| `active` | Current work is concentrated here |
| `blocked` | Work cannot proceed until an identified dependency is resolved |
| `ready` | Prerequisites are satisfied but work has not started |
| `planned` | Phase is expected but prerequisites are not yet complete |

## Current position

```text
Current phase: Phase 2 - Vision and architecture alignment
Current objective: preserve the full Kaizen vision, obtain an independent Claude research-gap audit, and reconcile verified evidence before final contract and standard work
Current implementation status: not started
Hermes posture: required first-class vault consumer; canonical writes remain unapproved
```

---

# Phase 0 - Repository and stewardship foundation

Status: complete

## Description

Establish the planning repository, authority hierarchy, read-first entrypoint, Git discipline, and separation between historical material, research evidence, proposed decisions, accepted decisions, and draft specifications.

## Detailed phase documents

- `00-entrypoint/LLM_START_HERE.md`
- `README.md`
- `01-project-standard/standard-revision-plan.md`

## Exit criteria

- The repository purpose is explicit.
- The repository is distinguished from the future production vault.
- The authority hierarchy is documented.
- Git and stewardship rules are documented.
- Future stewards have a required reading sequence.

---

# Phase 1 - Accept the v0.2 foundation

Status: complete

## Description

Resolve the foundational architecture and governance questions required before detailed document contracts or implementation planning can be trusted.

This phase established canonical Markdown, system-of-record boundaries, agent staging, typed data access, hammer-test requirements, lifecycle axes, identity, links, initial note types, and universal fields.

## Detailed phase documents

- `04-design-decisions/0001-two-zone-agent-write-model.md`
- `04-design-decisions/0002-search-before-create-and-diff-before-write.md`
- `04-design-decisions/0003-raw-markdown-is-canonical.md`
- `04-design-decisions/0004-system-of-record-boundaries.md`
- `04-design-decisions/0005-api-only-structured-data-access.md`
- `04-design-decisions/0006-hammer-tests-are-a-hard-gate.md`
- `04-design-decisions/0007-foundation-resolution-for-v0.2.md`

## Exit criteria

- Decisions 0001 through 0007 are accepted.
- Canonical system boundaries are explicit.
- Human authority boundaries are explicit.
- The initial lifecycle, ID, field, and note-type direction is accepted.

---

# Phase 2 - Vision and architecture alignment

Status: active

## Description

Restore and preserve the full intended Kaizen system vision before further contract, standard, or implementation-roadmap work.

This phase establishes Kaizen as a continuously improving, agent-connected project intelligence system rather than only a governed documentation pipeline.

It includes continuous external and operational intelligence, cross-project knowledge compounding, implementation and operational feedback loops, portfolio intelligence, context packs, project resumption, research queues, freshness, cost governance, dynamic Obsidian views, distinct agent roles, and the broader Operational Postgres database.

## Detailed phase documents

- `01-project-standard/kaizen-vision-and-architecture-alignment.md`
- `00-entrypoint/LLM_START_HERE.md`
- `ROADMAP.md`
- `04-design-decisions/0004-system-of-record-boundaries.md`
- `04-design-decisions/0005-api-only-structured-data-access.md`
- `04-design-decisions/0009-operational-postgres-and-observatory-boundary.md` - proposed boundary correction
- `05-specs/postgres-observatory-authority.md`
- `03-research-results/004-markdown-qdrant-postgres-architecture-claude-summary.md`

## Required work

1. Review and accept or revise the vision-alignment document.
2. Give Claude the complete repository and vision-alignment document.
3. Ask Claude to identify missing research, contradictions, unsupported assumptions, and implementation choices requiring evidence.
4. Require primary and authoritative sources where possible.
5. Separate research questions from design decisions and implementation details.
6. Reconcile Claude's findings through adopt, modify, reject, and defer decisions.
7. Update the central vision, decisions, roadmap, and affected draft specs.
8. Only then resume final document-contract and Project Standard work.

## Steward plan from Claude audit

Claude's audit is evidence, not doctrine. The project steward adopts the following dependency-ordered plan, with the modifications noted below.

### Step 1 - Repair and stabilize the current planning batch

- Correct the corrupted Markdown tokens in `05-specs/kaizen-field-registry.md` and `05-specs/kaizen-validation-gate-spec.md`.
- Keep only Phase 2 active; Phase 3 remains ready and dependent on Phase 2.
- Run `git diff --check`, scan for control characters and backup files, and review the complete diff before any commit.
- Do not run the Hermes read-only test until the entrypoint and governed drafts are internally consistent.

### Step 2 - Preserve and reconcile Claude's audit

- Preserve Claude's report in `03-research-results/` as a faithful evidence summary, and retain the raw report when the repository environment can access it.
- Create a concise Kaizen reconciliation using `adopt`, `modify`, `reject`, and `defer`.
- Do not promote Claude's recommendations directly into accepted doctrine.

### Step 3 - Resolve the Operational Postgres and Observatory boundary

- Draft a new decision that establishes the broader Operational Postgres database and the Observatory as one bounded domain within it.
- Treat the proposed domains as initial planning boundaries, not permanent physical schemas:

```text
observatory
ingestion
automation
governance
audit
reference
```

- Amend or supersede the affected portion of Decision 0004 through the accepted decision process; do not silently rewrite accepted doctrine.
- After the decision is accepted, revise or split `05-specs/postgres-observatory-authority.md` into the appropriate database-wide and Observatory-domain authority documents.

### Step 4 - Make the feedback loops operationally explicit

- Update current guidance so Kaizen is described as a loop rather than a one-way pipeline.
- Add the implementation-return path to the Decision 0008 dry run:

```text
task packet
-> implementation
-> completion report and test evidence
-> failures, deviations, and discoveries
-> reviewed updates to project intelligence
```

- Decide the artifact shape only after the dry run. Do not assume a new note type is required.
- Require the v0.2 standard to cover implementation, operational, and Kaizen self-improvement feedback paths.

### Step 5 - Run Research Batch A: agent access and write safety

Priority: P0/P1

Research and test:

- MCP filesystem and Obsidian security boundaries
- root confinement, traversal, symlink, and Windows junction behavior
- Hermes tool restriction and capability granularity
- staging-only write enforcement
- Windows atomic or recoverable file promotion
- durable JSONL event append behavior

Expected output:

- verified safety constraints
- native-control versus wrapper requirements
- runnable boundary tests
- go/no-go criteria for Hermes staging writes

This batch gates agent write access and must precede any approval of Hermes writes.

### Step 6 - Run Research Batch B: external intelligence providers and source rights

Priority: P1

Research:

- DataForSEO and credible alternatives
- SERP, ranking, visibility, and cost capabilities
- LLM citation and retrievability measurement providers
- scientific-paper, patent, news, documentation, dataset, and transcript ingestion constraints
- copyright, licensing, provider terms, storage, citation, and retention boundaries

Expected output:

- provider capability and cost matrix
- source-class legal and operational constraints
- migrate-versus-reference guidance
- freshness expectations for volatile providers and observations

### Step 7 - Run Research Batch C: storage, retrieval, and recovery constraints

Priority: P1

Research:

- Operational Postgres workload and domain-governance patterns
- Qdrant filtering, rebuildability, scope isolation, authority, supersedence, and privacy constraints
- vault and database backup, restore, recovery, and versioning posture

Dependencies:

- the Operational Postgres and Observatory boundary decision
- relevant findings from Research Batch A

Expected output:

- database authority and workload inputs, not table designs
- Qdrant design constraints, not final collection layouts
- backup and recovery requirements

### Step 8 - Run Research Batch D: knowledge quality and context assembly

Priority: P2

Research:

- source trust and evidentiary roles
- freshness classes and recheck triggers
- governed context-pack assembly
- project resumption and retrieval filtering

Dependencies:

- provider and source findings from Batch B
- storage and retrieval findings from Batch C

Expected output:

- options and recommended directions
- explicit deferrals for fields, scores, schemas, and automation not yet earned

### Step 9 - Resume contracts, operating conventions, and the v0.2 standard

After the alignment work and required research:

1. Recheck the document-contract batch against the aligned vision and verified evidence.
2. Run the Decision 0008 dry run, including the implementation-return path.
3. Resolve pipeline stages, folder placement, and feedback handling.
4. Draft and audit the complete Kaizen Project Standard v0.2.
5. Define implementation-planning inputs.
6. Create the implementation roadmap only after all earlier gates pass.

### Explicit deferrals

Do not research or specify these without a demonstrated need:

- GraphRAG
- exact Postgres tables
- exact Qdrant collections and chunk sizes
- exact MCP server selection before safety research
- exact model router
- exact dashboard queries
- exact source trust scores
- exact freshness fields
- exact monitoring cadence
- automatic portfolio prioritization
## Exit criteria

- The intended Kaizen vision is explicitly documented and human-reviewed.
- Claude has independently audited the repository against that vision.
- Research gaps have an evidence-focused plan.
- High-value research has been completed or explicitly deferred.
- Operational Postgres and Observatory terminology is aligned.
- Agent roles, MCP/tool posture, feedback loops, cross-project knowledge, and migration are represented in planning.
- The document-contract batch has been rechecked against the aligned vision.

---
# Phase 3 - Document contracts and planning reconciliation

Status: ready

Dependency: Phase 2 vision and architecture alignment must complete before final Phase 3 acceptance.

## Description

Reconcile the completed document-contract research into Kaizen-specific contracts without importing unnecessary ceremony or weakening authority boundaries.

This phase defines how source summaries, claims, decisions, specs, audits, and task packets must behave. It also defines approval freshness, scoped repository drift, and Hermes participation in these workflows.

## Detailed phase documents

- `03-research-results/006-document-contract-standards-reconciliation.md`
- `05-specs/kaizen-field-registry.md`
- `05-specs/kaizen-note-type-registry.md`
- `05-specs/kaizen-validation-gate-spec.md`
- `05-specs/promotion-event-schema.md`
- `05-specs/kaizen-hammer-test-strategy.md`
- `05-specs/staging-and-promotion-workflow.md`
- `07-hermes/hermes-permission-matrix.md`
- `07-hermes/hermes-write-access-preconditions.md`

## Remaining work

1. Review the reconciliation dispositions: adopt, modify, reject, and defer.
2. Review the six updated document contracts.
3. Confirm the audit verdict vocabulary.
4. Confirm the one-primary-spec rule for task packets.
5. Confirm scoped approval freshness and drift handling.
6. Correct any contradictions found during review.
7. Approve the batch for commit when the diff is accepted.

## Exit criteria

- The reconciliation is reviewed and accepted or revised.
- All six document contracts are internally consistent.
- Supporting validation, promotion, hammer, and Hermes drafts reflect the contracts.
- No research recommendation has silently become doctrine.
- The reviewed batch is committed with a clean working tree.

---

# Phase 4 - Resolve operating conventions through a real-project dry run

Status: planned

## Description

Test Decision 0008 against one real project before accepting its pipeline stages, folder placement, path conventions, plugin posture, source URL rules, validation thresholds, and current-state retrieval policy.

The purpose is to expose friction and ambiguity before these conventions become binding.

## Detailed phase documents

- `04-design-decisions/0008-v0.2-operating-conventions.md`
- `01-project-standard/baseline-v0.2-reconciliation-map.md`
- `05-specs/kaizen-note-type-registry.md`
- `05-specs/kaizen-validation-gate-spec.md`

## Required dry-run outputs

- A mapped project journey through the proposed pipeline stages.
- A proposed project folder tree populated only with earned artifacts.
- Examples of canonical relative paths and source-repository references.
- A list of conventions that worked without modification.
- A list of conventions that caused ambiguity or needless friction.
- A recommendation to accept, revise, split, or reject Decision 0008.

## Exit criteria

- One real project has been mapped through the proposed workflow.
- Stage and folder ambiguities are documented.
- Decision 0008 is accepted, revised and accepted, or rejected.
- Final pipeline and placement rules are available for the v0.2 standard.

---

# Phase 5 - Draft the complete Kaizen Project Standard v0.2

Status: planned

## Description

Produce the complete candidate standard from the accepted foundation, reconciled contracts, approved operating conventions, and source-of-truth boundaries.

The draft must replace obsolete baseline mechanics without losing the original project intent.

## Detailed phase documents

- `01-project-standard/kaizen-project-standard.md`
- `01-project-standard/baseline-v0.2-reconciliation-map.md`
- `01-project-standard/standard-revision-plan.md`
- accepted decisions in `04-design-decisions/`
- governing drafts in `05-specs/`
- `03-research-results/006-document-contract-standards-reconciliation.md`

## Required v0.2 contents

- Kaizen purpose and pipeline.
- Repository, vault, raw-storage, Postgres, Qdrant, source-repository, and operational-repository boundaries.
- Human and agent authority boundaries.
- Hermes role and access progression.
- Note types, fields, lifecycle, identity, and links.
- Project structure and portable path rules.
- Document contracts.
- Validation, promotion, supersedence, and approval-freshness rules.
- Plugin and Obsidian posture.
- Planning-to-implementation handoff requirements.

## Exit criteria

- One complete v0.2 draft exists.
- The draft contains no unresolved contradiction with accepted decisions.
- Open implementation choices are labeled rather than invented.
- The original baseline remains preserved as historical source material.

---

# Phase 6 - Audit and accept the Kaizen Project Standard v0.2

Status: planned

## Description

Audit the complete v0.2 draft for contradictions, missing boundaries, excessive structure, hidden authority, unenforceable rules, and implementation ambiguity.

This is the gate between planning doctrine and implementation-roadmap preparation.

## Detailed phase documents

- the future `01-project-standard/kaizen-project-standard-v0.2-draft.md`
- `03-research-results/005-veda-db-governance-hammer-patterns-claude-summary.md`
- `03-research-results/006-document-contract-standards-reconciliation.md`
- `05-specs/kaizen-validation-gate-spec.md`
- `05-specs/kaizen-hammer-test-strategy.md`

## Required audit areas

- source-of-truth ownership
- lifecycle and authority combinations
- Hermes and agent permissions
- canonical versus operational state
- staging and promotion
- validation and hammer-test coverage
- portability and plugin independence
- document-contract consistency
- approval freshness
- implementation-readiness criteria
- speculative structure that has not earned its place

## Exit criteria

- Audit findings are resolved or explicitly deferred.
- The human verdict is `pass` or `pass-with-notes` with non-blocking notes only.
- The v0.2 standard is explicitly accepted.
- Superseded or historical material is clearly identified.

---

# Phase 7 - Define implementation-planning inputs

Status: planned

## Description

Translate the accepted standard into bounded implementation workstreams without designing tables, tools, or deployment details prematurely.

This phase identifies what the implementation roadmap must cover and what planning artifact owns each concern.

## Detailed phase documents

- accepted Kaizen Project Standard v0.2
- `05-specs/kaizen-id-and-prefix-registry.md`
- `05-specs/kaizen-validation-gate-spec.md`
- `05-specs/staging-and-promotion-workflow.md`
- `05-specs/promotion-event-schema.md`
- `05-specs/kaizen-hammer-test-strategy.md`
- `07-hermes/hermes-write-access-preconditions.md`
- `07-hermes/hermes-first-read-only-test.md`
- `07-hermes/hermes-first-staging-write-test.md`

## Required planning workstreams

### Operational database

Before schema design:

1. document workloads and operational questions
2. define bounded domains
3. assign source-of-truth ownership
4. define project scope and permissions
5. define retention, backup, and recovery
6. define typed API boundaries
7. define schema authority and invariants
8. define hammer tests

Expected domains currently include:

```text
observatory
ingestion
automation
governance
audit
reference
```

### Canonical vault

Define:

- root structure
- project bootstrap
- note creation workflow
- canonical validation
- Git policy
- backup and recovery
- Obsidian configuration posture

### Staging and promotion tooling

Define:

- staging-root enforcement
- ID generation
- validation commands
- promotion command
- event logging
- rollback and recovery
- Windows path, symlink, and junction defenses

### Hermes

Define:

- canonical read/search tools
- staging-write wrapper
- permission profile
- provenance fields
- first read-only test
- first staging-write test
- audit logging
- escalation behavior

### MCP and typed tools

Define:

- whether MCP is needed for each capability
- narrow tool contracts
- project-scope injection
- authorization
- response schemas
- logging
- rate and budget controls
- prohibited broad filesystem or database surfaces

### Qdrant

Define:

- approved Markdown indexing boundary
- chunk identity and rebuild behavior
- payload filters
- superseded/rejected-content handling
- project isolation
- current-state treatment
- private-data exclusion

### Operations and hardening

Define:

- environments
- configuration and secrets
- monitoring
- backup and restore
- incident recovery
- migration ownership
- dependency/version policy
- CI validation
- hammer-test execution

## Exit criteria

- Every implementation workstream has a planning owner.
- Missing decisions are identified explicitly.
- Database workload discovery precedes schema design.
- Tool and agent permissions are defined before implementation.
- The implementation roadmap can be ordered by dependencies rather than enthusiasm.

---

# Phase 8 - Create the Kaizen implementation roadmap

Status: planned

## Description

Create a separate implementation roadmap that turns the accepted planning artifacts into sequenced, auditable build phases.

The implementation roadmap should cover at least:

- foundational tooling and repository decisions
- ID generation and Markdown validation
- canonical vault creation
- staging and promotion
- operational Postgres database
- Observatory and other database domains
- typed APIs and MCP surfaces
- Hermes read/search integration
- Hermes staging-write integration
- Qdrant indexing and retrieval
- Obsidian human interface
- backup, recovery, monitoring, and security
- CI and hammer-test gates
- first end-to-end project pipeline

## Future detailed phase document

- `IMPLEMENTATION_ROADMAP.md` - create only after Phases 0 through 7 are complete

## Required implementation-roadmap qualities

- dependency-ordered phases
- explicit prerequisites and exit criteria
- links to governing decisions and specs
- no database table design before workload and authority planning
- no Hermes write access before boundary tests
- no Qdrant authority or direct agent mutation
- no production rollout without backup and recovery
- task-packet-sized execution batches
- human review gates at authority-bearing transitions

## Exit criteria

- `IMPLEMENTATION_ROADMAP.md` exists and is reviewed.
- Each implementation phase links to accepted planning documents.
- The first implementation batch is small, reversible, and task-packet-ready.
- Coding begins only after the first implementation task packet is approved.

---

# Stewardship controls

The project steward must keep this roadmap current.

After meaningful planning work:

1. update phase status when evidence supports the change
2. link new governing documents to the owning phase
3. record blockers and unresolved decisions in the detailed owner document
4. prevent implementation work from bypassing unfinished planning gates
5. avoid adding phases, workstreams, or artifacts that have not earned their place
6. review Git status and diffs before commits
7. report exactly what changed and what remains

## Roadmap change rule

A roadmap update may clarify sequencing, ownership, links, status, or exit criteria.

A roadmap update must not silently:

- accept a proposed decision
- grant Hermes or another agent new authority
- redefine a system of record
- authorize implementation
- replace an owning specification
- hide a blocker by changing phase wording

Material governance changes belong in the appropriate decision or specification first, then the roadmap is updated to reflect them.

## Immediate next actions

1. Repair the corrupted field-registry and validation-gate tokens.
2. Preserve Claude's audit in `03-research-results/` and write the Kaizen reconciliation.
3. Review and accept, revise, or reject proposed Decision 0009 for the Operational Postgres and Observatory boundary.
4. Add the implementation-return feedback path to the Phase 4 dry-run requirements and Phase 5 standard requirements.
5. Review the full planning diff and create a clean Git checkpoint.
6. Run Research Batch A before any Hermes write-access test or implementation-roadmap work.
7. Do not begin the implementation roadmap until the vision, required research, v0.2 standard, and implementation-planning inputs are accepted.