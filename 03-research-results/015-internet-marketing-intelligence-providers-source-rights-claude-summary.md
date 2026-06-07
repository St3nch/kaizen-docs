# Research Result 015 - Internet Marketing Intelligence Providers, Source Rights, and Longitudinal Reuse

Status: active evidence summary with steward reconciliation
Date: 2026-06-06
Source: Claude Research Batch B report supplied by the project owner
Prompt: `02-research-prompts/007-internet-marketing-intelligence-providers-source-rights.md`

## Purpose

Preserve and reconcile the no-new-spend Research Batch B report covering DataForSEO, ranking-intelligence requirements, provider neutrality, source rights, longitudinal reuse, controlled sampling, and secondary-provider calibration.

This document is evidence and steward reconciliation. It does not authorize provider spending, paid API calls, schema design, implementation, or indefinite retention of provider payloads.

## Executive conclusion

The report strongly supports DataForSEO as Kaizen's first primary ranking-intelligence provider while preserving a provider-neutral architecture.

The durable direction is:

```text
DataForSEO
= first primary provider adapter

Kaizen intelligence model
= provider-neutral, longitudinal, portfolio-wide evidence system

Secondary providers
= selective calibration or gap-filling only
```

The report also strengthens the proposed dedicated Internet Marketing Intelligence database boundary.

## Adopt

### DataForSEO as the leading primary provider

Adopt DataForSEO as the leading first provider candidate because it offers broad machine-readable coverage across:

- organic and feature-rich SERPs;
- AI Overviews;
- keyword and domain intelligence;
- rankings and historical visibility;
- LLM responses;
- LLM mentions and citations;
- backlinks;
- technical and on-page intelligence;
- content, business, merchant, app, and domain surfaces where later justified.

This is a provider direction, not an exclusive commitment.

### Existing empirical corpus

Adopt the VEDA predecessor payload corpus as empirical evidence for:

- Google Organic Advanced SERP;
- Google Organic SERP with AI Overview;
- ChatGPT LLM Responses;
- Keywords For Site;
- Keyword Overview;
- Ranked Keywords;
- Historical Rank Overview;
- Search Intent;
- Keywords For Categories.

Real payloads outrank marketing pages when evaluating response shape.

### Distinct AI and LLM evidence families

Adopt the requirement to keep these distinct:

```text
AI Overview observation
LLM response observation
LLM scraper observation
LLM mention cluster
LLM citation observation
AI keyword-volume estimate
```

They differ in provenance, capture semantics, rights, cost, and analytical meaning.

### Provider-neutral observation posture

Adopt these conceptual requirements for every observation:

- provider identity;
- product and endpoint identity;
- request and provider task identity;
- capture time;
- location, language, device, engine, platform, model, or marketplace dimensions;
- raw artifact reference and hash where retained;
- provider cost and usage;
- normalization version;
- requesting project;
- later-related projects;
- privacy and rights class;
- freshness, correction, and invalidation state.

Provider-reported metrics remain provider-attributed.

### Collection tiers

Adopt four collection tiers:

```text
baseline
periodic monitoring
investigation
calibration
```

Collection should maximize information value per paid pull rather than run constantly.

### Controlled paid capture

Adopt the principle that future paid captures require:

- exact research question;
- approved endpoint family;
- conceptual target;
- maximum rows or calls;
- authorized provider-consumption ceiling;
- stop conditions;
- duplicate-call prevention;
- unique filenames;
- paired inventory notes;
- cost verification;
- steward approval before execution.

Unused account credit is not authorization to spend.

### Shared observations and project relationships

Adopt the distinction between:

```text
observation
request that caused collection
provider job and cost
requesting project
later-related projects
analysis or signal
strategy candidate
experiment and outcome
```

A lawful shared observation should be stored once and related to many projects rather than copied as project-owned truth.

### Provider disagreement

Adopt preservation of provider disagreement.

Do not average provider-specific measurements into false certainty.

```text
DataForSEO metric: X
secondary-provider metric: Y
capture and methodology differ
disagreement retained
```

### Dedicated Intelligence database direction

Research Batch B provides additional evidence for a dedicated Internet Marketing Intelligence database boundary because the workload is:

- portfolio-wide;
- longitudinal;
- provider-heavy;
- high-volume;
- rights-sensitive;
- analytically distinct from Kaizen Core operations;
- reusable across projects.

## Modify

### Provider rights remain unresolved

The report was too confident about retention, cross-project reuse, embeddings, and customer-report rights.

The correct current posture is:

```text
technical capability to retain and analyze
!=
verified contractual permission for every use
```

Until written provider clarification or legal review exists, classify rights claims as:

- expressly permitted;
- expressly restricted;
- not expressly prohibited;
- not expressly granted;
- provider clarification required;
- legal review required before external or customer-facing use.

### Raw archive policy remains provisional

Do not adopt "archive forever" as policy.

Use this planning posture:

```text
retain raw provisionally where allowed
-> assign rights and privacy class
-> preserve hash and provenance
-> review retention window
-> purge, reduce, or continue based on rights, value, and cost
```

Exact retention periods and storage technology remain deferred.

### Secondary providers remain optional

Ahrefs, Semrush, AI-visibility platforms, and other sources are candidates for decision-triggered calibration.

Do not establish recurring multi-provider subscriptions or annual calibration budgets now.

A later calibration request must state:

