---
id: kz-dec-01KV69T8Q4M7N2R5C3Y6P1FA9B
type: decision
status: proposed
project: kaizen-platform
created: 2026-06-15T15:50:00-04:00
updated: 2026-06-15T15:50:00-04:00
review_status: pending
previous_decision: 04-design-decisions/0016-northstar-baseline-business-rules.md
summary: "Propose the Phase 5 Northstar supplier-review amendment while preserving the accepted baseline decision."
---

# Decision 0018 - Northstar Supplier-Review Amendment

## Context

Decision 0016 established the accepted Northstar baseline rules. Packet 014A Phase 5 requires a rule-modifying amendment involving stale pricing and inactive suppliers while preserving baseline history.

Result 232 freezes the exact owner-controlled replacement rule.

## Proposed decision

Decision 0016 remains authoritative for the baseline implementation and baseline evidence. For amendment version 1, the inactive-supplier and review-status rules are superseded as follows.

### Inactive supplier handling

An active inventory item referencing an existing inactive supplier is no longer invalid solely because the supplier is inactive.

If that item requires reorder, it remains visible in the report for human review.

A missing supplier remains invalid and fails validation.

Inactive inventory items remain excluded from evaluation.

### Status rules

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

Pricing is stale when:

```text
(as_of_date - price_updated_at).days > 90
```

### Cost behavior

Rows with inactive suppliers retain their row-level `unit_cost` and `estimated_cost` values for deterministic review visibility.

Rows with either status below are excluded from the summary total estimated reorder cost:

```text
supplier_review_required
supplier_and_price_review_required
```

Rows with `reorder` or `price_review_required` continue to contribute their estimated cost to the summary total.

### Summary counters

Every emitted row counts as requiring reorder.

The price-review count includes:

```text
price_review_required
supplier_and_price_review_required
```

A fresh-price `supplier_review_required` row does not increment the price-review count.

## Why

The amended rule preserves operational visibility without treating an inactive supplier as immediately purchasable. It avoids losing reorder demand, exposes combined supplier and price risk explicitly, and prevents inactive-supplier estimates from inflating the actionable reorder-cost total.

## Consequences

- validation changes from fail-closed on every inactive supplier to report-with-review for existing inactive suppliers;
- status vocabulary expands by two values;
- summary total logic becomes status-sensitive;
- the baseline decision, fixtures, outputs, hashes, and implementation commit remain unchanged;
- amendment fixtures and expected outputs must be versioned separately;
- implementation requires a separately audited task packet after R2.

## Compatibility

All other Decision 0016 rules remain unchanged:

- equality threshold;
- reserved-inventory subtraction;
- inactive inventory exclusion;
- missing-supplier failure;
- SKU ordering;
- Decimal-only arithmetic;
- exact two-decimal rendering;
- deterministic `--as-of` behavior;
- local-only execution and no remote.

## Authority boundary

This proposed decision does not authorize implementation. It requires audit and explicit owner acceptance before the amended implementation packet may become implementation-ready.
