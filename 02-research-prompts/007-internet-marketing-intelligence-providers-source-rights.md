# Claude Research Prompt 007 - Internet Marketing Intelligence Providers, Source Rights, and Longitudinal Reuse

Status: active research prompt
Date: 2026-06-06

## Prompt

You are conducting Kaizen Research Batch B.

This is a provider, source-rights, workload, retention, and architecture-evidence task for Kaizen's portfolio-wide Internet Marketing Intelligence capability.

This is not a schema-design task.
This is not an implementation task.
This is not permission to purchase provider credits or run paid API calls.

Return a research report only.

Do not edit either repository.
Do not create files in either repository.
Do not stage, commit, push, reset, clean, or discard changes.
Do not install software or plugins.
Do not run live paid provider calls.
Do not purchase DataForSEO credit.
Do not design final tables, columns, indexes, partitions, or migrations.
Do not recommend collecting every endpoint merely because it exists.

## Repositories

Clone or read these repositories in fresh, disposable working directories.

### Kaizen planning repository

```text
https://github.com/St3nch/kaizen-docs
```

### V Ecosystem predecessor evidence repository

Use the existing local repository if available:

```text
C:\dev\v-ecosystem-docs
```

If that path is unavailable, locate and clone the corresponding public repository used by the Kaizen documents and report the exact repository URL and commit.

Start by reporting, for each repository:

```text
git status
git log -5 --oneline
```

Treat both repositories as read-only.

If either repository cannot be accessed, report the exact failure. Continue only when the missing repository is not required for the claim being evaluated; otherwise stop rather than substituting assumptions.

## Central objective

Determine what Kaizen must collect, retain, normalize, compare, and govern to support serious ranking and visibility work across many projects over many years.

DataForSEO is the intended first primary provider.

Kaizen must remain provider-neutral enough to support occasional comparison or calibration against another source such as Ahrefs without redesigning the intelligence system.

The intended provider posture is:

```text
primary provider
= routine and project-triggered observations

secondary comparison provider
= selective validation, calibration, and disagreement analysis
```

Do not assume DataForSEO is sufficient for every required intelligence family.
Do not assume Ahrefs is the correct second provider.
Evaluate alternatives where evidence supports them.

## Funding constraint

The project owner does not expect to have the additional DataForSEO credit needed for missing LLM citation and mention sampling until next month.

Therefore:

- complete all research that does not require new spending;
- use existing real DataForSEO payloads and inventories as empirical evidence;
- identify missing paid samples precisely;
- prepare a bounded, cost-controlled capture packet for later execution;
- do not treat missing paid samples as a reason to postpone rights, provider, workload, or architecture analysis;
- distinguish conclusions that are supportable now from conclusions that require the later paid capture.

The report must not recommend buying credit before the capture packet has exact endpoints, questions, limits, stopping rules, and a hard budget ceiling.

## Required Kaizen reading

Read these files first:

```text
00-entrypoint/LLM_START_HERE.md
ROADMAP.md

01-project-standard/kaizen-vision-and-architecture-alignment.md
01-project-standard/internet-marketing-intelligence-vision.md

03-research-results/004-markdown-qdrant-postgres-architecture-claude-summary.md
03-research-results/005-veda-db-governance-hammer-patterns-claude-summary.md
03-research-results/007-kaizen-vision-architecture-research-gap-audit-claude-summary.md
03-research-results/010-postgres-mcp-typed-tool-audit-claude-summary.md
03-research-results/012-step-5a-composed-tool-capability-map.md

04-design-decisions/0004-system-of-record-boundaries.md
04-design-decisions/0005-api-only-structured-data-access.md
04-design-decisions/0006-hammer-tests-are-a-hard-gate.md
04-design-decisions/0009-operational-postgres-and-observatory-boundary.md
04-design-decisions/0010-dedicated-internet-marketing-intelligence-database.md

05-specs/operational-postgres-authority.md
05-specs/kaizen-hammer-test-strategy.md
```

Important status distinctions:

- Decision 0009 is accepted.
- Decision 0010 is proposed.
- No Internet Marketing Intelligence schema is authorized.
- No final provider is accepted by this research prompt.

## Required VEDA predecessor reading

Use the VEDA repository as predecessor evidence already audited for Kaizen.

Read at minimum:

```text
veda/veda-schema-authority.md
veda/schema-reference.md
veda/observatory-models.md
veda/data-boundaries.md
veda/search-intelligence-layer.md
veda/observability-and-signal-role.md
veda/api-contract-principles.md

veda-strategy/schema-authority.md
veda-strategy/data-boundaries.md

ecosystem/ecosystem-schema-spine.md
ecosystem/db-posture.md
ecosystem/external-provider-integration-doctrine.md
ecosystem/decisions/ADR-001-veda-is-pure-observatory.md
ecosystem/decisions/ADR-009-no-direct-database-access.md
ecosystem/decisions/ADR-012-single-postgres-multi-schema.md

audit/full-audit/pass-3-schema-and-implementation-readiness.md
audit/full-audit/schema-authority-wave-checkpoint.md

transition-steward/batch-h-veda-family-design-pass.md
transition-steward/batch-h-veda-family-design-pass-corrections.md
transition-steward/batch-h-dataforseo-surface-inventory.md
transition-steward/batch-h-dataforseo-confidence-layer.md
transition-steward/dataforseo-research-tracker.md
transition-steward/dataforseo-capture-plan.md
transition-steward/dataforseo-json/README.md
```

Read every JSON inventory note in:

```text
transition-steward/dataforseo-json/
```

Inspect the paired JSON payloads when needed to verify structure or inventory claims.

Do not treat VEDA doctrine as Kaizen doctrine.
Classify predecessor patterns as:

```text
adopt
adapt
research further
defer
reject
```

## Existing empirical corpus

The VEDA predecessor repository contains real DataForSEO payloads and paired inventory notes for endpoint families including:

- Google Organic Advanced SERP;
- Google Organic Advanced with AI Overview;
- ChatGPT LLM Responses;
- Keywords For Site;
- Keyword Overview;
- Ranked Keywords;
- Historical Rank Overview;
- Search Intent;
- Keywords For Categories;
- controlled comparison samples where present.

Verify the actual current corpus rather than relying only on this list.

Important known distinction:

```text
ChatGPT LLM Responses
!=
LLM Mentions Search
```

Do not collapse them into one endpoint family or one evidence claim.

## Accepted Kaizen constraints

Treat these as governing unless a direct contradiction is found:

- Markdown and Obsidian own canonical human-readable project intelligence.
- Operational Postgres owns governed structured operational records.
- The Observatory is a bounded intelligence domain, not the name of all Postgres state.
- The proposed Intelligence database is portfolio-wide, longitudinal, and broader than SERP data.
- Qdrant is derivative and owns no unique truth.
- Agents use typed, scoped, logged tools and receive no arbitrary SQL or direct credentials.
- Raw evidence, normalized observations, derived signals, strategy candidates, experiments, and accepted decisions remain distinct layers.
- A provider payload does not become a universal schema merely because a field appears in one response.
- Provider-assigned metrics and classifications remain attributed to that provider.
- Shared lawful observations may support many projects without being duplicated as project-owned truth.
- Project-private, customer-restricted, licensed, and confidential data require separate governance.
- Opportunity and strategy outputs remain reviewable candidates until humans accept them.
- Paid data pulls require explicit scope, purpose, provider, budget ceiling, and authorization.
- No schema work begins from this research alone.

## Research standards

Use current primary sources wherever possible:

- official DataForSEO documentation, pricing, terms, API references, changelogs, and support material;
- official provider documentation for Ahrefs and other comparison candidates;
- official search-platform documentation where relevant;
- official licensing, API, retention, redistribution, and acceptable-use terms;
- actual provider payloads and source repositories;
- provider status pages and release notes;
- peer-reviewed or standards-based evidence for measurement limitations where applicable.

Use secondary sources only to identify questions or real-world failure modes.
Do not let reseller pages, affiliate reviews, generic SEO blogs, or vendor marketing claims determine architectural conclusions.

Record the date checked for every provider and terms source.

## Required analysis

### 1. Ranking-intelligence requirements inventory

Derive what Kaizen needs to support evidence-based ranking work for websites and other project types.

At minimum evaluate:

#### Search and SERP intelligence

- organic rankings;
- rank groups and absolute positions;
- SERP feature composition;
- featured snippets;
- People Also Ask;
- AI Overviews;
- related searches;
- perspectives, discussions, forums, video, image, news, shopping, local, map, and other relevant surfaces;
- location, language, device, operating system, and search-engine variants;
- current versus historical ranking observations;
- page and domain visibility;
- result ownership and citation/source identity.

#### Keyword and market intelligence

- keyword discovery;
- related keywords and suggestions;
- search volume and monthly history;
- search intent and intent probabilities;
- competition or difficulty estimates;
- categories, topics, clusters, and query expansion;
- keywords for a site;
- ranked keywords;
- domain and competitor intersections;
- estimated traffic or visibility;
- historical keyword and domain metrics.

#### Technical and on-page intelligence

