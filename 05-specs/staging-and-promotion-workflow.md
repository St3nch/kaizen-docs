# Spec - Staging and Promotion Workflow

Status: active draft aligned to accepted foundation
Date: 2026-06-04
Related decision: `04-design-decisions/0007-foundation-resolution-for-v0.2.md`

## Purpose

Define the safe path from agent-created drafts to canonical Kaizen notes.

The design goal is structural separation: agents may draft in staging, but only a human-controlled promotion operation may write canonical content.

Hermes is the designated Kaizen agent clerk and a required vault consumer. Kaizen therefore provides reliable canonical read/search access from the beginning while keeping Hermes writes staging-only until the boundary controls and tests pass.

## Folder boundary

Recommended initial layout:

```text
C:\dev\kaizen-vault
C:\dev\kaizen-staging
```

Rules:

- `kaizen-vault` is the canonical Markdown repository.
- `kaizen-staging` is disposable, non-canonical agent workspace.
- Staging is a sibling folder, not a nested vault folder.
- Staging does not need its own Git repository initially.
- Canonical notes must never link to files in staging.
- Qdrant indexing must read canonical content only.

## Permission boundary

Hermes write access remains unapproved until hands-on tests verify:

1. write operations are rooted to `C:\dev\kaizen-staging`
2. absolute paths outside staging are rejected
3. `..` path traversal is rejected
4. symlink/junction escape is rejected or impossible in the deployed wrapper
5. the canonical vault is read-only to Hermes
6. failed escape attempts are logged

If Hermes cannot enforce root-scoped writes directly, Kaizen must wrap all file tools in a service that enforces the boundary.

Prompt instructions are not an access-control mechanism.

## Staged note requirements

Agent-created staged notes must include:

```yaml
id:
type:
status: draft
project:
summary:
created:
updated:
review_status:
validation_status: pending
agent:
model:
session:
```

Additional type-specific fields remain required.

Hermes may use:

```yaml
review_status: not-required
```

only for note types explicitly allowed by the note-type registry. All governed types begin with:

```yaml
review_status: pending
```

Hermes may use `authority: none` or `authority: proposed` only where the registry permits it.

## Promotion preconditions

A staged note may be promoted only when all conditions are satisfied:

1. Search-before-create evidence exists.
2. The note passes static validation.
3. Required body sections exist.
4. All stable-ID relationships resolve.
5. All canonical Markdown links resolve.
6. No canonical note links back into staging.
7. No prohibited private/customer data is detected.
8. `validation_status: passed` is set by the validator, not the drafting agent.
9. Required human review is complete.
10. Authority transition, when applicable, is human-approved.
11. A recoverable promotion intent event is ready, and the recovery protocol is available if the file write and event sequence is interrupted.

## Promotion operation

Promotion is a human-controlled operation that performs these steps as one governed workflow:

1. Re-run validation against the exact staged file content.
2. Confirm destination path and verify it does not overwrite an unrelated note.
3. Confirm the note ID is globally unique and immutable.
4. Normalize canonical link syntax.
5. Remove `validation_status`.
6. Preserve durable agent provenance (`agent`, `model`, `session`).
7. Apply the approved `review_status` and `authority` transition.
8. Append and flush a unique `intent` event with a shared promotion `operation_id`.
9. Write and flush a uniquely named temporary file in the canonical destination directory.
10. Verify the temporary file hash against the approved content.
11. Install the canonical file using the approved same-volume replacement operation.
12. Append and flush a unique `committed` event using the same `operation_id` and referencing the intent event.
13. Verify the canonical file and terminal event are both present.
14. Remove or archive the staged copy only after successful verification.

If any step fails, the promotion is incomplete rather than silently successful. Partial state must remain visible and be reconciled through the recovery protocol in `05-specs/staging-write-wrapper-and-promotion-recovery.md`. The file write and JSONL append are separate operations and must not be described as one atomic transaction.

## Promotion log

Canonical repository path:

```text
_governance/promotion-log.jsonl
```

Each line is one JSON object.

Minimum event shape and phase requirements are defined in `05-specs/promotion-event-schema.md`. One promotion operation produces at least an `intent` record and a terminal `committed`, `failed`, or `recovered` record.


Required event fields:

- `event_id`
- `note_id`
- `action`
- `source_path`
- `destination_path`
- `prior_review_status`
- `new_review_status`
- `prior_authority`
- `new_authority`
- `approved_by`
- `approved_at`
- `validation_run_id`
- `basis`

## Append-only rule

Existing promotion events must never be edited or deleted.

Corrections are new events containing:

- a new `event_id`
- `action: correct`
- `corrects_event_id`
- correction reason
- corrected fields
- human approver and timestamp

Git history is supplementary evidence, not the promotion record itself.

## Rejection workflow

A rejected staged note remains outside the canonical vault.

The human reviewer may:

- return it for revision
- archive it in staging
- delete it through a reviewed cleanup action

Rejection does not create canonical content. A rejection event may be logged later when an operational governance service exists; it is not required for the v0.2 promotion log.

## Supersedence workflow

Superseding an accepted claim, decision, or spec requires:

1. a proposed replacement note
2. `supersedes` on the replacement
3. `superseded_by` on the prior note
4. a `## Supersedence rationale` section in the replacement
5. human approval at the same authority level as the prior note
6. promotion events for the replacement and prior-note metadata change
7. preservation of the prior note in canonical history

Hermes may propose supersedence but may not execute it.

## Failure handling

Promotion must fail closed.

Examples:

- log append succeeds but canonical write fails: append a corrective failure event and restore consistency
- canonical write succeeds but log append fails: revert the canonical write or mark the repository blocked until repaired
- destination collision: stop; never overwrite silently
- unresolved link or ID: stop
- stale approval after content changes: stop and require review again

Any material content change after human review makes the approval stale unless the reviewer explicitly approves the changed diff. Exact hashes detect change; scoped review determines whether a change is material, clerical, or unrelated.

## Future Postgres migration

When Postgres operational governance exists:

- the Postgres promotions table may become canonical for promotion events
- existing JSONL events are imported and retained
- the JSONL log may continue as a generated portable mirror
- Markdown note authority remains canonical in the note frontmatter; Postgres records the operational event that changed it

## Acceptance criteria

This workflow is implementation-ready when:

- [ ] root-scoped staging writes are proven
- [ ] promotion event JSON Schema is defined
- [ ] atomic or recoverable file-plus-log behavior is specified
- [ ] destination placement rules exist
- [ ] validator supports the accepted v0.2 registry
- [ ] a human-operated promotion command can be task-packeted
- [ ] staging escape and unauthorized promotion hammer tests are defined

## Related files

- `04-design-decisions/0001-two-zone-agent-write-model.md`
- `04-design-decisions/0007-foundation-resolution-for-v0.2.md`
- `05-specs/kaizen-field-registry.md`
- `05-specs/kaizen-note-type-registry.md`
- `05-specs/kaizen-validation-gate-spec.md`
- `07-hermes/hermes-write-access-preconditions.md`
