# Research Result 010 - Postgres MCP and Typed-Tool Comparative Audit

Status: active evidence summary
Date: 2026-06-05
Source: Claude Research Step 5A Postgres report supplied by the project owner

## Purpose

Preserve and reconcile Claude's comparative audit of Postgres MCP servers, managed Postgres integrations, SQL gateways, PostgREST, role/RLS patterns, and typed service architectures.

This file is evidence and steward reconciliation. It does not select a final Postgres integration, approve PostgREST as doctrine, authorize schema design, or permit agent SQL access.

## Executive finding

Claude found no surveyed Postgres MCP server suitable as-is for Kaizen's governed agent lanes.

The strongest current candidate architecture is a hybrid:

```text
Kaizen typed read tools
-> scoped gateway such as PostgREST
-> approved views/functions
-> read-only role and RLS
-> Operational Postgres

Kaizen typed operational services
-> domain functions and legal state transitions
-> scoped write role
-> Operational Postgres

Human/CI/admin tooling
-> migrations, DDL, backup, restore, diagnostics
```

This direction is adopted as the leading architecture candidate, but it is not yet accepted doctrine. It requires disposable prototype evidence and hammer tests.

## Adopt

### No arbitrary SQL for agents

Adopt the report's central conclusion that Hermes and general reasoning agents must not receive:

- arbitrary SQL execution;
- direct connection strings;
- raw database clients;
- unrestricted schema introspection;
- migration or DDL tools;
- role, extension, backup, or administrative controls.

The confirmed failure of a reference Postgres MCP server's application-level read-only wrapper demonstrates why server-side intent is insufficient when the underlying database role and query surface remain broad.

### Database-enforced scope is required

Adopt the requirement that project and privacy scope be enforced independently of agent behavior.

Useful controls include:

- dedicated database roles;
- row-level security;
- approved views;
- column grants;
- typed functions or RPC endpoints;
- transaction-scoped context injection;
- result limits and timeouts;
- trusted service-side scope injection;
- append-only audit evidence.

Application prompts and caller-supplied filters are not sufficient.

### RLS and views require effective-role proof

RLS being enabled is not itself proof of isolation.

Prototype and implementation review must verify:

- the effective execution role for every request;
- whether the role owns the table or can bypass RLS;
- whether privileged or service roles bypass policies;
- whether views execute with owner privileges or caller privileges under the chosen Postgres version and configuration;
- whether security-definer functions can bypass intended scope;
- whether function ownership, `search_path`, grants, and role switching create escalation paths;
- whether `FORCE ROW LEVEL SECURITY` or an equivalent design requirement is needed for relevant tables;
- whether connection pooling preserves the intended effective role and request identity.

Kaizen must test the complete role, object-ownership, view, function, and pooler chain rather than treating `ALTER TABLE ... ENABLE ROW LEVEL SECURITY` as the finished boundary.

### Read-only is not automatically safe

Adopt the finding that database reads can still leak:

- cross-project rows;
- private columns;
- system catalogs;
- schema and constraint details;
- query plans;
- privileged metadata;
- unbounded result sets;
- pooled-session context from another request.

Read/query tools require project, domain, schema, column, privacy, result-size, timeout, and catalog restrictions.

### Capability lanes

Adopt four Postgres capability lanes.

#### Lane 1 - Governed read and result access

Potential agent-facing capabilities:

- approved Observatory result lookup;
- project-scoped operational status;
- approved provenance and quality metadata;
- allowlisted reports;
- pagination and bounded filters.

No arbitrary SQL or caller-selected table/schema access.

#### Lane 2 - Governed operational mutations

Reserved for approved operational services:

- job and run creation;
- observation recording;
- legal workflow-state transitions;
- cost and usage recording;
- audit-event append;
- result publication and supersedence.

These are typed domain operations, not SQL tools.

#### Lane 3 - Schema and data administration

Human/CI/admin only:

- migrations and DDL;
- role and policy management;
- backup and restore;
- retention and partition management;
- extensions;
- maintenance and incident recovery.

#### Lane 4 - Developer and diagnostic access

Disposable, development, or tightly controlled environments only:

- schema inspection;
- query plans;
- health checks;
- index recommendations;
- migration verification;
- synthetic-data troubleshooting.

### Ecosystem reuse

Adopt the principle that Kaizen should reuse mature Postgres capabilities instead of rebuilding a database platform.

Candidate reusable capabilities include:

- RLS;
- views and column grants;
- stored functions;
- constraints and transactions;
- idempotency via uniqueness;
- PostgREST or equivalent typed gateways;
- connection pooling;
- migration tooling;
- managed backups;
- audit extensions and observability;
- query and performance diagnostics for admin/developer lanes.

Kaizen should own authority-bearing domain contracts and typed tools, not standard database plumbing.

