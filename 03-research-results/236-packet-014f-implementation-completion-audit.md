---
id: kz-result-01KV6C2Q8M4N7R3C5Y1P9T6FA
type: research-result
status: complete
project: kaizen-platform
created: 2026-06-15T19:35:00Z
updated: 2026-06-15T19:35:00Z
review_status: steward-reviewed
authority: accepted
summary: "Accept Packet 014F implementation and freeze the Phase 5 amendment-v1 return evidence."
---

# Research Result 236 - Packet 014F Implementation Completion Audit

## Verdict

```text
PASS
PACKET 014F IMPLEMENTATION ACCEPTED
AMENDMENT V1 RETURN EVIDENCE COMPLETE
BASELINE HISTORY PRESERVED
READY FOR GOVERNED CANONICAL RETURN PLANNING
```

## Repository binding

```text
starting commit:
bf9abb513d6f22fc5ed800301a8b3ba0b75c5189

ending commit:
0d181e70f0246776b8ab3948da181a9f41cfcd0e

branch:
main

tracked working tree:
clean

remotes:
none
```

Known untracked disposable artifacts remain:

```text
r1-output/
src/northstar_stockroom/__pycache__/
tests/__pycache__/
```

They were not staged, modified intentionally, deleted, or treated as implementation scope.

## Exact changed paths

```text
fixtures/amendment-v1/inventory.csv
fixtures/amendment-v1/suppliers.csv
src/northstar_stockroom/reporting.py
src/northstar_stockroom/validation.py
tests/test_cli.py
tests/test_reporting.py
tests/test_validation.py
```

No `models.py`, CLI contract, baseline fixture, dependency, remote, or unrelated source path changed.

## Rule implementation audit

The implementation matches Decision 0018 and the amendment-v1 specification:

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

Existing inactive suppliers no longer fail solely for inactivity. Missing suppliers still fail. Inactive inventory remains excluded.

Inactive-supplier rows retain exact row-level `unit_cost` and `estimated_cost`. Their estimated costs are excluded from the summary total. `supplier_and_price_review_required` increments the price-review counter; `supplier_review_required` does not. Every emitted row counts as requiring reorder.

Result: PASS.

## Test and compile evidence

```text
focused suite:
17 passed in 0.16s

full suite:
17 passed in 0.08s

compileall:
passed
```

Tests cover validation changes, missing-supplier regression, four-way status precedence, visible row costs, total exclusion, price-review counting, deterministic CLI output, inactive inventory exclusion, and baseline behavior.

Result: PASS.

## Deterministic amendment outputs

Both amendment runs produced identical hashes:

```text
reorder-report.csv:
1c1db2c6ba22a623d2ab0ec41f33bfb9f119f75cb2a1f2319909b0c5e35b1eb3

reorder-summary.md:
6c7f928819559a59ebda461f73f910c434f0ce51a0d8879cb621c408f4214397
```

The report contains exactly four rows in SKU order with the accepted four statuses. Row costs are visible. The summary records four reorder rows, two price-review rows, and total estimated reorder cost `300.00`, excluding the two inactive-supplier rows.

Result: PASS.

## Baseline preservation

Post-implementation baseline regeneration produced the exact accepted hashes:

```text
reorder-report.csv:
e4e3fec9ee4dd4451e9c177dbeeabd965b778589d0499ed7ac5e2cbf06eed5e1

reorder-summary.md:
7761937406bf65055c987be7a419024e58315614e90dd22e4539135955c5f849
```

The frozen R1 report remains bound to:

```text
f1e1b5ab50698159e620cb014d3f4c27c5a6a28101e0f0e78237db163fb0e4dc
```

No baseline fixture, Phase 3 corrected-return evidence, frozen R1 evidence, or owner-private oracle material changed.

Result: PASS.

## Failure atomicity

The invalid fixture run returned code `2`, reported `reserved_exceeds_on_hand`, and created no output directory or partial output files.

Result: PASS.

## Return evidence inventory

Required evidence exists beneath:

```text
C:\dev\kaizen\staging\_pilot\northstar\phase5-amendment-v1-return\
```

Files:

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

Versioned run and baseline-regression output directories are also present.

Result: PASS.

## Integrity note

All authorized text changes passed text-integrity scanning. The aggregate changed-tree scan also encountered known untracked `.pyc` cache files and correctly classified them as non-text; this is not a source-integrity failure.

## Scope boundary

This audit accepts the local Packet 014F implementation and evidence only. It does not by itself authorize direct canonical vault editing, remote creation, Northstar push, or Milestone 9 closure.

The next valid action is governed canonical return planning for the accepted amendment-v1 decision/specification/current-state changes, using Kaizen MCP and exact immutable evidence bindings.
