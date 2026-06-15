---
id: kz-result-01KV6J6Q8M2N7R5C3Y9P4T6BF
type: research-result
status: complete
project: kaizen-platform
created: 2026-06-15T22:00:00Z
updated: 2026-06-15T22:00:00Z
review_status: steward-reviewed
authority: proposed
summary: "Reconcile cross-family authority, identity, lifecycle, retention, privacy, transactions, and Milestone 11 readiness."
---

# Research Result 248 - Milestone 10 Authority and Lifecycle Reconciliation

## Reconciled family set

The four Milestone 9 candidates resolve into:

```text
1. execution attempt
2. verification run
3. return-bundle manifest
4. artifact provenance
5. governed-operation read model
6. connector invocation telemetry
```

The first candidate split into three families. No additional workload domain was introduced.

## Authority matrix

| Family or artifact | Canonical or operational owner | Authority class | May be rebuilt? | Human acceptance authority? |
|---|---|---|---|---|
| decisions, specs, audits, packets, owner records | Markdown / kaizen-docs | canonical narrative | no | yes |
| project command center, overview, current state | canonical vault Markdown | canonical project intelligence | no | no direct approval authority unless linked record says so |
| implementation-return evidence files | immutable files | authoritative bytes | no | no |
| generated output bytes | immutable files | authoritative bytes | no | no |
| governance JSONL | canonical vault file | authoritative event history | no | records accepted authority events but does not invent them |
| immutable preparation, plan, and approval evidence | staging files | authoritative operation evidence | no | approval evidence binds human action |
| Git commits and tracked-state facts | Git repository | authoritative repository state | no | no |
| execution attempt | automation operational family | authoritative structured execution state | no, though derived summaries may be rebuilt | no |
| verification run | audit operational family | authoritative structured machine evidence | no | no |
| return-bundle manifest | audit operational family | authoritative completeness metadata | no after freeze | no |
| artifact provenance | audit operational family | authoritative provenance metadata | no; projections may be rebuilt | no |
| governed-operation read model | governance operational family | derived index only | yes | no |
| connector invocation telemetry | audit operational family | authoritative boundary observation | no after event creation | no |

## One-owner rule

Each accepted operational family has one owner:

```text
execution attempt -> automation
verification run -> audit
return-bundle manifest -> audit
artifact provenance -> audit
governed-operation read model -> governance
connector invocation telemetry -> audit
```

The repeated use of `audit` does not merge the families. Their transaction boundaries and lifecycles remain distinct.

## Conceptual identity map

```text
project ID
  -> packet or governed task ID
      -> execution attempt ID
          -> verification run IDs
          -> produced artifact byte-object IDs
          -> return-bundle manifest ID

artifact logical ID
  -> artifact byte-object ID
      -> provenance assertion IDs
          -> producing attempt or verification run
          -> input or fixture identity
          -> repository commit
          -> canonical audit or owner-acceptance reference

governed operation ID
  -> preparation evidence
  -> immutable plan hash
  -> approval evidence
  -> canonical governance event
  -> Git binding
  -> derived read-model row or projection version

connector retry-group ID
  -> invocation attempt IDs
      -> boundary outcome events
      -> optional governed operation reference
      -> eventual group outcome
```

These are conceptual identities only. They do not authorize UUID formats, tables, columns, foreign keys, or indexes.

## Relationship rules

1. One execution attempt may have many verification runs.
2. One execution attempt may produce many artifact byte objects.
3. One attempt has zero or more return manifests over time, but only frozen manifests are audit evidence; a replacement manifest never erases an earlier rejected manifest.
4. One logical artifact may have many immutable byte objects across runs.
5. One byte object may be observed in multiple runs when output is deterministic; provenance assertions preserve each occurrence.
6. One governed operation has one immutable operation identity, while multiple connector invocations may attempt to inspect, approve, execute, or recover it.
7. A connector invocation outcome never changes governed-operation state by itself.
8. A read-model state must cite the authoritative operation evidence and projection version that produced it.
9. Human audit and acceptance records reference operational evidence but remain Markdown authority.

## Lifecycle matrix

| Family | Start | Terminal states | Retry/correction rule |
|---|---|---|---|
| execution attempt | reserved or started | succeeded, failed, cancelled, interrupted | retry creates new attempt linked to predecessor |
| verification run | started | passed, failed, timed-out, blocked, interrupted | rerun creates new run; classification correction supersedes |
| return manifest | draft inventory | frozen, audited-pass, audited-fail | new manifest version; never overwrite frozen failed manifest |
| artifact provenance | observed | verified, accepted-for-use, rejected, superseded-for-use, deleted-observation | correction is a new assertion |
| governance read model | projection created | no independent terminal business state | replay or rebuild from authority |
| connector invocation | attempted | boundary outcome recorded | new invocation in same retry group |

## Transaction boundaries

### Execution attempt

One bounded implementation or resumption attempt against one starting repository state and approved scope.

### Verification run

One exact verification command against one bound code/input state, producing one terminal result or blocked classification.

### Return manifest

One atomic freeze of the complete evidence inventory and its hashes for one attempt.

### Artifact provenance

One assertion binding one immutable byte object to its producer, inputs, code, command, and observation time.

