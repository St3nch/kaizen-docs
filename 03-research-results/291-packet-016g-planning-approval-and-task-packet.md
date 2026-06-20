# Packet 016G Planning Approval and Task Packet

Status: accepted for task-packet preparation; implementation not authorized
Milestone: 11 — Operational Data Foundation
Packet: 016G — Milestone 11 Attestation-Integrity and Recovery-Immutability Correction
Date: 2026-06-20
Docs starting commit: `d991d78d3de548ee0511f3da21741306c5f06ea4`
Platform starting commit: `c7bf918a9369617c9592171d84d7df71e286cde2`

## 1. Owner authorization recorded

The owner accepted the independent post-closure audit of Milestone 11, including Passes 1–9 and the final synthesis.

Milestone 11 remains closed under Result 288.

The owner authorized Packet 016G planning for:

- E001/G001: derive recovery generation `consistency_result` from an actual `inspect_consistency` call and add semantic tests.
- F001/G002: correct Class B retention basis and tests.
- H001: add `recovery_generation` legal-transition / immutability protection and narrow grants where appropriate.
- B004/F004: add `retention_hold` terminal-release protection and narrow grants where appropriate.
- B003/G003: update schema contract/test coverage for the full 0004 schema.
- H002: reconcile and document M11 test-count accounting.
- E003/A007, A008, and E002/B008 as explicit correction-or-deferral decision items.

All other findings are formally deferred unless directly touched by these corrections.

This authorization does not reopen Milestone 11 and does not authorize production deployment, HA/PITR/replication, cloud/removable automation, Observatory or Internet Marketing Intelligence work, Qdrant/vector work, agent direct SQL, MCP/network adapters, customer data, multi-user identity, retention deletion execution, physical deletion, next-milestone work, or major schema redesign.

No implementation begins until the 016G task packet is reviewed and separately approved with exact hash-bound evidence.

## 2. Evidence basis

This record is based on:

1. Result 288, which closed Milestone 11 under owner acceptance.
2. Results 289 and 290, which recorded the early independent audit status and strategic commentary.
3. Owner-provided independent audit Passes 4–9 and final synthesis, accepted by the owner in chat.
4. Read-only Go8 verification of the live project posture before this record was written.

Verified current posture before writing this record:

```text
Go8 version: 0.5.0
Go8 generic_execution: false
Kaizen MCP version: 0.1.0
Kaizen MCP mutations_enabled: true
Kaizen MCP remote_enabled: false

docs branch: main
docs HEAD: d991d78d3de548ee0511f3da21741306c5f06ea4
docs clean: true
docs upstream: origin/main
docs ahead/behind: 0/0

platform branch: main
platform HEAD: c7bf918a9369617c9592171d84d7df71e286cde2
platform clean: true
platform remotes: none
```

## 3. Packet objective

Packet 016G corrects the bounded Milestone 11 audit findings where recovery/retention attestations assert more than the code proves, and where recovery/retention integrity anchors remain mutable without database-layer backstops.

The packet must:

1. Derive recovery generation `consistency_result` from an actual `inspect_consistency` call.
2. Correct Class B retention basis to match retirement plus 12 months.
3. Add database-layer `recovery_generation` legal-transition / immutability protection.
4. Add database-layer `retention_hold` terminal-release protection.
5. Update schema contract/test coverage for the full 0004 schema.
6. Reconcile M11 test-count accounting.
7. Explicitly implement or defer E003/A007, A008, and E002/B008 decision items.

Milestone 11 remains closed. This packet is corrective hardening, not milestone reopening.

## 4. Two-tier structure

Tier 1 — clear correctness, lower risk:

- E001/G001
- F001/G002
- B003/G003
- H002

Tier 2 — database-layer safety, closer migration review:

- H001
- B004/F004

Both tiers are inside one packet, but Tier 2 requires explicit trigger/grant review before implementation approval.

## 5. Exact changed-path allowlist

Platform paths allowed:

