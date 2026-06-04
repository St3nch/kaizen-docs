# Prompt 001 — Obsidian as an Agent-Readable Project Vault

I am designing an Obsidian vault called **Kaizen**.

Kaizen is not a general personal notes vault. It is a project intelligence pipeline that moves each project through this flow:

Idea capture -> research -> source summaries -> claims -> spec planning -> audit -> implementation handoff -> coding agent builds in the source repo.

The vault must be readable and maintainable by both humans and AI agents. Agents may access the vault through filesystem tools or an Obsidian MCP server. Before I finalize the folder structure, frontmatter schema, templates, command-center notes, and plugin usage, I need a constraint-focused research report on Obsidian as an agent-readable system.

Research the following with practical implementation depth.

## Primary research questions

1. How do AI agents actually interact with Obsidian vaults when using:
   - raw filesystem access
   - Obsidian MCP servers
   - other common Obsidian automation interfaces?

2. What does an agent see when reading raw Markdown compared with what a human sees rendered in Obsidian?

3. Which Obsidian features are safe for agent-readable project infrastructure, and which are risky because they rely on rendering, plugin state, cache state, or UI-only behavior?

Evaluate at minimum:
- YAML frontmatter
- normal Markdown headings
- wikilinks
- relative Markdown links
- embedded notes
- embedded images/files
- callouts
- tags
- Dataview code blocks
- Templater syntax
- Commander buttons
- Canvas
- Kanban plugin files
- Tasks plugin syntax

4. What are the practical limits and failure modes of these plugins for a system like Kaizen?
   - Dataview
   - Templater
   - Commander
   - Kanban
   - Tasks
   - Canvas
   - Obsidian MCP server options

5. What frontmatter structures does Dataview query reliably?
   - scalar fields
   - arrays
   - nested objects
   - dates
   - status fields
   - links
   - missing fields
   - inconsistent field values

6. What performance or reliability issues appear as vaults grow?
   - large folder counts
   - many Dataview queries
   - many templates
   - thousands of notes
   - plugin indexing/caching behavior
   - sync conflicts
   - agent edits while Obsidian is open

7. What design patterns are recommended for vaults that need to be both human-usable and programmatically readable?

## Evidence requirements

Use the strongest sources available:

1. Official Obsidian documentation and official plugin docs
2. Plugin GitHub repos, issues, and release notes
3. Obsidian forum discussions from experienced users
4. MCP server documentation and repos
5. Real-world reports only when clearly labeled as anecdotal

Do not treat community opinion as confirmed fact unless multiple credible sources agree.

## Output format

Return a research report with these sections:

1. Executive summary — the 10 most important constraints for Kaizen
2. Confirmed capabilities — what Obsidian/plugins can reliably do
3. Failure modes — what commonly breaks or should not be depended on
4. Agent-readability rules — how Kaizen notes should be written so agents can read them cleanly
5. Plugin-by-plugin recommendations — use, avoid, or defer
6. Frontmatter recommendations — field types, naming rules, status patterns, date handling
7. Folder and linking recommendations — what structure is safest for both humans and agents
8. Explicit design implications for Kaizen — concrete rules Kaizen should adopt
9. Open questions / uncertain areas — things that need testing rather than assuming
10. Source list with links

Be blunt. Prioritize constraints, edge cases, and implementation risks over generic Obsidian enthusiasm.
