---
id: kz-spec-01KV6LEQ8M2N7R5C3Y9P4T6BP0
type: specification
status: proposed
project: kaizen-platform
created: 2026-06-16T01:20:00Z
updated: 2026-06-16T01:20:00Z
review_status: pending
authority: proposed
summary: "Define the non-executable typed service contract for the Milestone 11 first-slice operational data foundation."
---

# Milestone 11 Typed Service Contract

## Boundary

The service is the only runtime component allowed to mutate the first-slice operational database.

```text
agent or operator
-> typed local service
-> Postgres

agent or operator
-> no direct SQL
-> no database credentials
```

The service references canonical Markdown, Git, governance JSONL, immutable staging evidence, and artifact files but cannot rewrite their authority.

## Caller classes

```text
owner_operator
steward_agent
implementation_agent
verification_runner
audit_agent
recovery_operator
retention_operator
```

Every capability has an allowlist of caller classes. Caller class is not sufficient by itself; project scope and consequence checks also apply.

## Shared request envelope

Conceptual request fields:

```text
request_id
correlation_id
project_id
caller_class
service_actor_id
idempotency_key
contract_version
requested_at
payload
```

Rules:

- request and correlation IDs use typed prefixed ULIDs;
- project ID is mandatory for all first-slice capabilities;
- payload is a typed object, not arbitrary JSON;
- service rejects unknown fields by default;
- idempotency key is mandatory for mutations;
- all timestamps normalize to UTC;
- no caller supplies database-generated internal keys.

## Shared response envelope

```text
request_id
correlation_id
outcome
result_record_id
bounded_code
contract_version
completed_at
```

Outcomes:

```text
succeeded
replayed
rejected
conflict
not_found
recovery_required
```

No raw SQL error, stack trace, secret, credential, or unrestricted payload is returned.

## Error taxonomy

Common bounded errors:

```text
project_not_found
project_scope_mismatch
repository_not_found
storage_root_not_found
invalid_external_id
invalid_state_transition
terminal_record_immutable
idempotency_conflict
request_digest_mismatch
reference_not_found
reference_project_mismatch
path_invalid
path_traversal_rejected
file_missing
file_hash_mismatch
manifest_incomplete
manifest_already_frozen
provenance_conflict
canonical_reference_invalid
retention_hold_active
retention_not_due
recovery_required
concurrency_conflict
service_unavailable
```

The contract must distinguish caller error, conflict, authority rejection, and infrastructure failure.

## Capability: `reserve_execution_attempt`

Caller classes:

```text
owner_operator
steward_agent
implementation_agent
```

Input:

```text
project_id
packet_or_task_id
repository_id
starting_commit
allowed_scope_ref
predecessor_attempt_id optional
idempotency_key
```

Preconditions:

- project and repository active;
- repository belongs to project;
- starting commit shape valid;
- predecessor, when present, belongs to project and is terminal;
- allowed scope reference resolves to approved planning authority.

Transaction:

- create attempt in reserved state;
- create service audit event;
- persist idempotency result;
- commit atomically.

## Capability: `start_execution_attempt`

Input:

```text
execution_attempt_id
expected_status: reserved
idempotency_key
```

Effect:

```text
reserved -> started
```

Rejects stale expected status and terminal attempts.

## Capability: `finish_execution_attempt`

Input:

```text
execution_attempt_id
expected_status: started
terminal_status
bounded_outcome_code optional
idempotency_key
```

Allowed terminal statuses:

```text
succeeded
failed
interrupted
cancelled
```

Success means executor completion only, not verification or acceptance.

## Capability: `record_verification_run`

Caller classes:

```text
verification_runner
audit_agent
recovery_operator
```

Input:

```text
execution_attempt_id
verification_kind
normalized_command_id
repository_commit
input_set_ref optional
status
started_at
finished_at
return_code optional
summary_counts optional
bounded_error_code optional
raw_evidence_reference optional
idempotency_key
```

Rules:

- execution attempt exists and belongs to project;
- status is already terminal for this one run;
- blocked, timed-out, transport-failed, and failed are distinct;
- evidence reference, when present, is normalized, read, and hash-verified before `file_verified` is claimed;
- raw output remains a file.

Transaction:

- create verification row;
- create or verify byte-object reference for raw evidence when present;
- create audit event and idempotency result;
- commit atomically where all database facts share the boundary.

## Capability: `freeze_return_manifest`

Caller classes:

```text
audit_agent
owner_operator
recovery_operator
```

Input:

```text
execution_attempt_id
contract_version
manifest_version
predecessor_manifest_id optional
expected_repository_commit
required_item_contract
present_evidence_entries
missing_requirement_entries
verification_run_ids
idempotency_key
```