```text
src/kaizen/ops_recovery/backup.py
src/kaizen/ops_recovery/consistency.py
src/kaizen/ops_recovery/retention.py
src/kaizen/ops_db/schema_contract.py
src/kaizen/ops_db/migrations/0005_recovery_retention_integrity.sql
src/kaizen/ops_db/migrations/manifest.json
tests/test_ops_recovery_generation.py
tests/test_ops_recovery_hammer.py
tests/test_ops_recovery_integration.py
tests/test_ops_recovery_backup.py
tests/test_ops_db_schema_contract.py
tests/test_ops_db_migration_runner.py
tests/test_ops_db_migration_hammer.py
```

Optional new platform test files allowed only if cleaner than overloading existing files:

```text
tests/test_ops_recovery_consistency.py
tests/test_ops_recovery_retention.py
```

Docs paths allowed:

```text
03-research-results/291-packet-016g-planning-approval-and-task-packet.md
03-research-results/<next-id>-packet-016g-implementation-return.md
03-research-results/<next-id>-packet-016g-completion-audit.md
```

No other paths are authorized.

## 6. Current source hashes for optimistic binding

Platform source files:

```text
src/kaizen/ops_recovery/backup.py
sha256: 2c4ae6d1473c36b5e178c926efcc733449c3a508f81ac76cb9968d9ef4a5e17f

src/kaizen/ops_recovery/consistency.py
sha256: 5f4e13a75225a9e478eb39c7123f62823be725d4fd93e06e93c3b92ac52769f9

src/kaizen/ops_recovery/retention.py
sha256: 14edf6ebc591c2939f3d2dfdbd3ce9039a03df46bcd5de5a06ba2696ed912471

src/kaizen/ops_db/schema_contract.py
sha256: d479830fd295921935a39432b1b3c6d869ca24113316c052db46c34f56a0e240

src/kaizen/ops_db/migrations/manifest.json
sha256: a50470e8da5f4a5cf325c3811bb91e6d8ff608c4e84c571e3a25a58676029659
```

Platform test files:

```text
tests/test_ops_recovery_backup.py
sha256: 73053468691338a09fba4b3ba8a0c840ae8f7a4bc4e75590a1e68bdd832248bc

tests/test_ops_recovery_generation.py
sha256: 5369d2cffedc5d68fc4528b71f36ae060e37ed2577a995b2a858ac11170c974b

tests/test_ops_recovery_hammer.py
sha256: b8e2ed7e170d910b4c399a828d8302308fc4dc1aa0436fd19b17398069069bf0

tests/test_ops_recovery_integration.py
sha256: 19a508675a2001d8c490300a1cf30122670f44e8c2415d9dcbd784363a0d28ec

tests/test_ops_db_schema_contract.py
sha256: bc599d3ee90ea996759d9cc79f527171d7de9ead22bece638ef767ea91c48cba

tests/test_ops_db_migration_runner.py
sha256: 8867a193a809a09e61c9f6464f400b8160a87e03ad66b90d27fc065a9bf7b961

tests/test_ops_db_migration_hammer.py
sha256: 9decd60bea5207e6bedee874b4312a081eae55e4c92b5f0d662eb42209a5d607
```

New migration file:

```text
src/kaizen/ops_db/migrations/0005_recovery_retention_integrity.sql
sha256: TBD after file creation
```

New optional tests:

```text
tests/test_ops_recovery_consistency.py
sha256: TBD if created

tests/test_ops_recovery_retention.py
sha256: TBD if created
```

## 7. Current source defects confirmed before implementation

### E001/G001

`src/kaizen/ops_recovery/backup.py` currently hardcodes `consistency_check_result="passed"` and inserts `consistency_result = 'passed'` without calling `inspect_consistency`.

### F001/G002

`src/kaizen/ops_recovery/retention.py` currently computes Class B using `cutoff = as_of - timedelta(days=365)` and `execution_attempt.created_at <= cutoff`, while labeling the item `expiry_basis="project retired plus 12 months"`.

### B003/G003

`src/kaizen/ops_db/schema_contract.py` currently omits the six 0004 recovery/retention tables from `EXPECTED_TABLES`:

- `recovery_generation`
- `recovery_generation_object`
- `restore_proof`
- `retention_hold`
- `retention_plan`
- `retention_plan_item`

### H001 / B004-F004

`0004_recovery_and_retention_planning.sql` currently grants update authority on `operations.recovery_generation` status/hash/consistency columns and on `operations.retention_hold` release columns. No immutable/legal-transition trigger exists on `operations.recovery_generation`, and no terminal-release trigger exists on `operations.retention_hold`.

## 8. Required implementation details

### 8.1 E001/G001 — recovery consistency attestation

Implementation must:

1. Import/use `inspect_consistency` from `ops_recovery.consistency`.
2. Run it during the generation path before final manifest and database insertion.
3. Derive `consistency_result = "passed"` only when findings are empty.
4. Derive `consistency_result = "failed"` when findings are non-empty.
5. Ensure the generation manifest records the derived result.
6. Ensure the metadata row records the same derived result.
7. Stop using a literal `"passed"` as the source of truth.
8. Ensure inconsistent state cannot produce `status='verified'` with `consistency_result='passed'`.

Implementation must not weaken existing package/hash/file checks.

### 8.2 F001/G002 — Class B retention basis

Implementation must:

1. Use `project_ref.retired_at` as the Class B basis.
2. Require non-null `retired_at`.
3. Compute expiry as retirement plus 12 months.
4. Include Class B candidates only when `as_of >= retired_at + 12 months`.
5. Ensure `expiry_basis` truthfully describes the actual computation.
6. Add false-include and false-exclude tests.

Do not implement retention deletion execution.

### 8.3 H001 — recovery_generation transition / immutability

Migration 0005 must add DB-layer protection for `operations.recovery_generation`.

Required trigger behavior:

1. Reject all DELETE.
2. Reject all UPDATE when `OLD.status IN ('verified', 'failed')`.
3. Permit only a single legal transition from `creating` to `verified` or `creating` to `failed`.
4. During legal transition, immutable identity fields must not change:
   - `recovery_generation_id`
   - `created_at`
   - `source_database_identity`
   - `source_schema_version`
   - `migration_set_sha256`
   - `idempotency_key`
   - `request_digest`
5. During `creating -> verified`, required terminal anchors must be non-null and valid:
   - `encrypted_package_sha256`
   - `manifest_sha256`
   - `consistency_result`
6. During `creating -> failed`, package/hash anchors may remain null, but `consistency_result` must be `failed`.
7. `consistency_result` must be constrained by CHECK to `passed` or `failed`.
8. No post-terminal rewrite of status, consistency, hash, count, identity, or source fields is allowed.
9. Grant narrowing must revoke direct update surface where no longer needed, or retain only the minimum columns required for legal pre-terminal transition.

Implementation must verify existing retained rows remain valid.

### 8.4 B004/F004 — retention_hold terminal-release protection

Migration 0005 must add DB-layer protection for `operations.retention_hold`.

Required trigger behavior:

1. No DELETE grants may be added.
2. Permit first release only when old release fields are null and new release fields are non-null.
3. Reject release twice, un-release, release timestamp rewrite, release actor rewrite, and creation/scope field changes.
4. Freeze:
   - `retention_hold_id`
   - `project_id`
   - `hold_scope`
   - `hold_scope_id`
   - `reason`
   - `created_at`
   - `created_by_actor_id`
5. Grant narrowing must leave only the minimum release surface required, with the trigger enforcing first-write-only semantics.

### 8.5 B003/G003 — schema contract coverage

Implementation must:

1. Add the six 0004 tables to `EXPECTED_TABLES`.
2. Keep `schema_version` included.
3. Ensure tests fail if any 0004 recovery/retention table is omitted.
4. Prefer deriving or validating against the full migration set, not only 0001.

### 8.6 H002 — test-count accounting

Implementation/documentation must:

1. Produce an M11-scoped reproducible collect-only count.
2. Record exact command/tool.
3. Reconcile 394, 386, and 409.
4. Explain whether the mismatch was scoped-vs-total count, later test additions, duplicate/credential-gated counting, or documentation error.
5. Avoid replacing one unreproducible number with another.

