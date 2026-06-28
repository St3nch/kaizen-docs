# Kaizen Roadmap v0.4

Status: accepted and active; owner-accepted in `03-research-results/322-roadmap-v0.4-owner-acceptance.md`
Date: 2026-06-21
Owner: Kaizen project steward

## Purpose

This proposed roadmap extends the accepted post-Milestone-12 reading path into an end-to-end roadmap for both:

```text
Kaizen v1 internal operating completion
Full Kaizen project completion
```

It exists to prevent roadmap drift after Milestone 12 and Packet 018A. It does not authorize implementation, Milestone 13, platform mutation, vault mutation, database mutation, production work, provider work, customer-data work, deletion, or new doctrine.

`ROADMAP_V0.4.md` supersedes `ROADMAP_V0.3.md` as the accepted active roadmap after owner acceptance in `03-research-results/322-roadmap-v0.4-owner-acceptance.md`.

## Current checkpoint

```text
Kaizen Project Standard v0.2: authoritative
Active current-state pointer: 00-entrypoint/LLM_START_HERE.md
Active accepted roadmap: ROADMAP_V0.4.md
Previous accepted roadmap: ROADMAP_V0.3.md; superseded by Result 322
Milestones 1-14: closed
Latest milestone closure: Milestone 14 / Kaizen v1, owner-accepted in Result 368
Post-M12 audit program: Passes 1-3 complete and dispositioned in Results 313, 314, and 316
Packet 018A reading-path cleanup: implemented, audited, corrected, and owner-accepted in Results 318-321
SP-1: closed under Result 326
Milestone 13: closed and owner-accepted in Result 342
Milestone 14 / Kaizen v1: owner-accepted in Result 368
Platform HEAD at Packet 018A approval: b7593c5ee90fd32c1e2a86572cc570d307de2be6
Vault HEAD at Packet 018A approval: c898f261c0b341eb8419125247c8bd53ef567d6c
Live operational database: kaizen_ops migrated through 0005_recovery_retention_integrity
Current active lane: post-v1 hardening priority selection after completed 021B / 021C / 021D / 021E / 021F vault alignment, lean docs read-path refresh, Result 382 backup verification record, and Result 383 backup v2 key-custody recovery / restore proof record
```

## Project boundary definitions

### Kaizen

Kaizen is the governed project-intelligence system.

Kaizen helps projects move through:

```text
idea capture
-> research
-> source summaries
-> claims
-> decisions
-> specs
-> audits
-> task packets
-> coding-agent implementation
-> implementation return
-> project improvement after launch
```

Kaizen's purpose is to help projects become real, preserve intelligence, support safe agent work, govern decisions, and improve projects over time.

Kaizen is not a specific business, client service, marketplace workflow, or multi-agent business operator.

### Neon Ronin

Neon Ronin is a downstream project: a multi-agent business operating system for managing small businesses.

Kaizen may help plan, govern, build, audit, improve, and preserve intelligence for Neon Ronin.

Neon Ronin is not Kaizen.

### SearchClarity

SearchClarity is a business/service idea under Neon Ronin.

SearchClarity may provide example workloads, evidence, and perspective for what Kaizen's Observatory / Internet Marketing Intelligence capability needs to support.

SearchClarity is not Kaizen and is not a subsystem required to declare Kaizen complete.

### Observatory / Internet Marketing Intelligence

Observatory / Internet Marketing Intelligence is a bounded Kaizen intelligence capability for lawful, rights-cleared observations and project-improvement signals.

It may support downstream projects such as Neon Ronin, SearchClarity, and future business projects. It is not identical to those projects.

## Architecture anchor

The accepted architecture anchor remains:

```text
Markdown / Obsidian = canonical project intelligence
Operational Postgres database = operational source of truth for governed structured records; Observatory is one bounded intelligence domain
Qdrant = rebuildable semantic retrieval index over approved Markdown
Hermes / agents = constrained clerks through approved typed tools
Humans = authority-bearing review, approval, and promotion
```

Additional long-term architecture direction:

```text
Obsidian remains the primary human workspace for canonical Markdown intelligence.
The Kaizen vault is the living project-intelligence layer, not an evidence dump; vault admission is limited to durable project intelligence that helps projects continue, govern, execute, and improve.
Full-fidelity governance proof, raw prompts, raw tool output, immutable evidence, backup hash chatter, and detailed implementation-return records remain in their appropriate docs, staging/evidence, operational, recovery, source-repository, or archive layers unless distilled through governed review.
This admission rule applies immediately during development and must carry into launched Kaizen projects.
A progressive hybrid human interface is planned through reviewed decisions and later gates.
Tauri is the leading desktop-shell candidate, not irreversible doctrine.
Operational records belong in typed service boundaries, not arbitrary SQL access.
Qdrant owns no truth and must remain rebuildable.
Hermes and other agents are clerks, not authorities.
Observatory / IMI must fail closed on unclear rights, retention, legal, provider, and customer-data questions.
```

## Kaizen v1 definition

Kaizen v1 complete means:

```text
A local, single-steward, governed project-intelligence system can run at least one real, non-self internal project end-to-end through the governed loop with reliable docs, operational records, typed tools, backup/restore confidence, fresh-context resumption, and a safe local operator surface.
```

Kaizen v1 is the internal operating core. It does not need every full-system capability, but it must preserve the roadmap for the full system.

### v1 must include

```text
canonical Markdown / vault intelligence for the project loop
basic Obsidian workspace over canonical Markdown
kaizen_core operational foundation hardened for real use
typed operational APIs and safe local tool surface
minimal operator command surface
implementation-return loop proven on one real non-self internal project
fresh-context resumption on that real project
backup/restore confidence for the project data
human approval and review boundaries preserved
```

### v1 should include if cheap

```text
P3 preservation / disposition register surfaced
milestone-closeout reading-path refresh discipline operationalized
operational-record authority / rebuildability explicitly decided or deferred
```

### not required for v1 but required for full completion

```text
dynamic Obsidian experience
Tauri or equivalent progressive desktop shell
Qdrant retrieval and governed context packs
Hermes constrained-clerk integration
Observatory / Internet Marketing Intelligence foundation
project-improvement intelligence loop
portfolio support across multiple downstream projects
```

### unsafe or premature for v1

```text
provider paid collection
logged-in scraping
customer-data reuse
production cloud deployment
multi-user / tenant identity
autonomous business action
production MCP packaging
```

## Full Kaizen project completion definition

Full Kaizen project complete means:

```text
The intended living, agent-connected project-intelligence system is operating across the owner's project portfolio, helping downstream projects become real and improve through canonical Markdown, operational records, retrieval, constrained agents, human review, Observatory / IMI evidence, and project-improvement loops.
```

Full completion must include:

```text
Kaizen v1 internal operating core
dynamic Obsidian experience over canonical Markdown
client-neutral typed read model
Qdrant retrieval and governed context packs
Hermes / agent constrained-clerk workflows
progressive hybrid human interface / Tauri or successor shell
Observatory / Internet Marketing Intelligence foundation
project-improvement intelligence loop
portfolio support across multiple downstream projects
backup / restore posture maintained across the operating system
human authority boundaries preserved
```

Full completion does not mean:

```text
SearchClarity is implemented as part of Kaizen
Neon Ronin is implemented as part of Kaizen
Kaizen becomes a SaaS product
multi-user commercial tenancy is complete
autonomous business execution is authorized
provider/customer data may be used without rights and legal gates
```

## Closed milestone traceability

