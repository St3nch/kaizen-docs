# Spec - Operational Postgres Authority

Status: draft
Date: 2026-06-05
Related decisions:
- `04-design-decisions/0004-system-of-record-boundaries.md`
- `04-design-decisions/0005-api-only-structured-data-access.md`
- `04-design-decisions/0006-hammer-tests-are-a-hard-gate.md`
- `04-design-decisions/0009-operational-postgres-and-observatory-boundary.md`

## Purpose

Define what the future Kaizen Operational Postgres database is allowed to own, what it must never own, and the governance rules that must exist before tables or physical schemas are designed.

This is an authority and workload-boundary document, not a physical schema specification.

The Observatory is one bounded intelligence and analysis domain inside the Operational Postgres database. It is not the name of the entire database.

## Initial planning domains

```text
observatory
ingestion
automation
governance
audit
reference
```

These are planning boundaries for workload discovery. They do not yet authorize physical Postgres schemas, tables, services, queues, APIs, deployment units, retention classes, or separate databases.

Research and workload discovery may later merge, split, rename, or defer domains before implementation.

## Database-wide ownership

The Operational Postgres database may own structured operational records assigned to a governed domain, including:

- source registrations and source versions
- ingestion requests, jobs, attempts, and failures
- provider/API request and response records
- scheduled and manual automation runs
- queues, execution state, retries, and cancellations
- SERP, ranking, citation, mention, AI Overview, retrievability, market, and visibility observations
- normalized measurements and governed analytical result summaries
- cost and usage records
- approval, authorization, and operational governance events where later accepted
- agent, tool, validation, hammer-test, and security audit records
- intentionally global governed reference data
- provenance required to trace results to jobs, sources, actors, tools, and capture times

Every record family must have exactly one authoritative operational owner.

## Domain intent

### Observatory

Owns approved structured observations and analytical result records used for project and portfolio intelligence.

It must not own canonical project claims, decisions, specs, audits, strategies, task packets, or the operational machinery used to acquire and execute work.

### Ingestion

Owns source acquisition, source versions, provider requests, extraction, normalization, raw-payload references, retention state, retries, and failures.

Ingestion records do not become canonical project evidence until reviewed and represented through governed Kaizen artifacts or approved result records.

### Automation

Owns schedules, queues, runs, attempts, execution state, retries, cancellations, failures, and cost/usage attribution for operational work.

Live automation state must not be represented by mutable Markdown frontmatter as though it were operational truth.

### Governance

Owns structured operational authorization, budget, exception, risk-acceptance, release, and policy-version events where Postgres ownership is later accepted.

Canonical human-readable decisions and policies remain in Markdown.

### Audit

Owns machine and operational audit records, including agent/tool invocations, access outcomes, validation runs, hammer-test runs, security events, and failure evidence.

Human audit narratives and verdicts remain canonical Markdown unless a later accepted decision changes that boundary.

### Reference

Owns intentionally shared structured reference data such as provider identifiers, source classes, operational code lists, model/tool versions, and approved external-system identifiers.

Reference data requires explicit ownership, versioning, and change control.

## Must not own

The Operational Postgres database must not own:

- canonical project narrative
- accepted claims or doctrine
- decision rationale
- specification bodies
- human audit narratives or verdicts
- implementation handoff content
- semantic vectors that belong in Qdrant
- private or customer data exposed to agents without an explicit governed boundary
- a second canonical copy of Markdown project intelligence

## Record classification rule

Every record family must be classified as exactly one of:

1. project-scoped operational data
2. intentionally global governed reference data
3. system metadata

It must also identify its authoritative planning domain.

Any record family that does not fit cleanly is deferred until governed.

## Required integrity rules

1. Every project-scoped record identifies the project.
2. Every observation identifies source, job/run, capture time, and actor/system.
3. Every derived result identifies the observations or runs that produced it.
4. Published result summaries are immutable or explicitly superseded by new versions.
5. Status transitions reject illegal transitions.
6. State changes and required event/audit records commit atomically where one transaction boundary exists.
7. Failed, partial, cancelled, and stale states remain explicit.
8. Project-scoped data cannot be read across project boundaries through agent tools.
9. Paid actions record scope, vendor, approval, budget ceiling, actual cost, and outcome.
10. Cross-domain references use stable identifiers and preserve provenance.
11. No domain may silently become a second canonical home for Markdown project intelligence.

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

All access occurs through approved typed APIs or tools that enforce:

- project and domain scope
- authorization
- validation
- spend controls
- logging
- response-shape contracts
- private-data filtering
- rate limits and failure behavior

## Result boundary object

The Observatory should expose stable governed result summaries rather than raw rows.

A result should include at minimum:

- stable external result ID
- result type and schema version
- project scope
- source/run/observation provenance
- capture or analysis period
- confidence and quality indicators
- summary payload with governed fields
- published or superseded state

Exact storage design remains open.

## Migration and recovery posture

Before implementation, define:

- schema and migration ownership
- backup and restore process
- raw-data retention
- result retention
- idempotency strategy
- disaster-recovery expectations
- schema-freeze and expansion process
- cross-domain transaction boundaries
- append-only event requirements

## Approval and schema-change gate

Adding a record family, governed state, cross-system reference, or result schema version requires:

1. workload and authority review
2. specification update
3. migration plan
4. validation update
5. backup/recovery impact review
6. hammer coverage

## Research required before implementation

- actual workloads and operational questions
- DataForSEO and other provider capabilities, terms, costs, and data rights
- LLM citation and retrievability measurement maturity
- source ingestion, payload storage, and licensing constraints
- Postgres schema, migration, retention, and recovery patterns
- typed API and MCP tool boundaries
- project isolation
- append-only and audit-event requirements
- Qdrant and Postgres cross-system identifiers

## Open questions

- Should cost and usage remain primarily under automation, or become a later cross-cutting financial-control concern?
- Should governance and audit remain separate after workload discovery?
- Does source metadata belong entirely under ingestion, or partly under reference?
- Which operational events must be append-only?
- Which raw payloads stay outside Postgres while retaining references and provenance?
- What result-summary contract separates Observatory records from canonical Markdown evidence?
- Should later domain-specific authority specs be created only after workload friction earns them?
- Exact ID strategy and Postgres version target.

## Related files

- `04-design-decisions/0009-operational-postgres-and-observatory-boundary.md`
- `05-specs/kaizen-field-registry.md`
- `05-specs/kaizen-note-type-registry.md`
- `05-specs/kaizen-hammer-test-strategy.md`
- `ROADMAP.md`