### 8.7 E003/A007, A008, E002/B008 decision items

The implementation return must choose explicitly:

- implement now, or
- defer with rationale.

Default recommendation for 016G:

- Implement smoke-check evidence only if low-risk and directly coupled.
- Explicitly defer restore-forward and operational restore.
- Explicitly defer nonzero-file retained generation unless it can be proven without expanding scope.

## 9. Migration plan

New file:

```text
src/kaizen/ops_db/migrations/0005_recovery_retention_integrity.sql
```

Required properties:

```text
LF line endings
UTF-8
no BOM
SET LOCAL ROLE kaizen_ops_owner;
single forward migration
no edits to 0001-0004
manifest entry added only after final SHA is known
```

Required migration objects:

1. CHECK constraint for `operations.recovery_generation.consistency_result`.
2. Trigger function for `recovery_generation` transition / immutability.
3. Trigger on `operations.recovery_generation`.
4. Trigger function or shared function for `retention_hold` terminal-release protection.
5. Trigger on `operations.retention_hold`.
6. REVOKE/GRANT statements for narrowed update privileges.

Migration must not create new tables unless restore smoke evidence is explicitly implemented and approved.

## 10. Tests to add or modify

Required tests:

- E001/G001: generation path invokes consistency inspection; seeded inconsistent DB yields `consistency_result = failed` or fail-closed behavior; hardcoded `passed` would fail the test.
- F001/G002: retired 6 months ago plus old attempts yields no Class B candidate; retired 18 months ago plus recent attempts yields Class B candidate; `expiry_basis` matches actual computation.
- H001: legal terminal transition succeeds if needed; verified/failed row update is rejected; hash/manifest/consistency/status anchors cannot be rewritten after terminal.
- B004/F004: first release succeeds; re-release fails; un-release fails; release timestamp/actor rewrite fails; creation/scope fields cannot be changed.
- B003/G003: `EXPECTED_TABLES` contains all 21 live/schema tables; all six 0004 recovery/retention tables are required.
- Migration manifest: manifest includes 0005; 0005 SHA matches file; 0005 applies cleanly after 0001-0004.
- H002: reproducible M11-scoped count is recorded.

## 11. Live PostgreSQL verification plan

After implementation, use bounded Go8 tools only.

Required checks:

```text
go8.pg_health
go8.pg_server_isready
go8.pg_read_evidence_rows(database="kaizen_ops", schema_name="platform_meta", table="schema_version")
go8.pg_list_tables(database="kaizen_ops", schema_name="operations")
go8.pg_describe_table(database="kaizen_ops", schema_name="operations", table="recovery_generation")
go8.pg_describe_table(database="kaizen_ops", schema_name="operations", table="retention_hold")
go8.pg_list_triggers(database="kaizen_ops", schema_name="operations")
go8.pg_list_column_grants(database="kaizen_ops", schema_name="operations", table="recovery_generation", grantee="kaizen_ops_service")
go8.pg_list_column_grants(database="kaizen_ops", schema_name="operations", table="retention_hold", grantee="kaizen_ops_service")
go8.pg_read_evidence_rows(database="kaizen_ops", schema_name="operations", table="recovery_generation")
```

Required live assertions:

1. `schema_version` has 5 rows.
2. 0005 row SHA matches final 0005 file SHA.
3. `recovery_generation` trigger appears.
4. `retention_hold` trigger appears.
5. `consistency_result` CHECK is verified.
6. Existing retained generations remain present and valid.
7. Service grants are narrowed or justified.
8. Controlled negative checks prove terminal update rejection.
9. No DELETE grants appear.

If Go8 lacks a bounded tool for CHECK constraints or controlled negative checks, add bounded read-only/test-only tooling or use test suite output; do not use arbitrary SQL unless separately authorized.

## 12. Static verification plan

Required static checks:

```text
go8.sha256_file on 0005
go8.sha256_file on manifest.json
go8.search_text for hardcoded consistency "passed" source in backup.py
go8.search_text for inspect_consistency reference in backup.py
go8.search_text for expiry_basis in retention.py/tests
go8.search_text for recovery_generation trigger name in 0005
go8.search_text for retention_hold trigger name in 0005
go8.search_text for EXPECTED_TABLES additions
go8.git_diff_check
go8.run_python_module(root="platform", module="pytest", args=[...focused tests...])
go8.run_test_profile(profile="platform_full")
```

Required static assertions:

1. 0005 is LF/UTF-8/no-BOM.
2. 0005 SHA matches manifest.
3. No 0001-0004 migration file changed.
4. `backup.py` no longer uses literal `passed` as consistency source.
5. `retention.py` no longer uses `as_of - timedelta(days=365)` for Class B.
6. `EXPECTED_TABLES` includes all six 0004 tables.
7. No out-of-scope path changes.
8. Secret scan clean.
9. Platform full tests pass.

## 13. Documentation/result updates

Required docs result from this task-packet preparation:

```text
03-research-results/291-packet-016g-planning-approval-and-task-packet.md
```

Required implementation return later:

```text
03-research-results/<next-id>-packet-016g-implementation-return.md
```

Required completion audit later:

```text
03-research-results/<next-id>-packet-016g-completion-audit.md
```

## 14. Risks

1. Grant narrowing could break a legitimate lifecycle path if code relies on UPDATE instead of INSERT.
2. Trigger overreach could block legal `creating -> failed`.
3. Existing verified rows must remain valid under new CHECKs/triggers.
4. The generation path may need to preserve failed evidence without producing a false verified generation.
5. Retention Class B behavior change could alter future dry-run candidates.
6. H002 count reconciliation could expose scoped-vs-total ambiguity; document rather than overfit.
7. Restore smoke/nonzero-file proof could expand scope; must be explicit implement-or-defer.

## 15. Rollback / recovery

1. 0005 is forward-only.
2. Do not edit 0001-0004.
3. If 0005 is wrong after merge, fix with a later migration, not by rewriting history.
4. Failed migration must roll back transactionally under the existing runner.
5. Any implementation failure must leave platform repo and database in a classifiable state.
6. No live retention deletion or physical deletion is permitted.
7. Restore-forward limitations remain documented unless explicitly corrected.

## 16. Acceptance criteria

Packet 016G is acceptable only if all hold:

1. Platform starts from commit `c7bf918a9369617c9592171d84d7df71e286cde2`.
2. Docs start from commit `d991d78d3de548ee0511f3da21741306c5f06ea4`.
3. Changed paths are within the allowlist.
4. Generation path derives `consistency_result` from real inspection.
5. Seeded inconsistent state cannot produce `verified + passed`.
6. Class B uses retirement plus 12 months and tests prove false-include/false-exclude boundaries.
7. `recovery_generation` terminal anchors cannot be rewritten.
8. `retention_hold` released rows cannot be rewritten.
9. Full 0004 schema is covered by schema contract tests.
10. M11 test count is reproducible and reconciled.
11. E003/A007, A008, E002/B008 are explicitly implemented or deferred.
12. New migration 0005 applies cleanly and is recorded in `schema_version`.
13. Full platform tests pass.
14. Live DB verification confirms triggers/grants/evidence rows.
15. No excluded capability is introduced.
16. No 0001-0004 migration files are changed.
17. No implementation begins until owner approves implementation from this packet.

## 17. Implementation approval language

Owner may approve implementation with:

> I approve Packet 016G implementation using platform starting commit `c7bf918a9369617c9592171d84d7df71e286cde2` and docs starting commit `d991d78d3de548ee0511f3da21741306c5f06ea4`, with the changed-path allowlist and acceptance criteria in `03-research-results/291-packet-016g-planning-approval-and-task-packet.md`. I authorize implementation only within that scope. I do not authorize milestone reopening, production deployment, retention deletion, physical deletion, direct agent SQL, MCP/network adapters, next-milestone work, or any excluded capability.

## 18. Stop condition

This record authorizes task-packet preparation only. Implementation remains blocked until owner approval is given using the implementation approval language above or equivalent exact authorization.
