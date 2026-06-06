# Research Result 011 - Qdrant MCP and Typed-Tool Comparative Audit

Status: active evidence summary
Date: 2026-06-05
Source: Claude Research Step 5A Qdrant report supplied by the project owner

## Purpose

Preserve and reconcile Claude's comparative audit of Qdrant MCP servers, the official Qdrant API and SDKs, granular JWT/RBAC controls, retrieval gateways, indexing services, and rebuildability patterns.

This file is evidence and steward reconciliation. It does not select a final Qdrant integration, approve a collection layout, choose an embedding model, authorize agent mutation, or promote a prototype candidate into doctrine.

## Executive finding

Claude found no surveyed Qdrant MCP server suitable as-is for Kaizen's governed agent lanes.

The official Qdrant MCP server exposes a semantic-memory model built around agent-facing `find` and `store` operations. Its `store` tool permits agent-authored content to enter the vector index without a canonical Markdown source, which conflicts with Kaizen's accepted rule that Qdrant is derivative and owns no unique truth.

The strongest current candidate architecture is a hybrid:

```text
Kaizen typed retrieval tool
-> Kaizen retrieval gateway
-> short-lived project-scoped read token
-> mandatory Qdrant payload filters
-> Qdrant retrieval engine

Canonical Markdown
-> Kaizen-owned headless indexer
-> deterministic provenance and drift controls
-> scoped write token
-> Qdrant

Human/CI/admin tooling
-> collection, snapshot, index, cluster, and key administration
```

This direction is adopted as the leading architecture candidate, but it remains subject to disposable prototype and hammer evidence.

## Adopt

### Qdrant remains derivative

Adopt the report's central conclusion that all indexed content must be derivable from canonical Markdown or another explicitly governed canonical source.

Qdrant must not become the only home of:

- agent-authored memories;
- claims;
- decisions;
- project facts;
- authority state;
- provenance;
- private business knowledge.

A generic `store memory` tool is therefore incompatible with Kaizen's agent lanes.

### No direct Qdrant credentials or mutation tools for agents

Adopt the prohibition on giving Hermes or general reasoning agents:

- Qdrant API keys;
- generic store or upsert tools;
- payload mutation;
- delete tools;
- collection creation or deletion;
- payload-index administration;
- snapshots or restore;
- cluster, shard, alias, optimizer, or replication controls.

Agent-facing access is retrieval-only through approved typed tools.

### Capability lanes

Adopt four Qdrant capability lanes.

#### Lane 1 - Governed semantic retrieval

Potential agent-facing capabilities:

- project-scoped semantic search;
- approved dense, sparse, or hybrid retrieval;
- bounded recommendation or discovery operations;
- retrieval of approved source IDs, headings, scores, and governed metadata;
- mandatory privacy, authority, supersedence, freshness, and note-type filtering.

The agent cannot choose arbitrary collections, raw payload filters, scroll operations, or exact point IDs.

#### Lane 2 - Controlled indexing and synchronization

Reserved for a Kaizen-owned headless service:

- chunk generation;
- embedding generation;
- deterministic point IDs;
- content and source hashes;
- payload schema and embedding version capture;
- idempotent upsert;
- stale-point reconciliation;
- orphan removal;
- full rebuild;
- drift detection.

#### Lane 3 - Collection and infrastructure administration

Human/CI/admin only:

- collection create/delete/alias;
- payload-index administration;
- snapshots and restore;
- optimizer and cluster configuration;
- sharding and replication;
- RBAC/key administration;
- destructive cleanup and disaster recovery.

#### Lane 4 - Developer and diagnostic access

Disposable or tightly controlled environments only:

- collection inspection;
- point sampling;
- recall and precision evaluation;
- rebuild comparison;
- payload and filter diagnostics;
- synthetic-data troubleshooting.

### Ecosystem reuse

Adopt the principle that Kaizen should reuse mature Qdrant capabilities instead of rebuilding vector infrastructure.

Candidate reusable capabilities include:

- dense, sparse, and hybrid search;
- payload filtering and indexes;
- recommendation and discovery APIs;
- SDKs;
- quantization;
- snapshots;
- aliases;
- sharding and replication;
- managed hosting;
- monitoring and backup features;
- granular access controls where proven.

Kaizen should own the authority-bearing indexer, retrieval contract, scope injection, provenance, and audit behavior.

