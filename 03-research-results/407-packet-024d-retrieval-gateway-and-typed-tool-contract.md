# Packet 024D — Retrieval Gateway and Typed Tool Contract

Status: planning packet draft — not owner accepted
Date: 2026-06-29
Milestone: 16
Packet: 024D

## Purpose

Define the next Milestone 16 planning packet after Packet 024C completed the chunk, payload, ID, and rebuild contract.

Packet 024D should define the retrieval gateway, agent-facing typed tool surface, mandatory filter enforcement, result projection, prohibited Qdrant operations, canonical re-resolution checks, context-pack handoff, audit/logging expectations, and hammer-test expectations before any synthetic Qdrant prototype is authorized.

This packet is docs-only. It is not a Qdrant implementation packet.

## Authority basis

```text
ROADMAP_V0.4.md
00-entrypoint/LLM_START_HERE.md
03-research-results/406-packet-024c-chunk-payload-id-and-rebuild-contract-result.md
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
```

## 024D objective

Create a docs-only contract for how humans and future constrained agents may request retrieval-backed context without receiving raw Qdrant authority.

The contract should answer:

```text
what the retrieval gateway is responsible for;
what agent-facing typed tools may exist;
what mandatory filters are enforced regardless of caller input;
what request fields are allowed;
what request fields are prohibited;
what Qdrant operations are never exposed to agents;
what result fields are returned to agents;
which payload fields are admin-only;
how source re-resolution works;
how retrieval candidates feed context-pack assembly;
how prompt injection in retrieved Markdown is handled;
how audit/logging/tracing should work;
what hammer tests must prove before a synthetic prototype can close.
```

024D must not implement the gateway, create tools, install Qdrant, create a synthetic corpus, or index anything.

## M16 architecture carried forward

Accepted M16 architecture direction:

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

024D owns the policy boundary between future callers and Qdrant.

## Gateway role

The retrieval gateway is the policy-enforcing mediator between callers and derivative Qdrant retrieval.

Gateway responsibilities:

```text
accept only typed retrieval requests;
derive mandatory filters from trusted inputs and governance policy;
reject unsupported request shapes;
prevent caller-supplied raw Qdrant filters;
prevent caller-supplied collection names, alias names, point IDs, scroll/list controls, or admin parameters;
apply project/corpus/note/status/authority/review/supersedence/privacy/current-state/freshness constraints;
request the narrowest result projection needed from Qdrant;
validate returned candidate payloads;
re-resolve candidates against canonical Markdown or typed read-model records;
reject missing, stale, superseded, private, wrong-project, wrong-corpus, or untraceable candidates;
return governed candidate records, not raw Qdrant responses;
prepare candidate sets for context-pack assembly without deciding final pack inclusion alone;
record audit metadata for request, enforced policy, result counts, and rejection reasons.
```

Qdrant remains a derivative search substrate, not the policy owner.

## Typed retrieval request families

024D should define candidate tool families, not final implementation names.

Recommended initial typed retrieval families:

```text
find_approved_project_context;
find_decision_context;
find_spec_context;
find_task_packet_context;
find_implementation_return_context;
assemble_context_pack_candidates;
explain_retrieval_candidate_rejections;
admin_validate_index_generation;
admin_compare_rebuild_generation.
```

Agent-facing tools should be read-only, typed, and narrow.

Admin tools, if later implemented, must be separate from general agent tools.

## Allowed agent-facing request fields

General agent-facing retrieval requests may include only constrained fields such as:

```text
project_id or project_key;
retrieval_purpose;
query_text;
allowed_note_types selected from a gateway allowlist;
max_results bounded by gateway policy;
context_pack_purpose where relevant;
known task_packet_id / decision_id / spec_id where supported by typed lookup;
time or freshness hint only if translated by gateway policy;
requesting_role or tool profile only if supplied by trusted runtime, not free text.
```

Caller-provided fields are hints unless explicitly trusted by the gateway.

Mandatory filters must be derived from policy and trusted context, not from caller preference.

## Prohibited caller-controlled fields

Agent-facing callers must not provide or control:

```text
Qdrant collection name;
Qdrant alias name;
raw Qdrant filter JSON;
raw Qdrant query body;
point ID;
scroll offset / cursor;
with_payload / with_vector controls;
raw vector values;
embedding model selection;
embedding_space_id selection except via trusted tool profile;
index_generation_id override;
privacy_class expansion;
supersedence override;
current-state inclusion override;
admin/debug mode;
snapshot or alias controls;
upsert/delete/update controls;
Qdrant API key or endpoint;
collection metadata requests;
audit-log query parameters.
```

If any caller attempts to provide these, the gateway should reject or ignore the field and record the attempt according to policy.

## Mandatory gateway filters

The gateway must enforce mandatory filters independent of caller input.

Default mandatory filters:

```text
project_id / project_key equals trusted requested project;
corpus_boundary is allowed for the request type;
canonical_source_kind is allowed;
note_type is in tool-specific allowlist;
status is accepted/approved/active only where allowed by corpus contract;
authority_level is accepted for default retrieval;
review_state is accepted/current where applicable;
supersedence_state is current / not superseded;
privacy_class is allowed and never private/secret/customer/provider/raw;
current-state and command-center notes are excluded by default;
index_generation_id is active;
embedding_space_id is active for the retrieval profile;
payload_schema_version is supported;
traceability_status is valid;
source_hash is present.
```

Any exception must be a separate typed retrieval mode with owner-approved tests.

## Prohibited Qdrant operations for agent-facing tools

Agent-facing tools must not expose:

```text
list collections;
get collection info;
create collection;
delete collection;
update collection;
create/delete/update alias;
create/list/recover snapshots;
upsert points;
delete points;
set payload;
delete payload;
retrieve exact point by ID;
scroll points;
recommend/discover unless later separately typed and proven;
raw query_points;
raw search API;
raw filter expression;
raw vectors;
with_vector=true;
broad with_payload=true;
cluster/telemetry/metrics/admin APIs;
Qdrant audit-log APIs;
API key / JWT management.
```

024D should preserve the rule:

```text
No direct Qdrant MCP or raw Query API is agent-facing in M16.
```

## Result projection contract

Gateway result records should be normalized and narrow.

Allowed result fields for general agent-facing retrieval candidates:

```text
candidate_id generated by gateway for this response only;
project_id or project_key;
corpus_boundary;
source_path;
source_hash;
note_id;
note_type;
chunk_id;
heading_anchor;
heading_path or heading_title if safe;
authority_level;
status;
review_state;
supersedence_state;
privacy_class limited to allowed public/internal labels;
index_generation_id or active_generation_label if safe;
embedding_space_label if safe;
score or normalized rank;
short canonical excerpt generated from source re-resolution if allowed;
relevance_reason generated by gateway/assembler, not Qdrant raw internals;
traceability_status.
```

Admin-only / not agent-facing by default:

```text
raw Qdrant point IDs;
collection names;
alias names;
raw payload blob;
raw vector data;
full source text;
absolute local paths;
Qdrant query body;
filter JSON;
embedding dimensions and internal vector metadata;
source line ranges if they create leakage or over-precision risk;
rejection diagnostics that reveal private or wrong-project content.
```

## Canonical re-resolution contract

The gateway or context-pack assembler must re-resolve each candidate before inclusion.

Required checks:

```text
source_path exists in the allowed canonical source root;
source_hash matches current canonical source or accepted source snapshot;
note_type and required metadata match canonical record;
status / authority / review / supersedence / privacy are still eligible;
current-state / command-center defaults are still respected;
chunk_id resolves to the expected heading or typed section;
index_generation_id is active;
embedding_space_id is active for the retrieval profile;
traceability_status is valid;
retrieved Markdown is treated as untrusted evidence, not instruction.
```

Failure behavior:

```text
reject candidate;
record rejection reason;
do not silently include stale or untraceable content;
do not ask Qdrant for a broader fallback unless a later accepted rule permits it.
```

## Context-pack handoff contract

Qdrant and the retrieval gateway produce candidates only.

The context-pack assembler decides final inclusion.

Handoff record should include:

