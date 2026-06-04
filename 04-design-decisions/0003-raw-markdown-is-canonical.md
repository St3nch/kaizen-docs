# Decision 0003 - Raw Markdown Is Canonical

Status: proposed
Date: 2026-06-04
Related research: `03-research-results/003-obsidian-agent-readable-infrastructure-gpt-summary.md`

## Context

Kaizen will use Obsidian as its first human interface, but humans and agents must both be able to understand the system from files on disk. Rendered plugin output, visual layouts, and hidden plugin configuration create ambiguity and lock-in.

## Decision

Kaizen should treat plain Markdown files, flat validated YAML frontmatter, normal headings, body text, and validated links as the canonical representation of project intelligence.

Obsidian is the current human interface. It is not the source of truth independently of the files.

Plugin-generated or rendered state is non-canonical unless the same information exists in canonical Markdown/frontmatter.

## Consequences

### Canonical-capable

- `.md` files
- flat YAML frontmatter
- normal Markdown headings/body text
- validated internal links
- explicit source/provenance references

### Non-canonical convenience layers

- Dataview output
- Templater execution
- Commander buttons
- Canvas layouts
- Kanban rendering
- Excalidraw state
- advanced Tasks queries
- `.obsidian` UI state

### Required behavior

- Agents write static Markdown, not plugin-dependent templates or UI state.
- Important information must remain understandable with plugins disabled.
- Templates must produce valid static Markdown.
- Validation scripts enforce schema, links, and boundaries.

## Open questions

- Standardize Wikilinks or Markdown links as the default internal link form.
- Decide which `.obsidian` configuration files are intentionally versioned.
- Decide whether summary is canonical in frontmatter, body, or both.

## Related files

- `03-research-results/003-obsidian-agent-readable-infrastructure-gpt-summary.md`
- `05-specs/kaizen-field-registry.md`
- `05-specs/kaizen-note-type-registry.md`
