# Packet 017B — Recovery Realism Implementation Task Packet

Status: draft implementation task packet
Date: 2026-06-20
Milestone: 12 — Recovery Realism and Operational Restore Proof
Packet: 017B
Implementation status: not authorized

## 1. Purpose

Prepare the bounded implementation task packet for Milestone 12 recovery-realism proof.

This task packet is not implementation approval. It exists so an independent reviewer can audit the exact scope before the owner approves any code, database, or evidence mutation.

## 2. Accepted planning basis

Packet 017B is grounded in:

```text
03-research-results/293-packet-016g-completion-audit.md
03-research-results/295-post-milestone-11-roadmap-revision-proposal.md
03-research-results/296-owner-acceptance-of-milestone-12-recovery-realism-direction.md
03-research-results/297-packet-017a-milestone-12-recovery-realism-planning.md
03-research-results/298-packet-017a-owner-acceptance.md
04-design-decisions/0019-milestone-10-system-of-record-reconciliation.md
04-design-decisions/0020-milestone-11-operational-foundation-boundaries.md
```

Accepted Packet 017A source SHA-256:

```text
083bcca6ed295d903c36b351813475cfc7a8ebbc40f6c728e9d659b10d4a54b8
```

## 3. Starting checkpoints

Required starting checkpoints for later implementation approval:

```text
platform start commit: 00d86943ca54aaff89c4c8428b7bb529994f846c
docs start commit before this task packet: a289cd5381015e1b69fa6b5a3a18d5d55a13e38f
live database: kaizen_ops migrated through 0005_recovery_retention_integrity
```

Before any implementation begins, the implementation agent must re-verify:

```text
platform clean at 00d86943ca54aaff89c4c8428b7bb529994f846c
docs clean at the then-current task-packet commit
kaizen_ops schema_version includes 0001 through 0005
POSTGRES password/DSN values are never pasted into docs or chat
```

## 4. Objective

Packet 017B must prove recovery realism without expanding Kaizen into new product surface.

Required objective:

```text
Produce accepted, durable evidence that Kaizen can create a nonzero-file recovery generation after Packet 016G, restore it, verify file/database consistency, and prove restored typed-service usability or explicitly record a bounded deferral.
```

## 5. Existing evidence and remaining gap

Existing test evidence already includes a live recovery integration path in `tests/test_ops_recovery_integration.py` that:

```text
creates a nonzero evidence artifact in a disposable test setup
creates two recovery generations
restores one generation to a disposable restore database
verifies restored_file_count == 1
verifies restored_file_bytes matches the payload
records a restore proof
runs inspect_consistency against restored files
```

That is useful but not enough for Milestone 12 acceptance because Result 293 preserved these gaps:

```text
retained production generations are still zero-file and pre-016G
durable restore smoke-check evidence is deferred
restore-forward support is deferred
operational role-ready restore proof is deferred
controlled live negative update probes were not executed through bounded tooling
ruff/tooling disposition remains unresolved
```

Packet 017B should reuse and extend existing recovery integration structure where safe. It should not redesign the recovery system.

## 6. Required changed-path allowlist

Later implementation may modify only these platform paths unless Packet 017B is revised and re-approved:

```text
src/kaizen/ops_recovery/backup.py
src/kaizen/ops_recovery/restore.py
src/kaizen/ops_recovery/consistency.py
src/kaizen/ops_recovery/contracts.py
src/kaizen/ops_recovery/generation.py
src/kaizen/ops_recovery/retention.py
src/kaizen/ops_db/schema_contract.py
src/kaizen/ops_db/migrations/manifest.json
tests/test_ops_recovery_integration.py
tests/test_ops_recovery_restore.py
tests/test_ops_recovery_hammer.py
tests/test_ops_recovery_backup.py
tests/test_ops_recovery_retention.py
tests/test_ops_db_migration_hammer.py
tests/test_ops_db_migration_runner.py
tests/test_ops_service_integration.py
tests/test_ops_service_file_integration.py
```

Optional new platform files if needed:

```text
tests/test_ops_recovery_realism.py
tests/test_ops_recovery_negative_probes.py
tests/test_ops_recovery_smoke.py
```

Docs paths allowed:

```text
03-research-results/299-packet-017b-recovery-realism-implementation-task-packet.md
03-research-results/<next-id>-packet-017b-implementation-return.md
03-research-results/<next-id>-packet-017b-completion-audit.md
05-specs/milestone-12-recovery-realism-and-operational-restore-proof.md
```

Runbook/spec path allowed only if Packet 017B implementation actually updates operator guidance:

```text
05-specs/milestone-12-recovery-realism-and-operational-restore-proof.md
```

No vault paths are authorized.

No existing migration files 0001 through 0005 may be edited. A new migration is not expected for Packet 017B. If a migration becomes necessary, stop and revise the packet before implementation.

## 7. Required implementation steps

### Step 1 — Live post-016G recovery preflight

Verify:

```text
kaizen_ops schema_version has 0001 through 0005
0005 source_sha256 is a2cd249ca2ca4ec1dba7b244624647987cd4635bf0d4976233405fcb5f6a95bf
existing retained recovery generations are present
platform repo is clean
no secrets are emitted
```

### Step 2 — Nonzero-file recovery generation proof

Produce a nonzero-file recovery generation after 016G.

The proof must show:

```text
recovery_generation_id
generation created after 016G
status
file_count > 0
total_file_bytes > 0
consistency_result derived by inspection
encrypted_package_sha256
manifest_sha256
source_schema_version
migration_set_sha256
```

Preferred implementation posture:

- use a harmless local test project/storage artifact already compatible with the operational schema;
- do not mutate existing retained generations;
- do not physically delete existing evidence;
- use idempotency keys and bounded test data;
- record evidence in an implementation return rather than treating console output as memory.

### Step 3 — Restore proof

Restore the nonzero-file generation into a disposable restore database or explicitly approved safe local restore database.

The proof must show:

```text
restore_proof_id
restored_file_count > 0
restored_file_bytes > 0
restore status
restored artifact hash/length verification
post-restore inspect_consistency result
```

### Step 4 — Typed-service smoke check

After restore, run a bounded typed-service smoke check.

Preferred check:

```text
initialize OpsService against the restored database with a service-scoped DSN
perform one harmless read-only or reversible typed-service call
prove no arbitrary SQL path is used
prove no raw DSN or password appears in output
```

If write is needed, it must be:

- harmless;
- scoped to disposable restore DB only;
- performed through typed service code;
- documented as test evidence, not production mutation.

### Step 5 — Restore-forward boundary

Determine whether Packet 017B proves restore-forward.

Allowed outcomes:

```text
implemented and proven: older accepted generation can restore and migrate forward to current schema
explicitly deferred: restore-forward remains outside Milestone 12 with rationale
explicitly unsupported: current recovery contract supports same-migration-set restore only
```

The implementation return must choose one. Silence is not acceptable.

### Step 6 — Operational role-ready restore boundary

Determine whether Packet 017B proves operational role-ready restore.

Allowed outcomes:

```text
implemented and proven: restored DB roles/grants/service DSN work as operational restore
explicitly deferred: restore remains disposable integrity proof in Milestone 12
explicitly unsupported: operational restore requires later packet
```

The implementation return must choose one. Silence is not acceptable.

### Step 7 — Bounded negative-probe proof

Prove recovery/retention immutability through bounded tests or an approved test-only probe profile.

Required proof targets:

```text
terminal recovery_generation rewrite rejected
released retention_hold rewrite rejected
```

This must not use arbitrary ad hoc SQL against live `kaizen_ops` unless a bounded test-only profile is explicitly approved. Prefer disposable test DB or existing migration hammer patterns.

### Step 8 — Tooling/lint disposition

Resolve the `ruff` posture from Result 293.

Allowed outcomes:

```text
ruff adopted and reproducibly runnable
ruff explicitly not adopted for this milestone, with compile/full-test coverage retained
ruff deferred to later tooling hygiene packet
```

The implementation return must choose one.

### Step 9 — Runbook/spec correction

If implementation changes operator procedure or clarifies recovery posture, create/update:

```text
05-specs/milestone-12-recovery-realism-and-operational-restore-proof.md
```

The spec must include:

```text
what recovery proof covers
what restore proof covers
what remains disposable-only
what remains unsupported or deferred
safe command patterns without secrets
completion checklist
```

## 8. Required exclusions

Packet 017B must not implement:

```text
new operational record families
governed-operation read model
connector invocation telemetry
production deployment
retention deletion execution
physical evidence deletion
Observatory implementation
Qdrant
Hermes/UI work
provider purchases
customer data
multi-user identity
production MCP packaging
agent direct SQL
arbitrary SQL execution APIs
broad schema redesign
changes to migrations 0001 through 0005
vault mutation
```

## 9. Verification requirements

Later implementation must run or explicitly account for:

```text
go8.git_remote_status(root="platform")
go8.pg_read_evidence_rows(database="kaizen_ops", schema_name="platform_meta", table="schema_version")
go8.pg_read_evidence_rows(database="kaizen_ops", schema_name="operations", table="recovery_generation")
go8.pg_read_evidence_rows(database="kaizen_ops", schema_name="operations", table="restore_proof")
go8.pg_list_triggers(database="kaizen_ops", schema_name="operations", table="recovery_generation")
go8.pg_list_triggers(database="kaizen_ops", schema_name="operations", table="retention_hold")
python -m pytest --collect-only -q
go8.run_test_profile(profile="platform_compile")
go8.run_test_profile(profile="platform_full")
focused recovery realism tests
focused restore tests
focused negative-probe tests
```

If live credential-gated tests skip, the implementation return must say exactly which environment variables or tools were missing and what proof remains unperformed.

## 10. Acceptance criteria

Packet 017B implementation is acceptable only if:

1. it starts from the exact approved platform and docs commits;
2. all changed paths are inside the allowlist;
3. no 0001 through 0005 migration files are edited;
4. a nonzero-file post-016G recovery generation is proven;
5. a nonzero-file restore proof is proven;
6. post-restore consistency is proven;
7. typed-service smoke check is proven or explicitly deferred with owner-visible rationale;
8. restore-forward is proven, explicitly unsupported, or explicitly deferred;
9. operational role-ready restore is proven, explicitly unsupported, or explicitly deferred;
10. recovery/retention negative probes are proven through bounded tests/profile or explicitly blocked by missing approved tool surface;
11. ruff/tooling posture is resolved or explicitly deferred;
12. no secrets, passwords, raw DSNs, or private payloads appear in docs or outputs;
13. full platform tests pass or failures are bounded and owner-reviewed;
14. completion audit records exact hashes, evidence rows, test results, and residual limitations.

## 11. Implementation return requirements

The implementation return must include:

```text
platform start commit
platform end commit
docs start commit
changed paths
new/changed file hashes
nonzero recovery_generation evidence
restore_proof evidence
typed-service smoke result or explicit deferral
restore-forward decision
operational role-ready restore decision
negative-probe result
ruff/tooling disposition
test results
collect-only count
live DB evidence
residual limitations
```

## 12. Claude review prompt

Before owner implementation approval, send this packet to Claude or another independent reviewer with:

```text
Review Packet 017B as a planning/task packet only. Do not implement.

Check whether every required step traces to Result 293, Result 295, Result 297, Decision 0019, or Decision 0020.

Check whether the changed-path allowlist is tight enough.

Check whether live DB, no-secret, restore, typed-service, restore-forward, operational-role, negative-probe, and tooling requirements are specific and testable.

Check whether the packet accidentally authorizes new operational families, production deployment, retention deletion, physical deletion, Observatory, Qdrant, Hermes/UI, provider work, direct agent SQL, arbitrary SQL APIs, or vault mutation.

Return:
1. approve/revise recommendation;
2. must-fix issues;
3. nice-to-have refinements;
4. approval-readiness statement.
```

## 13. Owner implementation approval template

Only after independent review or owner review, implementation may be approved with exact language like:

```text
I approve Packet 017B implementation using platform starting commit `00d86943ca54aaff89c4c8428b7bb529994f846c` and docs starting commit `<current docs commit after task-packet acceptance>`, with the changed-path allowlist and acceptance criteria in `03-research-results/299-packet-017b-recovery-realism-implementation-task-packet.md`. I authorize implementation only within that scope. I do not authorize production deployment, retention deletion, physical evidence deletion, new operational record families, governed-operation read model implementation, connector telemetry implementation, direct agent SQL, arbitrary SQL APIs, Observatory, Qdrant, Hermes/UI, provider work, customer data work, multi-user identity, production MCP packaging, vault mutation, or broad schema redesign.
```

## 14. Current status

Packet 017B is prepared for review only.

Implementation remains blocked.
