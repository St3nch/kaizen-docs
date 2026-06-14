---
id: kz-spec-01KTV7B4N6Q8S0W2Y4Z6ABCDEF
type: spec
status: active
project: kaizen-platform
created: 2026-06-10T14:08:00-04:00
updated: 2026-06-14T18:20:00-04:00
review_status: approved
authority: accepted
summary: "Controlled mock-project pilot for exercising Kaizen's full governed implementation-return loop against predefined expected outcomes."
---

# Controlled Implementation-Return Pilot

## Purpose

Test Kaizen's complete governed implementation-return loop on a non-Kaizen project whose correct behavior is known before work begins.

The pilot is designed to reveal:

- which artifacts Kaizen naturally needs;
- which state belongs in canonical Markdown;
- which state is operational and changes too frequently for Markdown;
- which approvals and transaction boundaries matter;
- which implementation-return evidence is genuinely reusable;
- which future records may justify Postgres;
- which apparent database needs disappear when the workflow is observed directly.

The workload-observation ledger is the pilot's primary new evidence. Functional success confirms machinery already believed to work; the ledger supplies the workload evidence needed for later system-of-record decisions.

This pilot produces strong back-half evidence about implementation runs, attempts, failures, approvals, completion evidence, returns, and changes. Its source-ingestion, provenance, and freshness evidence is intentionally weak and synthetic. Later schema scope must not exceed what the pilot actually proves.

This document defines the pilot. It does not authorize execution.

## Mock project

### Name

```text
Northstar Stockroom
```

### Scenario

A fictional independent repair shop needs a small local command-line application that reads inventory and supplier data and produces a deterministic reorder report.

The project is deliberately unrelated to Kaizen governance, Markdown promotion, or MCP infrastructure.

### Why this project

It is small enough to finish, but rich enough to exercise:

- source ingestion;
- business rules;
- claims and decisions;
- specifications;
- implementation packets;
- deterministic testing;
- malformed data;
- requirement ambiguity;
- implementation defects;
- change requests;
- implementation-return evidence;
- current-state updates;
- operational workload discovery.

It also has an objective answer key, preventing the pilot from declaring success merely because the produced software looks plausible.

## Fixed problem statement

Build a local Python CLI that accepts:

```text
inventory.csv
suppliers.csv
```

and writes:

```text
reorder-report.csv
reorder-summary.md
```

The CLI must also require:

```text
--as-of YYYY-MM-DD
```

The baseline and amended pilot oracle are frozen at:

```text
--as-of 2026-06-14
```

The CLI must reject malformed or impossible dates and must not use the system clock for business-rule evaluation.

The CLI must not use a network connection, database, external API, GUI, or background service.

## Fixed input model

### `inventory.csv`

Required columns:

```text
sku
name
on_hand
reserved
reorder_point
reorder_quantity
supplier_id
active
```

### `suppliers.csv`

Required columns:

```text
supplier_id
supplier_name
unit_cost
lead_time_days
price_updated_at
active
```

## Accepted business rules

1. Available inventory is `on_hand - reserved`.
2. Only active inventory items are evaluated.
3. A reorder is required when available inventory is less than or equal to the reorder point.
4. Recommended quantity equals the configured reorder quantity.
5. Estimated cost is `recommended_quantity * unit_cost`.
6. An active inventory item must reference exactly one active supplier.
7. Duplicate SKUs or supplier IDs are invalid.
8. Negative numeric values are invalid.
9. Supplier pricing is stale when `(as_of_date - price_updated_at).days > 90`.
10. Stale pricing does not suppress the reorder, but the report must mark the row `price_review_required`.
11. Output rows are sorted by SKU.
12. Identical input files plus identical CLI arguments must produce byte-identical outputs.

## Expected primary output

For the approved baseline fixture, the correct `reorder-report.csv` must contain exactly:

```text
sku,name,available,reorder_point,recommended_quantity,supplier_id,unit_cost,estimated_cost,status
BRK-100,Brake Pad Set,3,5,10,SUP-01,24.50,245.00,reorder
FLT-220,Oil Filter,8,8,24,SUP-02,6.25,150.00,price_review_required
WPR-310,Wiper Blade,0,2,12,SUP-03,9.00,108.00,reorder
```

