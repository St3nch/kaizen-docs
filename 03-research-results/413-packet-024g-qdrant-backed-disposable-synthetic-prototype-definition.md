# Packet 024G — Qdrant-Backed Disposable Synthetic Prototype Definition

Status: planning packet draft — not owner accepted
Date: 2026-06-29
Milestone: 16
Packet: 024G

## Purpose

Define the next Milestone 16 packet after Packet 024F accepted the 024E synthetic local retrieval prototype as the policy-first local proof.

Packet 024G should define whether and how to introduce a local disposable Qdrant-backed synthetic prototype without indexing real Kaizen docs or vault content, without cloud dependency, and without exposing direct Qdrant access to agents.

This packet is a definition packet. It does not install Qdrant, connect to Qdrant, add embeddings, create collections, mutate platform, mutate vault, mutate staging, mutate database state, or index any corpus.

## Authority basis

```text
ROADMAP_V0.4.md
00-entrypoint/LLM_START_HERE.md
03-research-results/412-packet-024f-hammer-tests-and-prototype-audit-result.md
03-research-results/411-packet-024f-hammer-tests-and-prototype-audit.md
03-research-results/410-packet-024e-implementation-return.md
03-research-results/409-packet-024e-disposable-synthetic-local-prototype.md
03-research-results/408-packet-024d-retrieval-gateway-and-typed-tool-contract-result.md
03-research-results/406-packet-024c-chunk-payload-id-and-rebuild-contract-result.md
03-research-results/404-packet-024b-corpus-boundary-and-governance-contract-result.md
03-research-results/402-packet-024a-m16-definition-and-qdrant-evidence-reconciliation-result.md
03-research-results/400-milestone-16-qdrant-research-context-and-hammer-test-direction.md
```

## Starting commits for acceptance

024G planning may be accepted only against these observed commits unless superseded by later explicit owner acceptance:

```text
docs starting commit for this draft: 8446c8c3c67ba9da216c74d9570aaf449c95352c
platform reference commit: f5c8804df4a1e31f9163395c55c82d11dcbeeb46
vault observed commit: 6fbc8cefe4ed4cd57403648ee3977ee2aeaaba5b
```

If any starting commit changes before acceptance, re-verify and amend the packet before implementation planning.

## Current proof basis

024F accepted 024E as the policy-first local proof.

That proof established:

```text
local synthetic Markdown loading;
heading-bounded chunks;
required derivative payloads;
deterministic logical chunk IDs;
deterministic logical point IDs;
source hashes;
embedding-space placeholder identity;
index-generation identity;
gateway-style mandatory filtering;
canonical-style source re-resolution;
context-pack candidate handoff manifest;
synthetic hammer coverage;
no real corpus indexing;
no infrastructure setup;
no agent-facing retrieval tool exposure.
```

024G should preserve all of those constraints while defining the next possible layer: a disposable local Qdrant-backed proof.

## 024G objective

Define the conditions for a later implementation packet that may connect the already-proven synthetic derivative records to a disposable local Qdrant instance.

024G should answer:

```text
what Qdrant-backed behavior needs to be proven;
what infrastructure posture is allowed;
what remains forbidden;
which platform paths may later change;
which tests must pass;
how synthetic records are loaded into the disposable index;
how gateway policy remains the only caller-facing boundary;
how collection/generation/alias behavior is simulated or proven;
how cleanup and non-canonical status are proven;
what stop conditions prevent drift into real corpus work.
```

## Recommended posture

Recommended 024G posture:

```text
local disposable only;
synthetic corpus only;
platform implementation only if later accepted;
no cloud dependency;
no hosted embedding API;
no real docs/vault corpus;
no vault/staging/database mutation;
no production gateway;
no agent-facing retrieval tools;
no direct general-agent access to Qdrant;
no broad admin surface.
```

024G should not force Qdrant introduction if implementation constraints are not favorable. It should define the gate and allow a later accepted packet to choose the least risky local mode.

## Allowed future implementation shape

A later accepted implementation packet may choose one of these local-only approaches:

```text
Qdrant local mode / embedded-style client if available and compatible;
local disposable Qdrant server only when operator setup is explicitly approved;
test-double first with a separately accepted follow-up for real Qdrant if local server setup is too heavy.
```

024G should prefer the smallest approach that proves Qdrant-specific behavior not already proven by 024E.

The later implementation packet must record exact terminal/operator steps if the owner must run anything manually.

## What Qdrant-backed proof must add beyond 024E

024G should require the later Qdrant-backed synthetic prototype to prove only these additional behaviors:

```text
synthetic derivative payloads can be inserted into a disposable vector collection;
mandatory metadata filters are enforceable through the gateway's generated query path;
wrong-project and excluded synthetic records do not appear in gateway-visible results;
active generation constraints prevent stale-generation results;
point IDs remain deterministic;
payload schema and embedding-space markers remain part of retrieval validation;
collection/generation naming does not leak to agent-visible output;
cleanup/removal of disposable index state is documented;
Qdrant state remains derivative and rebuildable from synthetic source fixtures.
```

