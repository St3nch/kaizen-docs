# Packet 024A — M16 Definition and Qdrant Evidence Reconciliation

Status: planning packet draft — not owner accepted
Date: 2026-06-29
Milestone: 16
Packet: 024A

## Purpose

Define the first Milestone 16 planning packet after Milestone 15 closure and Result 400 Qdrant research-context recording.

Packet 024A should reconcile the owner-supplied M16 Qdrant research report with Kaizen's accepted architecture, M15 typed-read-model proof, prior Qdrant research, and roadmap boundaries.

This packet is docs-only. It is not a Qdrant prototype packet.

## Authority basis

```text
ROADMAP_V0.4.md
00-entrypoint/LLM_START_HERE.md
03-research-results/399-milestone-15-owner-closure-acceptance.md
03-research-results/398-packet-023i-m15-integrated-proof-and-closure-audit-result.md
03-research-results/400-milestone-16-qdrant-research-context-and-hammer-test-direction.md
03-research-results/396-packet-023h-implementation-return.md
03-research-results/011-qdrant-mcp-typed-tool-audit-claude-summary.md
03-research-results/012-step-5a-composed-tool-capability-map.md
03-research-results/004-markdown-qdrant-postgres-architecture-claude-summary.md
04-design-decisions/0004-system-of-record-boundaries.md
04-design-decisions/0008-v0.2-operating-conventions.md
05-specs/kaizen-id-and-prefix-registry.md
```

## M16 candidate purpose

Milestone 16 should provide rebuildable semantic retrieval over approved Kaizen Markdown and governed context-pack assembly.

The working M16 definition is:

```text
approved canonical Markdown corpus
-> deterministic chunk/index records
-> derivative Qdrant index
-> governed retrieval gateway / typed tools
-> candidate retrieval with source traceability
-> governed context-pack assembly
-> hammer-tested leakage, freshness, privacy, supersedence, and rebuildability boundaries
```

M16 is not merely a vector-search demo.

M16 must prove that retrieval is:

```text
derivative;
scoped;
traceable;
rebuildable;
freshness-aware;
privacy-aware;
authority-aware;
supersedence-aware;
context-pack-safe;
not directly agent-administered.
```

## Current posture from M15

Milestone 15 is owner-accepted closed and established the immediate prerequisite bridge:

```text
canonical Markdown notes
-> flat frontmatter and body-heading contracts
-> parser-backed typed read model
-> validation errors and typed records
-> current-state / command-center / review / resume views
-> governed context-pack manifest assembly
-> future Tauri-ready contract
```

M15 did not implement Qdrant.

M16 should consume the M15 typed-read-model posture. It should not bypass it.

## Evidence reconciliation

The owner-supplied M16 Qdrant report is accepted as useful planning evidence, not as final doctrine.

Result 400 records that the report aligns with Kaizen because it preserves:

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

024A should preserve that reconciliation and turn it into the accepted M16 definition candidate.

## Architecture direction

Recommended M16 architecture:

```text
canonical Markdown
-> Kaizen parser / typed read model
-> Kaizen indexer
-> Qdrant derivative index
-> Kaizen retrieval gateway / typed tools
-> governed context-pack assembler
-> human / approved agent use
```

Rejected architecture:

```text
agent
-> direct Qdrant MCP or raw Qdrant API
-> Qdrant
```

Rationale:

```text
read-only retrieval can still leak wrong-project content, private content, superseded material, stale chunks, collection names, exact points, broad metadata, and raw payloads;
Qdrant database permissions do not express Kaizen authority, privacy, freshness, supersedence, context-pack inclusion, or human approval rules;
Kaizen needs a gateway and typed read-model checks as the policy boundary.
```

## M16 capability lanes

M16 should distinguish at least these lanes:

```text
1. indexer lane: controlled headless job that reads approved canonical Markdown and builds derivative index records;
2. retrieval lane: gateway-mediated project-scoped candidate retrieval;
3. context-pack lane: validates candidates against canonical Markdown and assembles governed pack manifests / reviewed packs;
4. admin lane: human/admin collection, alias, snapshot, rebuild, and diagnostic operations;
5. prohibited agent lane: direct collection selection, raw filters, scroll/list, exact-point lookup, upsert, delete, admin, snapshot, alias, or key operations.
```

