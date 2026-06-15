---
id: kz-spec-01KV6LDQ8M2N7R5C3Y9P4T6BN0
type: specification
status: proposed
project: kaizen-platform
created: 2026-06-16T01:10:00Z
updated: 2026-06-16T01:10:00Z
review_status: pending
authority: proposed
summary: "Define the non-executable physical Postgres design for the Milestone 11 four-family first slice."
---

# Milestone 11 Minimum Postgres Physical Design

## Scope and posture

This design covers only:

```text
execution attempt
verification run
return-bundle manifest
artifact provenance
```

It is design evidence only. It does not create Postgres, SQL, migrations, roles, or code.

## Database posture

```text
minimum Postgres version: 16
preferred local proving-ground version: 18 when already installed and verified
encoding: UTF8
timezone: UTC at database and service boundaries
logical database name: kaizen_ops
primary logical schema: operations
migration metadata schema: platform_meta
extensions: none required for first slice
```

No extension may be added merely for convenience. Prefixed ULIDs are generated and validated by the typed service.

## Ownership roles

Conceptual roles:

```text
kaizen_ops_owner
owns schemas and migrations; unavailable to ordinary runtime

kaizen_ops_migrator
may apply approved migrations; no ordinary application use

kaizen_ops_service
runtime least-privilege role used only by the typed service

kaizen_ops_backup
read/backup privileges required for approved backup tooling

kaizen_ops_readonly
optional operator inspection role; no agent credentials
```

Agents, MCP clients, and generic scripts receive no database credentials.

## Schema inventory

### `operations.project_ref`

Purpose: stable operational reference to a project without becoming canonical project authority.

Fields:

```text
project_id text primary key
project_slug text not null
status text not null
created_at timestamptz not null
retired_at timestamptz null
```

Constraints:

- `project_id` follows accepted project-ID contract;
- `project_slug` unique among non-retired projects;
- allowed status: active, archived, tombstoned;
- table stores no project narrative.

### `operations.repository_ref`

Purpose: scoped repository identity used by execution attempts.

Fields:

```text
repository_id text primary key
project_id text not null
repository_kind text not null
canonical_name text not null
created_at timestamptz not null
retired_at timestamptz null
```

Constraints:

- foreign key to `project_ref`;
- unique `(project_id, canonical_name)`;
- repository kind allowlist such as implementation, docs, vault, platform, evidence;
- no absolute filesystem path.

### `operations.storage_root`

Purpose: governed storage-root identity used with relative paths.

Fields:

```text
storage_root_id text primary key
project_id text null
root_kind text not null
logical_name text not null
status text not null
created_at timestamptz not null
retired_at timestamptz null
```

Constraints:

- project may be null only for approved shared roots;
- unique `(project_id, logical_name)` with null-safe design;
- no physical path stored in the durable identity table;
- machine-local resolution remains service configuration.

### `operations.execution_attempt`

Fields:

```text
execution_attempt_id text primary key
project_id text not null
packet_or_task_id text not null
repository_id text not null
starting_commit char(40) not null
allowed_scope_ref text not null
predecessor_attempt_id text null
status text not null
outcome_code text null
created_at timestamptz not null
started_at timestamptz null
finished_at timestamptz null
idempotency_key text not null
request_digest char(64) not null
created_by_service text not null
```

Constraints:

- foreign keys to project and repository;
- predecessor references same project;
- unique `(project_id, idempotency_key)`;
- unique `(project_id, execution_attempt_id)` redundant only if needed for composite project-safe references; otherwise enforce through service and foreign keys;
- SHA-1 commit shape validated as 40 lowercase hex for Git compatibility; future non-SHA-1 repositories require a separate algorithm field before acceptance;
- legal status allowlist: reserved, started, succeeded, failed, interrupted, cancelled;
- timestamp checks:
  - reserved has no start/finish;
  - started has start but no finish;
  - terminal states require finish;
- terminal state updates blocked by trigger or service-plus-constraint design chosen in Packet 016C;
- outcome code required for failed/interrupted/cancelled and optional for succeeded.

