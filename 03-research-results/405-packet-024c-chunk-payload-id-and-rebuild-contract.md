# Packet 024C — Chunk, Payload, ID, and Rebuild Contract

Status: planning packet draft — not owner accepted
Date: 2026-06-29
Milestone: 16
Packet: 024C

## Purpose

Define the next Milestone 16 planning packet after Packet 024B completed the corpus boundary and governance contract.

Packet 024C should define the chunk, payload, deterministic identity, source-hash, index-generation, alias/cutover, stale-point, and rebuild-proof contracts before any Qdrant prototype, embedding installation, platform mutation, synthetic corpus file creation, or corpus indexing occurs.

This packet is docs-only. It is not a Qdrant implementation packet.

## Authority basis

```text
ROADMAP_V0.4.md
00-entrypoint/LLM_START_HERE.md
03-research-results/404-packet-024b-corpus-boundary-and-governance-contract-result.md
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
```

## 024C objective

Create a docs-only technical contract for how approved Markdown corpus records become deterministic derivative Qdrant index records.

The contract should answer:

```text
how Markdown records are chunked;
which payload fields are required, optional, or forbidden;
which payload fields must be filterable/indexable later;
how chunk IDs are generated;
how Qdrant point IDs are generated;
how source hashes are computed and used;
how embedding model/version participates in identity;
how index generations are named and proven;
how alias/cutover should work later;
how stale/deleted/superseded chunks are detected;
how rebuild proof and hammer tests should verify determinism;
what must remain deferred to 024D retrieval gateway planning and later implementation packets.
```

024C must not install, run, connect to, or configure Qdrant.

## Working M16 definition carried forward

M16 working definition:

```text
approved canonical Markdown corpus
-> deterministic chunk/index records
-> derivative Qdrant index
-> governed retrieval gateway / typed tools
-> candidate retrieval with source traceability
-> governed context-pack assembly
-> hammer-tested leakage, freshness, privacy, supersedence, and rebuildability boundaries
```

024C owns:

```text
deterministic chunk/index records;
source traceability;
rebuildability boundaries;
stale and supersedence detection contract.
```

## Chunking contract direction

024C should define the first chunking strategy as:

```text
M15 typed-section chunks where the note type has a stable typed read-model contract;
stable Markdown heading-bounded chunks where typed sections are not yet defined;
paragraph-bound deterministic sub-splits only when a section or heading chunk exceeds a later-approved size threshold;
no free sliding windows across heading or typed-section boundaries for the first prototype;
whole-note chunks only for tiny records that naturally contain one stable section.
```

Reason:

```text
typed-section and heading-bounded chunks preserve traceability;
they reduce authority/status/privacy mixing inside a chunk;
they support deterministic chunk IDs;
they make supersedence and stale detection auditable;
they align with M15 parser-backed Markdown contracts;
free sliding windows are deferred because they smear boundaries and complicate rebuild proof.
```

024C should not choose final token/character limits. It may define the required decision points for later implementation.

## Chunk boundary requirements

Each chunk must be traceable to:

```text
source root / source kind;
source path;
note identity where available;
note type;
heading path or typed-section name;
heading anchor where applicable;
chunk ordinal if sub-split;
source content hash;
chunker version;
canonical source commit or source snapshot identifier where applicable.
```

Chunks must never silently merge content across:

```text
different projects;
different source roots;
different note identities;
different authority levels;
different privacy classes;
different supersedence states;
different review states;
different heading/typed-section boundaries unless explicitly allowed by a later accepted rule.
```

## Payload schema direction

024C should classify payload fields into:

```text
required for M16;
optional later;
forbidden / dangerous.
```

Recommended required payload fields:

```text
project_id or project_key;
corpus_boundary;
canonical_source_kind;
source_root_id;
source_path;
source_commit_or_snapshot;
source_hash;
note_id;
note_type;
chunk_id;
chunk_ordinal;
heading_path;
heading_anchor;
authority_level;
status;
review_state;
supersedence_state;
privacy_class;
updated_or_effective_date;
index_generation_id;
chunker_version;
embedding_model_id;
embedding_model_version;
embedding_space_id;
payload_schema_version;
created_by_indexer;
traceability_status.
```

Recommended optional-later payload fields:

```text
project_slug;
source_title;
heading_title;
last_reviewed;
source_line_start;
source_line_end;
chunk_content_hash;
source_family;
corpus_manifest_id;
rights_class;
freshness_class;
reviewer_id_or_role;
quality_flags;
warning_flags.
```

Forbidden or dangerous payload fields by default:

```text
secrets;
credentials;
raw private text;
customer data;
provider-restricted data;
raw evidence bytes;
raw tool output;
chat transcripts;
unreviewed pasted report bodies;
full source document bodies unless separately accepted;
agent instructions from retrieved Markdown treated as executable instruction;
absolute local machine paths where portable/source-relative paths suffice;
API keys or Qdrant credentials;
collection names exposed for agent use;
operator-private recovery information.
```

## Filter/index field direction

024C should identify fields likely requiring Qdrant payload indexes later, without implementing them.

Recommended filter/index candidates:

```text
project_id or project_key;
corpus_boundary;
canonical_source_kind;
source_root_id;
note_type;
authority_level;
status;
review_state;
supersedence_state;
privacy_class;
updated_or_effective_date;
index_generation_id;
embedding_space_id;
payload_schema_version;
traceability_status.
```

Admin-only diagnostic fields may be indexable later but must not become agent lookup paths by default:

```text
source_path;
note_id;
chunk_id;
source_hash;
chunk_content_hash;
corpus_manifest_id.
```

## Deterministic chunk ID contract

024C should define chunk IDs as deterministic logical identifiers, not database-generated IDs.

Recommended logical formula family:

```text
chunk_id = stable_hash(
  canonical_source_kind,
  source_root_id,
  source_path,
  note_id,
  note_type,
  heading_path_or_typed_section,
  chunk_ordinal,
  chunker_version
)
```

The exact hash algorithm and encoding may be selected in 024C execution or deferred to a later implementation packet, but the contract must require:

```text
same canonical input produces same chunk_id;
content edits that do not move the heading/section keep the logical chunk identity but change source_hash / chunk_content_hash;
heading/section moves may produce a new logical chunk ID unless a later anchor-preservation rule exists;
chunker_version changes participate in identity to avoid silent collision across chunking algorithms;
no random UUIDs for logical chunk identity.
```

## Deterministic Qdrant point ID contract

Kaizen ID registry already states that future Qdrant point IDs should be deterministic from stable note identity, chunk identity, and embedding version.

024C should define the point-ID contract as:

```text
point_id = deterministic_uuid_or_hash(
  corpus_boundary,
  chunk_id,
  embedding_space_id,
  payload_schema_version
)
```

The exact point-ID formula may be finalized in 024C execution or later implementation, but it must satisfy:

```text
same chunk in same embedding space produces same point ID;
new embedding space produces distinct point IDs or distinct collection generation according to the accepted generation model;
point IDs are not derived from raw private text;
point IDs are not random for indexable logical content;
point IDs are not exposed to general agents;
point IDs are for indexer/admin/rebuild proof, not agent-facing retrieval control.
```

## Source hash contract

024C should define source hashes as canonical-source integrity evidence.

Recommended source hash requirements:

```text
hash normalized canonical Markdown bytes or a precisely specified normalized content representation;
record hash algorithm;
record source path and source commit/snapshot;
use source_hash to detect stale chunks;
use chunk_content_hash later if sub-chunk-level stale detection is needed;
missing source_hash fails closed;
mismatched source_hash rejects candidate from context-pack inclusion.
```

The contract should distinguish:

```text
source_hash: canonical note/file-level content hash;
chunk_content_hash: optional chunk-level content hash for diagnostics and rebuild comparison;
index_generation_hash or manifest hash: aggregate proof of an index generation.
```

## Embedding version / embedding space contract

024C should not choose the final embedding model.

It should define required identity fields:

```text
embedding_model_id;
embedding_model_version;
embedding_dimension;
embedding_distance_metric;
embedding_normalization_policy;
embedding_text_policy;
embedding_space_id.
```

The embedding_space_id should identify the complete embedding contract, not only model name.

Recommended rule:

```text
changing model, model version, dimensions, distance metric, normalization policy, or embedding text policy creates a new embedding_space_id.
```

Embedding-model selection belongs in a later packet unless 024C execution chooses only the required field contract.

## Index generation contract

