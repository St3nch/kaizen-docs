# Research Result 009 - Obsidian MCP Tool-Surface Comparative Audit

Status: active evidence summary
Date: 2026-06-05
Source: Claude Research Step 5A report supplied by the project owner

## Purpose

Preserve and reconcile Claude's comparative audit of Obsidian-specific MCP servers, filesystem MCP servers, and Obsidian REST/MCP bridges.

This file is evidence and steward reconciliation. It does not select a final MCP server, reject the Obsidian ecosystem, authorize Hermes writes, or reduce Kaizen to a generic Markdown folder.

## Executive finding

Claude found no current candidate suitable as-is for Kaizen agent writes. The surveyed servers generally expose broad CRUD, patch, delete, move, rename, template execution, or command execution rather than Kaizen's narrow create-only staging contract.

That conclusion is adopted.

Claude also leaned toward a Kaizen-owned or direct-filesystem read/search service and away from Obsidian plugin or REST integrations for the agent path.

That conclusion is modified.

Kaizen must preserve Obsidian-aware capabilities where they provide real value, while separating them by authority and risk.

## Verified candidate landscape

The report verified the remembered Obsidian community plugin as:

```text
Vault as MCP
ebullient/obsidian-vault-mcp
```

It also compared architecturally distinct candidates including:

- Obsidian plugins that expose MCP directly;
- Local REST API bridges;
- direct-filesystem MCP servers;
- read/search-oriented vault servers;
- the reference filesystem MCP server.

The report found recurring broad capabilities such as:

- note creation and overwrite;
- append and section patching;
- delete and trash;
- move and rename;
- metadata and tag mutation;
- template execution;
- Obsidian command execution;
- generic filesystem mutation.

These capabilities may be useful in selected human/admin workflows, but they are not automatically appropriate for Hermes or another constrained Kaizen clerk.

## Adopt

### No third-party write server is accepted as-is

Adopt the finding that none of the surveyed candidates implements Kaizen's required write contract:

- staging-only;
- create-new;
- no overwrite;
- final-path confinement;
- provenance;
- deterministic validation;
- append-only audit evidence;
- human-only canonical promotion;
- recoverable promotion.

The Kaizen staging-write wrapper remains required.

### Tool inventory and source inspection are mandatory

Adopt the requirement to inspect:

- actual MCP tool registration;
- request and response schemas;
- source-code path handling;
- tool omission versus runtime denial;
- authentication and exposure;
- audit logging;
- mutation semantics;
- Windows path behavior.

README claims and plugin popularity are not sufficient evidence.

### General-purpose filesystem servers are high risk

Adopt the report's warning that generic filesystem MCP servers have had real confinement failures involving prefix checks and unresolved links.

Any reused server must pass Kaizen's traversal, absolute-path, UNC, extended-path, symbolic-link, junction, overwrite, and sibling-directory hammer matrix.

### Read-only must be structural

Adopt the distinction between:

```text
tool absent from discovery
tool exposed but denied
tool exposed and prompt-restricted
```

Kaizen should prefer tools that are absent from the agent's capability surface rather than relying on runtime refusal or prompt obedience.

### Plugin-owned indexes are non-canonical

Adopt the finding that semantic or derived plugin indexes may be useful, but they are rebuildable convenience layers and cannot become canonical truth.

## Modify

### Do not reduce Obsidian to a filesystem-only read surface

Modify Claude's recommendation to prefer direct-filesystem or Kaizen-owned reads as the default answer.

Kaizen is intentionally an Obsidian vault and should benefit from Obsidian-aware capabilities such as:

- backlinks and outgoing links;
- aliases, headings, blocks, and embeds;
- property and tag search;
- Obsidian search semantics;
- active-note and reveal-in-UI actions;
- human review and navigation helpers;
- selected plugin-aware capabilities where they remain derived and non-canonical.

These should not be discarded merely because raw Markdown remains canonical.

