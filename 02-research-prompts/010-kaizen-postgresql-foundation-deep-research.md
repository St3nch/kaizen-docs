# Claude Opus Deep-Research Prompt — Kaizen PostgreSQL Foundation and Full Database Architecture

## Model and research mode

Use **Claude Opus**, not Sonnet, for the primary research and architecture report.

This assignment requires long-context repository analysis, technical comparison, adversarial review, and forward architecture reasoning. Sonnet may later be used for formatting or bounded follow-up work, but the main report should be produced by the strongest generally available Opus model in Claude at the time the research is run.

Use deep research / web research where available.

---

# 1. Role

You are conducting an independent, evidence-first technical architecture audit for a governed project called **Kaizen**.

Your job is not to endorse PostgreSQL, repeat common best practices, or decorate an already chosen design.

Your job is to determine the **best PostgreSQL foundation and long-term database posture for Kaizen as it actually exists now**, after Milestones 1–10 and the planning/design portion of Milestone 11.

You must:

- understand Kaizen before recommending database architecture;
- inspect the accepted workload, authority, lifecycle, recovery, and physical-design records;
- verify current PostgreSQL capabilities and support status from primary sources;
- identify mistakes, gaps, unnecessary complexity, missing capabilities, and future traps;
- recommend the smallest safe first implementation and the best long-term database topology;
- state exactly what should change, if anything, before Packet 016C implementation begins.

Do not assume the existing Milestone 11 design is correct merely because it was accepted for planning.

---

# 2. Repository and checkpoint

Clone the Kaizen documentation repository:

```text
https://github.com/St3nch/kaizen-docs.git
```

Required baseline checkpoint:

```text
branch:
main

minimum required ancestor commit:
c358dc55c655208550c6a485ba696102ca488655

commit message:
Prepare Packet 016C implementation readiness
```

Procedure:

1. Clone the repository.
2. Record the actual checked-out branch and HEAD.
3. Verify that the actual HEAD is either exactly the baseline commit or a descendant of it.
4. If the repository has moved forward, use the latest checked-out `main` state but preserve the baseline commit as the minimum context checkpoint.
5. Report any mismatch, missing commit, inaccessible repository, or dirty clone before analysis.
6. Do not modify or push the repository.

This is a public planning and doctrine repository. It does not contain every local implementation repository or private evidence object. Clearly distinguish what you can verify directly from what is referenced but unavailable.

---

# 3. Kaizen context to understand first

Kaizen is a governed project-intelligence and implementation-return system.

Its broad workflow is:

```text
idea
-> research
-> source summaries
-> claims and constraints
-> decisions
-> specifications
-> audits
-> task packets
-> implementation
-> tests
-> implementation return
-> governed canonical update
```

Kaizen is intended to become a model-agnostic evidence and provenance engine for project work, not merely a project-management database.

Current and future use includes:

- governed software-development work;
- implementation attempts, tests, frozen returns, and artifact provenance;
- research and report production;
- marketplace and service delivery;
- future Context and Tool Gateway capabilities;
- raw evidence -> normalized evidence -> LLM context views;
- later Observatory and Internet Marketing Intelligence workloads;
- future DataForSEO and external-provider integrations;
- future typed local services and controlled MCP exposure;
- possible later Qdrant retrieval when justified;
- human authority and fail-closed consequential controls.

Kaizen currently preserves different authorities:

```text
Markdown:
decisions, specifications, audits, task packets, owner acceptance, project intelligence

immutable files:
artifacts, frozen reports, implementation-return evidence, raw evidence bytes

Git:
repository commits and tracked state

canonical governance JSONL:
governed operation event history

immutable staging evidence:
plans, approvals, preparations, operation evidence

future operational Postgres:
only explicitly accepted structured operational state
```

A key design requirement is to prevent accidental dual authority.

---

# 4. Required repository reading

Read the current versions of these files completely before reaching conclusions:

```text
00-entrypoint/LLM_START_HERE.md
README.md
ROADMAP_V0.3.md
01-project-standard/kaizen-project-standard-v0.2.md
01-project-standard/kaizen-vision-and-architecture-alignment.md
```

Read these authority and database-boundary records:

