---
id: kz-result-01KV6L6Q8M2N7R5C3Y9P4T6BF0
type: research-result
status: complete
project: kaizen-platform
created: 2026-06-16T00:00:00Z
updated: 2026-06-16T00:00:00Z
review_status: steward-reviewed
authority: proposed
summary: "Present owner decision options and steward recommendations required before Milestone 11 physical design."
---

# Research Result 255 - Milestone 11 Owner Decision Options

## Decision set

Phase 11A requires six owner decisions before physical design may begin.

## 1. Retention classes

### Option A - Single long retention

Keep all accepted operational records for the life of Kaizen.

Pros: simple and evidence-preserving.

Cons: unnecessary growth, weak privacy minimization, and no distinction between authority-linked lineage and transient diagnostics.

### Option B - Tiered retention

```text
Class A - permanent authority-linked lineage
frozen return manifests, rejected return history, and provenance referenced by accepted audits or owner decisions

Class B - project lifetime plus 12 months
execution attempts, verification runs, and unaccepted provenance linked to retained projects

Class C - 90 days
connector and transient diagnostic telemetry unless linked to an unresolved incident or accepted audit

Class D - rebuildable
read models may be deleted and rebuilt at any time
```

Pros: preserves consequential evidence while limiting operational clutter.

Cons: requires explicit deletion jobs and hold rules.

### Recommendation

```text
Select Option B.
```

Reason: it matches Kaizen's authority model and minimizes data that has no permanent evidentiary value.

## 2. Connector telemetry retention and sampling

### Option A - Retain every invocation

Pros: maximum reliability evidence.

Cons: high volume, privacy risk, and likely storage of low-value successful reads.

### Option B - Consequence-aware retention

Retain:

- all blocked, rejected, timed-out, failed, and interrupted calls;
- all approval, execute, recover, and canonical-mutation attempts;
- all calls linked to active incidents;
- a bounded sample or aggregate of successful low-risk read calls.

Never retain prompt bodies, arbitrary arguments, file contents, credentials, tokens, or private customer payloads.

### Recommendation

```text
Select Option B.
```

## 3. Retry-group expiry

### Options

```text
7 days
30 days
90 days
no automatic expiry
```

### Recommendation

```text
30 days after the most recent invocation,
unless linked to an unresolved governed operation or active incident.
```

This is long enough for investigation without creating immortal diagnostic groups.

## 4. Project deletion semantics

### Option A - Hard cascade delete

Rejected. It can destroy accepted evidence and provenance.

### Option B - Archive only

Simple but may retain unnecessary operational data forever.

### Option C - Governed deletion with retained lineage

- ordinary project operational records may be deleted after an approved deletion plan;
- accepted decisions, governance events, rejected and accepted implementation lineage, and authority-linked provenance remain;
- retained records use a project tombstone or durable project identity;
- deletion never rewrites immutable evidence or canonical history.

### Recommendation

```text
Select Option C.
```

## 5. Path-reference policy

### Option A - Store absolute Windows paths

Rejected as durable identity. Paths are machine-specific and expose local layout.

### Option B - Store only relative strings

Better, but ambiguous without a governed root identity.

### Option C - Scoped storage identifiers plus relative paths

Example conceptual form:

```text
storage_root_id: northstar-repository
relative_path: fixtures/amendment-v1/inventory.csv
```

Absolute paths may appear only in ephemeral local diagnostics.

### Recommendation

```text
Select Option C.
```

## 6. First implementation slice

### Option A - All six accepted families

Rejected as too broad for the first proving ground.

### Option B - Governance read model and connector telemetry first

Rejected. They improve observability but do not solve the most important evidence-chain pain.

### Option C - Core implementation-evidence chain

```text
execution attempt
verification run
return-bundle manifest
artifact provenance
```

Defer:

```text
governed-operation read model
connector invocation telemetry
```

### Recommendation

```text
Select Option C.
```

## Consolidated steward recommendation

```text
retention: tiered Option B
connector telemetry: consequence-aware Option B
retry-group expiry: 30 days with unresolved-operation hold
project deletion: governed deletion with retained lineage Option C
path references: scoped storage IDs plus relative paths Option C
first slice: four-family implementation-evidence chain Option C
```

## Owner gate

These recommendations do not become active until exact-hash owner acceptance of Decision 0020 and the Phase 11A package.
