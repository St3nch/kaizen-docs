# Claude Research Prompt 004 - Obsidian MCP Tool-Surface Comparative Audit

Status: active research prompt
Date: 2026-06-05

## Prompt

You are conducting the Obsidian and filesystem portion of Kaizen Research Step 5A.

This is a comparative research and architecture-evaluation task only.

Do not edit the Kaizen repository.
Do not create files in it.
Do not stage, commit, push, reset, clean, or discard changes.
Do not install Obsidian plugins or MCP servers on the user's machine.
Do not connect any candidate to a real vault.
Do not select a final server solely from README claims.
Do not produce an implementation roadmap.

Return a report only.

## Repository

Read the live local planning repository:

```text
C:\dev\kaizen-docs
```

Start by reporting:

```text
git status
git log -5 --oneline
```

Treat this repository as Kaizen's planning and stewardship workspace, not the future production vault.

## Central objective

Compare prominent Obsidian-specific MCP servers, filesystem MCP servers, and Obsidian REST/MCP bridges against Kaizen's accepted authority, safety, portability, and agent-tool requirements.

The purpose is to determine whether Kaizen should later:

```text
reuse
wrap
fork
use read-only
reject
or build a narrow Kaizen-owned tool surface
```

Do not decide from popularity or feature count. Evaluate the actual tools, schemas, source code, permission model, path handling, logging, maintenance, and failure behavior.

## Required Kaizen reading

Read these files first:

```text
00-entrypoint/LLM_START_HERE.md
01-project-standard/kaizen-vision-and-architecture-alignment.md
ROADMAP.md
04-design-decisions/0001-two-zone-agent-write-model.md
04-design-decisions/0002-search-before-create-and-diff-before-write.md
04-design-decisions/0003-raw-markdown-is-canonical.md
04-design-decisions/0005-api-only-structured-data-access.md
04-design-decisions/0006-hammer-tests-are-a-hard-gate.md
04-design-decisions/0007-foundation-resolution-for-v0.2.md
05-specs/staging-write-wrapper-and-promotion-recovery.md
05-specs/staging-and-promotion-workflow.md
05-specs/promotion-event-schema.md
05-specs/kaizen-validation-gate-spec.md
05-specs/kaizen-hammer-test-strategy.md
07-hermes/hermes-permission-matrix.md
07-hermes/hermes-write-access-preconditions.md
07-hermes/hermes-first-read-only-test.md
07-hermes/hermes-first-staging-write-test.md
08-obsidian/README.md
03-research-results/008-agent-access-and-write-safety-claude-summary.md
```

## Accepted Kaizen constraints

Treat these as fixed unless you identify a contradiction requiring human review:

- canonical Markdown remains portable and understandable without plugins;
- Obsidian is the primary human interface, not the canonical data owner;
- Hermes and other agents must not receive broad canonical filesystem mutation;
- agent writes begin only through a narrow staging-create wrapper after later implementation gates pass;
- search-before-create and diff-before-write are mandatory;
- MCP is not an authorization boundary by itself;
- configured roots and prompt instructions are not sufficient security controls;
- final resolved-path confinement, operating-system permissions, audit evidence, and hammer tests are required;
- delete, move, rename, terminal, command execution, plugin management, and canonical promotion must not be exposed to Hermes clerk workflows;
- the future read-only Hermes test belongs to the implementation roadmap, not current planning;
- no candidate receives approval from this research alone.

## Candidate discovery

Research current maintained candidates as of the date of your report.

At minimum, verify and inspect:

1. the Obsidian community plugin the user remembers as likely named `Vault as MCP`;
2. prominent Obsidian MCP community plugins;
3. Obsidian Local REST API plus any built-in or companion MCP surface;
4. at least one direct filesystem MCP implementation used with Markdown vaults;
5. at least one read-only or search-focused Obsidian/Markdown MCP implementation;
6. any other candidate that is materially prominent, actively maintained, or architecturally distinct.

Do not assume candidate names from this prompt are exact. Verify:

- official plugin name;
- plugin ID;
- repository URL;
- maintainer or organization;
- latest release date;
- latest commit date;
- installation path;
- transport;
- dependencies.

If the remembered plugin cannot be verified, say so clearly.

## Source standards

Prefer primary sources:

- official GitHub repositories;
- source code;
- manifests and plugin registry entries;
- MCP tool schemas;
- release notes;
- official documentation;
- official issue trackers;
- security advisories and CVE databases.

Use secondary sources only to discover candidates or corroborate adoption.

For every candidate, record the date checked.

Do not infer security from the absence of reported vulnerabilities.

