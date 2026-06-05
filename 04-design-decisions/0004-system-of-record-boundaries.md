# Decision 0004 - System-of-Record Boundaries

Status: accepted
Date: 2026-06-04
Accepted: 2026-06-04
Amended by: `04-design-decisions/0009-operational-postgres-and-observatory-boundary.md`
Related research:
- `03-research-results/004-markdown-qdrant-postgres-architecture-claude-summary.md`
- `03-research-results/005-veda-db-governance-hammer-patterns-claude-summary.md`

## Context

Kaizen is evolving beyond an Obsidian-only design. Future semantic retrieval and Observatory capabilities introduce Qdrant and Postgres. Without explicit ownership boundaries, these layers could silently duplicate or contradict one another.

## Decision

Kaizen adopts these system-of-record boundaries:

| Layer | Owns | Must not own |
|---|---|---|
| Markdown / Obsidian | project intelligence, interpretations, claims, decisions, specs, audits, handoffs | raw/bulk operational observations |
| Operational Postgres database | structured operational records assigned to governed domains, including observations, jobs, runs, costs, logs, provenance, and governed result summaries | canonical project narrative or doctrine |
| Qdrant | no canonical domain; rebuildable semantic index over approved Markdown | unique truth, private/customer data, direct agent-authored state |
| Hermes / agents | no canonical domain; draft, search, lint, package, propose | approval, promotion, direct database or index mutation |

Qdrant must be rebuildable from canonical Markdown.

Postgres is not rebuildable from Markdown. It is the operational source of truth for the structured records it owns and therefore requires backups, recovery, retention, migrations, and schema authority.

The Observatory is one bounded intelligence and analysis domain within the Operational Postgres database. It is not the name or owner of the entire structured operational layer. Initial planning domains are defined by Decision 0009.

## Promotion flow

```text
structured observation in Postgres
-> governed result summary
-> approved typed API
-> staged Markdown insight/claim/opportunity
-> validation and review
-> accepted decision/spec/task packet
```

## Resolved boundary clarifications

- Promotion events begin as append-only JSONL in the canonical vault repository and may later move to Postgres operational governance.
- `current-state` notes are human-readable planning snapshots, not live operational state.
- Live jobs, runs, queues, measurements, costs, and automation state belong in Postgres when implemented.

## Consequences

- No content has two canonical homes.
- Raw rows do not enter Markdown.
- Narrative doctrine does not enter Postgres as canonical content.
- Qdrant collections can be wiped and rebuilt.
- Every cross-layer link uses stable IDs and preserves provenance.

## Open questions

- Exact shape and lifecycle of Observatory result summaries within the broader Operational Postgres database.
- Raw payload storage and retention location.
- Exact Postgres retention, backup, and recovery posture.

## Related files

- `04-design-decisions/0007-foundation-resolution-for-v0.2.md`
- `04-design-decisions/0009-operational-postgres-and-observatory-boundary.md`
- `05-specs/operational-postgres-authority.md`
- `05-specs/kaizen-field-registry.md`
- `05-specs/staging-and-promotion-workflow.md`
- `05-specs/kaizen-hammer-test-strategy.md`
