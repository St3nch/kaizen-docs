# Packet 024H — Qdrant-Backed Disposable Synthetic Prototype Implementation Packet

Status: implementation packet draft — not owner accepted
Date: 2026-06-29
Milestone: 16
Packet: 024H

## Purpose

Define the first implementation-capable packet for a local disposable Qdrant-backed synthetic prototype.

024H follows Packet 024G. It is still a draft. It does not authorize implementation until explicitly owner accepted.

## Owner environment fact

Owner clarified:

```text
Docker Desktop for Windows is installed.
The machine is a Windows host, not a VM.
```

024H therefore plans for Windows-host Docker Desktop.

## Authority basis

```text
ROADMAP_V0.4.md
00-entrypoint/LLM_START_HERE.md
03-research-results/414-packet-024g-qdrant-backed-disposable-synthetic-prototype-definition-result.md
03-research-results/412-packet-024f-hammer-tests-and-prototype-audit-result.md
03-research-results/410-packet-024e-implementation-return.md
03-research-results/408-packet-024d-retrieval-gateway-and-typed-tool-contract-result.md
03-research-results/406-packet-024c-chunk-payload-id-and-rebuild-contract-result.md
03-research-results/404-packet-024b-corpus-boundary-and-governance-contract-result.md
03-research-results/402-packet-024a-m16-definition-and-qdrant-evidence-reconciliation-result.md
```

## Starting commits for acceptance

```text
docs starting commit for this draft: 40ed81a0b972cb533e07c47a7ff4228210821dc9
platform implementation starting commit: f5c8804df4a1e31f9163395c55c82d11dcbeeb46
vault observed commit: 6fbc8cefe4ed4cd57403648ee3977ee2aeaaba5b
```

If any starting commit changes before acceptance, re-verify and amend the packet before implementation.

## Official setup basis

Qdrant local quickstart states that Docker should be installed and running before local use, exposes local service ports 6333 and 6334, and notes that Windows may need a named Docker volume instead of a local folder mount.

024H therefore prefers a named disposable Docker volume for this Windows-host proof.

## 024H objective

Implement the minimum platform-only Qdrant-backed disposable synthetic prototype needed to prove Qdrant-specific storage/query behavior that 024E did not prove.

The implementation should prove:

```text
synthetic derivative records can be written into a disposable local vector collection;
fixed deterministic synthetic vectors can be used without embedding downloads;
metadata filters can participate in the gateway-generated query path;
gateway validation remains the caller-facing policy boundary;
wrong-project and excluded synthetic candidates do not appear in accepted gateway-visible results;
stale generation and schema/embedding mismatches are rejected;
point IDs remain deterministic;
backend internals do not appear in general candidate projection;
disposable index state can be cleaned up;
backend state remains derivative and rebuildable from synthetic fixtures.
```

024H must not prove semantic retrieval quality, real-corpus suitability, production operations, or agent-facing retrieval tools.

## Allowed implementation root

Allowed implementation root:

```text
platform only
```

Docs changes during 024H execution are limited to implementation-return/result documentation and read-path refresh.

Vault, staging, database, kaizen_mcp, and chatgpt_mcp must not be mutated.

## Proposed platform path scope

Recommended platform paths for 024H implementation:

```text
src/kaizen/m16_qdrant_synthetic_prototype.py
tests/test_m16_qdrant_synthetic_proto.py
```

Permitted reuse/read-only dependency paths:

```text
src/kaizen/m16_retrieval_prototype.py
tests/test_m16_retrieval_proto.py
tests/fixtures/m16-synthetic-retrieval/*.md
```

Any additional path requires packet amendment before mutation.

## Proposed docs path scope for implementation return

Recommended docs paths after implementation:

```text
03-research-results/416-packet-024h-implementation-return.md
00-entrypoint/LLM_START_HERE.md
ROADMAP_V0.4.md
```

## Operator terminal posture

If 024H is accepted, terminal work may be required because Qdrant is not currently running.

The future implementation return must record:

```text
Docker availability check;
image pull evidence;
local container start evidence;
local-only port posture;
named disposable volume;
stop command evidence;
cleanup command evidence;
proof that no docs or vault folder was mounted;
proof that no real corpus was indexed.
```

The future accepted implementation packet may ask the owner to run exact PowerShell commands. No terminal work is authorized by this draft.

## Implementation design posture

024H should prefer deterministic synthetic vectors over real embeddings.

Recommended implementation design:

```text
reuse 024E synthetic fixtures and derivative payload construction;
generate fixed small vectors from deterministic chunk IDs or stable fixture order;
create one disposable synthetic collection;
write synthetic derivative records with deterministic point IDs;
query through platform-owned prototype functions only;
apply mandatory policy filters in gateway-generated query path;
validate returned records with existing gateway/re-resolution logic;
return gateway-normalized candidates only;
record cleanup expectations in implementation return.
```

