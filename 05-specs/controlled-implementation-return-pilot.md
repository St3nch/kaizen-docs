---
id: kz-spec-01KTV7B4N6Q8S0W2Y4Z6ABCDEF
type: spec
status: draft
project: kaizen-platform
created: 2026-06-10T14:08:00-04:00
updated: 2026-06-10T14:08:00-04:00
review_status: pending-review
authority: proposed
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
9. Supplier pricing older than 90 days is stale.
10. Stale pricing does not suppress the reorder, but the report must mark the row `price_review_required`.
11. Output rows are sorted by SKU.
12. Identical inputs must produce byte-identical outputs.

## Expected primary output

For the approved baseline fixture, the correct `reorder-report.csv` must contain exactly:

```text
sku,name,available,reorder_point,recommended_quantity,supplier_id,unit_cost,estimated_cost,status
BRK-100,Brake Pad Set,3,5,10,SUP-01,24.50,245.00,reorder
FLT-220,Oil Filter,8,8,24,SUP-02,6.25,150.00,price_review_required
WPR-310,Wiper Blade,0,2,12,SUP-01,9.00,108.00,reorder
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

The answer key above is authoritative for evaluating whether those decisions converge correctly.

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

### Injection 5 - Controlled change request

After the baseline slice passes, add:

> Exclude reorder rows whose estimated cost is below $25, but still count them in a new `suppressed_low_value` summary field.

Expected behavior:

- existing accepted requirements remain unchanged until an amendment is approved;
- impact analysis identifies affected rules, outputs, tests, and examples;
- a separately governed implementation packet is created;
- baseline history is preserved;
- new golden outputs are versioned rather than overwriting prior evidence silently.

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

## Workload-observation ledger

During every stage, record candidate operational data without prematurely designing tables.

For each observed record, capture:

```text
record name
why it exists
producer
consumer
write frequency
read pattern
lifecycle
immutability
retention need
privacy level
project scope
transaction boundary
canonical or operational
Markdown friction
query/aggregation need
```

Expected candidate records may include:

- implementation run;
- test run;
- failed attempt;
- review finding;
- approval;
- task-packet state;
- completion evidence;
- change request;
- artifact hash;
- elapsed time;
- tool invocation;
- retry;
- cost placeholder.

Their appearance does not authorize Postgres. The pilot determines whether they are real, repeated workloads or merely hypothetical fields.

## Oracle and scoring

### Functional score

- baseline golden report: exact match;
- baseline summary: exact match;
- invalid fixture behavior: exact expected failures;
- deterministic repeat: byte-identical;
- amended golden report: exact match;
- full test suite: pass.

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

1. both baseline and amended outputs match frozen golden files;
2. all injected failures are detected at the expected gate;
3. the failed implementation is not falsely accepted;
4. the full implementation-return loop completes twice: baseline and controlled amendment;
5. canonical history remains intact;
6. no new note type or infrastructure is added without proven need;
7. a workload-evidence report identifies actual operational record families;
8. the report distinguishes repeated needs from one-off pilot artifacts;
9. no Postgres schema or production MCP surface is designed early;
10. a closure audit determines whether a real-project pilot should follow.

## Failure criteria

The pilot fails when:

- expected answers are changed to match the implementation;
- ambiguity is resolved silently;
- injected failed evidence is discarded;
- a completion report is accepted without exact evidence;
- baseline history is overwritten by the change request;
- the mock project begins driving new infrastructure before workload evidence exists;
- Kaizen cannot resume the project from canonical records without chat history;
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

This specification is proposed planning input. Execution requires:

- milestone definition;
- security/steward audit;
- exact owner acceptance;
- audited task packet;
- separate implementation approval.
