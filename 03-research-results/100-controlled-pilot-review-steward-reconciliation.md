---
id: kz-result-01KTV7H2N4Q6S8W0Y2Z4ABCDEF
type: research-result
status: complete
project: kaizen-platform
created: 2026-06-10T14:32:00-04:00
updated: 2026-06-10T14:32:00-04:00
review_status: steward-reconciled
summary: "Steward reconciliation of Claude's review of the Northstar Stockroom controlled implementation-return pilot."
---

# Research Result 100 - Controlled Pilot Review Steward Reconciliation

## Source evidence

Claude reviewed the accepted planning-direction pilot specification and the live Kaizen repositories read-only.

Source report:

```text
Kaizen Controlled Pilot Review - Northstar Stockroom
Date: 2026-06-10
Status: research/audit evidence; not doctrine or authorization
```

## Verdict adopted

```text
REVISE NORTHSTAR STOCKROOM; DO NOT REPLACE IT
```

Northstar remains the correct first controlled pilot because it is deterministic, non-self-referential, small, and strong on implementation-return evidence. It requires stronger information separation and gate pressure before execution.

## Adopted findings

### Sealed oracle

The implementer must not see the accepted business-rule answers, frozen golden outputs, expected hashes, or failure-injection schedule.

The pilot must separate:

```text
implementer brief
sealed owner oracle
```

Both must be frozen by SHA-256 before implementation.

### Mandatory lane separation

- owner: oracle holder and final evaluator;
- GPT implementation lane: write, test, commit, return evidence;
- Claude audit lane: independent read/audit review;
- fresh-context agent: resumption proof.

The control is information access and independent evaluation, not theatrical job titles.

### Added injections

Add:

- silent golden-file edit attempt;
- dirty-repository governed-return attempt;
- wrong-but-green test that violates an accepted decision.

These test oracle integrity, live-scope discipline, read-only inspection, and whether the audit stage can block a green but incorrect implementation.

### Stronger amendment

Replace the purely additive low-value suppression request with a rule-modifying change involving stale pricing and inactive suppliers. This must update an accepted rule, impact analysis, tests, and versioned golden outputs while preserving baseline history.

### Exact resumption proof

Require two fresh-context checkpoints:

- baseline complete, before amendment;
- amendment approved, before implementation.

Include a missing-evidence trap. The agent must report the missing evidence rather than invent it.

### Workload ledger as primary new evidence

The workload-observation ledger is the pilot's main new output. Add recurrence counts and require concrete evidence pointers. A future Postgres candidate requires recurrence, demonstrated Markdown/file friction, and a real query or aggregation need.

The expected healthy outcome may be zero immediate Postgres candidates and a ranked watchlist.

## Modified findings

### Dirty-repository injection

Use this only after Milestone 8 fixes and proves the stale read-only loader/scope defect. In Milestone 9 it is field validation, not a debugging exercise.

### Golden regeneration

The fresh-context agent regenerates output from canonical records without seeing the expected hash. The owner/audit lane compares the result to the sealed oracle afterward.

## Deferred findings

A second controlled research/report pilot remains conditional. Milestone 9 closure or Milestone 10 analysis must decide whether front-half ingestion/provenance evidence is insufficient before any ingestion or Observatory schema work.

## Rejected additions

Do not add redundant hash-mismatch, rollback, identity, provider, database, UI, or deployment scenarios merely to increase coverage.

## Roadmap implications

- Milestone 9 remains after Milestone 8.
- Milestone 11 must be bounded to record families actually evidenced by the pilot and Milestone 10.
- Ingestion or Observatory schema work requires additional front-half evidence if Northstar does not produce it.

## Authority boundary

This reconciliation changes planning documents only. It does not authorize Milestone 7, 8, or 9 implementation, mock-project execution, oracle creation, repository creation, Postgres work, MCP production migration, or any deferred system.
