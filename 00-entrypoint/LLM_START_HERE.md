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

- Current phase: Milestones 1 through 12 are closed; Milestone 13 definition is accepted and active for planning.
- Latest closed milestone: Milestone 12, accepted by owner in `03-research-results/307-milestone-12-owner-acceptance.md`.
- Current gate: Milestone 13 planning and Claude-review remediation only. No Milestone 13 implementation, platform mutation, vault mutation, database mutation, production, deletion, provider/customer-data, Qdrant, Hermes, Tauri, Observatory, Neon Ronin, or SearchClarity work is authorized by this entrypoint.
- Active accepted planning roadmap: `ROADMAP_V0.4.md`, owner-accepted in `03-research-results/322-roadmap-v0.4-owner-acceptance.md` and corrected through SP-1 / M13 planning updates.
- Prior roadmap: `ROADMAP_V0.3.md`, preserved as the accepted Packet 018A roadmap surface before ROADMAP_V0.4.
- Post-M12 audit program: Passes 1 through 3 are complete and dispositioned in Results 310 through 316.
- Packet 018A reading-path cleanup is accepted in `03-research-results/321-packet-018a-owner-acceptance.md`.
- SP-1 P3 backlog register is accepted in `03-research-results/326-sp-1-owner-acceptance.md`; the M13 entry precondition is satisfied.
- Milestone 13 definition is accepted in `03-research-results/328-milestone-13-definition-owner-acceptance.md`.
- Packet 019A inventory is recorded in `03-research-results/329-packet-019a-m13-inventory-and-readiness-audit.md`; Claude review required an inventory-completion addendum and reading-path refresh.
- Active M13 packet drafts: 019B service-boundary proof, 019C local tool-surface approval, and 019D synthetic proof / M14 readiness contract.
- Platform repository: local-only at commit `b7593c5ee90fd32c1e2a86572cc570d307de2be6`; no remote exists.
- Docs repository: active M13 planning is ahead of `origin/main`; verify current HEAD before work.
- Canonical vault: local-only at commit `c898f261c0b341eb8419125247c8bd53ef567d6c`; no remote exists.
- Live operational database: `kaizen_ops` is migrated through `0005_recovery_retention_integrity`.
- Go8 repository: operational local tool server.
- Kaizen MCP remains a temporary non-Git proving ground; it is not production infrastructure.
- Observatory research remains a parallel evidence track only; no provider purchase, raw capture, crawler deployment, client-data reuse, physical schema, Qdrant, LangGraph, MCP-tool implementation, or hammer execution is authorized.
- Qdrant index: not implemented.
- Hermes Desktop / Hermes Agent: deferred; no canonical write authority or live integration is authorized.
- Accepted decisions remain governing inputs. Decision 0015 is a known reserved / unauthored / owner-deferred governance-compression gap; do not present it as accepted doctrine and do not fabricate it.
- Active workflow posture: move efficiently on low-risk planning and review fixes; preserve full gates for consequential mutation and implementation.
- Stewardship principle: structure and automation must earn their existence.

## Read-first sequence

This numbered list is a task-dependent reference map, not a requirement to load every file. Always read this entrypoint and active accepted `ROADMAP_V0.4.md` for roadmap work. Then load only the decisions, specs, audits, task packets, and implementation returns relevant to the requested work unless the user explicitly requests a full-repository audit.

