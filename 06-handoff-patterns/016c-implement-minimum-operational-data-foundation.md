---
id: kz-tp-01KV6LHQ8M2N7R5C3Y9P4T6BS0
type: task-packet
status: ready-for-approval
project: kaizen-platform
created: 2026-06-16T01:50:00Z
updated: 2026-06-16T16:25:00Z
review_status: passed
authority: proposed
summary: "Implement the database and immutable-migration proving ground for the Milestone 11 first slice."
---

# Task Packet 016C - Implement Database and Migration Proving Ground

## Authority

Ready for exact-hash owner approval. This packet is not yet approved and may not be executed.

Live preflight binding:

```text
result:
03-research-results/265-packet-016c-live-postgresql-preflight.md

result SHA-256:
1591d8d2ee5c903ab9d695c3a3d0ace24ede264686b153639ae6a2a0e9dd145d

PostgreSQL service:
postgresql-x64-18

PostgreSQL server:
18.4

psql.exe SHA-256:
1116C77F820606F52CD3D0F676012470D494092CBA321A6CBD898F4701EB944E

platform starting commit:
286b8273498e04a77730d6bdbeaadb2b1c7d2d6b

secret source:
Windows Credential Manager via owner-run short-lived process injection
```

Implementation remains prohibited until the owner approves this exact packet source hash.

## Objective

Implement only the database, roles, immutable migration framework, and physical first-slice schema required for later typed-service work.

Packet 016C does **not** implement operational service capabilities, artifact hashing, manifest freeze workflows, backup automation, retention execution, MCP tools, or production deployment.

## Starting checkpoint

```text
repository:
C:\dev\kaizen\platform

branch:
main

required starting commit:
286b8273498e04a77730d6bdbeaadb2b1c7d2d6b

required working tree:
clean

required remotes:
none
```

Stop on checkpoint drift unless the owner approves a revised packet binding.

## Database target

```text
minimum supported major version:
PostgreSQL 16

implementation target:
PostgreSQL 18, subject to live binary and service verification before approval

database:
kaizen_ops

operational authority domain:
Kaizen Core Operational

schemas:
platform_meta
operations

encoding:
UTF8

timezone:
UTC

data checksums:
required on

password encryption for newly created credentials:
scram-sha-256
```

If PostgreSQL 18 cannot be verified locally, stop and revise the packet rather than silently falling back.

## Driver and migration posture

```text
Python driver:
psycopg 3

planned dependency:
psycopg[binary]>=3.2,<4

ORM:
none

migration framework:
small Kaizen-owned immutable migration runner over reviewed SQL files
```

Reasons:

- the schema is small and deliberately relational;
- raw parameterized SQL remains inside a typed repository boundary, never agent-facing;
- avoiding an ORM keeps migration and constraint behavior explicit;
- migration files can be hashed, ordered, and audited without framework magic.

The runner must reject modified applied migrations and interrupted or out-of-order state.

## Git commit identity correction

The accepted implementation uses:

```text
commit_algorithm text
commit_digest text
```

It must not hard-code `char(40)` as universal commit identity.

The first implementation accepts `sha1` with 40 lowercase hexadecimal characters. The contract remains extensible to later algorithms through an allowlisted algorithm/digest validator.

## Secret and credential posture

- no credentials, DSNs, passwords, or secret files enter Git;
- the service and migration runner read one process-injected connection reference;
- owner-managed credential storage remains outside the repository;
- local launcher or operator environment injects the runtime secret;
- logs and errors redact passwords and connection strings;
- tests use a dedicated disposable local test database and least-privilege test role;
- agents and MCP tools receive no database credentials;
- `.env` files are not introduced by this packet.

The implementation-readiness audit must verify the exact local secret source and injection command before approval. Preferred Windows posture is owner-managed Windows Credential Manager or equivalent owner-controlled secret store feeding a short-lived process environment.

## Exact changed-path allowlist

Existing files allowed to change:

```text
pyproject.toml
src/kaizen/__init__.py
```

New source paths allowed:

```text
src/kaizen/ops_db/__init__.py
src/kaizen/ops_db/config.py
src/kaizen/ops_db/connection.py
src/kaizen/ops_db/errors.py
src/kaizen/ops_db/identifiers.py
src/kaizen/ops_db/migration_runner.py
src/kaizen/ops_db/schema_contract.py
src/kaizen/ops_db/migrations/0001_initial_operations.sql
src/kaizen/ops_db/migrations/manifest.json
```

