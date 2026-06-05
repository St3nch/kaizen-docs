# Obsidian

This folder holds research and design notes about Obsidian, plugins, MCP access, vault structure, and LLM-readable Markdown.

## Current status

No Obsidian plugin is final doctrine yet.

Initial assumptions to test:

- Dataview may be useful for human dashboards.
- Raw Markdown must remain readable without rendered Dataview output.
- Templater and Commander may be useful later, but should be earned.
- Canvas/Kanban/Tasks should be deferred until real workflow friction proves need.

## MCP safety posture

No filesystem or Obsidian MCP server is approved merely because it advertises an allowed root.

Before adoption, a candidate must be evaluated against:

- narrow tool capability exposure;
- final-path confinement;
- symbolic-link and Windows junction escape;
- traversal, absolute, UNC, device, and extended-length path handling;
- overwrite and sibling-repository protection;
- audit logging;
- the hammer matrix in `05-specs/staging-write-wrapper-and-promotion-recovery.md`.

MCP is an integration protocol, not the Kaizen authorization boundary.