---
id: kz-spec-01KV9NORTHSTARBASELINE0001
type: specification
status: accepted
project: kaizen-platform
created: 2026-06-14T19:35:00Z
updated: 2026-06-14T19:35:00Z
review_status: steward-reviewed
authority: accepted
summary: "Baseline implementation specification for the Northstar Stockroom deterministic reorder CLI."
---

# Northstar Stockroom Baseline Implementation Specification

## Authority

This specification implements Decision 0016 and the accepted Packet 014A Phase 2 owner rulings.

## Runtime boundary

```text
Python: 3.11
runtime dependencies: standard library only
development dependency: pytest >=8,<9
network: prohibited
database: prohibited
external API: prohibited
GUI/service/background process: prohibited
repository remote: prohibited
system-clock business-rule use: prohibited
```

## CLI contract

The CLI reads `inventory.csv` and `suppliers.csv` and requires:

```text
--as-of YYYY-MM-DD
--inventory PATH
--suppliers PATH
--output-dir PATH
```

The pilot invocation uses `--as-of 2026-06-14`.

It writes atomically:

```text
reorder-report.csv
reorder-summary.md
```

Malformed or impossible dates fail validation. No output file may be partially created or replaced when validation fails.

## Input schema

`inventory.csv` requires exactly these columns:

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

`suppliers.csv` requires exactly these columns:

```text
supplier_id
supplier_name
unit_cost
lead_time_days
price_updated_at
active
```

Unknown extra columns are rejected.

## Parsing and validation

- CSV is UTF-8 with a header row.
- Blank identifiers or names are invalid.
- Boolean values accept only lowercase `true` or `false`.
- Integer fields accept canonical base-10 integers only.
- `unit_cost` uses `decimal.Decimal` and must be finite and nonnegative.
- Dates use strict ISO `YYYY-MM-DD` parsing.
- Duplicate SKU or supplier ID is invalid.
- Negative numeric values are invalid.
- Reserved inventory greater than on-hand inventory is invalid.
- Every active inventory item must reference exactly one existing active supplier.
- Inactive inventory items are excluded from evaluation.
- Any validation finding fails the run before output mutation.
- Findings identify file, one-based data-row number, field when applicable, and deterministic code.

## Business rules

```text
available = on_hand - reserved
reorder_required = available <= reorder_point
recommended_quantity = reorder_quantity
estimated_cost = Decimal(recommended_quantity) * unit_cost
stale = (as_of_date - price_updated_at).days > 90
```

A stale price does not suppress reorder. Its row status is `price_review_required`; otherwise the status is `reorder`.

Only rows requiring reorder appear in the report.

## Output contract

The report header is:

```text
sku,name,available,reorder_point,recommended_quantity,supplier_id,unit_cost,estimated_cost,status
```

Report rules:

- rows sort by SKU ascending using ordinal text order;
- CSV uses comma delimiters and LF newlines;
- standard-library CSV quoting applies;
- `unit_cost` and `estimated_cost` render with exactly two decimal places;
- output ends with one LF newline.

The summary structure is:

```text
# Reorder Summary

- items evaluated: N
- items requiring reorder: N
- items requiring price review: N
- total estimated reorder cost: 0.00
- invalid rows: 0
```

The total uses Decimal arithmetic and exactly two decimal places. The file ends with one LF newline.

## Determinism

Identical input bytes and identical CLI arguments must produce byte-identical outputs.

The implementation must not depend on current date or time, locale, filesystem enumeration order, network state, environment-specific decimal formatting, or binary floating point.

## Atomic output behavior

The implementation validates and renders both outputs completely before replacing either destination. Temporary files remain inside the output directory. On failure, no newly accepted output pair may exist.

If destination files already exist, both are replaced only after complete validation and rendering succeed.

## Required tests

At minimum:

- baseline fixture behavior without using oracle bytes as implementation input;
- equality threshold and reserved subtraction;
- stale-price status;
- inactive inventory exclusion;
- active item referencing inactive supplier;
- duplicate SKU and supplier ID;
- missing supplier;
- negative values and reserved greater than on-hand;
- malformed decimal and date;
- malformed or impossible `--as-of` date;
- unknown or missing columns;
- exact SKU ordering and two-decimal formatting;
- deterministic repeat;
- no partial outputs on validation failure;
- safe replacement of an existing output pair.

## Explicit exclusions

- implementation coding before Packet 014B approval;
- oracle or golden-output access by the implementation lane;
- network, database, service, UI, deployment, remote, or credentials;
- amended behavior reserved for Packet 014A Phase 5;
- canonical execution during Packet 014A Phase 2.
