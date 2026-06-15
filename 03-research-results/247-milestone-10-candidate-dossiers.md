---
id: kz-result-01KV6J5Q8M2N7R5C3Y9P4T6BE
type: research-result
status: complete
project: kaizen-platform
created: 2026-06-15T21:40:00Z
updated: 2026-06-15T21:40:00Z
review_status: steward-reviewed
authority: proposed
summary: "Reconcile the four Milestone 10 operational-data candidates into evidence-backed record-family dispositions."
---

# Research Result 247 - Milestone 10 Candidate Dossiers

## Executive disposition

```text
Candidate 1 implementation execution and return records:
SPLIT
- execution attempt: accept-for-later-design
- verification run: accept-for-later-design
- return-bundle manifest: accept-for-later-design

Candidate 2 artifact and output provenance:
accept-for-later-design

Candidate 3 governed-operation operational read model:
accept-for-later-design as derived and non-authoritative

Candidate 4 connector invocation and reliability telemetry:
accept-for-later-design with privacy and retention stop conditions
```

No disposition authorizes physical schemas or implementation.

---

# Dossier 1 - Implementation Execution and Return Records

## Observed evidence

```text
C:\dev\kaizen\staging\_pilot\northstar\phase3-first-return\
C:\dev\kaizen\staging\_pilot\northstar\phase3-corrected-return\
C:\dev\kaizen\staging\_pilot\northstar\phase5-amendment-v1-return\
C:\dev\kaizen\staging\_pilot\northstar\r1-frozen-report.md
C:\dev\kaizen\staging\_pilot\northstar\r2-frozen-report.md
```

Observed recurrence:

```text
implementation return bundles: 3
test or verification runs: at least 5
baseline, failed, corrected, resumption, and amendment slices: present
```

## Finding: the candidate is not one record family

The observed evidence contains three different lifecycles and authorities:

1. **execution attempt** - mutable while work is running, then terminal;
2. **verification run** - one exact command against one exact repository state;
3. **return-bundle manifest** - frozen completeness and evidence index for one implementation attempt.

Combining them would blur operational state, test evidence, and immutable return packaging.

## Family 1A - Execution Attempt

### Purpose

Represent one bounded attempt to implement an approved packet or resumption task against one repository checkpoint.

### Proposed operational owner

```text
automation
```

The family describes execution state, not the human audit verdict.

### Producer and consumers

```text
producer:
approved execution service or bounded implementation lane

consumers:
verification runner
return-bundle producer
independent audit lane
steward status view
```

### Write trigger and frequency

Created when a bounded implementation or resumption attempt begins. Updated only through explicit lifecycle transitions. Milestone 9 produced multiple attempts across baseline correction, R1 completion, and amendment implementation.

### Conceptual lifecycle

```text
reserved
-> started
-> succeeded | failed | cancelled | interrupted
```

`accepted` is not an execution state. Acceptance belongs to human authority and audit records in Markdown.

### Transaction boundary

One attempt binds:

```text
approved packet or task reference
starting repository commit
allowed path scope
execution environment identity
one terminal execution outcome
```

Test runs and artifacts reference the attempt but remain separate records.

### Identity

Requires a stable attempt ID generated before execution starts. Retries receive new IDs and may reference a predecessor attempt.

### Immutability and correction

Lifecycle state may advance until terminal. Terminal records are append-corrected through superseding observations, never silently rewritten to change history.

### Privacy

May contain project ID, local repository identifier, commit hashes, actor or agent class, timing, and bounded failure codes. It must not contain source bytes, prompts, secrets, customer payloads, or arbitrary command output.

### Retention

Retain at least as long as the corresponding immutable return bundle and audit evidence. Exact global retention class remains a Milestone 11 owner decision.

### Queries justifying structured storage

- attempts by packet, project, commit, status, or predecessor;
- time from start to terminal outcome;
- failed versus successful attempts;
- attempts lacking required verification or return manifests;
- retry count and eventual accepted implementation lineage.

### Queries files serve adequately

- full implementation narrative;
- deviations and discoveries;
- human explanation of why an attempt failed.

### Disposition

```text
accept-for-later-design
```

---

## Family 1B - Verification Run

### Purpose

Represent one exact test, compile, lint, validation, hash, or deterministic-output command executed against one bound repository and input state.

### Proposed operational owner

```text
audit
```

Verification results are machine evidence. Human audit verdicts remain Markdown authority.

### Producer and consumers

```text
producer:
bounded verification runner

consumers:
implementation lane
return-bundle manifest
independent audit lane
closure assessment
```

