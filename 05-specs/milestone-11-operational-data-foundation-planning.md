---
id: kz-spec-01KV6L1Q8M2N7R5C3Y9P4T6BA0
type: specification
status: accepted
project: kaizen-platform
created: 2026-06-15T23:15:00Z
updated: 2026-06-15T23:50:00Z
review_status: approved
authority: accepted
accepted_by: owner.local
accepted_source_sha256: f798c16f56721670381b77e27f8dd548985e4c4f3d19fece3b157319038e8f82
summary: "Define Milestone 11 planning for the smallest evidence-backed operational data foundation without authorizing implementation."
---

# Milestone 11 - Operational Data Foundation Planning

## Purpose

Turn the accepted Milestone 10 authority and lifecycle reconciliation into a bounded, implementation-ready design program for Kaizen's first operational data foundation.

Milestone 11 begins with planning only. Physical design and implementation remain separate owner gates.

## Governing authority

```text
04-design-decisions/0004-system-of-record-boundaries.md
04-design-decisions/0005-api-only-structured-data-access.md
04-design-decisions/0006-hammer-tests-are-a-hard-gate.md
04-design-decisions/0009-operational-postgres-and-observatory-boundary.md
04-design-decisions/0019-milestone-10-system-of-record-reconciliation.md
03-research-results/247-milestone-10-candidate-dossiers.md
03-research-results/248-milestone-10-authority-and-lifecycle-reconciliation.md
03-research-results/249-milestone-10-reconciliation-audit.md
03-research-results/250-packet-015a-and-milestone-10-owner-closure-acceptance.md
05-specs/operational-postgres-authority.md
ROADMAP_V0.3.md
```

## Accepted conceptual families

Milestone 11 may plan only:

1. execution attempt;
2. verification run;
3. return-bundle manifest;
4. artifact provenance;
5. governed-operation read model;
6. connector invocation telemetry.

No Observatory, ingestion, customer-data, claims, Qdrant, or production-MCP family may be added without new evidence and owner approval.

## Recommended first implementation slice

The first physical and implementation slice should contain only the core implementation-evidence chain:

```text
execution attempt
-> verification run
-> return-bundle manifest
-> artifact provenance
```

Reasons:

- all four were exercised repeatedly by Northstar;
- they share direct implementation-return provenance;
- they solve the most expensive file-aggregation pain;
- their authority boundaries are already clear;
- they can be proved locally without touching canonical Markdown or governance authority;
- they provide the operational substrate needed before adding wider tool telemetry.

The governed-operation read model and connector telemetry should remain later Milestone 11 slices unless planning evidence proves they are required for the minimum viable foundation.

## Milestone structure

### Phase 11A - Planning and owner decisions

Produce and accept:

- exact first-slice selection;
- retention and deletion policy;
- privacy and path-reference policy;
- stable-ID strategy;
- physical database and schema authority;
- migration ownership and versioning rules;
- transaction and idempotency model;
- backup, restore, and disaster-recovery requirements;
- typed service boundary;
- hammer-test and proving-ground requirements;
- implementation packet sequence.

No database creation or SQL is authorized in Phase 11A.

### Phase 11B - Physical design

May begin only after separate owner acceptance of Phase 11A.

Expected design outputs may include:

- minimum Postgres version and local deployment posture;
- physical schemas, tables, columns, keys, constraints, and indexes;
- migration layout and ownership;
- roles and least-privilege access;
- service contracts;
- backup and restore design;
- test and hammer specifications.

Phase 11B still does not authorize implementation unless its packet is separately approved.

### Phase 11C - Local proving-ground implementation

May begin only after owner-approved physical design and implementation packet.

It must prove:

- migrations from empty state;
- upgrade and rollback/fail-forward behavior;
- transaction and illegal-state-transition enforcement;
- idempotency and retry behavior;
- project isolation;
- artifact-byte/file authority preservation;
- backup and restore;
- concurrency and hammer tests;
- typed access with no direct agent SQL;
- deterministic implementation return and governed completion.

## Owner decisions required in Phase 11A

### 1. Retention classes

Recommended tiered policy:

```text
Class A - permanent authority-linked lineage
frozen return manifests, rejected return history, provenance referenced by accepted audits or owner decisions

Class B - project-lifetime operational lineage
execution attempts and verification runs linked to active or retained projects

Class C - bounded diagnostic telemetry
connector invocation events and unreferenced transient diagnostics

Class D - rebuildable state
governed-operation read model
```

Exact durations and deletion behavior require owner acceptance.

### 2. Connector telemetry posture

Recommended planning default:

- retain all blocked, rejected, timed-out, and failed attempts;
- retain all consequential mutation attempts;
- permit later sampling or aggregation of successful low-risk read calls;
- store no prompt bodies, arbitrary arguments, file contents, secrets, or customer payloads.

### 3. Retry-group expiry

Recommended planning default:

```text
30 days after the most recent invocation, unless linked to an unresolved governed operation or active incident
```

