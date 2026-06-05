# Spec - Promotion Event Schema

Status: draft
Date: 2026-06-04
Related decision: `04-design-decisions/0007-foundation-resolution-for-v0.2.md`

## Purpose

Define the append-only event contract used when staged Kaizen content is promoted into the canonical vault.

Promotion events are operational governance records. They do not replace the promoted Markdown note or Git history.

## Canonical log path

```text
_governance/promotion-log.jsonl
```

Each line contains exactly one JSON object.

## Event actions

Initial action vocabulary:

```text
promote
amend
supersede
correct
rollback
```

Definitions:

- `promote` - staged content becomes canonical for the first time
- `amend` - an already canonical governed note is changed through an approved amendment
- `supersede` - a new canonical note replaces the binding force of an earlier note while preserving history
- `correct` - fixes a prior event record without deleting or editing it
- `rollback` - records a governed reversal after a failed or invalid promotion/amendment

Agents may draft content or proposals associated with these actions. Only a human-controlled workflow may append an authority-bearing promotion event.

## Minimum event shape

Each JSONL line is one event record. A promotion workflow uses multiple records with unique `event_id` values. The intent event ID also becomes the shared `operation_id` for the workflow.

Intent example:

```json
{
  "schema_version": "1.0",
  "event_id": "kz-prom-01JXEVENT...",
  "operation_id": "kz-prom-01JXEVENT...",
  "event_phase": "intent",
  "action": "promote",
  "note_id": "kz-spec-01JX...",
  "note_type": "spec",
  "project": "kaizen",
  "source_path": "C:/dev/kaizen-staging/specs/example.md",
  "destination_path": "projects/kaizen/specs/example.md",
  "content_sha256": "hex-sha256",
  "prior_review_status": "pending",
  "new_review_status": "approved",
  "prior_authority": "proposed",
  "new_authority": "accepted",
  "approved_by": "human-actor-id",
  "approved_at": "2026-06-04T23:30:00Z",
  "validation_run_id": "kz-val-01JX...",
  "basis": "Passed audit kz-aud-01JX..."
}
```

Committed example:

```json
{
  "schema_version": "1.0",
  "event_id": "kz-prom-01JXCOMMIT...",
  "operation_id": "kz-prom-01JXEVENT...",
  "event_phase": "committed",
  "action": "promote",
  "intent_event_id": "kz-prom-01JXEVENT...",
  "note_id": "kz-spec-01JX...",
  "note_type": "spec",
  "project": "kaizen",
  "destination_path": "projects/kaizen/specs/example.md",
  "content_sha256": "hex-sha256",
  "approved_by": "human-actor-id",
  "approved_at": "2026-06-04T23:30:00Z",
  "validation_run_id": "kz-val-01JX...",
  "basis": "Passed audit kz-aud-01JX..."
}
```
## Required fields for every event

| Field | Type | Rule |
|---|---|---|
| `schema_version` | string | initial value `1.0` |
| `event_id` | string | unique immutable ID for this JSONL line |
| `operation_id` | string | equals the intent event ID; copied unchanged to all later records in the workflow |
| `event_phase` | enum | `intent`, `committed`, `failed`, or `recovered` |
| `action` | enum | one of the approved action values |
| `note_id` | string | stable Kaizen note ID |
| `note_type` | enum | registered note type |
| `project` | string | lowercase kebab-case project slug |
| `destination_path` | string | canonical vault-relative path; never an absolute canonical path in the stored event |
| `content_sha256` | string | approved content hash for `intent`; verified canonical hash for terminal phases |
| `approved_by` | string | human actor identifier |
| `approved_at` | string | ISO-8601 UTC timestamp |
| `validation_run_id` | string | successful validation run ID |
| `basis` | string | concise approval basis and audit/decision references |

## Event-phase requirements

### `event_phase: intent`

Required:

- `source_path`
- approved `destination_path`
- approved `content_sha256`
- approval and validation evidence

### `event_phase: committed`

Required:

- `intent_event_id`
- verified canonical `content_sha256`
- the same `operation_id`, action, note, project, and destination as the intent

### `event_phase: failed`

Required:

- `intent_event_id`
- `failure_code`
- `failure_summary`
- observed filesystem state

