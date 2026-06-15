---
id: kz-result-01KV69K4R8T2M6N3Q5C7Y1P9FA
type: research-result
status: complete
project: kaizen-platform
created: 2026-06-15T15:35:00-04:00
updated: 2026-06-15T15:35:00-04:00
review_status: steward-reviewed
authority: proposed
summary: "Map the deterministic impact of the Phase 5 stale-price and inactive-supplier rule change before amendment drafting."
---

# Research Result 231 - Phase 5 Stale-Price and Inactive-Supplier Impact Analysis

## Verdict

```text
IMPACT MAP COMPLETE
BASELINE HISTORY PRESERVABLE
EXACT REPLACEMENT RULE TEXT STILL OWNER-CONTROLLED
NO IMPLEMENTATION AUTHORIZED YET
```

## Baseline rules affected

Decision 0016 currently establishes:

```text
stale price:
reorder remains eligible
status = price_review_required

active item + inactive supplier:
validation failure with inactive_supplier
```

The accepted baseline specification carries the same rules into validation, report status, output totals, fixtures, and required tests.

## Direct doctrine impact

The controlled change necessarily affects:

```text
04-design-decisions/0016-northstar-baseline-business-rules.md
05-specs/northstar-stockroom-baseline-implementation-spec.md
```

Baseline bytes must remain preserved. The amendment must create explicit superseding or versioned records rather than silently rewriting the historical baseline decision and specification without continuity evidence.

## Direct implementation impact

### Validation

```text
src/northstar_stockroom/validation.py
```

Current behavior rejects every active inventory item whose referenced supplier is inactive:

```text
inactive_supplier
```

Any replacement rule that permits some inactive-supplier rows to continue must change this validation boundary. The replacement must define exactly when the run fails, when a row is excluded, and when a row proceeds with a review status.

### Reporting

```text
src/northstar_stockroom/reporting.py
```

Current behavior calculates cost and assigns `price_review_required` solely from price age. A combined stale-price/inactive-supplier rule may affect:

- row eligibility;
- status vocabulary and precedence;
- whether unit cost may be used;
- whether estimated cost contributes to totals;
- summary counters;
- deterministic output bytes.

### Data model

```text
src/northstar_stockroom/models.py
```

A new status alone may not require a model change because `status` is currently a string. Any requirement to represent unavailable cost, multiple review reasons, or supplier state explicitly in output may require a typed model change.

### CLI

```text
src/northstar_stockroom/cli.py
```

The CLI should remain unchanged unless the replacement rule introduces a new explicit argument. No such argument is justified by current evidence.

## Direct test impact

```text
tests/test_validation.py
tests/test_reporting.py
tests/test_cli.py
```

Required amendment tests must cover at least:

1. active item with active supplier and fresh price;
2. active item with active supplier and stale price;
3. active item with inactive supplier and fresh price;
4. active item with inactive supplier and stale price;
5. inactive inventory item with either supplier state;
6. status precedence when more than one review condition applies;
7. exact total-cost behavior for rows whose supplier or price is not accepted for normal purchasing;
8. deterministic repeat and exact ordering;
9. preservation of baseline behavior under the baseline fixture or an explicit versioned fixture boundary.

The existing assertion that invalid fixtures include `inactive_supplier` must either remain valid for the retained failure case or be replaced with a more precise amendment fixture and finding.

## Fixture and output impact

Baseline fixtures and baseline hashes remain immutable:

```text
reorder-report.csv:
e4e3fec9ee4dd4451e9c177dbeeabd965b778589d0499ed7ac5e2cbf06eed5e1

reorder-summary.md:
7761937406bf65055c987be7a419024e58315614e90dd22e4539135955c5f849
```

Phase 5 must add versioned amended fixtures and outputs rather than overwrite:

```text
fixtures/baseline/
phase3-corrected-return baseline evidence
r1-output frozen evidence
```

A safe versioned shape is:

```text
fixtures/amendment-v1/
amended-output-v1/
```

Final names remain packet-controlled.

## Output-contract questions requiring exact owner rule

The durable records do not answer these consequential questions:

1. Does an inactive supplier always block purchasing, or only when its price is stale?
2. Does a stale price continue to permit an estimated cost, or must cost become unavailable?
3. If an inactive supplier row remains visible, what exact status is emitted?
4. If both conditions apply, which status wins or are multiple reasons represented?
5. Does the row count as requiring reorder, requiring review, both, or neither?
6. Is its estimated cost included in the summary total?
7. Does the run succeed with review output or fail validation?

These are business rules, not implementation details. They cannot be inferred from the phrase “stale pricing and inactive suppliers.”

## Minimal amendment boundary

Once exact rule text is released, the likely bounded implementation scope is:

```text
src/northstar_stockroom/validation.py
src/northstar_stockroom/reporting.py
possibly src/northstar_stockroom/models.py
tests/test_validation.py
tests/test_reporting.py
tests/test_cli.py
new versioned amendment fixtures
new versioned expected-output evidence
```

No database, network, service, dependency, remote, UI, deployment, or generalized policy engine is justified.

## R2 readiness impact

R2 can be prepared structurally now, but it cannot be validly run until the accepted change request and governing amendment proposal exist. R2 must receive:

- baseline canonical history;
- exact accepted change request;
- amended decision/spec proposal;
- pre-amendment Northstar checkpoint;
- no oracle or failure schedule;
- one deliberate missing-evidence condition.

## Next valid action

Release and freeze the exact owner-controlled replacement rule for stale pricing and inactive suppliers. Then draft the governed decision and specification amendments, audit them, and prepare R2 before implementation.
