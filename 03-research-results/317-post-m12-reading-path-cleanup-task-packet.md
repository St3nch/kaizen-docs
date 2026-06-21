# Packet 018A — Post-Milestone-12 Reading-Path Cleanup Task Packet

Status: task packet draft
Date: 2026-06-21
Packet ID: 018A
Scope class: documentation cleanup only
Authority lane: owner approval required before edits

## 1. Purpose

Packet 018A prepares the bounded documentation cleanup needed after the post-Milestone-12 audit program Passes 1 through 3.

The problem is not a broken milestone evidence chain. Result 312 and Result 314 accept that Milestones 7 through 12 are clean and followable end-to-end.

The problem is the active reading path:

```text
The docs evidence chain is intact, but the files a fresh agent reads first are stale, contradictory, and missing key pointers.
```

This packet is designed to repair that orientation layer without reopening closed milestones, inventing missing decisions, modifying platform code, mutating the vault, or selecting a next milestone.

## 2. Source bindings

This packet is based on the following accepted audit and steward-disposition results.

```text
Result 311 — Post-M12 roadmap and entrypoint audit
Source SHA-256: a7da10fbb3f93ab77abb9f3211bda637c8f3bb1b48de3341641af49066c93e27

Result 313 — Pass 1 steward disposition
Source SHA-256: 689628354191e840ddbfa76c0348b5390cefa550e41979698169104f237147ee

Result 312 — Post-M12 milestone traceability audit
Source SHA-256: 5075e5c53b74d2209c738f26c671b552cf6de180335edb784fd46bfe112c6d53

Result 314 — Pass 2 steward disposition
Source SHA-256: e8f4f15c0ada79d4162be600fdeae98388e3f3a48aad94fdee77f94ea0231a83

Result 315 — Post-M12 historical audit-signal disposition audit
Source SHA-256: 5c953f62fc0df996ee9b13888047e84c27e693c236326e17657c99cca2a08756

Result 316 — Pass 3 steward disposition
Source SHA-256: 32c6ed55d7f0acab90ea1cf1cc7b4a74a5e30bdd07d62e163b9da3ef43b53ab2
```

## 3. Starting checkpoint

Approved execution of this packet must bind to the exact docs repository commit present at approval time.

Current draft checkpoint observed while preparing this packet:

```text
docs HEAD: d2478cb3f7e776f0555fce90e2d2049e24f6551d
branch: main
remote/upstream: origin/main
working tree: clean
```

This checkpoint is informational until owner approval. Execution must re-verify it or use a later owner-approved starting commit.

## 4. Current in-scope file hashes

These hashes bind the files proposed for cleanup planning.

```text
00-entrypoint/LLM_START_HERE.md
SHA-256: 888e848b94a63200a211ccfd613608c36776880125affe071262969f1d7442db
line endings: LF

README.md
SHA-256: a86cb59f999d034e332af6511c0f3e1894898e4db79eb9143758da5a2820bc39
line endings: LF

ROADMAP_V0.3.md
SHA-256: 595fa063ea4c067280e5a7189fdef2ee24de720270e1e175f1e55644ad1be3ec
line endings: LF

ROADMAP.md
SHA-256: 0418bfc6c30823774fb2964d188f517cd020ef5ddbef2b44d04efe1b0274754f
line endings: LF

IMPLEMENTATION_ROADMAP.md
SHA-256: 0a73cb4984c28a0654d5c436a78352ac1dcdb9202b6142aa5b1d74c7167665a0
line endings: LF

IMPLEMENTATION_ROADMAP_V0.2.md
SHA-256: 1dd4eb3ae80b0d855091b26cf598b9a744a5141648b4739580bb90871f2e11bc
line endings: CRLF
```

## 5. Authorized cleanup objective

After implementation, a fresh steward should be able to read the active entrypoint and roadmap and immediately understand:

```text
Kaizen purpose and current operating standard;
Milestones 1 through 12 are closed;
Milestone 12 closure authority is Result 307;
post-M12 audit Passes 1 through 3 are complete and dispositioned;
the next valid gate is bounded roadmap/entrypoint cleanup or a later owner-approved roadmap decision, not Milestone 13 implementation;
M12 deferred boundaries remain live and tracked;
P3 historical-disposition repair items remain tracked and non-blocking;
old roadmap files are historical unless explicitly named current.
```

## 6. Files authorized for modification

### 6.1 Primary active reading path

```text
00-entrypoint/LLM_START_HERE.md
README.md
ROADMAP_V0.3.md
```

### 6.2 Historical roadmap surfaces

```text
ROADMAP.md
IMPLEMENTATION_ROADMAP.md
```

### 6.3 Conditional / preserve-bytes-sensitive file

```text
IMPLEMENTATION_ROADMAP_V0.2.md
```

This file has a preserve-bytes concern and CRLF line endings. The preferred approach is to avoid editing its body unless the implementation return explicitly proves the preservation tradeoff and owner intent.

Acceptable handling options:

```text
Option A: leave IMPLEMENTATION_ROADMAP_V0.2.md bytes unchanged and reframe it from README / entrypoint / ROADMAP_V0.3 as historical.
Option B: add a minimal historical banner only if the implementation plan explicitly accepts that this changes the file bytes and preserves the original historical snapshot through Git history.
```

Option A is preferred for first cleanup.

### 6.4 Decisions index

```text
04-design-decisions/README.md
```

This file may be modified only to correct stale index/status claims and to make the Decision 0015 gap explicit without authoring Decision 0015 doctrine.

## 7. Files not authorized for modification

This packet does not authorize modification of:

```text
platform repository files
vault repository files
staging evidence
Kaizen MCP files
Go8 files
migration SQL
PostgreSQL databases
backup archives
generated evidence files
existing accepted result files other than this packet's own future implementation-return records
04-design-decisions/0015-*.md, unless separately owner-approved
```

This packet does not authorize moving, deleting, or renaming historical evidence files.

## 8. Required edits by file

### 8.1 `00-entrypoint/LLM_START_HERE.md`

Required outcome:

```text
Make this the single authoritative current-state pointer.
```

Required content:

```text
current docs/platform/vault checkpoints as of implementation;
latest closed milestone: Milestone 12;
Milestone 12 owner closure authority: Result 307;
latest audit state: Results 310 through 316, with Passes 1–3 complete and dispositioned;
next gate: bounded documentation cleanup / post-M12 roadmap decision only, not implementation;
M12 deferred boundaries;
P3 repair-register pointer;
read-first order;
standing prohibitions.
```

Required removals / corrections:

```text
remove stale claim that Milestones 1 through 11 are the latest closed state;
remove stale post-M11-only next-gate wording;
remove stale docs/platform checkpoint claims if they no longer match implementation-time evidence;
correct the false claim that Decisions 0001 through 0020 all exist as accepted governing records while Decision 0015 is absent.
```

Decision 0015 handling:

```text
Do not create Decision 0015 doctrine.
State that Decision 0015 is a reserved / unauthored / owner-deferred governance-compression gap pending later disposition, if mentioned.
```

### 8.2 `README.md`

Required outcome:

```text
Convert README.md into a stable project overview and repository-boundary guide.
```

Required content:

```text
what Kaizen is;
the governed project loop;
repo boundary summary;
stewardship rule;
clear pointer to 00-entrypoint/LLM_START_HERE.md for current state and next gate.
```

Required removals / corrections:

```text
remove stale milestone-count claims;
remove stale HEAD/checkpoint claims;
remove stale next-gate claims such as Packet 016C implementation approval;
remove or update old read-first labels that imply M10/M11 is current.
```

README should not act as the active status ledger after this cleanup.

### 8.3 `ROADMAP_V0.3.md`

Required outcome:

```text
Refresh or supersede ROADMAP_V0.3.md as the single active roadmap surface.
```

Required content:

```text
current project standard pointer;
closed milestone table through Milestone 12;
active planning gate;
candidate next milestone lanes as candidates only;
explicit deferred-boundary table;
audit/backlog pointer;
standing prohibitions;
required gate sequence;
traceability links for each closed milestone.
```

Closed milestone table requirements:

```text
For at least Milestones 7 through 12, include links to:
- definition/spec;
- owner acceptance;
- closure result/audit;
- owner closure acceptance;
- residual/deferred-boundary summary.
```

Milestone 9 requirement:

```text
Explicitly state that Milestone 9's definition/spec is:
05-specs/controlled-implementation-return-pilot.md
```

Milestone 12 requirement:

```text
State that Milestone 12 is closed under Result 307.
Carry forward M12 deferred boundaries.
```

Post-M12 audit requirement:

```text
Point to Results 310–316 and state that Passes 1–3 are complete and dispositioned.
State that historical audit-signal repair items P3-001 through P3-009 are tracked and non-blocking for roadmap refresh unless later escalated.
```

Required removals / corrections:

```text
remove stale claim that no sequence after Milestone 11 is accepted;
remove or correct any ROADMAP.md.bak reference that implies an active canonical source;
remove stale forward sequence that ends at Milestone 11 as if current.
```

### 8.4 `ROADMAP.md`

Required outcome:

```text
Demote this file to historical planning provenance unless explicitly maintained.
```

Required repair:

```text
Remove or replace the claim that it contains a maintained current-gate summary.
Remove or clearly label the stale Milestone 5 current-position block as historical.
Point current readers to 00-entrypoint/LLM_START_HERE.md and the active roadmap.
```

Avoid large historical rewrites.

### 8.5 `IMPLEMENTATION_ROADMAP.md`

Required outcome:

```text
Reframe as historical completed first-slice roadmap for Milestones 1 through 5.
```

Required repair:

```text
Remove or replace active-planning status language.
Remove or label stale next-action text as historical.
Point current readers to the active entrypoint and active roadmap.
```

Avoid large historical rewrites.

### 8.6 `IMPLEMENTATION_ROADMAP_V0.2.md`

Preferred outcome:

```text
Leave bytes unchanged and reframe it from other files as a completed historical Milestone 6 snapshot.
```

Conditional repair if owner-approved within implementation:

```text
Add a minimal historical banner without changing body semantics.
```

