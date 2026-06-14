---
id: kz-result-01KV9NORTHSTARASOFDEFECT00
type: research-result
status: complete
project: kaizen-platform
created: 2026-06-14T18:15:00Z
updated: 2026-06-14T18:15:00Z
review_status: steward-reviewed
authority: proposed
summary: "Reconcile the missing deterministic reference date for Northstar stale-price evaluation."
---

# Research Result 199 - Northstar Stale-Price Reference-Date Preflight Failure and Correction Proposal

## Outcome

```text
corrected Packet 014A Phase 1: stopped before artifact creation
owner-private root created: no
disposable repository created: no
staging evidence root created: no
oracle or golden files created: no
fixtures created: no
Git repository initialized: no
```

## Defect

The accepted Northstar rule states:

```text
Supplier pricing older than 90 days is stale.
```

The accepted input model provides:

```text
price_updated_at
```

But the contract does not define the date against which the 90-day age is evaluated.

If implementation uses the machine clock, identical input files can produce different outputs on different execution dates. That directly conflicts with the accepted rule:

```text
Identical inputs must produce byte-identical outputs.
```

It also makes the sealed oracle time-dependent and prevents a stable fresh-context resumption proof.

## Root cause

The pilot specification defined the stale-price threshold but omitted the deterministic evaluation date from both the CLI contract and input model. Earlier audits checked the threshold and output expectations but did not cross-check time dependence against the byte-identical-output requirement.

## Smallest correction

Add one required CLI argument:

```text
--as-of YYYY-MM-DD
```

Freeze the baseline and amendment pilot oracle at:

```text
--as-of 2026-06-14
```

Stale pricing is evaluated as:

```text
(as_of_date - price_updated_at).days > 90
```

The implementation must reject malformed or impossible dates and must not read the system clock for business-rule evaluation.

## Why this is the smallest valid fix

This correction:

- preserves both CSV schemas;
- preserves all twelve existing business rules;
- makes the stale-price rule deterministic;
- preserves byte-identical output for identical files plus identical CLI arguments;
- keeps the baseline and amended oracle stable;
- adds no database, network, service, dependency, or infrastructure;
- does not resolve any of the deliberate business ambiguities prematurely;
- does not change the expected baseline report, totals, or summary counts.

## Required reconciliation after owner acceptance

1. amend the fixed problem statement to require `--as-of YYYY-MM-DD`;
2. clarify that determinism binds identical input files and identical CLI arguments;
3. add the exact stale-date formula and prohibit system-clock use;
4. bind the pilot oracle to `2026-06-14`;
5. rebind Packet 014A to the corrected pilot-spec SHA-256;
6. issue one compact corrected audit;
7. require a fresh Phase 1 approval against the new Packet 014A SHA-256.

The Phase 1 approval against Packet 014A SHA-256 `99fb1e573a2eec0053287799f6737ed53f3b6e03b5e488f2e01ce45733aa6b13` is consumed by this stopped preflight and must not be reused.

## Deterministic audit

```text
system-clock dependence removed: yes
CSV schemas unchanged: yes
expected baseline report unchanged: yes
expected total 503.00 unchanged: yes
summary counts unchanged: yes
all eight injections unaffected: yes
R1 and R2 remain stable: yes
implementation remains standard-library only: yes
smallest correction identified: yes
```

## Verdict

```text
PASS
CORRECTION REQUIRED BEFORE PHASE 1
```

## Proposed owner acceptance

```text
Accept the Northstar deterministic stale-price correction requiring --as-of YYYY-MM-DD, freezing the pilot at --as-of 2026-06-14, and prohibiting system-clock use for business-rule evaluation, with no other business-rule or expected-output change.
```