### Write trigger and frequency

Created per exact verification command. Multiple runs may exist for one execution attempt.

### Conceptual lifecycle

```text
started
-> passed | failed | timed-out | blocked | interrupted
```

A connector timeout is distinct from a test failure when no test result was returned.

### Transaction boundary

One run binds:

```text
verification kind
normalized command identity
repository commit and tracked-state assertion
input or fixture identity
start and finish time
return code or blocked classification
summary counts
raw evidence pointer
```

### Identity

Requires a stable verification-run ID. A repeated command is a new run, not an overwrite.

### Immutability and correction

Terminal result is immutable. Parsing or classification corrections create a superseding record linked to the original raw evidence.

### Privacy

Store normalized command identity and bounded summaries. Do not store environment secrets, full stdout containing private data, arbitrary paths beyond approved identifiers, or customer payloads.

### Retention

Retain terminal metadata with the associated attempt and return bundle. Raw output remains in frozen files under its own retention policy.

### Queries justifying structured storage

- all verification runs for an attempt or commit;
- pass/fail/timeout/block distribution by verification kind;
- latest successful baseline regression for a commit;
- whether a return bundle omitted a required gate;
- duration and retry trends.

### Queries files serve adequately

- exact human-readable test transcript;
- large raw output;
- detailed failure narrative.

### Disposition

```text
accept-for-later-design
```

---

## Family 1C - Return-Bundle Manifest

### Purpose

Represent the frozen inventory and completeness state of one implementation-return evidence bundle without moving its files into a database.

### Proposed operational owner

```text
audit
```

The manifest owns structured completeness metadata. The evidence files remain authoritative for their bytes.

### Producer and consumers

```text
producer:
return-freeze process

consumers:
independent audit lane
steward
owner closure review
recovery and resumption lanes
```

### Write trigger and frequency

Created once when an attempt's return evidence is frozen. A rejected attempt keeps its manifest permanently.

### Conceptual lifecycle

```text
draft-inventory
-> frozen
-> audited-pass | audited-fail
```

Owner acceptance is referenced, not owned by this family.

### Transaction boundary

One manifest binds one execution attempt to:

```text
required evidence contract version
exact evidence-file inventory
per-file hashes
missing or extra evidence findings
freeze timestamp
repository commit
verification-run references
audit-result reference
```

### Identity

Requires a stable manifest ID and an immutable bundle identity derived from the complete frozen inventory or an equivalent deterministic binding.

### Immutability and correction

A frozen manifest is immutable. Additional evidence creates a new manifest version; it does not rewrite the rejected or incomplete original.

### Privacy

May contain local evidence pointers, hashes, commit IDs, and bounded classifications. It must not inline private evidence contents.

### Retention

Permanent for governed implementation attempts unless a later accepted retention policy establishes a longer or legally required class. Rejected evidence must not be deleted merely because a corrected attempt later passes.

### Queries justifying structured storage

- which attempts have complete return bundles;
- all rejected or incomplete bundles;
- missing evidence by contract version;
- evidence file provenance by attempt;
- bundle-to-audit and bundle-to-owner-acceptance lineage.

### Queries files serve adequately

- full return narrative;
- exact evidence bytes;
- diff and test transcripts.

### Disposition

```text
accept-for-later-design
```

---

# Dossier 2 - Artifact and Output Provenance

## Observed evidence

Milestone 9 produced at least six hash manifests or frozen hash records across:

```text
baseline run 1
baseline run 2
R1 regenerated outputs
amendment run 1
amendment run 2
baseline regression after amendment
```

Provenance currently requires manual joins among command text, repository commit, fixtures, output paths, hash manifests, audit records, and owner decisions.

## Purpose

Represent structured provenance assertions about immutable artifact bytes while leaving the artifact bytes in file storage.

## Proposed operational owner

```text
audit
```

Artifact provenance is evidentiary metadata. It does not own project narrative or the bytes themselves.

## Conceptual components

```text
artifact identity:
logical role such as reorder-report or frozen R1 report

artifact byte object:
one immutable file instance at one content hash

provenance assertion:
claim that one byte object was produced by one run from bound inputs and code

hash assertion:
algorithm, digest, file length, and verification result

acceptance linkage:
reference to canonical Markdown audit or owner acceptance
```

These components may later remain one family or become a tightly bounded set. Milestone 10 does not decide physical decomposition.

## Producer and consumers

```text
producer:
verification or freeze process

consumers:
audit lane
resumption lane
return-bundle manifest
steward provenance queries
```

## Write trigger and frequency

Created whenever an artifact is frozen, generated for deterministic comparison, or independently re-verified.

