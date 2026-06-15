---
id: kz-result-01KV6F7Q8M2N7R5C3Y9P1T6FA
type: research-result
status: complete
project: kaizen-platform
created: 2026-06-15T20:20:00Z
updated: 2026-06-15T20:20:00Z
review_status: steward-reviewed
authority: proposed
summary: "Reconcile Milestone 9 workload observations, recurrence, file friction, and later operational-data candidates."
---

# Research Result 241 - Milestone 9 Workload Reconciliation Report

## Evidence source

The append-only workload ledger is frozen at:

```text
C:\dev\kaizen\staging\_pilot\northstar\phase6-workload-observation-ledger.jsonl
SHA-256: b8cd5db3597f3db9381a6881b52f4a6d0bd5cb84358a6ce9ce73489bb9c5fe94
```

It contains eight observed record families with evidence pointers, producers, consumers, recurrence counts, lifecycle, immutability, retention, privacy, transaction boundaries, file friction, and query needs.

## Recurrence summary

| Record family | Count | Baseline + amendment | Concrete friction | Real aggregation need |
|---|---:|---|---|---|
| implementation return bundle | 3 | yes | yes | yes |
| test run | 5 | yes | yes | yes |
| artifact hash manifest | 6 | yes | yes | yes |
| governed operation | 7 | yes | yes | yes |
| fresh-context resumption report | 2 | yes | yes | limited |
| failure-injection evidence | 8 | mostly baseline/control | yes | yes |
| decision/spec/packet authority transition | 5 | yes | yes | yes |
| connector invocation and retry | 12 observed incidents | yes | yes | yes |

## Accepted nomination bar

A later Postgres candidate requires all three:

```text
1. recurrence_count >= 3, or appearance in both baseline and amendment slices
2. concrete Markdown/file friction
3. a real query or aggregation need that files cannot reasonably serve
```

Passing the bar nominates a record family for Milestone 10 reconciliation only. It does not authorize a schema.

## Nominated candidates for Milestone 10 reconciliation

### 1. Implementation execution and return records

Combine implementation-attempt metadata and test-run metadata into one candidate lifecycle.

Evidence:

- three implementation return bundles;
- at least five test runs;
- baseline failed, corrected, R1, and amendment evidence;
- manual joins across commit, changed paths, command, result, duration, and audit verdict.

Why files are strained:

- one return spans many files;
- missing evidence is discovered by directory inspection;
- comparing attempts requires manual aggregation;
- suite identity and timing vary by text format.

Required later queries:

- all attempts by packet or repository commit;
- tests before and after code change;
- failed versus accepted return completeness;
- duration and retry trends;
- exact evidence bundle for one implementation attempt.

Nomination result: **YES**.

### 2. Artifact and output provenance

Evidence:

- six distinct hash manifests or frozen hash records;
- baseline, repeat, R1, amendment, and regression runs;
- exact equality chains currently reconstructed manually.

Why files are strained:

- hashes live in both JSON and Markdown;
- producer command, commit, fixture version, and audit verdict are separate;
- reverse lookup from hash to provenance is cumbersome.

Required later queries:

- which run produced this hash;
- all artifacts from a commit and fixture version;
- equality and lineage across baseline, R1, amendment, and regression;
- which audit accepted a frozen artifact.

Nomination result: **YES**.

### 3. Governed-operation operational index or read model

Evidence:

- bootstrap plus multiple amendment operations, stale plans, approvals, execution events, and retries;
- canonical governance log and immutable staging evidence already exist.

Why files are strained:

- current state spans preparation evidence, operation evidence, Git binding, and append-only events;
- cross-operation status and stale-binding analysis require repeated tool calls.

Required later queries:

- operations by packet, destination, actor, result, and state;
- stale or retired plans;
- retry and recovery history;
- connector-blocked versus platform-rejected operations.

Nomination result: **YES, read-model only**. Canonical Markdown and append-only governance evidence remain authoritative. A future database must never replace or rewrite them.

### 4. Connector invocation and reliability telemetry

Evidence:

- at least twelve observed connector blocks or retries across the pilot;
- repeated need to distinguish connector pre-blocking from platform rejection;
- durable audits currently summarize only selected incidents.

Why files are strained:

- most raw incidents exist in chat/tool history;
- counts and classifications are reconstructed manually;
- there is no durable machine-readable stream of consequence class, tool, operation, rejection layer, and eventual outcome.

Required later queries:

- block rate by tool and consequence class;
- connector versus platform rejection;
- retry count and eventual success;
- contract shapes associated with fewer blocks.

Nomination result: **YES**.

## Ranked watchlist, not nominated now

### Fresh-context resumption reports

Two records exist and both slices are represented, but the need remains primarily whole-document audit. Structured extraction would help, yet a database would be premature.

Result: **WATCHLIST**.

### Failure-injection evidence

The recurrence count is high, but the records are intentionally heterogeneous and pilot-specific. A later evaluation-record family may be justified after another controlled pilot demonstrates reuse.

Result: **WATCHLIST**.

## Rejected database candidates

### Decision, specification, and packet authority documents

Although recurrence and query needs exist, raw Markdown is the accepted canonical authority. The friction is primarily graph navigation and indexing, not transactional mutation.

Result: **REJECT as a primary database authority**. Milestone 10 may consider derived indexes only.

### Canonical project notes

Command center, overview, and current-state are low-volume human-readable project intelligence. Their lifecycle is governed and versioned through Git and Kaizen operations.

Result: **REJECT**.

### Frozen implementation-return Markdown prose

The prose is audit evidence, not a high-frequency transactional record. Structured metadata may be extracted into a later execution record, but the frozen return itself remains a file artifact.

Result: **REJECT as database-resident content**.

### R1/R2 narrative reports

Only two records exist, and whole-report review is part of their purpose.

Result: **REJECT for now**.

### Golden and generated output bytes

Binary/text artifact bytes belong in immutable file storage with hashes, not relational columns. Only provenance metadata is a candidate.

Result: **REJECT bytes; nominate provenance metadata only**.

## Markdown and file-evidence friction findings

1. Return completeness requires inspecting many sibling files rather than one structured attempt record.
2. Cross-run hash equality requires manual joins across JSON and Markdown.
3. Test counts, commands, and durations use inconsistent text shapes.
4. Operation status requires joining staging evidence, Git state, and governance events.
5. Connector failures are under-recorded outside chat history.
6. Accepted-source hash lineage and supersession require folder-wide searches.
7. Failure-injection coverage requires a manually assembled matrix.

## Recommendation

Proceed to **direct Milestone 10 workload and system-of-record reconciliation**, using the four nominated candidates as evidence inputs. Do not begin a new real-project pilot merely to repeat already-proven implementation-return mechanics. A later controlled research/report pilot should be planned separately to provide front-half evidence for research ingestion, source summaries, claims, and Observatory workloads that Northstar did not exercise.

No physical schema, migration, role, or production service is authorized by this report.
