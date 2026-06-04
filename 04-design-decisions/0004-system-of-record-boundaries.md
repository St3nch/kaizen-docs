# Decision 0004 - System-of-Record Boundaries

Status: proposed
Date: 2026-06-04
Related research:
- `03-research-results/004-markdown-qdrant-postgres-architecture-claude-summary.md`
- `03-research-results/005-veda-db-governance-hammer-patterns-claude-summary.md`

## Context

Kaizen is evolving beyond an Obsidian-only design. Future semantic retrieval and Observatory capabilities introduce Qdrant and Postgres. Without explicit ownership boundaries, these layers could silently duplicate or contradict one another.

## Decision

Kaizen should adopt these system-of-record boundaries:

| Layer | Owns | Must not own |
|---|---|---|
| Markdown / Obsidian | project intelligence, interpretations, claims, decisions, specs, audits, handoffs | raw/bulk operational observations |
| Postgres Observatory | observations, jobs, runs, costs, logs, provenance, governed result summaries | canonical project narrative or doctrine |
| Qdrant | no canonical domain; rebuildable semantic index over approved Markdown | unique truth, private/customer data, direct agent-authored state |
| Hermes / agents | no canonical domain; draft, search, lint, package, propose | approval, promotion, direct database or index mutation |

Qdrant must be rebuildable from canonical Markdown.

Postgres is not rebuildable from Markdown. It is the operational source of truth for the structured records it owns and therefore requires backups, recovery, retention, migrations, and schema authority.

## Promotion flow

```text
structured observation in Postgres
-> governed result summary
-> approved typed API
-> staged Markdown insight/claim/opportunity
-> validation and review
-> accepted decision/spec/task packet
```

## Consequences

- No content has two canonical homes.
- Raw rows do not enter Markdown.
- Narrative doctrine does not enter Postgres as canonical content.
- Qdrant collections can be wiped and rebuilt.
- Every cross-layer link uses stable IDs and preserves provenance.

## Open questions

- Exact shape and lifecycle of Observatory result summaries.
- Raw payload storage and retention location.
- Whether promotion events are stored in Markdown, Postgres, or both.
- Whether current project state is purely Markdown snapshot or partly operational state.

## Related files

- `05-specs/postgres-observatory-authority.md`
- `05-specs/kaizen-field-registry.md`
- `05-specs/kaizen-hammer-test-strategy.md`