1. `00-entrypoint/LLM_START_HERE.md` - this file; authoritative current-state pointer
2. `ROADMAP_V0.4.md` - current accepted roadmap for Kaizen v1 and full-project completion
3. `ROADMAP_V0.3.md` - previous Packet 018A roadmap surface
4. `01-project-standard/kaizen-project-standard-v0.2.md` - authoritative Kaizen Project Standard v0.2
5. `03-research-results/307-milestone-12-owner-acceptance.md` - latest milestone closure authority
6. `03-research-results/326-sp-1-owner-acceptance.md` - SP-1 closure / M13 entry precondition satisfied
7. `05-specs/milestone-13-operational-service-and-safe-local-tool-surface-hardening.md` - accepted M13 definition
8. `03-research-results/328-milestone-13-definition-owner-acceptance.md` - M13 definition owner acceptance
9. `03-research-results/329-packet-019a-m13-inventory-and-readiness-audit.md` - Packet 019A inventory
10. `03-research-results/333-milestone-13-claude-review-steward-disposition.md` - Claude review disposition
11. `03-research-results/334-packet-019a-inventory-completion-addendum.md` - 019A inventory-completion addendum
12. `03-research-results/330-packet-019b-service-boundary-proof-task-packet.md` - Packet 019B service-boundary proof draft
13. `03-research-results/331-packet-019c-local-tool-surface-approval-task-packet.md` - Packet 019C local tool-surface approval draft
14. `03-research-results/332-packet-019d-synthetic-proof-and-m14-readiness-contract-task-packet.md` - Packet 019D readiness-contract draft
15. `03-research-results/310-post-m12-audit-source-inventory.md` - post-M12 audit Pass 0 inventory
16. `03-research-results/311-post-m12-roadmap-and-entrypoint-audit.md` - post-M12 audit Pass 1
17. `03-research-results/312-post-m12-milestone-traceability-audit.md` - post-M12 audit Pass 2
18. `03-research-results/313-post-m12-audit-pass-1-steward-disposition.md` - Pass 1 disposition
19. `03-research-results/314-post-m12-audit-pass-2-steward-disposition.md` - Pass 2 disposition
20. `03-research-results/315-post-m12-historical-disposition-audit.md` - post-M12 audit Pass 3
21. `03-research-results/316-post-m12-audit-pass-3-steward-disposition.md` - Pass 3 disposition and tracked P3 repair register
22. `04-design-decisions/README.md` - decisions index and Decision 0015 gap note
23. `04-design-decisions/0019-milestone-10-system-of-record-reconciliation.md`
24. `04-design-decisions/0020-milestone-11-operational-foundation-boundaries.md`
25. `05-specs/milestone-12-recovery-realism-and-operational-restore-proof.md`
26. `05-specs/milestone-11-operational-data-foundation-planning.md`
27. `05-specs/milestone-10-workload-and-system-of-record-reconciliation.md`
28. `05-specs/controlled-implementation-return-pilot.md` - Milestone 9 definition/spec despite non-numbered filename
29. `05-specs/milestone-8-reliability-and-known-defect-closure.md`
30. `05-specs/milestone-7-durable-recoverability.md`
31. `IMPLEMENTATION_ROADMAP.md` - historical completed first-slice roadmap for Milestones 1 through 5
32. `IMPLEMENTATION_ROADMAP_V0.2.md` - historical accepted Milestone 6 implementation-roadmap snapshot; preserve bytes unless separately approved
33. `ROADMAP.md` - historical planning provenance only; not a current-gate source
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

The active sequence is now at a post-Milestone-12 cleanup and roadmap decision point:

```text
Milestones 1-8: closed
-> Milestone 9 controlled implementation-return pilot: closed under Result 244
-> Milestone 10 workload/system-of-record reconciliation: closed under Result 250
-> Milestone 11 operational data foundation: closed under Result 288
-> Packet 016G post-closure attestation-integrity correction: complete under Result 293
-> Milestone 12 recovery realism and operational restore proof: closed under Result 307
-> Post-Milestone-12 audit program Passes 1-3: complete and dispositioned under Results 313, 314, and 316
-> current gate: Packet 018A reading-path cleanup and later owner-approved post-M12 roadmap decision
```

Current authority:

- Result 244 closes Milestone 9;
- Result 250 closes Milestone 10;
- Decision 0019 records Milestone 10 system-of-record reconciliation;
- Decision 0020 records Milestone 11 operational foundation boundaries;
- Result 288 closes Milestone 11;
- Results 291 through 293 record Packet 016G planning approval, implementation return, and completion audit;
- Result 307 closes Milestone 12 and accepts its residual limitations;
- Results 310 through 316 record and disposition the post-M12 audit program Passes 0 through 3;
- Observatory work remains research-only under the OBR-01 through OBR-15 track;
- Qdrant, LangGraph, Hermes, UI, provider purchase, raw capture, and production MCP work remain deferred and separately gated.

## Active deferred boundaries and tracked repair backlog

Milestone 12 deferred boundaries remain live and visible:

- restore-forward remains deferred;
- operational role-ready restore remains deferred;
- ruff/tooling remains deferred;
- historical zero-file retained generations remain preserved as historical evidence.

Pass 3 repair items are tracked in Result 316 and must not be treated as closed:

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

## Non-blocking future work

- Decision 0015 reserved / unauthored / owner-deferred governance-compression gap disposition;
- Observatory rights, retention, measurement, reuse, and hammer research;
- post-Milestone-12 roadmap decision and next milestone definition;
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
