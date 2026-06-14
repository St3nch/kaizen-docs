---
id: kz-result-01KV9NORTHSTARFIXTUREDEFECT
type: research-result
status: complete
project: kaizen-platform
created: 2026-06-14T17:55:00Z
updated: 2026-06-14T17:55:00Z
review_status: steward-reviewed
authority: proposed
summary: "Reconcile a supplier-cost contradiction found before Packet 014A Phase 1 artifact creation."
---

# Research Result 197 - Northstar Fixture Contract Preflight Failure and Correction Proposal

## Outcome

```text
Packet 014A Phase 1: stopped before artifact creation
owner-private root created: no
disposable repository created: no
staging evidence root created: no
oracle or golden files created: no
fixtures created: no
Git repository initialized: no
```

## Defect

The accepted Northstar input model defines `unit_cost` at supplier level:

```text
suppliers.csv:
supplier_id
supplier_name
unit_cost
lead_time_days
price_updated_at
active
```

The accepted rules also require supplier IDs to be unique.

The accepted baseline output assigns two different unit costs to the same supplier ID:

```text
BRK-100 -> SUP-01 -> 24.50
WPR-310 -> SUP-01 -> 9.00
```

A valid `suppliers.csv` cannot represent both values while keeping `supplier_id` unique. Therefore no deterministic baseline fixture can produce the accepted golden output under the accepted schema and rules.

## Root cause

The pilot specification's expected output reused `SUP-01` for two SKUs with different prices. The planning and readiness audits checked workflow boundaries but did not cross-check output rows against the supplier-key cardinality and price-location contract.

The Phase 1 preflight correctly stopped before freezing a contradictory oracle.

## Smallest correction

Change only the Wiper Blade supplier ID in the expected baseline output:

```diff
-WPR-310,Wiper Blade,0,2,12,SUP-01,9.00,108.00,reorder
+WPR-310,Wiper Blade,0,2,12,SUP-03,9.00,108.00,reorder
```

Then define `SUP-03` once in `suppliers.csv` with unit cost `9.00`.

This preserves:

- all input columns;
- supplier-ID uniqueness;
- all twelve business rules;
- all expected quantities and statuses;
- total estimated cost `503.00`;
- summary counts;
- deliberate ambiguity;
- every injection, role, resumption, and workload requirement.

No schema change, new note type, implementation behavior, or infrastructure is required.

## Required document reconciliation

After exact owner acceptance:

1. amend `05-specs/controlled-implementation-return-pilot.md` with the one-row supplier correction;
2. update its `updated` timestamp only as required by accepted conventions;
3. revise Packet 014A's governing pilot-spec SHA-256 binding;
4. create a compact corrected Packet 014A audit binding the new hashes;
5. commit and push only those exact docs paths;
6. require a fresh Phase 1 approval against the revised Packet 014A SHA-256.

The earlier Phase 1 approval is consumed by this stopped preflight and must not be reused.

## Deterministic audit

```text
schema remains unchanged: pass
supplier uniqueness becomes satisfiable: pass
unit-cost source remains supplier-level: pass
expected report values unchanged except supplier ID: pass
expected total remains 503.00: pass
summary counts unchanged: pass
all injection contracts unaffected: pass
oracle boundary unaffected: pass
smallest correction identified: pass
```

## Verdict

```text
PASS
CORRECTION REQUIRED BEFORE PHASE 1
```

## Proposed owner acceptance

```text
Accept the Northstar fixture correction changing WPR-310 supplier_id from SUP-01 to SUP-03, with no other business-rule or expected-output change.
```
