# Claude Research Prompt 005 - Postgres MCP and Typed-Tool Comparative Audit

Status: active research prompt
Date: 2026-06-05

## Prompt

You are conducting the Postgres portion of Kaizen Research Step 5A.

This is a comparative research and architecture-evaluation task only.

Do not edit the Kaizen repository.
Do not create files in it.
Do not stage, commit, push, reset, clean, or discard changes.
Do not connect any candidate to a real Kaizen database.
Do not install or run database tools against valuable systems.
Do not select a final server from README claims alone.
Do not design exact Kaizen tables or migrations.
Do not produce an implementation roadmap.

Return a report only.

## Repository

Read the live local planning repository:

```text
C:\dev\kaizen-docs
```

Start by reporting:

```text
git status
git log -5 --oneline
```

Treat this repository as Kaizen's planning and stewardship workspace, not a running database project.

## Central objective

Compare prominent Postgres MCP servers, database gateways, typed API patterns, and related tooling against Kaizen's accepted system-of-record, authority, privacy, audit, cost, and capability-lane requirements.

The purpose is to determine which capabilities Kaizen should later:

```text
reuse
configure
wrap
fork
expose read-only
reserve for operational services
reserve for human/admin use
reject
```

Do not optimize only for the smallest possible tool surface. Preserve useful Postgres ecosystem capabilities while separating read, write, administrative, destructive, and authority-bearing operations.

## Required Kaizen reading

Read these files first:

```text
00-entrypoint/LLM_START_HERE.md
01-project-standard/kaizen-vision-and-architecture-alignment.md
ROADMAP.md
04-design-decisions/0004-system-of-record-boundaries.md
04-design-decisions/0005-api-only-structured-data-access.md
04-design-decisions/0006-hammer-tests-are-a-hard-gate.md
04-design-decisions/0009-operational-postgres-and-observatory-boundary.md
05-specs/operational-postgres-authority.md
05-specs/kaizen-hammer-test-strategy.md
07-hermes/hermes-permission-matrix.md
03-research-results/004-markdown-qdrant-postgres-architecture-claude-summary.md
03-research-results/007-kaizen-vision-architecture-research-gap-audit-claude-summary.md
03-research-results/009-obsidian-mcp-tool-surface-audit-claude-summary.md
```

## Accepted Kaizen constraints

Treat these as fixed unless you identify a direct contradiction requiring human review:

- Operational Postgres owns governed structured operational records assigned to explicit domains.
- Markdown owns canonical project narrative, accepted claims, decisions, specifications, audits, and implementation handoffs.
- Qdrant owns no canonical truth.
- Agents receive no arbitrary SQL, direct database credentials, unrestricted schema access, agent-controlled migrations, or raw administrative access.
- All agent-facing Postgres access must use approved typed, scoped, validated, logged tools or APIs.
- Every project-scoped record identifies its project.
- Cross-project reads require explicit governed authority.
- Private or customer data is not exposed by default.
- Paid operations require spend controls and evidence.
- Read-only is not automatically safe: queries can leak private rows, system catalogs, credentials, query text, or cross-project data.
- Tool discovery, parameter scope, database roles, row-level security, views, stored procedures, gateways, and operating-service boundaries all matter.
- Ecosystem capabilities should be preserved where useful, but canonical and authority-bearing operations remain governed.

## Capability-lane model

Use the following lanes when evaluating candidates.

### Lane 1 - Governed read and result access

Potential capabilities:

- retrieve approved Observatory result summaries;
- retrieve project-scoped operational status;
- inspect approved provenance and data-quality metadata;
- run parameterized, allowlisted reports;
- paginate and filter within explicit project/domain scope.

### Lane 2 - Governed operational mutations

Potential capabilities reserved for approved services:

- create jobs and runs;
- record observations;
- update legal workflow states;
- record costs and usage;
- append audit events;
- publish or supersede result summaries.

These should be typed domain operations, not arbitrary SQL.

### Lane 3 - Schema and data administration

Human/admin or controlled deployment capabilities:

- migrations;
- DDL;
- role and policy management;
- backup and restore;
- retention and partition management;
- maintenance, vacuum, indexing, and performance work;
- incident recovery.

These are not exposed to Hermes or general reasoning agents.

### Lane 4 - Developer and diagnostic access

Potentially useful in disposable or tightly controlled environments:

- schema inspection;
- query plans;
- diagnostics;
- test-data inspection;
- migration verification.

This lane requires separate credentials, environments, and audit policy.

## Candidate discovery

Research current maintained candidates as of the report date.

At minimum inspect:

1. prominent Postgres-specific MCP servers;
2. generic SQL/database MCP servers that support Postgres;
3. Supabase MCP or analogous managed-Postgres integrations where relevant;
4. official or prominent database gateways and typed API layers, such as PostgREST, Hasura, GraphQL engines, RPC/stored-procedure gateways, or equivalent patterns;
5. database proxy and policy patterns that may constrain query scope;
6. one read-only or analytics-oriented database tool surface;
7. one developer/admin-oriented tool surface for contrast.

Do not assume product names in this prompt are recommendations. Verify identity, maintenance, source, transport, authentication, and tool schemas.

