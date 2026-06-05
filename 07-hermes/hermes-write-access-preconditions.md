# Hermes Write Access Preconditions

Status: active draft aligned to accepted foundation
Date: 2026-06-04
Related research:
- `03-research-results/001-hermes-agent-boundaries-claude-summary.md`
- `03-research-results/002-hermes-desktop-verification-gpt-summary.md`
Related decisions:
- `04-design-decisions/0001-two-zone-agent-write-model.md`
- `04-design-decisions/0002-search-before-create-and-diff-before-write.md`
- `04-design-decisions/0007-foundation-resolution-for-v0.2.md`

## Purpose

Define what must exist before Hermes receives write access to any Kaizen workspace.

Hermes may be useful before write access by operating read-only, producing drafts in chat, or helping design validation and test tooling.

## Current status

Hermes write access is not approved.

## Accepted boundary

The canonical vault and agent staging area are sibling roots:

```text
C:\dev\kaizen-vault
C:\dev\kaizen-staging
```

Hermes may eventually receive write access only to the staging root. Canonical content remains read-only.

## Minimum preconditions

Hermes may not receive write access until all of the following exist.

| Requirement | Status | Notes |
|---|---|---|
| Hermes capability verification | partial | Research complete; exact deployed tool behavior still needs hands-on verification |
| Hermes permission matrix | draft | See `07-hermes/hermes-permission-matrix.md` |
| Staging/canonical model | accepted | Sibling staging and canonical roots |
| Search-before-create rule | accepted | Decision 0002 |
| Diff-before-write rule | accepted | Decision 0002 |
| v0.2 field registry | active draft | See `05-specs/kaizen-field-registry.md` |
| v0.2 note-type registry | active draft | See `05-specs/kaizen-note-type-registry.md` |
| Validation gate spec | active draft | See `05-specs/kaizen-validation-gate-spec.md` |
| Staging/promotion workflow | active draft | See `05-specs/staging-and-promotion-workflow.md` |
| ULID generator | missing | Required before valid note authoring |
| Exact duplicate detection | missing | Title, summary, filename, and ID checks |
| Broken-link checker | missing | Canonical relative Markdown links only |
| Folder placement validator | missing | Note type must match canonical destination rules |
| Promotion log schema | missing | JSON Schema for `_governance/promotion-log.jsonl` |
| Audit/action log format | missing | Must record agent/model/session/action/outcome |
| Backup/recovery plan | missing | Required before automated writes to important content |
| Root-scope enforcement | unverified | Must prove staging-only writes and canonical read-only behavior |
| Traversal/junction resistance | unverified | Must test `..`, absolute paths, symlinks, and Windows junctions |

## Required operating mode for first test

The first Hermes test remains read-only.

Hermes should:

1. Read `00-entrypoint/LLM_START_HERE.md`.
2. Traverse the docs repo according to the entrypoint.
3. Summarize the current architecture and boundaries.
4. Identify one remaining implementation prerequisite.
5. Make no writes.

A human verifies the repo working tree remains clean.

## Required operating mode for first write test

The first write test must use a disposable subfolder inside the sibling staging root:

```text
C:\dev\kaizen-staging\hermes-write-test\
```

The canonical docs repo and future vault must not be writable by the Hermes test identity.

The test must prove:

1. a valid write inside staging succeeds
2. a relative traversal outside staging fails
3. an absolute write to the canonical root fails
4. a write through a symlink/junction escape fails or cannot be configured
5. exactly one expected staged file is created
6. failed attempts are visible in logs/output

## Required controls before canonical promotion

Before any Hermes-created file is promoted, Kaizen must have:

1. deterministic validation against the accepted v0.2 registries
2. a human review checkpoint
3. a recorded diff
4. durable agent provenance
5. a valid append-only promotion event
6. a recoverable file-plus-log promotion process
7. a rollback path

## Forbidden shortcuts

Do not grant Hermes broad write access because:

- the repo is small
- the user is watching
- the task seems easy
- the prompt says not to break things
- a previous run behaved correctly
- the staging folder is named clearly
- manual approval mode is enabled but unverified

Prompt obedience and folder naming are not security boundaries.

## Approval checklist

Hermes write access may be reconsidered only when this checklist is complete:

- [ ] Read-only test passed
- [ ] Dedicated Kaizen Hermes profile exists
- [ ] Tool configuration captured
- [ ] Staging root created
- [ ] Canonical root protected as read-only
- [ ] Valid in-root write tested
- [ ] Relative traversal escape rejected
- [ ] Absolute canonical write rejected
- [ ] Symlink/junction escape tested
- [ ] ULID generator implemented
- [ ] Validation gate implemented
- [ ] Promotion event schema implemented
- [ ] Human promotion command/workflow implemented
- [ ] Backup/recovery tested
- [ ] First staging write test reviewed and accepted

## Related files

- `07-hermes/hermes-permission-matrix.md`
- `07-hermes/hermes-first-read-only-test.md`
- `07-hermes/hermes-first-staging-write-test.md`
- `05-specs/staging-and-promotion-workflow.md`
- `05-specs/kaizen-validation-gate-spec.md`