## Dependency posture

024H may require Python client support for the local vector backend.

Before adding a new dependency, implementation must check current platform dependency posture.

Allowed options:

```text
use an existing installed client if already present;
if not present, stop and record that dependency addition requires separate approval;
do not download packages during 024H unless the accepted packet explicitly approves dependency installation;
do not use hosted embedding or inference services.
```

Because this draft does not yet approve package installation, implementation must stop if the platform environment lacks the required client.

## Required implementation functions

024H implementation should expose pure wrapper functions or equivalent behavior for:

```text
build_synthetic_vector;
build_synthetic_collection_name;
create_disposable_collection;
upsert_synthetic_derivative_records;
query_synthetic_collection;
delete_disposable_collection;
verify_disposable_collection_absent.
```

Names may vary, but the behavior must remain narrow and testable.

## Required tests

024H implementation must add tests proving:

```text
synthetic vector generation is deterministic;
collection name is deterministic and synthetic-only;
disposable collection can be created;
synthetic derivative records can be inserted;
allowed candidate can be returned through gateway-normalized query;
wrong-project candidate is not accepted;
restricted-class candidate is not accepted;
superseded candidate is not accepted;
volatile operating notes remain excluded by default;
stale generation is rejected;
embedding-space mismatch is rejected;
payload-schema mismatch is rejected;
backend internals are not exposed in general result projection;
context-pack candidate manifest still uses gateway-normalized candidates;
cleanup removes the disposable collection;
existing 024E tests still pass.
```

## Required test commands

Minimum tests if local backend container and client are available:

```text
python -m pytest -k m16_qdrant_synthetic_proto
python -m pytest -k "m16_qdrant_synthetic_proto or m16_retrieval_proto"
python -m pytest -k "m16_qdrant_synthetic_proto or m16_retrieval_proto or test_markdown or test_validation or test_m15_read_model"
```

If local backend or client support is unavailable, implementation must stop or record bounded skip/failure evidence and must not be represented as complete.

## Expected implementation-return content

024H implementation return should record:

```text
owner acceptance line;
starting commits;
Docker/Desktop/local host posture;
operator commands actually run, if any;
backend image/version evidence;
container and volume names;
changed platform paths;
changed docs paths;
functionality implemented;
tests run and exact results;
cleanup result;
non-authorization preserved;
residual risks;
recommended next packet.
```

## Acceptance criteria for 024H implementation

024H implementation may be considered complete only if:

```text
platform implementation stays within accepted path scope;
local backend is disposable only;
synthetic fixtures are the only indexed material;
no docs/vault/staging/database mutation occurs;
no cloud dependency is introduced;
no hosted embedding or model download occurs;
no real corpus is indexed;
disposable state is cleaned up or cleanup residual is recorded;
gateway remains the only caller-facing retrieval boundary;
required tests pass or accepted stop condition is triggered before claim of completion;
read path is refreshed;
platform commit is local;
docs commit is pushed.
```

## Stop conditions

Stop immediately if implementation requires:

```text
real corpus data;
docs or vault folder mount into the backend container;
cloud account or remote service;
embedding model download;
new Python package installation without separate approval;
platform path expansion beyond accepted scope;
vault mutation;
staging mutation;
database mutation;
agent-facing tool exposure;
production gateway work;
state that cannot be cleaned up;
network exposure beyond localhost;
Docker unavailable or not running.
```

## Non-goals

024H must not do any of the following:

```text
index real Kaizen docs;
index real vault notes;
create a real corpus manifest;
implement production retrieval gateway;
create agent-facing retrieval tools;
use cloud backend;
install embedding models;
call hosted embedding APIs;
benchmark semantic quality;
modify vault;
modify staging;
mutate database;
modify kaizen_mcp;
modify chatgpt_mcp;
add Hermes write authority;
implement Tauri UI;
implement Observatory / IMI;
perform commercial operations.
```

## Non-authorization

This draft does not authorize 024H implementation until explicitly owner accepted.

Even if accepted, it does not authorize:

```text
real corpus indexing;
real docs/vault corpus manifest generation;
cloud dependency;
embedding model installation;
package installation unless explicitly included in the accepted packet;
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
Accept Packet 024H for narrow platform implementation of the Qdrant-backed disposable synthetic prototype at platform starting commit f5c8804df4a1e31f9163395c55c82d11dcbeeb46, using Windows-host Docker Desktop local-only setup, exact path scope, tests, cleanup, and non-authorization boundaries as listed.
```

After accepted 024H implementation, the likely next packet should be:

```text
024I — Qdrant-Backed Disposable Synthetic Prototype Hammer Audit
```
