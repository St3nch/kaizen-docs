# Packet 024B — Corpus Boundary and Governance Contract Result

Status: complete — docs-only corpus boundary and governance contract accepted for execution
Date: 2026-06-29
Milestone: 16
Packet: 024B
Execution repository: `C:\dev\kaizen-docs`

## Purpose

Record execution of Packet 024B after explicit owner acceptance.

Packet 024B defines the first Milestone 16 corpus boundary and governance contract before any Qdrant prototype, chunking implementation, embedding installation, platform mutation, vault mutation, or real corpus indexing occurs.

This result is docs-only. It does not implement Qdrant, install Qdrant, contact Qdrant, install embeddings, mutate platform, mutate vault, mutate staging, mutate database state, create synthetic corpus files, generate a corpus manifest, index a real corpus, or expose agent-facing retrieval tools.

## Owner acceptance

The owner accepted Packet 024B with this instruction:

```text
Accept Packet 024B for docs-only corpus boundary and governance contract, with exact docs-only scope and non-authorization boundaries as listed.
```

The accepted packet draft was:

```text
03-research-results/403-packet-024b-corpus-boundary-and-governance-contract.md
```

## Starting posture

Pre-execution posture verified:

```text
docs branch: main
docs starting commit: eb7a8b866adc79cf9f99ee551d9ad09296854853
docs working tree: clean and synced
platform branch: main
platform commit: 84071eef1e5859f651d3213c65f3bfc99a3c94f8
platform working tree: clean
vault branch: main
vault commit: 6fbc8cefe4ed4cd57403648ee3977ee2aeaaba5b
vault working tree: clean
```

## Authority basis

024B executes against:

```text
03-research-results/403-packet-024b-corpus-boundary-and-governance-contract.md
03-research-results/402-packet-024a-m16-definition-and-qdrant-evidence-reconciliation-result.md
03-research-results/400-milestone-16-qdrant-research-context-and-hammer-test-direction.md
03-research-results/399-milestone-15-owner-closure-acceptance.md
03-research-results/398-packet-023i-m15-integrated-proof-and-closure-audit-result.md
03-research-results/396-packet-023h-implementation-return.md
03-research-results/011-qdrant-mcp-typed-tool-audit-claude-summary.md
03-research-results/012-step-5a-composed-tool-capability-map.md
03-research-results/004-markdown-qdrant-postgres-architecture-claude-summary.md
04-design-decisions/0004-system-of-record-boundaries.md
04-design-decisions/0008-v0.2-operating-conventions.md
05-specs/kaizen-id-and-prefix-registry.md
ROADMAP_V0.4.md
00-entrypoint/LLM_START_HERE.md
```

## Corpus class taxonomy

M16 corpus planning now distinguishes these corpus families:

```text
synthetic trap corpus;
small approved real docs corpus;
small approved real vault corpus;
excluded private/raw/source/evidence material;
volatile operating notes;
historical/superseded material;
downstream project material;
future Observatory / IMI material.
```

## Default corpus posture

024B accepts this default posture for M16 planning:

```text
synthetic trap corpus is first and required before any real corpus;
real corpus pilot is later and requires explicit owner approval;
docs and vault content remain separable by corpus boundary;
raw/private/source/evidence material is excluded by default;
current-state notes are excluded from default semantic retrieval;
command-center notes are excluded from default semantic retrieval unless a later typed retrieval path explicitly includes them;
draft/proposed/rejected/archived/superseded records are excluded from default retrieval;
accepted/approved canonical project-intelligence records are the only default real-corpus candidates;
implementation-return records may be valuable but require explicit include/deny decision before first real corpus;
Neon Ronin and SearchClarity content must not be indexed as Kaizen corpus material unless separately selected as a governed workload/corpus;
Observatory / IMI data remains out of scope.
```

## Synthetic corpus contract

The first Qdrant prototype corpus must be synthetic and deliberately adversarial.

The synthetic corpus is safe for first use because:

```text
it contains no private, customer, credential, provider, commercial, or real project-sensitive content;
it is created only for retrieval-boundary proof;
it can be deleted/rebuilt without evidence loss;
it exists to prove failure modes before real corpus indexing;
it does not become canonical Kaizen knowledge.
```

Synthetic trap records should include:

```text
Project A approved decision;
Project B approved decision with similar wording;
private note that should never return;
superseded note that matches the query perfectly;
current-state note that should be excluded by default;
command-center note that should be excluded by default;
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

The synthetic corpus is for include/exclude policy and failure-mode proof, not retrieval-quality benchmarking.

## Real corpus candidate posture

024B does not approve any real corpus for indexing.

A later real corpus pilot may include only explicitly selected, approved, non-private, canonical Markdown records.

Candidate include families for later approval:

```text
accepted design decisions;
accepted specs;
accepted milestone closure records;
accepted implementation returns;
accepted source-summary / claim / audit records if they are promoted and non-private;
accepted project-standard records;
accepted note type / field / ID registry records.
```

Candidate exclude families by default:

```text
raw research dumps;
private notes;
source evidence bytes;
provider/customer data;
credentials or secret-bearing records;
staging records;
backup/recovery archives;
runtime logs;
chat transcripts;
unreviewed pasted reports;
draft planning records;
proposed/rejected/superseded/archived records;
current-state notes;
command-center notes;
Obsidian plugin state;
Tauri state;
Qdrant generated artifacts;
Postgres operational records;
Neon Ronin implementation material;
SearchClarity implementation material;
Observatory / IMI observations.
```

## Docs versus vault separation

024B accepts this initial separation posture:

```text
docs corpus and vault corpus remain separate corpus families;
synthetic prototype uses synthetic-only corpus;
first real pilot may select either docs-only, vault-only, or explicitly paired docs+vault corpus;
paired docs+vault corpus must still preserve source_kind and corpus_boundary fields;
Qdrant collection/alias strategy is deferred to 024C, but corpus boundary must not assume docs and vault can be mixed without policy labels.
```

Reason:

```text
docs and vault serve different authority/evidence purposes;
retrieval leakage across source families can confuse canonical project-intelligence, implementation evidence, and operational records;
separation improves later collection, alias, payload, rebuild, and audit design.
```

## Status / authority eligibility rules

Default retrieval eligibility for any future real corpus is:

```text
status must be accepted or active only where the note type is appropriate and accepted by governance;
authority_level must be accepted for default real-corpus retrieval;
review_state must not be proposed, ready, in-review, rejected, or superseded for default retrieval;
supersedence_state must be current / not superseded;
privacy_class must be public/project-internal-approved only, never private/secret/customer/provider/raw;
source_kind must be canonical Markdown, not generated index output;
record must have stable source path and content hash.
```

Any exception must be explicit, typed, and owner-approved.

## Current-state and command-center rule

024B preserves the conservative rule:

```text
current-state notes are excluded from default semantic retrieval at first;
command-center notes are excluded from default semantic retrieval at first;
any retrieval over current-state or command-center notes requires a later explicit typed tool / retrieval mode and owner-approved tests.
```

Reason:

```text
current-state and command-center notes are volatile operating surfaces;
they may contain transient priorities, blocker descriptions, and context that should not contaminate generic retrieval;
M15 already supplies typed resume/current-state/command-center views without requiring semantic indexing.
```

## Privacy and raw evidence rule

024B accepts this fail-closed privacy rule:

```text
if privacy class is missing, ambiguous, unsupported, private, secret, customer-data, provider-restricted, source-evidence, or raw-evidence, the record is not eligible for indexing;
if source rights or data ownership are unclear, the record is not eligible for indexing;
if the content would be unsafe to expose through a retrieval result, the content is not eligible for Qdrant payload or vector text.
```

This rule applies before Qdrant ingestion, not merely at retrieval time.

## Real corpus approval gate

A later explicit owner approval is required before any real corpus indexing.

Real corpus approval must identify:

```text
corpus name;
source roots;
allowed path patterns;
excluded path patterns;
allowed note types;
allowed statuses;
allowed authority levels;
allowed privacy classes;
current-state / command-center rule;
supersedence rule;
freshness rule;
source hash rule;
expected approximate record count;
whether docs-only, vault-only, or paired docs+vault;
reviewer acceptance line;
non-authorization boundaries.
```

Without that approval, only synthetic corpus planning/prototype work may proceed in later accepted implementation packets.

## Validation and audit requirements

Later packets must preserve these validation requirements:

```text
corpus manifest can be generated from canonical Markdown records;
manifest records include included and excluded paths;
each included item has source path, source kind, note type, status, authority, privacy, supersedence, and hash evidence;
excluded records have exclusion reason;
missing/ambiguous metadata fails closed;
real corpus manifest must be reviewed before indexing;
synthetic corpus manifest must include deliberate trap records;
no generated Qdrant output is canonical.
```

Implementation of this manifest belongs to later packets only if approved.

## Open decisions carried to 024C and later

024B carries these decisions forward:

```text
exact chunk ID formula;
exact Qdrant point ID formula;
minimum payload schema and payload indexes;
collection-per-generation versus other generation strategy;
alias cutover and rollback rules;
snapshot and restore proof scope;
embedding model baseline and fallback;
local server versus Python local mode for different test classes;
gateway typed-tool names and result shape;
context-pack manifest versus generated bundle posture;
human review surface for retrieval-backed context packs;
first real corpus include/deny decision for implementation-return records;
final docs-only / vault-only / paired docs+vault real pilot decision.
```

## Non-authorization preserved

024B execution did not authorize:

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
synthetic corpus file creation;
real corpus indexing;
corpus manifest generation;
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

## Changed docs paths for 024B execution

This execution creates this result record and refreshes the active read path:

```text
03-research-results/404-packet-024b-corpus-boundary-and-governance-contract-result.md
00-entrypoint/LLM_START_HERE.md
ROADMAP_V0.4.md
```

## Recommended next action

Proceed to draft the next docs-only planning packet:

```text
024C — Chunk, Payload, ID, and Rebuild Contract
```

Recommended 024C goal:

```text
define M16 chunk boundaries, required payload fields, deterministic chunk IDs, deterministic Qdrant point IDs, source hashing, index-generation model, alias/cutover posture, stale-point handling, and rebuild proof requirements before any synthetic Qdrant prototype is authorized.
```

Recommended owner acceptance line after review of this result:

```text
Accept Packet 024B completion and authorize drafting Packet 024C chunk, payload, ID, and rebuild contract as docs-only planning, with no Qdrant implementation or corpus indexing.
```
