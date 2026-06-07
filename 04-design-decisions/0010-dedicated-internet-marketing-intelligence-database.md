# Decision 0010 - Dedicated Internet Marketing Intelligence Database Boundary

Status: accepted
Date: 2026-06-05
Updated: 2026-06-06
Accepted: 2026-06-06
Owner: Kaizen project steward
Related accepted decisions:
- `04-design-decisions/0004-system-of-record-boundaries.md`
- `04-design-decisions/0005-api-only-structured-data-access.md`
- `04-design-decisions/0009-operational-postgres-and-observatory-boundary.md`
Related vision and evidence:
- `01-project-standard/internet-marketing-intelligence-vision.md`
- `01-project-standard/kaizen-vision-and-architecture-alignment.md`
- `03-research-results/010-postgres-mcp-typed-tool-audit-claude-summary.md`
- `03-research-results/012-step-5a-composed-tool-capability-map.md`
- `03-research-results/015-internet-marketing-intelligence-providers-source-rights-claude-summary.md`
- `05-specs/operational-postgres-authority.md`
- `05-specs/deferred-dataforseo-llm-ranking-capture-packet.md`
- `ROADMAP.md`

## Context

Decision 0009 established the broader Operational Postgres database and treated the Observatory as one bounded intelligence and analysis domain within it.

The Internet Marketing Intelligence vision introduces a distinct long-term workload:

- many years of SERP and ranking observations;
- LLM mention, citation, and retrievability observations;
- marketplace ranking and visibility observations;
- competitor, entity, product, seller, listing, and public-market observations;
- project relationships to shared observations;
- derived trends, signals, anomalies, and opportunity candidates;
- strategy hypotheses, experiments, and measured outcomes;
- future ranking and visibility data families that are not yet known.

This workload is portfolio-wide, longitudinal, high-volume, analytically oriented, and intended to compound across projects.

It differs materially from Kaizen's core operational workload, which includes governance, audit, automation, ingestion control, agent runs, approvals, permissions, and workflow state.

The distinction is large enough to justify a dedicated database boundary, while the exact physical design remains premature.

## Decision

Kaizen should plan for a dedicated **Internet Marketing Intelligence database** separate from the Kaizen Core Operational database.

Conceptually:

```text
Kaizen Core Operational Database
├── ingestion control
├── automation and workflow state
├── governance and authorization
├── audit and security events
├── agent and tool runs
├── provider-job orchestration
└── shared operational reference data

Internet Marketing Intelligence Database
├── longitudinal search and ranking observations
├── LLM visibility, mention, citation, and retrievability observations
├── marketplace and public-market observations
├── shared entities and market identities
├── project relevance and tracking relationships
├── derived trends, signals, and anomalies
├── opportunity and strategy candidates
└── experiment and outcome evidence
```

The name is conceptual. Final naming may change after workload and implementation review.

## Initial deployment direction

The initial implementation may place both databases on the same PostgreSQL server or managed PostgreSQL service:

```text
one PostgreSQL deployment
├── kaizen_core
└── kaizen_intelligence
```

This preserves a clear authority and workload boundary without requiring separate infrastructure on day one.

The intelligence database may later move independently to:

- a separate PostgreSQL server;
- a managed service;
- an analytical replica;
- a warehouse or columnar analytical system;
- time-series or compression extensions;
- another evidence-backed storage architecture.

No such move is approved by this decision.

## Why the boundary is justified

### Longitudinal value

The intelligence data is intended to grow over many years. Historical depth is part of its value rather than incidental retention.

### Volume and workload shape

Search and marketplace observations may scale across:

```text
queries
x locations
x languages
x devices
x engines or marketplaces
x providers
x capture times
x result positions
x tracked entities
```

The workload may eventually exceed ordinary Kaizen governance and workflow records by a large margin.

### Portfolio-wide reuse

Public and lawfully reusable observations should not be trapped inside the project that first requested them.

One observation may support many projects, analyses, experiments, and opportunities.

### Independent operational concerns

The intelligence workload may require different:

- retention policies;
- partitioning;
- compression;
- indexing;
- aggregation;
- analytical replicas;
- backup and restore strategy;
- cost controls;
- data-quality checks;
- provider-response reconciliation;
- access and privacy controls.

### Security and typed access

Separating the database boundary supports distinct credentials, roles, APIs, and operational identities.

Interactive reasoning agents should receive typed, scoped intelligence operations rather than broad access to either database.

## Provider-neutral design

The database boundary must remain provider-neutral.

