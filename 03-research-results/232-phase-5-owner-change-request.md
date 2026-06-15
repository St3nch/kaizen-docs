---
id: kz-result-01KV69S4Q7M2T8N5R3C6Y1P9FA
type: research-result
status: complete
project: kaizen-platform
created: 2026-06-15T15:45:00-04:00
updated: 2026-06-15T15:45:00-04:00
review_status: owner-accepted
authority: accepted
summary: "Freeze the exact owner-controlled Phase 5 stale-price and inactive-supplier replacement rule."
---

# Research Result 232 - Phase 5 Owner Change Request

## Exact owner rule

The owner supplied the following replacement behavior:

```text
Active items with inactive suppliers remain in the report as
supplier_review_required, but their estimated cost is excluded from totals.
If the price is also stale, use supplier_and_price_review_required.
```

## Deterministic interpretation

The rule is frozen with these implementation consequences:

1. An active inventory item referencing an existing inactive supplier no longer fails validation solely because the supplier is inactive.
2. If the item requires reorder and the supplier is inactive with a non-stale price, the row remains in the report with status:

```text
supplier_review_required
```

3. If the item requires reorder and the supplier is inactive with a stale price, the row remains in the report with status:

```text
supplier_and_price_review_required
```

4. A row under either inactive-supplier status remains a reorder row and contributes to the report row count.
5. Its row-level `unit_cost` and `estimated_cost` remain deterministically rendered from the supplier record for review visibility.
6. Its `estimated_cost` is excluded from the summary total estimated reorder cost.
7. The summary price-review count includes every row whose status reflects stale pricing:

```text
price_review_required
supplier_and_price_review_required
```

8. A supplier-review row with a fresh price does not increment the price-review count.
9. Missing suppliers remain validation failures.
10. Inactive inventory items remain excluded.
11. Baseline fixtures, outputs, hashes, and history remain immutable.

## Status precedence

```text
active supplier + fresh price:
reorder

active supplier + stale price:
price_review_required

inactive supplier + fresh price:
supplier_review_required

inactive supplier + stale price:
supplier_and_price_review_required
```

## Scope boundary

This change request authorizes decision/spec amendment drafting, R2 preparation, versioned fixture and golden-output planning, and separate implementation-packet drafting. It does not itself authorize implementation changes before those records are audited and accepted.
