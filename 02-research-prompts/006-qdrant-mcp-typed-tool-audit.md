# Claude Research Prompt 006 - Qdrant MCP and Typed-Tool Comparative Audit

Status: active research prompt
Date: 2026-06-05

## Prompt

You are conducting the Qdrant portion of Kaizen Research Step 5A.

This is a comparative research and architecture-evaluation task only.

Do not edit the Kaizen repository.
Do not create files in it.
Do not stage, commit, push, reset, clean, or discard changes.
Do not connect any candidate to a real Kaizen Qdrant instance.
Do not install or run tools against valuable collections.
Do not select a final server from README claims alone.
Do not design exact collections, payload schemas, chunk sizes, or embedding models.
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

Treat this repository as Kaizen's planning and stewardship workspace, not a running vector-database project.

## Central objective

Compare prominent Qdrant MCP servers, gateways, SDK wrappers, retrieval APIs, and operational tooling against Kaizen's accepted authority, privacy, rebuildability, project isolation, provenance, freshness, and capability-lane requirements.

Determine which capabilities Kaizen should later:

```text
reuse
configure
wrap
fork
expose read-only
reserve for indexing services
reserve for human/admin use
reject
```

Do not optimize only for the smallest tool surface. Preserve useful Qdrant ecosystem capabilities while separating retrieval, indexing, mutation, collection administration, destructive, and authority-bearing operations.

## Required Kaizen reading

Read these files first:

```text
00-entrypoint/LLM_START_HERE.md
01-project-standard/kaizen-vision-and-architecture-alignment.md
ROADMAP.md
04-design-decisions/0003-raw-markdown-is-canonical.md
04-design-decisions/0004-system-of-record-boundaries.md
04-design-decisions/0005-api-only-structured-data-access.md
04-design-decisions/0006-hammer-tests-are-a-hard-gate.md
05-specs/operational-postgres-authority.md
05-specs/kaizen-hammer-test-strategy.md
07-hermes/hermes-permission-matrix.md
03-research-results/004-markdown-qdrant-postgres-architecture-claude-summary.md
03-research-results/007-kaizen-vision-architecture-research-gap-audit-claude-summary.md
03-research-results/009-obsidian-mcp-tool-surface-audit-claude-summary.md
```

## Accepted Kaizen constraints

Treat these as fixed unless you identify a direct contradiction requiring human review:

- Qdrant is a rebuildable semantic retrieval layer over approved canonical knowledge.
- Qdrant owns no unique canonical truth.
- Canonical Markdown remains the source for approved project intelligence.
- Private or customer data is excluded unless explicitly governed.
- Agents receive no direct unrestricted Qdrant mutation or collection administration.
- Agent-facing Qdrant access must use approved typed, scoped, logged tools or APIs.
- Project, privacy, authority, freshness, supersedence, and note-type filters matter as much as vector similarity.
- Read-only is not automatically safe: retrieval can leak private payloads, out-of-scope projects, deleted/superseded chunks, collection names, or metadata.
- Index construction, embedding generation, reindexing, collection creation, payload-index changes, snapshots, and destructive operations belong to controlled operational or admin contexts.
- Ecosystem capabilities should be reused where valuable, but authority-bearing logic remains governed by Kaizen.

## Capability-lane model

Use these lanes when evaluating candidates.

### Lane 1 - Governed semantic retrieval

Potential capabilities:

- project-scoped semantic search;
- payload-filtered retrieval;
- hybrid dense/sparse search;
- reranking where supported;
- retrieval of source IDs, headings, and approved metadata;
- result limits and confidence information;
- exclusion of stale, superseded, private, or unapproved content.

### Lane 2 - Controlled indexing and synchronization

Capabilities reserved for approved services:

- chunk ingestion;
- embedding upsert;
- payload updates;
- stale-chunk removal;
- incremental reindexing;
- full rebuilds from canonical Markdown;
- source-hash reconciliation;
- provenance and schema-version recording.

These are operational service capabilities, not general reasoning-agent tools.

