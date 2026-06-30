# Packet 025B — Retrieval Expansion Options and Gate Contract

Status: complete — docs-only gate contract
Date: 2026-06-30
Milestone: post-M16 retrieval expansion gate

## Purpose

Choose the next retrieval expansion lane after Milestone 16 closure and 025A roadmap reconciliation.

025B is docs-only. It does not authorize implementation.

## Inputs

```text
03-research-results/427-packet-024r-milestone-16-closure-record.md
03-research-results/428-packet-024s-milestone-16-closure-handoff-correction.md
03-research-results/429-packet-025a-post-m16-roadmap-reconciliation-and-next-gate-selection.md
ROADMAP_V0.4.md
```

## Options considered

```text
larger docs corpus;
vault pilot corpus;
stronger retrieval/vector baseline;
provider-backed embedding baseline;
agent-facing retrieval contract;
production gateway packaging.
```

## Decision

The next retrieval expansion lane should be:

```text
larger docs corpus
```

The next packet should define a bounded expanded docs corpus and its evaluation contract before any implementation.

Recommended next packet:

```text
025C — Expanded Docs Corpus Boundary and Evaluation Contract
```

## Why this lane first

The larger docs corpus lane expands the proof surface while staying in the safest authority zone.

It keeps work inside the docs repository and avoids adding vault privacy risk, provider/model dependency risk, agent-tool risk, or production-gateway risk.

It also tests whether the M16 retrieval contracts still hold with more real project intelligence records, more headings, more overlap, and more stale or superseded-seeming evidence.

## Why other lanes are deferred

Vault pilot corpus is deferred because vault indexing introduces canonical living project-intelligence and privacy boundary risk.

Provider-backed embedding baseline is deferred because it adds model/provider dependency and reproducibility questions.

Agent-facing retrieval contract is deferred because agents should not consume retrieval tools until corpus expansion and policy behavior are stronger.

Production gateway packaging is deferred because M16 remains a local proof path, not production infrastructure.

Stronger retrieval/vector baseline is deferred until the larger docs corpus exposes concrete quality failures that justify changing the vector baseline.

## 025C expected scope

025C should define:

```text
expanded docs corpus inclusion rules;
explicit excluded paths;
source kinds and note types;
allowed file list or deterministic manifest rule;
privacy exclusions;
supersedence and freshness expectations;
quality query families;
negative checks;
rebuild behavior;
implementation stop conditions;
hammer-audit expectations.
```

## 025C should not authorize

025C should not authorize implementation unless separately accepted.

025C should not authorize:

```text
vault indexing;
staging indexing;
platform source indexing;
private-data indexing;
customer-data indexing;
provider-data indexing;
external embedding dependency;
agent-facing retrieval tools;
production retrieval gateway;
Hermes integration;
Tauri integration;
roadmap milestone renumbering.
```

## Non-authorization

025B does not authorize:

```text
platform mutation;
vault mutation;
staging mutation;
database mutation;
Qdrant writes;
retrieval implementation;
broader docs indexing;
vault indexing;
agent-facing tools;
production gateway;
Hermes implementation;
Tauri implementation.
```
