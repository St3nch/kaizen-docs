# Research Summary 003 - Obsidian as Agent-Readable Infrastructure

Status: evidence summary
Source: GPT Deep Research report provided by user
Date captured: 2026-06-04

## Purpose

Capture the research-backed constraints for using Obsidian as Kaizen's human interface while keeping raw Markdown readable by agents and portable to future systems.

This file is evidence. It is not doctrine by itself.

## High-confidence findings

1. Plain Markdown plus narrow, validated YAML should be the canonical layer for Kaizen project intelligence.
2. Obsidian is the current human interface, not the authority-bearing data model.
3. Dataview is a derived human dashboard/read model, not a source of truth.
4. Templater and Commander are human convenience layers and should be earned later.
5. Canvas, Kanban, Excalidraw, and advanced Tasks views should not hold canonical state.
6. Frontmatter should remain flat, typed, enum-controlled, and small.
7. Important state must exist in raw Markdown/frontmatter, never only in rendered plugin output.
8. Agents should write static Markdown directly and initially only into staging.
9. Filesystem access with validation is safer than broad Obsidian-native REST/MCP write access.
10. External writes while Obsidian is open should be treated as eventually consistent; raw diffs and validation matter more than rendered appearance.

## Immediate implications

- Revise the original plugin install order: Dataview may be useful later; Templater and Commander should not be required first-session infrastructure.
- Preserve a plain-text LLM entry point in every project command center.
- Add stable IDs, controlled enums, link validation, and frontmatter validation.
- Keep staging separate from canonical content and explicitly exclude staging from dashboards.
- Version-control only intentional `.obsidian` configuration; ignore high-churn workspace files.
- Prefer lowercase kebab-case, ASCII-safe filenames and globally unique names where practical.

## Candidate plugin classification

| Layer | Classification |
|---|---|
| Markdown body, headings, flat YAML | Canonical-capable |
| Wikilinks or Markdown links with validation | Canonical-capable |
| Dataview | Human read model only |
| Templater | Human creation convenience only |
| Commander | Human UI only |
| Canvas / Kanban / Excalidraw | Non-canonical, defer |
| Tasks plain checkboxes | Usable locally; advanced query layer non-canonical |
| Obsidian Local REST API | High blast radius; avoid as default |

## Open questions

- Should staging live in-vault, in a sibling repo, or in a shadow vault?
- Which internal link convention should Kaizen standardize on?
- Which `.obsidian` files should be version-controlled?
- How should external edits trigger or verify metadata re-indexing?
- Should summary live in frontmatter, body, or both?

## Related files

- `04-design-decisions/0003-raw-markdown-is-canonical.md`
- `05-specs/kaizen-field-registry.md`
- `05-specs/kaizen-note-type-registry.md`
- `08-obsidian/README.md`
