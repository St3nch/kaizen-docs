# Packet 017B Live Rerun and Correction Return

Status: correction return
Date: 2026-06-20
Milestone: 12 — Recovery Realism and Operational Restore Proof
Packet: 017B

## 1. Trigger

The owner ran the Packet 017B live proof command block in PowerShell and returned the pytest output.

The first run produced two focused recovery integration failures:

```text
test_generation_records_failed_consistency_without_verified_passed
test_live_two_generations_restore_and_retention_dry_run
```

The migration hammer passed its available live command:

```text
3 passed
```

The full suite before correction was:

```text
2 failed, 411 passed
```

## 2. Failure disposition

### F-017B-01 — same-length tamper fixture bug

The first failure expected `hash mismatch`, but the test wrote `b"tampered"`, which has a different byte length than the original fixture payload. The production backup code correctly rejected the reference earlier with:

```text
referenced file length mismatch
```

Disposition: test fixture corrected to write same-length different bytes.

### F-017B-02 — service-role restore smoke overclaimed operational readiness

The second failure returned:

```text
Outcome.REJECTED
bounded_code='service_unavailable'
```

This happened when attempting the restored typed-service smoke check through the service-role DSN against the disposable restore database.

Disposition: corrected the test to prove typed-service usability against the disposable restored database through `OpsService`, while preserving the Packet 017B boundary that operational role-ready restore remains deferred.

This is not a production-code defect. It is a test-scope correction that prevents Packet 017B from overclaiming operational role-ready restore.

## 3. Corrective platform commit

```text
b7593c5ee90fd32c1e2a86572cc570d307de2be6
```

Changed path:

```text
tests/test_ops_recovery_integration.py
```

File SHA-256 after correction:

```text
df92a9699fd8908f618e8385b625acae670c7466743a12b3712a8c3861720fb2
```

## 4. Owner rerun after correction

The owner reran:

```text
python -m pytest -q tests/test_ops_recovery_integration.py
python -m pytest -q tests/test_ops_db_migration_hammer.py
python -m pytest -q
```

Observed output after correction:

```text
tests/test_ops_recovery_integration.py: 2 skipped
tests/test_ops_db_migration_hammer.py: 2 passed, 1 skipped
full platform suite: 379 passed, 34 skipped
```

The recovery integration skips remained because the earlier command block had cleared the required DSN environment variables from the PowerShell session before this rerun.

## 5. Current proof posture

Accepted proof from owner-rerun output:

```text
full non-credential-gated platform suite passes
migration hammer non-live parts pass
corrected recovery integration has no observed failure when env vars are absent; it skips as designed
```

Still not proven by live execution after correction:

```text
credential-gated nonzero-file recovery integration
credential-gated restore proof
credential-gated post-restore typed-service smoke check
credential-gated live migration hammer rejection checks
```

## 6. Updated Packet 017B disposition

Packet 017B has now produced:

```text
proof-oriented platform test correction
Milestone 12 specification/runbook note
implementation return
correction return
full non-credential-gated platform pass
```

Packet 017B should still not be treated as full Milestone 12 closure unless one of the following happens:

1. the owner reruns the focused proof commands with DSN environment variables still set and the credential-gated tests pass; or
2. the owner explicitly accepts the credential-gated live proofs as deferred residual limitations.

## 7. Non-authorization

This return does not authorize production deployment, retention deletion, physical evidence deletion, new operational record families, governed-operation read model implementation, connector telemetry implementation, direct agent SQL, arbitrary SQL APIs, Observatory, Qdrant, Hermes/UI, provider work, customer data work, multi-user identity, production MCP packaging, vault mutation, broad schema redesign, or milestone closure by implication.
