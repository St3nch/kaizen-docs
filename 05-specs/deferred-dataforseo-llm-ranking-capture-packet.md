# Deferred DataForSEO LLM and Ranking Intelligence Capture Packet

Status: deferred planning artifact; execution not authorized
Date: 2026-06-06
Evidence:
- `03-research-results/015-internet-marketing-intelligence-providers-source-rights-claude-summary.md`
- `02-research-prompts/007-internet-marketing-intelligence-providers-source-rights.md`
- VEDA predecessor DataForSEO corpus and inventories

## Purpose

Define a controlled future capture process for missing DataForSEO LLM and ranking-intelligence payloads without authorizing spending or freezing provider request schemas.

## Execution gate

No paid call may run until all of these are true:

- the owner has funds available;
- current account access and funding requirements are verified directly in the account or with provider support;
- the steward approves the capture window;
- exact endpoint documentation is rechecked;
- exact request bodies are generated and reviewed;
- provider-consumption ceilings are recorded;
- unique output filenames are confirmed absent;
- no customer-private data is placed in the shared corpus.

## Budget model

Keep these separate:

```text
account funding requirement
!=
authorized API consumption
```

Initial planning limits:

- account funding: whatever current provider access requires, verified immediately before purchase;
- authorized API consumption for the first structural capture: maximum $10;
- emergency stop: $15 cumulative provider-reported cost;
- unused account balance: not authorization to spend.

The owner or steward may lower these ceilings before execution.

## Capture objectives

### Required first

1. LLM Mentions Search
   - one Google AI Overview or equivalent supported-platform control query;
   - one ChatGPT or equivalent supported-platform control query;
   - validate actual mention-cluster structure, citations, sources, search results, volume-like metrics, time-window fields, and entity fields.

2. LLM Mentions Aggregated Metrics
   - one supported-platform sample;
   - validate grouped metrics, volume-like fields, and target behavior.

3. Multi-platform LLM Responses
   - one non-ChatGPT supported platform;
   - validate whether the existing LLM Responses family generalizes across platforms.

### Optional only if budget and evidence justify

- cross-aggregated metrics;
- top domains;
- top pages;
- LLM Scraper;
- a search-forcing ChatGPT LLM Responses comparison;
- a narrower Keywords For Categories comparison;
- a second Search Intent comparison.

### Explicitly deferred

- Backlinks subscription captures;
- AI Mode;
- rectangle-enabled SERPs;
- deep PAA expansion;
- bulk keyword harvesting;
- OnPage production evaluation;
- marketplace APIs;
- constant monitoring.

## Query set

The packet should use three roles where supported:

```text
control query
= continuity with the predecessor corpus

Kaizen-project query
= an approved current project or public market question

comparison query
= target entity versus known competitors
```

At least one control query and one Kaizen-project query should be included.

Do not include customer-private or restricted information in the shared sample corpus.

## Execution-time request generation

Immediately before execution:

- confirm current endpoint names;
- confirm current platform and model values;
- confirm location and language support;
- confirm maximum rows and pagination behavior;
- confirm provider cost formula;
- generate exact request JSON;
- validate the request against provider documentation;
- estimate maximum cost;
- record the approved request checksum or exact reviewed body.

Do not copy old request bodies without revalidation.

## Required file naming

Use provider, product, platform, endpoint, target, and variant in each filename.

Example pattern:

```text
dataforseo-ai-optimization-{platform}-{endpoint}-{target-slug}-{variant}.json
```

Every JSON payload requires a paired inventory note:

```text
{same-stem}-inventory.md
```

## Required paired inventory

Each inventory note should record:

- research question;
- request profile;
- provider endpoint and version;
- actual versus requested platform/model behavior;
- top-level structure;
- observed nested item families;
- provider cost;
- rights and privacy class;
- raw-retention recommendation;
- provider-neutral versus provider-specific fields;
- differences from current documentation;
- decisions unblocked;
- unresolved questions.

## Stop conditions

Stop the capture window immediately when any of these occurs:

- provider-reported cumulative cost reaches $15;
- an individual call exceeds its approved estimate or ceiling;
- two consecutive empty or structurally unusable results occur for one endpoint;
- repeated provider errors indicate access or coverage problems;
- saved payload does not match the reviewed endpoint or target;
- duplicate filename or duplicate dimensional context is detected;
- output contains credentials or sensitive account data;
- current terms or access conditions differ materially from the reviewed plan;
- the steward or owner withdraws authorization.

## Duplicate prevention

Before every call:

- check the output filename does not exist;
- check the dimensional observation is not already present and fresh enough;
- check no equivalent task is pending;
- record request purpose and requesting project;
- use a request id or idempotency reference where the provider supports it.

## Quality checks

For every saved response:

- verify successful provider status;
- verify endpoint and function identity;
- verify platform/model/location/language values;
- verify provider task identity;
- verify provider-reported cost;
- compute and record a raw artifact hash;
- sanitize credentials or account-sensitive fields;
- create the paired inventory before the next endpoint family is called.

## Rights and retention gate

Until provider rights are clarified:

- keep raw captures internal;
- do not publish or redistribute payloads;
- do not expose long answer text or snippets in customer-facing output;
- mark retention as provisional;
- preserve hash and provenance;
- review retention before any purge or permanent archive policy is adopted.

## Completion criteria

The capture packet is complete only when:

- required endpoint families have at least one structurally useful payload;
- each payload has a paired inventory;
- actual cost is reconciled;
- documentation-versus-payload differences are recorded;
- no authorization ceiling was exceeded;
- the steward has enough evidence to update the provider family model;
- the capture itself has not silently become production collection.