### Lane 3 - Collection and infrastructure administration

Human/admin or deployment capabilities:

- create, delete, alias, rename, or migrate collections;
- payload-index administration;
- snapshots and restore;
- shard and replication changes;
- optimizer configuration;
- cluster management;
- API-key and access-control administration;
- destructive cleanup and disaster recovery.

These are never exposed to Hermes.

### Lane 4 - Developer and diagnostic access

Potentially useful in disposable or tightly controlled environments:

- collection inspection;
- payload-schema inspection;
- point sampling;
- query diagnostics;
- indexing verification;
- rebuild comparison;
- performance and recall testing.

This lane requires separate credentials, environments, and audit policy.

## Candidate discovery

Research current maintained candidates as of the report date.

At minimum inspect:

1. official Qdrant MCP integrations, if any;
2. prominent community Qdrant MCP servers;
3. generic vector-database MCP servers supporting Qdrant;
4. Qdrant official REST, gRPC, and SDK capabilities relevant to typed gateways;
5. retrieval frameworks or gateways that expose Qdrant search to agents;
6. one read/search-only surface;
7. one indexing/admin-oriented surface for contrast;
8. managed Qdrant Cloud controls where relevant.

Do not assume candidate names in this prompt are exact or recommended. Verify official identity, repository, owner, maintenance, transport, authentication, and tool schemas.

## Source standards

Prefer primary sources:

- Qdrant official documentation and repositories;
- candidate source code and MCP tool schemas;
- official SDK documentation;
- managed-service security and authentication documentation;
- issue trackers and release notes;
- security advisories and CVE databases.

For every candidate, record the date checked.

Do not infer safety from popularity, localhost use, hosted deployment, or absence of reported vulnerabilities.

## Required candidate analysis

### 1. Maintenance and provenance

Record:

- official identity and repository;
- owner and license;
- latest release and commit;
- maintenance status;
- contributor concentration;
- unresolved security-relevant issues;
- official, vendor-maintained, community, experimental, or abandoned status.

### 2. Architecture and transport

Determine:

- MCP server, REST wrapper, gRPC wrapper, SDK adapter, retrieval framework, or managed gateway;
- STDIO, HTTP, SSE, WebSocket, REST, gRPC, or hosted transport;
- authentication model;
- credential storage;
- TLS requirements;
- local versus hosted assumptions;
- whether the tool runs beside Qdrant, in a trusted service tier, or on a client machine.

### 3. Complete tool inventory

Inspect tool registration and schemas.

Classify each tool as:

```text
search
recommendation
hybrid search
scroll/list points
point retrieve
payload metadata read
collection metadata read
upsert
payload update
point delete
batch mutation
collection create/delete
alias administration
payload-index administration
snapshot/restore
cluster administration
embedding generation
rebuild/reindex
diagnostic
other
```

For every tool, record:

- exact name;
- request fields;
- response shape;
- collection and payload controls;
- whether caller chooses collection names;
- whether filters are caller-controlled or service-injected;
- mutation and destructive potential;
- whether unsafe tools can be absent from discovery;
- logging and error behavior.

### 4. Retrieval confinement

Read-only retrieval can still leak protected data.

Inspect whether the candidate enforces:

- project scope;
- collection allowlists;
- payload-field allowlists;
- private-data exclusion;
- authority-state filtering;
- supersedence filtering;
- freshness or stale-content filtering;
- note-type filtering;
- source and provenance requirements;
- result-size limits;
- score thresholds;
- pagination/scroll restrictions;
- exact-point retrieval restrictions;
- collection-discovery restrictions.

Determine whether filters are:

```text
injected by trusted service
caller supplied and validated
caller supplied but optional
hard-coded
not enforced
unclear
```

### 5. Collection and payload exposure

Determine whether the candidate exposes:

- collection names;
- collection configuration;
- point IDs;
- full payloads;
- vectors;
- shard/replication metadata;
- optimizer settings;
- snapshots;
- aliases;
- API keys or service metadata.

Classify useful metadata versus unnecessary attack-surface or privacy exposure.

