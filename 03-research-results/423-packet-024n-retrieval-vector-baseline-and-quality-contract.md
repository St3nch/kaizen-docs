# Packet 024N — Retrieval Vector Baseline and Quality Contract

Status: complete — docs-only contract
Date: 2026-06-30
Milestone: 16

## Purpose

Define the next governed Milestone 16 slice after 024M.

024N defines the retrieval-vector baseline and quality contract for the next implementation packet. It does not authorize implementation.

## Current state

M16 has proven:

```text
synthetic retrieval policy path;
local disposable Qdrant backend;
explicit real Markdown corpus allowlist;
source SHA-256 capture;
source re-resolution;
stale generation rejection;
wrong-boundary rejection;
gateway-normalized context-pack candidate manifests;
bounded real-corpus hammer audit.
```

Current limitation:

```text
024K used deterministic placeholder vectors.
```

## Decision

The next implementation slice should keep the same 024J/024K bounded corpus and replace placeholder vectors with a governed retrieval-vector baseline.

Do not expand corpus scope in the same packet.

Do not index vault content in the same packet.

Do not add agent-facing tools in the same packet.

## Baseline requirements

The first retrieval-vector baseline must define:

```text
vector provider label;
model or algorithm label;
model or algorithm version label;
vector dimension;
embedding space id;
payload schema version;
index generation id;
chunker version;
source hash binding;
rebuild rule when any baseline field changes.
```

The baseline must be deterministic enough for local audit.

## Allowed implementation boundary for next packet

A later implementation packet may modify platform code and tests only.

Allowed corpus remains:

```text
same explicit 024J M16 docs list only.
```

The implementation must not read from:

```text
vault;
staging;
platform source as corpus;
chatgpt_mcp;
kaizen_mcp;
Northstar pilot repo;
private data;
customer data;
provider data;
unlisted docs files.
```

## Quality query contract

The next implementation packet must define a small fixed query set before testing.

Minimum query families:

```text
Qdrant gateway and policy boundary;
source hash and source re-resolution;
corpus allowlist and real-corpus pilot;
stale generation or rebuild behavior;
context-pack candidate manifest;
semantic retrieval quality limitation or evidence.
```

Each query must declare:

```text
query id;
query text;
expected source family;
minimum accepted candidate count;
negative expectations;
reason the query exists.
```

## Negative checks

The later implementation proof must show that retrieval quality does not bypass governance.

Required negative checks:

```text
unlisted docs file is not accepted;
wrong corpus boundary is rejected;
stale generation is rejected;
stale vector space is rejected;
missing source hash is rejected;
changed source hash is rejected;
vault path is rejected;
current-state and command-center stay excluded unless separately authorized;
backend payload internals are not emitted as authority;
context-pack manifest remains human-review-required.
```

## Acceptance expectations

A later implementation proof should pass if:

```text
all baseline metadata is present;
all vectors match the baseline dimension;
accepted candidates source-resolve;
expected query families retrieve relevant M16 records;
negative checks fail closed;
existing 024K/024L governance checks still pass;
M15/Markdown/validation collateral remains green;
disposable Qdrant cleanup is proven.
```

## Stop conditions

Stop before implementation completion if:

```text
the baseline cannot be made reproducible enough for audit;
required corpus scope would expand beyond 024J;
model or vector behavior cannot be tested locally;
retrieval requires direct Qdrant authority;
accepted candidates cannot be source-revalidated;
quality improvements weaken leakage checks.
```

## Non-authorization

024N does not authorize:

```text
platform mutation;
vault mutation;
staging mutation;
database mutation;
Qdrant writes;
retrieval-vector implementation;
broader docs indexing;
vault indexing;
agent-facing tools;
production gateway;
Hermes integration;
Tauri implementation;
private-data indexing;
commercial or production use.
```

## Next packet

Recommended next packet:

```text
024O — Retrieval Vector Baseline Implementation
```