| Milestone | Name | Definition / spec | Owner acceptance | Closure result / audit | Owner closure acceptance | Residuals / notes |
|---|---|---|---|---|---|---|
| 1 | Tools foundation | Early first-slice roadmap and packet chain | Packet results 018-025 | Closed in first-slice chain | Captured in historical roadmap/results | Historical first-slice evidence |
| 2 | Canonical vault bootstrap | Early first-slice roadmap and packet chain | Packet results 018-025 | Closed in first-slice chain | Captured in historical roadmap/results | Historical first-slice evidence |
| 3 | Safe staging and promotion | Early first-slice roadmap and packet chain | Packet results 018-025 | Closed in first-slice chain | Captured in historical roadmap/results | Historical first-slice evidence |
| 4 | One governed project loop | `IMPLEMENTATION_ROADMAP.md` | Packet 009A/009B approvals | `03-research-results/062-milestone-4-implementation-return-closure-audit.md` | Result 062 closure audit | Historical first-slice completion |
| 5 | Slice retrospective and v0.2 consolidation | `01-project-standard/kaizen-project-standard-v0.2.md` | Result 067 | Result 067 | Result 067 | Project Standard v0.2 authoritative |
| 6 | Governed operator surface | `05-specs/milestone-6-governed-operator-surface.md` | Result 069 and Packet 010 approvals | Results 094/095 | Results 094/095 | Historical operator-surface foundation |
| 7 | Durable recoverability | `05-specs/milestone-7-durable-recoverability.md` | `03-research-results/104-milestone-7-owner-acceptance.md` | `03-research-results/131-milestone-7-final-closure-audit.md` | `03-research-results/132-milestone-7-owner-closure-acceptance.md` | Some survivability and cleanup work carried forward |
| 8 | Reliability and known-defect closure | `05-specs/milestone-8-reliability-and-known-defect-closure.md` | `03-research-results/134-milestone-8-owner-acceptance.md` | `03-research-results/161-milestone-8-closure-security-steward-audit.md` | `03-research-results/162-milestone-8-owner-closure-acceptance.md` | KZA audit-tail register tracked by P3-004 |
| 9 | Controlled implementation-return pilot | `05-specs/controlled-implementation-return-pilot.md` | Results 172/174 and Packet 013/014 approvals | `03-research-results/243-milestone-9-closure-audit.md` | `03-research-results/244-packet-014a-and-milestone-9-owner-closure-acceptance.md` | Spec filename is non-numbered by design; sealed-oracle waiver disclosed |
| 10 | Workload and system-of-record reconciliation | `05-specs/milestone-10-workload-and-system-of-record-reconciliation.md` | `03-research-results/246-milestone-10-specification-and-packet-015a-owner-acceptance.md` | `03-research-results/249-milestone-10-reconciliation-audit.md` | `03-research-results/250-packet-015a-and-milestone-10-owner-closure-acceptance.md` | Decision 0019 accepted |
| 11 | Operational data foundation | `05-specs/milestone-11-operational-data-foundation-planning.md` plus supporting M11 specs | `03-research-results/254-milestone-11-specification-and-packet-016a-owner-acceptance.md` | `03-research-results/288-packet-016f-owner-completion-acceptance-and-milestone-11-closure.md` | Result 288 | Post-closure 016G complete under Result 293; M11 raw audit Passes 4-9 tracked by P3-001 |
| 12 | Recovery realism and operational restore proof | `05-specs/milestone-12-recovery-realism-and-operational-restore-proof.md` | Results 296 and 298 | `03-research-results/306-milestone-12-closure-assessment.md` | `03-research-results/307-milestone-12-owner-acceptance.md` | Restore-forward, operational role-ready restore, ruff/tooling, and historical zero-file generations remain deferred |

Result 312 and Result 314 accept that Milestones 7 through 12 are clean and followable end-to-end. Do not broadly re-audit or reopen closed milestones without concrete contradictory evidence.

## Remaining v1 roadmap

### SP-1 — P3 governance-backlog closure register

Status: side packet candidate; not a milestone; not authorized.

Purpose:

```text
Preserve and disposition P3-001 through P3-009 so historical audit signals do not get lost.
```

Why it belongs here:

```text
It is low-risk documentation hygiene that protects evidence lineage without blocking the roadmap or consuming the main milestone lane.
```

Entry criteria:

```text
Result 316 repair register accepted as source
owner approves bounded docs-only side packet
```

Exit criteria:

```text
P3-001 through P3-009 are each recorded as closed, deferred, rejected, superseded, or tracked
no missing M11 audit text is fabricated
Decision 0015 is not fabricated
P3-008 is carried to the Observatory / IMI research gate
P3-009 closeout refresh discipline is carried into roadmap governance
```

Explicitly does not authorize:

```text
platform work
vault mutation
database mutation
Decision 0015 doctrine
M13 implementation
reopening closed milestones
```

### Milestone 13 — Operational service and safe local tool-surface hardening

Status: recommended next milestone candidate; not authorized.

Purpose:

```text
Make Kaizen's operational service and local tool surface safe enough to support a real internal project.
```

Why it comes here:

```text
Milestone 11 built the operational data foundation, but a real v1 proof requires caller-facing service safety, bounded tool behavior, recovery confidence, and operational-record authority to be hardened before a real project runs through Kaizen.
```

Entry criteria:

```text
Milestones 1-12 closed
Packet 018A accepted
ROADMAP_V0.4 accepted
SP-1 complete or explicitly owner-deferred
Milestone 13 definition drafted, audited, and owner-accepted
```

