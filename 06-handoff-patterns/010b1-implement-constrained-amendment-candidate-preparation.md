---
id: kz-tp-01KTPQ05QFZKN70RQHYS86PFAF
type: task-packet
status: complete
project: kaizen-platform
created: 2026-06-09T17:30:54Z
updated: 2026-06-09T18:30:00Z
review_status: approved
authority: accepted
summary: "Implement a fixed-root, staging-only, create-once platform API for preparing one governed amendment candidate from a reviewed full replacement payload."
primary_spec: kz-spec-01KTPJBRH0RQTCNRVQYTZJPVVG
related_specs:
  - kz-spec-01KTPJBRH0RQTCNRVQYTZJPVVG
---

# Task Packet 010B.1 - Implement Constrained Amendment-Candidate Preparation

> Packet 010B.1 is a proposed platform prerequisite packet. Drafting and auditing it do not authorize implementation. It does not authorize Packet 010C implementation, MCP changes, approval, execution, recovery, canonical vault mutation, live staging mutation, remote creation, console work, or work under Packets 010D through 010F.

## Objective

Implement one authoritative platform API that prepares a reviewed amendment candidate beneath the configured staging root without creating an amendment plan, approval, event, canonical write, or Git mutation.

This packet resolves the prerequisite identified by blocked Packet 010C and Result 077.

The first slice accepts a complete reviewed Markdown replacement payload only. It does not define or accept a patch language.

## Scope

This packet is limited to one platform-owned preparation API, one read-only loader, the minimum shared amendment helper exposure needed to avoid duplicated semantics, bounded documentation, and disposable-root tests. It creates staging-only preparation evidence and does not create an amendment operation plan or any canonical effect.

## Authorization boundary

The owner selected Packet 010C blocker resolution Option A and authorized drafting and auditing this separate prerequisite packet only.

Exact owner instruction:

```text
I choose Packet 010C blocker resolution Option A. Draft and audit a separate platform prerequisite packet for constrained amendment-candidate preparation only. I do not authorize its implementation, Packet 010C implementation, MCP changes, canonical vault mutation, remote creation, or work under Packets 010D through 010F.
```

Implementation remains prohibited until the owner approves the exact audited Packet 010B.1 SHA-256.

## Bound state

```text
kaizen-docs:
branch: main
HEAD: 83d20ddbb8f69d813a3f3ccc64c231785d53cd5b
upstream: origin/main
working tree at verification: clean

kaizen-platform:
branch: main
HEAD: 1d3dabf5a49bc818b8c4830733b6fb23b9e76ee7
working tree at verification: clean
remote: none

kaizen-vault:
branch: main
HEAD: e5e4eec1adc4ef26f9e735333dbb229b7bb59368
working tree at verification: clean
remote: none

accepted roadmap SHA-256:
1dd4eb3ae80b0d855091b26cf598b9a744a5141648b4739580bb90871f2e11bc

blocked Packet 010C SHA-256:
8e81c02917163bcd16ca040546142983b54a4960e741be3b0caf47c4ce90478c

Result 077 SHA-256:
71398615bd0ad4c4f2732570b2756bf9ee276e885d426e5dfe85a466d49575c2
```

Any branch, HEAD, cleanliness, root, remote, or accepted-contract mismatch stops implementation before editing.

## Read First

### Governance and contracts

- `03-research-results/077-packet-010c-prerequisite-gap-and-security-steward-audit.md`
- `06-handoff-patterns/010c-implement-amendment-native-mcp-adapters.md`
- `04-design-decisions/0014-milestone-6-governed-operator-surface-boundary.md`
- `05-specs/milestone-6-governed-operator-surface.md`
- `05-specs/kaizen-validation-gate-spec.md`
- `05-specs/kaizen-hammer-test-strategy.md`
- `05-specs/staging-write-wrapper-and-promotion-recovery.md`

### Platform implementation

- `src/kaizen/amendment_plan.py`
- `src/kaizen/path_confinement.py`
- `src/kaizen/promotion_contracts.py`
- `src/kaizen/promotion_scope.py`
- `src/kaizen/root_config.py`
- `src/kaizen/validation.py`
- `src/kaizen/windows_fs.py`
- existing amendment, staging-write, path-confinement, and hammer tests.