DataForSEO is the leading first provider candidate, but no provider response shape becomes Kaizen's universal model. Provider-reported metrics and classifications remain provider-attributed.

## Distinct AI and LLM evidence families

The design must not collapse AI Overviews, LLM responses, LLM scrapes, LLM mention clusters, LLM citations, and AI keyword-volume estimates into one generic AI-search record.

## Multi-provider comparison

Kaizen may later use secondary providers for selective calibration or gap-filling. Provider disagreement must be preserved rather than averaged into false certainty. No recurring secondary-provider subscription program or annual calibration budget is approved by this decision.

## Scope of ranking and visibility data

The intelligence database is not limited to conventional web-search rankings.

Likely data families include:

- organic SERP rankings;
- paid-search observations where later justified;
- local and map visibility;
- image, video, news, shopping, and other search surfaces;
- AI Overviews and generative search results;
- LLM mentions, citations, recommendations, and retrievability;
- marketplace rankings such as Fiverr, Etsy, Amazon, and future platforms;
- app-store, plugin-directory, software-directory, social, or discovery-platform rankings where future projects justify them;
- brand, product, seller, listing, page, domain, and entity visibility;
- new ranking or recommendation surfaces that emerge as platforms change.

This list is illustrative and intentionally incomplete.

The database boundary must remain provider-neutral, platform-neutral, and extensible enough to accommodate additional ranking and visibility evidence as Kaizen and its projects evolve.

## Observation ownership and project relationships

The system should distinguish:

```text
shared observation
request that caused collection
provider job and cost
project relevance
analysis or signal
opportunity or strategy candidate
experiment and outcome
```

A shared observation may relate to many projects without being duplicated as project-owned truth.

Project-private, customer-restricted, licensed, and confidential evidence remains separately governed and may require tighter storage or access boundaries.

## Cross-database identity

Separate PostgreSQL databases do not provide ordinary cross-database foreign keys.

Cross-database relationships should therefore use durable stable identifiers and service-level validation, such as conceptual:

```text
project_id
request_id
job_id
provider_run_id
experiment_id
observation_id
```

These names are examples, not approved fields.

The service layer must validate cross-database references, preserve provenance, and fail closed on ambiguous identity.

## Raw provider payloads

Large raw provider responses should not automatically be stored forever in normal relational tables.

A likely later pattern is:

```text
Internet Marketing Intelligence Database
├── normalized observations
├── hashes and provenance
├── request and provider metadata
└── governed pointer to retained raw artifact

object or file storage
└── compressed raw response when rights, cost, and retention policy permit
```

Exact retention and storage rules depend on provider rights, Research Batch B reconciliation, and later recovery planning.

The technical capability to retain and analyze data is not proof of contractual permission for every use. No indefinite raw-retention policy is approved by this decision.

Until written provider clarification or legal review exists, rights claims should be classified as expressly permitted, expressly restricted, not expressly prohibited, not expressly granted, provider clarification required, or legal review required before external or customer-facing use.

A provisional planning posture is:

```text
retain raw where allowed and useful
-> assign rights and privacy class
-> preserve hash and provenance
-> review retention window
-> purge, reduce, or continue based on rights, value, and cost
```

## Relationship to the Observatory

The Observatory remains the analytical and intelligence capability that interprets approved observations and produces governed results.

This decision changes the likely physical database boundary, not the Observatory's conceptual purpose.

The final design may place Observatory-owned workloads primarily in the Internet Marketing Intelligence database while keeping orchestration, authorization, audit, and workflow concerns in the Kaizen Core Operational database.

Exact ownership by record family remains deferred until workload analysis.

## Controlled paid-data capture

Paid data collection is an authority-bearing operational action.

Future captures require an exact research question, approved endpoint family, conceptual target, maximum rows or calls, an authorized provider-consumption ceiling, stop conditions, duplicate-call prevention, unique filenames, paired inventory notes, cost verification, and steward approval before execution.

Account funding and authorized API consumption are separate concepts. Unused provider balance is not authorization to spend.

The deferred planning artifact is:

- `05-specs/deferred-dataforseo-llm-ranking-capture-packet.md`
## Explicit schema-design gate

No schema design should begin from this decision alone.

Before tables, schemas, partitions, indexes, extensions, retention jobs, or APIs are designed, the owner and project steward must hold a focused database-design session.

That session should review at minimum:

- expected initial and future ranking-data families;
- provider response shapes;
- universal versus provider-specific observation fields;
- shared observation identity and deduplication;
- project relevance and privacy boundaries;
- historical versus current-state access patterns;
- expected query and analytical workloads;
- ingestion rate and storage growth;
- retention and raw-payload rights;
- correction, deletion, and provider reprocessing;
- time-based partitioning and compression needs;
- backup, restore, and disaster recovery;
- cross-database identifiers and consistency;
- cost attribution;
- prototype and hammer-test requirements.

The design session must explicitly allow for ranking and visibility data that has not yet been identified.

## What this decision does not approve

This decision does not approve:

- exact database names;
- exact PostgreSQL schemas;
- tables or columns;
- partition keys;
- indexes or materialized views;
- TimescaleDB or another extension;
- a data warehouse;
- object-storage technology;
- exact providers;
- exact ranking-data types;
- deduplication keys;
- retention periods;
- migration tooling;
- hosting provider;
- connection pooler;
- cross-database consistency mechanism;
- direct agent access;
- automatic opportunity or strategy decisions.

## Consequences

### Positive

- The long-lived intelligence asset receives a first-class boundary.
- Core governance and workflow records are not forced to share every scaling and retention choice with high-volume ranking data.
- Portfolio-wide observations can compound independently of project silos.
- Future analytical infrastructure can evolve without moving the entire Kaizen operational system.
- Separate credentials and typed APIs can enforce clearer authority boundaries.
- The design remains extensible to ranking and recommendation surfaces not yet known.

### Costs and risks

- Two databases introduce cross-database consistency and identity concerns.
- Operational tooling, backups, migrations, and monitoring become more complex.
- Premature separation could create unnecessary overhead if early workloads are small.
- The boundary could be misunderstood as a license to design schemas before provider and workload research.
- Some operational records may have ambiguous ownership until real workflows are modeled.

These risks are addressed by allowing one PostgreSQL deployment initially and requiring a focused design session before schema work.

## Relationship to Decision 0009

Decision 0009 remains accepted for its conceptual distinction between the broader operational layer and the Observatory domain.

Decision 0010 amends the likely physical deployment direction:

- Kaizen Core Operational records and Internet Marketing Intelligence records should be planned as separate database boundaries;
- the Observatory remains an analytical domain, not necessarily the name of either entire database;
- initial domain names from Decision 0009 remain planning concepts rather than guaranteed physical schemas.

Decision 0010 does not replace Decision 0009. Decision 0009 is explicitly amended to reflect the separate Core Operational and Internet Marketing Intelligence database boundaries.

## Research dependencies

Before acceptance or implementation planning, evaluate:

- Research Batch B provider rights, storage, retention, and cross-project reuse findings;
- expected ranking and visibility data families;
- provider-neutral observation modeling needs;
- volume and growth estimates;
- backup, recovery, partitioning, and archival patterns;
- cross-database transaction and consistency needs;
- privacy and customer-data boundaries;
- whether one server with two databases is operationally sufficient for the first implementation.

## Acceptance criteria

This decision is ready for human acceptance only when the reviewer confirms that:

- the Internet Marketing Intelligence workload is distinct enough to justify a dedicated database boundary;
- the database is broader than a SERP-only store;
- future ranking and visibility data types are intentionally expected;
- one PostgreSQL deployment with two databases is an acceptable initial direction;
- schema and physical design remain deferred;
- a focused human/steward database-design session is required before schema work;
- Decision 0009 will be amended explicitly rather than contradicted silently;
- provider, rights, workload, privacy, and recovery research still gates implementation.

## Open questions

- What should the final databases be named?
- Which record families stay in Core versus Intelligence?
- Which provider-job metadata belongs in Core, Intelligence, or both through references?
- Which raw payloads remain outside Postgres?
- Which ranking and recommendation surfaces will matter first?
- What volume and retention assumptions are realistic?
- Should analytical replicas or a warehouse appear later?
- What consistency guarantees are needed across the database boundary?
- What minimum prototype proves the separation is worthwhile?

## Related files

- `01-project-standard/internet-marketing-intelligence-vision.md`
- `01-project-standard/kaizen-vision-and-architecture-alignment.md`
- `04-design-decisions/0004-system-of-record-boundaries.md`
- `04-design-decisions/0005-api-only-structured-data-access.md`
- `04-design-decisions/0009-operational-postgres-and-observatory-boundary.md`
- `05-specs/operational-postgres-authority.md`
- `03-research-results/010-postgres-mcp-typed-tool-audit-claude-summary.md`
- `03-research-results/012-step-5a-composed-tool-capability-map.md`
- `ROADMAP.md`
