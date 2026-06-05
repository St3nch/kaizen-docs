# Decision 0002 — Search-Before-Create and Diff-Before-Write

Status: accepted
Date: 2026-06-03
Accepted: 2026-06-04
Related research: `03-research-results/001-hermes-agent-boundaries-claude-summary.md`

## Context

Agent-assisted document systems commonly fail through duplicated notes, invented file paths, broken links, and silent overwrites. Kaizen must remain navigable by both humans and LLMs. Duplicate and silently-mutated notes are especially damaging because they erode trust in the vault as a source of truth.

## Decision

Kaizen should adopt two mandatory workflow rules for agent operations:

1. **Search before create** — before creating any new note, an agent must search for existing related notes and report the result.
2. **Diff before write** — before modifying any existing note, an agent must produce a reviewable diff and stop for approval unless operating in an explicitly disposable staging area.

## Why

Search-before-create reduces duplicate notes and encourages reuse or extension of existing material.

Diff-before-write preserves auditability and makes accidental mutation visible before it becomes canonical.

Together, these rules make the vault more durable and easier for future agents to traverse.

## Required behavior

### Search-before-create

Before creating a note, the agent must report:

- search query used
- files found
- why a new note is still needed, or why an existing note should be updated instead

### Diff-before-write

Before modifying an existing file, the agent must report:

- target file path
- reason for change
- unified diff or equivalent reviewable patch
- whether validation was run
- whether human approval is required

## Exceptions

Potential exceptions may be allowed only in disposable staging/sandbox folders where files have no canonical status.

Even there, the agent should still report what it changed.

## Consequences

### Enables

- duplicate prevention
- safer edits
- clear review process
- easier rollback
- better human trust

### Prevents

- silent mutation
- duplicate note sprawl
- accidental overwrites
- unreviewed doctrine changes

### Costs

- Adds steps before creating or editing files
- Requires search tooling
- Requires diff tooling
- Requires agents to stop more often

## Open questions

- What search command/tool should be canonical for the future vault?
- What similarity threshold should trigger a duplicate warning?
- Which file changes can be auto-applied in staging?
- How should diffs be stored for long-term audit?

## Related files

- `07-hermes/hermes-permission-matrix.md`
- `07-hermes/hermes-write-access-preconditions.md`
- `05-specs/kaizen-validation-gate-spec.md`
