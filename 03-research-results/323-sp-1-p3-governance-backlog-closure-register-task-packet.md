# SP-1 — P3 Governance-Backlog Closure Register Task Packet

Status: task packet draft
Date: 2026-06-21
Packet: SP-1
Scope class: documentation / backlog-register cleanup only
Authority lane: owner approval required before implementation

## 1. Purpose

SP-1 creates a durable closure/disposition register for the P3 historical-governance backlog preserved after the post-Milestone-12 audit program.

The goal is simple:

```text
Do not let P3-001 through P3-009 rot in an audit file where future agents forget them.
```

This packet does not reopen closed milestones, reconstruct unavailable audit text, create Decision 0015 doctrine, start Milestone 13, or authorize implementation work outside bounded documentation cleanup.

## 2. Current project posture

At draft time, the verified docs repository posture is:

```text
root: docs
branch: main
HEAD: d16a401cd551ab7cfccdd1557bd1eeefbff8f697
latest commit: Accept ROADMAP v0.4
working tree: clean
upstream: origin/main
ahead/behind: 0 / 0
```

Current accepted roadmap:

```text
ROADMAP_V0.4.md
SHA-256: 8fe232fd4ea7c977499690b35e6fb6a112663ddced38824f88e619bd8ef8ab52
status: accepted and active
owner acceptance: 03-research-results/322-roadmap-v0.4-owner-acceptance.md
```

Current active entrypoint:

```text
00-entrypoint/LLM_START_HERE.md
SHA-256: 9f395f8325d4521e45cae04bdbab57b34f89e77e02f1e2fb8f481711d02dd1a3
```

Current state:

```text
Milestones 1-12 closed
Packet 018A closed
ROADMAP_V0.4 accepted and active
No Milestone 13 implementation authorized
No implementation authorized by this packet draft
```

## 3. Source bindings

SP-1 is based on these governing sources:

```text
03-research-results/316-post-m12-audit-pass-3-steward-disposition.md
SHA-256: 32c6ed55d7f0acab90ea1cf1cc7b4a74a5e30bdd07d62e163b9da3ef43b53ab2

03-research-results/321-packet-018a-owner-acceptance.md
SHA-256: 26ddbf0184e57f3b6cafdf4e81479a97b89ff1db74187700311700bb7ac6b7fc

03-research-results/322-roadmap-v0.4-owner-acceptance.md
SHA-256: 70d553726a365226f49bc43afc9e72809d60ca6f0f8c03b997ef7eb213a6267d

ROADMAP_V0.4.md
SHA-256: 8fe232fd4ea7c977499690b35e6fb6a112663ddced38824f88e619bd8ef8ab52

04-design-decisions/README.md
SHA-256: 77c933b35a455ede2a721f41d91caad5ce42b45528c7ae44d84b380db340b800
```

Supporting source if needed:

```text
03-research-results/315-post-m12-historical-disposition-audit.md
SHA-256: 5c953f62fc0df996ee9b13888047e84c27e693c236326e17657c99cca2a08756
```

## 4. Why this packet exists

Result 316 accepted P3-001 through P3-009 as tracked repair items. Packet 018A repaired the active reading path and decisions index enough to prevent immediate drift, but Packet 018A did not close the P3 backlog.

ROADMAP_V0.4 carries SP-1 as a side packet candidate and defines Milestone 13 entry criteria as:

```text
SP-1 complete or explicitly owner-deferred
```

Therefore SP-1 should either be completed now or explicitly owner-deferred before Milestone 13 definition work begins.

## 5. Authorized objective

Create a durable P3 governance-backlog closure register that records each item P3-001 through P3-009 as one of:

```text
closed
deferred
rejected
superseded
tracked
owner-deferred
unavailable / chat-sourced
still-open
```

The register must preserve source lineage and make clear which items remain future work.

## 6. Files authorized for creation or modification

Preferred new file:

```text
03-research-results/324-sp-1-p3-governance-backlog-closure-register.md
```

Optional implementation-return file if the packet is later implemented:

```text
03-research-results/325-sp-1-implementation-return.md
```

Optional owner-acceptance file if implementation is accepted:

```text
03-research-results/326-sp-1-owner-acceptance.md
```

Optional narrow active-roadmap correction, only if the implementation owner explicitly includes it:

```text
ROADMAP_V0.4.md
```

