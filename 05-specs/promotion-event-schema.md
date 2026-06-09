# Spec - Governed Mutation Event Schema

Status: implemented baseline
Date: 2026-06-04
Updated: 2026-06-09
Related decisions:
- `04-design-decisions/0007-foundation-resolution-for-v0.2.md`
- `04-design-decisions/0012-first-slice-contract-and-implementation-boundary.md`
- `04-design-decisions/0013-v0.2-first-slice-contract-reconciliation.md`

## Purpose

Define the append-only event contract for governed canonical Markdown mutations.

Events are operational governance evidence. They do not replace canonical Markdown, immutable operation evidence, or Git history.

## Canonical log path

```text
_governance/promotion-log.jsonl
```

Each line contains exactly one immutable JSON object.

## Implemented actions

```text
promote
amend
```

- `promote` creates a canonical note for the first time.
- `amend` changes one existing canonical Markdown note at the same destination through a reviewed bounded amendment.

Reserved but unimplemented vocabulary:

```text
supersede
correct
rollback
```

Reserved actions are not executable capabilities and require separate accepted decisions, specifications, implementations, hammer evidence, and governed return audits.

## Event phases

New v0.2 events use:

```text
intent
committed
failed
recovered
```

- `intent` is the durable pre-mutation record.
- `committed` is the normal successful terminal record.
- `failed` is a terminal record showing that the operation did not produce an accepted canonical result.
- `recovered` is a terminal record showing that recovery reconciled an incomplete operation.

An implementation response may use a result label such as `recovered_committed`; persisted new event phases remain those above.

Historical event lines are never rewritten or renamed. Readers must remain backward-compatible with already persisted event shapes and result labels.

Exactly one successful terminal event may exist for an operation:

```text
committed
or
recovered
```

## Universal required binding

Every new `promote` or `amend` event binds:

| Field | Rule |
|---|---|
| `schema_version` | serialized event-contract version |
| `event_id` | unique immutable ID for this line |
| `operation_id` | shared immutable operation ID |
| `event_phase` | one of the implemented phases |
| `action` | `promote` or `amend` |
| `note_id` | stable canonical note ID |
| `note_type` | registered note type |
| `project` | project slug |
| `destination_path` | vault-relative forward-slash path |
| `approved_by` | human actor string |
| `approved_at` | ISO-8601 UTC timestamp |
| `validation_run_id` | successful validation run ID |
| `plan_sha256` | immutable plan payload hash |
| `approval_sha256` | immutable approval payload hash |
| `content_sha256` | approved candidate hash for intent; installed hash for success |
| `basis` | concise reviewed basis |

Terminal records also include `intent_event_id` where the implemented schema requires it.

## Promotion-specific binding

A `promote` operation additionally binds:

- `source_path`;
- source content SHA-256 where present in the implemented schema;
- prior and new review status;
- prior and new authority;
- required dependency evidence where applicable.

First-time promotion must refuse an existing destination.

## Amendment-specific binding

An `amend` operation additionally binds:

- `prior_content_sha256`;
- `new_content_sha256` or equivalent installed candidate binding;
- `reviewed_diff_sha256`;
- `change_summary`;
- prior and new review status when changed;
- prior and new authority when changed.

The first amendment slice preserves note ID, type, project, and created timestamp. General overwrite, delete, move, rename, multi-note mutation, supersedence, correction, and rollback execution remain prohibited.

## Phase requirements

### Intent

The intent event is appended and durably flushed before canonical filesystem mutation.

It contains the approved operation identity, destination, exact content bindings, plan and approval hashes, validation evidence, actor, timestamp, and basis.

### Committed

A committed event is appended only after the canonical destination has been installed and verified by identity, size, and SHA-256.

It matches the same operation, action, note, destination, plan, approval, and approved content as the intent.

### Failed

A failed event records:

- the intent reference;
- stable failure code;
- concise failure summary;
- observed filesystem state;
- whether human review is required before any new attempt.

### Recovered

A recovered event records:

- the intent reference;
- recovery action;
- resulting filesystem state;
- verified canonical hash when a canonical result exists;
- human actor when recovery performs a consequential action.

## Append-only and compatibility rules

- Existing event lines are never edited or deleted.
- Existing note IDs, operation IDs, event IDs, timestamps, and hashes remain unchanged.
- Schema-version readers validate each line under its recorded version.
- New fields must not invalidate historical `1.0` lines.
- Machine-readable schema changes require regression tests against all existing log lines.
- Git is supplementary evidence, not the event record itself.

## Path rules

- `destination_path` is vault-relative and contains no traversal.
- `source_path` may be an absolute local staging path but is not portable identity.
- Canonical paths do not point into staging.
- Governance paths are allowed only when explicitly governed.

## Human actor limitation

The first slice accepts a trusted local actor string such as:

```text
owner.local
```

This is not authentication. Agents may not fabricate or select the owner actor. Shared or remote operation requires a later identity and authentication design.

## Recovery and retry rules

- An operation with a successful terminal event is idempotently complete.
- Recovery evaluates immutable plan, approval, event, filesystem, and hash evidence.
- Unknown canonical or temporary bytes fail closed.
- A new reviewed plan is required when immutable evidence no longer matches.
- Historical evidence is preserved even when recovery fails.

## Acceptance evidence

The implemented baseline is evidenced by:

- six ordered Packet 009B promotions;
- two governed amendments;
- exactly `intent -> committed` for all eight operations;
- Results 031, 040, 057, 059, 061, 062, and 065;
- platform test suite passing 230 tests at Milestone 4 closure.

## Open questions

- Durable multi-user actor identity and authentication.
- Future Postgres event import and mirror behavior.
- Machine-readable schema version increment policy after further action types exist.

## Related files

- `05-specs/staging-and-promotion-workflow.md`
- `05-specs/staging-write-wrapper-and-promotion-recovery.md`
- `05-specs/kaizen-validation-gate-spec.md`
- `05-specs/kaizen-hammer-test-strategy.md`