New test paths allowed:

```text
tests/test_ops_db_config.py
tests/test_ops_db_identifiers.py
tests/test_ops_db_migration_runner.py
tests/test_ops_db_schema_contract.py
tests/test_ops_db_migration_hammer.py
```

New operator documentation allowed:

```text
docs/OPERATIONS_DATABASE_PROVING_GROUND.md
```

No other path may change without an owner-approved packet revision.

## Required implementation

### 1. Configuration and connection boundary

- parse the process-injected connection reference without logging secrets;
- set UTC session posture;
- enforce expected database identity;
- expose no generic execution capability;
- provide bounded connection and version errors.

### 2. External-ID and commit-ID validation

- typed prefixed ULID validation for first-slice external IDs;
- algorithm-plus-digest validation;
- allow `sha1` initially;
- reject unknown algorithms and malformed digests.

### 3. Immutable migration runner

- discover only packaged allowlisted migration files;
- verify manifest order and SHA-256;
- acquire one database migration lock;
- apply migrations transactionally where Postgres permits;
- record migration ID, source SHA-256, applied time, and platform version;
- reject changed applied migration;
- reject gaps, duplicates, and out-of-order state;
- recover deterministically after interruption;
- provide status inspection without mutation.

### 4. Initial physical schema

Implement only the accepted tables and constraints required by the physical design:

```text
platform_meta.schema_version
operations.project_ref
operations.repository_ref
operations.storage_root
operations.execution_attempt
operations.verification_run
operations.return_manifest
operations.return_manifest_entry
operations.return_manifest_missing_item
operations.return_manifest_audit_link
operations.logical_artifact
operations.artifact_byte_object
operations.provenance_assertion
operations.idempotency_record
operations.service_audit_event
```

Narrow immutable-row and legal-transition triggers are allowed only where declarative constraints cannot enforce old/new behavior.

### 5. Role/bootstrap evidence

The migration SQL may define required grants and ownership expectations, but implementation must not embed owner passwords. Local role and database creation commands remain operator-executed and separately documented.

## Required tests

- empty database migration to current version;
- migration status before and after apply;
- exact migration replay is idempotent;
- changed applied migration rejected;
- missing, duplicate, or out-of-order migration rejected;
- interruption and restart at each migration boundary;
- concurrent migration runners: exactly one applies, others observe terminal state;
- all tables, constraints, indexes, grants, triggers, and schema-version rows match the reviewed contract;
- commit algorithm/digest validation;
- external-ID validation;
- secret redaction;
- connection to wrong database rejected;
- unsupported Postgres major version rejected;
- no cross-project foreign-key relationship can be committed;
- no artifact-byte column exists;
- no canonical Markdown body column exists.

## Hammer requirements

At minimum:

```text
100 concurrent migration-lock rounds
100 exact migration replay rounds
100 changed-manifest rejection rounds
100 cross-project foreign-key rejection rounds
```

Every profile records exact expected and observed distributions.

## Required verification gates

- full existing platform test suite;
- complete new ops-db suite;
- compileall;
- available Ruff check if configured and installed;
- static capability scan;
- changed-text integrity scan;
- exact changed-path allowlist verification;
- migration SQL review and source hashes;
- disposable database create/migrate/drop proof;
- clean platform Git state after local commit;
- no platform remote.

## Implementation return

Return must include:

- starting and ending platform commits;
- exact changed paths;
- Postgres client/server versions;
- database and role setup commands with secrets removed;
- migration manifest and hashes;
- schema inventory and constraint proof;
- test totals and hammer distributions;
- compile/lint status;
- secret-redaction proof;
- known limitations;
- confirmation that no typed service, MCP tool, artifact data, or canonical state was implemented.

## Stop conditions

Stop on:

- unverifiable PostgreSQL 18 runtime;
- need for an unapproved dependency or path;
- secret storage requiring repository files;
- non-transactional migration ambiguity;
- inability to enforce project-safe relationships;
- unexplained test or hammer result;
- requirement to mutate vault, Northstar, Kaizen MCP, or Go8;
- need to widen into service implementation.

## Explicit exclusions

- execution/verification service capabilities;
- manifest-freeze workflow;
- artifact hashing or provenance service;
- backup automation;
- retention deletion;
- governed-operation read model;
- connector telemetry;
- Observatory, ingestion, Qdrant, IMI, customer data, or multi-user identity;
- production MCP architecture;
- deployment;
- direct agent SQL;
- moving Markdown or artifact bytes into Postgres.
