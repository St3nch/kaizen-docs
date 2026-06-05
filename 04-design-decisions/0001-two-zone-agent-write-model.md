# Decision 0001 - Two-Zone Agent Write Model

Status: accepted
Date: 2026-06-03
Accepted: 2026-06-04
Related research: `03-research-results/001-hermes-agent-boundaries-claude-summary.md`
Related resolution: `04-design-decisions/0007-foundation-resolution-for-v0.2.md`

## Context

Kaizen is intended to support agent-assisted vault operations. Hermes may eventually draft notes, summaries, logs, and task packets. However, the Kaizen standard depends on clear source-of-truth boundaries. Allowing an agent to write directly into canonical folders creates risk: malformed files, silent overwrites, duplicated notes, status drift, and accidental doctrine changes.

The Hermes boundary research strongly recommends separating agent-writable staging areas from human-governed canonical areas.

## Decision

Kaizen adopts a two-zone write model:

1. **Staging zone** - agent-writable draft space for captures, summaries, proposals, validation reports, and test writes.
2. **Canonical zone** - human-governed source-of-truth space where accepted project intelligence lives.

Hermes may write only to an approved sibling staging root. Canonical content may change only through a human-controlled promotion workflow that includes deterministic validation and an append-only promotion event.

Recommended initial layout:

```text
C:\dev\kaizen-vault
C:\dev\kaizen-staging
```

The staging folder is not required to be its own Git repository.

## Why

This model makes safe behavior structural instead of relying on prompt obedience.

It supports:

- reversible draft creation
- deterministic validation before acceptance
- clear audit trails
- human control over authority
- safer agent experimentation
- easier rollback and containment

## Required enforcement

Before Hermes receives write access, Kaizen must verify:

- the write root is limited to the sibling staging folder
- absolute paths outside staging fail
- path traversal fails
- symlink/junction escape fails or is prevented by the wrapper
- canonical content remains read-only to Hermes
- failed boundary attempts are logged

If Hermes cannot enforce the root directly, Kaizen must wrap file operations in a tool that does.

## Consequences

### Enables

- agent drafting without source-of-truth authority
- validation gates before canonical promotion
- cleaner audit trails
- safer Hermes onboarding

### Prevents

- direct agent mutation of canonical notes
- agent-created authority
- silent overwrite of accepted decisions/specs
- staging content appearing canonical by proximity

### Costs

- adds a promotion step
- requires validation and promotion tooling
- requires separate root management
- requires human review

## Related files

- `04-design-decisions/0007-foundation-resolution-for-v0.2.md`
- `05-specs/staging-and-promotion-workflow.md`
- `07-hermes/hermes-write-access-preconditions.md`
- `05-specs/kaizen-validation-gate-spec.md`