- decision being validated;
- metric family;
- selected provider;
- expected value of disagreement analysis;
- minimum purchase or subscription period;
- export or API availability;
- maximum cost;
- stopping conditions.

### Competing-provider commercial details require fresh verification

Specific plan names, prices, API-unit rules, and coverage claims are volatile.

Treat them as candidate information requiring fresh verification at purchase time, not doctrine.

### Capture packet needs project relevance

A future paid packet should include:

- one continuity/control query from the existing corpus;
- one approved Kaizen-project query;
- one entity or competitor comparison where useful.

Do not place customer-private material in the shared evidence corpus.

### Exact provider request bodies are execution-time artifacts

Do not freeze request JSON months before execution.

The planning packet should specify endpoint, question, target, row/call ceiling, and budget. Generate and validate exact request bodies against current documentation immediately before the call.

### OnPage remains research-further

Do not assume Kaizen should replace provider-backed technical crawling with a custom crawler.

Later comparison should cover:

- DataForSEO OnPage;
- open-source crawling;
- Screaming Frog exports;
- project-owned technical checks;
- Search Console;
- Lighthouse or PageSpeed evidence.

### Marketplace coverage remains a gap

The report is strong for website, SERP, keyword, and LLM visibility but does not finish the broader marketplace-ranking vision.

A later focused addendum should map:

- Amazon;
- Etsy;
- Fiverr;
- merchant and shopping surfaces;
- app, plugin, and software directories;
- other project-specific marketplaces.

## Reject

Reject as doctrine:

- indefinite raw retention is already permitted;
- cross-project reuse and embeddings are legally settled;
- raw provider output may be redistributed or shown to customers without review;
- a recurring secondary-provider subscription program is required;
- a fixed annual provider-calibration budget is approved;
- current competitor pricing or plan details are durable facts;
- one provider's metric is universal truth;
- all DataForSEO endpoint families should be collected;
- marketplace coverage is complete;
- schema design may begin from this report.

## Defer

Defer:

- exact retention periods;
- raw-archive technology;
- provider-rights policy pending clarification;
- secondary-provider selection;
- recurring cadences;
- exact request bodies;
- exact database schema;
- partitioning, indexing, compression, and extensions;
- marketplace provider selection;
- paid LLM capture execution;
- Backlinks subscription and sampling;
- LLM Scraper sampling;
- OnPage implementation choice.

## Ranking-intelligence coverage direction

Kaizen should plan for these evidence families where project value justifies them:

### Search and SERP

- organic rankings;
- result groups and absolute positions;
- featured snippets;
- People Also Ask;
- AI Overviews;
- cited sources;
- related searches;
- discussions, forums, perspectives, video, image, news, shopping, local, and map surfaces;
- location, language, device, and engine dimensions;
- historical movement.

### Keyword and market

- domain keyword discovery;
- related and suggested keywords;
- search volume and history;
- provider-attributed intent and difficulty;
- categories and topics;
- competitor intersections;
- visibility and traffic estimates.

### Authority and technical

- backlinks and referring domains;
- new and lost links;
- anchors and link gaps;
- technical crawl and indexability;
- redirects, canonicals, robots, sitemaps, metadata, structured data, and internal links;
- performance and user-experience evidence where useful.

### AI and LLM visibility

- answer observations;
- brand, entity, domain, product, and recommendation mentions;
- citations and cited URLs;
- provider-attributed share of voice;
- platform, model, language, and geography;
- repeated-run variability;
- citation and mention changes over time;
- retrievability and recommendation evidence.

### Project-owned outcomes

Provider intelligence must be joined analytically with project-owned evidence such as:

- Search Console;
- analytics;
- conversions and revenue;
- experiments;
- releases and content changes;
- technical deployments;
- customer or marketplace outcomes.

## VEDA reuse crosswalk

### Adopt

- observation versus strategy separation;
- schema authority above migrations;
- provider-neutral observations;
- immutable activity and audit trail;
- provider configuration governance;
- paid-data approval;
- supersedence and invalidation;
- no direct database access;
- hammer-test doctrine.

### Adapt

- raw archive posture;
- confidence layer;
- single-Postgres and multi-schema assumptions;
- VEDA versus VEDA Strategy separation;
- provider-specific family designs.

### Research further

- AI surface parent and child families;
- LLM mention cluster placement;
- background retrieval canonicalization;
- OnPage and crawler posture;
- marketplace surfaces.

### Reject

- direct import of VEDA table or field designs;
- VEDA-specific service and system ownership;
- unconditional archive-forever assumptions;
- provider payloads as universal schemas.

## Decision 0010 consequence

The dedicated Internet Marketing Intelligence database direction is ready for narrow owner review.

The boundary may be accepted while retaining explicit gates for:

- provider-rights clarification;
- retention policy;
- database design;
- paid sampling;
- implementation planning.

## Immediate no-spend follow-up

Before paid capture execution:

1. preserve this research and reconciliation;
2. prepare provider-rights clarification questions;
3. review proposed Decision 0010;
4. keep the bounded capture packet deferred;
5. decide whether marketplace coverage needs a focused addendum;
6. hold the owner/steward database-design session only after the boundary decision is reviewed.

## Source limitation

The raw Claude report was supplied as conversation text after Claude read both local repositories through MCP filesystem access and performed current web research. This stewarded summary preserves its load-bearing findings and corrects unsupported or overconfident conclusions.