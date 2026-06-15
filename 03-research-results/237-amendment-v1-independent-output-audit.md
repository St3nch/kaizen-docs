---
id: kz-result-01KV6D3Q8M4N7R3C5Y1P9T6FA
type: research-result
status: complete
project: kaizen-platform
created: 2026-06-15T19:45:00Z
updated: 2026-06-15T19:45:00Z
review_status: steward-reviewed
authority: proposed
summary: "Independently verify frozen amendment-v1 output bytes against the accepted rule and fixtures before sealed-oracle attestation."
---

# Research Result 237 - Amendment V1 Independent Output Audit

## Verdict

```text
PASS AGAINST ACCEPTED DECISION, SPECIFICATION, FIXTURES, AND DETERMINISTIC BYTE CONTRACT
SEALED OWNER-ORACLE ATTESTATION STILL PENDING
```

## Frozen outputs

```text
reorder-report.csv SHA-256:
1c1db2c6ba22a623d2ab0ec41f33bfb9f119f75cb2a1f2319909b0c5e35b1eb3

reorder-summary.md SHA-256:
6c7f928819559a59ebda461f73f910c434f0ce51a0d8879cb621c408f4214397
```

Both independently generated amendment runs produced the same hashes.

## Expected report derivation

The versioned amendment fixture contains four active reorder-eligible rows and one inactive inventory row that is excluded.

The accepted rule yields:

```text
ACT-FRESH:
active supplier, fresh price
status = reorder
estimated cost = 10 * 10.00 = 100.00
included in summary total

ACT-STALE:
active supplier, stale price
status = price_review_required
estimated cost = 10 * 20.00 = 200.00
included in summary total
increments price-review count

INA-FRESH:
inactive supplier, fresh price
status = supplier_review_required
estimated cost = 10 * 30.00 = 300.00
visible in row
excluded from summary total
no price-review increment

INA-STALE:
inactive supplier, stale price
status = supplier_and_price_review_required
estimated cost = 10 * 40.00 = 400.00
visible in row
excluded from summary total
increments price-review count
```

Rows sort by SKU ascending. The frozen report contains exactly these four rows in the required order and with the required statuses and two-decimal values.

## Expected summary derivation

```text
items evaluated:
4 active inventory items

items requiring reorder:
4 emitted rows

items requiring price review:
ACT-STALE + INA-STALE = 2

total estimated reorder cost:
100.00 + 200.00 = 300.00

invalid rows:
0
```

The frozen summary contains exactly:

```text
# Reorder Summary

- items evaluated: 4
- items requiring reorder: 4
- items requiring price review: 2
- total estimated reorder cost: 300.00
- invalid rows: 0
```

## Byte-contract audit

- UTF-8 text: pass
- LF newlines: pass
- exact report header: pass
- exact SKU ordering: pass
- exact status strings: pass
- exact two-decimal formatting: pass
- terminal LF: pass
- deterministic repeat: pass
- baseline hashes unchanged: pass

## Oracle boundary

The owner-private amendment-v1 oracle is intentionally outside every Go8 root and was not accessed. This audit therefore does not claim direct sealed-oracle comparison.

The final owner-only action is to compare the sealed oracle's two expected hashes with:

```text
1c1db2c6ba22a623d2ab0ec41f33bfb9f119f75cb2a1f2319909b0c5e35b1eb3
6c7f928819559a59ebda461f73f910c434f0ce51a0d8879cb621c408f4214397
```

If both match, the owner may attest that the amendment-v1 oracle comparison passed and authorize governed canonical return planning. If either differs, Phase 5 must remain open and the discrepancy must be investigated without modifying frozen evidence.