### Retrieval reads require confinement

Adopt the finding that read-only vector access can still leak:

- cross-project content;
- private payloads;
- superseded or stale chunks;
- collection inventory;
- exact known points;
- entire collections through scroll/list operations.

Search-only labels are insufficient without mandatory database-enforced scope and tool-surface reduction.

## Modify

### JWT payload filters are a leading candidate, not presumed complete enforcement

Modify any conclusion that Qdrant JWT payload filters are already proven to be the exact equivalent of Postgres RLS for Kaizen.

They are a strong candidate because they may enforce collection scope, read-only access, expiry, and mandatory payload filtering independently of agent-supplied query parameters.

However, a disposable prototype must verify that the enforced scope applies consistently to every exposed path, including:

- similarity search;
- hybrid search;
- recommendation and discovery;
- scroll/list points;
- exact-point retrieval;
- batch retrieval;
- count, facet, and metadata operations where relevant;
- aliases and collection routing;
- error responses that might disclose existence.

A filter that protects search but not exact-point or scroll access is not an acceptable boundary.

### The official `find` tool is a candidate implementation detail, not a preferred architecture

Modify the report's suggestion to fork or wrap the official server's `find` tool as the default lean.

Kaizen should compare:

1. wrapping/forking the official `find` implementation with `store` removed;
2. building a very small typed retrieval tool over the official SDK;
3. using a gateway that mints short-lived scoped tokens and injects mandatory filters.

The decision should favor the smallest auditable surface, not reuse for its own sake.

If removing `store` requires inheritance tricks, hidden generic methods remain, or the server's semantic-memory assumptions fight Kaizen's provenance model, a small Kaizen retrieval tool may be safer and simpler than a fork.

### Deterministic IDs require a stable chunk identity model

Modify the report's shorthand that point IDs should derive from `source path + heading/block anchor + content hash`.

Including content hash directly in the point ID may create a new point for every edit and complicate stable supersedence and lineage.

The prototype should compare:

- stable logical chunk IDs with content hash stored in payload;
- content-addressed IDs;
- versioned chunk IDs with explicit supersedence;
- deterministic IDs based on stable note ID, anchor, chunk ordinal, and schema version.

The governing requirement is reproducibility, provenance, and drift detection—not one preselected formula.

### Freshness must not be reduced to one boolean

Modify the example mandatory filter `fresh = true`.

Freshness may be:

- source-class dependent;
- timestamp based;
- recheck-state based;
- valid-until based;
- unknown but still retrievable with warning;
- superseded independently of freshness.

The retrieval contract should use governed freshness fields and policies rather than assuming one universal boolean.

### Private and authority tags must be indexer-owned

The indexer, not an agent or caller, must derive and validate:

- project scope;
- privacy classification;
- authority state;
- supersedence state;
- source identity;
- note type;
- freshness metadata.

If these fields are caller-controlled, mandatory filters can be defeated by mis-tagging during ingestion.

## Reject

Reject:

- the official MCP `store` tool for Kaizen agent use;
- agent-authored semantic memory with no canonical source;
- direct unrestricted Qdrant MCP access;
- one cluster-wide full-access API key for agents;
- generic scroll/list or exact-point tools in Hermes profiles;
- caller-selected collections and raw payload filters;
- generic upsert/delete/admin tools for agents;
- treating snapshots as canonical truth;
- treating a read-only credential as sufficient without project/privacy/authority filters;
- selecting exact collections, chunk sizes, embedding models, or payload schema before prototype and workload evidence.

## Defer

Defer until prototype and workload evidence exists:

- final retrieval server or gateway;
- official MCP fork versus small Kaizen SDK tool;
- managed versus self-hosted Qdrant;
- exact collection strategy;
- exact payload schema;
- exact chunking method;
- exact embedding model and versioning policy;
- exact sparse-vector and reranking strategy;
- exact point-ID algorithm;
- exact freshness representation;
- exact snapshot and recovery design;
- exact token-minting and rotation implementation.

## Candidate disposition

### Official Qdrant MCP server

Disposition: `reject store; evaluate find only through source audit and disposable prototype`.

Its semantic-memory model conflicts with Kaizen's canonical-source and rebuildability rules. The `find` path may contain reusable ideas, but the server is not accepted as-is.

### Qdrant REST, gRPC, and official SDKs

Disposition: `preferred infrastructure foundation for prototype evaluation`.