### Use capability lanes instead of one universal MCP surface

The likely architecture is a composed tool model:

```text
Obsidian-aware read/navigation tools
+
Kaizen-governed read/search/context tools
+
Kaizen staging-create wrapper
+
human/admin-only Obsidian/plugin tools
```

A single server does not need to own every capability.

### Obsidian plugin and REST candidates are not automatically rejected

Modify the report's blanket rejection for the agent path.

Some plugin or REST tools may be suitable for constrained read/navigation operations if:

- unsafe tools are absent from the exposed profile;
- authentication and local exposure are acceptable;
- the tool does not create plugin-owned canonical truth;
- logging is enabled and adequate;
- the candidate passes disposable hands-on tests;
- the capability provides value not worth rebuilding.

Broad CRUD, delete, command execution, and template execution remain excluded from Hermes clerk profiles.

### Human/admin utilities remain part of the design

Obsidian's ecosystem may provide useful human-operated or administrative tools even when those tools are not exposed to Hermes.

Examples may include:

- opening or revealing notes;
- workspace navigation;
- template management;
- selected command execution;
- manual metadata operations;
- plugin maintenance;
- migration and repair utilities.

These capabilities require separate profiles and authority boundaries rather than blanket rejection.

### Read confinement applies to the read/navigation lane

Read-only does not mean low risk.

A read/search/backlink/property tool can still expose private or out-of-scope files if its root confinement is broken. The same traversal, absolute-path, UNC, device-path, extended-length, symbolic-link, Windows junction, sibling-directory, and final-resolved-path requirements from Batch A apply to read/navigation tools.

Every candidate read surface must therefore prove:

- final resolved-path confinement rather than string-prefix checks;
- project, folder, privacy, and exclusion boundaries;
- no access to sibling repositories or private roots;
- audit evidence for denied reads;
- safe handling of symlinks, junctions, reparse points, and aliases;
- no hidden broad-file fallback behind a narrow search tool.

The read lane is capability-reduced, not security-exempt.

### Capability is not canonical truth

Kaizen may freely use valuable capabilities such as:

- semantic search;
- Dataview or property-aware views;
- Smart Connections or other plugin-assisted retrieval;
- backlinks and link graphs;
- Obsidian search semantics;
- block, heading, alias, and embed navigation.

The governing rule is:

> Use the capability; do not let its plugin index become the only path to a fact or the system of record.

Derived indexes and views must remain rebuildable. Canonical facts, authority, provenance, and accepted project intelligence remain in portable Markdown or another explicitly governed system of record.

### Configure-versus-wrap mechanism

A candidate's safety disposition depends on how unsafe tools are removed.

#### Configurable plugin MCP

For a plugin such as `Vault as MCP`, verify whether a reduced profile makes unsafe tools absent from MCP tool discovery.

Acceptable evidence:

- delete, overwrite, patch, move, rename, command, template, and admin tools are not advertised;
- the server process cannot invoke them through a hidden generic tool;
- the reduced configuration is captured, testable, and auditable.

Runtime refusal after advertising a dangerous tool is defense in depth, not sufficient profile separation.

#### Coarse-token REST API

When one bearer token grants the full REST surface, configuration alone cannot create a safe read profile.

Preserving useful read capabilities requires a thin Kaizen proxy or gateway that:

- exposes only allowlisted read/navigation operations;
- validates and narrows parameters;
- enforces project, path, privacy, and result-size scope;
- keeps the coarse upstream credential away from the agent;
- logs every forwarded request and denial;
- exposes no generic pass-through endpoint.

This is `preserve via wrap`, not `reject` and not `rebuild the capability`.

### Headless and interactive contexts

Kaizen needs both contexts.

#### Headless canonical context

Overnight jobs, CI validation, Qdrant rebuilds, recovery, and other unattended work must not require the Obsidian GUI to be open.

They use portable canonical Markdown, deterministic parsers, and governed headless services.