### 6. Mutation semantics

For every mutation tool, determine:

- typed indexing operation versus generic point mutation;
- project and source scope;
- source-hash and content-hash requirements;
- embedding-model/version capture;
- schema-version capture;
- idempotency;
- batch behavior;
- partial failure handling;
- stale-point deletion;
- supersedence behavior;
- provenance;
- audit evidence;
- whether the operation can inject unsupported facts into retrieval.

Compare against the rule that Qdrant must be rebuildable from canonical Markdown.

### 7. Configure-versus-wrap analysis

Determine how unsafe capabilities become unavailable.

#### Configure

Acceptable only when:

- mutation/admin tools are absent from discovery;
- credentials are collection- or operation-scoped where possible;
- collection and payload scope are enforced independently;
- generic collection selection is absent;
- configuration is capturable and testable.

#### Wrap

Required when:

- one API key grants broad collection access;
- the upstream API exposes search and destructive mutation through the same credential;
- filters must be injected by trusted Kaizen logic;
- payload fields need redaction;
- authority, supersedence, privacy, freshness, or project scope must be mandatory;
- collection names must be hidden;
- audit and rate controls need to be added.

A wrapper must expose typed retrieval or indexing operations and no generic pass-through endpoint.

### 8. Authentication and access control

Evaluate:

- API keys;
- read-only versus read/write credentials;
- collection-level or cluster-level scope;
- managed-cloud controls;
- TLS;
- key rotation;
- service identities;
- tenant/project separation;
- credential propagation through gateways;
- local development credentials;
- audit identity.

### 9. Hybrid retrieval and ecosystem value

Identify mature capabilities Kaizen should reuse rather than rebuild, such as:

- dense vector search;
- sparse vector search;
- hybrid queries;
- filters and payload indexes;
- recommendations;
- reranking integrations;
- quantization;
- snapshots;
- aliases;
- replication and sharding;
- client SDKs;
- managed hosting;
- monitoring;
- backup and restore.

Separate useful infrastructure from authority-bearing Kaizen logic.

### 10. Headless and interactive contexts

#### Headless indexing/retrieval context

Examples:

- nightly or event-driven indexing;
- full rebuild from canonical Markdown;
- stale-point cleanup;
- CI validation;
- recovery and restore;
- automated context-pack assembly.

These use controlled services and typed operations.

#### Interactive retrieval context

Examples:

- agent semantic search;
- human exploration;
- project command-center search;
- claim or decision review;
- context discovery.

Interactive retrieval must still enforce project, privacy, authority, supersedence, freshness, and result-size limits.

### 11. Rebuildability and drift detection

Evaluate how candidates support:

- deterministic point IDs;
- source-path and heading IDs;
- content hashes;
- embedding model/version;
- payload schema version;
- incremental updates;
- stale point detection;
- full wipe and rebuild;
- rebuild comparison;
- orphan detection;
- source-to-index reconciliation;
- snapshot and restore without confusing snapshots with canonical truth.

### 12. Logging, audit, and observability

Determine whether the candidate records:

- actor/client identity;
- session and request ID;
- tool name;
- project and collection scope;
- injected filters;
- search text or redacted representation;
- result count and point IDs;
- mutation count;
- duration and timeout;
- denial reason;
- embedding model/version;
- rebuild/index run ID;
- source hashes;
- partial failure.

Check whether logs can expose private queries, payloads, or document content.

## Comparative architectural options

Compare:

### Option A - Direct Qdrant MCP

```text
agent -> MCP tools -> Qdrant
```

### Option B - Read-only reduced Qdrant MCP

```text
agent -> search-only tools -> scoped credentials/collections -> Qdrant
```

### Option C - Retrieval gateway

```text
agent -> Kaizen retrieval gateway -> Qdrant API/SDK
```

### Option D - Kaizen indexing service plus retrieval tools

```text
canonical Markdown -> Kaizen indexer -> Qdrant
agent -> Kaizen retrieval tools -> Qdrant
```

### Option E - Hybrid