024G should not attempt to prove retrieval quality, commercial-scale performance, real corpus suitability, or production operations.

## Embedding posture

024G should avoid introducing a real embedding model unless separately justified.

Recommended first Qdrant-backed synthetic option:

```text
use deterministic synthetic vectors with fixed small dimensions for tests;
record embedding_space_id as synthetic;
do not download model weights;
do not call hosted embedding APIs;
do not benchmark semantic quality;
focus on payload filtering, point identity, generation handling, and gateway projection.
```

Reason:

```text
024G is about proving governed retrieval boundaries over Qdrant-backed storage;
semantic embedding quality can wait until the boundary is proven;
model selection remains a later governed decision.
```

## Proposed future platform path scope

A later implementation packet may propose paths similar to:

```text
src/kaizen/m16_qdrant_synthetic_prototype.py
tests/test_m16_qdrant_synthetic_proto.py
```

The existing 024E files may be reused but should not be expanded casually:

```text
src/kaizen/m16_retrieval_prototype.py
tests/test_m16_retrieval_proto.py
tests/fixtures/m16-synthetic-retrieval/*.md
```

Any additional path must be listed in a later accepted implementation packet before mutation.

## Proposed future test families

A later implementation packet should require tests proving:

```text
disposable index can be built from synthetic derivative records;
query path uses gateway-generated constraints rather than caller-controlled backend controls;
allowed synthetic candidate can return;
wrong-project synthetic candidate is excluded;
restricted-class synthetic candidate is excluded;
superseded synthetic candidate is excluded;
volatile operating notes are excluded by default;
stale generation is excluded;
embedding-space mismatch is excluded;
payload-schema mismatch is excluded;
result projection does not reveal backend collection/generation internals beyond accepted safe labels;
context-pack candidate manifest still uses gateway-normalized candidates;
cleanup/rebuild behavior is documented or tested;
existing 024E local tests still pass.
```

## Operator terminal posture

No terminal work is required for this 024G draft.

If a later implementation packet requires the owner to run a local service or install a tool, the packet must include:

```text
exact command;
expected output;
version check;
stop command;
cleanup command;
where data is written;
proof that no real corpus is touched;
proof that no network/cloud dependency is required unless separately accepted.
```

Without those details, do not ask the owner to run terminal commands.

## Expected 024G deliverable

024G execution should create a docs-only definition result:

```text
03-research-results/414-packet-024g-qdrant-backed-disposable-synthetic-prototype-definition-result.md
```

The result should include:

```text
owner acceptance line;
starting commits;
024E/024F proof basis;
allowed local-only Qdrant-backed prototype posture;
forbidden work;
embedding posture;
future path scope candidates;
future test families;
operator terminal posture;
stop conditions;
recommended next implementation packet.
```

## Acceptance criteria for 024G execution

024G execution is complete only if:

```text
no implementation is performed;
no Qdrant instance is installed or contacted;
no embedding model is installed;
no platform files are changed;
no vault files are changed;
no staging files are changed;
no database state is changed;
no real corpus is indexed;
Qdrant-backed synthetic prototype posture is explicit;
operator terminal posture is explicit;
future implementation scope is bounded;
read path is refreshed;
docs commit is pushed.
```

## Stop conditions

Stop immediately if 024G requires:

```text
platform mutation;
vault mutation;
staging mutation;
database mutation;
Qdrant installation or connection;
embedding installation;
network access;
real corpus indexing;
new synthetic fixture creation;
production gateway work;
agent-facing retrieval tools;
path expansion beyond docs definition result/read-path updates.
```

## Non-authorization

This draft packet does not authorize 024G execution until explicitly owner accepted.

Even if accepted, it does not authorize:

```text
Qdrant-backed implementation;
Qdrant installation;
Qdrant connection;
cloud dependency;
embedding model installation;
real corpus indexing;
real docs/vault corpus manifest generation;
platform mutation;
vault mutation;
staging mutation;
database mutation;
production gateway implementation;
agent-facing retrieval tools;
Hermes write authority;
Tauri implementation;
Observatory / IMI implementation;
commercial operations;
backup deletion;
restore work.
```

## Recommended owner acceptance line

Recommended owner action after review:

```text
Accept Packet 024G for docs-only Qdrant-backed disposable synthetic prototype definition, with exact docs-only scope and non-authorization boundaries as listed.
```

After accepted 024G execution, the likely next packet should be:

```text
024H — Qdrant-Backed Disposable Synthetic Prototype Implementation Packet
```

024H should be implementation-bearing only if it defines exact local setup, path scope, tests, cleanup, and stop conditions.