#### Interactive Obsidian context

Human-adjacent and review workflows may use richer Obsidian-aware capabilities, including:

- reveal/open note;
- backlinks;
- property and tag search;
- active-note context;
- workspace navigation;
- selected plugin-assisted retrieval.

These contexts are complementary. The headless path protects portability and automation; the interactive path preserves Obsidian's ecosystem value.

The `open/reveal note` capability is a model example: it is Obsidian-native, useful for placing evidence before a human reviewer, pointless to rebuild, and non-authoritative when implemented without mutation.
## Reject

Reject:

- adopting any surveyed full-CRUD server as the Hermes write surface;
- treating plugin registry inclusion as security approval;
- exposing delete, move, rename, overwrite, command execution, template execution, or plugin administration to the Hermes clerk profile;
- treating Smart Connections, Dataview, or another plugin index as canonical truth;
- assuming localhost, a configured vault root, or a bearer token provides sufficient authorization;
- rebuilding valuable Obsidian functionality merely because Kaizen needs stricter write controls;
- making direct filesystem access the only acceptable form of agent integration.

## Defer

Defer until hands-on evidence exists:

- final Obsidian MCP server selection;
- final Local REST API use;
- final direct-filesystem read server;
- whether `Vault as MCP` can expose a sufficiently reduced read/navigation profile;
- whether an existing server should be wrapped, forked, or used only for humans;
- exact Obsidian plugin stack;
- exact semantic search integration before Qdrant planning is reconciled.

## Steward architecture direction

Kaizen should preserve four distinct capability lanes.

### Lane 1 - Canonical portable access

Purpose:

- raw Markdown reads;
- deterministic metadata parsing;
- validation;
- stable identity and relationship handling;
- operation without Obsidian.

This protects portability and recovery.

### Lane 2 - Obsidian-aware read and navigation

Potential capabilities:

- backlinks and outgoing links;
- aliases and embeds;
- heading and block navigation;
- property/tag search;
- Obsidian search;
- reveal/open note for human review;
- workspace-oriented navigation.

This lane should reuse the Obsidian ecosystem where safe and valuable.

### Lane 3 - Kaizen-governed agent writes

Only through Kaizen-owned typed operations such as:

```text
create_staged_markdown
produce_governed_diff
validate_staged_artifact
```

No third-party broad CRUD tool substitutes for this lane.

### Lane 4 - Human/admin Obsidian operations

Potentially includes broader plugin and command capabilities under explicit human control.

These tools are not part of the Hermes clerk profile and may use separate configuration, credentials, or execution environments.

## Candidate disposition

### Vault as MCP

Disposition: `read/navigation evaluation`, `human/admin evaluation`, and `source inspection required`.

It is especially important because the user has prior experience with it and it demonstrates real Obsidian ecosystem value. Its broad mutation tools prevent as-is adoption for Hermes, but its read, search, link, task, template, or periodic-note capabilities should be inventoried rather than dismissed.

### Obsidian Local REST API and MCP bridges

Disposition: `constrained capability evaluation` and `human/admin utility evaluation`.

Full CRUD, active-file mutation, command execution, optional plaintext exposure, and logging defaults require caution. Selected read/navigation functions may still be useful behind a reduced profile or gateway.

### Direct-filesystem read/search servers

Disposition: `portable read baseline` and `interim read evaluation`.

They align with canonical Markdown portability but do not replace Obsidian-aware capabilities. Any candidate requires source inspection and Windows confinement tests.

### Semantic or plugin-indexed search

Disposition: `derived convenience layer only`.

Potentially useful for human exploration or agent recall, but must remain rebuildable and subordinate to canonical Markdown and the future Qdrant design.

### Generic filesystem MCP server

Disposition: `reference and hammer-test target`, not preferred production dependency.

Its vulnerability history provides important test cases.

## Configure-versus-wrap matrix

