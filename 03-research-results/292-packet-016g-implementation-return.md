# Packet 016G Implementation Return

Status: source implementation committed; live database migration application pending
Milestone: 11 — Operational Data Foundation
Packet: 016G — Milestone 11 Attestation-Integrity and Recovery-Immutability Correction
Date: 2026-06-20
Docs starting commit: `3fec0b8338d10e8dcc00b2f0e1ca496009c1dd15`
Platform starting commit: `c7bf918a9369617c9592171d84d7df71e286cde2`
Platform implementation commit: `00d86943ca54aaff89c4c8428b7bb529994f846c`

## 1. Scope statement

Packet 016G implementation was authorized from Result 291 with Milestone 11 remaining closed under Result 288.

Implementation stayed within the authorized changed-path allowlist. No milestone reopening, production deployment, retention deletion, physical deletion, direct agent SQL, MCP/network adapter work, next-milestone work, or excluded capability was performed.

## 2. Changed paths

Platform paths changed in commit `00d86943ca54aaff89c4c8428b7bb529994f846c`:

```text
src/kaizen/ops_db/migrations/0005_recovery_retention_integrity.sql
src/kaizen/ops_db/migrations/manifest.json
src/kaizen/ops_db/schema_contract.py
src/kaizen/ops_recovery/backup.py
src/kaizen/ops_recovery/retention.py
tests/test_ops_db_migration_hammer.py
tests/test_ops_db_migration_runner.py
tests/test_ops_db_schema_contract.py
tests/test_ops_recovery_integration.py
tests/test_ops_recovery_retention.py
```

No 0001–0004 migration files were changed.

## 3. Implemented corrections

### E001/G001

`src/kaizen/ops_recovery/backup.py` now derives `consistency_result` from `inspect_consistency` through an all-project consistency inspection helper. The generation manifest records the derived value. The idempotent metadata row records `verified` only when the derived value is `passed`; otherwise it records `failed`.

A new integration test covers an inconsistent file state and asserts that the metadata row is recorded as `status='failed'` and `consistency_result='failed'` rather than `verified + passed`.

### F001/G002

`src/kaizen/ops_recovery/retention.py` now gates Class B retention on `project_ref.retired_at + 12 months` instead of `execution_attempt.created_at <= as_of - 365 days`. It requires non-null `retired_at` and preserves the `project retired plus 12 months` expiry basis as the now-truthful computation label.

New retention tests cover both the false-include and false-exclude cases:

- retired 6 months ago plus old attempt yields no Class B candidate.
- retired 18 months ago plus recent attempt yields a Class B candidate.

### H001

New migration `0005_recovery_retention_integrity.sql` adds:

- `recovery_generation_consistency_result_check`
- `validate_recovery_generation_update()`
- `recovery_generation_transition_guard`

The trigger rejects terminal-row mutation and allows only controlled `creating -> verified` or `creating -> failed` transitions. Existing source behavior inserts terminal rows directly; the trigger is therefore a DB-layer guard for future transitions and post-terminal rewrite prevention.

The migration revokes the prior service UPDATE grant on `operations.recovery_generation` status/hash/consistency columns.

### B004/F004

New migration `0005_recovery_retention_integrity.sql` adds:

- `validate_retention_hold_update()`
- `retention_hold_release_guard`

The trigger permits only first release and rejects release rewrites, un-release, and creation/scope field mutation. The service role retains only the release-column UPDATE surface needed for first release, with the trigger enforcing first-write-only semantics.

### B003/G003

`src/kaizen/ops_db/schema_contract.py` now includes the six 0004 recovery/retention tables:

```text
recovery_generation
recovery_generation_object
restore_proof
retention_hold
retention_plan
retention_plan_item
```

Schema contract tests now read the full 0001–0005 migration set and explicitly assert that the six 0004 tables are required.

### H002

A post-016G collect-only run produced 413 collected tests. This reconciles the prior 409 current-tree count as follows: 409 was the pre-016G total collect-only count; 016G added four test cases, giving 413. The earlier closure figures 394 and 386 remain historical/scoping/accounting figures and should not be reused as current total-tree counts.

### E003/A007, A008, E002/B008 decision items

These were explicitly deferred in this implementation return:

- post-restore typed-service smoke-check durable evidence remains deferred,
- nonzero-file retained production generation proof remains deferred,
- restore-forward support remains deferred,
- disposable-vs-operational restore boundary remains the documented Milestone 11 posture.

The deferral is explicit and does not silently convert these into completed evidence.

## 4. Migration metadata

New migration file:

```text
src/kaizen/ops_db/migrations/0005_recovery_retention_integrity.sql
```

New migration SHA-256:

```text
a2cd249ca2ca4ec1dba7b244624647987cd4635bf0d4976233405fcb5f6a95bf
```

`manifest.json` includes migration id:

```text
0005_recovery_retention_integrity
```

## 5. Verification performed

### Static and unit verification

Focused pytest run:

```text
python -m pytest -q tests/test_ops_db_migration_runner.py tests/test_ops_db_schema_contract.py tests/test_ops_recovery_retention.py tests/test_ops_recovery_integration.py
```

Result:

```text
8 passed, 4 skipped
```

Migration hammer focused run:

```text
python -m pytest -q tests/test_ops_db_migration_hammer.py
```

Result:

```text
2 passed, 1 skipped
```

Compile profile:

```text
go8.run_test_profile(profile="platform_compile")
```

Result:

```text
returncode: 0
```

Full platform test profile:

```text
go8.run_test_profile(profile="platform_full")
```

Result:

```text
379 passed, 34 skipped
```

Collect-only count:

```text
python -m pytest --collect-only -q
```

Result:

```text
413 tests collected
```

Static checks:

- `git diff --check`: passed.
- `src/kaizen/ops_recovery/backup.py` no longer contains `consistency_check_result="passed"`.
- `src/kaizen/ops_recovery/retention.py` no longer contains `timedelta` Class-B cutoff logic.
- 0001–0004 migration hashes remain unchanged.
- 0005 migration hash matches the manifest entry.

### Lint limitation

The `platform_ruff` profile could not run because the platform virtual environment does not have `ruff` installed:

```text
No module named ruff
```

This is recorded as an environment/tooling limitation, not a passing lint result.

## 6. Live database verification status

Read-only live PostgreSQL posture was checked:

```text
PostgreSQL: 18.4
host: 127.0.0.1
port: 5432
server ready: true
```

Live `kaizen_ops.platform_meta.schema_version` remained at four rows at the time of this return:

```text
0001_initial_operations
0002_execution_verification_service
0003_file_provenance_and_manifest_service
0004_recovery_and_retention_planning
```

The live 0005 migration was not applied by GPT/Go8 in this pass because the exposed bounded tools do not provide the platform migration runner. The available `pg_run_migration_file` tool can execute a SQL file, but using it would bypass the platform migration runner's `platform_meta.schema_version` recording. That would violate the 016G acceptance criteria requiring a version-5 schema_version row.

Therefore live migration application and post-apply verification remain pending and must be performed with the platform migration runner or another approved bounded tool that preserves `schema_version` semantics.

## 7. Manual live-apply command for owner/operator

Run only if the operator has the correct `KAIZEN_OPS_DSN` configured for `kaizen_ops` and intends to apply 0005 live:

```powershell
Set-Location C:\dev\kaizen\platform
$env:KAIZEN_OPS_DSN = '<owner-supplied kaizen_ops DSN; do not paste into chat>'
.\.venv\Scripts\python.exe -m kaizen.ops_db.migration_runner apply --application-version packet-016g
```

After application, verify:

```powershell
.\.venv\Scripts\python.exe -m kaizen.ops_db.migration_runner status
```

Expected result: 0001–0005 applied with 0005 source SHA `a2cd249ca2ca4ec1dba7b244624647987cd4635bf0d4976233405fcb5f6a95bf`.

## 8. Completion status

Source implementation is committed at platform commit `00d86943ca54aaff89c4c8428b7bb529994f846c`.

Packet 016G is not yet completion-accepted because live migration application and post-apply live DB verification remain pending.

## 9. Next required step

Apply migration 0005 through the platform migration runner, then run the live PostgreSQL verification set from Result 291:

- schema_version row count and 0005 SHA,
- trigger inventory,
- grants on `recovery_generation`,
- grants on `retention_hold`,
- existing recovery generation rows still valid,
- controlled negative checks if an approved bounded tool/profile is available.

Only after that should a Packet 016G completion audit be written.
