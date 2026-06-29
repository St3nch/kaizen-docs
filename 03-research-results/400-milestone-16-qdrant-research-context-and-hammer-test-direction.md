# Milestone 16 Qdrant Research Context and Hammer-Test Direction

Status: evidence/context recorded — planning direction, not implementation authorization
Date: 2026-06-29
Milestone: 16 candidate
Related roadmap lane: `Milestone 16 — Qdrant retrieval and governed context packs`

## Purpose

Record the owner-supplied Milestone 16 Qdrant research report and the immediate steward reconciliation after Milestone 15 closure.

This result captures the current M16 direction before any M16 definition/spec, task packet, Qdrant installation, platform mutation, vault mutation, or prototype work begins.

## Source inputs

Owner-supplied research report:

```text
Kaizen Milestone 16 Research Report
```

Current accepted project context:

```text
ROADMAP_V0.4.md
00-entrypoint/LLM_START_HERE.md
03-research-results/399-milestone-15-owner-closure-acceptance.md
03-research-results/398-packet-023i-m15-integrated-proof-and-closure-audit-result.md
03-research-results/396-packet-023h-implementation-return.md
03-research-results/011-qdrant-mcp-typed-tool-audit-claude-summary.md
03-research-results/012-step-5a-composed-tool-capability-map.md
03-research-results/004-markdown-qdrant-postgres-architecture-claude-summary.md
04-design-decisions/0004-system-of-record-boundaries.md
04-design-decisions/0008-v0.2-operating-conventions.md
05-specs/kaizen-id-and-prefix-registry.md
```

## Current Kaizen posture

Milestone 15 is owner-accepted closed.

M15 proved:

```text
canonical Markdown notes
-> flat frontmatter and body-heading contracts
-> parser-backed typed read model
-> validation errors and typed records
-> current-state / command-center / review / resume views
-> governed context-pack manifest assembly
-> future Tauri-ready contract
```

M15 did not authorize Qdrant implementation.

The current roadmap candidate after M15 is:

```text
Milestone 16 — Qdrant retrieval and governed context packs
```

## Research report disposition

The owner-supplied research report is accepted as useful M16 evidence input, not as automatic doctrine or implementation authorization.

The report is directionally aligned with Kaizen because it preserves these boundaries:

```text
Markdown remains canonical;
Qdrant is derivative only;
Qdrant must be rebuildable from approved Markdown;
Qdrant results must trace back to canonical Markdown;
agents must not receive direct unrestricted Qdrant access;
retrieval must be mediated by typed tools / a Kaizen retrieval gateway;
context packs remain governed bundles, not raw vector-search output;
current-state notes should be excluded from default semantic retrieval at first;
M16 must not drift into Hermes, Tauri, Observatory, customer data, provider collection, commercial operations, or broad UI.
```

## Architecture direction carried forward

The preferred M16 architecture is:

```text
canonical Markdown
-> Kaizen parser / typed read model
-> Kaizen indexer
-> Qdrant derivative index
-> Kaizen retrieval gateway / typed tools
-> governed context-pack assembler
-> human / approved agent use
```

Rejected shortcut:

```text
agent
-> direct Qdrant MCP or raw Qdrant API
-> Qdrant
```

Reason:

```text
read-only retrieval can still leak wrong-project content, private content, superseded material, stale chunks, collection names, exact points, broad metadata, and raw payloads;
Qdrant database permissions do not express Kaizen authority, privacy, freshness, supersedence, context-pack inclusion, or human approval rules;
Kaizen needs a gateway and typed read-model checks as the policy boundary.
```

## Hammer-test stance

The owner asked whether Qdrant should be hammer tested and noted that useful testing requires content to search.

Steward answer recorded here:

```text
Yes, hammer test the Qdrant design.
No, do not start by indexing the real Kaizen docs/vault corpus.
```

M16 should hammer test Qdrant through staged content:

```text
1. contract/policy tests without Qdrant;
2. disposable synthetic Qdrant corpus with trap records;
3. small approved real-corpus pilot only after synthetic hammer tests pass;
4. M16 closure audit with rebuildability and leakage evidence.
```

## Synthetic hammer corpus direction

The first Qdrant prototype corpus should be synthetic Markdown-derived content deliberately designed to tempt failure.

Recommended trap records:

```text
Project A approved decision;
Project B approved decision with similar wording;
private note that should never return;
superseded note that matches the query perfectly;
current-state note that should be excluded by default;
draft note;
proposed note;
rejected note;
archived note;
bad-metadata note;
stale old-generation chunk;
prompt-injection chunk;
missing-source-hash chunk;
wrong-project payload chunk;
embedding-version-mismatch chunk.
```

The point is not to test whether Qdrant can retrieve semantically similar text. The point is to prove the retrieval gateway, payload filters, rebuild contract, context-pack assembler, and source-traceability checks fail closed.

## Hammer tests to carry into M16

M16 should include hammer tests for:

```text
wrong-project leakage;
private-class leakage;
superseded content leakage;
stale index results;
missing source path/hash;
agent-supplied filter override;
raw scroll/list dump attempts;
exact point lookup attempts;
collection-name leakage;
source text blob without canonical trace;
embedding model mismatch;
corrupted payload metadata;
duplicate/stale chunks after rebuild;
current-state default retrieval leakage;
prompt injection inside retrieved Markdown.
```

Expected behavior:

```text
unsafe candidates are absent from gateway results or rejected by context-pack assembly;
agent-facing tools expose no raw scroll/list/exact-point/admin path;
all accepted candidate results trace back to canonical Markdown through stable source identifiers;
rebuild/alias/generation checks prove Qdrant is current and derivative;
retrieved Markdown is treated as untrusted evidence, not executable instruction.
```

## M16 sequencing recommendation

Recommended M16 packet sequence after this context record:

```text
024A — M16 definition and Qdrant evidence reconciliation
024B — Corpus boundary and governance contract
024C — Chunk, payload, ID, and rebuild contract
024D — Retrieval gateway and typed tool contract
024E — Disposable synthetic local prototype
024F — Hammer tests and prototype implementation return
024G — Approved real-corpus pilot
024H — M16 closure audit
```

024A should be docs-only.

024A should not authorize implementation, Qdrant install, platform mutation, vault mutation, real corpus indexing, or agent-facing tools.

## Decisions already strongly supported

The research and current Kaizen posture strongly support:

```text
Architecture D: Kaizen indexer + governed retrieval gateway + separate human/admin Qdrant tooling;
local-first staged prototype before any Qdrant Cloud dependency;
synthetic trap corpus before real corpus;
M15 typed-section / stable-heading chunking as the first chunking direction;
dense local embeddings plus typed filters as the first retrieval mode;
hybrid dense+sparse retrieval deferred until the governance plane is proven;
current-state notes excluded from default semantic retrieval;
context packs assembled by Kaizen from verified candidates, not emitted directly by Qdrant;
no direct Qdrant MCP or raw Query API for agents.
```

## Open decisions for 024A through 024D

Do not settle these casually in this result. Carry them into bounded M16 planning packets:

```text
docs versus vault collection separation;
first approved real corpus boundary;
whether approved implementation-return records enter the first real corpus;
exact current-state and command-center retrieval exclusion rules;
exact chunk ID formula;
exact Qdrant point ID formula;
minimum payload schema and payload indexes;
embedding model baseline and fallback;
local server versus Python local mode for different test classes;
collection-per-generation versus other generation strategy;
alias cutover and rollback rules;
snapshot and restore proof scope;
gateway typed-tool names and result shape;
context-pack manifest versus generated bundle posture;
human review surface for retrieval-backed context packs.
```

## Non-authorization

This result does not authorize:

```text
Milestone 16 owner acceptance;
Packet 024A implementation;
Qdrant installation;
Qdrant connection;
Qdrant Cloud account or purchase;
embedding model installation;
platform mutation;
vault mutation;
staging mutation;
database mutation;
real corpus indexing;
canonical context-pack file generation;
agent-facing Qdrant tools;
Hermes write authority;
Tauri implementation;
Observatory / IMI implementation;
provider collection;
customer-data reuse;
commercial operations;
production deployment;
backup deletion;
restore work.
```

## Recommended next action

Draft a docs-only M16 definition/spec and evidence-reconciliation packet:

```text
024A — M16 Definition and Qdrant Evidence Reconciliation
```

Recommended 024A goal:

```text
turn the owner-supplied M16 Qdrant research and prior internal Qdrant evidence into an accepted Milestone 16 definition candidate, with explicit architecture, non-goals, packet sequence, stop conditions, and open decisions for later packets.
```
