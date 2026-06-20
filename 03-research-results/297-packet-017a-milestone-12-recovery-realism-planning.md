# Packet 017A — Milestone 12 Recovery Realism Planning

Status: draft planning packet
Date: 2026-06-20
Milestone: 12 — Recovery Realism and Operational Restore Proof
Packet: 017A
Implementation status: not authorized

## 1. Purpose

Define the planning scope for Milestone 12 without beginning implementation.

Packet 017A exists because Packet 016G completed the bounded Milestone 11 correction but preserved recovery-realism limitations that must not be hand-waved away.

## 2. Governing evidence

Packet 017A is grounded in:

```text
03-research-results/293-packet-016g-completion-audit.md
03-research-results/295-post-milestone-11-roadmap-revision-proposal.md
03-research-results/296-owner-acceptance-of-milestone-12-recovery-realism-direction.md
04-design-decisions/0019-milestone-10-system-of-record-reconciliation.md
04-design-decisions/0020-milestone-11-operational-foundation-boundaries.md
ROADMAP_V0.3.md
00-entrypoint/LLM_START_HERE.md
```

## 3. Current checkpoint

```text
Milestones 1-11: closed
Milestone 11 closure: Result 288
Packet 016G completion: Result 293
Milestone 12 direction accepted: Result 296
Platform HEAD: 00d86943ca54aaff89c4c8428b7bb529994f846c
Docs HEAD before Packet 017A draft: 3d1bad8e38550ab446481c0e4a96788d0fc60aaa
Vault HEAD: c898f261c0b341eb8419125247c8bd53ef567d6c
Operational database: kaizen_ops migrated through 0005_recovery_retention_integrity
```

## 4. Milestone 12 objective

Milestone 12 should prove Kaizen's recovery system under realistic, service-usable, nonzero-file conditions before adding new operational record families.

Milestone 12 should answer:

1. Can Kaizen create and retain a nonzero-file recovery generation with derived consistency evidence?
2. Can that generation be restored and used by typed operational services after restore?
3. What is the explicit restore-forward boundary from prior schema versions to current schema versions?
4. What is the explicit boundary between disposable integrity restore and operational role-ready restore?
5. Can recovery/retention immutability protections be proven through bounded negative probes without unsafe ad hoc SQL?
6. What runbook corrections are required before recovery is considered operationally credible?

## 5. Audit-to-step traceability matrix

| Source finding or limitation | Required Milestone 12 step | Acceptance evidence |
|---|---|---|
| Result 293: retained production generations are zero-file and pre-016G | Produce a nonzero-file recovery generation after 016G | Recovery generation row with `file_count > 0`, `total_file_bytes > 0`, derived `consistency_result`, package and manifest hashes |
| Result 293: durable restore smoke-check evidence deferred | Restore the nonzero-file generation and run a typed-service smoke check against the restored DB | Smoke-check output showing bounded typed service can read/write only an approved harmless test record family or perform a read-only service check, with no secret exposure |
| Result 293: restore-forward support deferred | Decide and test whether restore from an older generation can be migrated forward to current schema, or explicitly document unsupported boundary | Restore-forward test result or accepted deferral decision with rationale |
| Result 293: operational role-ready restore proof deferred | Decide whether Milestone 12 proves operational role-ready restore or keeps disposable integrity restore only | Live/post-restore role inventory, grant evidence, service DSN usability proof, or explicit deferral decision |
| Result 293: controlled live negative update probes not executed | Add bounded tool/profile or test-only fixture to prove recovery/retention trigger rejection behavior | Controlled negative-probe output showing terminal recovery_generation and released retention_hold rewrites are rejected |
| Result 293: ruff unavailable in platform venv | Decide whether to install/adopt ruff for platform or document it as non-adopted for this milestone | Tooling decision and reproducible lint/compile/test output |
| Decision 0020: backup and recovery posture requires file/database consistency verification | Prove database-and-file generation matching with nonzero artifact file | Manifest, file archive, restore verification, and consistency inspection evidence |
| Decision 0020: hammer posture requires backup/restore, crash-boundary, stale-reference, idempotency coverage | Define exact hammer/focused test set for recovery realism | Test profile list and results |
| Result 295: avoid premature new families | Explicitly exclude governed-operation read model and connector telemetry from Milestone 12 | Packet scope/exclusion section and clean changed-path review |

