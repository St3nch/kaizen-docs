---
id: kz-spec-01KV69V2Q8M4N7R3C5Y1P6FA9B
type: specification
status: accepted
project: kaizen-platform
created: 2026-06-15T15:55:00-04:00
updated: 2026-06-15T16:25:00-04:00
review_status: approved
authority: accepted
accepted_by: owner.local
accepted_source_sha256: d1dfc7289980a958df0b01d81973d07a833f6614866d0b930b902a10552ee914
baseline_spec: 05-specs/northstar-stockroom-baseline-implementation-spec.md
governing_decision: 04-design-decisions/0018-northstar-supplier-review-amendment.md
summary: "Proposed amendment-v1 specification for inactive-supplier review handling and stale-price status precedence."
---

# Northstar Stockroom Amendment V1 Specification

## Authority

This specification implements proposed Decision 0018 and Result 232. It does not replace the accepted baseline specification for baseline evidence.

## Preserved runtime boundary

```text
Python: 3.11
runtime dependencies: standard library only
network: prohibited
database: prohibited
external API: prohibited
GUI/service/background process: prohibited
repository remote: prohibited
system-clock business-rule use: prohibited
```

The CLI arguments, input columns, atomic output pair, UTF-8 requirement, strict parsing, Decimal arithmetic, deterministic ordering, and baseline validation rules remain unchanged except where explicitly amended below.

## Amended validation contract

For every active inventory item:

- a missing supplier remains a validation error with deterministic code `missing_supplier`;
- an existing inactive supplier is not a validation error solely because it is inactive;
- all supplier fields must still pass normal parsing and structural validation;
- inactive inventory items remain excluded before report evaluation.

The baseline `inactive_supplier` validation finding is retired for amendment-v1 execution. Baseline evidence containing that finding remains preserved and must not be rewritten.

## Amended business rules

```text
available = on_hand - reserved
reorder_required = available <= reorder_point
recommended_quantity = reorder_quantity
estimated_cost = Decimal(recommended_quantity) * unit_cost
stale = (as_of_date - price_updated_at).days > 90
supplier_inactive = supplier.active == false
```

Only rows requiring reorder appear in the report.

Status selection is exact and mutually exclusive:

```text
if supplier_inactive and stale:
    supplier_and_price_review_required
elif supplier_inactive:
    supplier_review_required
elif stale:
    price_review_required
else:
    reorder
```

## Row-level cost contract

Every report row retains:

- `unit_cost` from the supplier record;
- `estimated_cost = recommended_quantity * unit_cost`;
- exact two-decimal formatting.

This preserves review visibility even when the supplier is inactive.

## Summary total contract

The summary total estimated reorder cost includes only rows with status:

```text
reorder
price_review_required
```

It excludes rows with status:

```text
supplier_review_required
supplier_and_price_review_required
```

The exclusion applies only to the summary total. It does not blank, zero, or remove the row-level `estimated_cost` field.

## Summary counter contract

```text
items evaluated:
all active inventory items

items requiring reorder:
all emitted report rows

items requiring price review:
rows with status price_review_required
plus rows with status supplier_and_price_review_required

invalid rows:
0 for a successful run
```

No new supplier-review summary counter is introduced in amendment v1.

## Output contract

The baseline report header remains unchanged:

```text
sku,name,available,reorder_point,recommended_quantity,supplier_id,unit_cost,estimated_cost,status
```

The status field may now contain exactly:

```text
reorder
price_review_required
supplier_review_required
supplier_and_price_review_required
```

Rows remain sorted by SKU ascending. CSV and Markdown newline, quoting, Decimal, and atomic replacement contracts remain unchanged.

## Versioned fixtures and expected outputs

Baseline artifacts are immutable:

```text
fixtures/baseline/
phase3-corrected-return outputs and hashes
r1-output frozen hashes
```

Amendment implementation must add a separate versioned fixture set and generate versioned expected-output evidence. Recommended repository paths:

```text
fixtures/amendment-v1/inventory.csv
fixtures/amendment-v1/suppliers.csv
```

Expected-output evidence must live outside the baseline golden namespace and must not overwrite any baseline bytes. Exact evidence paths are fixed by the amended implementation packet.

The amendment fixture must contain deterministic coverage for:

1. active supplier + fresh price;
2. active supplier + stale price;
3. inactive supplier + fresh price;
4. inactive supplier + stale price;
5. inactive inventory exclusion;
6. at least one missing-supplier invalid case in a separate invalid fixture or unit test.

## Required tests

At minimum:

- existing baseline suite remains green;
- active item with inactive supplier no longer fails solely for inactivity;
- missing supplier still fails;
- exact four-way status precedence;
- inactive-supplier rows retain row-level unit and estimated cost;
- inactive-supplier estimated costs are excluded from summary total;
- combined supplier-and-price review increments price-review count;
- supplier-only review does not increment price-review count;
- all emitted rows count as requiring reorder;
- amended fixture repeat is byte-identical;
- baseline output hashes remain unchanged;
- no partial outputs on amended validation failure;
- no modification of baseline golden or frozen R1 evidence.

## Likely bounded implementation scope

```text
src/northstar_stockroom/validation.py
src/northstar_stockroom/reporting.py
possibly src/northstar_stockroom/models.py
tests/test_validation.py
tests/test_reporting.py
tests/test_cli.py
fixtures/amendment-v1/inventory.csv
fixtures/amendment-v1/suppliers.csv
```

`models.py` should remain unchanged unless implementation proves a typed model change is necessary. No generalized rule engine or new dependency is authorized.

## Explicit exclusions

- editing or deleting baseline evidence;
- changing baseline fixtures to make amended tests pass;
- oracle access by implementation or R2 lanes;
- database, network, service, UI, deployment, remote, or credentials;
- implementation before the amended packet is audited and approved;
- Phase 6 or Milestone 9 closure work.
