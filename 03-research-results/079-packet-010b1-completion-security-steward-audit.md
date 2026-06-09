---
id: kz-aud-01KTPW9PS7XT4CDAGFSZ3MHXZQ
type: audit
status: complete
project: kaizen-platform
created: 2026-06-09T18:30:00Z
updated: 2026-06-09T18:30:00Z
review_status: approved
summary: "Completion, security, and steward audit for Packet 010B.1 constrained amendment-candidate preparation implementation at platform commit c56e1cc."
related_specs:
  - kz-spec-01KTPJBRH0RQTCNRVQYTZJPVVG
---

# Research Result 079 - Packet 010B.1 Completion Security and Steward Audit

## Scope

Audit the implemented Packet 010B.1 and its documentation return at
the bound platform commit.

## Evidence Reviewed

The exact approved packet hash, platform commits, test evidence, repository state, and implementation findings below were reviewed.

## Bound evidence

```text
approved Packet 010B.1 SHA-256:
9e88e85c0595ec5cf541936366726bf3264f39df8c225c46fa7b98333d7f993b

platform commit:
c56e1cc4f7ca75e060f94127930dc78b8cd7bcc0

predecessor platform commit:
1d3dabf5a49bc818b8c4830733b6fb23b9e76ee7

commit message:
Implement constrained amendment candidate preparation
```

## Verdict

```text
pass - Packet 010B.1 complete; constrained staging-only amendment-candidate preparation implemented with no plan, approval, event, canonical, MCP, or Git mutation capability added
```

## Findings

### Implementation findings

- Added one platform-owned API surface:
  `prepare_amendment_candidate` and
  `load_prepared_amendment_candidate` in
  `src/kaizen/amendment_candidate.py`.
- Added one immutable typed result dataclass:
  `AmendmentCandidatePreparation`.
- Reused accepted platform primitives without duplication:
  `validate_destination`, `validate_operation_id`,
  `validate_validation_id`, `canonical_json_bytes`, `hash_record`,
  `sha256_bytes`, `parse_markdown_note`, `validate_note`,
  `normalize_amendment_candidate`, `validate_amendment_continuity`,
  `validate_amendment_relationships_and_links`,
  `rendered_amendment_diff`, `utc_now`, `enforce_promotion_scope`,
  `staging_write_mutex`, and the native Windows handle-relative
  primitives.
- Exposed five existing helpers in `amendment_plan.py` for shared
  use by `amendment_candidate`; no plan, approval, execution,
  recovery, event, or canonical mutation semantics changed.
- Added no new third-party dependency.

## Post-review fix findings

### F-01 - Generated-timestamp retries reuse existing evidence

Pass.

The existing-directory branch now executes before `utc_now()` is
called. On identical retry under `prepared_at=None`, the function
delegates to `load_prepared_amendment_candidate`, confirms the
existing record matches the request, and returns the original
result with `idempotent: true`. The regression test
`test_generated_timestamp_retry_reuses_existing_preparation`
verifies `second.prepared_at == first.prepared_at` and
`second.preparation_sha256 == first.preparation_sha256`.

### F-02 - Packet-mismatched reuse returns idempotency_conflict

Pass.

The existing-directory branch maps the loader's
`live_packet_mismatch`, `live_vault_drift`, and
`preparation_binding_mismatch` errors to `idempotency_conflict`.
The regression test
`test_packet_mismatched_retry_is_idempotency_conflict` confirms the
correct error code and that every byte of every evidence file is
preserved.

### F-03 - Nested mutex removed

Pass.

`prepare_amendment_candidate` acquires `staging_write_mutex`
exactly once at the outer boundary. The inner locked function no
longer re-acquires the same mutex. The complete preparation state
machine runs under one cross-process lock.

## Security findings

### S-01 - No new mutation surface

Pass.

The new module imports nothing from `promotion_events`,
`promotion_workflow`, `amendment_workflow`, `atomic_install`,
`atomic_replace`, or any execution or recovery surface. No
governance-log append, no plan write under `_promotion/operations`,
no approval, no canonical mutation, and no Git or remote action
occurs on any path.

### S-02 - Pre-write validation precedes all writes

Pass.

Packet ID format, operation ID format, promotion-scope enforcement,
prior-hash format, UTF-8 validity, content size, null-byte
rejection, destination confinement, prior-hash equality,
continuity, no-change rejection, canonical validation, relationship
and link resolution, and diff rendering all complete before any
`create_new_relative_file` call. On any pre-write failure no
`_promotion/preparations` directory exists.