Exit criteria:

```text
typed service boundaries verified
safe local tool surface verified
minimal operator command surface proven on synthetic data
no arbitrary SQL
no uncontrolled mutation
bounded command envelopes
idempotency and lock behavior tested
retention and recovery behavior tested
operational-record authority / rebuildability decision recorded or explicitly deferred
```

Required proof:

```text
service-layer audit completion
hammer / failure coverage where appropriate
tool-contract inventory
operator-surface spec
implementation return
closure audit
owner closure acceptance
```

Explicitly does not authorize:

```text
production MCP packaging
Tauri UX implementation
Hermes write authority
Qdrant
Observatory / IMI implementation
provider/customer-data work
Neon Ronin implementation
SearchClarity implementation
production deployment
multi-user identity
```

Dependencies:

```text
Milestone 11 operational foundation
Milestone 12 recovery realism evidence
Packet 018A accepted reading path
SP-1 if not owner-deferred
```

### Milestone 14 — First real internal project end-to-end governed run

Status: recommended final v1 milestone; not authorized.

Purpose:

```text
Prove Kaizen v1 by running one real, non-self internal project through the full governed project loop.
```

Why it comes here:

```text
Kaizen v1 must be proven on real work. Northstar was a controlled pilot and Kaizen itself is self-referential. v1 needs a real downstream project workload.
```

Entry criteria:

```text
Milestone 13 closed
real internal project selected
project-specific boundaries documented
backup / restore plan ready for the project data
fresh-context resumption test plan ready
```

Exit criteria:

```text
real project onboarded into Kaizen
research -> spec -> audit -> task packet -> implementation -> implementation return loop completed
fresh-context resumption demonstrated
operational records generated by real work
backup taken and restore-verified for that project data
v1 completion audit completed
owner accepts Kaizen v1
```

Required proof:

```text
project governed artifact chain
operational records from real workload
implementation-return evidence
fresh-context resumption evidence
backup and restore proof
v1 closure audit
owner v1 acceptance
```

Explicitly does not authorize:

```text
Qdrant
Hermes write authority
Tauri shell
Observatory / IMI implementation
provider/customer-data work
commercial operations
production deployment
multi-user identity
Neon Ronin implementation beyond the selected bounded workload
SearchClarity implementation beyond the selected bounded workload
```

Candidate workloads:

```text
Neon Ronin planning/build lane
SearchClarity as a Neon Ronin business/service lane
another internal project selected by owner
```

Important boundary:

```text
The selected workload proves Kaizen. It does not become Kaizen.
```

## Post-v1 / full-system roadmap

### Milestone 15 — Dynamic Obsidian experience and typed read model

Status: post-v1 full-system candidate; not authorized.

Purpose:

```text
Make Kaizen's canonical Markdown intelligence easier to navigate, review, resume, and operate without creating a second source of truth.
```

Includes:

```text
dynamic command centers
review queues
resume views
current-state dashboards
client-neutral typed read model
basic governed context-pack assembly
```

Does not authorize:

```text
Tauri shell implementation unless separately gated
Qdrant
Hermes write authority
Observatory / IMI implementation
```

### Milestone 16 — Qdrant retrieval and governed context packs

Status: post-v1 full-system candidate; not authorized.

Purpose:

```text
Provide rebuildable semantic retrieval over approved Kaizen Markdown and governed context-pack assembly.
```

Boundary:

```text
Qdrant owns no truth. Markdown remains canonical.
```

Entry gate:

```text
approved Markdown corpus of real size
retrieval contracts that preserve scope, authority, supersedence, freshness, and privacy
```

### Milestone 17 — Hermes constrained-clerk integration

Status: post-v1 full-system candidate; not authorized.

Purpose:

```text
Allow Hermes and other agents to assist through approved typed tools as constrained clerks.
```

Boundary:

```text
no approval authority
no promotion authority
no arbitrary SQL
no direct canonical writes
no spending
no publication
```

Entry gate:

```text
read-only test lane
boundary hammer tests
staging-write proof before any write authority
```

### Milestone 18 — Progressive hybrid UX / Tauri operator shell

Status: post-v1 full-system candidate; not authorized.

Purpose:

```text
Build the future Kaizen operator interface over typed services.
```

Includes:

```text
Tauri or equivalent desktop shell
operator review surface
evidence cards
agent/job status
health and cost views
Obsidian navigation
```

Boundary:

```text
Obsidian remains the canonical workspace.
The shell is not a second source of truth.
Tauri remains leading candidate until accepted by a later implementation gate.
```

### Milestone 19 — Observatory / Internet Marketing Intelligence foundation

Status: post-v1 full-system candidate; not authorized.

Purpose:

```text
Stand up Kaizen's bounded intelligence domain for lawful, rights-cleared observations that can help downstream projects improve.
```

Correct framing:

```text
Observatory / IMI supports projects like Neon Ronin and SearchClarity.
It is not SearchClarity itself.
```

Includes:

```text
kaizen_intelligence boundary
provider-neutral observation model
rights, freshness, provenance, and retention fields
governed insight summaries
market / project signal tracking
```

Entry gates:

```text
OBR-01 through OBR-15 dispositioned proportionately
provider rights resolved
legal / customer-data boundaries resolved
focused database-design session completed
```

Does not authorize:

```text
logged-in scraping
unknown-rights data capture
customer-data reuse
provider purchase without approval
SearchClarity implementation
Neon Ronin implementation
```

### Milestone 20 — Project-improvement intelligence loop

Status: post-v1 full-system candidate; not authorized.

Purpose:

```text
Use Observatory / IMI and Kaizen records to produce evidence-backed improvement candidates for downstream projects.
```

Includes:

```text
project lenses
derived signals
opportunity candidates
strategy candidates
experiment proposals
outcome tracking
learning loop
```

Example downstream workloads:

```text
Neon Ronin
SearchClarity
future small-business projects
```

Boundary:

```text
Kaizen recommends, structures, and tracks intelligence.
Humans approve business action.
Neon Ronin may execute business operations later.
```

### Milestone 21 — Kaizen full-system portfolio support acceptance

Status: recommended final full-project milestone; not authorized.

Purpose:

```text
Prove Kaizen can support multiple real downstream projects through the full project-intelligence and improvement loop.
```

Completion meaning:

```text
Kaizen is operating across multiple real projects, preserving canonical intelligence, supporting governed agent work, enabling retrieval, surfacing improvement opportunities, tracking outcomes, and compounding learning across the portfolio.
```

Required proof:

```text
multiple downstream projects onboarded
canonical project intelligence preserved
operational records generated
retrieval / context packs working
agent-clerk workflows bounded
Observatory / IMI producing governed insights
project-improvement loop exercised
human approval boundaries preserved
backup / restore posture maintained
```

Example downstream projects:

```text
Neon Ronin
SearchClarity as a Neon Ronin business/service line
future projects
```

Intentionally outside scope:

```text
commercial SaaS deployment
multi-user tenant platform
autonomous business execution
Neon Ronin implementation as a separate project
SearchClarity implementation as a separate business line
```

## Capability matrix

| Capability | Current status | v1 relevance | Full-project relevance | Recommended phase | Authorization status |
|---|---|---|---|---|---|
| Markdown / vault canonical intelligence | Proven through earlier milestones | Required | Foundational | Done; carry forward | Accepted foundation |
| Obsidian human workspace | Basic use; dynamic experience deferred | Basic required | Dynamic required | v1 basic; M15 dynamic | Basic accepted; dynamic gated |
| Operational Postgres / kaizen_core | Built through M11; live through 0005 | Required | Required | M13 hardening | Planning only |
| Typed operational APIs | Built/planned through M11 | Required | Required | M13 | Planning only |
| Safe local tool surface | Go8/local tools exist | Required | Required | M13 | Planning only |
| Minimal operator console | Historical operator surface exists | Required | Required | M13 | Planning only |
| Tauri or equivalent UX | Leading candidate, not final | Not required | Required | M18 | Deferred/gated |
| Progressive hybrid interface | Proposed direction | Not required | Required | M15/M18 | Deferred/gated |
| Obsidian bridge/navigation | URI/deep-link preferred first | Deferred | Optional if friction proves need | Later optional | Evidence-gated |
| Hermes agent integration | Deferred | Not required | Required | M17 | Deferred/gated |
| Qdrant semantic retrieval | Not implemented | Not required | Required | M16 | Deferred/gated |
| Observatory DB / IMI | Accepted boundary; no implementation | Not required | Required | M19 | Research-only until gates |
| Project-improvement intelligence loop | Not implemented | Not required | Required | M20 | Deferred/gated |
| Neon Ronin | Downstream project | Possible v1 workload | Portfolio workload | M14/M21 as workload | Separate project |
| SearchClarity | Neon Ronin business/service idea | Possible v1 workload | Portfolio workload/use case | M14/M21 as workload | Separate project/business line |
| Provider rights/data collection | Unresolved | Blocked | Gate for IMI | OBR research before M19 | Blocked |
| Customer-data reuse/deletion doctrine | Unresolved/legal | Blocked | Gate for IMI | OBR/legal before M19 | Blocked |
| Multi-user identity | Not designed | Not required | Outside current full roadmap | Separate roadmap | Deferred beyond roadmap |
| Backup/restore hardening | Baseline proven; residuals accepted | Required baseline | Required stronger posture | M13/M14 baseline; later hardening | Baseline accepted; future gated |
| Fresh-context resumption | Proven on Northstar synthetic | Required real proof | Required | M14 | Planning only |
| P3 audit/governance backlog | Tracked | Should surface | Ongoing discipline | SP-1 | Side packet only |
| Production packaging/deployment | Not designed | Not required | Outside current full roadmap | Separate roadmap | Deferred beyond roadmap |