```text
04-design-decisions/0004-system-of-record-boundaries.md
04-design-decisions/0005-api-only-structured-data-access.md
04-design-decisions/0006-hammer-tests-are-a-hard-gate.md
04-design-decisions/0009-operational-postgres-and-observatory-boundary.md
04-design-decisions/0019-milestone-10-system-of-record-reconciliation.md
04-design-decisions/0020-milestone-11-operational-foundation-boundaries.md
05-specs/operational-postgres-authority.md
```

Read the Milestone 10 evidence:

```text
03-research-results/247-milestone-10-candidate-dossiers.md
03-research-results/248-milestone-10-authority-and-lifecycle-reconciliation.md
03-research-results/249-milestone-10-reconciliation-audit.md
03-research-results/250-packet-015a-and-milestone-10-owner-closure-acceptance.md
```

Read the complete Milestone 11 planning and physical-design package:

```text
05-specs/milestone-11-operational-data-foundation-planning.md
03-research-results/255-milestone-11-owner-decision-options.md
03-research-results/256-milestone-11-first-slice-and-trust-boundary.md
03-research-results/257-milestone-11-phase-11a-readiness-audit.md
03-research-results/258-milestone-11-phase-11a-owner-acceptance.md
05-specs/milestone-11-identity-lifecycle-recovery-contract.md
05-specs/milestone-11-minimum-postgres-physical-design.md
05-specs/milestone-11-typed-service-contract.md
05-specs/milestone-11-backup-restore-retention-design.md
05-specs/milestone-11-hammer-and-failure-test-plan.md
03-research-results/259-packet-016b-physical-design-audit.md
03-research-results/260-packet-016b-owner-acceptance-and-implementation-readiness-authorization.md
05-specs/milestone-11-local-postgres-preflight.md
03-research-results/261-packet-016c-implementation-readiness-audit.md
06-handoff-patterns/016c-implement-minimum-operational-data-foundation.md
```

Search the repository for:

```text
Postgres
PostgreSQL
Operational Postgres
Observatory
IMI
Qdrant
DataForSEO
provenance
retention
backup
restore
RLS
row-level security
migration
JSONB
pgvector
TimescaleDB
logical replication
PITR
WAL
```

Do not rely only on the files listed above if search reveals newer or contradictory records.

---

# 5. Current accepted first-slice design

The accepted Milestone 11 first slice currently contains:

```text
execution attempt
verification run
return-bundle manifest
artifact provenance
```

Deferred from the first implementation slice:

```text
governed-operation read model
connector invocation telemetry
```

Current planned technical posture:

```text
minimum supported PostgreSQL major:
16

implementation target:
18, pending live preflight and this research

Python:
3.11

planned driver:
psycopg 3

ORM:
none

migration system:
small Kaizen-owned immutable migration runner over reviewed SQL

database:
kaizen_ops

logical schemas:
platform_meta
operations

extensions:
none required in first slice

access:
typed service only; no direct agent SQL
```

Current Packet 016C is implementation-readiness complete but implementation is not yet authorized.

This research occurs at the correct intervention point: before database creation, SQL migrations, roles, or service implementation.

---

# 6. Source hierarchy

Use primary and authoritative sources wherever possible.

## Highest-priority sources

- official PostgreSQL documentation;
- official PostgreSQL release notes, versioning, security, and support pages;
- official Psycopg 3 documentation and release notes;
- official extension-maintainer documentation and repositories;
- official Microsoft/Windows documentation when evaluating Windows behavior;
- official cloud-provider documentation only when evaluating a named managed service;
- primary license texts and official vendor licensing statements;
- original research papers or official technical specifications where applicable.

## Secondary sources

Use high-quality engineering reports, issue trackers, mailing-list discussions, and operational postmortems to identify real-world failure modes and deployment friction.

Secondary sources may challenge official documentation but must not silently replace it.

## Weak sources

Blogs, SEO pages, vendor comparison pages, Reddit, and forum posts may surface questions or field experience. Treat them as leads, not architectural authority, unless independently corroborated.

## Citation rules

- cite every version-sensitive, security-sensitive, or capability-sensitive claim;
- include source publication or update dates when relevant;
- distinguish official support from community workarounds;
- identify when Windows support lags Linux or managed-service support;
- state when evidence is anecdotal or incomplete;
- do not cite AI-generated summaries as sources.

---

# 7. Core research questions

## A. PostgreSQL version selection

Compare PostgreSQL 16, 17, and 18 using the versions currently supported when the research is performed.