## Lifecycle

```text
observed
-> hash-verified
-> provenance-bound
-> accepted | rejected | superseded-for-use
```

The byte object never changes. A later artifact is a new byte object.

## Transaction boundary

One provenance assertion binds:

```text
artifact byte hash
artifact role
producing execution or verification run
repository commit
input or fixture identities
command identity
output path or storage reference
verification timestamp
```

## Identity

Content hash is necessary but not sufficient as the logical artifact ID. Identical bytes may legitimately appear in multiple runs. The system therefore needs both:

```text
stable logical artifact role or artifact ID
immutable byte-object identity
```

## Immutability and correction

Byte hash assertions are immutable observations. Incorrect provenance is superseded with a correction record referencing the original assertion.

## Privacy

Provenance metadata may reveal filenames, project identifiers, hashes, and local storage references. It must not inline customer data or artifact contents. Sensitive storage references may require redaction or scoped identifiers.

## Retention

Retain provenance as long as the artifact or any canonical acceptance references it. Artifact deletion must not erase the historical fact that it existed; deletion state becomes a separate governed observation.

## Queries justifying structured storage

- which run produced a given hash;
- all byte objects for a logical artifact role;
- whether two runs produced identical bytes;
- artifacts derived from a commit or fixture version;
- artifacts accepted, rejected, missing, superseded, or deleted;
- complete chain from input to output to audit verdict.

## Queries files serve adequately

- retrieving artifact bytes;
- inspecting a report or evidence file;
- preserving raw generated output.

## Canonical boundary

```text
artifact bytes:
file authority

provenance metadata:
future operational authority if later implemented

human meaning and acceptance:
canonical Markdown authority
```

## Disposition

```text
accept-for-later-design
```

---

# Dossier 3 - Governed-Operation Operational Read Model

## Observed evidence

Milestone 9 used:

- atomic bootstrap operations;
- amendment preparations, plans, approvals, executions, and events;
- stale plans after sequential vault commits;
- operation status inspection;
- dirty-repository blocking;
- connector retries;
- immutable staging evidence;
- append-only canonical governance JSONL;
- Git-bound live state.

Cross-operation status required repeated joins across evidence directories, governance events, and repository state.

## Purpose

Provide a queryable, derived operational view of governed operations without changing the authority of immutable operation evidence, canonical governance JSONL, or Git.

## Proposed operational owner

```text
governance
```

## Authority decision for this milestone

```text
canonical governance JSONL remains authoritative
immutable preparation and operation evidence remains authoritative
Git remains authoritative for repository commit and working-tree facts
the proposed read model is derived, disposable, and rebuildable
```

Milestone 10 does not move governance authority into Postgres.

## Producer and consumers

```text
producer:
deterministic projector over authoritative operation evidence, governance events, and Git bindings

consumers:
operator status views
recovery tools
audit queries
stale-plan and incomplete-operation detection
```

## Write trigger and frequency

Projection updates after authoritative evidence is created or repository state changes. The projector may replay all evidence or incrementally apply verified events.

## Conceptual lifecycle

The read model has no independent business lifecycle. It reflects operation states such as:

```text
prepared
planned
approved
committed
recovery-required
recovered
stale
retired
blocked-by-live-binding
```

Every displayed state must include its authoritative evidence basis and projection timestamp.

## Transaction boundary

One projection update consumes one verified authoritative evidence change and produces a consistent derived view. It must not create approval, execution, or canonical mutation authority.

## Identity

Primary conceptual identity is the immutable operation ID. Packet ID, destination, plan hash, event ID, and repository binding are references or alternate lookup keys.

## Immutability and correction

The read model is mutable and rebuildable. Corrections occur by fixing projection logic or replaying authoritative evidence, never by rewriting canonical governance history.

## Privacy

May contain operation IDs, packet IDs, actor IDs, destinations, hashes, timestamps, status codes, and repository bindings. It must exclude candidate contents, private payloads, secrets, and unrelated file data.

## Retention

The read model may be recreated and need not have independent permanent retention. Authoritative source evidence follows its own permanent governance retention.

## Queries justifying structured storage

- all operations by packet, destination, actor, or status;
- approved but unexecuted operations;
- stale plans and their invalidating repository commit;
- recovery-required operations;
- retries and terminal events;
- operation-to-event and operation-to-Git lineage.

## Queries files serve adequately

- inspecting one operation's immutable evidence;
- reviewing one exact plan or approval;
- proving canonical event bytes.

## Failure and recovery

