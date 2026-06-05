# Kaizen Vision and Architecture Alignment

Status: active draft
Date: 2026-06-05
Owner: Kaizen project steward

## Purpose

Preserve the intended Kaizen vision in one authoritative planning document so it cannot be lost, diluted, or reconstructed incorrectly from scattered chat history.

This document aligns the project's central identity before additional research, roadmap design, or implementation planning proceeds.

It does not authorize implementation. It defines the system Kaizen is intended to become and identifies the capabilities that later research, decisions, specifications, and implementation roadmaps must address.

## Central definition

Kaizen is a continuously improving, agent-connected project intelligence system built around a dynamic Obsidian vault.

It gathers, organizes, evaluates, and compounds project knowledge; connects external and operational evidence to human-reviewed decisions; and produces audited implementation-ready handoffs that coding agents can execute in each project's own source repository.

Kaizen is not merely a governed documentation pipeline.

It is intended to become a living intelligence environment that:

- takes projects from early ideas to implementation-ready instructions
- helps humans and agents resume work without reconstructing context
- continuously gathers relevant external and operational intelligence
- improves projects as new evidence appears
- compounds reusable knowledge across projects
- supports technical, product, business, marketing, and strategic decisions
- learns from implementation outcomes and operational results
- improves its own contracts, workflows, retrieval, and governance through observed evidence

## Current planning status

The current repository, `kaizen-docs`, is the planning and stewardship repository used to design Kaizen.

It is not the production Kaizen vault.

The production vault, operational database, semantic retrieval layer, MCP/tool layer, ingestion services, and agent integrations have not yet been implemented.

The current work must therefore distinguish between:

- the intended system vision
- accepted architecture and governance decisions
- draft specifications
- open research questions
- future implementation choices

## Intended project flow

Kaizen should support the full project lifecycle:

```text
project idea or migrated project material
-> project definition and current-state orientation
-> research questions and intelligence requests
-> source collection and operational observations
-> source summaries and reusable knowledge
-> claims, conflicts, risks, and opportunities
-> human-reviewed decisions and strategies
-> specifications
-> audit and implementation-readiness review
-> governed context pack and task packet
-> coding-agent implementation in the project's source repository
-> completion report, test evidence, deviations, and discoveries
-> updated project intelligence, decisions, specs, and next work
```

This is a loop, not a one-way pipeline.

## Core feedback loops

### Research and intelligence loop

```text
project needs
-> research questions
-> source discovery and ingestion
-> evidence and observations
-> claims and reusable knowledge
-> decisions and strategy
-> new gaps and monitoring needs
```

### Implementation loop

```text
task packet
-> coding-agent implementation
-> tests and completion report
-> discovered constraints, failures, and deviations
-> Kaizen review
-> updated claims, decisions, specs, and current state
```

### Operational and market loop

```text
implemented or operating project
-> operational, customer, market, SERP, citation, and performance signals
-> governed operational results
-> project and portfolio analysis
-> revised strategy, priorities, research, decisions, and specs
```

### Kaizen self-improvement loop

```text
observed workflow friction and outcomes
-> evidence about templates, fields, validators, tools, retrieval, and agent behavior
-> governed review
-> improved Kaizen contracts, workflows, permissions, and interfaces
```

Kaizen must improve through observed evidence, not speculative structure or unreviewed agent preference.

## System layers

### Obsidian and canonical Markdown

Obsidian is the primary human interface.

Canonical project intelligence remains portable Markdown with flat validated YAML, ordinary headings, durable IDs, explicit provenance, and validated relationships.

The Obsidian experience should eventually be dynamic and operationally useful, including derived views for:

- project command centers
- project resumption
- incoming intelligence
- research queues
- unresolved claims and conflicts
- stale evidence and approvals
- pending decisions and audits
- review queues
- implementation readiness
- portfolio priorities
- reusable knowledge and capabilities
- recent implementation and operational feedback

Dynamic views are convenience and control surfaces. They must not become hidden canonical truth.

### Operational Postgres database

Kaizen requires a broader operational Postgres database. The Observatory is one major domain within it, not the name of the entire database.

Expected governed domains currently include:

```text
observatory
ingestion
automation
governance
audit
reference
```

The database should own structured operational records such as:

- ingestion sources and source versions
- research and monitoring jobs
- provider requests and responses
- scheduled and manual runs
- SERP observations
- ranking and visibility observations
- LLM citation, mention, and retrievability observations
- AI Overview and related search-surface observations
- market and competitor observations
- cost and usage records
- workflow and agent action logs
- provenance
- governed result summaries
- approval and operational governance events where later adopted

Postgres must not become the canonical home for project narrative, accepted decisions, specification bodies, or audit prose.

### Observatory

The Observatory is the intelligence and analysis domain over approved operational observations.

It should support project and portfolio questions such as:

- what changed in a market or search surface
- which opportunities are strengthening or weakening
- which projects share the same market or capability need
- which assumptions are contradicted by current evidence
- which content, products, services, or entities have weak visibility
- where LLM citation or retrievability is changing
- which projects deserve new research or strategic review

Observatory outputs become governed result summaries before they influence canonical project claims, decisions, or strategies.

### Qdrant

Qdrant is a rebuildable semantic retrieval layer over approved canonical knowledge.

It owns no canonical truth.

Its practical purpose includes helping Kaizen assemble relevant context for:

- research
- project resumption
- claim review
- decision review
- specification drafting
- audit
- task-packet creation
- coding-agent handoff
- cross-project knowledge discovery

Retrieval must preserve project scope, authority, supersedence, freshness, and privacy boundaries.

### MCP and typed tools

Hermes, GPT, Claude, and other approved agents are expected to connect to Kaizen through constrained MCP servers or equivalent typed tool interfaces.

MCP is an integration protocol, not an authorization boundary by itself.

Kaizen must expose narrow capabilities with explicit scope, validation, logging, rate limits, cost controls, and permission checks.

Broad unrestricted vault, filesystem, SQL, provider, or Qdrant access is not acceptable merely because the connection uses MCP.

### Project source repositories

Implementation occurs in each project's own source repository.

Kaizen supplies governed context packs, specifications, task packets, acceptance criteria, and implementation constraints.

Coding agents return completion reports, validation evidence, failures, deviations, and discovered constraints to Kaizen for review and learning.

## Agent roles

### Hermes

Hermes is Kaizen's designated operational and research clerk.

Expected responsibilities include:

- routine collection and source processing
- filing and organization
- search and retrieval
- source-summary drafting
- claim extraction drafts
- research queue support
- monitoring support
- context-pack assembly support
- validation and hygiene
- staged note creation and proposed diffs after controls are proven

Hermes does not approve, promote, issue human verdicts, grant authority, commit or push, directly mutate Postgres or Qdrant, spend money without authorization, or mark work implementation-ready.

### GPT and Claude

Frontier reasoning models such as GPT and Claude are intended to support higher-reasoning tasks, including:

- deep research synthesis
- architecture analysis
- contradiction detection
- evidence review
- claim and decision critique
- strategy analysis
- specification critique
- audit drafting
- implementation-readiness analysis
- task-packet review
- independent second opinions

They remain constrained collaborators, not authority-bearing actors.

### Coding agents

Coding agents implement approved task packets in project source repositories.

They must:

- respect the approved scope and do-not-touch boundaries
- run required validation
- report exact changes
- report failures and deviations
- produce a completion report
- avoid inventing unresolved product or architecture decisions

### Humans

Humans retain final authority over:

- approval and rejection
- accepted claims, decisions, specs, and task packets
- audit verdicts
- promotion
- project and portfolio priorities
- spending and paid data collection
- external actions and publication
- credentials and permissions
- exceptions and risk acceptance

## Research and intelligence sources

Kaizen should eventually gather or ingest project-relevant information from source classes including:

- scientific papers
- patents and patent applications
- official technical documentation
- standards and specifications
- government and institutional publications
- news articles
- professional and expert analysis
- video and podcast transcripts
- conference talks and presentations
- public datasets
- provider APIs
- SERP and search visibility providers such as DataForSEO
- LLM citation and retrievability measurements
- competitor and market observations
- vendor claims
- community discussions and anecdotes
- project-provided documents and operational records

Source type, trust, provenance, freshness, and evidentiary role must remain distinguishable.

Kaizen must preserve separation between:

- what was directly observed
- what a source claims
- what an agent inferred
- what a human accepted

## Research queues and continuous monitoring

Kaizen should represent not only completed research, but what still needs to be learned.

Future governed capabilities should include:

- research questions
- intelligence requests
- missing evidence
- unresolved conflicts
- proposed source ingestion
- requested paid data pulls
- periodic monitoring topics
- recheck triggers
- research priority
- budget or approval requirements
- failed or partial research attempts