These provide mature retrieval and administration capabilities, but Kaizen must expose only narrow typed operations and hold credentials outside the agent.

### Qdrant granular JWT/RBAC

Disposition: `leading Lane 1 enforcement candidate`.

It requires prototype proof across all retrieval paths and careful token minting, expiry, revocation, rotation, collection scope, and mandatory payload filters.

### Qdrant Cloud

Disposition: `managed-hosting candidate`.

Evaluate operational maturity, backups, auditability, regional/privacy requirements, cost, and whether the required granular controls are available and testable.

### Generic vector MCP servers

Disposition: `default reject unless demonstrably reducible`.

Source-audit tool inventory, collection choice, filter enforcement, credential scope, and mutation/admin absence before further consideration.

### Retrieval frameworks

Disposition: `library and orchestration evaluation only`.

Useful for composing retrieval, reranking, or evaluation pipelines, but they must not own canonical truth or bypass Kaizen's gateway, scope, and provenance controls.

## Configure-versus-wrap matrix

| Candidate pattern | Safe reduction mechanism | Required proof | Likely Kaizen use |
|---|---|---|---|
| Official MCP `find` plus `store` | Fork or wrap | `store` absent from discovery and unreachable; mandatory filters cannot be overridden | Lane 1 candidate only if simpler than a small typed tool |
| Official API with full API key | Reject for agents | None; key must stay in admin/indexer boundary | Lane 2/3 service use only |
| Read-only API key without payload scope | Wrap and upgrade | Mandatory project/privacy/authority filters and collection scope added independently | Insufficient alone |
| Granular JWT/RBAC | Configure plus thin gateway | Every retrieval path obeys collection and payload scope; expiry/revocation/rotation proven | Leading Lane 1 candidate |
| Kaizen-owned indexer over SDK | Kaizen-owned contract | Deterministic provenance, idempotency, drift detection, rebuildability, scoped credential | Lane 2 |
| Generic vector MCP | Wrap, fork, or reject | Unsafe tools absent; caller cannot choose collection/filter; no generic pass-through | Usually reject |
| Admin SDK/console | Separate identity and environment | Human/CI gate, audit, recovery plan | Lane 3/4 |

## Authentication and token constraints

Prototype and implementation planning must account for:

- separation of admin, indexer, retrieval, and developer credentials;
- short-lived project-scoped retrieval tokens;
- collection scope;
- mandatory payload filters;
- token expiry and revocation;
- signing-key rotation;
- alternate-key rollout where supported;
- credential isolation from agent tool arguments and logs;
- request, actor, project, and session identity propagation;
- no cross-project token reuse inside one agent context;
- failure closed when token minting or scope resolution is ambiguous.

The retrieval gateway may hold token-minting authority, but it must expose no generic token-mint or raw Qdrant pass-through tool to agents.

## Rebuildability requirements

The Lane 2 indexer must establish:

- stable canonical source identity;
- stable logical chunk identity;
- source path and stable note ID;
- heading, block, or anchor identity;
- content hash;
- embedding model and version;
- payload schema version;
- project;
- note type;
- authority state;
- privacy classification;
- supersedence metadata;
- freshness metadata;
- index run ID;
- captured/indexed timestamps;
- deterministic reconciliation behavior.

Required operations:

- idempotent incremental index;
- changed-content update;
- orphan detection;
- stale-point removal;
- targeted re-embedding;
- model/schema migration detection;
- full wipe and rebuild;
- inventory comparison;
- source-to-index drift report.

A snapshot may accelerate recovery but never replaces canonical Markdown as the authority.

## Prototype requirements

Use a disposable Qdrant instance and synthetic canonical Markdown representing multiple projects, privacy classes, authority states, supersedence states, and freshness conditions.

### Retrieval-confinement tests

- Project A cannot retrieve Project B through similarity search.
- Private points remain hidden.
- Unapproved and superseded points remain hidden.
- Stale or freshness-unknown behavior matches the governed policy.
- Exact-point retrieval cannot bypass scope.
- Scroll/list cannot dump out-of-scope data.
- Recommendation, discovery, count, facet, and batch operations cannot bypass scope.
- Caller-supplied filters cannot weaken mandatory filters.
- Collection names and administrative metadata remain hidden where required.
- Error responses do not reveal protected point existence.
- Expired and revoked tokens fail closed.
- Tokens are not reused across project contexts.

