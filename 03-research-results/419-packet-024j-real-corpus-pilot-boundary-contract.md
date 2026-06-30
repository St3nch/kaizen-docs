# Packet 024J — Real Corpus Pilot Boundary Contract

Status: complete — docs-only contract
Date: 2026-06-30
Milestone: 16

## Owner approval

Accepted scope:

```text
define first approved real Markdown corpus pilot boundary;
define eligible and excluded note types;
define source path boundaries;
define privacy, authority, supersedence, freshness, and rebuild rules;
define audit expectations for the later real-corpus implementation packet.
```

Non-authorization:

```text
no real corpus indexing;
no Qdrant writes;
no platform mutation;
no vault mutation;
no staging mutation;
no database mutation;
no embedding model installation;
no agent-facing retrieval tools;
no production gateway.
```

## Purpose

024J defines the first governed real-corpus pilot boundary after the synthetic backend proof and hammer audit passed.

This packet does not perform indexing. It defines what a later implementation packet may index and what that packet must prove.

## First pilot corpus

The first real corpus pilot is limited to the Kaizen docs repository, not the vault.

Allowed root:

```text
docs
```

Allowed source path prefix:

```text
03-research-results/
```

Allowed files:

```text
400-milestone-16-qdrant-research-context-and-hammer-test-direction.md
401-packet-024a-m16-definition-and-qdrant-evidence-reconciliation.md
402-packet-024a-m16-definition-and-qdrant-evidence-reconciliation-result.md
403-packet-024b-corpus-boundary-and-governance-contract.md
404-packet-024b-corpus-boundary-and-governance-contract-result.md
405-packet-024c-chunk-payload-id-and-rebuild-contract.md
406-packet-024c-chunk-payload-id-and-rebuild-contract-result.md
407-packet-024d-retrieval-gateway-and-typed-tool-contract.md
408-packet-024d-retrieval-gateway-and-typed-tool-contract-result.md
409-packet-024e-disposable-synthetic-local-prototype.md
410-packet-024e-implementation-return.md
411-packet-024f-hammer-tests-and-prototype-audit.md
412-packet-024f-hammer-tests-and-prototype-audit-result.md
413-packet-024g-qdrant-backed-disposable-synthetic-prototype-definition.md
414-packet-024g-qdrant-backed-disposable-synthetic-prototype-definition-result.md
415-packet-024h-qdrant-backed-disposable-synthetic-prototype-implementation.md
416-packet-024h-implementation-return.md
417-packet-024h-qdrant-synthetic-proof-record.md
418-packet-024i-qdrant-synthetic-proof-hammer-audit.md
419-packet-024j-real-corpus-pilot-boundary-contract.md
```

Reason:

```text
These records are already public-style Kaizen governance/proof records for M16 itself. They are real Markdown, have known provenance, and avoid vault/customer/private/project data while still being semantically meaningful enough for a first real-corpus pilot.
```

## Eligible note shape

A file is eligible only if all are true:

```text
it is a Markdown file under the allowed source path prefix;
it is listed in this contract or in a later owner-approved amendment;
it is already committed in docs;
it is not marked draft, private, rejected, superseded, archived, volatile, or secret;
it has stable source path and source SHA-256 at index time;
it can be parsed by the existing Markdown/read-model path or a bounded M16 pilot reader;
it is treated as evidence text, not executable instruction.
```

## Excluded content

The first real corpus pilot must exclude:

```text
vault files;
staging files;
platform source files;
platform tests and fixtures;
chatgpt_mcp files;
kaizen_mcp files;
Northstar pilot repo files;
logs;
raw private data;
customer or provider data;
secret-bearing files;
backup archives or manifests;
current-state notes;
command-center notes;
roadmaps outside this exact M16 pilot list;
draft packets;
uncommitted files;
any file not explicitly listed here.
```

## Authority and privacy rules

Qdrant remains derivative and owns no truth.

A later implementation packet must enforce:

```text
Markdown source path and SHA-256 are recorded for every chunk;
accepted results must re-resolve to the source file before being used;
missing source, changed source, wrong root, wrong path, or SHA mismatch rejects the candidate;
backend payloads are not exposed directly as authority;
retrieved text is never treated as instruction from the user or system;
results are filtered by project/corpus boundary before context-pack assembly;
private, secret, draft, rejected, superseded, archived, volatile, or wrong-corpus content cannot enter accepted results.
```

## Supersedence and freshness rules

For this first pilot, freshness is simple and strict:

```text
index generation id is required;
embedding space id is required, even if the first embedding remains deterministic or placeholder;
payload schema version is required;
source SHA-256 is required;
all accepted chunks must match the active generation;
stale generations must be rejected;
changed source files require rebuild before acceptance;
no mixed-generation context pack is valid.
```

Supersedence rule:

```text
If a later record supersedes an earlier record, the implementation packet must either exclude the superseded source or mark its chunks rejected by policy before context-pack assembly.
```

## Rebuild rules

A later implementation packet must prove that the pilot index is rebuildable from the allowed Markdown list.

Minimum rebuild evidence:

```text
source manifest with paths and SHA-256 values;
chunk manifest with stable chunk ids;
payload schema version;
index generation id;
embedding space id;
collection name or alias kept internal;
cleanup path for disposable local runtime;
repeat build produces stable logical ids for unchanged sources;
source change creates a new generation or fails closed.
```

## Gateway and context-pack expectations

A later implementation packet must keep the policy boundary in Kaizen code, not Qdrant.

Expected flow:

```text
allowed Markdown manifest
-> chunk/read-model derivation
-> derivative payloads and vectors
-> local disposable Qdrant collection
-> gateway query
-> canonical source re-resolution
-> policy filter
-> safe candidate projection
-> context-pack candidate manifest
```

Not allowed:

```text
agent direct Qdrant access;
raw scroll/list/dump tools;
exact point lookup exposed to agents;
collection names surfaced as user-facing authority;
context packs emitted directly by Qdrant;
retrieval result accepted without source re-resolution.
```

## Audit expectations for later implementation packet

The later real-corpus implementation packet must include tests for:

```text
allowed M16 source can be retrieved;
unlisted docs file cannot be retrieved;
wrong root cannot be indexed;
changed source SHA rejects stale candidate;
stale generation rejected;
missing source path rejected;
missing source SHA rejected;
private/draft/rejected/superseded flags rejected;
current-state and command-center excluded by default;
raw backend metadata hidden from safe result shape;
context-pack candidate manifest remains gateway-normalized;
cleanup leaves no disposable collection dependency behind.
```

## Stop conditions

Stop immediately if:

```text
any path outside the approved list is needed;
any vault file is needed;
any source contains private or secret material;
indexing would require platform or vault mutation not separately authorized;
Qdrant returns candidates that cannot be source-revalidated;
real corpus indexing begins before a separate implementation packet is accepted.
```

## Result

024J completes the docs-only real-corpus pilot boundary contract.

Next packet should be implementation planning for the first real-corpus pilot under this exact boundary.
