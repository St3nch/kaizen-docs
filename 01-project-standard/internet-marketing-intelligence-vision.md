# Kaizen Internet Marketing Intelligence Vision

Status: active vision draft
Date: 2026-06-05
Owner: Kaizen project steward
Related documents:
- `01-project-standard/kaizen-vision-and-architecture-alignment.md`
- `04-design-decisions/0004-system-of-record-boundaries.md`
- `04-design-decisions/0009-operational-postgres-and-observatory-boundary.md`
- `04-design-decisions/0010-dedicated-internet-marketing-intelligence-database.md` - proposed
- `05-specs/operational-postgres-authority.md`
- `03-research-results/012-step-5a-composed-tool-capability-map.md`
- `ROADMAP.md`

## Purpose

Preserve a central part of the intended Kaizen vision before provider research, data-retention research, database design, or implementation planning proceeds.

Kaizen is intended to become a shared, longitudinal Internet Marketing Intelligence system for the owner's full project portfolio.

This capability is not a project-specific SEO dashboard and is not limited to SearchClarity.

It should accumulate public-market and operational evidence over years, reuse that evidence across many projects, detect patterns and opportunities, propose evidence-grounded strategies, and learn from the outcomes of approved project experiments.

This document defines the intended capability and its architectural consequences. It does not authorize implementation, choose providers, define database tables, select models, or approve automated business actions.

## Central definition

> Kaizen's Internet Marketing Intelligence capability is a shared, longitudinal signal system that accumulates public-market evidence across projects, connects that evidence to project-specific goals and outcomes, and produces reviewable opportunity candidates and strategy hypotheses grounded in traceable evidence.

The intended operating loop is:

```text
public internet, marketplace, research, and project-performance evidence
-> normalized longitudinal observations
-> cross-source and cross-project analysis
-> signals, anomalies, trends, and gaps
-> opportunity candidates and strategy hypotheses
-> human review and bounded project experiments
-> measured outcomes
-> improved future analysis and recommendations
```

Kaizen should optimize for evidence quality, historical compounding, provenance, freshness, cross-project reuse, and measured outcomes rather than raw speed.

## Analogy: portfolio-wide marketing signal engine

The closest useful analogy is a quantitative trading system, but operating on marketing and business-intelligence timescales rather than microseconds.

A trading system consumes shared market data and allows multiple strategies or portfolios to act on that data through their own objectives and risk constraints.

Kaizen should similarly support:

```text
shared marketing-intelligence core
├── SearchClarity lens
├── WordPress plugin project lens
├── Fiverr market lens
├── Etsy market lens
├── Amazon market lens
├── future product and service lenses
└── portfolio-level opportunity analysis
```

One public observation may be relevant to many projects. The observation should not become owned exclusively by the project that first requested it.

## Long-term objective

After accumulating sufficient governed evidence, Kaizen should be able to produce reviewable statements such as:

> Chaz, I found a recurring visibility gap across several markets. The evidence points toward opportunity X. Here are the supporting observations, affected projects, uncertainty, contradictory evidence, and the smallest experiment that could test it.

Or:

> Chaz, a strategy hypothesis appears to be forming from SERP changes, LLM citation patterns, recent research, marketplace behavior, and prior project outcomes. It is not proven, but it is grounded enough to justify review and a bounded test.

Kaizen must show its evidence and uncertainty. It must not silently convert correlation, model output, or a single provider observation into accepted strategy.

## Intelligence layers

### Layer 1 - Shared longitudinal observations

The shared foundation consists of reusable observations about public or governed external environments.

Potential observation classes include:

- keywords and queries;
- search-engine result pages;
- ranking positions;
- result features and AI Overviews;
- LLM mentions, citations, and retrievability;
- domains, pages, products, listings, sellers, and entities;
- marketplace search results;
- competitor visibility and movement;
- public pricing and positioning;
- geographic, language, device, and temporal context;
- provider, collection method, and raw capture provenance;
- cost, request, job, and run evidence;
- papers, patents, articles, standards, datasets, and transcripts;
- public customer complaints, questions, and community signals where collection is lawful and appropriate.

A public observation should be stored once when reuse is permitted and then associated with every relevant project, request, analysis, or experiment.

### Layer 2 - Project lenses

Projects interpret shared evidence through their own goals and constraints.

A project lens may include:

- target queries and markets;
- tracked domains, entities, products, services, sellers, or listings;
- competitors;
- geography, language, and device priorities;
- business model and conversion goals;
- risk tolerance;
- budget and approved provider spend;
- active experiments;
- private analytics and project-specific outcomes;
- customer or contractual restrictions.

Project relevance is separate from observation ownership.

```text
shared observation
├── relevant to SearchClarity
├── relevant to a WordPress plugin project
├── relevant to an Etsy service opportunity
└── relevant to a future product not yet created
```

### Layer 3 - Derived signals and intelligence

Kaizen should derive reviewable analytical outputs from accumulated observations.

Potential outputs include:

- trend changes;
- ranking and citation movement;
- market saturation;
- competitor acceleration or decline;
- visibility gaps;
- weakly served query clusters;
- cross-market similarities;
- emerging entities, topics, formats, or technologies;
- repeated customer or buyer pain points;
- unusual changes requiring investigation;
- evidence conflicts;
- transferable strategy patterns;
- candidate market opportunities;
- project-fit and experiment candidates;
- freshness and confidence warnings.

Derived intelligence is not automatically accepted truth. It must preserve the observations, methods, assumptions, and uncertainty behind it.

### Layer 4 - External research and explanatory knowledge

Market observations show what appears to be happening. Research may help explain why, what may happen next, or how to test a response.

Relevant evidence may include:

- scientific papers;
- patents and patent applications;
- official documentation and standards;
- search-engine and marketplace documentation;
- technical articles and expert analysis;
- public datasets;
- conference talks and transcripts;
- legal, policy, and platform changes;
- project implementation and operational evidence.

The intended synthesis is:

```text
market observations
+
external research
+
portfolio history
+
project constraints
=
reviewable opportunity or strategy hypothesis
```

No source class automatically outranks all others. Evidentiary role, trust, freshness, provenance, and contradiction must remain visible.

### Layer 5 - Opportunity and strategy candidates

Kaizen should produce candidates for human review, not autonomous doctrine.

A future opportunity candidate should be able to include:

- concise opportunity statement;
- supporting observations and sources;
- affected projects or markets;
- historical trend window;
- confidence and uncertainty;
- contradictory or missing evidence;
- estimated freshness;
- legal or provider-use constraints;
- expected cost and risk;
- smallest useful experiment;
- conditions that would falsify the hypothesis.

A future strategy hypothesis should include similar evidence and should distinguish:

- directly observed facts;
- source claims;
- statistical or analytical inference;
- model-generated reasoning;
- human acceptance or rejection.

## Shared does not mean unrestricted

The intelligence system should maximize lawful reuse while preserving privacy and authority boundaries.

Conceptual visibility classes include:

```text
global public
portfolio internal
project private
customer restricted
```

These are planning concepts, not accepted enum values.

Examples of potentially shareable portfolio-wide evidence:

- public SERPs;
- public websites;
- public marketplace listings;
- public papers and patents;
- public news and documentation;
- provider observations whose terms permit retention and reuse.

Examples requiring tighter scope:

- private analytics;
- customer data;
- revenue and conversion data;
- unpublished product information;
- credentials;
- contractual or licensed datasets;
- confidential strategies;
- customer-specific rankings or reports.

Cross-project intelligence must not become cross-project data leakage.

## Observation reuse and request provenance

Kaizen should distinguish the observed market fact from the project requests and jobs that caused it to be collected.

Conceptually:

```text
observation
!=
request
!=
provider job
!=
project relevance
!=
analysis
!=
strategy candidate
```

If two projects request the same useful observation within an acceptable time window, Kaizen may be able to reuse one provider capture when licensing, freshness, and operational rules permit.

The system must still preserve:

- which project requested it;
- who or what approved the spend;
- the provider and exact request parameters;
- provider cost;
- capture time;
- raw-response provenance or retained reference;
- which analyses and candidates used it;
- which projects later became associated with it.

This separation reduces duplicated cost without erasing accountability.

## Operational Postgres and Observatory consequences

Operational Postgres is the long-lived structured system for governed observations, jobs, requests, costs, analyses, and outcomes.

The Observatory is the analytical and intelligence domain over approved operational evidence.

The Observatory should eventually support portfolio-wide questions such as:

- what visibility or demand patterns are strengthening across markets;
- which opportunities appear in more than one project or marketplace;
- which projects could share one research or collection effort;
- which search, citation, or marketplace changes matter to multiple projects;
- which prior strategies transferred successfully across markets;
- which recommendations failed and under what conditions;
- which public observations are stale, contradictory, or too weak to support action;
- where a new product, service, content asset, or experiment may be justified.

Potential workload concepts include:

```text
observations
entities and markets
project relevance
signals and trends
opportunity candidates
strategy hypotheses
experiments
outcomes and evaluations
```

These are not table names and do not authorize schema design.

## Proposed dedicated intelligence database boundary

Detailed proposed decision:

- `04-design-decisions/0010-dedicated-internet-marketing-intelligence-database.md`

The long-lived Internet Marketing Intelligence workload is likely distinct enough from Kaizen's core governance, audit, automation, and workflow state to justify a dedicated database boundary.

The current proposed direction is:

```text
one PostgreSQL deployment initially
├── Kaizen Core Operational database
└── Internet Marketing Intelligence database
```

The intelligence database is intentionally broader than a SERP store. It may eventually hold additional ranking, recommendation, visibility, marketplace, citation, and discovery data as Kaizen and its projects evolve.

No exact schema, table, partition, index, extension, provider model, or retention design is approved.

Before physical design begins, the owner and project steward must hold a focused database-design session covering known and anticipated ranking data, provider payloads, observation identity, project relationships, privacy, workload, growth, retention, recovery, and cross-database consistency.
## Canonical Markdown consequences