### `event_phase: recovered`

Required:

- `intent_event_id`
- `recovery_action`
- resulting filesystem state
- human actor when recovery changes canonical or staged content
## Conditional fields

### `promote`

Required:

- `source_path`
- `prior_review_status`
- `new_review_status`
- `prior_authority`
- `new_authority`

### `amend`

Required:

- `prior_content_sha256`
- `content_sha256`
- `change_summary`
- prior and new review/authority values when changed

### `supersede`

Required:

- `supersedes_note_id`
- `superseded_note_event_id`
- `supersedence_rationale`
- approval posture at least equal to the superseded record

### `correct`

Required:

- `corrects_event_id`
- `correction_reason`
- `corrected_fields`

A correction event does not mutate the earlier line.

### `rollback`

Required:

- `rolls_back_event_id`
- `rollback_reason`
- `repository_state_before`
- `repository_state_after`

## Review-context rules

`review_context` is required only when approval depends on repository or external implementation state. It may contain:

- `governing_ids`
- `repository`
- `ref`
- `reviewed_paths`
- `reviewed_interfaces`
- `dependency_versions`
- `hammer_run_ids`

The context must be scoped. Do not bind an approval to every unrelated repository commit.

## Path rules

- `destination_path` is vault-relative and uses forward slashes.
- `source_path` may be an absolute local staging path for early v0.2 operation, but it must not be treated as portable identity.
- Stored canonical paths must never contain `..` traversal.
- Paths must not point into `_governance/` except for governance artifacts explicitly allowed by policy.

## Transition rules

Initial legal authority-bearing promotion transition:

```text
review_status: pending -> approved
authority: proposed -> accepted
```

Other allowed transitions depend on note type and action.

The event validator must reject:

- agent identifiers in `approved_by`
- approval timestamps before the referenced validation run
- `authority: accepted` with `review_status` other than `approved`
- a note type that cannot carry accepted authority
- changed content whose hash no longer matches the approved content
- missing or failed validation run references
- duplicate event IDs
- duplicate initial promotion for the same note ID without an intervening governed rollback

## Recoverability and event phases

The canonical file write and promotion-event append are separate operations. They cannot be treated as one atomic transaction.

The business `action` describes what is being governed, such as `promote`, `amend`, or `supersede`. The `event_phase` describes progress of one operation.

At minimum:

1. append and flush a unique `intent` event;
2. write and flush a temporary file in the destination directory;
3. verify the temporary file hash;
4. install the canonical file using the approved same-volume replacement operation;
5. append and flush a unique `committed` event using the same `operation_id` and referencing the intent;
6. run recovery when an intent lacks a terminal event.

Each JSONL line has a unique `event_id`. Multiple records for one workflow copy the intent event ID into `operation_id`; later records must still use new unique `event_id` values.

Recovery appends a new `failed` or `recovered` record. Existing lines are never edited or deleted.

Recovery rules are defined in `05-specs/staging-write-wrapper-and-promotion-recovery.md`.
## Schema versioning

Breaking changes to the event shape require a new major `schema_version`.

Existing JSONL lines remain valid under the version recorded on each line.

A future Postgres promotions table must preserve the original event IDs, timestamps, hashes, and schema versions during import.

## Machine-readable schema

Before implementation, create:

```text
schemas/promotion-event.schema.json
```

The JSON Schema should enforce:

- required universal fields
- action-specific conditional fields
- string patterns for IDs and hashes
- allowed enums
- `additionalProperties: false` after early stabilization

## Open questions

- Exact human actor-ID format.
- Whether `source_path` should be omitted after Postgres becomes canonical for operations.
- Exact JSON Schema for scoped `review_context` and material-drift classification.
- Exact Windows API and language binding used for same-volume replacement and durable flush behavior.
- Whether amendment events require a reviewed diff artifact ID.

## Related files

- `05-specs/staging-and-promotion-workflow.md`
- `05-specs/staging-write-wrapper-and-promotion-recovery.md`
- `05-specs/kaizen-validation-gate-spec.md`
- `05-specs/kaizen-id-and-prefix-registry.md`
- `05-specs/kaizen-hammer-test-strategy.md`