This conditional repair must explicitly record the byte-change decision in the implementation return.

### 8.7 `04-design-decisions/README.md`

Required outcome:

```text
Remove stale impression that the decision ledger stops at Decision 0013.
```

Required content if modified:

```text
Decision 0015 is currently absent / reserved / unauthored / owner-deferred pending later disposition.
Decisions 0016 through 0020 exist and must not be hidden by a stale index.
```

Boundary:

```text
Do not author Decision 0015 doctrine.
Do not infer that Decision 0015 was accepted.
```

## 9. Required tracked backlog pointer

The cleanup must preserve a pointer to the Pass-3 repair register:

```text
P3-001  M11 Passes 4–9 / final synthesis preservation gap
P3-002  M11 A005 test-count reconciliation gap
P3-003  M11 B001/B006/B010 low still-open items
P3-004  M6–8 KZA per-ID disposition register
P3-005  Decision 0015 reserved/gap record
P3-006  decisions index README stale
P3-007  Result 302 expansion sweep
P3-008  watchlist + Observatory/OBR backlog families
P3-009  milestone-closeout refresh process repair
```

The active roadmap may summarize these and link to Result 316 rather than duplicating full text.

## 10. Milestone-closeout refresh process rule

If scope remains clean, implementation should add a lightweight standing rule to the active roadmap or entrypoint:

```text
After every future milestone closure, the steward must verify and, if needed, refresh the active reading path: entrypoint, README status pointer, active roadmap, decisions index if affected, and deferred-boundary table.
```

This addresses Result 315 / P3-009.

If adding this rule causes scope creep, defer it explicitly as P3-009 rather than losing it.

## 11. Acceptance criteria

Packet 018A implementation is acceptable only if all of the following are true:

```text
1. 00-entrypoint/LLM_START_HERE.md no longer presents M11/016G as the latest state.
2. README.md no longer presents stale milestone counts, stale repository checkpoints, or stale next-gate claims.
3. ROADMAP_V0.3.md no longer says no sequence after M11 is accepted.
4. The active reading path states Milestone 12 is closed and links to Result 307.
5. M12 deferred boundaries are visible from the active reading path.
6. M7–M12 traceability is surfaced in the active roadmap or an equivalent active roadmap table.
7. Milestone 9's spec path is explicitly identified as 05-specs/controlled-implementation-return-pilot.md.
8. Decision 0015 is not falsely presented as an accepted governing decision.
9. The P3 repair backlog is referenced and not treated as closed.
10. Historical roadmap files no longer pretend to be the current gate source.
11. IMPLEMENTATION_ROADMAP_V0.2.md is either left byte-unchanged or any byte change is explicitly justified as owner-approved within the implementation return.
12. No platform, vault, database, staging, or evidence files are modified.
13. Git diff contains only authorized documentation files.
14. Git diff check passes with no conflict markers or whitespace errors.
15. Final docs working tree is clean after commit.
```

## 12. Verification requirements

Implementation return must include:

```text
starting docs commit;
changed paths;
before/after SHA-256 for each changed file;
summary of each required edit;
proof that no unauthorized paths changed;
git diff-check result;
final docs commit;
final clean/synced status.
```

If any required edit is intentionally deferred, the return must name it and explain why it does not violate acceptance criteria.

## 13. Explicit exclusions

Packet 018A does not authorize:

```text
platform mutation
database mutation
vault mutation
staging evidence mutation
backup archive mutation
physical evidence deletion
retention deletion
production deployment
Milestone 13 acceptance
next-milestone implementation
new operational record families
schema redesign
migration changes
PostgreSQL changes
Kaizen MCP changes
Go8 changes
Observatory work
Qdrant work
Hermes/UI work
provider work
customer data work
direct agent SQL
arbitrary SQL APIs
production MCP packaging
authoring Decision 0015 doctrine
fabricating missing M11 audit text
reopening closed milestones absent concrete contradictory evidence
broad historical rewrite
file moves/renames unless separately justified and approved
```

## 14. Recommended implementation sequence

```text
1. Verify docs repository is clean and at owner-approved starting commit.
2. Read source bindings: Results 313, 314, 316.
3. Read current target files and confirm hashes.
4. Update entrypoint first.
5. Simplify README second.
6. Refresh active roadmap third.
7. Add minimal historical banners / status corrections to historical roadmap files.
8. Handle decisions index README if included.
9. Re-check Decision 0015 wording across active reading path.
10. Re-check M12 deferred-boundary visibility.
11. Re-check P3 backlog pointer visibility.
12. Run diff check and status verification.
13. Commit with a narrow documentation-cleanup message.
14. Record implementation return.
```

## 15. Owner approval wording

Suggested owner approval phrase:

```text
Approve Packet 018A documentation cleanup using docs starting commit <COMMIT> and task packet SHA-256 <SHA-256>. Scope is limited to the active reading-path cleanup and authorized documentation files. No platform, vault, database, staging, production, deletion, Decision 0015 doctrine, or next-milestone work is authorized.
```

## 16. Non-authorization

This packet draft does not itself authorize edits. It prepares the cleanup scope for owner review and approval.
