---
id: kz-tp-01KTW3B2C4D6F8G0H2J4K6M8NP
type: task-packet
status: draft
project: kaizen-platform
created: 2026-06-11T21:35:00Z
updated: 2026-06-11T21:35:00Z
review_status: pending-review
authority: proposed
summary: "Packet 012B platform prepared-candidate loader, concurrency, dirty-repository, and stale-binding reliability proof."
primary_spec: kz-spec-01KTW2M8N4Q6S8W0Y2Z4ABCDEF
---

# Task Packet 012B - Platform Loader and Concurrency Reliability

> Platform-only implementation packet. This draft authorizes no implementation until the owner approves its exact audited SHA-256. It does not authorize Kaizen MCP, Go8, connector, vault, canonical, lifecycle, dependency, or remote changes.

## Objective

Close the platform portion of Milestone 8 defects M8-D01 and M8-D02 by proving the frozen C1 prepared-candidate loader contract, root-causing the intermittent concurrency error-code drift, enforcing deterministic same-operation behavior without mixed evidence, and proving dirty-repository and stale live-binding failures close before preparation evidence is created.

## Bound authority

```text
Authoritative standard SHA-256:
5b441a9e024446d8a9acb3a58a8a24ad020772f4e7666d697c495352bebb6e90

Roadmap v0.3 SHA-256:
f8f5d20048169178111bd0f31fd41dc363777b90ff72957a5d19ec0b77b57f82

Milestone 8 specification SHA-256:
971f9e6d8b74bf8ea8dfbdcf14c78287eaacfe1cf5c91761aa80016e19ba46a3

Milestone 8 audit SHA-256:
51a5a1315273a07b407fa62747141bf716a8af5dba383c77d05b9837ee3ae404

Packet 012A contract-freeze SHA-256:
a5a3650adf067fe1a96c8d0a26d087e6f387c5a9e35828cfea9784add0e77ea9

Packet 012A completion audit SHA-256:
bae3a5a09e95f6f48a6c7d8e559de20ecda1e4e3de26ee05cd40722ba5c4dd77

Owner authorization to draft and audit only:
03-research-results/139-packet-012a-owner-completion-acceptance.md
```

## Repository and implementation boundary

Only this implementation repository is in scope:

```text
C:\dev\kaizen\platform
```

Expected implementation checkpoint:

```text
branch: main
HEAD: 1b8be1d6d42d768587dddb2be8415fa24b670561
working tree: clean
remotes: none
```

No remote may be created. Packet implementation may produce a local platform commit only after exact owner approval.

## Read-first implementation paths

Search before editing, then inspect at minimum:

```text
src/kaizen/amendment_candidate.py
src/kaizen/promotion_scope.py
src/kaizen/windows_fs.py
src/kaizen/promotion_events.py
src/kaizen/root_config.py
tests/test_amendment_candidate.py
tests/test_amendment_candidate_hammer.py
tests/test_live_promotion.py
```

Additional platform paths may be changed only when inspection proves they are the smallest location required by this packet. Every added path must be reported before editing and included in exact changed-path evidence.

## Verified planning findings

### Current loader behavior

`load_prepared_amendment_candidate` currently:

- validates staging configuration and operation ID;
- enforces promotion scope before reading preparation evidence;
- verifies the exact six-file preparation set and rejects symlinked evidence files;
- verifies the preparation self-hash;
- verifies operation binding always;
- verifies packet binding only when `packet_id` is supplied;
- verifies live binding only when `promotion_scope` is supplied;
- verifies replacement, candidate, prior, diff, and validation hashes;
- verifies validation-run binding;
- performs no explicit mutex acquisition, temporary evidence creation, event creation, Git write, or canonical mutation.

This appears consistent with C1 but is not yet proven by a complete accepted test matrix.

### Current preparation ordering

`prepare_amendment_candidate` acquires `staging_write_mutex` before entering the complete locked state machine. Inside the mutex, `_prepare_amendment_candidate_locked`:

