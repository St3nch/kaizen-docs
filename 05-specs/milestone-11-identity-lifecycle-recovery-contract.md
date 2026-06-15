---
id: kz-spec-01KV6L9Q8M2N7R5C3Y9P4T6BJ0
type: specification
status: proposed
project: kaizen-platform
created: 2026-06-16T00:30:00Z
updated: 2026-06-16T00:30:00Z
review_status: pending
authority: proposed
summary: "Define conceptual identity, lifecycle, transaction, idempotency, correction, recovery, retention, and trust contracts for the Milestone 11 first slice."
---

# Milestone 11 Identity, Lifecycle, and Recovery Contract

## Scope

This contract covers only:

```text
execution attempt
verification run
return-bundle manifest
artifact provenance
```

It is a conceptual and behavioral contract. It does not define physical tables, columns, indexes, SQL, migrations, or service implementation.

## Global rules

1. Every record has one typed, prefixed, globally unique external ID.
2. Every record is explicitly project-scoped.
3. IDs do not derive from mutable file paths.
4. Retry, rerun, correction, and supersedence create new IDs.
5. Terminal history is never silently rewritten.
6. Human verdict and owner acceptance remain canonical Markdown.
7. Artifact and evidence bytes remain files.
8. Database records may reference but never replace Git, governance JSONL, staging evidence, or canonical Markdown authority.
9. Unknown state fails closed.
10. Every mutating operation supports an idempotency key scoped to project, capability, and logical request.

## External ID families

Recommended prefixes:

```text
kz-exec-<ULID>   execution attempt
kz-vrun-<ULID>   verification run
kz-rman-<ULID>   return-bundle manifest
kz-art-<ULID>    logical artifact
kz-byte-<ULID>   immutable artifact byte object
kz-prov-<ULID>   provenance assertion
```

The exact prefixes may be revised in Packet 016B, but the distinction among logical artifacts, byte objects, and provenance assertions is mandatory.

## Execution attempt contract

### Required conceptual fields

```text
execution_attempt_id
project_id
packet_or_task_id
repository_id
starting_commit
allowed_scope_reference
predecessor_attempt_id when retrying
status
created_at
started_at
finished_at
bounded_outcome_code
idempotency_key
```

### Legal lifecycle

```text
reserved -> started
reserved -> cancelled
started -> succeeded
started -> failed
started -> interrupted
started -> cancelled
```

Illegal examples:

```text
succeeded -> failed
failed -> succeeded
cancelled -> started
interrupted -> succeeded
```

A new attempt is required after any terminal state.

### Meaning of success

`succeeded` means the bounded execution completed according to the executor's contract. It does not mean tests passed, audit passed, owner accepted, or canonical return completed.

## Verification run contract

### Required conceptual fields

```text
verification_run_id
project_id
execution_attempt_id
verification_kind
normalized_command_identity
repository_commit
input_set_reference
status
started_at
finished_at
return_code when available
summary_counts
bounded_error_code
raw_evidence_storage_reference when retained
idempotency_key
```

### Legal lifecycle

```text
started -> passed
started -> failed
started -> timed_out
started -> blocked
started -> transport_failed
started -> interrupted
```

### Classification rule

- `blocked` means the responsible test or platform did not receive the request.
- `transport_failed` means delivery or result transport failed without a trusted terminal test result.
- `timed_out` means no trusted terminal result was returned before the bounded deadline.
- `failed` means the verification process ran and returned a failing result.

These states may not be collapsed.

## Return-bundle manifest contract

### Required conceptual fields

```text
return_manifest_id
project_id
execution_attempt_id
contract_version
manifest_version
predecessor_manifest_id when replacing
status
frozen_at
repository_commit
inventory_digest
required_item_count
present_item_count
missing_item_count
extra_item_count
evidence_entries
verification_run_references
audit_record_reference when available
idempotency_key
```

### Lifecycle

```text
draft -> frozen
frozen -> audited_pass
frozen -> audited_fail
```

A frozen manifest is immutable. Adding evidence or correcting inventory requires a new manifest version linked to the predecessor.

### Atomic freeze

Freeze succeeds only when:

- the complete inventory is enumerated;
- every retained file has a verified digest;
- required, missing, and extra findings are computed;
- repository and attempt bindings match;
- all manifest metadata and evidence entries commit together.