### Tool-surface tests

- Agent profile advertises no store, upsert, payload update, delete, scroll, exact-point, collection, snapshot, alias, or admin tools unless a specific bounded read operation is explicitly approved.
- Hidden generic request or pass-through tools do not exist.
- Tool discovery differs correctly between agent, indexer, admin, and developer profiles.

### Indexer and rebuild tests

- Indexing the same canonical input twice is idempotent.
- One changed source updates only governed affected chunks.
- Deleted sources produce detected and removed orphans.
- Full wipe and rebuild reproduces the expected logical inventory.
- Embedding-model and payload-schema changes trigger visible drift.
- Mis-tagged project, privacy, authority, or supersedence metadata is rejected.
- Agent-supplied metadata cannot override indexer-owned fields.
- Snapshot restore and canonical rebuild converge to a governed equivalent state.

### Security and operations tests

- Self-hosted instances require authentication, TLS, and network restriction before use.
- Full-access credentials never appear in agent logs or payloads.
- Audit events capture actor, project, tool, mandatory filter class, result count, duration, and index/rebuild run IDs without logging private content bodies.
- Key rotation and token invalidation are tested.

## Hammer-test additions

Future Qdrant hammer coverage should include:

- cross-project semantic-match attempts;
- private-point retrieval attempts;
- superseded and unapproved content retrieval;
- exact-point bypass;
- scroll/list bulk exfiltration;
- recommendation/discovery bypass;
- filter weakening and removal attempts;
- store/upsert/delete/admin tool-discovery absence;
- full-access credential exposure attempts;
- deterministic rebuild and drift comparison;
- indexer metadata spoofing;
- stale-point and orphan reconciliation;
- token expiry, revocation, and rotation;
- self-hosted insecure-default detection;
- audit completeness.

## Cross-system consequence

Qdrant retrieval results must point back to canonical Markdown through stable source identifiers.

A future retrieval response should likely include:

- canonical note ID;
- source path;
- heading or anchor;
- content hash or version reference;
- project;
- note type;
- authority and freshness indicators;
- score and retrieval method;
- index schema/model version;
- enough provenance to re-open and verify the canonical source.

Qdrant must not return an unsupported text blob that cannot be traced to canonical content.

## Likely document updates

After human review and prototype evidence, findings may affect:

- `04-design-decisions/0004-system-of-record-boundaries.md`
- `04-design-decisions/0005-api-only-structured-data-access.md`
- a future Qdrant authority and retrieval specification
- `05-specs/kaizen-hammer-test-strategy.md`
- `07-hermes/hermes-permission-matrix.md`
- `ROADMAP.md`

Do not place Qdrant-specific authority rules inside `05-specs/operational-postgres-authority.md` beyond cross-system identifier references. Qdrant deserves its own later specification if implementation planning confirms it.

## Roadmap consequences

Step 5A should record:

```text
Qdrant comparative audit complete
-> official store-memory model rejected
-> granular JWT/RBAC and typed retrieval gateway identified as leading candidates
-> Kaizen-owned headless indexer required
-> prototype and hammer evidence still required
-> no final server, collection, chunking, or embedding design selected
```

After Results 009, 010, and 011 are reconciled, Kaizen should produce a composed multi-surface capability map across Obsidian, Operational Postgres, and Qdrant before drafting any architecture decision.

## Ordered next actions

1. Preserve this audit and reconciliation as evidence.
2. Review Postgres Result 010 and Qdrant Result 011 together.
3. Do not promote PostgREST or Qdrant JWT/RBAC into accepted doctrine without disposable prototype evidence.
4. Draft a composed Step 5A capability map covering all three stores and their read, write, admin, developer, headless, and interactive lanes.
5. Add prototype and hammer requirements to Phase 7 implementation-planning inputs.
6. Keep Hermes limited to future typed Lane 1 read and retrieval operations.
7. Keep Postgres mutations and Qdrant indexing inside controlled headless services.
8. Keep migrations, collection administration, backup, restore, and diagnostics outside general agent profiles.
9. Continue to Research Batch B only after the Step 5A reconciliation batch is reviewed and checkpointed.

## Source limitation

The raw Claude report was supplied as an uploaded text artifact. This file preserves its load-bearing findings and the project steward's reconciliation. The raw report should also be retained where repository access and archival policy permit.
