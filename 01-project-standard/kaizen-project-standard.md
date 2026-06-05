# Kaizen Project Standard

Status: baseline import placeholder

This file is reserved for the evolving Kaizen Project Standard.

The full original baseline currently exists in the user-provided planning document and has not yet been imported into this repository. It must be imported deliberately before a complete v0.2 rewrite is produced.

## Import rule

Do not reconstruct the baseline from memory.

Import the original document exactly enough to preserve its intent, then revise it through tracked decisions and the process in:

- `01-project-standard/standard-revision-plan.md`

## Current baseline themes

- Kaizen is a project intelligence pipeline.
- The future vault is separate from this docs repository.
- Every project follows one pipeline.
- Notes must be readable by humans and LLM agents.
- Research is evidence, not doctrine.
- Hermes handles high-volume, low-risk work only after boundaries are defined.
- Frontier models handle reasoning-heavy audit and architecture work.
- Structure is earned, not prebuilt.
- Implementation-ready task packets are the primary pipeline outcome.

## Accepted architecture additions

The following are now accepted inputs to the v0.2 rewrite:

- raw Markdown as canonical project intelligence
- Postgres as operational source of truth for Observatory records
- Qdrant as a rebuildable semantic index
- API-only structured-data access for agents
- sibling staging with human-controlled promotion
- search-before-create and diff-before-write
- governed field and note-type registries
- hammer tests as a hard gate
- one immutable prefixed ULID note ID
- relative Markdown body links and stable-ID frontmatter relationships
- append-only JSONL promotion events

These accepted decisions are documented in `04-design-decisions/0001` through `0007`.

## Next action

1. Import the full original standard.
2. Reconcile it against accepted Decisions 0001-0007.
3. Resolve the remaining v0.2 questions in `standard-revision-plan.md`.
4. Draft `kaizen-project-standard-v0.2-draft.md`.
5. Audit the draft against research and governance patterns.
6. Accept and supersede the baseline only after explicit human review.

## Related files

- `01-project-standard/standard-revision-plan.md`
- `00-entrypoint/LLM_START_HERE.md`
- `04-design-decisions/`
- `05-specs/`
