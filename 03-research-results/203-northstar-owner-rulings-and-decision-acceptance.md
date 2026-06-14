---
id: kz-result-01KV9NORTHSTAROWNERRULINGS
type: research-result
status: complete
project: kaizen-platform
created: 2026-06-14T19:25:00Z
updated: 2026-06-14T19:25:00Z
review_status: owner-accepted
authority: accepted
summary: "Record accepted Northstar baseline business-rule rulings."
---

# Research Result 203 - Northstar Owner Rulings and Decision Acceptance

## Accepted rulings

```text
D1 A - available <= reorder_point triggers reorder
D2 A - available = on_hand - reserved
D3 A - stale pricing keeps reorder eligible and marks price_review_required
D4 A - inactive inventory is excluded; active item plus inactive supplier fails validation
D5 A - report rows sort by SKU ascending
D6 A - Decimal arithmetic with exactly two decimal places; binary float prohibited
```

## Owner instruction

The owner delegated the six rulings to the steward's recommendation. Option A was recommended for each question in Result 202 and is therefore accepted as the exact owner ruling.

## Rationale

The accepted options preserve the manager-source equality rule, treat reserved stock as unavailable, keep needed reorders visible while flagging stale prices, prevent inactive inventory from creating purchasing work, fail loudly on active-to-inactive supplier defects, produce stable ordering, and avoid binary floating-point drift.

## Consequences

The rulings authorize drafting Decision 0016, the baseline specification, Packet 014B, the final implementer brief, and governed overview/current-state candidates. They do not authorize implementation coding, canonical execution, injection release, fresh-context trials, or oracle disclosure.
