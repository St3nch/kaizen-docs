# Spec — Kaizen Validation Gate

Status: draft
Date: 2026-06-03
Related research: `03-research-results/001-hermes-agent-boundaries-claude-summary.md`

## Purpose

Define the validation gate that must eventually run before any agent-created or agent-modified note becomes accepted Kaizen content.

This spec is a draft. It describes the desired validation system but does not implement it yet.

## Problem

LLM agents can create useful Markdown quickly, but they frequently fail at structured document discipline:

- malformed YAML
- missing required fields
- invented statuses
- duplicate notes
- broken links
- wrong folder placement
- missing sources
- vague acceptance criteria
- silent overwrites

Kaizen needs deterministic validation so agent work can be checked by code before it is trusted.

## Design goal

The validation gate should answer one question:

```text
Can this proposed note or change be accepted into canonical Kaizen content?
```

The gate should return:

- pass/fail status
- specific errors
- file paths involved
- recommended fixes where deterministic
- raw command output suitable for agent correction

## Required checks

### 1. Markdown file check

Validate that the target file is a normal Markdown file and not binary or unsupported content.

### 2. YAML/frontmatter parse

Every durable Kaizen note must have parseable YAML frontmatter.

Failures should identify the file and parse error.

### 3. Required field check

Required frontmatter fields should be present based on note type.

Candidate baseline required fields:

```yaml
type:
status:
project:
pipeline_stage:
summary:
created:
updated:
```

This list may vary for repo-level docs that are not vault notes.

### 4. Enum validation

Controlled fields must match allowed values.

Candidate enums:

```yaml
type:
  - overview
  - current-state
  - roadmap
  - decision
  - spec
  - task-packet
  - source-summary
  - claim
  - raw-source
  - source-import-map

status:
  - draft
  - proposed
  - active
  - accepted
  - pending-review
  - blocked
  - complete
  - archived
  - rejected
  - superseded

pipeline_stage:
  - capture
  - research
  - spec
  - audit
  - handoff
  - build
```

The enum list must be reconciled with the current Kaizen Project Standard before implementation.

### 5. Folder placement validation

Note type must be allowed in its folder.

Examples:

| Note type | Allowed canonical folder |
|---|---|
| decision | `decisions/` or `04-design-decisions/` depending on context |
| spec | `specs/` or `05-specs/` depending on context |
| task-packet | `handoffs/` or `06-handoff-patterns/` depending on context |
| source-summary | `research/` or `03-research-results/` depending on context |

The docs repo and future vault may have different placement maps.

### 6. Duplicate detection

Before accepting a new note, the gate should check:

- exact title match
- exact summary match
- similar filename
- optional near-duplicate content similarity

Near-duplicate detection can be deferred until exact checks exist.

### 7. Link validation

The gate should validate:

- Markdown links
- relative file links
- Obsidian wikilinks when used

Broken links should block promotion unless explicitly marked as external/unresolved.

### 8. Source/provenance validation

Research-derived notes should include source references.

Claim notes should include evidence references or exact quotes when possible.

Agent-created notes should eventually include provenance such as:

```yaml
agent:
model:
session:
source_docs:
validation_run:
```

Exact field names are not final.

### 9. Task packet readiness check

A task packet should not be marked ready unless it has:

- approved spec reference
- objective
- source files to read first
- output location
- constraints
- acceptance criteria
- audit checklist

### 10. Protected-file check

The gate should block unauthorized changes to protected files such as:

- accepted decisions
- accepted specs
- project standard doctrine
- Hermes skill files
- source-of-truth rules

## Non-goals

This validation gate does not decide whether a spec is good.

It does not decide whether a design is correct.

It does not replace human or frontier-model review.

It only validates structural and policy compliance.

## Suggested implementation approach

Build as a small script or set of scripts that can be run by:

- humans
- Hermes
- CI
- pre-promotion workflow

Suggested command shape:

```text
kaizen-validate <path-or-diff>
```

The command should return nonzero on failure and print actionable errors.

## Acceptance criteria for this spec

This spec becomes implementation-ready when:

- [ ] The final frontmatter schema is defined.
- [ ] Folder placement rules are defined for the docs repo.
- [ ] Folder placement rules are defined for the future vault.
- [ ] Duplicate detection minimum viable behavior is defined.
- [ ] Link validation requirements are defined.
- [ ] Protected-file rules are defined.
- [ ] A first implementation task packet can be written from this spec.

## Open questions

- Should validation live in this repo, the future vault, or a separate utility repo?
- Should docs-repo files use the same frontmatter standard as vault files?
- Which fields are required for non-vault planning docs?
- Should accepted decisions be immutable except through superseding decisions?
- How should validation reports be stored?

## Related files

- `03-research-results/001-hermes-agent-boundaries-claude-summary.md`
- `04-design-decisions/0001-two-zone-agent-write-model.md`
- `04-design-decisions/0002-search-before-create-and-diff-before-write.md`
- `07-hermes/hermes-permission-matrix.md`
- `07-hermes/hermes-write-access-preconditions.md`
