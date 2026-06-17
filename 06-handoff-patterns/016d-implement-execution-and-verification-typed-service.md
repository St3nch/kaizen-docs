---
id: kz-tp-01KV6T2F4M2N7C5Y3P8T6D1E40
type: task-packet
status: ready-for-approval
project: kaizen-platform
created: 2026-06-17T15:31:00Z
updated: 2026-06-17T15:31:00Z
review_status: passed
authority: proposed
summary: "Implement the bounded execution-attempt and verification-run typed service over the accepted Milestone 11 PostgreSQL foundation."
---

# Task Packet 016D - Implement Execution and Verification Typed Service

## Authority

Ready for exact-hash owner approval. This packet is not yet approved and may not be executed.

Governing records:

```text
Milestone 11 planning:
05-specs/milestone-11-operational-data-foundation-planning.md

Identity, lifecycle, and recovery contract:
05-specs/milestone-11-identity-lifecycle-recovery-contract.md

Minimum PostgreSQL physical design:
05-specs/milestone-11-minimum-postgres-physical-design.md

Typed service contract:
05-specs/milestone-11-typed-service-contract.md
SHA-256: af5f0d00647ccdf8518adcebe05a372d75095718bba30d9350f479ef45e9fa4b

Hammer and failure-test plan:
05-specs/milestone-11-hammer-and-failure-test-plan.md

Packet 016C owner completion acceptance:
03-research-results/270-packet-016c-owner-completion-acceptance.md
SHA-256: 69b0dd6552e78c18fbc096fe45b84ff9f1efc94d2d66b3391697cadf26a097f1
```

## Objective

Implement only the transport-neutral typed service and repository boundary for:

```text
reserve execution attempt
start execution attempt
finish execution attempt
record verification run
get one execution evidence graph limited to attempts and verification runs
list project attempts with bounded filters and pagination
```

Packet 016D establishes the first runtime write path into `kaizen_ops` without giving agents, MCP tools, or callers direct SQL or database credentials.

## Starting checkpoint

```text
repository:
C:\dev\kaizen\platform

branch:
main

required starting commit:
1472d65d20162a07a3ca12b73f52348ed67dd291

required working tree:
clean

required remotes:
none
```

Stop on checkpoint drift unless the owner approves a revised packet binding.

## Scope boundary

Included:

- typed request and response envelopes;
- bounded service error taxonomy;
- execution-attempt repository operations;
- verification-run repository operations;
- idempotency enforcement;
- atomic service audit events;
- project-safe reads and writes;
- expected-state lifecycle transitions;
- bounded list and graph reads;
- a reviewed migration for service grants or narrowly required schema corrections;
- a local service-role proving ground;
- deterministic integration and hammer tests.

Excluded:

- return-manifest freeze;
- return-manifest entries or missing-item writes;
- artifact byte-object creation;
- artifact provenance writes;
- file reading or hashing;
- canonical authority links;
- retention planning or execution;
- backup or restore implementation;
- MCP adapters or tools;
- HTTP, RPC, queue, worker, or daemon deployment;
- Observatory, connector telemetry, IMI, Qdrant, customer data, or multi-user identity;
- direct agent SQL;
- vault mutation;
- platform remote creation.

## Service architecture

```text
caller
-> typed in-process service interface
-> typed repository
-> Psycopg parameterized SQL
-> kaizen_ops
```

The service is transport-neutral and testable without MCP or a network listener.

The service receives a process-injected service-role DSN. No caller receives that DSN. The repository module is the only new source boundary allowed to contain SQL, and all SQL is fixed, parameterized, and capability-specific.

## Required capabilities

### 1. `reserve_execution_attempt`

Input:

```text
request envelope
packet_or_task_id
repository_id
starting commit algorithm and digest
allowed_scope_ref
predecessor_attempt_id optional
```

Required behavior:

- validate request, project, repository, commit identity, and predecessor;
- create one `reserved` execution attempt;
- create one service audit event;
- create or replay one idempotency record;
- commit atomically;
- exact retry returns `replayed` with the original attempt ID;
- changed payload under the same idempotency key returns `idempotency_conflict`.

### 2. `start_execution_attempt`

Required transition:

```text
reserved -> started
```

Required behavior:

- compare expected status;
- reject stale or terminal state;
- update lifecycle timestamp;
- create audit and idempotency records atomically.

### 3. `finish_execution_attempt`

Required transition:

```text
started -> succeeded | failed | interrupted | cancelled
```

Required behavior:

- reject all other terminal states;
- preserve the distinction between executor completion and verification acceptance;
- create audit and idempotency records atomically.

### 4. `record_verification_run`

Required behavior:

- accept one already-terminal verification observation;
- distinguish passed, failed, blocked, timed out, transport failed, and interrupted outcomes according to the accepted contract and physical schema;
- bind the run to one project-safe execution attempt;
- permit a normalized command ID and bounded summary counts;
- reference raw evidence only by existing storage-root identity and relative path;
- do not read, hash, or claim `file_verified` in this packet;
- create audit and idempotency records atomically.

### 5. `get_execution_evidence_graph`

Returns only:

```text
execution attempt
verification runs
```

Requirements:

- explicit project ID and attempt ID;
- project-safe joins;
- deterministic ordering;
- no raw evidence bytes or unrestricted payloads;
- return-manifest and provenance families remain absent until Packet 016E.

### 6. `list_project_attempts`

Supported filters:

```text
project_id required
packet_or_task_id optional
status optional
repository_id optional
created_after optional
created_before optional
cursor optional
page_size bounded by configured maximum
```

No arbitrary query language, caller-defined sort expression, SQL fragment, or unbounded page is allowed.

## Shared request and response model

Required request fields:

```text
request_id
correlation_id
project_id
caller_class
service_actor_id
idempotency_key for mutations
contract_version
requested_at
```

Required response fields:

```text
request_id
correlation_id
outcome
result_record_id optional
bounded_code optional
contract_version
completed_at
```

Allowed outcomes:

```text
succeeded
replayed
rejected
conflict
not_found
recovery_required
```

Unknown request fields fail closed.

## Caller classes

Packet 016D recognizes only:

```text
owner_operator
steward_agent
implementation_agent
verification_runner
audit_agent
recovery_operator
```

Capability allowlists must match the accepted typed service contract. Caller class never overrides project scope, lifecycle, idempotency, or reference checks.

## Error contract

At minimum:

```text
project_not_found
project_scope_mismatch
repository_not_found
invalid_external_id
invalid_state_transition
terminal_record_immutable
idempotency_conflict
request_digest_mismatch
reference_not_found
reference_project_mismatch
path_invalid
path_traversal_rejected
concurrency_conflict
recovery_required
service_unavailable
```

No raw Psycopg exception, SQLSTATE, SQL text, stack trace, secret, credential, or DSN may cross the service boundary.

## Transaction and concurrency rules

- every mutation begins by creating or locking the idempotency key;
- exact retries replay the previously committed result;
- conflicting retries reject without partial writes;
- lifecycle updates compare expected current status;
- state change, audit event, and idempotency result commit atomically;
- deadlock, lock timeout, or serialization conflict becomes a bounded retryable conflict;
- unknown commit outcome returns `recovery_required`, never guessed success;
- cross-project references fail before mutation;
- no caller may provide database-native internal keys.

## Database role and migration posture

Packet 016D may add one immutable migration:

```text
0002_execution_verification_service.sql
```

It may contain only:

- service-role schema usage;
- table and sequence privileges required for the six capabilities;
- narrow indexes or constraints proven necessary by implementation tests;
- no broad grants;
- no ownership transfer;
- no destructive DDL;
- no manifest, provenance, retention, backup, telemetry, or later-slice objects.

The migration must be applied through the Packet 016C migration runner and bound by SHA-256 in the packaged manifest.

## Secret posture

- the owner sets `kaizen_ops_service` credentials outside Git;
- an owner-run short-lived process injects the service DSN;
- no service credential enters ChatGPT, an MCP tool argument, a repository file, test fixture, log, or evidence record;
- integration tests use a dedicated disposable service test role and database or an owner-injected disposable DSN;
- all secret environment variables are cleared after owner-run tests;
- logs include IDs, capability, outcome, bounded code, and latency only.

## Exact changed-path allowlist

Existing files allowed to change:

```text
pyproject.toml
src/kaizen/ops_db/__init__.py
src/kaizen/ops_db/migrations/manifest.json
docs/OPERATIONS_DATABASE_PROVING_GROUND.md
```