### S-03 - Create-once evidence is enforced through native primitives

Pass.

All six evidence files are created through
`create_new_relative_file` (NtCreateFile with `FILE_CREATE`
disposition) under an `open_verified_parent` chain that rejects
reparse points segment by segment. No overwrite, truncate,
rename-over, delete, or move primitive is called.

### S-04 - Idempotency and conflict paths fail closed

Pass.

Identical retry reuses the original evidence bytes and the
original timestamp. Conflicting retry preserves all existing
evidence and returns `idempotency_conflict`. Partial evidence (any
file present without a valid `preparation.json`) returns
`preparation_recovery_required` and is not modified, deleted, or
completed automatically.

### S-05 - Live scope enforcement is reused

Pass.

`enforce_promotion_scope` is called from both the preparation and
loader paths. Live-mode preparation rejects caller-supplied
`prepared_at`. The live binding is recorded in `preparation.json`
and verified by the loader.

### S-06 - Loader is read-only

Pass.

`load_prepared_amendment_candidate` performs no writes. It
validates the exact file set, verifies the self-hash, verifies
every evidence-file hash, verifies operation and optional packet
and live bindings, and verifies that the embedded validation
evidence matches.

### S-07 - Hostile inputs fail closed before staging effects

Pass.

Hostile operation IDs (`../escape`, `kz-prom-../../escape`,
`kz-prom-not-a-ulid`) fail in `validate_operation_id` before any
filesystem effect. Hostile destinations fail in
`validate_destination` or in the native parent-verification chain.

## Test evidence

```text
focused Packet 010B.1 suite (contract + hammer):
15 passed

full platform suite:
267 passed

inherited Packet 010B baseline:
252 passed

new tests added by Packet 010B.1:
15

regression:
none
```

## Repository findings

```text
kaizen-platform:
main
HEAD c56e1cc4f7ca75e060f94127930dc78b8cd7bcc0
remote: none

kaizen-docs:
main
HEAD 83d20ddbb8f69d813a3f3ccc64c231785d53cd5b (predecessor of this
audit and the 010B.1 completion-report return)
remote: origin/main

kaizen-vault:
main
HEAD e5e4eec1adc4ef26f9e735333dbb229b7bb59368
unchanged by implementation
remote: none

kaizen-mcp:
non-Git temporary proving ground
unchanged by implementation
```

Only the exact Packet 010B.1 platform paths changed. No canonical
vault, staging, MCP, remote, connector, console, or Packet 010C
work occurred.

## Contradictions and Gaps

No contradiction blocks closure. The two implementation deviations below remain non-blocking and intentionally deferred.

## Non-blocking notes

1. The `tests/fixtures/amendment_candidate/**` subtree listed in
   the original expected test paths was not created. Fixtures are
   inline Python strings in the two new test modules. Reviewed and
   intentionally deferred rather than expanded into scope.
2. `load_prepared_amendment_candidate` verifies evidence file
   content via `Path.read_bytes()` after a separate
   `is_symlink()` check, while the write path uses native
   reparse-rejecting handles end-to-end. Reviewed and intentionally
   deferred; staging is governed and the create-new-only discipline
   closes the practical attack surface. The parity gap is recorded
   for a future packet rather than expanded into 010B.1 scope.
3. Packet 010C must be separately revised and re-audited before it
   becomes eligible for owner approval. Packet 010B.1 completion
   does not authorize Packet 010C implementation, MCP changes,
   canonical vault mutation, remote creation, console work, or work
   under Packets 010D through 010F.
4. The backup/remote checkpoint remains required before the first
   mutation-capable Milestone 6 packet ships outside the platform
   repository.

## Recommendation

Accept Packet 010B.1 as complete at platform commit `c56e1cc4f7ca75e060f94127930dc78b8cd7bcc0`. Keep Packet 010C separate and unimplemented until its revised exact-hash owner gate is satisfied.

## Human Verdict

Packet 010B.1 is complete. No authority is granted for Packet 010C implementation, MCP changes, vault/staging mutation, remote creation, console work, or Packets 010D through 010F.

## Current gate

```text
Packet 010A: complete
Packet 010B: complete
Packet 010B.1: complete at platform commit c56e1cc
Packet 010C: blocked draft pending revision and separate audit
Packet 010C implementation: prohibited
Local operator console: not authorized
Vault/staging mutation: not authorized
Packets 010D through 010F: not approved
```