Also identify any newer beta or release-candidate version, but do not recommend pre-release software for Kaizen's first operational foundation.

Evaluate:

- support lifetime;
- security posture;
- Windows native maturity;
- Psycopg compatibility;
- backup and restore tooling;
- extension compatibility;
- managed-service availability;
- major-version upgrade path;
- logical-replication compatibility;
- features Kaizen would actually use;
- known regressions or operational concerns;
- portability from Windows proving ground to future Linux or managed hosting.

Give a clear recommendation:

```text
use PostgreSQL 18
use PostgreSQL 17
use PostgreSQL 16
or defer implementation
```

Do not choose a version merely because it is newest.

## B. Native PostgreSQL capability audit

Evaluate at minimum:

- transactions and isolation levels;
- declarative constraints;
- deferred constraints;
- exclusion constraints;
- range and multirange types;
- temporal primary, unique, and foreign-key constraints if available;
- generated columns;
- identity columns and sequences;
- native UUID and UUIDv7 support;
- Kaizen-prefixed ULIDs versus UUIDv7 versus internal numeric keys;
- advisory locks;
- `SKIP LOCKED`;
- `LISTEN` / `NOTIFY`;
- `MERGE` and upsert behavior;
- materialized views;
- full-text search;
- trigram search;
- JSONB and JSONPath;
- arrays;
- partitioning;
- row-level security;
- logical replication;
- physical streaming replication;
- WAL archiving and point-in-time recovery;
- checksums;
- statistics and `pg_stat_statements`;
- query timeouts and resource controls;
- large objects and bytea, specifically whether Kaizen should reject them for artifact storage;
- foreign data wrappers, if relevant later.

For every capability, classify:

```text
use in first slice
design for later
watchlist
reject for Kaizen
```

Explain the exact Kaizen problem it solves or why it should remain unused.

## C. Full Kaizen database topology

Evaluate realistic long-term topologies:

### Option 1

```text
one PostgreSQL cluster
one database
multiple logical schemas/domains
```

### Option 2

```text
one PostgreSQL cluster
separate databases for operational Kaizen, Observatory, and IMI
```

### Option 3

```text
separate clusters or managed instances for major authority/security domains
```

### Option 4

A staged hybrid that starts simple and separates domains only when workload, security, retention, or scaling evidence justifies it.

Consider:

- transaction boundaries;
- backup and restore coupling;
- extension requirements;
- retention differences;
- security and customer-data isolation;
- future provider/raw-payload workloads;
- Observatory time-series volume;
- IMI business intelligence;
- Qdrant identifiers;
- one-person operational burden;
- future migration to managed PostgreSQL;
- blast radius;
- cost.

Recommend the best **initial topology** and the best **credible evolution path**.

## D. Complete system-of-record boundary

Build a matrix covering at least:

- execution attempts;
- verification runs;
- return manifests;
- artifact provenance;
- governance operation indexes;
- connector telemetry;
- schedules and queues;
- costs and usage;
- source registrations and versions;
- provider requests and raw payloads;
- normalized observations;
- Observatory results;
- claims and evidence summaries;
- canonical decisions/specs/audits;
- project notes;
- artifact bytes;
- frozen reports;
- embeddings and vectors;
- customer and private data;
- reference data;
- deletion and retention events.

For each classify:

```text
authoritative Postgres record
Postgres metadata referencing external authority
derived and rebuildable Postgres state
Markdown authority
file authority
Git authority
governance JSONL authority
Qdrant candidate
separate database/domain candidate
defer or reject
```

Identify any current Kaizen design that accidentally creates dual authority.

## E. Schema and data-model review

Audit the current proposed schema in:

```text
05-specs/milestone-11-minimum-postgres-physical-design.md
```

Review:

- table boundaries;
- project isolation;
- composite keys and foreign keys;
- external versus internal identifiers;
- commit algorithm and digest representation;
- idempotency storage;
- lifecycle enforcement;
- immutable/versioned records;
- manifest entry and missing-item design;
- logical artifact versus byte-object model;
- provenance assertions;
- storage-root references;
- machine audit events;
- indexes;
- trigger use;
- nullability;
- timestamp semantics;
- status and error-code representation;
- future schema evolution.

Look for:

- normalization mistakes;
- over-normalization;
- missing constraints;
- brittle enums or status values;
- unnecessary tables;
- tables that should be append-only;
- fields that should use reference tables, checks, domains, or typed service validation;
- hidden future migration traps;
- misuse of JSON or text;
- query patterns that the proposed indexes do not support;
- assumptions tied too tightly to Git, Windows, or local paths.

Return exact keep/change/remove recommendations.

## F. Identifier strategy

Compare:

- Kaizen-prefixed ULIDs;
- native UUIDv7;
- UUIDv4;
- bigint internal keys plus external IDs;
- text primary keys;
- binary/native UUID storage with typed prefixes only at service boundaries.

Evaluate:

- ordering and index locality;
- interoperability with Markdown IDs;
- human debugging;
- storage and index size;
- collision and generation behavior;
- offline generation;
- future multi-system use;
- migration complexity.

Recommend:

- canonical external ID form;
- physical database key form;
- whether both should exist;
- whether the current text-primary-key posture should change before Packet 016C.

## G. Security and project isolation

Design the best realistic posture for a local single-owner proving ground that may later become multi-project and possibly multi-user.

Evaluate:

- SCRAM authentication;
- TLS locally and remotely;
- Windows service accounts;
- schema owners;
- migrator, runtime, backup, and readonly roles;
- RLS as defense in depth;
- composite project-safe foreign keys;
- service-layer authorization;
- `search_path` hardening;
- SQL injection resistance;
- statement and lock timeouts;
- connection limits;
- denial-of-service controls;
- logging and redaction;
- secret storage and rotation;
- backup-key separation;
- cross-project leakage tests;
- eventual customer/private-data isolation.

State what should exist in Packet 016C, what belongs later, and what should never be delegated to RLS alone.

## H. Backup, restore, and disaster recovery

Compare:

- `pg_dump` / `pg_restore`;
- `pg_dumpall` for globals;
- `pg_basebackup`;
- WAL archiving and PITR;
- physical replication;
- logical replication;
- third-party tools such as pgBackRest or Barman where justified;
- native Windows operational constraints;
- encrypted portable Kaizen recovery generations;
- coordinated database and artifact-file generations;
- checksums and verification;
- cross-major-version restore;
- Windows-to-Linux migration;
- local daily backup versus accepted 30-day off-machine generation;
- RPO and RTO appropriate to Kaizen's actual workload.

Recommend a staged recovery posture:

```text
first local proving ground
later serious local use
future managed or Linux production
```

Do not overbuild production PITR if Kaizen's first workload does not justify it, but do not create a dead-end backup design.

## I. Extensions and ecosystem dependencies

Evaluate at minimum:

- `pg_stat_statements`;
- `pg_trgm`;
- `pgcrypto`;
- `pgvector`;
- `ltree`;
- TimescaleDB;
- any extension genuinely relevant to provenance, temporal history, search, or Observatory workloads.

For each report:

```text
problem solved
core PostgreSQL alternative
first-slice need
Windows native support
Linux support
managed-service availability
license and source
upgrade burden
backup/restore implications
portability risk
recommendation
```

Do not recommend an extension just because it exists.

## J. Windows, Linux, containers, and managed PostgreSQL

Compare:

- native Windows PostgreSQL;
- WSL2;
- Linux VM;
- Docker or Podman;
- dedicated Linux machine;
- managed PostgreSQL.

Evaluate:

- reliability;
- backup and restore;
- file-volume interaction;
- extension availability;
- operational complexity for one owner;
- security;
- performance;
- upgrade path;
- portability;
- cost;
- disaster recovery;
- future customer workloads.

Recommend:

```text
best proving-ground environment now
best medium-term operating target
best migration path
```

## K. Psycopg, migrations, and service architecture

Audit the planned use of:

```text
Psycopg 3
no ORM
Kaizen-owned immutable SQL migration runner
typed service before MCP exposure
```

Compare against realistic alternatives:

- Alembic without SQLAlchemy ORM use;
- Flyway;
- Liquibase;
- Sqitch;
- yoyo-migrations;
- plain reviewed SQL plus custom manifest;
- SQLAlchemy Core;
- async versus sync Psycopg;
- connection pooling;
- transaction retry strategy.

Recommend the smallest option that provides:

- immutable migration evidence;
- source hashes;
- deterministic ordering;
- interruption recovery;
- upgrade testing;
- low operational burden;
- no accidental generic database tool for agents.

## L. Future Observatory, IMI, and retrieval compatibility