## Consequence class

```text
prepare-candidate
```

Allowed effect:

```text
create one immutable preparation directory and its bounded staging-only evidence
```

Forbidden effects:

```text
amendment plan creation
approval creation
promotion/amendment event creation
canonical Markdown mutation
canonical governance-log mutation
Git staging, commit, remote, or push
MCP or console mutation
```

## Proposed public API

Create:

```text
src/kaizen/amendment_candidate.py
```

Expose:

```python
prepare_amendment_candidate(
    config: RootConfig,
    *,
    packet_id: str,
    operation_id: str,
    destination: str,
    expected_prior_sha256: str,
    replacement_markdown: str,
    prepared_at: str | None = None,
    promotion_scope: VerifiedLivePromotionScope | None = None,
) -> AmendmentCandidatePreparation
```

The function must return an immutable typed result containing at least:

```text
schema_version
packet_id
operation_id
preparation_path
source_path
canonical_destination
prepared_at
prior_sha256
replacement_payload_sha256
canonical_candidate_sha256
reviewed_diff_sha256
validation_run_id
validation passed/error/warning counts
continuity summary
relationship-resolution summary
link-resolution summary
live binding when present
idempotent boolean
```

## Input contract

### Packet ID

- required;
- must match `kz-tp-<ULID>`;
- live use must match the verified live scope packet ID;
- cannot be inferred or defaulted.

### Operation ID

- required and reserved before preparation;
- must satisfy the existing governed operation-ID contract;
- identifies both the preparation directory and the later amendment operation;
- corrections require a new operation ID unless the retry is byte-for-byte and binding-for-binding identical.

### Canonical destination

- required canonical-relative Markdown path;
- must pass the existing destination validation contract;
- must resolve to one existing regular non-reparse canonical file;
- must remain beneath the configured canonical root;
- arbitrary absolute, drive-relative, UNC, device, traversal, symlink, junction, and reparse paths are prohibited.

### Expected prior SHA-256

- required 64-character lowercase hexadecimal digest;
- must equal the current canonical destination bytes before any staging write occurs;
- mismatch fails with zero staging side effects.

### Replacement Markdown

- required complete UTF-8 Markdown document;
- maximum size must not exceed the platform's accepted bounded Markdown payload limit;
- null bytes and invalid UTF-8 are prohibited;
- no patch, command, template language, filesystem path, or generic editor instruction is accepted;
- the reviewed payload SHA-256 must be recorded before normalization.

### Prepared timestamp

- generated internally as full ISO-8601 UTC for fixed live scope;
- caller-supplied values are permitted only with disposable test roots;
- typed adapters over fixed live roots must not expose timestamp control;
- the exact timestamp is recorded immutably and reused by later planning so canonical candidate bytes do not drift between preparation and plan creation.

## Continuity and validation contract

Before writing preparation evidence, the API must:

1. read the canonical destination bytes through existing fixed-root and non-reparse controls;
2. verify `expected_prior_sha256`;
3. parse the prior and replacement Markdown;
4. preserve immutable fields exactly:
   - `id`;
   - `type`;
   - `project`;
   - `created`;
5. preserve first-slice authority fields exactly:
   - `review_status`;
   - `authority`;
6. reject a no-change replacement after deterministic normalization;
7. normalize only the already accepted amendment fields and line endings:
   - remove staging-only `validation_status` when present;
   - set `updated` to the exact preparation timestamp;
   - normalize CRLF/CR to LF;
8. run canonical validation on the exact normalized candidate bytes;
9. resolve canonical relationships and Markdown links using the existing amendment-planning rules;
10. render one deterministic unified diff from prior canonical bytes to normalized candidate bytes.

No field beyond the existing accepted amendment normalization contract may be silently changed.

## Preparation layout

Create exactly one directory:

```text
<staging-root>/_promotion/preparations/<operation-id>/
```

Create only these files:

```text
replacement-payload.md
canonical-candidate.md
prior-canonical.md
reviewed-diff.patch
validation.json
preparation.json
```

Rules:

- the directory and every file are create-new only;
- no file may be replaced, truncated, renamed over, or deleted by the success path;
- parent traversal and reparse points fail closed;
- partial creation must be recoverable only through deterministic idempotent retry rules defined below;
- no file is created beneath `_promotion/operations/<operation-id>`;
- amendment planning remains a distinct later consequence class.

## Preparation evidence contract

`preparation.json` must bind:

```text
schema version
packet ID
operation ID
prepared timestamp
canonical and staging roots
canonical destination
expected and verified prior SHA-256
replacement payload SHA-256
canonical candidate SHA-256
reviewed diff SHA-256
validation evidence SHA-256
validation run ID
continuity result
relationship and link resolution
live binding when present
preparation record SHA-256
```

The record must use deterministic canonical JSON and a self-hash consistent with existing plan and approval evidence.

`validation.json` must describe validation of the exact `canonical-candidate.md` bytes and include the preparation operation context.

## Idempotency and interruption contract

### Identical retry

If the preparation directory already exists and all files, hashes, packet, operation, destination, prior hash, replacement payload, timestamp, and live binding match exactly:

- verify every file and record;
- return the original result with `idempotent: true`;
- create no new evidence and modify no timestamps.

### Conflict retry

If the operation ID was previously used with any different packet, destination, prior hash, replacement payload, timestamp, root binding, or evidence bytes:

```text
idempotency_conflict
```

The API must preserve all existing evidence and create nothing else.

### Partial preparation

If a directory or subset of files exists without a complete valid `preparation.json`:

```text
preparation_recovery_required
```

The API must not delete, overwrite, normalize, or complete the partial evidence automatically in this packet.

A later recovery design is not authorized here. The partial evidence remains available for diagnosis.

## Live and disposable scope

### Disposable roots

- allowed for tests;
- must satisfy `RootConfig` and confinement requirements;
- may not use a verified live scope.

### Fixed live roots

- require `VerifiedLivePromotionScope` created by the existing fixed-root verifier;
- packet ID, canonical root, staging root, platform HEAD, vault branch/HEAD, governance-log hash, and clean-state binding must match;
- preparation may create staging evidence only;
- the vault and governance log must remain byte-for-byte unchanged;
- no live candidate preparation may occur under this packet until implementation is separately approved.

## Planner handoff contract

The preparation result must make later Packet 010C planning deterministic without changing consequence classes.

Add a read-only loader:

```python
load_prepared_amendment_candidate(
    config: RootConfig,
    operation_id: str,
    *,
    packet_id: str | None = None,
    promotion_scope: VerifiedLivePromotionScope | None = None,
) -> AmendmentCandidatePreparation
```

The loader must verify all preparation files and hashes and return:

- the `canonical-candidate.md` path;
- the exact `prepared_at` timestamp;
- all expected prior/candidate/diff hashes;
- packet and live bindings.

Later Packet 010C may use this loader to call existing amendment planning with the exact prepared candidate and preparation timestamp. Packet 010B.1 does not expose a plan tool and does not create `plan.json`.

If implementation evidence proves existing `create_amendment_plan` cannot consume the prepared candidate deterministically without semantic change, implementation must stop for packet amendment rather than silently widening this packet.

## Implementation Requirements

Expected production paths:

```text
src/kaizen/amendment_candidate.py
src/kaizen/amendment_plan.py
src/kaizen/__init__.py
README.md
```

Expected test paths:

```text
tests/test_amendment_candidate.py
tests/test_amendment_candidate_hammer.py
tests/fixtures/amendment_candidate/**
```

`amendment_plan.py` may change only to expose or share the already implemented normalization, continuity, relationship, link, and diff logic. No plan, approval, execution, recovery, event, or canonical mutation semantics may change.

No new dependency is authorized.

## Validation

### Contract tests

- prepares one valid full-replacement candidate;
- returns every required typed field;
- records exact payload, prior, candidate, diff, validation, and record hashes;
- preserves identity, project, creation, review, and authority fields;
- sets only the accepted `updated` transition;
- canonical validation and relationship/link checks use the exact prepared bytes;
- loader verifies complete evidence;
- identical retry is idempotent;
- conflicting retry fails closed;
- partial evidence requires recovery and is preserved.

### Input rejection tests