Allowed ROADMAP_V0.4 correction scope is limited to the stale tail wording in `Immediate next planning work`, replacing roadmap-audit / owner-acceptance draft language with current accepted-roadmap posture and SP-1 / M13 planning sequence.

Do not touch other active docs unless separately approved.

## 7. Files not authorized for modification

SP-1 does not authorize modification of:

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
accepted milestone closure records
historical audit files
missing M11 audit text
Decision 0015 doctrine files
source repositories
```

SP-1 does not authorize moving, deleting, or renaming historical evidence files.

## 8. Required register contents

The new P3 register must include:

```text
source binding to Result 316;
brief purpose;
current project posture;
one row or subsection for each P3 item;
source evidence known from Result 316;
allowed disposition;
current recommended disposition;
owner-decision requirement;
blocking status for M13;
implementation notes;
explicit prohibitions;
next-step effect after register creation.
```

At minimum, it must record these items.

### P3-001 — M11 Passes 4-9 / final synthesis preservation gap

Required register treatment:

```text
Disposition: tracked / unavailable-chat-sourced unless recovered later.
Do not fabricate missing M11 audit text.
Do not reopen Milestone 11 absent concrete contradictory evidence.
Record that known M11 post-closure issues were partly closed by Packet 016G and partly carried as deferred or low-open items.
```

Owner decision required:

```text
yes, if the owner wants a future recovery attempt from chat history or wants the item marked owner-deferred.
```

M13 blocker:

```text
no, unless owner escalates it.
```

### P3-002 — M11 A005 test-count reconciliation gap

Required register treatment:

```text
Disposition: tracked / unavailable-chat-sourced or owner-deferred.
Record that the original 394 -> 386 unique-test-count explanation is not currently preserved in docs unless chat evidence is recovered.
```

Owner decision required:

```text
yes, if recovery is desired; otherwise owner-defer.
```

M13 blocker:

```text
no, unless owner escalates it.
```

### P3-003 — M11 B001 / B006 / B010 low still-open items

Required register treatment:

```text
Disposition: still-open / tracked low.
B001: migration hash semantics, current-safe while files are LF/no-BOM.
B006: canonical current-provenance / manifest-audit read semantics consumer-trap.
B010: service-layer vs migration advisory-lock namespace comparison.
```

Owner decision required:

```text
no, unless escalated later.
```

M13 blocker:

```text
no, but M13 definition may reference them if relevant to service/tool hardening.
```

### P3-004 — M6-8 KZA per-ID disposition register

Required register treatment:

```text
Disposition: tracked / deferred register candidate.
Create no fake KZA closures.
Record that future per-ID disposition may use Result 166, Result 167, and Packet 013A-013E outcomes.
```

Owner decision required:

```text
yes, if the later KZA register surfaces still-open or unclear items.
```

M13 blocker:

```text
no.
```

### P3-005 — Decision 0015 reserved/gap record

Required register treatment:

```text
Disposition: partially closed for active reading-path false claim; still tracked for final ledger treatment.
04-design-decisions/README.md now explicitly says no Decision 0015 file exists and warns not to fabricate doctrine.
Do not create Decision 0015 doctrine.
```

Owner decision required:

```text
yes, only if owner wants a future Decision 0015 governance-compression decision or a final reserved-number ledger record.
```

M13 blocker:

```text
no.
```

### P3-006 — decisions index README stale

Required register treatment:

```text
Disposition: closed or substantially closed by Packet 018A.
04-design-decisions/README.md now records Decisions 0001-0010 accepted, Decision 0011 proposed, Decisions 0012-0014 accepted, Decision 0015 absent/reserved/unauthored/owner-deferred, and Decisions 0016-0020 accepted.
```

Owner decision required:

```text
no.
```

M13 blocker:

```text
no.
```

### P3-007 — Result 302 expansion sweep

Required register treatment:

```text
Disposition: tracked / deferred sweep candidate.
Later sweep should cover ADR-011 early architecture/tool/interface/provider audits from Results 007-017, Result 025 deferred Gate-E tail, and ADR-004 partly-proved / unproved categories.
```

Owner decision required:

```text
no, unless later sweep surfaces blockers.
```

M13 blocker:

```text
no.
```

### P3-008 — watchlist plus Observatory / OBR backlog families

Required register treatment:

```text
Disposition: tracked / deferred to future Observatory / OBR gates.
Carry fresh-context resumption report family, failure-injection evidence family, Observatory/OBR rights-retention-measurement, and related operational-data / research backlog items as future candidates.
```

Owner decision required:

```text
no, unless promoted to a milestone candidate.
```

M13 blocker:

```text
no.
```

Boundary:

```text
Do not start Observatory, OBR, provider, crawling, marketplace collection, customer-data, or IMI implementation work.
```

### P3-009 — milestone-closeout refresh process repair

Required register treatment:

```text
Disposition: tracked process repair / should-fix.
Record requirement that every future milestone closure includes active reading-path refresh or verification: entrypoint, README status pointer, active roadmap, decisions index if affected, deferred-boundary table, and current next-gate statement.
```

Owner decision required:

```text
yes, if encoded as doctrine or standing process rule beyond this register.
```

M13 blocker:

```text
no, but M13 definition should include a closeout reading-path refresh check.
```

## 9. Acceptance criteria

SP-1 implementation is acceptable only if all of the following are true:

```text
1. A durable P3 register exists.
2. P3-001 through P3-009 each have an explicit disposition.
3. No missing M11 audit text is fabricated.
4. Decision 0015 doctrine is not authored or implied.
5. Closed milestones are not reopened.
6. P3-006 is recognized as closed/substantially closed if the decisions README remains at SHA 77c933b35a455ede2a721f41d91caad5ce42b45528c7ae44d84b380db340b800 or equivalent content.
7. Remaining P3 items are clearly tracked, deferred, owner-deferred, still-open, or unavailable.
8. The register states whether SP-1 completion clears the ROADMAP_V0.4 M13 entry condition.
9. No platform, vault, database, staging, production, provider, customer-data, Qdrant, Hermes, Tauri, or MCP implementation work occurs.
10. Git diff contains only authorized documentation files.
11. Final docs working tree is clean after commit.
```

## 10. Verification requirements

Implementation return must include:

```text
starting docs commit;
changed paths;
before/after SHA-256 for each changed file;
summary of P3 dispositions;
proof no unauthorized paths changed;
git diff-check result;
final docs commit;
final clean/synced status.
```

If any P3 item is intentionally left open, the implementation return must name it and explain why SP-1 can still close as a register packet.

## 11. Explicit exclusions

SP-1 does not authorize:

```text
Milestone 13 implementation
Milestone 13 acceptance
platform mutation
vault mutation
staging evidence mutation
database mutation
migration changes
PostgreSQL changes
new operational record families
governed-operation read model implementation
connector telemetry implementation
retention deletion
physical evidence deletion
production deployment
production MCP packaging
Qdrant
Hermes integration
Tauri or UI implementation
Observatory / IMI implementation
OBR implementation
provider purchase
crawling
logged-in marketplace collection
customer-data reuse
Neon Ronin implementation
SearchClarity implementation
Decision 0015 doctrine creation
fabrication of missing M11 audit text
reopening closed milestones absent concrete contradictory evidence
broad historical rewrite
file moves or renames
```

## 12. Recommended implementation sequence

```text
1. Verify docs repository is clean and at owner-approved starting commit.
2. Re-read Result 316, Packet 018A owner acceptance, Result 322, ROADMAP_V0.4, and decisions README.
3. Create the P3 register as a new docs result file.
4. Optionally correct ROADMAP_V0.4 stale tail wording only if explicitly approved in implementation scope.
5. Verify P3-001 through P3-009 each has a disposition.
6. Re-check no Decision 0015 doctrine was created.
7. Re-check no fabricated M11 audit text was added.
8. Run git diff and diff-check.
9. Commit the authorized docs-only changes.
10. Record implementation return.
```

## 13. Owner approval wording

Suggested owner approval phrase:

```text
Approve SP-1 P3 governance-backlog closure register implementation using docs starting commit d16a401cd551ab7cfccdd1557bd1eeefbff8f697 and task packet SHA-256 <SHA-256>. Scope is limited to creating the P3 register and, if included, the narrow ROADMAP_V0.4 stale-tail wording correction. No platform, vault, database, staging, production, provider, customer-data, Qdrant, Hermes, Tauri, Observatory, Decision 0015 doctrine, missing-audit reconstruction, or Milestone 13 work is authorized.
```

## 14. Non-authorization

This task packet draft does not authorize implementation by itself. Implementation requires exact owner approval bound to this packet SHA-256 and the verified starting docs commit.