Partial freeze is not a valid state.

## Artifact provenance contract

### Logical artifact

Represents a role across versions or runs, such as `reorder-report`.

### Byte object

Represents one immutable file instance identified by a cryptographic digest, byte length, media type, and storage reference.

### Provenance assertion

Binds one byte object to:

```text
logical artifact
producing execution attempt or verification run
repository commit
input and fixture references
normalized command identity
observation time
hash verification result
canonical audit or acceptance references when available
```

### Correction

An incorrect assertion is superseded by a new assertion. The original remains preserved with a correction relationship.

## Storage reference contract

Durable references use:

```text
storage_root_id
relative_path
```

Requirements:

- storage roots are governed reference identities;
- relative paths must be normalized and traversal-safe;
- absolute paths are prohibited in durable identity fields;
- storage-root resolution occurs inside the typed service;
- references cannot grant access outside the configured root;
- a missing file creates an explicit missing-artifact observation, not silent deletion.

## Idempotency contract

Each mutating capability accepts an idempotency key.

The service must:

1. bind the key to project, capability, actor/service identity, and normalized request digest;
2. return the original result for an exact retry;
3. reject reuse with different request content;
4. preserve terminal errors when replaying exact failed requests where safe;
5. never create duplicate execution attempts, verification runs, manifest freezes, or provenance assertions for one exact logical request.

Idempotency retention must be at least as long as the retry horizon for the associated operation.

## Transaction boundaries

### Start or finish attempt

The lifecycle transition and required structured audit observation commit atomically.

### Record verification

The terminal verification metadata and evidence reference commit atomically. Raw file creation may occur separately but must be hash-verified before the record claims the evidence exists.

### Freeze manifest

Manifest metadata, inventory entries, per-file digests, and completeness findings commit atomically.

### Record provenance

Byte-object observation and provenance assertion may commit together when the byte object is new. Existing byte objects may receive additional assertions without mutation.

## File/database consistency states

Allowed explicit states:

```text
file_pending
file_verified
file_missing
file_hash_mismatch
reference_unresolvable
```

No record may claim `file_verified` without reading and hashing the exact referenced bytes.

## Recovery rules

1. Recovery starts from authoritative database state plus referenced file and Git facts.
2. An attempt left `started` after a service crash becomes `interrupted` only through a governed recovery action with evidence.
3. Unknown terminal outcome remains unresolved or interrupted; it is never guessed as succeeded.
4. A manifest draft may be abandoned; a frozen manifest may not be edited.
5. A database record referencing missing bytes remains preserved with an explicit missing state.
6. Restored files with no matching provenance record are unregistered artifacts until explicitly reconciled.
7. Restored database provenance with missing files triggers a consistency incident.
8. Recovery actions create new audit observations and never erase the original failure state.

## Retention and deletion contract

### Class A

Permanent unless an extraordinary owner-approved legal or security process establishes otherwise.

Includes:

- frozen manifests;
- rejected return history;
- provenance referenced by accepted audit or owner decisions.

### Class B

Project lifetime plus 12 months, subject to holds.

Includes ordinary execution attempts, verification runs, and unaccepted provenance.

### Deletion

- runs only through a typed governed service;
- checks class, holds, canonical references, and relationship integrity;
- preserves project tombstones and Class A lineage;
- never cascade-deletes canonical Markdown, governance JSONL, Git history, immutable staging evidence, or evidence bytes merely because operational metadata expires;
- produces a deletion result record and bounded counts.

## Privacy contract

Allowed structured content:

```text
IDs
hashes
bounded status and error codes
timestamps
summary counts
repository commit references
scoped storage references
normalized verification kinds and command identities
```

Prohibited by default:

```text
prompts
conversation text
source or artifact bytes
full stdout/stderr
secrets or credentials
environment dumps
customer payloads
arbitrary tool arguments
```

## Typed service contract requirements

Every service capability must declare:

- caller class;
- consequence class;
- project scope;
- accepted lifecycle states;
- idempotency behavior;
- transaction boundary;
- emitted audit observation;
- bounded error taxonomy;
- response shape;
- privacy filtering;
- retry safety.

## Required physical-design proofs

Packet 016B must prove that a future physical design can enforce this contract through constraints, transactions, typed service behavior, migrations, and tests without shifting canonical authority into Postgres.