# Decision 0009 - Operational Postgres and Observatory Boundary

Status: accepted
Date: 2026-06-05
Accepted: 2026-06-05
Related accepted decision:
- `04-design-decisions/0004-system-of-record-boundaries.md`
Related evidence and planning:
- `01-project-standard/kaizen-vision-and-architecture-alignment.md`
- `03-research-results/004-markdown-qdrant-postgres-architecture-claude-summary.md`
- `03-research-results/005-veda-db-governance-hammer-patterns-claude-summary.md`
- `03-research-results/007-kaizen-vision-architecture-research-gap-audit-claude-summary.md`
- `05-specs/operational-postgres-authority.md`
- `ROADMAP.md`

## Context

Accepted Decision 0004 correctly established that structured operational records belong in Postgres rather than canonical Markdown or Qdrant. However, it named the full structured operational layer `Postgres Observatory` and assigned observations, jobs, runs, costs, logs, provenance, and governed results to that single label.

The restored Kaizen vision is broader.

Kaizen is expected to support structured operational concerns that are not all Observatory responsibilities, including:

- source ingestion and source-version tracking
- scheduled and manual automation runs
- provider requests and results
- costs and usage
- governance and approval events
- audit and security records
- stable reference data
- observations and analytical result summaries

Treating all of these concerns as Observatory ownership would blur subsystem authority and encourage a single undifferentiated database model.

This decision resolves the conceptual boundary before table, schema, API, or deployment design begins.

## Decision

Kaizen will use the term **Operational Postgres database** for the broader structured operational data layer.

The **Observatory** is one bounded intelligence and analysis domain within that broader database. It is not the name of the entire database.

Decision 0004 remains accepted for its core system-of-record rule:

- Markdown / Obsidian owns canonical project intelligence and governing narrative.
- Postgres owns the structured operational records assigned to it.
- Qdrant is a rebuildable semantic index and owns no unique truth.
- agents own no canonical domain.

This decision supersedes only the terminology and ownership scope implied by the phrase `Postgres Observatory` in Decision 0004.

## Initial planning domains

The following names are adopted as **initial planning domains** for workload discovery and authority analysis:

```text
observatory
ingestion
automation
governance
audit
reference
```

These names do not yet authorize:

- physical Postgres schemas
- tables
- services
- repositories
- queues
- APIs
- deployment units
- retention classes
- separate databases

Research and workload discovery may later merge, split, rename, or defer domains before implementation.

## Domain intent

### Observatory

Intended to own approved structured observations and analytical result records used for project and portfolio intelligence, such as:

- SERP, ranking, visibility, mention, citation, and retrievability observations
- market and competitor observations
- normalized measurements
- governed analytical results
- strategy and research queue inputs derived from approved observations
- data-quality and confidence metadata for operational observations

The Observatory must not own canonical project claims, decisions, specs, audits, strategies, or task packets.

### Ingestion

Intended to own the operational process of acquiring and normalizing external information, such as:

- source registrations and versions
- ingestion requests and jobs
- provider requests and responses
- raw-payload references and retention state
- extraction and normalization status
- source acquisition provenance
- retry and failure state

Ingestion records do not become canonical project evidence until they are reviewed and represented through governed Kaizen artifacts or approved result records.

### Automation

Intended to own structured execution state for scheduled and manual operational work, such as:

- workflow and job definitions where later approved
- runs and run attempts
- schedules
- queues
- execution status
- cost and usage attribution
- retry, cancellation, and failure state

Automation state must not be represented by mutable Markdown frontmatter as though it were live operational truth.

### Governance

Intended to own structured operational governance events where Postgres ownership is later accepted, such as:

- approval and authorization events
- budget and paid-data authorization
- operational promotion or release events
- policy and permission version references
- exception and risk-acceptance records

Canonical human-readable decisions and policies remain in Markdown unless a later accepted decision assigns a different canonical home.

### Audit

Intended to own machine and operational audit records, such as:

- agent and tool invocation logs
- access and authorization outcomes
- validation and hammer-test runs
- security-relevant events
- change and failure evidence
- immutable or append-only event records where later specified

Human audit verdicts and audit narratives remain canonical Markdown unless a later decision explicitly changes that boundary.

### Reference

Intended to own intentionally shared structured reference data that is not canonical project narrative, such as:

- provider identifiers
- source-class identifiers
- controlled operational code lists
- model and tool version references
- approved project and external-system identifiers

Reference data must have explicit ownership, versioning, and change-control rules before use.

## Cross-domain rules