024C should define index generations before implementation.

Recommended first model:

```text
collection-per-generation plus active alias/cutover is the preferred first implementation model;
index_generation_id identifies one complete built generation;
active alias points to the current generation;
old generation remains only for bounded rollback/audit window;
real corpus generation requires manifest review before alias cutover;
synthetic generation may be used for hammer tests once implementation is authorized.
```

024C should not create collections or aliases.

It should define the future proof requirements:

```text
record corpus manifest input;
record included/excluded counts;
record index_generation_id;
record payload_schema_version;
record chunker_version;
record embedding_space_id;
record source commits/snapshots;
record deterministic point count;
record aggregate hash/manifest hash;
prove active generation can be rebuilt from canonical source inputs.
```

## Stale / deleted / superseded handling

024C should define fail-closed stale handling:

```text
candidate with inactive index_generation_id is rejected by gateway/assembler;
candidate with source_hash mismatch is rejected;
candidate with supersedence_state not current is rejected from default retrieval;
candidate with missing or ambiguous metadata is rejected;
old-generation points must not be reachable through active default retrieval after alias cutover;
deleted source chunks are removed by full generation rebuild or marked unreachable by generation cutoff;
silent in-place mutation of accepted generation is disallowed except through a later accepted repair rule.
```

Deletion semantics should remain conservative:

```text
Qdrant deletion is derivative cleanup, not canonical deletion;
canonical source deletion/disposition remains governed elsewhere;
index cleanup never proves canonical deletion.
```

## Rebuild proof requirements

024C should define the minimum rebuild proof later packets must satisfy:

```text
same corpus manifest + same chunker_version + same payload_schema_version + same embedding_space_id produces same chunk IDs and point IDs;
all included records trace back to source_path and matching source_hash;
all excluded records have an exclusion reason;
private/raw/source/evidence and current-state/command-center defaults are absent from default generation;
superseded/draft/proposed/rejected/archived material is absent from default generation;
rebuild detects stale source_hash differences;
old generation is not returned through active alias after cutover;
manifest hash and point count are recorded;
no generated Qdrant state becomes canonical truth.
```

## Context-pack assembly implication

024C should preserve that Qdrant returns candidates only.

Candidate results must carry enough payload to allow later gateway/context-pack validation:

```text
project_id or project_key;
corpus_boundary;
source_path;
source_hash;
note_id;
note_type;
chunk_id;
heading_anchor;
authority_level;
status;
review_state;
supersedence_state;
privacy_class;
index_generation_id;
embedding_space_id;
traceability_status;
score or ranking metadata.
```

Context-pack assembly should re-resolve source references against canonical Markdown before inclusion.

## Expected 024C deliverable

024C execution should create a docs-only result:

```text
03-research-results/406-packet-024c-chunk-payload-id-and-rebuild-contract-result.md
```

The result should include:

```text
first chunking strategy;
chunk boundary requirements;
required/optional/forbidden payload fields;
future filter/index field candidates;
deterministic chunk ID contract;
deterministic Qdrant point ID contract;
source hash contract;
embedding space identity contract;
index generation and alias/cutover posture;
stale/deleted/superseded handling;
rebuild proof requirements;
open decisions carried to 024D and later implementation packets;
recommendation for next packet.
```

## Acceptance criteria for 024C execution

024C execution is complete only if:

```text
no implementation is performed;
no Qdrant instance is installed or contacted;
no embedding model is installed;
no platform files are changed;
no vault files are changed;
no staging files are changed;
no database state is changed;
no synthetic corpus files are created;
no real corpus is indexed;
no corpus manifest is generated;
chunk, payload, ID, hash, generation, stale, and rebuild contracts are explicit;
open decisions are carried forward;
read path is refreshed;
docs commit is pushed.
```

## Stop conditions

Stop immediately if 024C requires:

```text
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
collection or alias creation;
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

This draft packet does not authorize 024C execution until explicitly owner accepted.

Even if accepted, it does not authorize:

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
collection or alias creation;
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
Accept Packet 024C for docs-only chunk, payload, ID, and rebuild contract, with exact docs-only scope and non-authorization boundaries as listed.
```

After accepted 024C execution, the likely next packet should be:

```text
024D — Retrieval Gateway and Typed Tool Contract
```
