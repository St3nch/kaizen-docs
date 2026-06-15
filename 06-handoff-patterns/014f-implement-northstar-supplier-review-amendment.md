---
id: kz-tp-01KV6A2FQ7M4N8R3C5Y1P9T6BW
type: task-packet
status: draft
project: kaizen-platform
created: 2026-06-15T16:05:00-04:00
updated: 2026-06-15T16:05:00-04:00
review_status: pending
authority: proposed
summary: "Implement and verify Northstar amendment v1 supplier-review behavior without altering baseline history."
---

# Task Packet 014F - Implement Northstar Supplier-Review Amendment

## Objective

Implement the accepted Phase 5 amendment for inactive-supplier review visibility, stale-price status precedence, and summary-total exclusion while preserving all baseline evidence.

## Governing records

```text
03-research-results/232-phase-5-owner-change-request.md
04-design-decisions/0018-northstar-supplier-review-amendment.md
05-specs/northstar-stockroom-amendment-v1-spec.md
03-research-results/231-phase-5-stale-price-inactive-supplier-impact-analysis.md
```

Exact source hashes must be recorded after owner acceptance and before implementation begins.

## Starting checkpoint

```text
repository:
C:\dev\kaizen-pilot-northstar

branch:
main

required starting HEAD:
bf9abb513d6f22fc5ed800301a8b3ba0b75c5189

required tracked working tree:
clean

required remotes:
none
```

Disposable untracked `r1-output/` and Python cache artifacts must be handled through reviewed bounded cleanup or preserved outside implementation scope before coding. Never use reset, stash, broad clean, or destructive evidence deletion.

## Authorized implementation scope

Existing paths:

```text
src/northstar_stockroom/validation.py
src/northstar_stockroom/reporting.py
tests/test_validation.py
tests/test_reporting.py
tests/test_cli.py
```

Conditionally authorized only if implementation proves necessary:

```text
src/northstar_stockroom/models.py
```

New versioned fixture paths:

```text
fixtures/amendment-v1/inventory.csv
fixtures/amendment-v1/suppliers.csv
```

New implementation-return evidence must be created beneath:

```text
C:\dev\kaizen\staging\_pilot\northstar\phase5-amendment-v1-return\
```

The implementation lane must not access owner-private oracle material or write owner-controlled golden bytes.

## Required behavior

### Validation

- Existing inactive suppliers no longer fail validation solely because they are inactive.
- Missing suppliers still fail deterministically.
- Inactive inventory remains excluded.
- Every other baseline validation rule remains unchanged.

### Four-way status matrix

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

### Cost behavior

- Every emitted row retains exact `unit_cost` and calculated `estimated_cost`.
- Rows with inactive suppliers are excluded from the summary total estimated reorder cost.
- Active-supplier rows remain included, including stale-price rows.

### Counter behavior

- Every emitted row increments items requiring reorder.
- `price_review_required` and `supplier_and_price_review_required` increment items requiring price review.
- `supplier_review_required` does not increment items requiring price review.
- No new summary field is introduced.

## Baseline preservation gate

Before and after implementation, verify these accepted hashes remain unchanged:

```text
baseline reorder-report.csv:
e4e3fec9ee4dd4451e9c177dbeeabd965b778589d0499ed7ac5e2cbf06eed5e1

baseline reorder-summary.md:
7761937406bf65055c987be7a419024e58315614e90dd22e4539135955c5f849

frozen R1 report:
f1e1b5ab50698159e620cb014d3f4c27c5a6a28101e0f0e78237db163fb0e4dc
```

Do not modify:

```text
fixtures/baseline/
C:\dev\kaizen\staging\_pilot\northstar\phase3-corrected-return\
C:\dev\kaizen\staging\_pilot\northstar\r1-frozen-report.md
owner-private control material
```

## Required tests

At minimum:

1. all existing 13 baseline tests pass unchanged or with only specification-required expectation updates that do not weaken baseline coverage;
2. inactive supplier plus fresh price emits `supplier_review_required`;
3. inactive supplier plus stale price emits `supplier_and_price_review_required`;
4. active supplier plus stale price remains `price_review_required`;
5. active supplier plus fresh price remains `reorder`;
6. missing supplier still fails;
7. inactive inventory remains excluded;
8. inactive-supplier row costs remain visible;
9. inactive-supplier costs are excluded from summary total;
10. combined review increments price-review count;
11. supplier-only review does not increment price-review count;
12. amendment fixture output is byte-identical across two runs;
13. baseline fixture output remains byte-identical to accepted baseline hashes;
14. amended validation failure creates no partial outputs;
15. exact changed-path scope passes.

## Required implementation return

Freeze, before independent audit:

```text
implementation-return.md
test-results.txt
compile-results.txt
amendment-run-1-sha256.json
amendment-run-2-sha256.json
baseline-regression-sha256.json
invalid-fixture-result.txt
changed-paths.txt
repository-status.txt
```

The return must include starting and ending commit, exact changed paths, every command and return code, amended output hashes, baseline-preservation evidence, no-remote evidence, no-oracle-access confirmation, deviations, and discoveries.

## Independent audit gate

The independent audit must compare implementation and tests against Decision 0018 and the amendment-v1 specification, not merely accept a green suite. It must verify the four-way matrix, cost-total exclusion, counter behavior, baseline immutability, and evidence completeness.

Owner/audit oracle comparison occurs only after implementation outputs and report are frozen.

## Stop conditions

Stop immediately on unexpected starting HEAD or tracked dirt, need for an unapproved source path, baseline evidence mutation, ambiguous behavior, oracle exposure, missing evidence, remote creation, or unexplained test results.

## Explicit exclusions

- Phase 6 closure work;
- database or production infrastructure;
- new runtime dependencies;
- generic policy engine;
- canonical vault mutation before governed return;
- remote creation or push.
