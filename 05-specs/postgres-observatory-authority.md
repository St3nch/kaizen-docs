# Spec - Postgres Observatory Authority

Status: draft
Date: 2026-06-04
Related decisions:
- `04-design-decisions/0004-system-of-record-boundaries.md`
- `04-design-decisions/0005-api-only-structured-data-access.md`
- `04-design-decisions/0006-hammer-tests-are-a-hard-gate.md`

## Purpose

Define what the future Kaizen Postgres Observatory is allowed to own, what it must never own, and the governance rules that must exist before tables are designed.

This is an authority document, not a physical schema specification.

## Observatory owns

- source registry metadata
- crawl and ingestion jobs
- scheduled task runs
- SERP, ranking, citation, mention, AI Overview, and visibility observations
- provider/API request records
- cost and usage records
- agent action logs
- immutable or explicitly versioned derived result summaries
- provenance required to trace results to jobs, sources, actors, and capture times

## Observatory must not own

- canonical project narrative
- accepted claims or doctrine
- decision rationale
- spec bodies
- audit verdicts
- implementation handoff content
- raw customer/private data exposed to agents
- semantic vectors that belong in Qdrant

## Table classification rule

Every table must be classified as exactly one of:

1. project-scoped data
2. intentionally global reference data
3. system metadata

Any table that does not fit cleanly is deferred until governed.

## Required integrity rules

1. Every observation identifies project, source, job/run, capture time, and actor/system.
2. Every derived result identifies the observations or runs that produced it.
3. Published result summaries are immutable or explicitly superseded by new result versions.
4. Status transitions reject illegal transitions.
5. State changes and required event/audit records commit atomically.
6. Failed and partial states remain explicit.
7. Project-scoped data cannot be read across project boundaries through agent tools.
8. Paid actions record scope, vendor, approval, budget ceiling, actual cost, and outcome.

## JSON discipline

JSON may store:

- raw external payload snapshots when justified
- bounded provider metadata with genuinely variable shape

JSON must not store:

- repeatedly queried governed fields
- unresolved domain design
- hidden cross-boundary state
- canonical narrative

If a concept is repeatedly filtered, validated, classified, or joined, it needs governed columns or a governed child table.

## Access boundary

No agent, MCP tool, or general script may use direct Postgres access.

All access occurs through approved APIs/tools that enforce:

- project/domain scope
- authorization
- validation
- spend controls
- logging
- response-shape contracts
- private-data filtering

## Result boundary object

The Observatory should expose stable result summaries rather than raw rows.

A result should include at minimum:

- stable external result ID
- result type and schema version
- project scope
- source/run/observation provenance
- capture or analysis period
- confidence/quality indicators
- summary payload with governed fields
- published/superseded state

Exact storage design remains open.

## Migration and recovery posture

Before implementation, define:

- migration ownership
- backup and restore process
- raw-data retention
- result retention
- idempotency strategy
- disaster recovery expectations
- schema-freeze and expansion process

## Approval and schema change gate

Adding a table family, governed state, cross-system reference, or result schema version requires:

1. authority review
2. schema/spec update
3. migration plan
4. validation update
5. hammer coverage

## Open questions

- Dedicated promotions table versus Markdown promotion records.
- Append-only results table versus versioned result families.
- Exact ID strategy and Postgres version target.
- Raw object storage location.
- Retention policy for raw evidence supporting active notes.
- Whether source trust classification is human-only or agent-proposed/human-confirmed.

## Related files

- `05-specs/kaizen-field-registry.md`
- `05-specs/kaizen-note-type-registry.md`
- `05-specs/kaizen-hammer-test-strategy.md`