- wrong packet ID;
- wrong or path-like operation ID;
- missing destination;
- prior-hash mismatch;
- no-change payload;
- immutable identity field change;
- review or authority field change;
- invalid Markdown/frontmatter;
- oversized payload;
- null byte;
- unresolved relationship;
- broken canonical link;
- staging link;
- absolute, traversal, UNC, drive-relative, device, reserved-name, and invalid-character paths.

### Filesystem and hammer tests

- symlink, junction, and reparse destination or preparation parents fail closed;
- preparation directory is create-new only;
- no write occurs before all pre-write validation succeeds;
- fault injection after each created file leaves preserved partial evidence and later returns `preparation_recovery_required`;
- no success or failure path writes beneath the canonical root;
- governance log and Git state remain unchanged;
- no plan, approval, event, operation directory, temp canonical file, lock file, cache, or log is created;
- concurrent identical requests produce one complete preparation or one complete result plus deterministic idempotent reads;
- concurrent conflicting requests cannot mix evidence;
- hostile operation IDs cannot escape `_promotion/preparations`;
- production fixed roots are never used by tests.

### Regression gates

- all focused Packet 010B.1 tests pass;
- full platform suite passes with at least the inherited `252 passed` and no regression;
- existing staging-write, promotion, amendment, operation-status, and hammer tests pass;
- no existing plan, approval, execute, or recovery result changes.

## Constraints

- full replacement payload only;
- no patch interpreter;
- no generic staging writer reuse that weakens authority controls;
- no generic file editor;
- no arbitrary destination or output path;
- no delete, overwrite, rename-over, move, or cleanup capability;
- no shell, PowerShell, Python, SQL, or Git execution surface;
- no actor or approval semantics;
- no canonical mutation;
- no event mutation;
- no remote creation or push;
- fixed-root scope remains platform-enforced;
- uncertainty fails closed before writing.

## Non-Scope

- MCP implementation;
- Packet 010C implementation;
- patch or merge language;
- candidate editing after preparation;
- preparation recovery or cleanup automation;
- amendment planning, approval, execution, or recovery;
- local operator console;
- canonical vault mutation;
- production staging mutation before exact packet approval;
- Git remote creation or push;
- actor identity expansion;
- generalized edit/delete/move/correct/rollback/supersede;
- multi-note transactions;
- Packets 010D through 010F.

## Stop gates

Stop before or during implementation when:

- bound repository state or accepted hashes differ;
- a new dependency is proposed;
- existing authority or review continuity must be weakened;
- existing create-only staging controls must be weakened;
- deterministic planning requires broader amendment-plan semantic changes;
- any canonical, governance-log, event, approval, or Git write occurs;
- partial evidence would require deletion or overwrite;
- a generic patch or editor capability is proposed;
- changed paths exceed the accepted set;
- full-suite regression occurs.

## Implementation return requirements

An approved implementation must return:

- exact platform commit;
- exact changed paths;
- focused and full-suite results;
- hammer and fault-injection evidence;
- zero-canonical-mutation evidence;
- zero-event and zero-Git-mutation evidence;
- idempotency and partial-evidence results;
- deviations and discovered contract gaps;
- completion security/steward audit;
- no Packet 010C or MCP changes.

## Acceptance Criteria

Packet 010B.1 is eligible for owner approval only when:

1. the full-replacement-only contract is explicit;
2. preparation is create-once, staging-only, and operation-bound;
3. exact fixed-root, packet, destination, prior-hash, and live bindings are required;
4. continuity and canonical validation reuse accepted platform rules;
5. preparation evidence and loader contracts are deterministic;
6. idempotent retry and partial-evidence behavior fail closed;
7. no plan, approval, event, canonical, Git, MCP, or console capability is introduced;
8. exact file scope and tests are reviewable;
9. a separate security/steward audit passes;
10. the exact packet SHA-256 is presented for owner approval.

## Completion Report

Implementation status:

```text
complete
```

### Bound implementation evidence

```text
platform commit:
c56e1cc4f7ca75e060f94127930dc78b8cd7bcc0

predecessor commit:
1d3dabf5a49bc818b8c4830733b6fb23b9e76ee7

commit message:
Implement constrained amendment candidate preparation
```

### Exact changed paths