## Modify

### PostgREST plus RLS is a candidate, not accepted architecture

Modify any conclusion that PostgREST+RLS should be immediately adopted as doctrine.

The report provides a strong evidence-backed candidate because it can:

- avoid raw SQL pass-through;
- expose approved tables, views, and functions;
- switch roles per request;
- use RLS for project isolation;
- limit columns through views and grants;
- support typed RPC operations.

However, Kaizen must first prove in a disposable prototype that:

- project isolation survives real request paths;
- connection pooling does not leak context;
- no granted function bypasses intended scope;
- system catalogs and hidden schemas remain inaccessible;
- private columns remain excluded;
- tool contracts cannot become generic pass-through endpoints;
- logging and audit identity are adequate;
- operational complexity is justified.

Until those tests pass, PostgREST remains the leading Lane 1 candidate, not final doctrine.

### Database functions are not automatically safe domain services

Modify the report's suggestion that typed functions alone provide the Lane 2 boundary.

Functions may still:

- run with elevated privileges;
- accept overly broad identifiers;
- bypass RLS under security-definer semantics;
- allow illegal state transitions;
- hide side effects;
- expose private data;
- weaken auditability.

Every Lane 2 function or service requires explicit ownership, role, scope, transaction, provenance, idempotency, state-transition, and audit rules.

### Role names are illustrative

Treat names such as:

```text
kaizen_agent_ro
kaizen_svc
kaizen_admin
```

as examples for prototype planning, not accepted production identifiers.

The accepted requirement is role separation, not these exact names.

### Gateway and wrapper layering

A typed Kaizen tool layer should remain in front of any general REST, GraphQL, RPC, or database gateway.

The agent should receive operations such as:

```text
get_observatory_result
list_project_jobs
get_result_provenance
```

It should not receive a generic URL builder, table selector, SQL string, RPC name selector, or unrestricted filter expression.

## Reject

Reject:

- direct SQL MCP as a production Hermes surface;
- application-only `BEGIN ... READ ONLY` as the sole write-prevention control;
- superficial `SELECT` prefix checks;
- managed service-role or superuser credentials for agents;
- unrestricted schema introspection for production agent lanes;
- DDL, migration, role, extension, backup, or destructive tools for general agents;
- direct table mutation that bypasses Kaizen domain logic;
- trusting a read-only label without database-role and scope verification;
- promoting PostgREST, GraphQL, or another gateway to doctrine without prototype evidence.

## Defer

Defer until workload and prototype evidence exists:

- final gateway selection;
- PostgREST versus another typed API layer;
- exact database roles and names;
- exact RLS policies;
- exact views and function signatures;
- exact table and schema design;
- exact pooler;
- exact JWT or token model;
- exact migration framework;
- exact hosting provider;
- final audit extension and logging stack.

## Candidate disposition

### Reference Postgres MCP

Disposition: `reject for use; retain as vulnerability and hammer-test reference`.

Its archived status and confirmed read-only escape demonstrate the danger of forwarding agent-supplied SQL through an application-level transaction wrapper.

### Developer-focused Postgres MCP servers

Disposition: `Lane 4 evaluation only`.

Potentially useful for development and diagnostics against synthetic or non-production databases, but broad SQL and schema visibility prevent use in Hermes production lanes.

### Supabase MCP

Disposition: `human/developer admin evaluation only; reject for governed agent access with privileged service credentials`.

A service-role-style credential that bypasses RLS is incompatible with Kaizen's project and privacy boundary.

### Neon MCP

Disposition: `developer/admin evaluation; read-only claims require database-role proof`.

A read-only header or mode is defense in depth. It does not replace database roles, RLS, query-surface restrictions, and hands-on verification.

### PostgREST

Disposition: `leading Lane 1 prototype candidate`.

It is valuable because it exposes structured requests rather than raw SQL and can use database roles, grants, views, functions, and RLS. It remains behind Kaizen typed tools and requires prototype evidence before adoption.

### Broad CRUD or DDL database MCP servers

Disposition: `reject for general agent and production use`.

Their tool surfaces are useful only as negative comparison or possibly isolated developer utilities.

## Configure-versus-wrap matrix

| Candidate pattern | Safe reduction mechanism | Required proof | Likely Kaizen use |
|---|---|---|---|
| SQL MCP with broad query tool | Usually reject or isolate | Database role blocks writes, single-statement enforcement, no catalog/private access, tool absent from production profile | Lane 4 only |
| Managed-platform MCP with privileged key | Separate human/admin environment or reject | Credential scope cannot bypass RLS or cross project boundaries | Human/admin only |
| PostgREST or equivalent gateway | Configure database roles/RLS/views, then wrap | Mandatory scope, approved endpoints, private-column exclusion, no generic pass-through | Lane 1 candidate |
| GraphQL/data API gateway | Configure plus wrap | Schema and resolver allowlists, role enforcement, query-cost limits, mutation exclusion | Lane 1 candidate if proven |
| Typed Kaizen domain service | Kaizen-owned contract | State transitions, idempotency, project scope, provenance, audit, transaction behavior | Lane 2 |
| Migration/DBA tools | Separate credentials and execution environment | Human/CI gate, audit, backup and rollback plan | Lane 3 |

