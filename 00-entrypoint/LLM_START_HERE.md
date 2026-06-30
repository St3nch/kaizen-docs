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
- Current gate: Packet 024N retrieval vector baseline and quality contract completed and is recorded in `03-research-results/423-packet-024n-retrieval-vector-baseline-and-quality-contract.md`. Platform remains at `cfbb9453aedfff73b95f2a30596c97ef506fed32`. Next recommended step is 024O retrieval vector baseline implementation.
- Completed M14 workload: Neon Ronin parent project idea-to-implementation-docs proof with SearchClarity child workspace/business-lane dependency, repo reconciliation, Stage B docs-only implementation return, fresh-context proof, and backup/restore proof.
- Post-v1 backup records: `03-research-results/382-packet-022abc-post-v1-backup-generation-transfer-and-cleanup-record.md` records the earlier encrypted backup generation, USB and Google Drive verification, and plaintext cleanup; `03-research-results/383-packet-022f-through-022j-backup-v2-key-custody-and-restore-proof-record.md` records the completed age identity v2 recovery, fresh backup, USB / Google verification, restore proof, and source plaintext cleanup.
- Core M14 boundary: SearchClarity captures the signal. Neon Ronin scores the signal. Kaizen governs the project-intelligence and implementation-doc chain.
- Kaizen docs repository role: this repo is Kaizen's construction, governance, proof, and audit workbench. It is not the long-term home of downstream project truth.
- Kaizen Obsidian Vault role: the intended canonical living project-intelligence layer for governed projects, not an evidence dump. Post-v1 vault alignment is complete through 021F; vault admission hardening now applies during development and must carry into launched Kaizen projects. Project-to-vault promotion flow is a governed distillation path: source docs/evidence -> durable project intelligence -> vault admission test -> bounded vault amendment. No vault mutation is authorized by docs-only read-path work.
- Active accepted planning roadmap: `ROADMAP_V0.4.md`, owner-accepted in `03-research-results/322-roadmap-v0.4-owner-acceptance.md` and corrected through SP-1 / M13 / M14 / post-v1 updates.
- Platform repository: local-only at commit `84071eef1e5859f651d3213c65f3bfc99a3c94f8`; no remote exists.
- Docs repository: synced to `origin/main` at commit `68c5bbff073e15b03e453bb1bd69c6a91c866b4e`; verify current HEAD and sync posture before push or handoff work.
- Canonical vault: local-only at commit `6fbc8cefe4ed4cd57403648ee3977ee2aeaaba5b`; no remote exists.
- Neon Ronin / SearchClarity posture: current material is legacy planning and verification material, not an official governed Go8 root or active repo-sync target. Future work should smartly distill useful material into the vault under vault admission rules; real Neon Ronin and SearchClarity repos may be created later under separately approved project-bootstrap packets.
- Live operational database: `kaizen_ops` is migrated through `0005_recovery_retention_integrity`.
- Go8 repository: operational local tool server.
- Kaizen MCP remains a temporary non-Git proving ground; it is not production infrastructure.
- Qdrant index: not implemented.
- Hermes Desktop / Hermes Agent: deferred; no canonical write authority or live integration is authorized.
- Observatory / IMI remains a shared capability / ownership-boundary candidate. No Observatory / IMI implementation is authorized.
- Accepted decisions remain governing inputs. Decision 0015 is a known reserved / unauthored / owner-deferred governance-compression gap; do not present it as accepted doctrine and do not fabricate it.
- Active workflow posture: complete lean docs read-path cleanup, then decide sync posture and post-v1 hardening priorities; preserve full gates for consequential mutation and implementation.
- Stewardship principle: structure and automation must earn their existence.

## Read-first sequence

This numbered list is a task-dependent reference map, not a requirement to load every file. Always read this entrypoint and active accepted `ROADMAP_V0.4.md` for roadmap work. Then load only the decisions, specs, audits, task packets, and implementation returns relevant to the requested work unless the user explicitly requests a full-repository audit.

1. `00-entrypoint/LLM_START_HERE.md` - this file; authoritative current-state pointer
2. `ROADMAP_V0.4.md` - accepted roadmap and current post-v1 lane
3. `03-research-results/368-milestone-14-kaizen-v1-owner-acceptance.md` - Kaizen v1 / Milestone 14 owner acceptance
4. `03-research-results/367-packet-020l-kaizen-v1-completion-audit-and-owner-acceptance-readiness.md` - v1 completion audit and caveats
5. `03-research-results/382-packet-022abc-post-v1-backup-generation-transfer-and-cleanup-record.md` - post-v1 backup record
6. `03-research-results/383-packet-022f-through-022j-backup-v2-key-custody-and-restore-proof-record.md` - backup v2 key custody and restore proof
7. `03-research-results/373-post-v1-claude-audit-disposition-and-reading-path-refresh-closure.md` - post-v1 audit disposition
8. `03-research-results/375-post-v1-vault-planning-doc-reconciliation-audit.md` - vault planning reconciliation
9. `03-research-results/README.md` - cluster index for historical research/result lookup; use instead of loading old result chains by default
10. `06-handoff-patterns/README.md` - historical handoff packet library index; not a current work queue
11. `04-design-decisions/README.md` - decisions index and current decision lookup
12. `04-design-decisions/0019-milestone-10-system-of-record-reconciliation.md` - system-of-record reconciliation authority
13. `04-design-decisions/0020-milestone-11-operational-foundation-boundaries.md` - operational foundation boundary authority
14. `05-specs/milestone-14-first-real-internal-project-governed-run.md` - M14 definition, now completed/accepted by Result 368
15. `05-specs/milestone-12-recovery-realism-and-operational-restore-proof.md` - recovery realism spec when recovery work is relevant
16. `05-specs/milestone-11-operational-data-foundation-planning.md` - operational data foundation spec when database work is relevant
17. `05-specs/milestone-10-workload-and-system-of-record-reconciliation.md` - workload/system-of-record spec when boundary work is relevant
18. Historical roadmaps only when investigating lineage: `IMPLEMENTATION_ROADMAP.md`, `IMPLEMENTATION_ROADMAP_V0.2.md`, `ROADMAP.md`