1. validates request and scope;
2. checks whether the operation preparation directory already exists;
3. if it exists, loads and compares accepted evidence for idempotency or conflict;
4. otherwise opens and reads the canonical destination;
5. derives candidate, diff, validation, and record bytes;
6. installs the six preparation files.

The observed one-time `destination_path_invalid` therefore cannot be explained by two contenders bypassing the process-level mutex in the current thread-based test alone. Root-cause evidence must examine native-handle timing, fixture lifetime, destination stability, process versus thread serialization, and whether the error arose outside the operation-conflict path.

### Existing test coverage

Existing tests already cover:

- complete prepare-and-load success with packet binding;
- canonical bytes unchanged and no operation evidence created by successful preparation;
- identical retry idempotency and unchanged evidence mtimes;
- sequential conflicting retry returning `idempotency_conflict` without evidence changes;
- partial preparation returning `preparation_recovery_required`;
- prior-hash mismatch writing no preparation evidence;
- changed candidate detection;
- extra-file rejection;
- hostile operation input rejection;
- fault-injected partial evidence preservation and recovery-required retry;
- eight concurrent identical requests with one creator and seven idempotent results;
- one two-request conflict with one winner, one `idempotency_conflict`, and no mixed candidate text.

Missing or insufficient proof includes the complete C1 error matrix, independent no-mutex/no-temporary-evidence loader proof, symlink cases, preparation self-hash mismatch, operation-binding mismatch, wrong packet, stale live binding, every evidence-file hash, validation-run mismatch, at least 100 conflicting rounds, exact loser-code distribution, exact evidence-hash coherence, process-boundary serialization where supported, and dirty/stale live-scope fail-before-write behavior.

## Work package A - C1 platform loader proof

Prefer tests only unless a test exposes a real implementation defect.

Add focused disposable-root tests that prove:

| Case | Required result |
|---|---|
| missing preparation directory | `preparation_missing` |
| incomplete or extra file set | `preparation_recovery_required` |
| preparation directory or evidence symlink | fail closed with the accepted preparation safety class; no file followed |
| preparation self-hash mismatch | `preparation_hash_mismatch` |
| record operation differs from requested operation | `preparation_binding_mismatch` |
| omitted `packet_id` and omitted live scope | evidence and operation binding still verified; successful load when evidence is sound |
| supplied wrong packet | `live_packet_mismatch` |
| supplied stale live scope | `live_vault_drift` |
| changed replacement payload | `preparation_evidence_changed` |
| changed canonical candidate | `preparation_evidence_changed` |
| changed prior canonical bytes | `preparation_evidence_changed` |
| changed reviewed diff | `preparation_evidence_changed` |
| changed validation bytes | `preparation_evidence_changed` |
| invalid validation JSON | `validation_evidence_invalid` or the narrower already-frozen recovery class only if exact audit proves the contract requires correction |
| validation-run mismatch | `validation_evidence_invalid` |

Tests must recompute outer preparation hashes where needed so each test reaches the intended inner check instead of failing earlier for the wrong reason.

### Loader no-mutation proof

A dedicated test must snapshot before and after:

- canonical fixture tree bytes and paths;
- preparation evidence bytes, paths, and mtimes;
- absence of `_promotion/operations/<operation-id>`;
- absence of governance events;
- absence of operation-scoped temporary files.

The test must also monkeypatch the loader-visible `staging_write_mutex` symbol, if imported, or the underlying mutex entry point to fail if called. If no loader path references the mutex, static inspection plus a fail-on-call monkeypatch at the module boundary is acceptable. The proof must show loader execution does not create or acquire a write mutex and does not create evidence.

## Work package B - concurrency root cause and deterministic behavior

### Root-cause sequence

Before changing production code:

1. reproduce the existing two-contender test repeatedly without fixture teardown overlap;
2. record whether `destination_path_invalid` appears and capture only structured error codes and non-sensitive exception classes;
3. hold one immutable canonical destination open for the entire contender lifetime;
4. verify both contenders share the same `RootConfig`, destination path, expected prior hash, packet ID, operation ID, and prepared timestamp, differing only in replacement payload;
5. verify executor shutdown completes before fixture cleanup;
6. inspect whether `staging_write_mutex` serializes across threads and processes on Windows;
7. add a bounded process-level test only if the mutex contract claims cross-process serialization and the repository test environment can run it deterministically;
8. identify whether any `NativeFsError` originates before or after operation binding exists.

Test-only instrumentation is preferred. Production logging, public APIs, or persistent diagnostics are not authorized unless exact evidence proves they are required and the packet is re-audited before use.

### Error decision rule

Do not translate every canonical-read or native filesystem failure to `idempotency_conflict`.

- When a complete preparation already binds the operation and the request differs, return `idempotency_conflict`.
- When an incomplete installed preparation exists, return `preparation_recovery_required`.
- When no operation binding exists and the canonical destination genuinely cannot be safely opened, preserve the lower-level safety failure through the accepted structured platform error.
- If the observed mismatch was fixture interference or an over-specific assertion, repair the test and document why no production change was required.
- If C2's loser-code wording is disproved by a genuine pre-binding safety failure, stop implementation and return an exact proposed contract correction plus audit. Do not silently weaken assertions.

### Required 100-round stress proof

Add a focused stress test or fixed test helper that performs at least 100 independent conflicting rounds. Each round must use a fresh operation ID and isolated preparation directory while sharing a stable disposable canonical fixture.

For every round record in test assertions or a deterministic returned summary:

- creator count;
- loser count;
- loser error-code distribution;
- selected candidate hash;
- reviewed-diff hash;
- preparation hash;
- exact final six-file evidence set;
- recomputed hashes for every evidence file;
- agreement between `preparation.json`, `validation.json`, and loaded result;
- proof that candidate, prior, diff, validation, timestamp, packet, and operation fields all belong to one request.

Required outcome under stable fixtures:

```text
creator count per round: 1
conflicting loser count per round: 1 or the exact configured contender count minus 1
normal loser code: idempotency_conflict
mixed evidence: 0
incomplete final preparations: 0
unexpected error codes: 0
```

Any unexpected code must fail the test while preserving its distribution in the failure message.

### Identical-request stress

Retain and strengthen the identical-request proof so one request creates evidence and all identical contenders return `idempotent: true` with one preparation hash and unchanged evidence bytes.

## Work package C - dirty repository and stale binding

Use disposable Git repositories and disposable governance logs. Never dirty or rewrite the live platform or vault repositories for tests.

Prove the fixed live-scope verifier and operation verifier bind and distinguish:

| Condition | Required structured error |
|---|---|
| platform worktree dirty before plan scope | `live_platform_drift` |
| vault worktree dirty before plan scope | `live_git_dirty` |
| platform HEAD changes after binding | `live_platform_drift` |
| vault HEAD changes after binding | `live_git_head_mismatch` |
| governance-log hash changes after binding | `live_vault_drift` |
| packet differs from bound packet | `live_packet_mismatch` |

For every condition, assert failure occurs before any new preparation directory, temporary evidence, approval, event, canonical write, or Git mutation is created.

Existing `test_live_promotion.py` coverage must be extended rather than duplicated where it already proves the exact condition. Tests must state whether they exercise plan-time verification or post-plan operation verification.

## Production-change policy

### M8-D01

Default expectation: test-only closure. A production change is permitted only when a new C1 test demonstrates current code violates a frozen behavior.

### M8-D02

Production change status: evidence-dependent.

- No code change when the one-time error came from fixture teardown, destination mutation, or an invalid test assumption.
- Small internal ordering or error-translation change only when a deterministic reproduction proves current behavior violates C2.
- No new public API, persistent instrumentation, generalized transaction layer, or broad refactor.

## Allowed implementation paths

Pre-authorized candidate paths, subject to exact need:

```text
src/kaizen/amendment_candidate.py
tests/test_amendment_candidate.py
tests/test_amendment_candidate_hammer.py
tests/test_live_promotion.py
```