Projects should be able to request one-time research and continuing monitoring.

## Freshness and staleness

Different knowledge classes age at different rates.

Kaizen must eventually govern freshness without pretending all evidence has the same shelf life.

Examples:

| Knowledge class | Typical behavior |
|---|---|
| mathematical principles | usually stable |
| scientific conclusions | may evolve with new research |
| patents and legal landscapes | change over time |
| official product or API documentation | may change quickly |
| competitor positioning and pricing | volatile |
| news | immediately time-sensitive |
| SERP and ranking data | highly volatile |
| LLM citation and retrievability data | highly volatile |

The eventual design may include captured time, reviewed time, source update time, freshness classes, recheck triggers, and stale-evidence handling. Exact schema remains a later research and specification task.

## Cross-project and durable knowledge

Kaizen should compound knowledge across projects rather than isolate every project in a silo.

It needs explicit distinctions between:

- project-local intelligence
- shared domain knowledge
- cross-project intelligence
- reusable implementation and strategy patterns

A durable wiki or knowledge layer should eventually hold reusable understanding such as:

- domain concepts
- technical and architectural patterns
- scientific findings
- market mechanisms
- research methods
- recurring constraints
- lessons learned
- reusable decision principles
- capability patterns

This durable layer should inform future projects while preserving provenance, scope, and authority.

## Capability reuse

Kaizen should help identify when a project need or implementation result represents:

- project-specific functionality
- a reusable project pattern
- shared Kaizen tooling
- a platform capability in another system
- a standalone shared service

Examples include:

- source ingestion
- transcript processing
- SERP collection
- LLM citation testing
- report generation
- review queues
- cost controls
- monitoring
- research synthesis
- context-pack generation

This capability-reuse function should reduce duplicated infrastructure and expose when several projects depend on the same missing capability.

## Portfolio intelligence

Kaizen should eventually support portfolio-level questions across all projects, including:

- which projects have the strongest evidence
- which projects have the clearest opportunities
- which projects are blocked by missing research or decisions
- which assumptions have become stale
- which projects share capabilities or dependencies
- where research is being duplicated
- which project deserves time, money, data, or compute next
- which projects are producing reusable knowledge
- which projects have unresolved risks or failed audits

Portfolio views are derived control surfaces over canonical notes and approved operational data, not separate hidden truth.

## Context packs and project resumption

Kaizen should enable humans and agents to continue work without repeated manual reconstruction.

It must eventually answer:

- where the project left off
- what is currently true
- what was accepted
- what is proposed
- what is blocked
- what changed
- what is stale
- what must be read first
- what must not be reopened without new evidence
- what the next valid action is

Kaizen should assemble governed context packs appropriate to the task rather than sending an agent the entire vault or relying on unfiltered semantic similarity.

## Human review queues

A dynamic Kaizen experience should expose human queues such as:

- staged notes awaiting review
- claims awaiting acceptance
- conflicts awaiting resolution
- proposed decisions awaiting approval
- specs awaiting audit
- failed validations
- stale approvals
- task packets awaiting implementation-readiness approval
- paid data requests awaiting authorization
- agent escalations
- source-ingestion requests
- research items awaiting prioritization

These queues are central to operating a governed multi-agent system.

## Cost and resource governance

Kaizen may coordinate paid or resource-intensive work such as:

- SERP pulls
- crawl and extraction jobs
- transcript generation
- embeddings
- frontier-model research
- recurring monitoring
- provider APIs
- storage and compute

The operational system should eventually support questions such as:

- what intelligence gathering cost per project
- which jobs have standing approval
- which providers or sources are producing value
- which monitoring jobs should continue
- which project is consuming resources
- when new approval is required
- whether collected intelligence justifies its cost

Exact cost-control schemas and policies remain later research and implementation work.

## Project migration and onboarding

When the production Kaizen vault is ready, useful project documents from existing repositories will be migrated into Kaizen.

Examples include Neon Ronin, SearchClarity, and future projects.

Migration does not mean moving source code or operational runtime state into the vault.

A governed migration process should eventually address:

- document inventory
- canonical-home classification
- migrate versus reference decisions
- source-repository links
- duplicate and conflict handling
- stable identity assignment
- provenance
- historical versus current material
- project-local versus shared-domain knowledge
- post-migration ownership
- validation and review