## Authentication and pooling constraints

Prototype and later implementation planning must account for:

- role and credential separation;
- service-role and superuser bypass risk;
- connection-pool identity behavior;
- transaction-pool versus session-pool mode;
- transaction-scoped context rather than persistent session assumptions;
- short-lived credentials where practical;
- secret isolation from agent tool payloads and logs;
- propagation of actor, project, request, and operation identity into audit evidence.

Any project context set through a pooled connection must be injected and cleared inside the transaction boundary or derived from a verified request identity. Session state must not leak between requests.

## Prototype requirements

The leading candidate architecture requires a disposable Postgres prototype with synthetic data only.

Minimum test groups:

### Project and privacy isolation

- Project A cannot read Project B.
- Private rows and columns remain hidden.
- Missing project context fails closed.
- Cross-project identifiers do not bypass RLS.
- System catalogs and disallowed schemas remain inaccessible.

### Query-surface safety

- No arbitrary SQL tool appears in the Lane 1 profile.
- Generic table, schema, function, and URL selectors are absent.
- Result-size, pagination, and timeout limits hold.
- Data-modifying CTE and stacked-statement probes cannot bypass the role boundary.
- Side-effect functions are not granted to the read role.

### Pooling and identity

- Project context does not leak across pooled transactions.
- Role and request identity are preserved in audit evidence.
- Invalid, missing, expired, or wrong-project tokens fail closed.

### Lane 2 mutation behavior

- Illegal state transitions fail atomically.
- Idempotent retries do not duplicate records.
- Partial failure rolls back.
- Audit and provenance records are created in the same valid transaction boundary where appropriate.
- Spend limits and authorization checks cannot be bypassed.

### Administration separation

- Agent profiles contain no DDL, migration, role, extension, backup, restore, or maintenance tools.
- Developer diagnostics are absent from production agent discovery.
- Admin credentials cannot be reached through typed agent endpoints.

## Hammer-test additions

Future Postgres hammer coverage should include:

- stacked-statement escape attempts;
- data-modifying CTEs;
- `SET ROLE` and role-escalation attempts;
- security-definer function abuse;
- catalog and hidden-schema reads;
- project-context omission and substitution;
- pool-context leakage;
- unbounded query and recursive-query denial;
- private-column leakage;
- result-limit enforcement;
- illegal state transitions;
- duplicate idempotency keys;
- transaction rollback;
- audit completeness;
- admin tool-discovery absence.

## Likely document updates

After human review and prototype evidence, findings may affect:

- `04-design-decisions/0005-api-only-structured-data-access.md`
- `05-specs/operational-postgres-authority.md`
- `05-specs/kaizen-hammer-test-strategy.md`
- `07-hermes/hermes-permission-matrix.md`
- `ROADMAP.md`

Do not update accepted Decision 0005 merely to name PostgREST. The decision already requires typed, scoped, logged access and prohibits arbitrary SQL. Candidate implementation patterns belong in draft specifications or a later evidence-backed decision.

## Roadmap consequences

Step 5A should record:

```text
Postgres comparative audit complete
-> leading Lane 1 candidate identified
-> prototype and hammer evidence still required
-> no final gateway selected
```

Phase 7 implementation-planning inputs should eventually define:

- read, mutation, admin, and developer lanes;
- role and credential separation;
- required scope and privacy enforcement;
- tool discovery boundaries;
- prototype evidence;
- migration, backup, and recovery ownership;
- typed result and mutation contracts.

## Ordered next actions

1. Preserve this audit and reconciliation as evidence.
2. Do not promote PostgREST into accepted doctrine yet.
3. Review this result together with Qdrant Result 011 and the composed Step 5A capability map in Result 012.
4. Add Postgres prototype and hammer requirements to later implementation-planning inputs.
5. Keep Hermes limited to future typed Lane 1 read/result tools.
6. Keep Lane 2 mutations inside approved operational services.
7. Keep migrations, DDL, roles, backup, restore, and diagnostics outside general agent profiles.
8. Proceed to Research Batch B only after the combined Step 5A reconciliation is reviewed and checkpointed.

## Source limitation

The raw Claude report was supplied as an uploaded Markdown artifact. This file preserves its load-bearing findings and the project steward's reconciliation. The raw report should also be retained where repository access and archival policy permit.
