# Decision 0005 - API-Only Structured Data Access

Status: accepted
Date: 2026-06-04
Accepted: 2026-06-04
Related research:
- `03-research-results/002-hermes-desktop-verification-gpt-summary.md`
- `03-research-results/004-markdown-qdrant-postgres-architecture-claude-summary.md`
- `03-research-results/005-veda-db-governance-hammer-patterns-claude-summary.md`

## Context

Hermes and future agents may need Observatory summaries and semantic search. Direct database clients, arbitrary SQL, and direct Qdrant mutation would bypass scope controls, validation, authorization, spend controls, and audit logging.

## Decision

Agents should access Postgres and Qdrant only through approved, typed, scoped, logged tools or APIs.

Hermes must never receive:

- a Postgres connection string
- arbitrary SQL execution
- a raw database client
- direct Qdrant collection write/delete tools
- unbounded paid-data execution tools

## Initial tool classes

### Read-only

- semantic vault search
- related-note lookup
- duplicate-candidate check
- approved Observatory result lookup
- note read
- schema/content lint

### Staging-only mutation

- create staged note
- propose edit as diff
- draft Observatory insight

### Request-only

- propose source ingestion
- propose bounded data collection
- propose supersedence
- escalate boundary or conflict

### Human-only / nonexistent for agents

- promote note
- approve decision/spec
- mark implementation-ready
- execute paid pull without authorization
- mutate canonical files
- run arbitrary SQL
- mutate Qdrant collections

## Required controls

- project/domain scope injected by the tool layer
- hard budget ceilings
- rate limits
- sanitized append-only audit logs
- exact action classification
- explicit failure and partial-result reporting
- no action allowed by analogy when it is not defined

## Open questions

- Exact action-class vocabulary.
- Whether ingestion execution is manual, scheduled, or a separate approved service command.
- How approval staleness is represented.
- Whether tools belong in one Kaizen API or several bounded services.

## Related files

- `07-hermes/hermes-permission-matrix.md`
- `07-hermes/hermes-write-access-preconditions.md`
- `05-specs/postgres-observatory-authority.md`
- `05-specs/kaizen-hammer-test-strategy.md`