## Downstream project / workload table

| Project or workload | Role relative to Kaizen | Boundary |
|---|---|---|
| Northstar | Synthetic/controlled pilot evidence | Not a product goal |
| Neon Ronin | Downstream project; multi-agent business OS | Not Kaizen; may be selected as real v1 workload |
| SearchClarity | Business/service idea under Neon Ronin | Not Kaizen; may inform Observatory/IMI needs or serve as workload |
| Future small-business projects | Future Kaizen-supported workloads | Not Kaizen subsystems |

## Observatory / IMI gate table

| Gate | Purpose | Required before |
|---|---|---|
| OBR-01 through OBR-15 disposition | Rights, retention, measurement, provider, and legal clarity | Any Observatory / IMI implementation planning |
| Provider rights resolution | Confirm allowed collection and reuse | Provider purchase or capture |
| Customer-data legal / deletion doctrine | Prevent unlawful reuse or retention | Customer-data workflows |
| Focused database-design session | Define kaizen_intelligence boundary from evidence | M19 implementation planning |
| Owner-approved bounded capture | Prevent accidental provider/customer-data work | Any real data capture |

## Deferred / blocked / post-v1 table

| Capability | Disposition | Named gate before work |
|---|---|---|
| Qdrant | Post-v1 M16 | Approved Markdown corpus and retrieval contracts |
| Hermes | Post-v1 M17 | Read-only test, boundary hammer tests, staging-write proof |
| Tauri / progressive UX | Post-v1 M15/M18 | Decision 0011 continuation and prototype evidence |
| Observatory DB / IMI | Post-v1 M19 | OBR gates and DB-design session |
| Project-improvement intelligence loop | Post-v1 M20 | M19 governed observations and measurement doctrine |
| Provider collection | Blocked | Written rights and owner-approved bounded capture |
| Customer-data reuse | Blocked | Legal/contract review and deletion doctrine |
| Multi-user identity | Deferred beyond current roadmap | Separate roadmap |
| Production deployment / SaaS | Deferred beyond current roadmap | Separate commercialization/deployment roadmap |
| Neon Ronin implementation | Separate project | Own Kaizen-governed project lane |
| SearchClarity implementation | Separate Neon Ronin business/service lane | Own Kaizen/Neon-Ronin-governed project lane |

## P3 backlog pointer

P3 remains tracked under Result 316 and must not be treated as closed by this roadmap.

```text
P3-001 M11 Passes 4-9 / final synthesis preservation gap
P3-002 M11 A005 test-count reconciliation gap
P3-003 M11 B001/B006/B010 low still-open items
P3-004 M6-8 KZA per-ID disposition register
P3-005 Decision 0015 reserved/gap record
P3-006 decisions index README stale
P3-007 Result 302 expansion sweep
P3-008 watchlist plus Observatory/OBR backlog families
P3-009 milestone-closeout refresh process repair
```

Disposition:

```text
SP-1 side cleanup packet + durable backlog register + closeout discipline, not the main next milestone.
```

## Standing prohibitions

Until each milestone or research track receives its own accepted definition and task packet:

- no Milestone 13 implementation;
- no production MCP migration or packaging;
- no physical Postgres or Observatory schema beyond accepted operational foundation boundaries;
- no Observatory data collection, provider purchase, crawling, logged-in marketplace collection, or customer-data reuse;
- no Qdrant, LangGraph, Hermes, broad UI, Tauri, or Internet Marketing Intelligence implementation;
- no Neon Ronin implementation as part of Kaizen roadmap work;
- no SearchClarity implementation as part of Kaizen roadmap work;
- no vault push or remote creation;
- no generalized correction, delete, move, rollback, or multi-note mutation semantics;
- no retention deletion or physical evidence deletion;
- no Decision 0015 doctrine creation without owner approval;
- no fabrication of missing M11 audit text;
- no inferred owner approval.

## Required gate for every milestone

```text
evidence
-> milestone definition
-> security/steward audit
-> exact owner acceptance
-> task packet
-> separate implementation approval
-> implementation and tests
-> governed return
-> closure audit
-> owner closure acceptance
-> active reading-path refresh check
```

## v1 acceptance criteria

Kaizen v1 may be declared only when:

```text
Milestone 13 is closed and owner-accepted
one real non-self internal project is selected and bounded
that project completes the governed project loop through implementation return
fresh-context resumption is demonstrated on that project
operational records are generated by real work
backup and restore are verified for that project data
human approval boundaries are preserved
v1 closure audit passes
owner explicitly accepts Kaizen v1
```

## Full-system acceptance criteria

Full Kaizen project completion may be declared only when:

```text
Kaizen v1 is closed and owner-accepted
multiple downstream projects are onboarded
canonical project intelligence is preserved across the portfolio
operational records support real portfolio work
Qdrant retrieval and context packs are working and rebuildable
Hermes / agent-clerk workflows are bounded and useful
progressive interface supports review, resumption, and operation
Observatory / IMI produces governed insights from rights-cleared evidence
project-improvement loop is exercised and measured
human approval boundaries remain intact
backup / restore posture remains maintained
full-system closure audit passes
owner explicitly accepts full Kaizen completion
```

## Roadmap change rule

This roadmap is accepted and active after owner acceptance in Result 322. Any change to the milestone sequence, v1 definition, full-completion definition, project boundaries, or standing prohibitions requires a governed roadmap amendment.

## Immediate next planning work

ROADMAP_V0.4 is accepted and active under Result 322. SP-1 is closed under Result 326. Milestone 13 is closed under Result 342. Milestone 14 / Kaizen v1 is owner-accepted under Result 368.

Current active planning lane:

```text
021B current-state vault amendment: executed, verified, committed locally, and returned in Result 378
021C command-center vault amendment: complete
021D overview vault amendment: complete
021E command-center stale-sequence cleanup: executed, verified, and committed locally
021F current-state stale-sequence cleanup: executed, verified, and committed locally
lean docs read-path refresh for LLM_START_HERE.md and ROADMAP_V0.4.md: complete
022A / 022B / 022C post-v1 backup generation, transfer verification, and plaintext cleanup: recorded in Result 382
022F / 022G / 022H / 022I / 022J backup v2 key-custody recovery, fresh backup generation, transfer verification, restore proof, and plaintext cleanup: recorded in Result 383
022Q vault admission doctrine hardening: vault commit 6fbc8cefe4ed4cd57403648ee3977ee2aeaaba5b; docs commit 51eea6fe3f6a87974ec7af85b066e8ebf7b8e929
022S lean-record index cleanup: committed and pushed at docs commit 3418673ad12487d0d76a9591521f82ae3cab45b8
-> current gate: select the next bounded post-v1 hardening priority after 022S lean-record index cleanup
-> later: docs repo / Neon Ronin sync posture, Claude UX / operator-workflow planning, vault promotion flow planning, and broader post-v1 hardening backlog
```

Kaizen v1 is accepted. Post-v1 work remains governed: vault amendment preparation, plan generation, execution, future implementation, repository push, vault promotion, platform mutation, database mutation, downstream project mutation, and Observatory / IMI work require exact owner approval for the relevant packet, operation, plan hash, starting commits, path scope, tests, and non-authorization boundaries.

Lean-record cleanup is now an explicit post-v1 hardening item. Kaizen must optimize for LLM readability and operational follow-through, not accumulate a junk drawer of development recordings. Cleanup should preserve decision-critical, audit-critical, recovery-critical, and source-of-truth boundary details, while deleting, consolidating, or marking historical any redundant implementation chatter that no longer helps Kaizen operate.