Without designing those systems prematurely, determine whether the first Kaizen database choices would obstruct:

- SERP and citation observations;
- time-series measurements;
- DataForSEO job and cost records;
- provider request and raw-payload references;
- marketplace monitoring;
- AI retrievability measurements;
- customer-isolated service work;
- Qdrant vector references;
- DuckDB/Polars analytical workflows;
- Context and Tool Gateway context assembly;
- Cloudflare and mcp2py-style acquisition systems.

Recommend future-proofing that is cheap now, and reject speculative scaffolding that adds present complexity.

---

# 8. Adversarial review requirements

Actively hunt for:

- version assumptions that have become stale;
- features whose Windows support is weaker than expected;
- backup plans that restore the database but not matching files;
- extension lock-in;
- migration systems that are too custom or too magical;
- text-ID and index-bloat problems;
- trigger overuse;
- weak project isolation;
- hidden superuser dependence;
- unsafe `search_path` behavior;
- missing transaction retry rules;
- connection-pool or concurrency hazards;
- unbounded JSONB growth;
- inappropriate partitioning;
- logical-replication misconceptions;
- RLS bypass conditions;
- deletion and retention contradictions;
- customer-data and licensing blind spots;
- database architecture that would become hard for one owner to operate;
- areas where Kaizen is building infrastructure before the workload earns it.

For each finding provide:

```text
finding ID
severity
confidence
exact Kaizen evidence
external technical evidence
why it matters
smallest safe correction
whether it blocks Packet 016C
whether it affects later packets only
```

Also identify at least five current design choices that are sound and should be preserved.

---

# 9. Required deliverable

Produce one comprehensive Markdown report with:

1. executive verdict;
2. repository and source-verification record;
3. Kaizen workload and authority summary;
4. PostgreSQL 16/17/18 comparison;
5. recommended PostgreSQL version and operating environment;
6. native capability classification matrix;
7. full database-topology recommendation;
8. complete system-of-record matrix;
9. current physical-schema audit;
10. identifier-strategy recommendation;
11. security and project-isolation design;
12. backup, restore, and disaster-recovery recommendation;
13. extension decision matrix;
14. Windows/Linux/container/managed-service comparison;
15. Psycopg and migration-tooling recommendation;
16. future Observatory/IMI/retrieval compatibility analysis;
17. errors, gaps, risks, and improvement register;
18. strengths to preserve;
19. exact Packet 016C impact assessment;
20. exact impacts on Packets 016D–016F;
21. staged implementation roadmap for the full Kaizen database foundation;
22. do-not-build-yet list;
23. unresolved owner decisions;
24. source register with dates and authority classification;
25. limitations and inaccessible evidence.

## Exact Packet-impact format

End with a table:

| Existing artifact | Keep | Revise | Retire | Exact reason | Blocking before implementation? |
|---|---:|---:|---:|---|---:|

Include at least:

```text
05-specs/milestone-11-minimum-postgres-physical-design.md
05-specs/milestone-11-typed-service-contract.md
05-specs/milestone-11-backup-restore-retention-design.md
05-specs/milestone-11-hammer-and-failure-test-plan.md
05-specs/milestone-11-local-postgres-preflight.md
06-handoff-patterns/016c-implement-minimum-operational-data-foundation.md
```

Then provide:

```text
FINAL RECOMMENDATION

Packet 016C may proceed unchanged
or
Packet 016C may proceed after listed non-blocking corrections
or
Packet 016C must be revised before approval
or
PostgreSQL implementation should be deferred
```

Do not leave the implementation disposition ambiguous.

---

# 10. Restrictions

- Read-only research and audit.
- Do not modify or push the repository.
- Do not assume local implementation repositories are available unless explicitly supplied.
- Do not claim to have tested PostgreSQL or Kaizen code unless you actually ran and recorded the test.
- Do not promote findings directly into Kaizen doctrine.
- Do not invent owner decisions.
- Do not recommend broad rewrites without exact evidence.
- Do not recommend trendy extensions, microservices, Kubernetes, or distributed systems without a demonstrated Kaizen need.
- Do not move Markdown authority, artifact bytes, governance JSONL, or Git history into PostgreSQL merely for convenience.
- Be explicit when a recommendation is for the first slice, later Milestone 11 work, or future architecture only.

The output is research evidence for steward reconciliation and owner decision. It is not implementation authorization.