- crawlability and indexability;
- status codes, redirects, canonicals, robots, and sitemaps;
- internal linking;
- metadata and structured data;
- duplicate or thin content signals;
- page speed or user-experience inputs where available;
- content-analysis and relevance surfaces;
- technologies and site characteristics.

#### Link and authority intelligence

- backlinks and referring domains;
- new and lost links;
- anchors;
- domain and page authority proxies;
- link intersections and gaps;
- historical link movement;
- provider-specific quality or spam metrics.

#### AI and LLM visibility

- LLM answers and response structure;
- brand, entity, product, and domain mentions;
- citations and cited URLs;
- source and competitor share of voice;
- prompt/query families;
- model, platform, geography, and language;
- repeated-run variability;
- citation and mention changes over time;
- AI search volume or impression-like metrics where genuinely supported;
- retrievability or recommendation evidence.

#### Project-owned outcome data

Identify data that should come from project-owned systems rather than a provider, such as:

- Search Console;
- analytics;
- conversions and revenue;
- experiments;
- releases and content changes;
- technical deployments;
- customer or marketplace outcomes.

For every requirement, state whether it is:

```text
required for most projects
required only for certain project classes
useful but optional
poor value
not provider-appropriate
```

### 2. DataForSEO endpoint coverage map

Map ranking-intelligence requirements to current DataForSEO products and endpoints.

For each relevant endpoint family, record:

- official product and endpoint name;
- purpose;
- important request dimensions;
- response family and major nested structures;
- current access requirements;
- pricing or cost drivers;
- rate or job constraints;
- existing empirical sample in the VEDA corpus;
- sample quality and structural coverage;
- whether another sample is needed;
- whether the family belongs in routine, periodic, investigation-only, or deferred collection;
- rights and retention questions;
- likely value for Kaizen projects.

Include, where currently offered and relevant:

- SERP APIs;
- DataForSEO Labs;
- AI Optimization and LLM visibility products;
- Backlinks;
- OnPage;
- Content Analysis;
- Keywords Data;
- Business Data;
- Merchant or product surfaces;
- App Data;
- Domain Analytics or technology surfaces;
- other material current families.

Do not treat all endpoint families as equally valuable.

### 3. Existing-sample versus missing-sample inventory

Produce a matrix of:

```text
endpoint family
existing JSON sample
paired inventory
comparison coverage
structural confidence
missing question
additional paid sample required?
priority
```

Separate:

- no additional sample needed;
- useful cheap comparison later;
- paid LLM sample required next month;
- access confirmation required;
- low-value or unnecessary sample;
- another provider or project-owned source required.

### 4. LLM mentions and citations completion plan

Research DataForSEO's current LLM and AI-search products precisely.

Verify:

- current product and endpoint names;
- supported models or platforms;
- mention versus citation semantics;
- query/prompt inputs;
- location and language support;
- response structure;
- repeated-run behavior;
- access requirements;
- current minimum credit or account requirement;
- per-call and cost-multiplier behavior;
- raw-response and retention terms;
- differences from the existing ChatGPT LLM Responses sample;
- current product maturity and documented limitations.

The owner reports that a former $200 credit requirement has been lowered to $100. Verify the current requirement from primary sources and report uncertainty if the rule is account-specific or not publicly documented.

Prepare a no-execution capture packet for next month containing:

- exact endpoint list;
- exact research question per call;
- prompt/query set;
- entity, brand, domain, or project contexts;
- model/platform variants;
- geography and language;
- number of repeated runs and rationale;
- maximum calls;
- estimated cost by endpoint and total;
- hard budget ceiling;
- stopping conditions;
- duplicate-call prevention;
- required filenames;
- required paired inventory notes;
- quality checks;
- what decisions each sample will unblock.

Do not propose spending the full $100 merely because it is available.

### 5. Collection-tier design

Define evidence-based collection tiers without designing schedules or schemas.

At minimum:

#### Baseline collection

For project onboarding, market entry, or major strategy reset.

#### Periodic monitoring

For selected high-value keywords, competitors, links, LLM visibility, and technical regression signals.

#### Investigation pulls

For ranking drops, algorithm changes, new competitors, emerging query clusters, citation shifts, or experiment review.

#### Calibration pulls

For occasional comparison with a secondary provider.

For each tier, identify:

- candidate data families;
- likely cadence ranges;
- triggers;
- cost controls;
- retention value;
- project versus shared ownership;
- stopping conditions.

The owner expects this collection to be selective rather than constant. Design for high information value per pull.

### 6. Provider rights, retention, and reuse

Research what Kaizen may lawfully and contractually do with each provider's data.

At minimum evaluate:

- raw-response retention;
- normalized fact retention;
- derived metrics and aggregations;
- cross-project reuse;
- portfolio-wide analysis;
- internal model-assisted analysis;
- embeddings;
- redistribution;
- customer reporting;
- public display;
- archival duration;
- account termination effects;
- required attribution;
- deletion obligations;
- caching limits;
- terms that differ by endpoint family or upstream source.

Distinguish:

```text
explicitly permitted
explicitly restricted
unclear / counsel or provider clarification required
inferred but not safe to rely on
```

Do not carry VEDA's "archive always" language into Kaizen without verifying provider rights and storage cost.

### 7. Raw archive strategy requirements

Without selecting technology, determine which payload classes likely need:

- full raw retention;
- time-limited raw retention;
- hash and metadata only;
- normalized observations only;
- deletion after processing;
- separate private or licensed storage;
- reprocessing capability.

Consider:

- structural volatility;
- cost of re-pulling;
- provider rights;
- payload size;
- copyright or personal-data content;
- auditability;
- normalization bugs;
- future schema evolution.

### 8. Provider-neutral observation requirements

Define conceptual requirements, not tables.

The system must preserve:

- provider identity;
- product and endpoint identity;
- request and provider task identity;
- capture time;
- location, language, device, engine, platform, model, or marketplace dimensions;
- raw artifact reference and hash where retained;
- cost and usage;
- normalization version;
- provider-specific metric names and methodology notes;
- freshness and quality;
- project that requested collection;
- all projects later related to the observation;
- privacy and rights class;
- correction or invalidation state.

Determine which observation families can be normalized across providers and which must remain provider-specific.

### 9. Multi-provider comparison strategy

Evaluate Ahrefs and at least two other credible candidates where relevant.

Possible comparison families include:

- keywords and search volume;
- rankings and visibility;
- backlinks and referring domains;
- competitor discovery;
- content or technical-site intelligence;
- AI/LLM visibility.

For each candidate, assess:

- coverage;
- API access;
- pricing and minimum commitments;
- retention and reuse rights;
- methodology transparency;
- export limits;
- historical data;
- strengths relative to DataForSEO;
- whether occasional calibration is commercially practical.

Recommend a selective comparison posture, not permanent duplicate collection.

The system must preserve disagreement rather than averaging providers into false certainty.

Example conceptual output:

```text
metric family: monthly search volume
DataForSEO value: X
secondary provider value: Y
capture dates: different
methodologies: provider-specific
comparison status: disagreement retained
```

### 10. Confidence and disagreement model

Use the VEDA confidence-layer work as predecessor evidence.

Determine how Kaizen should distinguish:

- provider-reported fact;
- directly observed result;
- normalized measurement;
- inferred entity match;
- derived trend;
- cross-provider agreement;
- cross-provider disagreement;
- insufficient sample;
- stale evidence;
- corrected or invalidated observation.

Do not design numeric confidence scores unless evidence supports them.

### 11. Shared observations and project relationships

Determine how a portfolio-wide system should reason about:

```text
observation collected because of Project A
also relevant to Project B
later used in Project C experiment
```

Cover:

- request provenance;
- project relevance;
- shared public observations;
- project-private observations;
- customer-restricted observations;
- cost attribution;
- duplicate-call avoidance;
- reuse decisions;
- experiment and outcome linkage.

### 12. Cost governance

Define planning requirements for:

- pre-call estimation;
- approved budget ceiling;
- cost by provider, endpoint, project, request, and purpose;
- shared-observation cost attribution;
- duplicate detection;
- expensive flags and multipliers;
- retry cost;
- cancellation and failure;
- budget alerts;
- monthly and project-level caps;
- sample versus production collection.

Use the VEDA controlled-sampling rules as predecessor evidence and improve them where needed.

### 13. Data quality and provider drift

Research how Kaizen should detect:

- endpoint-version changes;
- new or missing fields;
- enum expansion;
- nested-type variation;
- silent default changes;
- pricing changes;
- access changes;
- delayed or partial results;
- provider methodology changes;
- disagreement with project-owned outcomes;
- normalization regressions.

Recommend contract tests and sample-based drift checks without implementing them.

### 14. Decision 0010 evidence

Assess whether current evidence supports the proposed dedicated Internet Marketing Intelligence database boundary.

Classify:

```text
accept direction
accept with modifications
keep proposed pending paid samples
reject
```

Do not design the schema.

Identify exactly what must be decided in the later owner/steward database-design session.

### 15. VEDA reuse crosswalk

Produce a concise crosswalk of VEDA predecessor patterns:

```text
pattern
Kaizen disposition
reason
required modification
future destination
```

At minimum include:

- observation versus strategy separation;
- schema authority above migrations;
- provider-neutral normalized observations;
- raw archive posture;
- event/activity trail;
- confidence layer;
- provider configuration;
- cost approval;
- supersedence and invalidation;
- no direct database access;
- hammer-test doctrine;
- single-Postgres/multi-schema assumptions;
- VEDA versus VEDA Strategy separation.

## Required output

Return one report with these sections.

### 1. Executive verdict

Answer directly:

- Is DataForSEO a suitable primary provider?
- What does it cover well?
- What does it not cover well?
- What must be collected to support ranking projects?
- Which missing samples actually require next month's paid credit?
- What should Kaizen avoid collecting?
- Is occasional secondary-provider comparison worthwhile?
- Does Decision 0010 remain justified?

### 2. Repository and Git state observed

Include both repositories, branches, latest commits, clean/dirty status, and no-write confirmation.

### 3. Ranking-intelligence requirements matrix

Use columns:

```text
intelligence need
project value
project classes
required/optional
preferred source
collection tier
notes
```

### 4. DataForSEO endpoint coverage matrix

Use columns:

```text
product / endpoint
intelligence family
existing sample
sample confidence
access requirement
cost drivers
collection tier
rights questions
Kaizen disposition
```

### 5. Existing versus missing sample inventory

Identify exactly what already exists and what remains missing.

### 6. LLM mention and citation findings

Separate current verified facts from assumptions and missing paid evidence.

### 7. Next-month paid capture packet

Provide an exact bounded plan with estimated cost, hard ceiling, call count, stopping conditions, filenames, and inventory-note requirements.

### 8. Collection-tier recommendation

Cover baseline, periodic, investigation, and calibration pulls.

### 9. Rights and retention matrix

Use columns:

```text
provider / source
raw retention
normalized retention
derived use
cross-project reuse
customer reporting
public display
embeddings / model analysis
confidence
source
```

### 10. Provider-neutral observation requirements

Conceptual requirements only. No tables or migrations.

### 11. Secondary-provider comparison

Compare Ahrefs and at least two alternatives. Recommend when comparison is worth its cost.

### 12. Confidence and disagreement recommendation

Explain how Kaizen preserves provider attribution and disagreement.

### 13. Shared-observation and cost-attribution recommendation

Explain cross-project reuse without duplicating truth.

### 14. Raw archive recommendation

Classify payload families by full retention, limited retention, metadata-only, normalized-only, or unresolved.

### 15. VEDA reuse crosswalk

Use adopt, adapt, research further, defer, and reject.

### 16. Decision 0010 recommendation

State whether to accept, modify, defer, or reject the dedicated Intelligence database direction.

### 17. Database-design-session inputs

List the exact questions and workload evidence the owner and steward must review later.

Do not provide a schema.

### 18. Prototype and hammer-test inputs

Define later tests for provider adapters, drift, cost controls, project scope, duplicate avoidance, rights classes, and cross-provider comparisons.

Do not run them.

### 19. Files likely needing later revision

List exact Kaizen paths and likely changes. Do not edit them.

### 20. Ordered next actions

Provide a dependency-aware sequence that distinguishes work possible now from next month's paid capture.

### 21. Final no-write and no-spend confirmation

Confirm:

- no repository files changed;
- no files created;
- no software installed;
- no paid provider calls run;
- no provider credit purchased;
- no commits or pushes;
- no schema designed;
- no prototypes run.

## Important constraints

Do not:

- redo the already completed generic VEDA governance audit;
- ignore the existing real JSON corpus;
- treat provider documentation as proof of actual payload shape when a real sample exists;
- treat one sample as a universal schema contract;
- collapse LLM Responses, LLM Mentions, AI Overviews, citations, recommendations, and retrievability into one family;
- turn provider metrics into provider-neutral facts without attribution;
- average provider disagreement into false certainty;
- design a DataForSEO-shaped database;
- recommend constant or indiscriminate collection;
- recommend buying credit before the bounded capture packet exists;
- assume raw payloads may be retained forever;
- put canonical strategy or project narrative in the Intelligence database;
- expose raw SQL or direct provider credentials to agents;
- begin implementation;
- block all research on next month's paid LLM sample;
- derail the current Kaizen planning roadmap.

The central question is:

> What provider coverage, rights, retention, sampling, and provider-neutral intelligence requirements must Kaizen establish now so that it can rank many kinds of projects over years, finish the missing DataForSEO LLM sampling next month without waste, and later compare selected observations against Ahrefs or another provider without redesigning the system?

Return the report only.
