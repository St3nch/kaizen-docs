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
- Implementation work begins after the acceleration exit gate is satisfied and Implementation Roadmap v0.1 authorizes the first milestone.

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
Current phase: implementation checkpoint remediation gate
Current objective: close Research Result 025 Gate A and Gate B findings before canonical-promotion approval or implementation
Current implementation status: Milestones 1 and 2 implemented; Milestone 3 staging boundary implemented; canonical promotion not approved
Active execution roadmap: IMPLEMENTATION_ROADMAP.md
Hermes posture: deferred beyond the first slice; canonical writes remain unapproved
```

The planning roadmap has handed active first-slice execution to `IMPLEMENTATION_ROADMAP.md`. Historical phase detail remains below for provenance. The current closure authority is:

- `03-research-results/024-implementation-checkpoint-red-team-audit-claude-summary.md`
- `03-research-results/025-implementation-checkpoint-audit-steward-reconciliation.md`

---

# Acceleration amendment - 2026-06-07

Status: active

## Reason

The planning foundation is coherent enough to begin a narrow implementation slice. Remaining broad research is useful but no longer treated as a serial prerequisite unless it protects canonical data, authority, security, customer information, money, or an expensive-to-reverse boundary.

## Accepted acceleration direction

```text
minimum safe contract freeze
-> Implementation Roadmap v0.1
-> first implementation task packet
-> one working vertical slice
-> retrospective
-> evidence-driven v0.2 consolidation
```

## Pre-implementation-roadmap blockers

Implementation Roadmap v0.1 is now written. The remaining gate before Milestone 1 is the first reviewed task packet:

1. Decision 0008 accepted after dry-run simulation - complete.
2. First-slice document, metadata, validation, and promotion contracts explicitly frozen.
3. Tooling language and repository selected.
4. First vertical slice scoped with explicit exclusions.
5. First task-packet format confirmed for implementation use.

## First vertical slice

The first slice proves:

```text
canonical Markdown
-> deterministic validation
-> sibling staging
-> human-controlled safe promotion
-> task packet
-> implementation completion evidence
-> governed current-state update
```

Included:

- Kaizen ID generation;
- Markdown and flat-YAML parsing;
- first-slice validation;
- safe human-operated promotion with confinement, hashes, journal, and recovery;
- canonical vault bootstrap;
- one realistic project chain;
- completion report and self-audit.

Excluded:

- Hermes integration;
- Operational Postgres;
- Internet Marketing Intelligence database;
- Qdrant;
- DataForSEO;
- marketplace-ranking collection;
- desktop control shell;
- Obsidian bridge plugin;
- final backup architecture;
- advanced knowledge-quality scoring.

## Reclassified research lanes

### Step 6A marketplace-ranking coverage

Remains required before the focused Internet Marketing Intelligence database-design session. It does not block the first vertical slice or Implementation Roadmap v0.1.

### Research Batch C

Compressed for the first slice to Git remote backup for canonical Markdown, disposable staging, and the accepted promotion-recovery protocol. Full Postgres, Qdrant, and production recovery work moves to the relevant implementation phase.

### Research Batch D

Deferred until real promoted corpora exist. Initial notes use the accepted fields, stable source relationships, timestamps, and existing confidence enum.

### Decision 0011 and interface work

Remain future implementation inputs and do not block the first slice.

## Planning exit gate

The planning roadmap may hand off to Implementation Roadmap v0.1 when:

- Decision 0008 is accepted;
- the first-slice contracts are frozen;
- the tools language and repository are selected;
- the first slice and exclusions are recorded;
- the first implementation milestone and task packet are reviewable.

The exit gate does not require completion of every future research lane.

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

It includes continuous external and operational intelligence, cross-project knowledge compounding, implementation and operational feedback loops, portfolio intelligence, a shared longitudinal Internet Marketing Intelligence signal system, context packs, project resumption, research queues, freshness, cost governance, dynamic Obsidian views, distinct agent roles, and the broader Operational Postgres database.

## Detailed phase documents

- `01-project-standard/kaizen-vision-and-architecture-alignment.md`
- `01-project-standard/internet-marketing-intelligence-vision.md`
- `00-entrypoint/LLM_START_HERE.md`
- `ROADMAP.md`
- `04-design-decisions/0004-system-of-record-boundaries.md`
- `04-design-decisions/0005-api-only-structured-data-access.md`
- `04-design-decisions/0009-operational-postgres-and-observatory-boundary.md` - accepted boundary decision
- `04-design-decisions/0010-dedicated-internet-marketing-intelligence-database.md` - accepted database boundary; no schema authorization
- `05-specs/operational-postgres-authority.md`
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
- Keep Hermes test plans current, but do not run live Hermes integration tests during the current vision-and-architecture planning phase.

### Step 2 - Preserve and reconcile Claude's audit

- Preserve Claude's report in `03-research-results/` as a faithful evidence summary, and retain the raw report when the repository environment can access it.
- Create a concise Kaizen reconciliation using `adopt`, `modify`, `reject`, and `defer`.
- Do not promote Claude's recommendations directly into accepted doctrine.

### Step 3 - Resolve the Operational Postgres and Observatory boundary - complete

- Decision 0009 establishes the broader Operational Postgres database and the Observatory as one bounded domain within it.
- Treat the proposed domains as initial planning boundaries, not permanent physical schemas:

```text
observatory
ingestion
automation
governance
audit
reference
```

- Decision 0009 explicitly amends the affected terminology and ownership scope in Decision 0004.
- After the decision is accepted, revise or split `05-specs/operational-postgres-authority.md` into the appropriate database-wide and Observatory-domain authority documents.

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

### Step 5 - Research Batch A: agent access and write safety - research complete

- Completed prompt: `02-research-prompts/003-agent-access-and-write-safety.md`
- Reconciliation: `03-research-results/008-agent-access-and-write-safety-claude-summary.md`
- Remaining planning outputs: staging-write wrapper specification, promotion-recovery specification, updated boundary tests, and implementation-roadmap prerequisites. Live Hermes tests are deferred to the implementation roadmap.

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

### Step 5A - Comparative MCP and typed-tool audits

Priority: P0/P1

Before selecting integration tools, compare prominent real implementations against Kaizen's authority and safety requirements.

#### Obsidian and filesystem tools

- Completed Obsidian audit prompt: `02-research-prompts/004-obsidian-mcp-tool-surface-audit.md`
- Reconciliation: `03-research-results/009-obsidian-mcp-tool-surface-audit-claude-summary.md`

Evaluate:

- Obsidian community-plugin MCP servers, including previously used `Vault as MCP` if verified
- direct filesystem MCP servers
- Obsidian Local REST API wrappers
- read-only versus read/write tool separation
- search, metadata, patch, delete, command-execution, and plugin dependencies
- Windows path confinement, junctions, symlinks, and audit logging
- preserve useful ecosystem capabilities rather than optimizing only for the smallest possible tool surface
- classify capabilities into portable access, Obsidian-aware navigation, Kaizen-governed writes, and human/admin operations
- apply final-path confinement and private-scope tests to read/navigation tools, not only write tools
- distinguish capability use from canonical truth ownership
- determine configure-versus-wrap per candidate based on actual tool discovery and credential granularity
- preserve both headless canonical access and interactive Obsidian-aware access

#### Postgres tools

- Completed Postgres audit prompt: `02-research-prompts/005-postgres-mcp-typed-tool-audit.md`
- Reconciliation: `03-research-results/010-postgres-mcp-typed-tool-audit-claude-summary.md`
- Leading Lane 1 candidate: PostgREST plus database roles/RLS/views/functions, pending disposable prototype and hammer evidence

Evaluate prominent Postgres MCP servers, database gateways, and typed API patterns for:

- read-only query surfaces
- parameterized, allowlisted queries
- project and domain scope injection
- row-level security and database roles
- schema introspection exposure
- mutation, DDL, migration, and transaction capabilities
- credential handling
- result-size and cost limits
- query logging and audit evidence
- whether unsafe SQL tools can be completely absent

Kaizen must not expose arbitrary SQL, direct credentials, unrestricted schema mutation, or agent-controlled migrations.

#### Qdrant tools

- Completed Qdrant audit prompt: `02-research-prompts/006-qdrant-mcp-typed-tool-audit.md`
- Reconciliation: `03-research-results/011-qdrant-mcp-typed-tool-audit-claude-summary.md`
- Leading Lane 1 candidate: typed retrieval gateway backed by scoped Qdrant JWT/RBAC and mandatory payload filters, pending disposable prototype and hammer evidence
- Required Lane 2 direction: Kaizen-owned headless indexer with provenance, drift detection, and full rebuild

Evaluate prominent Qdrant MCP servers, gateways, and typed API patterns for:

- scoped semantic search
- payload filtering and project isolation
- collection discovery and metadata exposure
- point upsert, delete, collection creation, and index mutation
- rebuild and reindex operations
- authority, supersedence, freshness, and privacy filters
- result-size, rate, and cost controls
- logging and traceability
- whether all mutation tools can be absent from agent-facing surfaces

Kaizen agents should normally receive constrained search and retrieval tools. Index construction, rebuilds, collection administration, and destructive mutation remain controlled operational capabilities.

#### Required output

For each candidate, record:

```text
maintenance and provenance
transport and authentication
tool inventory
read/write/admin separation
scope enforcement
Windows behavior where relevant
logging and auditability
known vulnerabilities or issue reports
Kaizen reuse/wrap/fork/reject recommendation
hands-on verification still required
```

Do not select a final server from README claims alone. Tool schemas and source code must be inspected, and high-risk candidates must be tested in disposable environments.
- Composed capability map: `03-research-results/012-step-5a-composed-tool-capability-map.md`

### Step 6 - Research Batch B: Internet Marketing Intelligence providers, source rights, and longitudinal reuse

Priority: P1

Status: research complete; steward reconciliation preserved in `03-research-results/015-internet-marketing-intelligence-providers-source-rights-claude-summary.md`

Research prompt:

- `02-research-prompts/007-internet-marketing-intelligence-providers-source-rights.md`

Current posture:

- provider, rights, retention, workload, VEDA-reuse, and provider-neutral architecture research is complete at the evidence level;
- the existing real DataForSEO JSON corpus and paired inventories remain the empirical baseline;
- no live provider collection occurred during Research Batch B;
- missing DataForSEO captures remain deferred and governed by `05-specs/deferred-dataforseo-llm-ranking-capture-packet.md`;
- execution requires separate owner and steward authorization;
- Ahrefs or another provider remains an optional calibration source rather than a routine duplicate collector;
- marketplace-ranking coverage remains a focused follow-up gap.

Required vision input:

- `01-project-standard/internet-marketing-intelligence-vision.md`

Research Batch B investigated the data foundation for Kaizen's portfolio-wide marketing signal engine rather than merely comparing provider feature lists.

Completed output:

- provider capability and coverage direction;
- source-rights and retention uncertainty classification;
- provider-neutral observation requirements;
- longitudinal reuse and project-relationship guidance;
- selective collection tiers;
- deferred DataForSEO capture packet;
- secondary-provider calibration posture;
- Decision 0010 evidence and database-design-session inputs;
- remaining marketplace-ranking research gap.

Evidence:

- `03-research-results/015-internet-marketing-intelligence-providers-source-rights-claude-summary.md`
- `05-specs/deferred-dataforseo-llm-ranking-capture-packet.md`
- `04-design-decisions/0010-dedicated-internet-marketing-intelligence-database.md` - accepted

### Step 6A - Research marketplace-ranking coverage before database design

Priority: P1

Status: planned; required before the owner/steward Internet Marketing Intelligence database-design session

Purpose:

- close the remaining marketplace-ranking gap left by Research Batch B;
- determine which project classes need marketplace-specific ranking and visibility intelligence;
- map relevant sources for Amazon, Etsy, Fiverr, merchant and shopping surfaces, app stores, plugin directories, software directories, and other project-specific marketplaces;
- distinguish marketplace data available through DataForSEO from data requiring marketplace-native APIs, exports, project-owned analytics, or another provider;
- identify source rights, retention, freshness, cost, and cross-project reuse constraints;
- define provider-neutral evidence families without designing tables or schemas.

Required output:

- marketplace and directory coverage matrix;
- project-class relevance matrix;
- primary versus secondary source recommendations;
- rights, retention, and cost constraints;
- missing empirical samples and later capture requirements;
- database-design-session inputs;
- explicit deferrals for unsupported or low-value surfaces.

Gate:

- complete and reconcile this addendum before the focused owner/steward Internet Marketing Intelligence database-design session;
- do not block acceptance of the high-level dedicated database boundary solely on this addendum;
- do not begin schema design from this step.

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
- `05-specs/staging-write-wrapper-and-promotion-recovery.md`
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
- A dry-run implementation-return path covering completion reports, tests, deviations, discoveries, and reviewed Kaizen updates.

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
- Implementation, operational, and Kaizen self-improvement feedback paths.

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
- `05-specs/staging-write-wrapper-and-promotion-recovery.md`
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

Phase 7 defines Hermes test plans and gates; it does not run live Hermes integration tests.

Define:

- canonical read/search tools
- staging-write wrapper
- permission profile
- provenance fields
- first read-only test plan, prerequisites, and evidence contract
- first staging-write test plan, prerequisites, and evidence contract
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
- comparative Obsidian, Postgres, and Qdrant tool-surface audit results
- explicit read, write, administrative, and destructive capability separation

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

- `IMPLEMENTATION_ROADMAP.md` - active first-slice roadmap under the acceleration amendment

## Hermes integration execution order

The implementation roadmap must place Hermes testing in this order:

### H1 - Read-only integration

Run `07-hermes/hermes-first-read-only-test.md` only after:

- a Kaizen integration environment exists;
- canonical read/search interfaces are available;
- a dedicated Hermes profile is captured;
- the exposed tool set is verified;
- write, patch, delete, move, terminal, and skill-management capabilities are absent or disabled;
- the target repository or vault is clean and backed up.

This test proves orientation and no-write behavior only. It does not prove staging-write safety.

### H2 - Wrapper and boundary hammer tests

Before Hermes receives any staging-write tool:

- implement the narrow staging-create wrapper;
- apply operating-system permissions;
- run traversal, absolute-path, UNC, extended-path, symlink, junction, race, overwrite, and audit-log probes in a disposable sandbox;
- obtain a human go/no-go verdict.

### H3 - Controlled staging-write integration

Run `07-hermes/hermes-first-staging-write-test.md` only after H2 passes and the validator, ID generator, provenance, and audit logging are available.

### H4 - Human-operated promotion and recovery

Implement and test canonical promotion only after the validation and recovery prerequisites exist. Hermes never receives canonical promotion authority.
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


### Human-interface implementation-planning inputs

Evidence:

- `03-research-results/013-kaizen-human-interface-architecture-claude-summary.md`
- `03-research-results/014-human-interface-technical-verification-and-amendment.md`
- `04-design-decisions/0011-progressive-hybrid-human-interface-direction.md` - proposed

Carry forward this staged direction:

```text
2A - canonical reader, validation, staging, promotion, CLI
2B - minimal client-neutral read model
first desktop-shell prototype
2C - Postgres prototype and scoped database lanes
2D - Qdrant retrieval and indexing prototype
later intelligence and operational panels
optional bridge plugin only if earned
```

The first desktop-shell prototype should focus on a minimal Home, Projects, Review Inbox, and Open-in-Obsidian flow. It must not require the entire database and retrieval architecture to be complete.

Later prototype gates should cover:

- desktop-to-Obsidian note and heading navigation;
- changed-since-review and promotion safety;
- application-command permission and capability selection;
- client-neutral canonical reads;
- intelligence pagination and evidence linking;
- optional bridge only if a real feature needs active-note context.

This lane remains a future implementation input and does not authorize UI implementation before its own roadmap gate.
## Immediate next actions

1. Close Result 025 Gate A documentation and backup findings.
2. Correct the two canonical bootstrap notes and validate them.
3. Preserve the owner-approved local-only development period; decide public/private remote visibility at the working-project checkpoint.
4. Add and review the adopted cross-process, abrupt-termination, malformed-log, and no-side-effect hammer tests.
5. Retire the current combined Packet 006 from approval consideration.
6. Draft and security-audit Packet 006A for the Windows first-time atomic-install primitive.
7. Draft Packet 006B only after 006A passes and is steward-audited.
8. Keep canonical promotion, Hermes/MCP writes, databases, providers, retrieval, and UI implementation unauthorized until their gates close.