### Governance read-model update

One deterministic projection update from verified authoritative evidence. Failure changes no authority.

### Connector invocation

One attempted tool call at one boundary. A retry is a new transaction.

## Recovery findings

- Execution state recovery must never convert an unknown attempt state into success.
- Verification timeout without returned output remains `timed-out` or `blocked`, not `failed test`.
- Frozen return manifests are never edited during recovery.
- Artifact provenance corrections preserve the original assertion.
- Governance read models recover by replay from immutable evidence and Git.
- Connector telemetry loss does not replay or authorize the underlying call.

## Privacy matrix

| Family | Allowed minimum | Prohibited by default |
|---|---|---|
| execution attempt | project, task, commit, status, timing, bounded error | source bytes, prompts, secrets, customer payloads |
| verification run | kind, normalized command ID, result, counts, duration, raw-evidence pointer | unrestricted stdout/stderr, environment secrets |
| return manifest | paths or scoped references, hashes, completeness findings | inlined evidence contents |
| artifact provenance | logical role, hashes, producer/input references | artifact bytes, private payload contents |
| governance read model | operation IDs, actors, hashes, states, bindings | candidate Markdown, private operation payloads |
| connector telemetry | tool contract, consequence class, boundary, outcome, latency, retry linkage | prompt bodies, arbitrary arguments, credentials, full exception payloads |

## Retention findings

### Permanent or authority-linked retention

- immutable return manifests and rejected return history;
- artifact provenance referenced by canonical audits or owner acceptance;
- authoritative governance JSONL and immutable operation evidence;
- execution and verification records needed to preserve accepted implementation lineage.

### Rebuildable retention

- governed-operation read model has no independent permanent-retention requirement.

### Owner decisions required before Milestone 11 design

1. exact retention classes for execution, verification, and unaccepted artifact-provenance metadata;
2. whether successful low-risk connector events are fully retained, sampled, or summarized;
3. retry-group expiry for connector telemetry;
4. project deletion semantics when canonical evidence must remain;
5. whether local path references are stored directly, normalized, or replaced by scoped storage identifiers.

These decisions block physical design but do not block Milestone 10 reconciliation acceptance.

## Real query matrix

| Family | Queries that justify structured storage |
|---|---|
| execution attempt | attempts by packet/commit/status; retry chains; missing verification; time to terminal result |
| verification run | runs by commit/kind/result; timeouts versus failures; required-gate completeness; duration trends |
| return manifest | complete/incomplete bundles; missing evidence; rejected manifests; attempt-to-audit lineage |
| artifact provenance | hash-to-producer lookup; deterministic equality; input/code/output lineage; acceptance status |
| governance read model | approved-unexecuted operations; stale plans; recovery-required operations; operation/event/Git lineage |
| connector telemetry | pre-MCP block rate; boundary classification; retries to success; contract-specific reliability |

These are observed operational needs, not speculative dashboard requests.

## Queries that should remain file-based

- full human audit narratives;
- exact source, output, report, or evidence bytes;
- one immutable plan or approval document;
- detailed incident postmortems;
- canonical project status and strategy.

## Candidate dispositions

| Candidate or split family | Disposition | Reason |
|---|---|---|
| execution attempt | accept-for-later-design | recurrent mutable-to-terminal operational state and real cross-attempt queries |
| verification run | accept-for-later-design | recurrent machine evidence with distinct timeout/block/failure semantics |
| return-bundle manifest | accept-for-later-design | repeated completeness audits and immutable failed-history requirement |
| artifact provenance | accept-for-later-design | repeated cross-run hash and lineage joins; bytes remain files |
| governance read model | accept-for-later-design | query need proven; strictly derived and rebuildable |
| connector telemetry | accept-for-later-design | material repeated connector friction; privacy and retention stop conditions apply |
| decision/spec/packet content in operational DB | reject | Markdown remains canonical authority |
| artifact bytes in operational DB | reject | file bytes remain authoritative |
| R1/R2 prose as structured operational records | watchlist | only two records; whole-document review remains primary |
| failure-injection family | watchlist | heterogeneous and pilot-specific; needs another pilot before generalization |
| Observatory and ingestion families | insufficient-evidence | Northstar did not exercise them |

## Milestone 11 readiness

### Ready for Milestone 11 conceptual and physical-design planning

Milestone 11 may be defined for the six accepted conceptual families, subject to owner acceptance of Decision 0019 and the retention/privacy decisions above.

### Not ready for implementation

Before implementation, Milestone 11 must separately define and approve:

- physical scope and minimum viable family set;
- exact retention and deletion classes;
- privacy review and payload minimization;
- schema and migration authority;
- backup, restore, and recovery requirements;
- typed service boundaries before MCP exposure;
- hammer-test strategy;
- local proving-ground plan;
- owner implementation gate.

### Excluded from Milestone 11 without new evidence

```text
Observatory observations
ingestion jobs and raw payloads
customer data
claims or source-summary databases
Qdrant
production MCP packaging
multi-user identity
```

## Steward recommendation

Accept all six reconciled families for later Milestone 11 design, with the governance read model permanently classified as derived unless a future explicit decision changes authority. Preserve Markdown, immutable file evidence, governance JSONL, and Git as their current authorities.