Projection failure must not block inspection of authoritative evidence. A corrupt or missing projection is repaired by replay. If projection output conflicts with authoritative evidence, authoritative evidence wins and the discrepancy becomes an audit event.

## Disposition

```text
accept-for-later-design
```

with mandatory classification:

```text
derived
rebuildable
non-authoritative
```

---

# Dossier 4 - Connector Invocation and Reliability Telemetry

## Observed evidence

Milestone 9 recorded at least twelve connector blocks or retries. Multiple calls were blocked before Kaizen or Go8 received them, then later succeeded unchanged after schema refresh or retry. Current evidence is scattered across chat history and selected audit prose.

## Purpose

Record the minimum durable facts needed to distinguish connector-layer blocking from MCP adapter rejection, platform rejection, tool success, retry linkage, and eventual outcome.

## Proposed operational owner

```text
audit
```

This is machine and operational audit telemetry, not workflow authority.

## Producer and consumers

```text
producer:
connector boundary, MCP adapter, or trusted local telemetry bridge

consumers:
tooling maintainers
reliability audit
operator diagnostics
contract-design review
```

## Minimum conceptual event

```text
invocation ID
correlation or retry-group ID
tool contract identifier and version
consequence class
project or operation reference when present
attempt timestamp
boundary reached
outcome classification
bounded error code
latency when observable
retry predecessor or successor reference
eventual group outcome
```

## Required boundary classification

```text
blocked-before-mcp
adapter-rejected
platform-rejected
platform-succeeded
transport-failed
timed-out-without-result
result-returned
```

A test or mutation failure must not be inferred when the call never reached the responsible system.

## Write trigger and frequency

One event per connector invocation attempt. Retry attempts receive new invocation IDs within one retry group.

## Lifecycle

Each invocation is terminal after one outcome. A retry group may remain open until success, explicit abandonment, or policy-defined expiry.

## Transaction boundary

One telemetry event describes one attempted call at one boundary. It never mutates the underlying governed operation or substitutes for platform evidence.

## Identity

Requires a stable invocation ID and retry-group or correlation ID. Operation IDs and packet IDs are optional references, not substitutes for invocation identity.

## Immutability and correction

Events are append-only observations. Misclassification is corrected by a superseding classification event; raw event identity remains preserved.

## Privacy and minimization

Allowed:

- tool identifier and contract version;
- consequence class;
- bounded operation or project references;
- boundary and outcome code;
- timestamps and latency;
- retry linkage.

Prohibited by default:

- prompt or conversation bodies;
- arbitrary tool arguments;
- candidate Markdown;
- file contents;
- secrets, credentials, tokens, or private customer data;
- full exception dumps containing payloads.

Argument fingerprints may be considered later only if deterministic, non-reversible, privacy-reviewed, and necessary for grouping contract shapes.

## Retention

Exact duration remains unresolved. Before Milestone 11 design, the owner must choose a retention class and decide whether successful low-risk events are sampled, summarized, or retained fully. Until then, no production telemetry implementation is authorized.

## Queries justifying structured storage

- block rate by tool contract and consequence class;
- boundary distribution: connector, adapter, platform, or transport;
- retries before eventual success;
- contracts associated with repeated pre-blocking;
- median latency and timeout rate;
- incidents lacking an eventual outcome.

## Queries files serve adequately

- detailed narrative of one severe incident;
- exact chat context when intentionally preserved;
- human root-cause analysis.

## Failure and recovery

Telemetry failure must never authorize, deny, or replay a governed mutation. Missing telemetry creates observability debt, not operation authority. Duplicate events must be detectable through invocation identity.

## Disposition

```text
accept-for-later-design
```

with stop conditions before Milestone 11 design:

```text
owner retention decision required
sampling policy decision required
privacy review of any argument fingerprint required
```

---

# Cross-Dossier Contradictions and Resolutions

## Execution versus audit ownership

Execution attempts belong to `automation`; verification runs and return-bundle manifests belong to `audit`. This avoids making the executor authoritative for its own evidence acceptance.

## Governance events versus read model

Governance JSONL and immutable operation evidence remain authoritative. The read model is useful precisely because it is derived and rebuildable. It must not silently become the event authority.

## Artifact bytes versus provenance

Bytes remain files. Structured provenance may own operational metadata about those bytes but cannot replace them.

## Connector telemetry versus platform evidence

Connector telemetry reports whether and how a call crossed boundaries. It does not determine whether an operation is approved, committed, or valid; platform and governance evidence do.

## Decision and audit narratives

All human decisions, audit verdicts, acceptance records, and explanatory narratives remain canonical Markdown.