1. Every record family has exactly one authoritative operational owner.
2. Cross-domain references use stable identifiers and preserve provenance.
3. No domain may silently become a second canonical home for Markdown project intelligence.
4. Project scoping is required unless a record is intentionally global and governed as such.
5. Agents access operational records only through approved typed tools or APIs.
6. Agents do not receive arbitrary SQL or direct database credentials.
7. Cost, authorization, privacy, retention, and audit controls apply at the tool and service boundaries.
8. Raw or private payloads require explicit storage and retention decisions; they do not automatically belong in ordinary relational tables.
9. Schema and migration authority must be defined before implementation.
10. Backup, recovery, retention, and failure behavior are implementation gates, not afterthoughts.

## Information flow

The intended high-level flow is:

```text
external or operational source
-> ingestion records
-> automation run state
-> approved observation or result record
-> typed and scoped access
-> staged Kaizen evidence, claim, strategy, or decision proposal
-> validation, audit, and human review
-> accepted canonical project intelligence
```

Implementation and operational feedback may also flow back into Kaizen:

```text
project implementation or operation
-> structured run, test, market, customer, or visibility evidence
-> governed operational result
-> reviewed project-intelligence update
```

## Consequences

### Positive

- The Observatory can remain focused on intelligence and analysis rather than becoming a name for all structured state.
- Workload discovery can proceed without prematurely designing tables.
- Ingestion, automation, governance, audit, and reference concerns gain explicit planning ownership.
- Provider, cost, permission, privacy, and recovery research can be mapped to the correct domain.
- Future APIs and hammer tests can target narrower authority boundaries.

### Costs and risks

- Existing accepted and draft documents use `Postgres Observatory` and require controlled reconciliation.
- More domain names can create false certainty if treated as permanent schemas.
- Cross-domain boundaries may be wrong until real workloads and research are available.
- Splitting specifications too early could create unnecessary bureaucracy.

These risks are addressed by treating the domain list as planning boundaries and requiring evidence before physical design.

## Supersedence and amendment effect

This accepted decision supersedes only the following portion of Decision 0004:

> the use of `Postgres Observatory` as the name and implied owner of the entire structured operational layer.

Decision 0004's broader system-of-record rules remain accepted.

Affected current guidance and draft specifications must be updated through a reviewable reconciliation batch, including:

- `00-entrypoint/LLM_START_HERE.md`
- `01-project-standard/baseline-v0.2-reconciliation-map.md`
- `01-project-standard/standard-revision-plan.md`
- `04-design-decisions/0004-system-of-record-boundaries.md` through an explicit amendment or supersedence note
- `05-specs/operational-postgres-authority.md`
- `05-specs/kaizen-note-type-registry.md`

Historical research and prompts may retain original terminology when clearly identified as historical evidence.

## Research required before implementation

This decision establishes conceptual ownership only.

Implementation planning still requires evidence on:

- actual workloads and operational questions
- DataForSEO and other provider capabilities, terms, costs, and data rights
- LLM citation and retrievability measurement maturity
- source ingestion, payload storage, and licensing constraints
- Postgres schema, migration, retention, and recovery patterns
- typed API and MCP tool boundaries
- project isolation
- append-only and audit-event requirements
- backup and disaster recovery
- Qdrant and Postgres cross-system identifiers

## Acceptance criteria

This decision is ready for acceptance only when the human reviewer confirms that:

- the broader structured layer should be called the Operational Postgres database;
- the Observatory is one bounded domain rather than the whole database;
- the domain names are planning boundaries, not approved physical schemas;
- Decision 0004's canonical-system boundaries otherwise remain in force;
- no table, service, or deployment design is being approved by this decision;
- affected documents will be reconciled only after acceptance.

## Open questions

- Should costs and usage remain primarily under `automation`, or require a later cross-cutting financial-control domain?
- Should governance and audit remain separate planning domains after workload discovery?
- Does source metadata belong entirely under ingestion, or partly under reference?
- Which operational events must be append-only?
- Which raw payloads stay outside Postgres while retaining references and provenance?
- What result-summary contract separates Observatory records from canonical Markdown evidence?
- Should the final specification be one database-wide authority document plus domain-specific specifications, or one consolidated authority document?

## Related files

- `04-design-decisions/0004-system-of-record-boundaries.md`
- `04-design-decisions/0005-api-only-structured-data-access.md`
- `04-design-decisions/0006-hammer-tests-are-a-hard-gate.md`
- `01-project-standard/kaizen-vision-and-architecture-alignment.md`
- `03-research-results/007-kaizen-vision-architecture-research-gap-audit-claude-summary.md`
- `05-specs/operational-postgres-authority.md`
- `ROADMAP.md`