`src/kaizen/promotion_scope.py` or `src/kaizen/windows_fs.py` may be edited only after a focused failing test proves the defect is there and the owner is informed before the edit. No other source path is pre-authorized.

## Required verification profile

Run as separate bounded steps and preserve exact outcomes:

1. syntax/compile check using the accepted platform compile profile;
2. focused loader tests;
3. focused concurrency tests;
4. focused dirty-repository and stale-binding tests;
5. at least 100 conflicting concurrency rounds;
6. existing amendment candidate hammer tests;
7. relevant static capability scan over every changed Python path;
8. full platform pytest suite;
9. repeat the focused concurrency test enough times to show stable error distribution;
10. Git diff check, hidden-Unicode scan, exact changed-path verification, and clean post-commit status.

Ruff is not required. Do not add Ruff or another dependency for symmetry.

## Required implementation return

Return an implementation result containing:

- exact starting and ending platform HEADs;
- exact changed paths and source hashes;
- whether M8-D01 required production code or tests only;
- the proven root cause of M8-D02;
- whether M8-D02 required production code;
- complete C1 case-to-test mapping and actual error codes;
- dirty/stale case-to-test mapping and actual error codes;
- 100-round creator, loser, and error-code distribution;
- all final evidence-file and preparation-hash coherence assertions;
- focused, hammer, compile, capability, and full-suite results;
- any contract correction proposed instead of implemented;
- local platform commit hash, if code or tests changed;
- confirmation that no remote was created and nothing was pushed;
- confirmation that Kaizen MCP, Go8, vault, live staging, and connectors were untouched.

## Acceptance criteria

Packet 012B is complete only when:

1. every C1 platform behavior has a direct test or an exact justified mapping to an existing test;
2. omitted packet/live binding preserves operation and evidence verification;
3. loader no-mutation and no-mutex behavior is directly proven;
4. M8-D02 has a supported root cause, not merely a green rerun;
5. at least 100 stable conflicting rounds produce exactly one creator and no mixed evidence;
6. identical contenders are idempotent and share one preparation hash;
7. partial evidence remains recovery-required;
8. dirty platform, dirty vault, stale platform HEAD, stale vault HEAD, stale governance log, and wrong packet fail closed before preparation evidence;
9. no public API or authority is widened;
10. compile, focused, hammer, capability, and full-suite checks pass;
11. changed paths equal the reviewed allowlist;
12. a separate completion security/steward audit passes;
13. the owner accepts exact implementation and audit hashes before Packet 012C begins.

## Stop gates

Stop and report before continuing when:

- any expected repository checkpoint differs;
- any test requires touching a live repository or live staging evidence;
- the loader creates evidence, acquires a write mutex, or mutates canonical state;
- concurrency produces mixed evidence or incomplete installed evidence;
- an unexpected loser code cannot be attributed to a stable pre-binding safety failure;
- production instrumentation or a public API appears necessary;
- a proposed change reaches outside the allowed platform paths;
- a dependency, Ruff adoption, remote, push, connector, Kaizen MCP, Go8, or canonical change appears necessary;
- a frozen C1 or C2 contract is contradicted by evidence;
- full-suite behavior regresses.

## Explicit non-scope

- Kaizen MCP code, tests, scripts, packaging, health, or registry;
- Go8 code, tests, scripts, health, or registry;
- connector refresh or mutation;
- process start, stop, kill, restart, PID, port, ngrok, or lifecycle work;
- dependency upgrades or Ruff adoption;
- new platform public APIs or canonical action types;
- canonical vault mutation or live staging mutation;
- remote creation, platform push, or vault push;
- generalized delete, move, correction, rollback, or multi-note transactions;
- Packet 012C, 012D, or 012E implementation;
- Milestone 9 execution;
- Postgres, Qdrant, Hermes, UI, cleanup, or deletion.

## Owner implementation gate

Implementation must not begin until the owner supplies the exact approval phrase produced by the reviewed Packet 012B audit and bound to this packet's final SHA-256.
