---
id: kz-dec-01KV9NORTHSTARBASELINE0016
type: decision
status: active
project: kaizen-platform
created: 2026-06-14T19:30:00Z
updated: 2026-06-14T19:30:00Z
review_status: approved
authority: accepted
summary: "Accept the six baseline Northstar Stockroom business-rule decisions for Milestone 9."
---

# Decision 0016 - Northstar Baseline Business Rules

## Context

Packet 014A Phase 2 preserved contradictory source claims about reorder equality and identified six consequential ambiguities. Result 202 bound those questions. Result 203 records the owner's delegated acceptance of the steward's recommended option A for each question.

Decision 0015 remains reserved for the separate governance-compression candidate and is not consumed by this pilot decision.

## Decision

### Equality threshold

A reorder is required when:

```text
available <= reorder_point
```

### Reserved inventory

Available inventory is:

```text
available = on_hand - reserved
```

### Stale pricing

Pricing is stale when:

```text
(as_of_date - price_updated_at).days > 90
```

Stale pricing does not suppress reorder. The row status becomes:

```text
price_review_required
```

### Inactive records

Inactive inventory items are excluded from evaluation and do not create purchasing work.

An active inventory item referencing an inactive supplier is invalid and must fail validation.

### Output ordering

Report rows sort by SKU ascending.

### Decimal behavior

Monetary calculations use decimal arithmetic only. Binary floating-point arithmetic is prohibited.

`unit_cost`, `estimated_cost`, and total estimated reorder cost render with exactly two decimal places.

## Deterministic consequences

- equality behavior is explicit and testable;
- reserved inventory cannot be double-allocated;
- stale prices remain visible without suppressing operational need;
- active-to-inactive supplier integrity defects fail loudly;
- output order is byte-stable;
- decimal formatting is exact and auditable.

## Compatibility

This decision preserves the accepted Northstar pilot specification, corrected supplier fixture, deterministic `--as-of` date, local-only implementation boundary, sealed-oracle separation, and all Packet 014A phase gates.

## Authority boundary

This decision authorizes specification and task-packet drafting only. It does not authorize implementation coding, canonical execution, injection release, fresh-context trials, or oracle disclosure.
