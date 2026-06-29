# Packet 024C — Chunk, Payload, ID, and Rebuild Contract Result

Status: complete — docs-only chunk, payload, ID, and rebuild contract accepted for execution
Date: 2026-06-29
Milestone: 16
Packet: 024C
Execution repository: `C:\dev\kaizen-docs`

## Purpose

Record execution of Packet 024C after explicit owner acceptance.

Packet 024C defines the chunk, payload, deterministic identity, source-hash, index-generation, alias/cutover, stale-point, and rebuild-proof contracts before any Qdrant prototype, embedding installation, platform mutation, synthetic corpus file creation, or corpus indexing occurs.

This result is docs-only. It does not implement Qdrant, install Qdrant, contact Qdrant, install embeddings, mutate platform, mutate vault, mutate staging, mutate database state, create synthetic corpus files, generate a corpus manifest, index a real corpus, create collections/aliases, or expose agent-facing retrieval tools.

## Owner acceptance

The owner accepted Packet 024C with this instruction:

```text
Accept Packet 024C for docs-only chunk, payload, ID, and rebuild contract, with exact docs-only scope and non-authorization boundaries as listed.
```

The accepted packet draft was:

```text
03-research-results/405-packet-024c-chunk-payload-id-and-rebuild-contract.md
```

## Starting posture

Pre-execution posture verified:

```text
docs branch: main
docs starting commit: e8eec4004a21eca93089ce4a410944151a11a825
docs working tree: clean and synced
platform branch: main
platform commit: 84071eef1e5859f651d3213c65f3bfc99a3c94f8
platform working tree: clean
vault branch: main
vault commit: 6fbc8cefe4ed4cd57403648ee3977ee2aeaaba5b
vault working tree: clean
```

## Authority basis

024C executes against:

```text
03-research-results/405-packet-024c-chunk-payload-id-and-rebuild-contract.md
03-research-results/404-packet-024b-corpus-boundary-and-governance-contract-result.md
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

## Contract scope accepted

024C accepts the contract layer for:

```text
deterministic chunk/index records;
source traceability;
payload requirements;
logical chunk identity;
derivative Qdrant point identity;
embedding-space identity;
index generation identity;
alias/cutover posture;
stale/deleted/superseded handling;
rebuild proof requirements.
```

024C does not select final embedding model, implement chunking, generate manifests, or create Qdrant collections.

## Chunking strategy

The first M16 chunking strategy is:

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

024C does not choose final token/character limits.

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

## Payload schema contract

024C classifies payload fields into required, optional-later, and forbidden/dangerous.

Required M16 payload fields:

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

Optional-later payload fields:

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

## Future filter/index field candidates

Likely future Qdrant payload-index candidates:

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

Chunk IDs are deterministic logical identifiers, not database-generated IDs.

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

Required behavior:

```text
same canonical input produces same chunk_id;
content edits that do not move the heading/section keep the logical chunk identity but change source_hash / chunk_content_hash;
heading/section moves may produce a new logical chunk ID unless a later anchor-preservation rule exists;
chunker_version changes participate in identity to avoid silent collision across chunking algorithms;
no random UUIDs for logical chunk identity.
```

The exact hash algorithm and encoding remain finalizable in later implementation planning, but the deterministic contract is accepted.

## Deterministic Qdrant point ID contract

Qdrant point IDs should be deterministic from stable chunk identity and embedding/index version.

Point-ID contract:

```text
point_id = deterministic_uuid_or_hash(
  corpus_boundary,
  chunk_id,
  embedding_space_id,
  payload_schema_version
)
```

Required behavior:

```text
same chunk in same embedding space produces same point ID;
new embedding space produces distinct point IDs or distinct collection generation according to the accepted generation model;
point IDs are not derived from raw private text;
point IDs are not random for indexable logical content;
point IDs are not exposed to general agents;
point IDs are for indexer/admin/rebuild proof, not agent-facing retrieval control.
```

## Source hash contract

Source hashes are canonical-source integrity evidence.

Requirements:

```text
hash normalized canonical Markdown bytes or a precisely specified normalized content representation;
record hash algorithm;
record source path and source commit/snapshot;
use source_hash to detect stale chunks;
use chunk_content_hash later if sub-chunk-level stale detection is needed;
missing source_hash fails closed;
mismatched source_hash rejects candidate from context-pack inclusion.
```

Hash meanings:

```text
source_hash: canonical note/file-level content hash;
chunk_content_hash: optional chunk-level content hash for diagnostics and rebuild comparison;
index_generation_hash or manifest hash: aggregate proof of an index generation.
```

## Embedding-space identity contract

024C does not choose final embedding model.

Required identity fields:

```text
embedding_model_id;
embedding_model_version;
embedding_dimension;
embedding_distance_metric;
embedding_normalization_policy;
embedding_text_policy;
embedding_space_id.
```

Embedding-space rule:

```text
changing model, model version, dimensions, distance metric, normalization policy, or embedding text policy creates a new embedding_space_id.
```

The embedding_space_id identifies the complete embedding contract, not only the model name.

## Index generation contract

Recommended first index-generation model:

```text
collection-per-generation plus active alias/cutover is the preferred first implementation model;
index_generation_id identifies one complete built generation;
active alias points to the current generation;
old generation remains only for bounded rollback/audit window;
real corpus generation requires manifest review before alias cutover;
synthetic generation may be used for hammer tests once implementation is authorized.
```

024C does not create collections or aliases.

Future proof requirements:

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

Fail-closed stale handling:

```text
candidate with inactive index_generation_id is rejected by gateway/assembler;
candidate with source_hash mismatch is rejected;
candidate with supersedence_state not current is rejected from default retrieval;
candidate with missing or ambiguous metadata is rejected;
old-generation points must not be reachable through active default retrieval after alias cutover;
deleted source chunks are removed by full generation rebuild or marked unreachable by generation cutoff;
silent in-place mutation of accepted generation is disallowed except through a later accepted repair rule.
```

Deletion semantics:

```text
Qdrant deletion is derivative cleanup, not canonical deletion;
canonical source deletion/disposition remains governed elsewhere;
index cleanup never proves canonical deletion.
```

## Rebuild proof requirements

Minimum rebuild proof later packets must satisfy:

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

Qdrant returns candidates only.

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

## Open decisions carried to 024D and later

024C carries these decisions forward:

```text
final hash algorithm / encoding for chunk_id and point_id;
final chunk-size threshold;
final embedding model baseline and fallback;
exact collection naming and alias naming convention;
exact rollback retention window for old generations;
exact snapshot proof scope;
exact gateway typed-tool names and request/response shape;
which payload fields are visible to agents versus admin-only;
whether line ranges are required in first prototype;
whether chunk_content_hash is mandatory in first implementation;
first real corpus include/deny decision for implementation-return records;
final docs-only / vault-only / paired docs+vault real pilot decision.
```

## Non-authorization preserved

024C execution did not authorize:

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

## Changed docs paths for 024C execution

This execution creates this result record and refreshes the active read path:

```text
03-research-results/406-packet-024c-chunk-payload-id-and-rebuild-contract-result.md
00-entrypoint/LLM_START_HERE.md
ROADMAP_V0.4.md
```

## Recommended next action

Proceed to draft the next docs-only planning packet:

```text
024D — Retrieval Gateway and Typed Tool Contract
```

Recommended 024D goal:

```text
define the retrieval gateway, agent-facing typed tool surface, mandatory filter enforcement, result projection, prohibited Qdrant operations, canonical re-resolution checks, context-pack handoff, audit/logging expectations, and hammer-test expectations before any synthetic Qdrant prototype is authorized.
```

Recommended owner acceptance line after review of this result:

```text
Accept Packet 024C completion and authorize drafting Packet 024D retrieval gateway and typed tool contract as docs-only planning, with no Qdrant implementation or corpus indexing.
```