Indexes justified by accepted queries:

```text
(project_id, created_at desc)
(project_id, packet_or_task_id, created_at desc)
(project_id, repository_id, starting_commit)
(predecessor_attempt_id)
(status) partial index for non-terminal states
```

### `operations.verification_run`

Fields:

```text
verification_run_id text primary key
project_id text not null
execution_attempt_id text not null
verification_kind text not null
normalized_command_id text not null
repository_commit char(40) not null
input_set_ref text null
status text not null
started_at timestamptz not null
finished_at timestamptz not null
return_code integer null
summary_passed integer null
summary_failed integer null
summary_skipped integer null
summary_errors integer null
bounded_error_code text null
raw_evidence_storage_root_id text null
raw_evidence_relative_path text null
raw_evidence_sha256 char(64) null
idempotency_key text not null
request_digest char(64) not null
```

Constraints:

- foreign key to execution attempt with same project;
- unique `(project_id, idempotency_key)`;
- status allowlist: passed, failed, timed_out, blocked, transport_failed, interrupted;
- `finished_at >= started_at`;
- passed/failed may have return code; blocked/transport failure may not claim one unless trusted;
- raw evidence root/path/hash are all-null or all-present;
- relative path rejects absolute and traversal forms;
- summary counts non-negative;
- passed requires failed/errors zero when those values are supplied;
- no full stdout/stderr field.

Indexes:

```text
(execution_attempt_id, started_at)
(project_id, verification_kind, started_at desc)
(project_id, repository_commit, status)
(status, started_at desc)
```

### `operations.return_manifest`

Fields:

```text
return_manifest_id text primary key
project_id text not null
execution_attempt_id text not null
contract_version text not null
manifest_version integer not null
predecessor_manifest_id text null
status text not null
frozen_at timestamptz not null
repository_commit char(40) not null
inventory_digest char(64) not null
required_item_count integer not null
present_item_count integer not null
missing_item_count integer not null
extra_item_count integer not null
audit_record_id text null
idempotency_key text not null
request_digest char(64) not null
```

Constraints:

- unique `(execution_attempt_id, manifest_version)`;
- unique `(project_id, idempotency_key)`;
- version starts at 1;
- predecessor, when present, points to same attempt and lower version;
- status allowlist: frozen, audited_pass, audited_fail;
- counts non-negative;
- `present + missing >= required` contract validated by service because extras and optional items complicate a universal SQL equation;
- frozen row immutable except one-way audit status and audit-record reference transition if accepted design keeps audit status here; preferred design stores audit linkage append-only in separate table to preserve manifest immutability.

### `operations.return_manifest_entry`

Fields:

```text
return_manifest_id text not null
entry_ordinal integer not null
entry_kind text not null
storage_root_id text not null
relative_path text not null
sha256 char(64) not null
byte_length bigint not null
media_type text null
requirement_status text not null
verification_run_id text null
primary key (return_manifest_id, entry_ordinal)
```

Constraints:

- unique `(return_manifest_id, storage_root_id, relative_path)`;
- non-negative byte length;
- requirement status allowlist: required_present, optional_present, unexpected_present;
- missing required items are represented separately in `return_manifest_missing_item`, not as fake file rows;
- relative path normalized and traversal-safe.

### `operations.return_manifest_missing_item`

Fields:

```text
return_manifest_id text not null
missing_ordinal integer not null
requirement_key text not null
expected_kind text not null
expected_storage_root_id text null
expected_relative_path text null
primary key (return_manifest_id, missing_ordinal)
```

Purpose: preserve exact missing-evidence findings without inventing file hashes.

### `operations.return_manifest_audit_link`

Fields:

```text
return_manifest_id text not null
link_id text primary key
canonical_record_id text not null
link_kind text not null
created_at timestamptz not null
```

Allowlist link kinds: audit_pass, audit_fail, owner_acceptance, completion_record.

This append-only table is preferred over mutating the frozen manifest.

### `operations.logical_artifact`

Fields:

```text
logical_artifact_id text primary key
project_id text not null
artifact_role text not null
created_at timestamptz not null
retired_at timestamptz null
```

Unique `(project_id, artifact_role)` unless the role contract later requires namespace/version dimensions.

### `operations.artifact_byte_object`

Fields:

```text
artifact_byte_object_id text primary key
project_id text not null
sha256 char(64) not null
byte_length bigint not null
media_type text null
storage_root_id text not null
relative_path text not null
file_state text not null
observed_at timestamptz not null
last_verified_at timestamptz null
```

Constraints:

- unique `(project_id, sha256, byte_length, storage_root_id, relative_path)`;
- file state allowlist: file_pending, file_verified, file_missing, file_hash_mismatch, reference_unresolvable;
- file_verified requires last_verified_at;
- bytes remain external files.

### `operations.provenance_assertion`

Fields:

```text
provenance_assertion_id text primary key
project_id text not null
logical_artifact_id text not null
artifact_byte_object_id text not null
execution_attempt_id text null
verification_run_id text null
repository_commit char(40) not null
input_set_ref text null
normalized_command_id text null
assertion_status text not null
supersedes_assertion_id text null
observed_at timestamptz not null
idempotency_key text not null
request_digest char(64) not null
```

Constraints:

- one or both producer references may be present according to service contract; at least one required;
- all referenced records share project ID;
- unique `(project_id, idempotency_key)`;
- assertion status: active, superseded, rejected;
- supersedence points to same logical artifact and byte object or explicitly records correction scope;
- prior assertion never overwritten.

Indexes:

```text
(logical_artifact_id, observed_at desc)
(artifact_byte_object_id)
(execution_attempt_id)
(verification_run_id)
(project_id, repository_commit)
```

### `operations.idempotency_record`

Purpose: service-level exact retry binding across capabilities.

Fields:

```text
project_id text not null
capability text not null
idempotency_key text not null
request_digest char(64) not null
result_kind text not null
result_record_id text null
error_code text null
created_at timestamptz not null
expires_at timestamptz null
primary key (project_id, capability, idempotency_key)
```

Rules:

- exact retry with same digest returns prior result;
- different digest is conflict;
- authority-linked operations may have no expiry;
- ordinary records retain idempotency binding at least through their retry horizon.

### `operations.service_audit_event`

Purpose: bounded machine audit of first-slice service mutations, not replacement for canonical governance events.

Fields:

```text
service_audit_event_id text primary key
project_id text not null
capability text not null
record_type text not null
record_id text not null
outcome text not null
bounded_code text null
service_actor text not null
occurred_at timestamptz not null
correlation_id text null
```

No arbitrary payload body.

## Immutable enforcement strategy

Preferred:

- SQL constraints for field and relation validity;
- triggers only for narrow immutable-row and legal-transition enforcement where constraints cannot express old/new comparisons;
- typed service for higher-order contract checks;
- append-only link/assertion tables instead of updating frozen evidence rows.

Triggers must remain small, deterministic, versioned through migrations, and directly tested.

## Project isolation

All child records carry `project_id`, even where derivable, to allow composite foreign keys and fail-closed project-safe joins.

Physical design should use composite unique keys and composite foreign keys where necessary so a child cannot reference a parent in another project.

Row-level security is deferred from the first local single-operator slice unless Packet 016B audit proves it materially improves safety without obscuring service logic. Project isolation must still be enforced through composite keys and service filters.

## Query limits

Typed list operations require:

- explicit project ID;
- bounded date or cursor range;
- maximum page size;
- allowlisted sort order;
- no arbitrary SQL filters;
- no cross-project portfolio query in first slice.

## Migration metadata

`platform_meta.schema_version` conceptually records:

```text
version
migration_id
applied_at
source_sha256
application_version
```

Migration files, when later authorized, must be immutable after application.

## Design conclusion

The first slice is relational because identities, legal transitions, exact retry bindings, manifests, and provenance relationships are repeatedly queried and require integrity constraints. JSON may hold bounded non-query-critical summaries only after explicit review; it must not become a dumping ground for governed fields.