## Required candidate analysis

### 1. Maintenance and provenance

Record:

- repository and owner;
- license;
- release cadence;
- latest release and commit;
- open issues and unresolved security-relevant issues;
- contributor concentration;
- archived, abandoned, experimental, or maintained status;
- whether the Obsidian community registry includes it.

### 2. Architecture and transport

Determine:

- in-process Obsidian plugin, external MCP server, REST bridge, or direct filesystem service;
- STDIO, HTTP, SSE, WebSocket, local REST, or other transport;
- authentication model;
- local-only assumptions;
- whether Obsidian must be open;
- plugin and API dependencies;
- whether the server can operate without Obsidian;
- Windows-specific assumptions.

### 3. Complete tool inventory

Inspect tool registration and schemas in source code.

Classify every exposed tool as one of:

```text
read
search
metadata read
create
append
patch
replace
overwrite
move or rename
delete
command execution
plugin or skill management
administrative
other
```

For each tool, record:

- exact tool name;
- request fields;
- response shape;
- filesystem or Obsidian operation performed;
- whether it can create or mutate canonical content;
- whether it can be omitted entirely from tool discovery;
- whether read-only mode removes it or merely rejects at runtime;
- logging and error behavior.

### 4. Read-only separation

Determine whether the candidate supports a genuinely reduced read-only surface.

Distinguish:

```text
tool absent from discovery
tool exposed but denied
tool exposed and prompt-restricted
configuration-only read-only mode
operating-system enforced read-only
unclear
```

Check whether read-only mode still exposes indirect mutation through:

- command execution;
- Obsidian commands;
- template actions;
- plugin APIs;
- rename or move;
- metadata mutation;
- append-upsert behavior;
- automatic file creation;
- indexing or cache writes inside the vault.

### 5. Path and vault confinement

Inspect actual path validation code.

Determine whether the candidate uses:

- lexical normalization;
- string-prefix checks;
- canonical or real-path resolution;
- handle-based final-path resolution;
- symlink handling;
- Windows junction or reparse-point handling;
- strict descendant checks with separator boundaries;
- allowed and denied subpaths;
- separate read and write roots;
- extension allowlists;
- overwrite protections;
- race-resistant operations.

Evaluate these path forms:

```text
relative traversal
absolute path
drive-relative path
UNC path
device path
extended-length path
mixed separators
case variants
symbolic-link escape
Windows junction escape
sibling-repository path
existing-file overwrite
```

Classify each candidate's confinement as:

```text
strongly evidenced
partially evidenced
string-only or likely insufficient
not implemented
unclear
```

### 6. Search and retrieval capabilities

Compare:

- filename and path search;
- full-text search;
- regex search;
- frontmatter and tag search;
- backlinks and outgoing links;
- headings and block references;
- semantic search;
- result ranking;
- snippets and document maps;
- pagination and result limits;
- project or folder filters;
- stale or excluded content handling.

Identify which search features are useful for future Kaizen read-only integration and which require unacceptable plugin-owned truth.

### 7. Mutation semantics

For every mutation tool, determine:

- create-only versus upsert;
- append behavior when a file does not exist;
- overwrite defaults;
- targeted patch behavior;
- optimistic concurrency or expected-hash checks;
- diff generation;
- delete permanence or trash behavior;
- rename and move behavior;
- automatic frontmatter changes;
- validation before mutation;
- audit evidence;
- rollback or recovery.

Compare these semantics against:

```text
05-specs/staging-write-wrapper-and-promotion-recovery.md
```

### 8. Obsidian command execution

If a candidate can execute Obsidian commands or arbitrary plugin actions:

- inventory the tool and its parameters;
- determine whether it can be fully absent or disabled;
- identify destructive or authority-bypassing possibilities;
- inspect allowlist or denylist support;
- classify it as incompatible, wrapper-required, or potentially safe for a non-Hermes administrative profile.

### 9. Logging, provenance, and observability

Determine whether the candidate records:

- actor/client identity;
- session;
- tool name;
- request ID;
- target path;
- result;
- mutation hash;
- denial reason;
- timestamps;
- transport/authentication outcome;
- audit persistence.

Check whether logs may expose vault contents, secrets, or private data.

### 10. Authentication and exposure

Evaluate:

- unauthenticated local servers;
- API keys;
- bearer tokens;
- OAuth or MCP authorization support;
- origin and host binding;
- LAN exposure;
- HTTPS requirements;
- credential storage;
- token rotation;
- request replay;
- client isolation.

### 11. Kaizen fit

For each candidate, classify:

```text
reuse as-is
reuse read-only only
wrap behind Kaizen gateway
fork and reduce tool surface
use only as a human/admin utility
reject
needs hands-on verification
```

Explain why.

Do not recommend a candidate for Hermes staging writes unless it can satisfy the accepted wrapper and hammer-test requirements. General-purpose filesystem or Obsidian CRUD tools are not equivalent to the Kaizen staging-create wrapper.

## Comparative baseline

Include an explicit comparison between these architectural options:

### Option A - Obsidian plugin MCP

```text
agent -> MCP plugin inside Obsidian -> vault
```

### Option B - Local REST bridge

```text
agent -> MCP wrapper -> Obsidian Local REST API -> vault
```

### Option C - Direct filesystem read/search MCP

```text
agent -> filesystem MCP -> Markdown files
```

### Option D - Kaizen-owned typed read/search service

```text
agent -> Kaizen typed tools -> validated Markdown/query services
```

### Option E - Hybrid

```text
existing read/search integration
+
Kaizen-owned staging-create wrapper
```

Compare:

- portability;
- security boundary quality;
- Obsidian dependency;
- Windows behavior;
- search richness;
- logging;
- maintenance burden;
- testability;
- fit for Hermes, GPT, and Claude;
- human/admin versus agent use.

## Hands-on prototype recommendations

Do not run prototypes in this task.

For any candidate worth further consideration, define a disposable test plan covering:

- empty disposable vault;
- dedicated low-privilege Windows identity;
- exact server version and configuration capture;
- tool-discovery snapshot;
- read-only probes;
- attempted mutation probes;
- traversal, UNC, extended path, symlink, and junction probes;
- overwrite and sibling-directory probes;
- log inspection;
- cleanup;
- success and failure evidence.

## Required output

Return one report with these sections.

### 1. Executive verdict

State:

- whether any candidate is suitable as-is for Kaizen;
- whether a read-only candidate appears promising;
- whether Kaizen still needs its own staging-write wrapper;
- the strongest and weakest candidates;
- the most important hands-on tests still required.

### 2. Repository and Git state observed

Include:

- branch;
- latest commit;
- clean or dirty status;
- confirmation that no files were changed.

### 3. Candidate inventory

Use columns:

```text
Candidate
Official identity
Architecture
Maintenance status
Transport/auth
Primary use
Date checked
```

### 4. Tool inventory matrix

Use columns:

```text
Candidate
Tool name
Class
Mutation potential
Can be omitted?
Read-only behavior
Kaizen concern
```

Do not collapse distinct tools into vague summaries.

### 5. Security and confinement matrix

Use columns:

```text
Candidate
Root model
Path validation
Symlink handling
Windows junction handling
Overwrite behavior
Command execution
Audit logging
Confidence
```

### 6. Search and retrieval comparison

Compare practical read/search capabilities and plugin dependencies.

### 7. Mutation-semantics comparison

Compare every candidate against the Kaizen create-only wrapper and diff-before-write model.

### 8. Authentication and deployment comparison

Cover local exposure, tokens, client isolation, and operational requirements.

### 9. Known vulnerabilities and security-relevant issues

Include direct links or citations to primary evidence.

Separate confirmed vulnerabilities from unresolved issue reports and theoretical concerns.

### 10. Kaizen disposition

For each candidate, choose:

```text
adopt
modify or wrap
fork
read-only evaluation
reject
defer
```

### 11. Recommended Kaizen tool architecture

Recommend the best current direction without selecting an implementation prematurely.

State whether the likely design is:

```text
existing read/search tool
+
Kaizen-owned typed staging wrapper
```

or another evidence-backed architecture.

### 12. Prototype and hammer-test plan

Provide a candidate-specific disposable test plan.

### 13. Files likely needing later revision

List exact Kaizen paths and describe likely changes after reconciliation.

Do not edit them.

### 14. Ordered next actions

Give the project steward a dependency-aware sequence.

### 15. Final no-write confirmation

Confirm that you:

- changed no files;
- created no files;
- installed nothing;
- staged nothing;
- committed nothing;
- pushed nothing.

## Important constraints

Do not:

- equate Obsidian community-plugin approval with Kaizen security approval;
- assume localhost means safe;
- assume read-only configuration is structurally enforced;
- trust root-prefix checks without source inspection;
- recommend broad CRUD or command-execution tools for Hermes;
- treat plugin indexes, caches, or rendered views as canonical truth;
- select a candidate because it has the most tools;
- recommend live testing against a real vault;
- weaken human-only promotion authority;
- create the implementation roadmap.

Be skeptical, source-driven, and specific.

Return the report only.