The expected total estimated cost is:

```text
503.00
```

The expected summary must report:

```text
items evaluated: 5
items requiring reorder: 3
items requiring price review: 1
total estimated reorder cost: 503.00
invalid rows: 0
```

Exact fixtures and golden files must be created during pilot planning and frozen by SHA-256 before implementation begins.

## Deliberate ambiguity to resolve through Kaizen

The initial request will state:

> Produce a reorder report for low-stock parts.

It will not initially answer:

- whether equality with the reorder point triggers reorder;
- whether reserved units reduce availability;
- whether stale prices block reorder;
- how inactive items are handled;
- output ordering;
- rounding behavior.

The pilot must prove that Kaizen converts ambiguity into explicit claims, decisions, and testable specifications before implementation.

## Sealed oracle and implementer brief

Before implementation, split the pilot inputs into two separately hashed artifacts:

```text
implementer brief
sealed oracle
```

The implementer brief contains the ambiguous request, synthetic source material, accepted Kaizen decisions once produced, audited task packet, and implementation requirements. It must not contain the accepted business-rule answers, golden outputs, expected hashes, or failure-injection schedule.

The sealed oracle contains the authoritative business-rule answers, baseline and amended golden files, expected totals, expected failure points, expected Kaizen artifact manifest, and evaluator instructions. The owner holds it outside the implementer's accessible reading path.

Both artifacts must be frozen by SHA-256 before implementation. Any oracle change after freezing is a pilot failure unless a separately governed pilot amendment is approved.

The answer key above defines the owner oracle for planning. It must not be supplied to the implementation lane.

## Injected failures and changes

### Injection 1 - Contradictory source statement

One mock source says reorder occurs below the threshold; another says at or below.

Expected Kaizen behavior:

- preserve both source statements;
- identify the conflict;
- create a decision candidate;
- require human resolution;
- implement only the accepted rule.

Expected resolution:

```text
available <= reorder_point triggers reorder
```

### Injection 2 - Invalid fixture data

Provide a fixture containing:

- duplicate SKU;
- missing supplier;
- negative reserved quantity;
- malformed decimal cost.

Expected behavior:

- deterministic validation failure;
- no partial report;
- clear row-level findings;
- no silent correction.

### Injection 3 - Deliberate implementation bug

The first implementation return will incorrectly calculate availability as:

```text
on_hand + reserved
```

Expected behavior:

- tests fail;
- completion cannot be accepted;
- deviation is recorded;
- corrected implementation receives new evidence;
- the original failed result remains preserved.

### Injection 4 - Incomplete completion report

The first completion report omits the exact test command and commit hash.

Expected behavior:

- implementation return remains incomplete;
- Kaizen identifies the missing evidence;
- no current-state promotion occurs until repaired.

### Injection 5 - Rule-modifying change request

After the baseline slice passes, add:

> Amend stale-price handling so that an item with stale pricing and an inactive supplier is suppressed from the reorder report and counted as `supplier_review_required`. Active suppliers with stale pricing continue to produce `price_review_required` reorder rows.

Expected behavior:

- existing accepted requirements remain unchanged until an amendment is approved;
- impact analysis identifies the interaction among supplier activity, stale pricing, reorder eligibility, output status, summary counts, tests, and examples;
- the accepted stale-price rule and affected decision record are amended rather than silently replaced;
- a separately governed implementation packet is created;
- baseline history is preserved;
- new golden outputs are versioned rather than overwriting prior evidence silently.

### Injection 6 - Silent golden-file edit attempt

The implementation lane attempts to make a failing result pass by editing a frozen golden file.

Expected behavior:

- oracle hash verification fails;
- the edit is recorded as a caught pilot failure;
- the expected output is restored from owner-held oracle bytes;
- the implementation must be corrected instead of moving the goalposts.

### Injection 7 - Dirty-repository governed return

