---
id: kz-result-01KTW3D6F8G0H2J4K6M8N0PQRS
type: research-result
status: complete
project: kaizen-platform
created: 2026-06-11T20:26:00Z
updated: 2026-06-11T20:26:00Z
review_status: steward-reviewed
authority: proposed
summary: "Packet 012B implementation result for platform loader, concurrency, dirty-state, and stale-binding reliability."
---

# Research Result 141 - Packet 012B Platform Loader and Concurrency Implementation

## Authority

```text
Packet 012B SHA-256:
8d89f773a79be91437695e939bbc6884a9a61237536bac9b03b95ce67b870387

Owner implementation approval:
exact phrase supplied in active governed session
```

## Starting checkpoint

```text
repository: C:\dev\kaizen\platform
branch: main
HEAD: 1b8be1d6d42d768587dddb2be8415fa24b670561
working tree: clean
remotes: none
```

## Ending checkpoint

```text
repository: C:\dev\kaizen\platform
branch: main
HEAD: 0ebb3f74ed06c8bbe8b22ef9699f0abaf7cc90bf
commit message: Prove amendment loader and concurrency reliability
remotes: none
push: none
```

## Exact changed paths

```text
tests/test_amendment_candidate.py
SHA-256: 65873d15db11def10fbe79e4b9c87e96044bd91759158b4001f54fba5bb39e69

tests/test_amendment_candidate_hammer.py
SHA-256: 9586a418dba879ef4516c38e477221a8df8de9e3d9d8a51f1587433c665d6e20
```

No production source file changed.

## M8-D01 determination

```text
disposition after Packet 012B: proven fixed at platform layer
production code required: no
test changes required: yes
```

The current loader already satisfied the frozen C1 behavior. Packet 012B added direct proof for:

- missing preparation;
- omitted optional packet/live binding with full evidence verification;
- wrong packet;
- preparation self-hash mismatch;
- operation binding mismatch;
- validation-run mismatch;
- each evidence-file hash;
- read-only behavior;
- no write-mutex entry;
- no operation event, governance event, temporary evidence, canonical mutation, or evidence mtime change;
- symlinked evidence rejection without following the target.

Observed structured errors:

```text
missing preparation: preparation_missing
wrong packet: live_packet_mismatch
self-hash mismatch: preparation_hash_mismatch
operation mismatch: preparation_binding_mismatch
changed evidence: preparation_evidence_changed
validation-run mismatch: validation_evidence_invalid
symlinked evidence: preparation_recovery_required
```

## M8-D02 root cause

```text
root cause: test fixture interference on Windows
production defect: no
production code required: no
test repair required: yes
```

The conflicting-request fixture calculated `expected_prior_sha256` independently inside each concurrent contender. That caused both threads to read the same canonical destination while the other contender could hold a native file handle. A focused run reproduced the resulting Windows `PermissionError` during the fixture hash read.

This explains the intermittent destination-access drift. The race was outside operation conflict semantics: the supposedly immutable expected prior hash was being derived inside the race.

The fix computes the expected prior SHA-256 once before launching contenders and passes the same immutable value to every request. No exception translation or production ordering change was made.

## Concurrency proof

### Existing two-contender test

The corrected test now shares exactly:

- one `RootConfig`;
- one destination;
- one expected prior hash;
- one packet ID;
- one operation ID;
- one prepared timestamp;
- two differing replacement payloads.

Result:

```text
creator: 1
loser: 1
loser code: idempotency_conflict
mixed evidence: 0
```

### 100-round stress test

```text
rounds: 100
creators: 100
conflicting losers: 100
idempotency_conflict: 100
unexpected error codes: 0
mixed evidence: 0
incomplete final preparations: 0
```

Every round verified:

- exact six-file preparation set;
- preparation self-hash;
- replacement payload hash;
- canonical candidate hash;
- prior canonical hash;
- reviewed diff hash;
- validation hash;
- validation-run agreement;
- packet and operation agreement;
- one selected replacement label in candidate and diff;
- absence of the losing replacement label from candidate and diff.

Concurrency boundary proven directly by Packet 012B tests:

```text
thread-level conflicting preparation under the existing Windows mutex implementation
```

Cross-process mutex semantics were not newly exercised by Packet 012B. No flaky subprocess harness was added. Existing implementation inspection still shows the entire preparation state machine is entered beneath `staging_write_mutex`.

## Dirty repository and stale binding proof

Existing disposable live-mirror tests passed for:

```text
dirty vault before plan: live_git_dirty
dirty platform before plan: live_platform_drift
platform HEAD drift after binding: live_platform_drift
vault HEAD drift after binding: live_git_head_mismatch
wrong packet: live_packet_mismatch
governance/log or bound-environment drift: fail closed before approval or execution
```

No live platform, vault, staging, governance log, connector, or process state was mutated.

## Verification results

```text
focused loader + hammer tests:
20 passed in 18.36s

focused loader + hammer + live-promotion tests after final edits:
43 passed in 20.56s

live-promotion dirty/stale suite:
23 passed in 29.13s

full platform suite:
281 passed in 38.40s

compileall src + tests:
pass

static capability scan of changed Python paths:
pass, no findings

hidden Unicode and binary scan:
pass

Git diff check:
pass

exact changed-path verification:
pass
```

Ruff was not added or run because the accepted platform policy does not require it.

## Boundary confirmation

```text
Kaizen MCP changes: none
Go8 changes: none
connector changes: none
platform production source changes: none
vault changes: none
live staging changes: none
remote creation: none
platform push: none
process restart or termination: none
dependency changes: none
Packet 012C work: none
```

## Completion status

```text
Packet 012B implementation: complete
Packet 012B completion audit: required next
Packet 012C: not authorized
```