No general agent profile should expose the admin or prohibited lanes.

## Corpus posture

024A should not select the final approved real corpus.

It should establish the M16 sequence:

```text
synthetic trap corpus first;
small approved real corpus later;
no broad docs/vault indexing before synthetic hammer tests pass;
no private/raw/source/evidence corpus by default;
current-state notes excluded from default semantic retrieval at first.
```

Corpus decisions should be resolved in Packet 024B.

## Chunk / payload / ID posture

024A should not select final chunk size, payload schema, or Qdrant point-ID formula.

It should preserve the supported direction:

```text
M15 typed-section / stable-heading chunking is the first candidate;
Qdrant point IDs should be deterministic from stable note identity, chunk identity, and embedding/index version;
payload fields must support project, type, authority, status, review, privacy, supersedence, freshness, source hash, index generation, and canonical source tracing;
raw private content and secrets must not be stored in payload;
exact contract belongs in Packet 024C.
```

## Retrieval gateway posture

024A should not define final tool names or final Qdrant API calls.

It should establish that the gateway must enforce:

```text
project scope;
approved corpus boundary;
note-type allowlist;
privacy class;
authority level;
status;
review state;
supersedence state;
freshness / index-generation validity;
current-state exclusion defaults;
result limits;
payload projection;
canonical source traceability;
no agent override of mandatory filters.
```

Exact retrieval gateway contract belongs in Packet 024D.

## Hammer-test posture

M16 should include hammer testing, but not against the real Kaizen corpus first.

The staged hammer approach is:

```text
1. contract/policy tests without Qdrant;
2. disposable synthetic Qdrant corpus with trap records;
3. small approved real-corpus pilot only after synthetic hammer tests pass;
4. M16 closure audit with rebuildability and leakage evidence.
```

Synthetic trap corpus should include:

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

Hammer tests should include:

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

## Recommended M16 packet sequence

024A should propose this M16 sequence:

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

This sequence may be amended later only through governed review.

## 024A deliverable

024A execution should create a docs-only M16 definition/evidence-reconciliation result.

Recommended output path:

```text
03-research-results/402-packet-024a-m16-definition-and-qdrant-evidence-reconciliation-result.md
```

That result should:

```text
summarize the owner-supplied Qdrant report;
reconcile it against accepted Kaizen doctrine and M15 closure;
define M16 purpose, architecture, capability lanes, and non-goals;
record the synthetic-first hammer-test stance;
record recommended M16 packet sequence;
carry open decisions forward into 024B through 024D;
recommend whether the owner should accept M16 definition and authorize drafting Packet 024B.
```

## Acceptance criteria for 024A execution

024A execution is complete only if:

```text
no implementation is performed;
no Qdrant instance is installed or contacted;
no embedding model is installed;
no platform files are changed;
no vault files are changed;
no staging files are changed;
no database state is changed;
M16 purpose and boundaries are clear;
report evidence is reconciled with prior internal Qdrant evidence;
open decisions are explicitly deferred to later packets;
next recommended packet is clearly named;
read path is refreshed;
docs commit is pushed.
```

## Stop conditions

Stop immediately if 024A requires:

```text
Qdrant installation;
Qdrant connection;
Qdrant Cloud account or purchase;
embedding model installation;
platform mutation;
vault mutation;
staging mutation;
database mutation;
real corpus indexing;
synthetic prototype implementation;
agent-facing retrieval tools;
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

## Non-authorization

This draft packet does not authorize M16 definition acceptance by itself.

It does not authorize:

```text
M16 implementation;
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

## Recommended owner acceptance line

Recommended owner action after review:

```text
Accept Packet 024A for docs-only M16 definition and Qdrant evidence reconciliation, with exact docs-only scope and non-authorization boundaries as listed.
```

After accepted 024A execution, the likely next packet should be:

```text
024B — Corpus Boundary and Governance Contract
```