Preconditions:

- execution attempt terminal;
- repository binding matches;
- every present file reference resolves under an approved storage root;
- exact bytes are read and hashed;
- no duplicate root/path entry;
- predecessor belongs to same attempt and has lower version;
- all verification runs belong to same attempt and project.

Transaction:

- validate complete inventory;
- calculate deterministic inventory digest;
- write manifest, entries, missing items, audit event, and idempotency result;
- commit atomically;
- return frozen manifest ID and digest.

No draft manifest is exposed as accepted evidence.

## Capability: `link_manifest_authority_record`

Caller classes:

```text
owner_operator
audit_agent
steward_agent
```

Input:

```text
return_manifest_id
canonical_record_id
link_kind
idempotency_key
```

Creates append-only linkage to an audit, owner acceptance, or completion record. Does not mutate the frozen manifest.

## Capability: `record_artifact_provenance`

Caller classes:

```text
verification_runner
audit_agent
implementation_agent
```

Input:

```text
logical_artifact_id or create-role request
storage_root_id
relative_path
expected_sha256 optional
execution_attempt_id optional
verification_run_id optional
repository_commit
input_set_ref optional
normalized_command_id optional
supersedes_assertion_id optional
idempotency_key
```

Rules:

- at least one producing attempt or verification run;
- all references share project;
- exact bytes are read and hashed;
- supplied expected hash, when present, must match;
- logical artifact and byte-object identities remain distinct;
- supersedence never deletes prior assertion.

Transaction:

- resolve or create logical artifact;
- resolve or create byte object;
- create provenance assertion;
- update file verification observation;
- create audit and idempotency records;
- commit atomically.

## Capability: `verify_artifact_reference`

Read/inspection capability that re-hashes exact bytes and records a new verification observation. It may change file-state metadata but not provenance history.

## Capability: `get_execution_evidence_graph`

Caller classes:

```text
owner_operator
steward_agent
audit_agent
recovery_operator
```

Returns bounded graph:

```text
attempt
verification runs
return manifests and entries
artifact byte objects
provenance assertions
canonical authority links
```

Requirements:

- explicit project ID and attempt ID;
- project-safe joins only;
- no raw file contents;
- bounded collection sizes;
- stable cursor or deterministic ordering.

## Capability: `list_project_attempts`

Filters:

```text
project_id required
packet_or_task_id optional
status optional
repository_id optional
created_after optional
created_before optional
cursor optional
page_size <= configured maximum
```

No arbitrary query language.

## Capability: `inspect_consistency`

Caller classes:

```text
audit_agent
recovery_operator
owner_operator
```

Checks:

- project/reference integrity;
- terminal state timestamp consistency;
- file existence and hash state;
- manifest inventory digest;
- provenance links;
- missing storage roots;
- expired but held records;
- orphaned records.

Read-only unless a separate recovery capability is invoked.

## Capability: `plan_retention_deletion`

Creates a dry-run plan only.

Input:

```text
project_id
as_of
requested_classes
reason
```

Output:

- exact candidate counts by record family;
- hold and canonical-reference blocks;
- retained Class A counts;
- tombstone effects;
- deterministic plan digest.

## Capability: `execute_retention_deletion`

Not part of the first implementation packet unless separately approved. Physical design may define it, but implementation remains a later gate because deletion is consequential.

## Capability: `recover_interrupted_attempt`

Requires recovery operator and explicit evidence.

Allowed effect:

```text
started -> interrupted
```

It may not mark unknown work succeeded. Recovery emits a structured observation and references durable evidence supporting the transition.

## Service concurrency posture

- use transactions with explicit row locking or optimistic expected-state updates;
- lifecycle mutations compare expected current status;
- idempotency record created or locked before side-effecting mutation;
- manifest freeze uses one transaction and deterministic inventory digest;
- duplicate provenance assertions resolve through idempotency and unique constraints;
- deadlock and serialization failures return bounded retryable conflict codes.

## Privacy and logging

Logs may include:

```text
request ID
correlation ID
project ID
typed capability
record IDs
outcome
bounded code
latency
```

Logs may not include:

```text
payload bodies
prompts
secrets
credentials
raw stdout/stderr
artifact contents
customer data
absolute paths
```

## Service independence from MCP

The contract is transport-neutral. A later MCP adapter may expose a subset, but the service must be testable and operable without MCP. MCP cannot widen the service's authority or error taxonomy.

## Design conclusion

The typed service is deliberately narrow. It turns accepted lifecycle and provenance rules into bounded operations while keeping human authority and immutable evidence outside the database.