Exact migration tooling is deferred until the vault contracts and folder conventions are accepted.

## Dynamic Obsidian experience

Kaizen should be powerful and dynamic for human use while remaining portable and understandable as raw Markdown.

Potential derived experiences include:

- project and portfolio dashboards
- command centers
- current-state views
- research and monitoring queues
- graph and relationship navigation
- pending human reviews
- implementation-readiness views
- stale evidence and decision alerts
- recent operational intelligence
- context-pack generation
- project resume views
- reusable knowledge discovery

Plugins and UI layers may provide convenience, but important information must remain available through canonical Markdown and approved operational APIs.

## Kaizen planning and implementation sequence

The recommended program sequence is:

```text
1. Preserve and align the full Kaizen vision.
2. Have Claude independently audit the vision and current repository.
3. Ask Claude to identify research gaps, contradictions, and claims needing verification.
4. Create evidence-focused research plans and execute the highest-value research.
5. Reconcile research into decisions and specifications.
6. Complete and audit the Kaizen Project Standard.
7. Define implementation-planning inputs and dependencies.
8. Have Claude propose a dependency-ordered implementation roadmap.
9. Independently review and reconcile that roadmap.
10. Produce approved task packets for small, reversible implementation batches.
11. Build, test, observe, and feed implementation results back into Kaizen planning.
```

Claude's future roadmap is a proposal, not automatic doctrine. It must be checked against accepted Kaizen boundaries and verified evidence before implementation begins.

## Research expectations for Claude

Claude should be asked to:

- read the complete Kaizen planning repository
- treat this document as the central vision statement
- distinguish accepted doctrine, active drafts, research evidence, and open vision requirements
- identify where external research is needed
- recommend primary or authoritative sources where possible
- separate stable architecture decisions from implementation-specific choices
- identify contradictions and missing feedback loops
- propose research questions before proposing tables or software
- avoid assuming a specific MCP server, database schema, Qdrant layout, agent router, plugin stack, or monitoring cadence without evidence
- return adopt, modify, reject, and defer recommendations

## Implementation-roadmap expectations

The future implementation roadmap should cover at minimum:

- production vault structure and migration
- canonical Markdown contracts and validation
- ID generation
- staging and promotion
- Obsidian human interface and dynamic views
- operational Postgres domains
- source ingestion and monitoring
- Observatory analysis and governed results
- Qdrant indexing and retrieval
- context-pack assembly
- MCP and typed tool interfaces
- Hermes integration
- GPT and Claude integration boundaries
- agent routing and escalation
- human review queues
- cost and authorization controls
- source trust and freshness
- project and portfolio intelligence
- implementation feedback ingestion
- operational feedback ingestion
- backup, recovery, security, monitoring, and CI
- hammer-test gates
- Kaizen self-improvement telemetry and governance

The roadmap must order work by dependencies and evidence, not by novelty.

## Non-goals of this document

This document does not decide:

- exact Postgres tables
- exact Qdrant collections or chunk sizes
- exact MCP server implementation
- exact Obsidian plugins
- exact source trust scores
- exact freshness fields
- exact monitoring schedules
- exact model router
- exact dashboard queries
- exact project migration tooling
- exact provider contracts

Those require research, decisions, specifications, and later implementation planning.

## Immediate consequences

1. Vision and architecture alignment becomes the active planning priority.
2. The current document-contract batch remains valuable but must be reviewed against this broader vision.
3. The planning roadmap must be revised so this alignment and research phase precede final v0.2 standard acceptance.
4. The entrypoint must use this document as a read-first source.
5. "Postgres Observatory" terminology must be corrected to distinguish the broader Operational Postgres database from the Observatory domain.
6. Claude should next perform an independent research-gap and architecture-alignment review.
7. No implementation roadmap should be accepted until that research and reconciliation are complete.

## Related files

- `ROADMAP.md`
- `00-entrypoint/LLM_START_HERE.md`
- `01-project-standard/standard-revision-plan.md`
- `04-design-decisions/0004-system-of-record-boundaries.md`
- `04-design-decisions/0005-api-only-structured-data-access.md`
- `05-specs/postgres-observatory-authority.md`
- `05-specs/kaizen-validation-gate-spec.md`
- `05-specs/kaizen-hammer-test-strategy.md`
- `07-hermes/hermes-permission-matrix.md`
- `03-research-results/006-document-contract-standards-reconciliation.md`