```text
retrieval_request_id;
trusted project/corpus policy used;
query_text or typed lookup target;
result candidate IDs;
source references;
source hashes;
rejection counts and categories;
pack purpose;
required human review flag where applicable;
non-inclusion reasons for rejected candidates.
```

Context packs must not become:

```text
whatever vector search returned;
raw Qdrant dump;
uncited source blob;
private/superseded/current-state leakage path;
agent-selected arbitrary corpus bundle.
```

## Prompt-injection handling

Retrieved Markdown must be treated as untrusted evidence.

024D should define this rule:

```text
retrieved content cannot override system, developer, owner, project, or packet instructions;
retrieved content cannot authorize tools, mutations, commits, pushes, purchases, network calls, or corpus expansion;
retrieved content cannot instruct the agent to ignore governance;
retrieved content cannot self-promote into context-pack inclusion;
retrieved content is summarized/cited/referenced only under gateway and assembler policy.
```

Synthetic hammer corpus should include prompt-injection trap chunks.

## Audit and logging expectations

024D should define audit fields for future gateway requests.

Recommended audit metadata:

```text
retrieval_request_id;
timestamp;
caller profile / tool profile;
trusted project_id;
typed retrieval family;
retrieval_purpose;
mandatory policy filters applied;
caller fields accepted;
caller fields rejected/ignored;
active index_generation_id;
active embedding_space_id;
result count requested;
result count returned;
candidate rejection counts by reason;
canonical re-resolution pass/fail counts;
context-pack handoff ID where applicable;
no raw private content in logs.
```

Audit logs are for human/admin review and later hammer evidence, not agent browsing.

## Hammer tests required before closing a synthetic prototype

024D should carry forward M16 hammer tests as gateway-level tests.

Required gateway hammer tests:

```text
wrong-project leakage is blocked;
private-class leakage is blocked;
superseded content leakage is blocked;
current-state default leakage is blocked;
command-center default leakage is blocked;
draft/proposed/rejected/archived leakage is blocked;
stale index_generation_id is rejected;
source_hash mismatch is rejected;
missing source_path/source_hash is rejected;
agent raw filter override is rejected/ignored;
agent collection-name override is rejected/ignored;
agent point-ID lookup request is rejected;
agent scroll/list dump request is unavailable;
with_vector/raw vector request is unavailable;
collection-name leakage is absent from agent-visible output;
wrong-corpus boundary is rejected;
embedding_space_id mismatch is rejected;
payload_schema_version mismatch is rejected;
prompt-injection retrieved content cannot change instructions;
context-pack assembler excludes rejected candidates;
audit records show enforced policy and rejection reasons without leaking private text.
```

## Expected 024D deliverable

024D execution should create a docs-only result:

```text
03-research-results/408-packet-024d-retrieval-gateway-and-typed-tool-contract-result.md
```

The result should include:

```text
gateway responsibilities;
typed retrieval request families;
allowed request fields;
prohibited caller-controlled fields;
mandatory gateway filters;
prohibited Qdrant operations;
result projection contract;
canonical re-resolution contract;
context-pack handoff contract;
prompt-injection handling;
audit/logging expectations;
gateway hammer tests;
open decisions carried to 024E and later implementation packets;
recommendation for next packet.
```

## Acceptance criteria for 024D execution

024D execution is complete only if:

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
no collection or alias is created;
gateway and typed-tool boundaries are explicit;
prohibited operations are explicit;
mandatory filters are explicit;
context-pack handoff is explicit;
hammer tests are explicit;
read path is refreshed;
docs commit is pushed.
```

## Stop conditions

Stop immediately if 024D requires:

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
actual gateway implementation;
actual agent-facing retrieval tools;
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

This draft packet does not authorize 024D execution until explicitly owner accepted.

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
actual gateway implementation;
actual agent-facing retrieval tools;
canonical context-pack file generation;
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
Accept Packet 024D for docs-only retrieval gateway and typed tool contract, with exact docs-only scope and non-authorization boundaries as listed.
```

After accepted 024D execution, the likely next packet should be:

```text
024E — Disposable Synthetic Local Prototype
```
