# Historical Audit Disposition Sweep Plan

Status: proposed planning sweep
Date: 2026-06-20

## Purpose

Create a governed plan to ensure prior independent audits, Claude audits, steward audits, red-team findings, nice-to-have refinements, lower-priority concerns, and future-hardening notes are not lost.

This plan exists because audit output is project evidence and work-queue signal. Audit categories such as `nice-to-have`, `lower priority`, `future hardening`, `defer`, `watchlist`, and `partly proved` must be explicitly dispositioned rather than treated as disposable prose.

## Trigger

During Packet 017B review, Claude returned must-fix items, nice-to-have refinements, scope-risk observations, traceability review, and approval-readiness guidance. Result 300 recorded and dispositioned all of that audit signal.

The owner then correctly raised that the same discipline should apply to past audits.

## Current evidence that disposition sometimes exists

At least one major historical Claude red-team audit already has a real finding-by-finding reconciliation:

```text
03-research-results/024-implementation-checkpoint-red-team-audit-claude-summary.md
03-research-results/025-implementation-checkpoint-audit-steward-reconciliation.md
```

Result 025 includes a table of audit findings F-01 through F-29 with dispositions, gates, steward decisions, and closure evidence.

Other known reconciliation-style records include, at minimum:

```text
03-research-results/097-post-milestone-6-forward-roadmap-audit-reconciliation.md
03-research-results/100-controlled-pilot-review-steward-reconciliation.md
03-research-results/163-observatory-existing-evidence-reconciliation.md
03-research-results/263-claude-postgresql-research-steward-reconciliation.md
03-research-results/300-claude-packet-017b-audit-disposition.md
```

This sweep must verify which audit records are already dispositioned and which still contain untracked audit signal.

## Required sweep method

The sweep must be docs-only and read-only until a later owner-approved correction packet.

For each audit-like result, inspect whether it contains any of these signal categories:

```text
must-fix
required edit
nice-to-have
lower-priority concern
future hardening
deferred concern
watchlist
partly proved
unproved
scope risk
approval-readiness condition
recommended sequence
smallest recommended addition
open question
```

Then classify each signal as:

```text
closed
closed by later milestone/result
accepted and active
owner-deferred
deferred to future named gate
rejected with rationale
superseded by later decision/result
still-open and needs backlog record
unclear; needs human review
```

## Minimum audit families to sweep

The first sweep should cover these families before expanding further:

### Early architecture and tool-surface audits

```text
007 kaizen vision architecture research gap audit
008 agent access and write safety
009 Obsidian MCP tool surface
010 Postgres MCP typed tool audit
011 Qdrant MCP typed tool audit
013 human-interface architecture
014 human-interface technical verification
015 Internet Marketing Intelligence provider/source-rights audit
016 planning acceleration audit
017 Decision 0008 dry-run simulation
```

### Early implementation and first-slice safety audits

```text
024 implementation checkpoint red-team audit
025 implementation checkpoint audit steward reconciliation
026 through 067 first-slice and v0.2 acceptance audits, as needed by signal references
```

### Roadmap / pilot / milestone-sequence audits

```text
097 post-Milestone-6 forward-roadmap audit reconciliation
098 revised roadmap and pilot security audit
100 controlled pilot review reconciliation
101 revised controlled pilot and roadmap audit
133 Milestone 8 security/steward audit
135 Packet 012A security/steward audit
```

### Observatory and database-foundation audits

```text
163 Observatory existing evidence reconciliation
164 Observatory current market reconnaissance and gap register
165 Observatory roadmap amendment audit
252 Milestone 11 Phase 11A readiness audit
253 post-Milestone-11 Claude audit scope expansion
262 Claude PostgreSQL foundation research intake
263 Claude PostgreSQL research steward reconciliation
```

### Milestone 11 / Packet 016G / Milestone 12 audits

```text
288 Milestone 11 closure records and related completion/security audit chain
289 independent audit status after pass 3
290 independent audit strategic commentary
291 Packet 016G planning approval and task packet
292 Packet 016G implementation return
293 Packet 016G completion audit
295 post-Milestone-11 roadmap revision proposal
297 Packet 017A planning
299 Packet 017B task packet
300 Claude Packet 017B audit disposition
```

## Required output of the sweep

The sweep should produce a compact register, not a giant rewrite.

Recommended output:

```text
03-research-results/302-historical-audit-disposition-register.md
```

The register should include one row per load-bearing audit signal:

```text
source result
source section or line reference if available
signal type
summary
current disposition
closure or deferral evidence
recommended next action
blocks Milestone 12? yes/no
requires owner decision? yes/no
```

## Rules for handling old audit items

1. Do not reopen old milestones automatically.
2. Do not rewrite accepted historical records just to normalize language.
3. Do not promote audit recommendations into doctrine without owner acceptance.
4. Do not treat `nice-to-have` as ignorable.
5. Do not treat `deferred` as closed unless a later result explicitly closes or supersedes it.
6. Do not let low-priority hardening items silently vanish.
7. Keep Milestone 12 focused unless an old audit item directly affects recovery realism or operational restore proof.
8. Use later accepted results as closure evidence where possible.
9. Preserve rejected findings with rationale; rejected is a disposition, not deletion.
10. Preserve owner-deferred items separately from steward-deferred items.

## Relationship to Packet 017B

This sweep should not block Packet 017B unless it finds a prior audit item that directly affects:

```text
recovery generation proof
restore proof
database-and-file consistency proof
typed-service smoke proof
restore-forward boundary
operational role-ready restore boundary
live DB read-only boundary
no-secret handling
retained evidence integrity
```

If such an item is found, Packet 017B should be revised before implementation approval.

## Proposed immediate next step

Perform the first historical audit-disposition sweep and produce Result 302.

Result 302 should start with a bounded pass over the major audit families listed above. It should avoid trying to solve every old item immediately; the first job is to make the audit signal visible and classified.

## Non-authorization

This sweep plan does not authorize implementation, platform mutation, database mutation, vault mutation, milestone reopening, retention deletion, physical deletion, provider work, Qdrant, Hermes/UI, production MCP packaging, or any new operational family.
