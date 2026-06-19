---
id: kz-result-01KVBTM11AUDITP3STATUS
type: research-result
status: complete
project: kaizen-platform
created: 2026-06-18T00:00:00Z
updated: 2026-06-18T00:00:00Z
review_status: pending-owner-review
authority: post-closure-audit-status
summary: "Document the independent Milestone 11 audit status after completed Passes 1 through 3."
---

# Research Result 289 - Milestone 11 Independent Audit Status After Pass 3

## Purpose

This result freezes the independent post-closure audit status for Milestone 11 after completion of audit Passes 1 through 3.

It is a documentation and continuity record only. It does not reopen or re-close Milestone 11, approve a correction packet, authorize implementation, authorize live database mutation, or supersede Result 288.

## Scope covered so far

```text
Pass 1: Governance, authority, requirements, packets, results, and closure claims
Pass 2: Platform checkpoint, packet commit diffs, and path-scope verification
Pass 3: PostgreSQL schema, migration, manifest, constraint, trigger, ownership, and privilege audit
```

The audit is not final. No Milestone 11 technical verdict has been issued by the independent audit.

## Current authoritative baseline

Milestone 11 remains closed by Result 288 unless and until the owner explicitly reopens it or approves a correction packet.

```text
closure record:
03-research-results/288-packet-016f-owner-completion-acceptance-and-milestone-11-closure.md

closure record SHA-256:
2710261d8381321fbd1af621f26968b5cc23d3628d8730926d460f8e3e09f6ff

docs closure commit:
3bbde9c31025a6215455904dc516fadb16874ac8

platform closure commit:
c7bf918a9369617c9592171d84d7df71e286cde2
```

## Audit method so far

The independent audit is being performed in passes.

The external auditor has basic filesystem and source-reading tools, but does not have direct access to local-only platform Git operations, live PostgreSQL inspection, or Go8. The local platform repository intentionally has no remote.

The operating pattern is therefore:

```text
Claude / external auditor:
read public docs and owner-provided source/evidence; perform independent analysis

Go8 / local owner environment:
extract bounded local evidence from platform and live systems when needed

Project steward:
route evidence, preserve findings, and document audit status without overstating proof
```

This separation is intentional and should continue until a dedicated audit-bundle export tool exists.

## Pass 1 summary - governance and closure-chain audit

Pass 1 verified that the public `kaizen-docs` repository was available at the closure commit after the delayed push was corrected.

```text
docs HEAD:
3bbde9c31025a6215455904dc516fadb16874ac8

subject:
Accept Packet 016F and close Milestone 11

branch:
main

closure tail:
016D through 016F plus Results 267 through 288 present
```

Pass 1 found the Milestone 11 governance chain complete and internally hash-consistent:

```text
Decision 0019
-> Decision 0020
-> Milestone 11 specifications
-> Packets 016A through 016F
-> owner approvals
-> implementation returns
-> steward audits
-> owner acceptances
-> Result 288 closure
```

Pass 1 did not issue a final verdict. It classified closure claims into:

```text
independently verified from docs
internally consistent but owner-reported
requires platform-source verification
requires test-body verification
requires live-system verification
contradicted
unsupported
```

### Pass 1 key strengths

- Exact-hash governed packet workflow is intact across Milestone 11.
- The 016D scope-correction process worked as intended: implementation stopped on a scope mismatch, a revised packet was approved, and work resumed only afterward.
- Decision 0020 and the Milestone 11 specifications preserve system-of-record boundaries.
- Result 288 explicitly preserves non-authorizations and limitations rather than silently expanding scope.

### Pass 1 key unresolved items

```text
A005: unique-test accounting decreased from 394 in Packet 016E acceptance to 386 in Packet 016F closure; requires Pass 8 recount and explanation.

A007: restore-proof cadence appears to require typed-service read-only smoke tests against restored state; closure evidence does not yet independently prove this.

A008: older-schema restore-forward proof and nonzero-file backup proof remain unresolved; retained generations appear current-schema and zero-file.

A009: off-machine survivability remains explicitly deferred; local retained generations do not prove Google Drive plus USB posture for Milestone 11 closure.

A010: recovery and retention capabilities appear placed in ops_recovery rather than the typed-service caller-class envelope; requires later source-pass review.
```

