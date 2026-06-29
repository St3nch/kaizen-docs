# Packet 024E — Disposable Synthetic Local Prototype

Status: implementation packet draft — not owner accepted
Date: 2026-06-29
Milestone: 16
Packet: 024E

## Purpose

Define the first implementation-capable Milestone 16 packet after 024A through 024D established the M16 Qdrant retrieval doctrine, corpus boundary, rebuild contract, and retrieval gateway contract.

Packet 024E should implement only a disposable synthetic local prototype proving the core M16 contracts against synthetic trap content.

This packet must not index real Kaizen docs or vault content.

## Authority basis

```text
ROADMAP_V0.4.md
00-entrypoint/LLM_START_HERE.md
03-research-results/408-packet-024d-retrieval-gateway-and-typed-tool-contract-result.md
03-research-results/407-packet-024d-retrieval-gateway-and-typed-tool-contract.md
03-research-results/406-packet-024c-chunk-payload-id-and-rebuild-contract-result.md
03-research-results/404-packet-024b-corpus-boundary-and-governance-contract-result.md
03-research-results/402-packet-024a-m16-definition-and-qdrant-evidence-reconciliation-result.md
03-research-results/400-milestone-16-qdrant-research-context-and-hammer-test-direction.md
03-research-results/399-milestone-15-owner-closure-acceptance.md
03-research-results/398-packet-023i-m15-integrated-proof-and-closure-audit-result.md
03-research-results/396-packet-023h-implementation-return.md
```

## Starting commits for acceptance

024E implementation may be accepted only against these starting commits unless superseded by a later explicit owner acceptance:

```text
docs starting commit for this draft: b4485164c30b00342b8431f4e16295832320d243
platform starting commit: 84071eef1e5859f651d3213c65f3bfc99a3c94f8
vault observed commit: 6fbc8cefe4ed4cd57403648ee3977ee2aeaaba5b
```

If any starting commit changes before acceptance, re-verify and amend the packet before implementation.

## 024E objective

Implement the minimum platform-only synthetic prototype needed to prove these M16 contracts without real corpus indexing:

```text
synthetic corpus records;
M15-compatible typed-section / stable-heading chunking behavior;
required payload construction;
deterministic chunk IDs;
deterministic point IDs;
source hashes;
embedding-space identity placeholder;
index-generation identity;
gateway-style mandatory filter enforcement;
canonical-style re-resolution against synthetic source fixtures;
rejection of wrong-project/private/superseded/current-state/command-center/stale/untraceable candidates;
context-pack candidate handoff records;
unit hammer tests for the synthetic gateway boundary.
```

024E is not required to use a live Qdrant server.

024E should prefer a pure local/deterministic in-memory prototype unless later implementation evidence shows a tiny local dependency is needed.

## Design posture

The first useful prototype should prove Kaizen policy mechanics before proving Qdrant infrastructure.

Recommended 024E posture:

```text
use platform test fixtures representing synthetic Markdown records;
parse/chunk the synthetic records through deterministic logic;
build Qdrant-shaped derivative records without contacting Qdrant;
use an in-memory fake/vector result layer or deterministic candidate list to simulate retrieval output;
run gateway validation and rejection logic against those synthetic candidates;
return normalized candidate records only;
prove no raw Qdrant control surface exists.
```

Reason:

```text
M16 risk is governance leakage, not Qdrant's ability to search;
synthetic tests should prove the policy wall before introducing Qdrant server complexity;
Qdrant-backed prototype can follow in a later packet if 024E passes.
```

## Allowed implementation root

Allowed implementation root:

```text
platform only
```

Docs changes during 024E execution are limited to implementation-return/result documentation and read-path refresh.

## Proposed platform path scope

Recommended platform paths for 024E implementation:

```text
src/kaizen/m16_retrieval_prototype.py
tests/test_m16_retrieval_prototype.py
tests/fixtures/m16-synthetic-retrieval/project-a-approved-decision.md
tests/fixtures/m16-synthetic-retrieval/project-b-approved-decision.md
tests/fixtures/m16-synthetic-retrieval/project-a-private-note.md
tests/fixtures/m16-synthetic-retrieval/project-a-superseded-decision.md
tests/fixtures/m16-synthetic-retrieval/project-a-current-state.md
tests/fixtures/m16-synthetic-retrieval/project-a-command-center.md
tests/fixtures/m16-synthetic-retrieval/project-a-draft-note.md
tests/fixtures/m16-synthetic-retrieval/project-a-rejected-note.md
tests/fixtures/m16-synthetic-retrieval/project-a-prompt-injection.md
tests/fixtures/m16-synthetic-retrieval/project-a-bad-metadata.md
```

If implementation reveals that fewer fixtures are sufficient, reduce scope before writing. If additional paths are needed, stop and amend the packet.

## Proposed docs path scope for implementation return

Recommended docs paths after implementation:

```text
03-research-results/410-packet-024e-implementation-return.md
00-entrypoint/LLM_START_HERE.md
ROADMAP_V0.4.md
```

Do not update vault or staging.

## Prototype records and trap cases

Synthetic fixture set must cover:

```text
Project A approved decision eligible for retrieval;
Project B approved decision with similar wording that must not leak into Project A retrieval;
Project A private note that must never return;
Project A superseded decision that matches the query but must be rejected;
Project A current-state note excluded by default;
Project A command-center note excluded by default;
Project A draft/proposed note excluded by default;
Project A rejected note excluded by default;
Project A prompt-injection note treated as untrusted evidence;
Project A bad-metadata note rejected fail-closed.
```

Optional if still narrow:

```text
archived note;
missing-source-hash candidate;
stale generation candidate;
embedding-space mismatch candidate;
payload-schema mismatch candidate.
```

## Required prototype functions

024E implementation should expose pure Python functions or dataclasses equivalent to:

```text
load_synthetic_markdown_records(paths) -> records;
build_synthetic_chunks(records, chunker_version=...) -> chunks;
build_derivative_payloads(chunks, embedding_space_id=..., index_generation_id=...) -> payload records;
build_logical_chunk_id(...);
build_logical_point_id(...);
build_source_hash(...);
filter_gateway_candidates(request, derivative_records, trusted_policy) -> candidate/rejection result;
re_resolve_candidate(candidate, synthetic_source_records) -> pass/fail;
build_context_pack_candidate_manifest(request, accepted_candidates, rejected_candidates) -> manifest.
```

Names may vary, but behavior must remain pure, deterministic, and testable.

## Required tests

024E must add unit tests proving:

```text
approved Project A record can be returned for Project A request;
Project B similar record is blocked from Project A request;
private record is rejected;
superseded record is rejected;
current-state record is rejected by default;
command-center record is rejected by default;
draft/proposed/rejected record is rejected;
prompt-injection text does not alter instructions or policy;
bad metadata fails closed;
chunk IDs are deterministic;
point IDs are deterministic;
source hashes detect content changes;
inactive/stale index generation is rejected;
embedding-space mismatch is rejected;
payload-schema mismatch is rejected;
agent-supplied raw filter / collection / point-ID controls are rejected or ignored;
result projection does not include raw point IDs, collection names, raw payload, raw vectors, or full source blobs;
context-pack candidate manifest includes accepted candidates and rejection reasons without leaking private text.
```

## Required test commands

Minimum test commands:

```text
python -m pytest tests/test_m16_retrieval_prototype.py
python -m pytest tests/test_markdown.py tests/test_validation.py tests/test_m15_read_model.py tests/test_m16_retrieval_prototype.py
```

If test names differ, implementation return must record the exact commands actually run and why they satisfy 024E scope.

## Expected implementation-return content

024E implementation return should record:

```text
owner acceptance line;
starting commits;
changed platform paths;
changed docs paths;
functionality implemented;
synthetic fixture list;
contract points proven;
tests run and exact results;
non-authorization preserved;
residual risks;
recommended next packet.
```

## Non-goals

024E must not do any of the following:

```text
install Qdrant;
connect to Qdrant;
use Qdrant Cloud;
install embedding models;
call hosted embedding APIs;
index real docs content;
index real vault content;
create real corpus manifest;
create Qdrant collections or aliases;
implement production gateway service;
create actual agent-facing retrieval tools;
modify vault;
modify staging;
mutate database;
modify kaizen_mcp;
modify chatgpt_mcp;
add Hermes write authority;
implement Tauri UI;
implement Observatory / IMI;
use provider/customer data;
perform commercial operations.
```

## Stop conditions

Stop immediately if implementation requires:

```text
Qdrant installation or connection;
new external service;
network access;
embedding model download;
real corpus data;
vault mutation;
staging mutation;
database mutation;
agent-facing tool exposure;
raw Qdrant API/MCP exposure;
secrets;
customer/provider/private data;
path expansion beyond accepted platform/docs paths;
changing accepted M16 contracts instead of implementing the synthetic proof.
```

## Acceptance criteria for 024E implementation

024E implementation may be considered complete only if:

```text
platform implementation stays within accepted path scope;
docs implementation return is written;
no vault/staging/database mutation occurs;
no Qdrant install or connection occurs;
no embedding model install occurs;
no real corpus indexing occurs;
synthetic fixtures cover required trap classes;
chunk/payload/ID/source-hash/index-generation behavior is deterministic;
gateway mandatory filters reject unsafe candidates;
canonical-style re-resolution rejects stale/untraceable candidates;
context-pack candidate manifest is generated from accepted/rejected synthetic candidates;
required tests pass;
read path is refreshed;
platform commit is local;
docs commit is pushed.
```

## Non-authorization

This draft does not authorize 024E implementation until explicitly owner accepted.

Even if accepted, it does not authorize:

```text
real Qdrant installation;
real Qdrant connection;
Qdrant Cloud;
embedding model installation;
real corpus indexing;
real docs/vault corpus manifest generation;
vault mutation;
staging mutation;
database mutation;
production gateway implementation;
agent-facing retrieval tools;
Hermes write authority;
Tauri implementation;
Observatory / IMI implementation;
provider collection;
customer-data reuse;
commercial operations;
backup deletion;
restore work.
```

## Recommended owner acceptance line

Recommended owner action after review:

```text
Accept Packet 024E for narrow platform implementation of the disposable synthetic local retrieval prototype at platform starting commit 84071eef1e5859f651d3213c65f3bfc99a3c94f8, with exact path scope, tests, and non-authorization boundaries as listed.
```

After accepted 024E implementation, the likely next packet should be:

```text
024F — Hammer Tests and Prototype Implementation Return
```