### 4. Project-deletion semantics

Recommended planning default:

- remove ordinary project-scoped operational data only through a governed deletion process;
- retain canonical decisions, governance events, accepted/rejected implementation lineage, and authority-linked provenance;
- replace deleted project data with a tombstone or retained project identity where needed for provenance;
- never cascade-delete accepted audit evidence merely because a project is archived.

### 5. Path-reference policy

Recommended planning default:

- store scoped storage identifiers and repository-relative paths;
- avoid absolute local paths as durable operational identity;
- permit absolute paths only in ephemeral local diagnostics;
- resolve storage identifiers through typed services.

### 6. First implementation slice

Recommended selection:

```text
execution attempt
verification run
return-bundle manifest
artifact provenance
```

Recommended deferral:

```text
governed-operation read model
connector invocation telemetry
```

## Stable-ID requirements

Phase 11A must choose one governed strategy that preserves:

- globally unique external IDs;
- stable project and packet references;
- distinct logical artifact and immutable byte-object identity;
- new IDs for retries and reruns;
- predecessor/supersedence linkage;
- no identity derived only from mutable paths;
- compatibility with Markdown IDs and existing Kaizen ULID conventions.

The planning recommendation is typed prefixed ULIDs at service boundaries, with database-native internal keys permitted only if never exposed as canonical external identity.

## Transaction and correction requirements

1. Execution state transitions reject illegal transitions.
2. Verification timeout or connector block must not become a failed test result.
3. Freezing a return manifest and its file inventory is atomic.
4. Frozen manifests are immutable; corrections create new versions.
5. Provenance assertions never overwrite prior assertions.
6. Artifact bytes remain outside Postgres.
7. Accepted audit or owner authority remains Markdown.
8. State mutation and required audit events commit atomically where they share one boundary.
9. Cross-project reads fail closed.
10. Unknown recovery states remain unresolved rather than guessed.

## Typed service boundary

No agent or MCP tool receives direct SQL or database credentials.

The first typed service must expose only bounded capabilities such as:

```text
start execution attempt
record terminal execution outcome
record verification run
freeze return-bundle manifest
record or verify artifact provenance
read one attempt and its evidence graph
list attempts by bounded project and packet filters
```

Exact API or MCP tools remain Phase 11B design work.

## Backup and recovery planning requirements

Before implementation approval, define:

- local database backup object;
- encryption and storage posture;
- backup frequency and retention;
- restore to disposable root;
- schema migration plus data restore ordering;
- evidence-file and database consistency checks;
- recovery point and recovery time targets;
- handling of database restored without corresponding artifact files;
- handling of artifact files restored without matching provenance records.

## Hammer-test requirements

The physical design must include tests for:

- concurrent attempt creation;
- duplicate retry submissions;
- illegal lifecycle transitions;
- duplicate verification and provenance observations;
- manifest freeze races;
- cross-project access attempts;
- migration interruption and restart;
- backup/restore consistency;
- service crash between database commit and file-evidence freeze;
- stale or missing artifact references;
- read-model rebuild if later included;
- connector-event deduplication if later included.

## Required Phase 11A outputs

1. owner-decision proposal covering the six required choices;
2. first-slice architecture and exclusion statement;
3. conceptual service and trust-boundary design;
4. retention, privacy, deletion, and path-reference policy;
5. stable-ID and relationship specification;
6. transaction, idempotency, correction, and recovery specification;
7. backup/restore and hammer-test design requirements;
8. phased Packet 016 series;
9. independent readiness audit;
10. explicit physical-design owner gate.

## Post-Milestone-11 external audit

After Milestone 11 is fully implemented and closed, run an independent Claude audit that:

- begins with the conclusions of the June 10 post-Milestone-6 roadmap audit;
- audits Milestones 8 through 11 against those recommendations;
- reads the full current Kaizen vision, standard, roadmap, accepted decisions, system boundaries, major research, and live repository state;
- identifies what Kaizen has actually proved versus what remains conceptual;
- recommends the next milestone sequence after 11;
- evaluates whether a real-project pilot or controlled research/report pilot should precede typed operational APIs or production MCP work;
- does not promote recommendations directly into doctrine.

A draft prompt is maintained at:

```text
02-research-prompts/009-post-milestone-11-full-project-roadmap-audit.md
```

## Explicit exclusions

Phase 11A does not authorize:

- Postgres installation, database creation, schemas, SQL, migrations, roles, or credentials;
- source-code implementation;
- services, APIs, MCP tools, queues, or workers;
- production deployment;
- canonical-vault mutation except later governed status updates;
- Qdrant, Observatory, ingestion, IMI, customer-data, or multi-user identity implementation;
- Milestone 12 definition before the post-Milestone-11 audit and owner review.

## Completion condition

Milestone 11 planning is complete when Phase 11A artifacts are audited and explicitly accepted by the owner. Physical design and implementation remain separately gated.