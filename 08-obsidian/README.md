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
## Ecosystem value posture

Kaizen intentionally uses Obsidian and should benefit from its ecosystem rather than treating the application as a decorative viewer over a folder.

Potential Obsidian-aware capabilities include:

- backlinks and outgoing links;
- aliases, embeds, headings, and blocks;
- property and tag search;
- Obsidian search semantics;
- reveal/open-note actions for human review;
- workspace and navigation helpers;
- selected plugin-derived views and search aids.

The safety goal is not to eliminate these capabilities. It is to classify them into separate lanes:

```text
portable canonical access
Obsidian-aware read/navigation
Kaizen-governed agent writes
human/admin Obsidian operations
```

Raw Markdown remains canonical and portable. Obsidian-aware capabilities may enrich navigation, review, and discovery without becoming hidden canonical truth.

A third-party Obsidian MCP server may be reused, wrapped, forked, or assigned to a human/admin profile when evidence supports it. Broad mutation tools do not belong in the Hermes clerk profile, but their existence does not require rejecting the rest of the server's useful ecosystem capabilities.
## Read-lane confinement

Read/navigation tools must pass the same root-confinement and privacy tests as write tools where applicable. A read-only escape can expose credentials, private project data, sibling repositories, or excluded sources.

Required controls include:

- final resolved-path confinement;
- symlink and Windows junction handling;
- project and folder scope;
- private-root exclusions;
- no generic read-file escape behind specialized search tools;
- denied-read logging without leaking protected content.

## Capability versus truth

Kaizen may use semantic search, Dataview, Smart Connections, backlinks, Obsidian search, and other plugin-aware capabilities.

These capabilities remain derived and rebuildable. They must not become the only path to a fact or the canonical store of project intelligence.

## Integration contexts

Kaizen supports two complementary contexts:

```text
headless canonical access
interactive Obsidian-aware access
```

Headless jobs such as CI validation, overnight filing, recovery, and Qdrant rebuilds must work without Obsidian running.

Interactive workflows may use backlinks, properties, tags, reveal/open-note actions, active-note context, and other Obsidian-native capabilities that improve human review.

## Configure versus wrap

- Plugin MCP candidates must prove unsafe tools are absent from discovery in the constrained profile.
- Runtime denial of an advertised dangerous tool is not sufficient.
- Coarse-token REST APIs require a thin Kaizen allowlisting proxy for selected read/navigation endpoints.
- The proxy must hold the upstream credential, enforce scope, validate parameters, and expose no generic pass-through operation.