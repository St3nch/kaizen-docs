---
id: kz-dec-01KV6J7Q8M2N7R5C3Y9P4T6BG
type: decision
status: proposed
project: kaizen-platform
created: 2026-06-15T22:10:00Z
updated: 2026-06-15T22:10:00Z
review_status: pending
authority: proposed
summary: "Propose the accepted operational record-family boundaries resulting from Milestone 10 reconciliation."
---

# Decision 0019 - Milestone 10 System-of-Record Reconciliation

## Context

Milestone 9 proved Kaizen's implementation-return loop and produced repeated operational evidence across implementation attempts, test runs, frozen return bundles, artifact hashes, governed operations, and connector retries.

Results 247 and 248 reconcile those observed workloads against Decisions 0004 and 0009. The objective is to establish conceptual authority and lifecycle boundaries before any physical database design.

## Proposed decision

### Accepted for later Milestone 11 design

The following conceptual operational record families are accepted for later design only:

```text
execution attempt
verification run
return-bundle manifest
artifact provenance
governed-operation read model
connector invocation telemetry
```

This acceptance does not authorize schemas, SQL, migrations, services, APIs, MCP tools, deployment, or implementation.

### Operational ownership

```text
execution attempt:
automation

verification run:
audit

return-bundle manifest:
audit

artifact provenance:
audit

governed-operation read model:
governance

connector invocation telemetry:
audit
```

Each family has exactly one operational owner.

### Canonical and file authority preserved

```text
Markdown / kaizen-docs:
decisions, specifications, audits, human verdicts, task packets, and owner records

canonical vault Markdown:
project command centers, overviews, and current-state notes

immutable files:
implementation-return evidence, frozen reports, generated artifact bytes, and hash evidence

canonical governance JSONL:
authoritative governance event history

immutable staging evidence:
authoritative preparation, plan, approval, and operation evidence

Git:
authoritative repository commit and tracked-state facts
```

No accepted operational family becomes a second canonical home for these authorities.

### Execution and return split

The prior combined candidate `implementation execution and return records` is split into:

1. execution attempt;
2. verification run;
3. return-bundle manifest.

Execution state belongs to `automation`. Machine verification evidence and frozen return completeness belong to `audit`. Human audit and acceptance remain Markdown authority.

### Artifact boundary

Artifact bytes remain immutable files. Artifact provenance may later own structured metadata binding byte hashes to producing attempts, verification runs, inputs, repository commits, and canonical audit or acceptance references.

The operational layer must not store artifact bytes merely for convenience.

### Governed-operation read-model boundary

The governed-operation read model is:

```text
derived
rebuildable
non-authoritative
```

It may project status from immutable operation evidence, canonical governance JSONL, and Git bindings. It may not create approval, execution, recovery, or canonical mutation authority.

Canonical governance JSONL remains authoritative unless a later explicit owner-accepted decision changes that boundary.

### Connector telemetry boundary

Connector telemetry may later record only the minimum facts needed to distinguish:

```text
blocked before MCP
adapter rejected
platform rejected
platform succeeded
transport failure
timeout without result
retry linkage and eventual outcome
```

It must exclude prompt bodies, arbitrary tool arguments, candidate contents, secrets, credentials, private customer content, and full payload-bearing exception dumps by default.

### Stable identity and retry rules

- each execution retry receives a new attempt ID linked to its predecessor;
- each verification rerun receives a new verification-run ID;
- frozen return manifests are immutable and versioned rather than overwritten;
- artifact byte identity and logical artifact identity remain distinct;
- each governed operation retains one immutable operation ID;
- each connector retry receives a new invocation ID within one retry group.

### Recovery and correction rules

- unknown execution state may not be rewritten as success;
- timeout without test output is not a test failure;
- frozen failed manifests remain preserved;
- provenance corrections supersede prior assertions;
- governance read models recover by replay from authority;
- telemetry loss never authorizes or replays an underlying mutation.

## Rejected and deferred boundaries

Rejected as primary operational database content:

```text
canonical Markdown decisions and specifications
canonical project notes
human audit narratives
artifact and report bytes
owner acceptance prose
```

Watchlist:

```text
fresh-context resumption report metadata
failure-injection record family
```

Insufficient evidence:

```text
Observatory observations
ingestion jobs and raw payloads
research source summaries and claims at scale
customer data operations
```

## Owner decisions required before physical design

Milestone 11 physical-design planning must not close until the owner decides:

1. retention classes for execution, verification, return-manifest, and unaccepted provenance metadata;
2. connector telemetry retention and successful-event sampling posture;
3. retry-group expiry;
4. project-deletion semantics where canonical evidence must remain;
5. direct local paths versus scoped storage identifiers;
6. the smallest first implementation slice among the accepted families.

## Consequences

- Milestone 11 has an evidence-bounded conceptual scope.
- Operational execution state is separated from audit evidence and human acceptance.
- Artifact provenance becomes queryable without moving artifact bytes.
- Governance status can later be indexed without replacing governance authority.
- Connector friction can be measured without storing private prompts or payloads.
- Observatory, ingestion, Qdrant, customer data, and production MCP work remain outside the evidenced scope.

## Explicit non-authorization

This decision does not authorize:

- a Postgres database or schema;
- tables, columns, keys, indexes, constraints, or SQL;
- migration tooling;
- database roles or credentials;
- services, APIs, MCP tools, queues, or workers;
- deployment or production operations;
- Milestone 11 implementation.

## Acceptance condition

This decision becomes active only after exact-hash owner acceptance following the Milestone 10 reconciliation audit.