Historical prompt clusters, implementation-return chains, audit setup records, and old handoff packets are preserved evidence. Do not load them by default; follow `03-research-results/README.md` and `06-handoff-patterns/README.md` by task-specific cluster.
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

The active sequence is post-v1 hardening priority selection:

```text
Milestones 1-14: closed
-> Kaizen v1 owner acceptance: recorded in Result 368
-> Claude audit required fixes: closed in Result 373
-> vault planning reconciliation: recorded in Result 375
-> 021B / 021C / 021D / 021E / 021F vault alignment and stale-sequence cleanup: complete
-> lean docs read-path refresh: complete
-> post-v1 backup generation, transfer verification, and plaintext cleanup: recorded in Result 382
-> backup v2 key-custody recovery, fresh backup, transfer verification, restore proof, and plaintext cleanup: recorded in Result 383
-> Packet 023E M15 parser/validator prototype implementation return: recorded in Result 390
-> Packet 023F current-state and command-center contract draft: recorded in Result 391
-> Packet 023F implementation return: recorded in Result 392
-> Packet 023G review-item notes and review queue views draft: recorded in Result 393
-> Packet 023G implementation return: recorded in Result 394
-> Packet 023H resume view and context-pack assembly draft: recorded in Result 395
-> Packet 023H implementation return: recorded in Result 396
-> Packet 023I M15 integrated proof and closure audit draft: recorded in Result 397
-> Packet 023I M15 integrated proof and closure audit result: recorded in Result 398
-> Milestone 15 owner closure acceptance: recorded in Result 399
-> Milestone 16 Qdrant research context and hammer-test direction: recorded in Result 400
-> Packet 024A M16 definition and Qdrant evidence reconciliation draft: recorded in Result 401
-> Packet 024A M16 definition and Qdrant evidence reconciliation result: recorded in Result 402
-> Packet 024B corpus boundary and governance contract draft: recorded in Result 403
-> Packet 024B corpus boundary and governance contract result: recorded in Result 404
-> Packet 024C chunk, payload, ID, and rebuild contract draft: recorded in Result 405
-> Packet 024C chunk, payload, ID, and rebuild contract result: recorded in Result 406
-> Packet 024D retrieval gateway and typed tool contract draft: recorded in Result 407
-> Packet 024D retrieval gateway and typed tool contract result: recorded in Result 408
-> Packet 024E disposable synthetic local prototype draft: recorded in Result 409
-> Packet 024E disposable synthetic local prototype implementation return: recorded in Result 410
-> Packet 024F hammer tests and prototype audit draft: recorded in Result 411
-> Packet 024F hammer tests and prototype audit result: recorded in Result 412
-> Packet 024G Qdrant-backed disposable synthetic prototype definition draft: recorded in Result 413
-> Packet 024G Qdrant-backed disposable synthetic prototype definition result: recorded in Result 414
-> Packet 024H Qdrant-backed disposable synthetic prototype implementation packet draft: recorded in Result 415
-> Packet 024H implementation return / prerequisite stop: recorded in Result 416
-> current gate: draft 024H-R1 client dependency and local runtime prerequisite packet
```

Current authority:

Low-risk docs-only hardening may use one bundled approval for exact-path edits, checks, staging, commit, and docs upstream sync when preflight passes. Stop on scope expansion, new files, deletion, moves, failed checks, unexpected dirty state, or any non-docs mutation.

- Result 368 accepts Kaizen v1 completion for Milestone 14;
- Result 373 closes the Claude audit disposition and required reading-path fixes;
- Result 375 reconciles older vault planning docs and confirms vault updates should use bounded amendment semantics;
- Results 376 through 381 record the completed 021B / 021C vault amendment sequence through implementation returns;
- later 021D / 021E / 021F vault alignment and stale-sequence cleanup are recorded in the docs/vault history and reflected in the current vault HEAD;
- Result 382 records the earlier post-v1 encrypted backup generation, USB verification, Google Drive re-download verification, and owner-reported plaintext cleanup;
- Result 383 records completed backup v2 key-custody recovery, fresh encrypted backup generation, USB verification, Google Drive re-download verification, disposable restore proof, and source plaintext cleanup;
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
