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

- Current phase: Kaizen v1 is owner-accepted for Milestone 14.
- Latest closed milestone: Milestone 14, accepted by owner in `03-research-results/368-milestone-14-kaizen-v1-owner-acceptance.md`.
- Accepted v1 checkpoint: docs commit `2a76363d3257cd3230746dc4c5901965291591d8`, with recorded caveats and post-v1 hardening deferred.
- Current post-acceptance docs checkpoint: verify current HEAD before work; post-v1 audit preparation and reading-path refresh records may exist after the accepted checkpoint.
- Current gate: 021B current-state vault amendment is executed and returned; 021C command-center vault amendment candidate is prepared for owner review. Kaizen v1 remains accepted unless a later accepted audit records an acceptance-invalidating defect.
- Completed M14 workload: Neon Ronin parent project idea-to-implementation-docs proof with SearchClarity child workspace/business-lane dependency, repo reconciliation, Stage B docs-only implementation return, fresh-context proof, and backup/restore proof.
- Core M14 boundary: SearchClarity captures the signal. Neon Ronin scores the signal. Kaizen governs the project-intelligence and implementation-doc chain.
- Kaizen docs repository role: this repo is Kaizen's construction, governance, proof, and audit workbench. It is not the long-term home of downstream project truth.
- Kaizen Obsidian Vault role: the intended canonical living project-intelligence layer for governed projects. Post-v1 work must define the project-to-vault promotion flow.
- Active accepted planning roadmap: `ROADMAP_V0.4.md`, owner-accepted in `03-research-results/322-roadmap-v0.4-owner-acceptance.md` and corrected through SP-1 / M13 / M14 / post-v1 updates.
- Platform repository: local-only at commit `b7593c5ee90fd32c1e2a86572cc570d307de2be6`; no remote exists.
- Docs repository: local work is ahead of `origin/main`; verify current HEAD and sync posture before push or handoff work.
- Canonical vault: local-only at commit `c898f261c0b341eb8419125247c8bd53ef567d6c`; no remote exists.
- Neon Ronin repository: Stage B M14 proof commits are local and ahead of origin; no push is authorized by this file.
- Live operational database: `kaizen_ops` is migrated through `0005_recovery_retention_integrity`.
- Go8 repository: operational local tool server.
- Kaizen MCP remains a temporary non-Git proving ground; it is not production infrastructure.
- Qdrant index: not implemented.
- Hermes Desktop / Hermes Agent: deferred; no canonical write authority or live integration is authorized.
- Observatory / IMI remains a shared capability / ownership-boundary candidate. No Observatory / IMI implementation is authorized.
- Accepted decisions remain governing inputs. Decision 0015 is a known reserved / unauthored / owner-deferred governance-compression gap; do not present it as accepted doctrine and do not fabricate it.
- Active workflow posture: 021B current-state vault amendment is returned; decide whether to approve 021C command-center exact prepare/plan generation, then continue overview inspection, sync posture decisions, and post-v1 hardening; preserve full gates for consequential mutation and implementation.
- Stewardship principle: structure and automation must earn their existence.

## Read-first sequence

This numbered list is a task-dependent reference map, not a requirement to load every file. Always read this entrypoint and active accepted `ROADMAP_V0.4.md` for roadmap work. Then load only the decisions, specs, audits, task packets, and implementation returns relevant to the requested work unless the user explicitly requests a full-repository audit.