New source paths allowed:

```text
src/kaizen/ops_service/__init__.py
src/kaizen/ops_service/contracts.py
src/kaizen/ops_service/errors.py
src/kaizen/ops_service/repository.py
src/kaizen/ops_service/service.py
src/kaizen/ops_service/serialization.py
src/kaizen/ops_db/migrations/0002_execution_verification_service.sql
```

New test paths allowed:

```text
tests/test_ops_service_contracts.py
tests/test_ops_service_repository.py
tests/test_ops_service_service.py
tests/test_ops_service_integration.py
tests/test_ops_service_hammer.py
```

No other platform path may change without an owner-approved packet revision.

## Required tests

Unit and contract tests:

- strict request-envelope validation;
- unknown-field rejection;
- caller-capability allowlists;
- bounded error serialization;
- no secret or raw database-error leakage;
- external-ID and commit-ID validation reuse;
- bounded page-size and cursor validation;
- deterministic request digest calculation.

Live PostgreSQL integration tests:

- reserve, start, and finish lifecycle;
- each legal terminal outcome;
- every illegal transition rejected;
- exact mutation retry replays original result;
- changed payload under same idempotency key rejected;
- audit and idempotency rows commit atomically with mutations;
- injected transaction failure leaves no partial rows;
- verification outcomes remain distinct;
- project-safe attempt and verification references;
- wrong-project graph and list access rejected;
- bounded pagination and deterministic ordering;
- service role cannot mutate disallowed tables;
- service role cannot create schema, table, role, database, or extension;
- service role cannot execute migration DDL;
- no manifest or provenance write capability exists.

## Hammer requirements

At minimum:

```text
100 concurrent identical reserve requests
  expected: 1 succeeded, 99 replayed, 1 execution row, 1 logical idempotency result

100 concurrent conflicting reserve requests sharing one idempotency key
  expected: 1 succeeded, 99 idempotency conflicts, no mixed payload

100 concurrent start attempts on one reserved execution
  expected: 1 succeeded, 99 replayed or bounded stale-state conflicts according to request identity

100 concurrent terminal transitions on one started execution
  expected: exactly 1 terminal transition, all others bounded conflicts or exact replays, no terminal-state drift

100 duplicate verification submissions
  expected: 1 created, 99 replayed

100 cross-project mutation and read attempts
  expected: 100 rejected, zero leaked rows
```

Every hammer records exact expected and observed distributions.

## Required verification gates

- complete existing platform suite;
- complete new ops-service suite;
- live disposable service-role integration suite;
- all required hammer profiles;
- compileall;
- available Ruff check if configured and installed;
- static capability scan;
- changed-text integrity scan;
- exact changed-path verification;
- migration source-hash verification;
- service-role privilege inspection;
- clean platform Git state after local commit;
- no platform remote;
- no vault, Kaizen MCP, Go8, Northstar, or staging mutation.

## Implementation return

Return must include:

- starting and ending platform commits;
- exact changed paths;
- migration ID and SHA-256;
- service-role setup with secrets removed;
- capability inventory;
- request and response contract proof;
- lifecycle and idempotency proof;
- transaction and failure-injection proof;
- project-isolation proof;
- privilege proof;
- hammer distributions;
- full test totals;
- compile/lint and integrity status;
- secret-redaction proof;
- known limitations;
- explicit confirmation that manifest freeze, provenance, file hashing, backup, retention, MCP, and deployment were not implemented.

## Stop conditions

Stop on:

- platform checkpoint drift;
- need for an unapproved dependency or path;
- need to alter Packet 016C migration bytes;
- inability to keep service-role credentials outside Git and agent-visible arguments;
- need for direct SQL, a generic query language, or caller-provided SQL fragments;
- inability to prove exact retry idempotency;
- inability to enforce one terminal outcome under concurrency;
- unexplained hammer distribution;
- need to implement manifest, provenance, file, backup, retention, telemetry, MCP, or deployment scope;
- need to mutate vault, Northstar, Kaizen MCP, Go8, or staging.

## Approval gate

Implementation may begin only after the owner approves this exact packet SHA-256 against platform starting commit:

```text
1472d65d20162a07a3ca12b73f52348ed67dd291
```
