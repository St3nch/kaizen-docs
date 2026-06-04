# Decision 0001 — Two-Zone Agent Write Model

Status: proposed
Date: 2026-06-03
Related research: `03-research-results/001-hermes-agent-boundaries-claude-summary.md`

## Context

Kaizen is intended to support agent-assisted vault operations. Hermes may eventually draft notes, summaries, logs, and task packets. However, the Kaizen standard depends on clear source-of-truth boundaries. Allowing an agent to write directly into canonical folders creates risk: malformed files, silent overwrites, duplicated notes, status drift, and accidental doctrine changes.

The Hermes boundary research strongly recommends separating agent-writable staging areas from human-governed canonical areas.

## Decision

Kaizen should adopt a two-zone write model:

1. **Staging zone** — agent-writable draft space for raw captures, summaries, proposals, validation reports, and test writes.
2. **Canonical zone** — human-governed source-of-truth space where accepted project docs, specs, decisions, and handoffs live.

Hermes may write only to approved staging zones. Canonical content may be changed only through a promotion process that includes validation and human approval.

## Why

This model makes the safe behavior structural instead of relying on prompt obedience.

It supports:

- reversible draft creation
- deterministic validation before acceptance
- clear audit trails
- human control over doctrine
- safer agent experimentation
- easier rollback

## Consequences

### Enables

- Agent drafting without granting source-of-truth authority
- Validation gates before canonical promotion
- Cleaner audit trails
- Safer Hermes onboarding

### Prevents

- Silent mutation of canonical files
- Agent-created doctrine
- Direct overwrite of accepted specs/decisions
- Confusing draft material with accepted truth

### Costs

- Adds a promotion step
- Requires validation scripts
- Requires clear folder ownership rules
- Requires humans to review promoted material

## Proposed folder implications

For the future vault, candidate staging folders may include:

```text
_inbox/
_drafts/
_agent-staging/
```

For this docs repo, staging should be introduced only when needed, not prebuilt without use.

## Open questions

- What exact staging folder names should the future vault use?
- Should research results land in staging first, or can they enter `03-research-results/` as evidence directly?
- Should the docs repo and future vault use the same staging conventions?
- What promotion metadata should be required?

## Related files

- `07-hermes/hermes-permission-matrix.md`
- `07-hermes/hermes-write-access-preconditions.md`
- `05-specs/kaizen-validation-gate-spec.md`
