---
id: kz-tp-01KV9NORTHSTAR014B00000000
type: task-packet
status: draft
project: kaizen-platform
created: 2026-06-14T19:40:00Z
updated: 2026-06-14T19:40:00Z
review_status: pending
authority: proposed
summary: "Implement the deterministic Northstar Stockroom baseline CLI in the disposable local repository."
---

# Task Packet 014B - Northstar Baseline Implementation

## Purpose

Implement the accepted Northstar Stockroom baseline specification in the disposable local-only repository without access to the sealed oracle, golden outputs, expected hashes, evaluator instructions, or unreleased injection schedule.

## Governing inputs

```text
Packet 014A:
0057a945fa9b5b31959d1241bb72279a02c150dfde65d225215556b0a0a46e3e

Result 203 owner rulings:
8be75b1617ec347e5c23d2872dd879e603b81f440c8e7867cad5e1174391daf7

Decision 0016:
69745ab83b32a0193c95328b8d0a1d4e3d36661f74f6369c260a669e592cb9e9

Baseline specification:
933a736e2868d50f2783b7e33d2b56cad008bdb9bbd2bbc19bef59874a08b5c3

Northstar starting commit:
8f87de7d317bc0e8c28d84031c24ec6baff41be1
```

## Repository and branch

```text
root:
C:\dev\kaizen-pilot-northstar

branch:
main

remote:
none
```

The repository must be clean and at the exact starting commit before implementation begins.

## Allowed changed paths

```text
README.md
pyproject.toml
src/northstar_stockroom/__init__.py
src/northstar_stockroom/__main__.py
src/northstar_stockroom/cli.py
src/northstar_stockroom/models.py
src/northstar_stockroom/validation.py
src/northstar_stockroom/reporting.py
tests/test_cli.py
tests/test_validation.py
tests/test_reporting.py
fixtures/invalid/inventory.csv
fixtures/invalid/suppliers.csv
```

No other path may change without a separately reviewed packet amendment.

## Implementation requirements

- Python 3.11.
- Standard-library runtime only.
- Pytest is the only development dependency.
- Implement the exact CLI, validation, business rules, deterministic rendering, and atomic output behavior in the accepted specification.
- Do not read, infer, search for, or request oracle or golden-output material.
- Do not use network, database, external API, GUI, service, background process, credentials, deployment, or repository remote.
- Do not use the system clock for business-rule evaluation.
- Do not use binary floating point for monetary calculations.
- Preserve the repository as local-only.

## Required tests

Implement all tests required by the baseline specification, including invalid fixture coverage and atomic output failure behavior.

The first implementation-return sequence is controlled by Packet 014A Phase 3 and may include deliberate failure injections. Those injections are not released by this packet draft or Phase 2.

## Verification

Before return:

1. run focused tests;
2. run the full repository suite;
3. run compile checks;
4. run Ruff if configured;
5. run the baseline CLI twice with identical inputs and arguments;
6. prove output hashes are identical across runs;
7. prove invalid fixtures create no partial accepted output pair;
8. verify exact changed paths;
9. verify no remote exists;
10. commit only exact reviewed paths locally.

## Required implementation return

Return evidence must include:

```text
starting commit
ending commit
exact changed paths
commands executed
focused and full test summaries
compile/lint summaries
baseline output paths and SHA-256 values
deterministic-repeat hashes
invalid-fixture failure evidence
no-partial-output evidence
working-tree state
remote state
known limitations
```

## Stop conditions

Stop immediately on:

- starting commit or clean-state mismatch;
- oracle exposure or suspected contamination;
- need to change an unapproved path;
- need for a runtime dependency;
- need for network, database, service, remote, or credentials;
- ambiguity not resolved by Decision 0016 or the specification;
- inability to meet atomic output behavior.

## Authority boundary

Packet 014B is not implementation-authorized by drafting or audit. Implementation requires separate owner approval during Packet 014A Phase 3.

No canonical Kaizen mutation, injection release, fresh-context trial, or oracle comparison is authorized by Packet 014B alone.