Operational Postgres may own structured observations, analyses, and workflow records.

Canonical Markdown should own the durable human-readable project intelligence that results from review, including where appropriate:

- accepted claims;
- reviewed opportunity briefs;
- strategy decisions;
- project plans;
- specifications;
- experiment plans;
- outcome interpretation;
- lessons learned;
- reusable strategy principles.

A database-generated candidate must not silently become project doctrine. It enters the governed review flow before it changes a project's accepted strategy.

## Learning from outcomes

The feedback loop is essential.

```text
Kaizen proposes a candidate strategy
-> human approves a bounded experiment
-> project implements the experiment
-> ranking, citation, traffic, conversion, revenue, cost, or qualitative outcomes return
-> Kaizen compares expected and observed results
-> the hypothesis is strengthened, weakened, narrowed, rejected, or marked inconclusive
-> lessons inform future projects and strategies
```

Kaizen should eventually retain:

- what was proposed;
- why it was proposed;
- what evidence supported it;
- what project tested it;
- what was actually implemented;
- what changed during implementation;
- measured results;
- confounding variables;
- contradictory evidence;
- transferability to other projects or markets;
- whether the result remained valid over time.

Without outcome feedback, Kaizen would produce plausible ideas without learning whether they work.

## Human authority and automation boundary

Kaizen may identify, rank, and explain opportunities and strategy hypotheses.

Humans retain authority over:

- accepting or rejecting a strategy;
- approving provider spend;
- authorizing experiments;
- selecting projects to act;
- publishing content or changing live systems;
- accepting risk;
- deciding whether evidence is sufficient.

Future automation may perform approved collection, analysis, monitoring, and reporting. It must not autonomously commit the portfolio to business, financial, legal, customer, or publication actions without explicit authority.

## Research Batch B consequences

Research Batch B must investigate the data foundation for this intelligence capability, not merely compare provider feature lists.

It should research:

- which public and licensed data may be retained longitudinally;
- which provider terms allow historical storage, cross-project reuse, derived analysis, and model-assisted processing;
- what must be stored, summarized, transformed, cited, linked, or left at the source;
- DataForSEO and credible alternatives;
- SERP, ranking, marketplace, visibility, and LLM-citation providers;
- source-class copyright, licensing, and retention boundaries;
- scientific-paper, patent, news, documentation, dataset, and transcript constraints;
- freshness, recapture, and historical-snapshot requirements;
- lawful observation deduplication and reuse across projects;
- public versus portfolio, project, customer, and licensed-data scope;
- provenance needed for opportunities and strategy hypotheses;
- cost attribution when one observation serves many projects;
- provider and source changes that require revalidation;
- restrictions on redistribution, quotation, embeddings, summaries, and derived datasets;
- how evidence-backed candidates should expose uncertainty and contradiction.

The result should inform later storage, retrieval, governance, and implementation planning without prematurely selecting providers or schemas.

## Research Batch C and D consequences

Batch C should later address:

- long-term observation storage and partitioning requirements;
- shared versus scoped data access;
- backup, restore, retention, and deletion;
- Qdrant rebuildability from governed sources;
- historical-versus-current observation handling;
- analytical workload boundaries;
- prototype requirements for portfolio-wide and project-scoped queries.

Batch D should later address:

- evidentiary roles and source trust;
- signal confidence and uncertainty;
- freshness and recheck policies;
- opportunity and strategy candidate assembly;
- context packs for portfolio and project decisions;
- contradiction handling;
- conditions for escalating a signal to human review.

## Explicit deferrals

Do not yet define:

- exact Postgres tables;
- exact schemas or physical database domains;
- exact observation deduplication keys;
- exact opportunity or strategy scores;
- exact machine-learning models;
- exact anomaly-detection methods;
- exact provider set;
- exact collection cadence;
- exact retention periods;
- automatic portfolio prioritization;
- automatic business action;
- exact Markdown note types for opportunities or strategies.

These require provider, legal, workload, and prototype evidence.

## Vision acceptance questions

Human review should confirm:

1. Kaizen is intended to become a portfolio-wide Internet Marketing Intelligence system, not only a project documentation and implementation-planning system.
2. Public and lawfully reusable observations should compound across projects rather than remain trapped in project silos.
3. Projects retain separate goals, private data, budgets, experiments, and outcome context.
4. Kaizen should produce reviewable opportunity and strategy candidates with evidence, uncertainty, contradictions, and recommended experiments.
5. Outcomes must return to Kaizen so the system learns which recommendations work and under what conditions.
6. Operational Postgres and the Observatory are central to the long-term intelligence capability.
7. Canonical Markdown remains the human-reviewed home for accepted strategies, decisions, and durable project intelligence.
8. Provider rights, licensing, retention, privacy, and cost constraints must be researched before implementation design.

## Immediate planning consequence

Before Research Prompt 007 is written, the roadmap and central vision should link to this document.

Prompt 007 should use this vision as a required input and investigate the legal, provider, retention, freshness, provenance, reuse, and cost foundations needed to build the intended intelligence machine.
