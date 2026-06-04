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

## Current architecture additions under review

The following are proposed and must not be treated as accepted doctrine yet:

- raw Markdown as canonical project intelligence
- Postgres as operational source of truth for Observatory records
- Qdrant as a rebuildable semantic index
- API-only structured-data access for agents
- staging-only agent writes with human promotion
- governed field and note-type registries
- hammer tests as a hard gate

## Next action

1. Import the full original standard.
2. Review the proposed decisions individually.
3. Resolve the lifecycle, ID, staging, linking, and promotion questions.
4. Draft Kaizen Project Standard v0.2.
5. Audit and accept it only after human review.

## Related files

- `01-project-standard/standard-revision-plan.md`
- `00-entrypoint/LLM_START_HERE.md`
- `04-design-decisions/`
- `05-specs/`
