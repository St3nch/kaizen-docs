---
id: kz-result-01KV9NORTHSTAR014APHASE2GATE
type: research-result
status: pending-owner-decision
project: kaizen-platform
created: 2026-06-14T19:15:00Z
updated: 2026-06-14T19:15:00Z
review_status: steward-reviewed
authority: proposed
summary: "Preserve Northstar source claims and bind the six owner decisions required before baseline specification drafting."
---

# Research Result 202 - Packet 014A Phase 2 Source Conflict and Owner Decision Gate

## Purpose

Preserve the initial Northstar source statements without silently reconciling them, identify the consequential ambiguities, and obtain explicit owner rulings before any accepted Northstar decision, baseline specification, implementation packet, or implementer-brief freeze is created.

## Governing packet

```text
Packet 014A SHA-256:
0057a945fa9b5b31959d1241bb72279a02c150dfde65d225215556b0a0a46e3e

Phase:
2 - Baseline Kaizen planning loop

Phase 1 completion:
03-research-results/201-packet-014a-phase-1-completion.md
SHA-256: 7f73fdab6d911ae85605937e4352b1d5c977eb55a321b37ed6490890ea38dd62
```

## Source bindings

### Source A

```text
path:
C:\dev\kaizen\staging\_pilot\northstar\implementer-brief\source-a.md

SHA-256:
15c2cf236866c7a4957853441d478b5ff2a4feb72ebbefbd959994ff96567e51
```

Preserved claim:

> Parts should be reordered when available stock falls below the configured reorder point.

Additional statements:

- reserved units are tracked separately;
- older supplier pricing should be called out for review.

### Source B

```text
path:
C:\dev\kaizen\staging\_pilot\northstar\implementer-brief\source-b.md

SHA-256:
cc40630773488d7df49431d2f15d8da681373233fa81f420d7fbd3310a895988
```

Preserved claim:

> A part at the reorder point is already low enough to order.

Additional statements:

- reports should be easy to compare between runs;
- inactive records should not create purchasing work.

## Injection 1 conflict

```text
Source A:
available < reorder_point

Source B:
available <= reorder_point
```

The claims are materially incompatible at equality. No implementation rule may be inferred until the owner resolves this conflict.

## Consequential ambiguity matrix

## D1 - Equality threshold

Question:

```text
Does available inventory exactly equal to reorder_point trigger reorder?
```

Options:

```text
A. Yes: available <= reorder_point
B. No: available < reorder_point
```

Downstream effect:

- determines whether FLT-220 appears in the baseline report;
- changes reorder counts and total estimated cost;
- changes tests and golden outputs.

## D2 - Reserved inventory

Question:

```text
Does reserved inventory reduce availability?
```

Options:

```text
A. available = on_hand - reserved
B. available = on_hand
```

Downstream effect:

- changes available quantities;
- may change which rows reorder;
- affects validation for reserved values.

## D3 - Stale pricing

Question:

```text
What happens when supplier pricing is older than 90 days relative to --as-of?
```

Options:

```text
A. Reorder remains eligible and row is marked price_review_required
B. Reorder is blocked until pricing is refreshed
C. Reorder remains eligible with no special status
```

Downstream effect:

- changes FLT-220 status or inclusion;
- changes summary counts;
- changes implementation and tests.

## D4 - Inactive records

Question:

```text
How are inactive inventory items and inactive suppliers handled?
```

Options:

```text
A. Inactive inventory items are excluded; active items referencing inactive suppliers fail validation
B. All inactive records are excluded silently
C. Any inactive item or supplier causes validation failure
```

Downstream effect:

- determines evaluation count;
- determines validation behavior;
- controls whether purchasing work is created from inactive records.

## D5 - Output ordering

Question:

```text
How are report rows ordered?
```

Options:

```text
A. SKU ascending
B. Estimated cost descending, then SKU ascending
C. Preserve inventory input order
```

Downstream effect:

- determines byte-level output stability;
- changes golden files and repeatability tests.

## D6 - Decimal and rounding behavior

Question:

```text
How are monetary values calculated and rendered?
```

Options:

```text
A. Decimal arithmetic; unit_cost and estimated_cost rendered with exactly two decimal places; no binary float
B. Binary float with display rounded to two decimal places
C. Decimal arithmetic with variable-width output
```

Downstream effect:

- controls deterministic bytes;
- affects exact totals and formatting;
- affects validation of malformed decimal values.

## Required owner response

The owner must explicitly choose one option for each decision:

```text
D1: A or B
D2: A or B
D3: A, B, or C
D4: A, B, or C
D5: A, B, or C
D6: A, B, or C
```

Optional rationale may be supplied, but the option choices are mandatory.

## Post-decision work authorized by Phase 2

After exact owner rulings are received, the steward may complete the remaining Phase 2 batch:

1. record accepted Northstar decisions;
2. draft the baseline Northstar specification;
3. draft and audit the baseline implementation task packet;
4. prepare the canonical project overview and current-state candidates through normal governed staging only;
5. freeze the final implementer brief with accepted decisions and audited packet references;
6. stop before canonical execution, implementation coding, or injection release.

## Current stop condition

```text
accepted Northstar decisions: none
baseline specification: not drafted
baseline implementation packet: not drafted
canonical candidate preparation: not started
implementation coding: unauthorized
canonical execution: unauthorized
injection release: unauthorized
```