## 6. Proposed Packet 017A deliverables

Packet 017A should produce:

1. a Milestone 12 specification or planning result;
2. a changed-path allowlist for any later implementation packet;
3. a live-data safety plan;
4. a no-secret handling plan;
5. a restore proof plan for nonzero-file evidence;
6. a typed-service smoke-check plan;
7. a restore-forward decision plan;
8. an operational-role restore decision plan;
9. a bounded negative-probe plan;
10. a test/hammer profile plan;
11. a completion-audit checklist;
12. a later exact owner approval template.

## 7. Candidate implementation slices for later approval

Packet 017A should evaluate these slices and recommend an order.

### Slice 1 — Nonzero-file recovery generation and restore proof

Create or use an approved harmless test artifact file, produce a recovery generation, restore it, and verify file/database consistency.

### Slice 2 — Typed-service smoke check

After restore, run a bounded typed-service proof that validates restored database usability without broad mutation or arbitrary SQL.

### Slice 3 — Restore-forward boundary

Determine whether an older generation can be restored and then migrated forward to the current schema version, or explicitly document the unsupported boundary.

### Slice 4 — Operational role-ready restore boundary

Determine whether the restored database should be operationally role-ready in Milestone 12 or remain disposable integrity proof only.

### Slice 5 — Negative-probe tooling/profile

Add or use a bounded test profile that proves terminal recovery rows and released retention holds cannot be rewritten.

### Slice 6 — Runbook and operator docs

Correct recovery/runbook instructions to reflect actual behavior, limits, and commands.

## 8. Required exclusions

Milestone 12 must exclude unless separately authorized:

```text
new operational record families
governed-operation read model implementation
connector telemetry implementation
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
```

## 9. Live safety requirements

Packet 017A must define how later implementation will avoid damaging live evidence.

At minimum:

1. no physical deletion;
2. no retention deletion execution;
3. no rewrite of existing recovery generations;
4. no rewrite of existing restore proofs;
5. no secret values in docs, command output, or tool payloads;
6. no raw DSN copied into docs or chat;
7. no production deployment;
8. restore tests use disposable restore databases or explicitly approved safe local test databases;
9. all DB mutation must go through accepted migration runner or typed service paths, not ad hoc SQL, unless a bounded test-only probe is explicitly approved.

## 10. Verification requirements

Later Milestone 12 implementation should not be accepted without:

```text
platform clean start and clean end
exact platform start commit
exact docs start commit
exact changed-path allowlist
migration/source hashes if any migration exists
nonzero-file generation proof
restore proof
post-restore consistency proof
typed-service smoke-check proof or explicit deferral
restore-forward proof or explicit deferral
operational role-ready proof or explicit deferral
bounded negative-probe proof
focused tests
full platform tests
collect-only count
compile profile
lint/tooling disposition
live DB evidence where applicable
completion audit
```

## 11. Proposed later implementation approval template

After Packet 017A is reviewed and accepted, later implementation approval should use exact language similar to:

```text
I approve Packet 017B implementation using platform starting commit <exact commit> and docs starting commit <exact commit>, with the changed-path allowlist and acceptance criteria in <exact Packet 017A/017B planning record>. I authorize implementation only within that scope. I do not authorize production deployment, retention deletion, physical evidence deletion, new operational record families, direct agent SQL, arbitrary SQL APIs, Observatory, Qdrant, Hermes/UI, provider work, or production MCP packaging.
```

## 12. Packet 017A completion criteria

Packet 017A planning is complete only when:

1. every Result 293 residual limitation is mapped to a step, deferral, or explicit non-goal;
2. the later implementation path is split into bounded slices;
3. safety and no-secret handling are explicit;
4. live DB/file boundaries are explicit;
5. a changed-path allowlist is proposed;
6. verification requirements are specific;
7. owner has not been asked to approve implementation prematurely.

## 13. Current recommendation

Accept this Packet 017A planning draft after review, then optionally send it to Claude for independent planning audit before any implementation packet is approved.