## Pass 2 summary - platform checkpoint and path-scope audit

Pass 2 used owner-provided Go8 evidence from the local platform repository. The external auditor cross-checked the Go8 bundle against directly readable `.git` text metadata available in its environment. Cross-checkable fields matched.

Verified platform checkpoint:

```text
platform branch:
main

platform HEAD:
c7bf918a9369617c9592171d84d7df71e286cde2

subject:
Implement Packet 016F recovery closure

working tree:
clean

remotes:
none
```

Verified platform implementation chain:

```text
286b8273498e04a77730d6bdbeaadb2b1c7d2d6b
-> 1472d65d20162a07a3ca12b73f52348ed67dd291  Packet 016C
-> b28fd53818b062d41613efdd250def6e48b376de  Packet 016D
-> b4e219633a9c16cd0d9e2425e33a89572754ac06  Packet 016E
-> c7bf918a9369617c9592171d84d7df71e286cde2  Packet 016F
```

Path-scope result:

```text
Packet 016C: 16/16 changed paths inside allowlist
Packet 016D: 15/15 changed paths inside revised allowlist
Packet 016E: 18/18 changed paths inside allowlist
Packet 016F: 17/17 changed paths inside allowlist
```

No out-of-scope, undocumented, generated, cache, or unexpected binary files were reported in the packet commits.

Packet 016F's claimed `17` changed paths were confirmed by Go8 commit metadata.

### Pass 2 migration hash forwarding

Go8 verified the four migration file SHA-256 values at platform HEAD:

```text
0001_initial_operations.sql:
5f04181bb69b3506c81299036d8795c675d6fd0cef5479900e94ee1ab0a7b166

0002_execution_verification_service.sql:
c73e488078939c914cfffecc6800556580089da4ab547ef1350b2a910e5ea7ba

0003_file_provenance_and_manifest_service.sql:
85427249e774d6d8e3fcda920efadca012f7b735f6b56008f3c3bd5774378495

0004_recovery_and_retention_planning.sql:
f5e41732da684f749820019af95db63df8986b768b6d0c2bf679d12a1b686396
```

All four migration files were reported as LF text, making the current runner-vs-recovery hashing concern low-risk for the current state.

### Pass 2 new items

```text
A011: Strength - all four packet commits are within exact enumerated allowlists; no-scope-creep claim independently corroborated.

A012: Strength - 016D scope-correction discipline propagated forward; 016E and 016F pre-included the recurring migration-test paths.

A013: Requires Pass 8 - 016F allowed but did not create dedicated test_ops_recovery_contracts.py, test_ops_recovery_consistency.py, or test_ops_recovery_retention.py; consistency and retention modules may rely only on integration/hammer tests.

A014: Low/reviewed - secret scan reported two medium content-shape findings in test files only; owner inspection identified dummy redaction-test fixtures, with no matched secret values returned.
```

## Pass 3 summary - schema and migration audit

Pass 3 audited source-level schema and migration behavior only. No tests were run and no live PostgreSQL inspection was supplied.

Files read by the external auditor:

```text
src/kaizen/ops_db/migrations/0001_initial_operations.sql
src/kaizen/ops_db/migrations/0002_execution_verification_service.sql
src/kaizen/ops_db/migrations/0003_file_provenance_and_manifest_service.sql
src/kaizen/ops_db/migrations/0004_recovery_and_retention_planning.sql
src/kaizen/ops_db/migrations/manifest.json
src/kaizen/ops_db/migration_runner.py
src/kaizen/ops_db/schema_contract.py
```

Pass 3 identified a 21-table final schema:

```text
platform_meta:
- schema_version

operations reference / identity:
- project_ref
- repository_ref
- storage_root

operations execution / verification:
- execution_attempt
- verification_run

operations return-manifest evidence:
- return_manifest
- return_manifest_entry
- return_manifest_missing_item
- return_manifest_audit_link

operations artifact / provenance:
- logical_artifact
- artifact_byte_object
- provenance_assertion

operations service control:
- idempotency_record
- service_audit_event

operations recovery / retention:
- recovery_generation
- recovery_generation_object
- restore_proof
- retention_hold
- retention_plan
- retention_plan_item
```

### Pass 3 strengths