## Source standards

Prefer primary sources:

- official repositories and documentation;
- MCP manifests and tool schemas;
- server source code;
- Postgres official documentation;
- vendor security and authentication documentation;
- issue trackers;
- release notes;
- security advisories and CVE databases.

For every candidate, record the date checked.

Do not infer safety from popularity, localhost deployment, managed hosting, or absence of reported vulnerabilities.

## Required candidate analysis

### 1. Maintenance and provenance

Record:

- official identity and repository;
- owner and license;
- latest release and commit;
- maintenance status;
- contributor concentration;
- unresolved security-relevant issues;
- official, vendor-maintained, community, experimental, or abandoned status.

### 2. Architecture and transport

Determine:

- MCP server, REST gateway, GraphQL gateway, RPC layer, proxy, or SDK wrapper;
- STDIO, HTTP, SSE, WebSocket, local socket, or hosted transport;
- authentication model;
- credential storage;
- TLS requirements;
- network exposure;
- whether the tool runs beside the database, in a trusted service tier, or on a client machine.

### 3. Complete tool inventory

Inspect tool registration and schemas.

Classify each tool as:

```text
read/query
parameterized report
schema introspection
metadata/catalog read
stored procedure or RPC
insert
update
delete
bulk load
transaction control
DDL
migration
role/policy administration
backup/restore
maintenance
diagnostic
other
```

For every tool, record:

- exact name;
- request fields;
- response shape;
- SQL or database action performed;
- whether arbitrary SQL is accepted;
- whether table/schema names are caller-controlled;
- whether unsafe tools can be absent from discovery;
- logging and error behavior.

### 4. Query and read confinement

Read-only access can still leak sensitive data.

Inspect whether the candidate enforces:

- project scope;
- domain scope;
- row-level security;
- schema allowlists;
- table/view allowlists;
- column filtering;
- private-field redaction;
- result-size limits;
- query timeouts;
- statement timeouts;
- pagination;
- aggregation restrictions;
- system-catalog restrictions;
- prepared or parameterized queries;
- SQL-function or RPC allowlists.

Determine whether scope is:

```text
injected by trusted service
caller supplied and validated
caller supplied but untrusted
role enforced
RLS enforced
view enforced
not enforced
unclear
```

### 5. Arbitrary SQL risk

For candidates exposing SQL execution, inspect:

- single statement versus multiple statements;
- read-only transaction mode;
- `SELECT`-only parsing;
- common-table expressions;
- data-modifying CTEs;
- functions with side effects;
- `COPY`;
- `SET ROLE`;
- `SET search_path`;
- temporary objects;
- extensions;
- large or recursive queries;
- catalog access;
- query cancellation;
- prepared statement behavior.

Do not treat a superficial `SELECT` prefix check as safe.

### 6. Mutation semantics

For every write-capable candidate, determine:

- typed operation versus arbitrary SQL;
- legal state-transition enforcement;
- idempotency keys;
- optimistic concurrency;
- transaction boundaries;
- append-only event behavior;
- provenance capture;
- project/domain scope;
- spend authorization;
- retry behavior;
- partial failure behavior;
- audit evidence;
- whether direct table writes bypass domain logic.

Compare mutation tools against the Operational Postgres authority specification.

### 7. Configure-versus-wrap analysis

Determine how unsafe capabilities become unavailable.

#### Configure

Acceptable only when:

- unsafe tools are absent from MCP discovery;
- database roles prohibit the operation independently;
- RLS or views enforce scope;
- generic SQL pass-through is absent;
- configuration is capturable and testable.

#### Wrap

Required when:

- one credential grants a broad database surface;
- one generic query tool accepts caller-controlled SQL;
- the upstream API exposes more tables/columns/functions than the agent should see;
- scope and redaction must be injected by a trusted service;
- audit or cost controls need to be added.

A wrapper must expose typed operations and no generic pass-through endpoint.

### 8. Authentication, roles, and credential boundaries

Evaluate:

- database users and roles;
- least privilege;
- read-only roles;
- RLS and policy ownership;
- service-role bypass risks;
- connection pooling identity behavior;
- transaction-pool versus session-pool effects;
- secrets at rest;
- credential rotation;
- short-lived credentials;
- managed-service service keys;
- local developer credentials;
- audit identity propagation.

### 9. Schema introspection and metadata exposure

Determine whether the tool exposes:

- table and column names;
- comments and descriptions;
- indexes and constraints;
- function definitions;
- RLS policies;
- roles and privileges;
- extension inventory;
- connection or server metadata.

Classify which metadata is useful for agents and which may leak private architecture or attack surface.

### 10. Logging, audit, and observability

Determine whether the candidate records:

- actor and client identity;
- session and request ID;
- tool name;
- operation or query template;
- bound parameters with redaction;
- project/domain scope;
- rows returned or changed;
- duration and timeout;
- cost or provider charge where relevant;
- denial reason;
- transaction/result state;
- database role and policy context.

Check whether logs can expose secrets, customer data, or raw private rows.

### 11. Headless and interactive contexts

Separate:

#### Headless operational context

Examples:

- ingestion workers;
- scheduled jobs;
- Observatory computation;
- monitoring;
- CI and migration verification;
- backup and recovery.

These use service identities and typed operational capabilities.

#### Interactive analytical context

Examples:

- human review of approved result summaries;
- scoped exploratory analysis;
- project dashboards;
- agent-assisted interpretation of governed results.

Interactive convenience must not expose broad SQL or private cross-project data.

### 12. Ecosystem-value preservation

Identify capabilities that would be wasteful to rebuild, such as:

- connection pooling;
- RLS;
- views and materialized views;
- stored procedures;
- PostgREST or GraphQL query generation;
- migration tooling;
- observability;
- managed backups;
- logical replication;
- query analysis;
- policy engines.

The goal is not to build a custom database platform. Kaizen should own authority-bearing domain contracts and typed tools while reusing mature Postgres infrastructure.

## Comparative architectural options

Compare:

### Option A - Direct Postgres MCP with SQL

```text
agent -> MCP SQL tool -> Postgres
```

### Option B - Read-only Postgres MCP

```text
agent -> reduced query tools -> read-only role/views/RLS -> Postgres
```

### Option C - Managed gateway

```text
agent -> MCP wrapper -> PostgREST/GraphQL/RPC gateway -> Postgres
```

### Option D - Kaizen typed service

```text
agent -> Kaizen typed tools -> domain service -> Postgres
```

### Option E - Hybrid

```text
approved read/report gateway
+
Kaizen typed operational mutations
+
separate human/admin database tooling
```

Compare security, scope, ecosystem value, logging, cost, maintenance burden, headless operation, and suitability for Hermes, reasoning agents, operational workers, developers, and humans.

## Hands-on prototype recommendations

Do not run prototypes in this task.

For candidates worth considering, define disposable tests using a temporary Postgres instance with synthetic data only.

At minimum test:

- exact tool-discovery snapshot;
- absence versus denial of unsafe tools;
- project A cannot read project B;
- private columns remain hidden;
- system catalogs and disallowed schemas are inaccessible;
- arbitrary SQL is rejected where prohibited;
- data-modifying CTEs and side-effect functions are blocked;
- result-size and timeout limits;
- mutation idempotency;
- illegal state transitions;
- transaction rollback;
- audit-log completeness;
- credential rotation;
- role and RLS enforcement through connection pooling;
- no DDL, migration, role, or extension management through agent tools.

## Required output

Return one report with these sections.

### 1. Executive verdict

State:

- whether any candidate is suitable as-is;
- which read/report surfaces are promising;
- whether Kaizen still needs typed domain tools;
- which ecosystem capabilities should be reused;
- the strongest and weakest architectures;
- the most important hands-on tests.

### 2. Repository and Git state observed

Include branch, latest commit, clean or dirty status, and no-write confirmation.

### 3. Candidate inventory

Use columns:

```text
Candidate
Official identity
Architecture
Maintenance
Transport/auth
Primary use
Date checked
```

### 4. Tool inventory matrix

Use columns:

```text
Candidate
Tool name
Class
Arbitrary SQL?
Mutation/admin potential
Can be absent?
Kaizen concern
```

### 5. Scope and security matrix

Use columns:

```text
Candidate
Role model
RLS support
Schema/table scope
Column/privacy controls
Result limits
Logging
Confidence
```

### 6. Read/query comparison

Compare read-only, parameterized, view-based, RPC, GraphQL, REST, and arbitrary-SQL patterns.

### 7. Mutation and administration comparison

Compare typed domain operations, raw writes, DDL, migrations, roles, backup, and destructive capabilities.

### 8. Configure-versus-wrap matrix

For each candidate, state whether safe reduction is achieved by configuration, database roles/RLS, a wrapper, a fork, or rejection.

### 9. Authentication and credential analysis

Cover roles, service credentials, pooling, managed-service keys, rotation, and identity propagation.

### 10. Known vulnerabilities and security-relevant issues

Use primary evidence. Separate confirmed vulnerabilities, unresolved issues, and theoretical risks.

### 11. Ecosystem-value map

Identify what Kaizen should reuse rather than rebuild.

### 12. Kaizen capability-lane recommendation

Recommend a composed Postgres tool architecture without designing tables.

### 13. Prototype and hammer-test plan

Provide candidate-specific disposable tests.

### 14. Files likely needing later revision

List exact Kaizen paths and likely changes. Do not edit them.

### 15. Ordered next actions

Give a dependency-aware sequence.

### 16. Final no-write confirmation

Confirm no files, installs, database connections, staging, commits, or pushes occurred.

## Important constraints

Do not:

- recommend arbitrary SQL for Hermes;
- assume read-only SQL is safe without role, RLS, view, function, and catalog analysis;
- assume a managed service key is appropriately scoped;
- expose migration, DDL, role, backup, extension, or destructive tools to general agents;
- design exact Kaizen tables;
- treat Postgres as canonical project narrative;
- discard mature Postgres ecosystem capabilities merely because Kaizen needs typed authority boundaries;
- connect to a real database;
- produce the implementation roadmap.

Be skeptical, source-driven, and precise.

Return the report only.