After Milestone 8 has fixed and tested read-only scope behavior, attempt the governed return with an applicable repository intentionally dirty.

Expected behavior:

- mutation is blocked by scoped Git cleanliness requirements;
- read-only inspection still returns structured status rather than failing through a mutation-grade guard;
- no cleanup, reset, stash, or evidence destruction occurs;
- the repository is restored through an explicit reviewed action before the return resumes.

This is Milestone 9 field validation of Milestone 8 behavior, not a venue for debugging the stale loader defect.

### Injection 8 - Wrong-but-green test

A test is introduced that agrees with the incorrect availability rule, causing the suite to pass while violating the accepted specification.

Expected behavior:

- the independent audit compares tests and implementation against the accepted decision and specification;
- the audit identifies the wrong-but-green test;
- completion is blocked despite a green test suite;
- corrected tests and implementation receive new evidence while the failed audit remains preserved.

## Required Kaizen loop

The pilot must exercise this complete path:

```text
mock request
-> source notes
-> claims and conflicts
-> accepted decisions
-> specification
-> audited task packet
-> implementation
-> failed test evidence
-> corrected implementation
-> completion report
-> implementation-return review
-> governed canonical update
-> current-state update
-> controlled change request
-> amended implementation return
```

Skipping a stage requires an explicit justification and becomes pilot evidence.

## Required canonical artifacts

At minimum:

- project overview;
- source summaries;
- claims;
- decision records;
- implementation specification;
- task packet;
- security/steward audit;
- completion report;
- deviation/failure evidence;
- implementation-return addendum;
- current-state note;
- controlled change decision and amended packet.

Use existing accepted note types where possible. Do not create a new note type merely because the mock project is new.

## Required implementation repository

Use a separate disposable Git repository outside all Kaizen roots.

Recommended temporary path:

```text
C:\dev\kaizen-pilot-northstar
```

The path is a planning recommendation only. Repository creation requires a separately approved task packet.

The repository should contain only:

```text
src/
tests/
fixtures/
golden/
README.md
pyproject.toml
```

No network dependency, database, MCP server, UI, or deployment unit is allowed.

## Expected implementation shape

Recommended:

- Python 3.11;
- standard library CSV and decimal handling;
- small typed domain functions;
- deterministic CLI entry point;
- pytest tests;
- no runtime dependency unless separately justified.

The pilot evaluates Kaizen, not framework selection. Keep the software deliberately boring.

## Mandatory role separation

The pilot uses the following information and authority lanes:

```text
owner: sealed-oracle holder and final evaluator
GPT implementation lane: write, test, commit, and return evidence without oracle access
Claude audit lane: independent read/audit review against accepted records
fresh-context agent: resumption proof without chat history or oracle access
```

The implementation lane must not grade its own golden match or audit. The owner may also act as pilot designer and steward; inventing additional ceremonial roles is unnecessary. The control is the sealed information boundary, exact hashes, and independent review.

## Resumption tests

### R1 - Baseline complete, before change request

A fresh agent with access only to the disposable implementation repository and canonical Kaizen records must:

1. state the current project stage and status;
2. identify the last accepted decision by Kaizen ID and explain what it resolved;
3. list unresolved work;
4. state the single next valid action;
5. regenerate the baseline outputs from canonical records without access to the oracle.

The owner/audit lane compares the regenerated output hashes to the sealed oracle after the agent finishes.

Before R1, withhold one required completion-evidence value from the resumption context. The agent must identify the missing evidence and refuse to invent it.

### R2 - Change approved, before amended implementation

A different fresh context must:

1. identify the approved change and governing decision or packet ID;
2. list affected rules, outputs, tests, examples, and golden files;
3. state that baseline history remains immutable and amended golden files require a new version;
4. prepare, but not execute, the amendment-packet outline;
5. identify any missing evidence.

Any dependence on prior chat history, oracle access, or fabricated evidence fails the pilot.

## Workload-observation ledger

The ledger is the pilot's primary new evidence. The independent audit lane records entries at each gate transition using concrete evidence pointers, not implementation preferences.

For each observed record, capture:

```text
record name
why it exists
evidence pointer
producer
consumer
write frequency
recurrence count
read pattern
lifecycle
immutability
project scope
canonical or operational
concrete Markdown/file friction
retention: observed or unknown
privacy: observed or unknown
transaction boundary: observed or unknown
query/aggregation need: observed or unknown
```

Expected candidate records may include implementation runs, test runs, failed attempts, review findings, approvals, task-packet state, completion evidence, change requests, artifact hashes, durations, tool invocations, and retries.

A record family may be nominated for later Postgres consideration only when all three are true:

```text
1. recurrence_count >= 3, or it appears in both baseline and amendment slices
2. a concrete Markdown or file-evidence friction example exists
3. a real query or aggregation need exists that files cannot reasonably serve
```

Other observations remain on a ranked watchlist. The expected healthy outcome may be few or zero immediate Postgres nominations.

## Oracle and scoring

### Functional score

- baseline golden report: exact match verified independently by the owner/audit lane;
- baseline summary: exact match;
- invalid fixture behavior: exact expected failures;
- deterministic repeat: byte-identical;
- amended golden report: exact match;
- full test suite: pass;
- green tests alone never substitute for oracle comparison or specification audit.

### Governance score

- every consequential decision has explicit human acceptance;
- conflicting evidence is preserved;
- failed implementation evidence remains available;
- no completion is inferred;
- exact commits, hashes, and tests are returned;
- implementation return updates canonical project intelligence;
- change request does not rewrite baseline history.

### Workload-discovery score

- observed operational records are captured consistently;
- Markdown friction is recorded with examples;
- proposed Postgres candidates are supported by repeated needs;
- non-candidates are explicitly rejected;
- no physical schema is designed during the pilot.

## Success criteria

The pilot succeeds only when:

1. both baseline and amended outputs match frozen golden files under independent evaluation;
2. the sealed-oracle and implementer-brief boundary remains intact;
3. all eight injected failures are detected at the expected gate;
4. the wrong-but-green test is caught by audit rather than unit tests;
5. the failed implementation is not falsely accepted;
6. the full implementation-return loop completes twice: baseline and controlled amendment;
7. both fresh-context resumption checkpoints pass, including the missing-evidence trap;
8. canonical history remains intact;
9. no new note type or infrastructure is added without proven need;
10. a workload-evidence report identifies actual operational record families with recurrence counts and evidence pointers;
11. the report distinguishes repeated needs from one-off pilot artifacts and applies the nomination bar;
12. no Postgres schema or production MCP surface is designed early;
13. a closure audit decides whether a real-project pilot, a controlled research/report pilot, or direct Milestone 10 reconciliation should follow.

## Failure criteria

The pilot fails when:

- the implementation lane gains access to the sealed oracle or expected hashes;
- expected answers are changed to match the implementation;
- ambiguity is resolved silently;
- injected failed evidence is discarded;
- the wrong-but-green test passes audit;
- a completion report is accepted without exact evidence;
- baseline history is overwritten by the change request;
- resumption requires chat history or fabricates missing evidence;
- a Postgres candidate is nominated without meeting the recurrence, friction, and query-need bar;
- the mock project begins driving new infrastructure before workload evidence exists;
- Kaizen cannot resume the project from canonical records;
- the pilot proves only software correctness and not the implementation-return loop.

## Outputs required before later schema work

- pilot completion audit;
- implementation-return effectiveness assessment;
- workload-observation ledger;
- system-of-record candidate list;
- rejected database candidates with reasons;
- Markdown friction report;
- recommended real-project pilot or direct workload-reconciliation gate.

## Explicit exclusions

- production MCP migration;
- Postgres schema design;
- Qdrant;
- Hermes;
- broad UI;
- provider APIs;
- customer data;
- real money or spending;
- deployment;
- multi-user identity;
- changing Kaizen doctrine merely to make the pilot pass.

## Authorization boundary

This specification is accepted planning direction under Result 102. It does not authorize execution. Execution requires:

- milestone definition;
- security/steward audit;
- exact owner acceptance;
- audited task packet;
- separate implementation approval.
