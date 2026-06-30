# Packet 024M — Real-Corpus Expansion Decision and Next-Slice Contract

Status: complete — docs-only decision
Date: 2026-06-30
Milestone: 16

## Purpose

Choose the next Milestone 16 slice after 024L passed.

024M is docs-only and does not authorize implementation.

## Current state

024K and 024L proved the bounded real-corpus pilot over the explicit 024J M16 docs list.

Proven:

```text
local disposable Qdrant path;
explicit docs allowlist;
source SHA-256 capture;
source re-resolution;
wrong-boundary rejection;
stale-generation rejection;
gateway-normalized context-pack candidate manifest;
M15/Markdown/validation collateral remains green.
```

Remaining gap:

```text
024K used deterministic placeholder vectors.
```

## Decision

Do not expand the corpus yet.

Do not index the vault yet.

Do not add agent-facing retrieval tools yet.

Next slice:

```text
024N — Retrieval Vector Baseline and Quality Contract
```

024N should define the first real retrieval-vector baseline and quality rules while keeping the same bounded 024J/024K corpus.

## Why

Kaizen should prove retrieval behavior on the smallest governed corpus before expanding to more docs or the vault.

Recommended sequence:

```text
same bounded corpus
-> vector baseline contract
-> implementation proof
-> quality/leakage audit
-> broader corpus decision
```

## 024N should define

```text
baseline selection criteria;
vector dimension contract;
space id format;
payload metadata fields;
rebuild behavior when the baseline changes;
quality query set;
expected-source assertions;
negative query assertions;
024K/024L leakage checks;
stop conditions.
```

## Non-authorization

024M does not authorize:

```text
platform mutation;
vault mutation;
staging mutation;
database mutation;
Qdrant writes;
vector implementation;
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

```text
024N — Retrieval Vector Baseline and Quality Contract
```