- Migration runner is single-transaction, advisory-locked, drift-detecting, and has downgrade/unknown-migration guards.
- Migrations are extension-free and portable relative to the accepted PostgreSQL baseline.
- The schema uses strong CHECK discipline for hashes, commit digests, statuses, non-negative counts, timestamps, and relative-path rejection.
- Project isolation is mostly enforced with composite project-scoped foreign keys.
- Several evidence tables are protected by immutability triggers.
- Grants are least-privilege and column-scoped from migration source.
- No DELETE privilege is granted by migration source.
- PUBLIC privilege hardening is present.
- Durable byte and Markdown columns remain prohibited by schema-contract guard logic.

### Pass 3 confirmed findings

#### KZ-M11-B001 - migration hash interpretation divergence

```text
severity: LOW
status: confirmed, current-safe
```

The migration runner hashes normalized text via `read_text(...).encode(...)`, while the recovery path was previously observed to hash raw bytes. Because all current migration files are LF/no-BOM, both methods currently produce the same accepted hashes. Future CRLF or BOM edits could create a fail-closed recovery mismatch.

Smallest safe correction: use raw-byte hashing in the runner or enforce LF SQL files through repository controls.

#### KZ-M11-B002 - manifest binding is sufficient but could be stronger

```text
severity: LOW / improvement
status: resolved as non-blocking
```

The runner trusts `manifest.json` and verifies each migration against the manifest. The manifest bytes appear folded into recovery migration-set hashing. Standalone acceptance binding of `manifest.json` would be belt-and-suspenders, not required to preserve current correctness.

#### KZ-M11-B003 - schema inventory guard omits recovery and retention tables

```text
severity: MEDIUM
status: confirmed
```

`schema_contract.EXPECTED_TABLES` covers only the original `schema_version` plus 14 operations tables from migration 0001. It omits the six recovery/retention tables from migration 0004:

```text
recovery_generation
recovery_generation_object
restore_proof
retention_hold
retention_plan
retention_plan_item
```

Impact: the schema inventory guard cannot detect missing or dropped recovery/retention tables.

Recommended correction: update `EXPECTED_TABLES` to include the six 0004 tables, or derive expected tables by schema version.

Recommended priority: fix before Milestone 12.

#### KZ-M11-B004 - retention hold release is mutable at schema layer

```text
severity: MEDIUM
status: confirmed
```

Migration 0004 grants update on `retention_hold.released_at` and `retention_hold.released_by_actor_id`, but the schema has no terminal trigger preventing release rewrites, back-dating, re-acting, or un-release back to NULL.

Impact: retention/legal hold release auditability is weaker than other trigger-enforced immutable records.

Recommended correction: add a BEFORE UPDATE trigger that permits the first release write but rejects later release changes and freezes creation fields.

#### KZ-M11-B005 - recovery consistency result is unconstrained text

```text
severity: LOW at schema layer; feeds higher implementation concern
status: confirmed at schema layer
```

`recovery_generation.consistency_result` is `text NOT NULL` without a CHECK constraint over an accepted vocabulary. The schema cannot distinguish a genuine consistency result from an arbitrary literal. This supports, but does not prove, the earlier implementation concern that consistency may be hardcoded as `passed`.

Recommended correction: constrain the accepted vocabulary and verify Pass 6 generation code computes the result.

#### KZ-M11-B006 - dead or consumer-trap status semantics

```text
severity: LOW
status: confirmed as design-choice plus consumer trap
```

Immutability and append-only design make some status values practically unreachable or non-authoritative:

```text
return_manifest.audited_pass / audited_fail
provenance_assertion.superseded / rejected on prior rows
```

Currentness is represented by successor links or audit-link rows, not by mutating prior status. This can mislead future consumers unless canonical read queries are documented and enforced.

#### KZ-M11-B007 - role bootstrap is out-of-band

```text
severity: MEDIUM
status: confirmed from source; live verification required
```

Migrations assume roles such as `kaizen_ops_owner`, `kaizen_ops_service`, `kaizen_ops_backup`, and `kaizen_ops_readonly` already exist. They do not create roles.

Impact: migrations alone cannot stand up a clean operational database.

Recommended correction: add an idempotent owner-run role/bootstrap runbook or script and verify with live role inspection.

