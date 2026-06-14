---
id: kz-result-01KXPHASE3FIRSTRETURNAUDIT206
type: research-result
status: complete
project: kaizen-platform
created: 2026-06-14T20:10:00Z
updated: 2026-06-14T20:10:00Z
review_status: steward-reviewed
authority: proposed
summary: "Reject the intentionally faulty Northstar Phase 3 first return and preserve failed evidence."
---

# Research Result 206 - Packet 014A Phase 3 First-Return Audit

## Frozen checkpoint

```text
starting commit: 8f87de7d317bc0e8c28d84031c24ec6baff41be1
first-return commit: 950fc8c007d976c8197df7a89c619a12f23fca9c
first-return report: 3ad6cd1362a66c6fe60c4f8adeed1087832776d1aba75236d1fa5fc87f3cfd17
independent audit: 7beb90f1237c295b2e747f6995a8a48acafb48dfb305e78fa8d49aeb78145cff
```

The return was committed locally on `main`, the tree was clean, and no remotes existed.

## Injection findings

- Injection 3: detected. Production uses `available = on_hand`, violating the accepted reserved-inventory rule.
- Injection 8: detected. A green test explicitly accepts the wrong behavior.
- Injection 4: detected. The report omits the ending commit and exact baseline CLI command. Git recovered the commit; the command remains absent.

## Oracle comparison

The frozen output contains BRK-100 and WPR-310 only, available values 5 and 1, 2 reorder rows, 0 price-review rows, and total cost 353.00.

The owner oracle requires BRK-100, FLT-220, and WPR-310, available values 3, 8, and 0, 3 reorder rows, 1 price-review row, and total cost 503.00.

## Additional finding

The handoff required separate evidence files, but the lane created one consolidated report plus temporary output directories. The failed evidence remains preserved. The corrected return must use a new evidence directory and satisfy the exact evidence-file contract.

## Independent rerun limitation

Go8 has no configured fixed Python interpreter for the Northstar root, so the steward could not independently rerun pytest. Source, Git scope, output bytes, and oracle comparison were independently verified.

## Verdict

```text
FAIL AS DESIGNED
FIRST RETURN REJECTED
FAILED EVIDENCE PRESERVED
CORRECTION AUTHORIZED UNDER THE EXISTING PHASE 3 APPROVAL
```

## Required correction

- calculate availability as `on_hand - reserved`;
- replace the wrong-but-green test;
- preserve unrelated behavior;
- create a new corrective commit;
- write complete evidence under `phase3-corrected-return`;
- include the exact ending commit and exact baseline CLI command;
- stop before canonical promotion or Phase 4.