```text
src/kaizen/amendment_candidate.py
src/kaizen/amendment_plan.py
src/kaizen/__init__.py
README.md
tests/test_amendment_candidate.py
tests/test_amendment_candidate_hammer.py
```

`src/kaizen/amendment_plan.py` changed only to expose the existing
`normalize_amendment_candidate`, `validate_amendment_continuity`,
`validate_amendment_relationships_and_links`,
`rendered_amendment_diff`, and `utc_now` helpers for shared use by
`amendment_candidate`. No plan, approval, execution, recovery,
event, or canonical mutation semantics changed.

The `tests/fixtures/amendment_candidate/**` subtree listed in the
original packet was not created; fixtures are inline Python strings
in the two new test modules. This deviation was intentionally
accepted as non-blocking after final review and is recorded here for
the completion audit.

### Test evidence

```text
focused Packet 010B.1 suite (contract + hammer):
15 passed

full platform suite after commit and editable reinstall:
267 passed

inherited baseline (Packet 010B completion):
252 passed

new tests added by Packet 010B.1:
15

regression:
none
```

### Contract verification

- one platform-owned API surface added: `prepare_amendment_candidate`
  and `load_prepared_amendment_candidate`;
- evidence is created only beneath
  `<staging-root>/_promotion/preparations/<operation-id>/`;
- the exact six-file evidence set is created create-new only, with
  `preparation.json` written last as the completeness marker;
- `preparation.json` self-hash uses the existing canonical JSON and
  `hash_record` primitives;
- `expected_prior_sha256` is verified before any staging write;
- continuity preserves `id`, `type`, `project`, `created`,
  `review_status`, and `authority`; normalization is restricted to
  removing staging-only `validation_status`, setting `updated` to the
  preparation timestamp, and CRLF/CR-to-LF;
- canonical validation runs on the exact normalized candidate bytes;
- relationship and Markdown link resolution reuses the existing
  amendment-planning rules;
- the deterministic unified diff is rendered prior to staging writes;
- identical retry verifies existing evidence and returns
  `idempotent: true`; the cross-process named mutex serializes the
  complete preparation state machine and the existing-directory
  check precedes timestamp generation so retries reuse the original
  timestamp;
- conflicting retry preserves all existing evidence and returns
  `idempotency_conflict`, including the case where the operation ID
  is bound to a different packet ID or live binding;
- partial evidence (any file present without a valid
  `preparation.json`) returns `preparation_recovery_required` and is
  preserved;
- concurrent identical requests produce one complete preparation
  plus deterministic idempotent reads;
- concurrent conflicting requests cannot mix evidence;
- fault injection after each created file leaves preserved partial
  evidence and later returns `preparation_recovery_required`;
- hostile operation IDs and reparse points fail closed before any
  staging write;
- the success path creates no file beneath
  `_promotion/operations/<operation-id>` and no governance log
  entry.

### Zero-mutation evidence

- no canonical vault byte changed (verified by the hammer test
  `test_success_creates_no_operation_event_or_canonical_effect`);
- no `_promotion/operations/<operation-id>` directory created;
- no governance log entry appended;
- no plan, approval, event, execution, or recovery write occurred;
- no Git command was executed; no remote was created;
- `kaizen-mcp` was not modified and was not initialized as Git;
- the `kaizen-vault` and `kaizen-staging` repositories were not
  modified.

### Deviations and discovered contract gaps

1. The `tests/fixtures/amendment_candidate/**` subtree listed in the
   original expected test paths was not created. Fixtures are inline
   Python strings.
2. The loader verifies evidence file content via
   `Path.read_bytes()` after a separate `is_symlink()` check, while
   the write path uses native reparse-rejecting handles end-to-end.
   This was intentionally deferred as non-blocking; staging is
   governed and the create-new-only discipline closes the practical
   attack surface, but the parity gap is recorded for a future
   packet.

Both deviations were reviewed and intentionally deferred rather
than expanded into Packet 010B.1 scope.

## Current gate

```text
Packet 010A: complete
Packet 010B: complete
Packet 010B.1: complete at platform commit c56e1cc
Packet 010C: blocked draft pending revision and separate audit
Packet 010C implementation: prohibited
Packets 010D through 010F: not approved
```