#### KZ-M11-B008 - disposable restore is not operational restore

```text
severity: MEDIUM
status: confirmed from source/design; live verification required
```

The 016F disposable restore pattern with `--no-owner --no-privileges` is appropriate for integrity proof, but not sufficient to produce an operational governed database with correct roles, grants, and ownership.

Impact: operational disaster recovery remains unproven by the disposable restore proof.

Recommended correction: document and later prove a DR-to-operational path.

#### KZ-M11-B009 - recovery generation status lacks schema transition guard

```text
severity: LOW
status: confirmed from source
```

`recovery_generation` permits updates to status and related result fields but has no schema-level legal-transition guard. A row could theoretically regress from verified to creating or flip between terminal states if the service layer allowed it.

Recommended correction: add a transition trigger such as `creating -> verified|failed`, with terminal states frozen.

#### KZ-M11-B010 - trigger-dependent isolation and advisory-lock namespace remain open

```text
severity: LOW
status: routed
```

Some cross-project safety depends on triggers rather than only composite foreign keys. The migration advisory-lock key also needs comparison against service-layer advisory-lock keys in Pass 4.

## Current correction candidate list

The following items are candidates for a bounded correction packet if confirmed as accepted by the owner after the remaining audit passes:

```text
1. Update schema_contract.EXPECTED_TABLES to include recovery/retention tables.
2. Add terminal release protection for retention_hold.
3. Add recovery_generation legal-transition guard.
4. Add CHECK vocabulary for recovery_generation.consistency_result.
5. Standardize migration hashing semantics or enforce LF SQL files.
6. Add or document an idempotent role/bootstrap runbook for operational recovery.
7. Document current-provenance and manifest-audit canonical read semantics.
8. Prove or document the distinction between disposable restore proof and operational DR readiness.
```

## Hot unresolved findings after Pass 3

These remain the highest-risk unresolved items:

```text
1. Recovery generation consistency may still be hardcoded as passed.
   Routed to Pass 6.

2. Class B retention may still compute from record created_at rather than project_ref.retired_at.
   Schema supports the correct retired_at basis, but implementation requires Pass 7.

3. Unique test accounting changed from 394 to 386 without final reconciliation.
   Routed to Pass 8.

4. Restore typed-service smoke proof remains unresolved.
   Routed to Pass 6 and Pass 9.

5. Older-schema restore-forward and nonzero-file backup proof remain unresolved.
   Routed to Pass 6, Pass 8, and Pass 9.
```

## Current cumulative audit state

```text
Pass 1: complete
Pass 2: complete
Pass 3: complete
Pass 4: not started
Pass 5: not started
Pass 6: not started
Pass 7: not started
Pass 8: not started
Pass 9: not started

final Milestone 11 technical verdict: not issued
Milestone 11 closure status from Result 288: still recorded as closed
correction packet: not yet approved or authorized
next milestone: still not authorized
```

## Recommended next pass

The next pass should be:

```text
Pass 4 - Typed service and concurrency audit
```

Target files:

```text
src/kaizen/ops_service/contracts.py
src/kaizen/ops_service/errors.py
src/kaizen/ops_service/serialization.py
src/kaizen/ops_service/repository.py
src/kaizen/ops_service/service.py
```

Initial Pass 4 questions:

```text
1. Are service-layer advisory-lock keys disjoint from migration LOCK_KEY 1161975683?
2. Do service read paths use correct current-provenance and audit-link semantics rather than dead status values?
3. Is idempotency reserve -> complete atomic and conflict-safe?
4. Does the service ever issue DELETE, DDL, broad SQL, or unbounded reads?
5. Are caller allowlists and envelopes enforced consistently?
6. Are errors and secrets redacted correctly?
7. Does any service path expose physical paths, request digests, idempotency keys, raw bytes, or credentials improperly?
8. Is retention-hold release protected at the service layer despite schema mutability?
```

## Explicit non-effects of this result

This result does not:

- reopen Milestone 11;
- close or re-close Milestone 11;
- approve a correction packet;
- authorize any implementation;
- authorize live PostgreSQL mutation;
- authorize tests or recovery drills;
- authorize deletion, restore promotion, backup destruction, or remote creation;
- authorize the next milestone;
- supersede Result 288.
