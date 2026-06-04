# Hermes Write Access Preconditions

Status: draft policy
Date: 2026-06-03
Related research:
- `03-research-results/001-hermes-agent-boundaries-claude-summary.md`
- `03-research-results/002-hermes-desktop-verification-gpt-summary.md`

## Purpose

Define what must exist before Hermes receives write access to any Kaizen workspace.

This document is intentionally conservative. Hermes may be useful before write access by operating read-only, producing proposed drafts in chat, or helping design validation scripts.

## Current status

Hermes write access is not approved.

## Minimum preconditions

Hermes may not receive write access until all of the following exist.

| Requirement | Status | Notes |
|---|---|---|
| Hermes capability verification | pending | Must verify Desktop/Agent behavior against official docs and hands-on testing |
| Hermes permission matrix | draft | See `07-hermes/hermes-permission-matrix.md` |
| Staging/canonical folder model | draft | See decision 0001 |
| Search-before-create rule | draft | See decision 0002 |
| Diff-before-write rule | draft | See decision 0002 |
| Frontmatter schema | missing | Needs JSON Schema or equivalent deterministic validator |
| Validation gate spec | draft | See `05-specs/kaizen-validation-gate-spec.md` |
| Duplicate detection approach | missing | Exact and near-duplicate policy needed |
| Broken-link checker | missing | Must handle wikilinks and Markdown links |
| Folder placement validator | missing | Note type must match allowed folder(s) |
| Human promotion workflow | missing | Needs review step and approval record |
| Audit log format | missing | Must record agent/model/session/action/provenance |
| Backup/recovery plan | missing | Needed before any automated writes to important files |
| Git discipline | partial | Repo exists; branch/PR policy not yet defined |

## Required operating mode for first test

The first Hermes test must be read-only.

The test should ask Hermes to:

1. Read `00-entrypoint/LLM_START_HERE.md`.
2. Traverse the repo according to the documented order.
3. Produce a proposed summary of the repo structure.
4. Identify one missing doc or unresolved question.
5. Make no writes.

## Required operating mode for first write test

The first write test must be confined to a disposable staging folder.

Suggested folder:

```text
_sandbox/hermes-write-test/
```

The test should ask Hermes to:

1. Search for an existing note topic.
2. Report search results.
3. Create a single draft note in the sandbox.
4. Run or simulate validation.
5. Report what it wrote.
6. Stop without modifying canonical folders.

## Required controls before canonical promotion

Before any Hermes-created file is promoted into canonical docs or a future vault, Kaizen must have:

1. A deterministic validation command.
2. A human review checkpoint.
3. A recorded diff.
4. A provenance record.
5. A rollback path.

## Forbidden shortcuts

Do not grant Hermes broad write access because:

- the repo is small
- the user is watching
- the task seems easy
- the prompt says not to break things
- a previous run behaved correctly
- manual approval mode is enabled but unverified

Prompt obedience is not a security boundary.

## Approval checklist

Hermes write access may be reconsidered only when this checklist is complete:

- [ ] Hermes capability verification complete
- [ ] File write behavior tested in sandbox
- [ ] Tool restrictions verified
- [ ] Staging folder created
- [ ] Canonical folders protected
- [ ] Validation gate implemented
- [ ] Promotion process documented
- [ ] Human approval process documented
- [ ] Backup/recovery tested
- [ ] First write test reviewed and accepted

## Related files

- `07-hermes/hermes-permission-matrix.md`
- `04-design-decisions/0001-two-zone-agent-write-model.md`
- `04-design-decisions/0002-search-before-create-and-diff-before-write.md`
- `05-specs/kaizen-validation-gate-spec.md`