| Candidate type | Safe reduction mechanism | Required proof | Likely disposition |
|---|---|---|---|
| Plugin MCP with per-profile tool configuration | Configure | Unsafe tools absent from discovery and no hidden generic escape tool | Reuse selected read/navigation capabilities if confinement passes |
| Plugin MCP with advertised-but-denied unsafe tools | Wrap or fork | Gateway removes tools before discovery; server cannot bypass gateway | Do not expose directly to Hermes |
| Local REST API with one coarse bearer token | Wrap | Thin allowlisted proxy, scoped parameters, credential isolation, logging | Preserve selected read/navigation endpoints through Kaizen gateway |
| Direct-filesystem read/search MCP | Configure plus wrap where needed | Final-path confinement, private-root exclusion, read-only OS identity, hammer tests | Portable/headless read candidate |
| Semantic/plugin index | Configure as derived retrieval | Rebuildability, source links, no exclusive facts, freshness labeling | Convenience layer only |
| Broad CRUD or command server | Human/admin profile or reject | Separate identity, explicit human operation, no Hermes discovery | Never the clerk write surface |
## Required hands-on comparison

Before selecting an Obsidian integration, create a disposable test program that records:

- exact plugin/server version;
- exact configuration;
- complete advertised tool list;
- whether unsafe tools can be absent;
- read/search/navigation behavior;
- hidden or indirect file mutations;
- plugin cache/index writes;
- authentication and local exposure;
- audit logging;
- traversal, UNC, extended path, symlink, and junction behavior for both read and write attempts;
- private-root and sibling-repository read-denial behavior;
- whether unsafe tools are absent from discovery or merely denied at runtime;
- whether coarse upstream credentials require a Kaizen read proxy;
- usefulness of Obsidian-aware features compared with raw filesystem reads.

The comparison should explicitly measure value, not only danger.

## Value-preservation questions

Each candidate audit must answer:

1. Which capabilities depend on Obsidian and would be costly or foolish to rebuild?
2. Which capabilities are merely wrappers around ordinary Markdown filesystem operations?
3. Which capabilities are safe for a read/navigation profile?
4. Which capabilities belong only in a human/admin profile?
5. Which capabilities must remain Kaizen-owned because they carry authority?
6. Can the candidate expose multiple profiles or servers with genuinely different tool sets?
7. Does the candidate improve the human-agent review loop inside Obsidian?

## Roadmap consequences

Step 5A should not aim only to find the safest minimal server.

It should produce a capability map across:

```text
portable canonical access
Obsidian-aware read/navigation
Kaizen-governed writes
human/admin plugin operations
```

Postgres and Qdrant audits should use the same principle: preserve useful ecosystem capabilities while separating read, write, administrative, destructive, and authority-bearing operations.

## Likely future decision

After candidate source audits and disposable prototypes, Kaizen may need a decision defining its composed agent-tool architecture.

That decision should not preselect a product. It should define:

- capability lanes;
- profile separation;
- canonical and derived truth;
- tool discovery requirements;
- authentication and logging expectations;
- which operations must be Kaizen-owned;
- which ecosystem capabilities may be reused.

## Ordered next actions

1. Preserve this report and reconciliation as evidence.
2. Update the Obsidian posture to explicitly preserve ecosystem value.
3. Prepare the Postgres and Qdrant comparative prompts using capability lanes, read confinement, configure-versus-wrap analysis, and headless-versus-interactive context separation.
4. Before final Obsidian selection, source-audit and prototype the most valuable candidates, including `Vault as MCP`.
5. Keep the Kaizen staging-write wrapper as the only proposed Hermes write path.
6. Do not adopt or reject an Obsidian integration until both safety and ecosystem value are measured.

## Source limitation

The raw Claude report was supplied as an uploaded Markdown artifact outside the Windows repository environment. This file preserves its load-bearing findings and the project steward's reconciliation. The raw report should also be retained when the repository environment can access it.