1. `00-entrypoint/LLM_START_HERE.md` - this file; authoritative current-state pointer
2. `ROADMAP_V0.4.md` - accepted roadmap; current post-v1 lane should be read with later acceptance and reading-path refresh records
3. `03-research-results/368-milestone-14-kaizen-v1-owner-acceptance.md` - Kaizen v1 / Milestone 14 owner acceptance
4. `03-research-results/367-packet-020l-kaizen-v1-completion-audit-and-owner-acceptance-readiness.md` - v1 completion audit and caveats
5. `03-research-results/366-packet-020k-backup-restore-proof-result.md` - M14 backup/restore proof result
6. `03-research-results/364-packet-020j-stage-b-closure-audit-and-sequence-reconciliation.md` - Stage B closure and sequence reconciliation
7. `03-research-results/363-packet-020j-stage-b-implementation-return.md` - Stage B implementation return
8. `05-specs/milestone-14-first-real-internal-project-governed-run.md` - M14 definition, now completed/accepted by Result 368
9. `03-research-results/378-packet-021b-vault-amendment-implementation-return.md` - 021B executed current-state vault amendment return
10. `03-research-results/379-packet-021c-kaizen-platform-command-center-vault-amendment-candidate.md` - 021C command-center vault amendment candidate
11. `03-research-results/376-packet-021b-kaizen-platform-post-v1-current-state-vault-amendment-candidate.md` - 021B current-state vault amendment candidate
12. `03-research-results/377-packet-021b-vault-amendment-plan-generation-result.md` - generated immutable 021B amendment plan result
13. `03-research-results/375-post-v1-vault-planning-doc-reconciliation-audit.md` - vault planning-doc reconciliation
14. `03-research-results/374-post-v1-vault-promotion-flow-planning-packet.md` - post-v1 vault alignment planning context
15. `03-research-results/369-kaizen-v1-post-acceptance-claude-adversarial-audit-prompt.md` - post-v1 adversarial audit prompt
16. `03-research-results/370-kaizen-v1-post-acceptance-claude-mcp-tool-requirements.md` - Claude audit MCP tool requirements
17. `03-research-results/371-claude-mcp-build-and-audit-quickstart.md` - Claude MCP build/audit quickstart
18. `03-research-results/372-claude-post-v1-audit-start-prompt.md` - one-paste Claude audit start prompt
19. `03-research-results/342-milestone-13-owner-closure-acceptance.md` - Milestone 13 closure authority
20. `03-research-results/345-packet-020a-m14-workload-selection-and-boundary-registration.md` - M14 selected workload
21. `03-research-results/346-packet-020b-phase-0-boundary-and-collision-pre-registration.md` - M14 boundary/scoring pre-registration
22. `03-research-results/355-packet-020f-parent-child-and-observatory-boundary-candidate.md` - parent/child and Observatory boundary candidate
23. `03-research-results/310-post-m12-audit-source-inventory.md` - post-M12 audit Pass 0 inventory
24. `03-research-results/311-post-m12-roadmap-and-entrypoint-audit.md` - post-M12 audit Pass 1
25. `03-research-results/312-post-m12-milestone-traceability-audit.md` - post-M12 audit Pass 2
26. `03-research-results/313-post-m12-audit-pass-1-steward-disposition.md` - Pass 1 disposition
27. `03-research-results/314-post-m12-audit-pass-2-steward-disposition.md` - Pass 2 disposition
28. `03-research-results/315-post-m12-historical-disposition-audit.md` - post-M12 audit Pass 3
29. `03-research-results/316-post-m12-audit-pass-3-steward-disposition.md` - Pass 3 disposition and tracked P3 repair register
30. `04-design-decisions/README.md` - decisions index and Decision 0015 gap note
31. `04-design-decisions/0019-milestone-10-system-of-record-reconciliation.md`
32. `04-design-decisions/0020-milestone-11-operational-foundation-boundaries.md`
33. `05-specs/milestone-12-recovery-realism-and-operational-restore-proof.md`
34. `05-specs/milestone-11-operational-data-foundation-planning.md`
35. `05-specs/milestone-10-workload-and-system-of-record-reconciliation.md`
36. `05-specs/controlled-implementation-return-pilot.md` - Milestone 9 definition/spec despite non-numbered filename
37. `05-specs/milestone-8-reliability-and-known-defect-closure.md`
38. `05-specs/milestone-7-durable-recoverability.md`
39. `IMPLEMENTATION_ROADMAP.md` - historical completed first-slice roadmap for Milestones 1 through 5
40. `IMPLEMENTATION_ROADMAP_V0.2.md` - historical accepted Milestone 6 implementation-roadmap snapshot; preserve bytes unless separately approved
41. `ROADMAP.md` - historical planning provenance only; not a current-gate source
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

The active sequence is 021C post-v1 canonical-vault alignment:

```text
Milestones 1-14: closed
-> Kaizen v1 owner acceptance: recorded in Result 368
-> Claude audit required fixes: closed
-> vault planning reconciliation: recorded in Result 375
-> 021B current-state vault amendment: executed, returned, and committed locally
-> 021C command-center vault amendment candidate: prepared
-> current gate: owner decision to approve 021C exact prepare/plan generation, or request revision
```

Current authority:

- Result 368 accepts Kaizen v1 completion for Milestone 14;
- Result 373 closes the Claude audit disposition and required reading-path fixes;
- Result 375 reconciles older vault planning docs and confirms the first vault update should use bounded amendment semantics;
- Result 376 records the 021B current-state vault amendment candidate;
- Result 377 records the generated immutable 021B amendment plan;
- Result 378 records the executed 021B implementation return;
- Result 379 records the 021C command-center vault amendment candidate;
- ROADMAP_V0.4 remains the accepted roadmap, but its active-lane text must be read with later M13/M14/post-v1 records;
- Stage B execution is complete for the bounded M14 Neon Ronin reference-example docs change;
- no Git push, further downstream mutation, platform/vault/staging/database mutation, or Observatory / IMI implementation is authorized by this file without exact owner approval;
- Observatory / IMI remains a shared capability / ownership-boundary candidate and is not implemented.

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
