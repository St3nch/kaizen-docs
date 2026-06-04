# LLM Start Here

This is the first file every LLM should read when working in the `kaizen-docs` repository.

## What this repo is

`kaizen-docs` is the planning and stewardship repository for Kaizen.

Kaizen is intended to become an Obsidian-based project intelligence pipeline. The future vault will help move projects through this flow:

```text
idea capture -> research -> source summaries -> claims -> specs -> audit -> handoff packet -> coding agent build
```

This repository is **not** the Obsidian vault. It is the design workspace used to research, plan, and govern that future vault.

## Current project posture

- Current phase: early planning and research
- Future vault: not created yet
- Hermes Desktop / Hermes Agent: downloaded by user, not yet granted Kaizen write authority
- Primary immediate need: research-backed design constraints before building the vault
- Stewardship principle: keep the system lean until structure earns its existence

## How to traverse this repo

Read in this order unless the user gives a narrower task:

1. `00-entrypoint/LLM_START_HERE.md` — this file
2. `01-project-standard/kaizen-project-standard.md` — current baseline standard
3. `02-research-prompts/` — approved deep research prompts
4. `03-research-results/` — raw or summarized research results when added
5. `04-design-decisions/` — accepted or proposed decisions
6. `05-specs/` — draft specs for the future vault/system
7. `07-hermes/` — Hermes capability, boundaries, skill-file planning
8. `08-obsidian/` — Obsidian/plugin/MCP constraints
9. `06-handoff-patterns/` — task packet and coding-agent handoff standards
10. `99-archive/` — old material, not current doctrine unless explicitly restored

## Folder map

| Folder | Purpose | Doctrine status |
|---|---|---|
| `00-entrypoint/` | LLM and human orientation | Current guidance |
| `01-project-standard/` | Baseline Kaizen standard drafts | Candidate doctrine |
| `02-research-prompts/` | Prompts used for Claude/GPT/Hermes research | Inputs only |
| `03-research-results/` | Research outputs and summaries | Evidence, not doctrine |
| `04-design-decisions/` | Explicit decisions about Kaizen design | Doctrine when status is accepted |
| `05-specs/` | Draft implementation/planning specs | Draft until reviewed |
| `06-handoff-patterns/` | Agent task packet patterns | Draft until reviewed |
| `07-hermes/` | Hermes Desktop/Agent research and policy | Draft until validated |
| `08-obsidian/` | Obsidian features, plugins, vault architecture | Evidence and draft constraints |
| `99-archive/` | Retired or superseded docs | Not current |

## Rules for LLMs

1. Never assume this repo is the future Obsidian vault.
2. Never create vault folders here unless the user explicitly asks for a simulated or draft structure.
3. Do not promote research to doctrine without a decision note.
4. Do not mark Hermes as approved for write access until a Hermes policy exists.
5. Prefer small, durable Markdown files over huge monolithic docs.
6. Use explicit file paths when referencing docs.
7. Keep documents readable as raw Markdown; do not rely on rendered-only features.
8. Treat Dataview, Templater, Commander, Canvas, Kanban, and Tasks as design subjects, not assumptions.
9. Preserve the source-of-truth boundary: Kaizen planning lives here; running code and operational data do not.
10. When uncertain, create a research question or decision proposal rather than inventing policy.

## Current open questions

- Which Obsidian features are safe for agent-readable infrastructure?
- Which plugin dependencies should be required, deferred, or avoided?
- What exact authority should Hermes Desktop / Hermes Agent have?
- What validation must happen before agent-created notes are accepted?
- What is the minimum viable vault structure for the first real Kaizen project?
- How should research findings become accepted Kaizen doctrine?

## Required output style for future docs

Prefer this shape:

```markdown
# Clear Title

## Purpose

Why this file exists.

## Current status

Draft, accepted, superseded, or archived.

## Content

The actual useful material.

## Open questions

What is unresolved.

## Related files

Explicit links or paths.
```

## Stewardship stance

Kaizen should grow by evidence, not enthusiasm.

The default answer to new structure, automation, plugins, folders, and agent powers is:

```text
not yet — earn it
```

When a feature proves useful through real friction or research-backed need, document it, decide it, and only then add it to the standard.