```text
Kaizen-owned indexing/rebuild service
+
wrapped ecosystem retrieval capabilities
+
separate human/admin Qdrant tooling
```

Compare security, scope, ecosystem value, logging, maintenance burden, headless operation, rebuildability, and suitability for Hermes, reasoning agents, operational workers, developers, and humans.

## Hands-on prototype recommendations

Do not run prototypes in this task.

For candidates worth considering, define disposable tests using a temporary Qdrant instance and synthetic Markdown-derived data only.

At minimum test:

- exact tool-discovery snapshot;
- absence versus denial of mutation/admin tools;
- project A cannot retrieve project B;
- private payloads remain hidden;
- unapproved and superseded chunks are excluded;
- stale chunks are excluded or explicitly labeled;
- collection names and admin metadata remain hidden where required;
- exact-point retrieval cannot bypass filters;
- scroll/list operations cannot dump the collection;
- result-size and timeout limits;
- generic upsert/delete is unavailable to agents;
- indexing is idempotent;
- stale-point cleanup is correct;
- full rebuild reproduces expected point inventory;
- hash/model/schema drift is detected;
- audit-log completeness;
- credentials cannot administer collections or snapshots through agent tools.

## Required output

Return one report with these sections.

### 1. Executive verdict

State:

- whether any candidate is suitable as-is;
- which retrieval surfaces are promising;
- whether Kaizen needs its own indexing and retrieval gateway;
- which ecosystem capabilities should be reused;
- the strongest and weakest architectures;
- the most important hands-on tests.

### 2. Repository and Git state observed

Include branch, latest commit, clean or dirty status, and no-write confirmation.

### 3. Candidate inventory

Use columns:

```text
Candidate
Official identity
Architecture
Maintenance
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
Collection choice
Mutation/admin potential
Can be absent?
Kaizen concern
```

### 5. Scope and security matrix

Use columns:

```text
Candidate
Credential scope
Collection scope
Mandatory filters
Payload/privacy controls
Result limits
Logging
Confidence
```

### 6. Retrieval comparison

Compare dense, sparse, hybrid, filtered, recommendation, scroll, and exact-point operations.

### 7. Mutation and administration comparison

Compare upsert, payload update, delete, collection/index administration, snapshots, and destructive capabilities.

### 8. Configure-versus-wrap matrix

For each candidate, state whether safe reduction is achieved by configuration, credentials, a wrapper, a fork, or rejection.

### 9. Authentication and credential analysis

Cover API keys, managed cloud controls, collection scope, rotation, service identities, and identity propagation.

### 10. Rebuildability and drift analysis

Explain how Kaizen can prove Qdrant is derivative, current, scoped, and reproducible.

### 11. Known vulnerabilities and security-relevant issues

Use primary evidence. Separate confirmed vulnerabilities, unresolved issues, and theoretical risks.

### 12. Ecosystem-value map

Identify what Kaizen should reuse rather than rebuild.

### 13. Kaizen capability-lane recommendation

Recommend a composed Qdrant architecture without designing exact collections or payload schemas.

### 14. Prototype and hammer-test plan

Provide candidate-specific disposable tests.

### 15. Files likely needing later revision

List exact Kaizen paths and likely changes. Do not edit them.

### 16. Ordered next actions

Give a dependency-aware sequence.

### 17. Final no-write confirmation

Confirm no files, installs, Qdrant connections, staging, commits, or pushes occurred.

## Important constraints

Do not:

- recommend direct unrestricted Qdrant access for Hermes;
- assume search-only is safe without mandatory project, privacy, authority, supersedence, freshness, and result limits;
- expose generic scroll, exact-point, upsert, delete, collection, snapshot, or admin tools to general agents;
- make Qdrant canonical truth;
- design exact collections, chunk sizes, payload schemas, or embedding models;
- discard mature Qdrant capabilities merely because Kaizen needs typed authority boundaries;
- connect to a real Qdrant instance;
- produce the implementation roadmap.

Be skeptical, source-driven, and precise.

Return the report only.
