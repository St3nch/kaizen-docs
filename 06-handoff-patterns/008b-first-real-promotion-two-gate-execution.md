---
id: kz-tp-01KTMHA87Z529QH5ES2GWKV2AG
type: task-packet
status: active
project: kaizen-platform
summary: Govern the first real canonical promotion through a plan-only Gate A and a separately approved execution Gate B.
created: 2026-06-08T21:15:00Z
updated: 2026-06-08T21:15:00Z
review_status: pending
authority: proposed
primary_spec: 05-specs/staging-write-wrapper-and-promotion-recovery.md
related_specs:
  - 05-specs/staging-write-wrapper-and-promotion-recovery.md
  - 05-specs/staging-and-promotion-workflow.md
  - 05-specs/promotion-event-schema.md
  - 05-specs/kaizen-hammer-test-strategy.md
---

# Task Packet 008B - First Real Promotion Through Two Explicit Gates

> Security status: Gate A originally passed Result 036, but the approved exact command failed before mutation because `--json` was placed after the subcommand. Result 037 passes the corrected command for renewed owner review. Gate B remains prohibited.

## Objective

Promote one truthful, low-risk source-summary from governed staging into the canonical Kaizen vault while proving the complete first real promotion path without collapsing planning and execution into one approval.

The chosen artifact records Packet 007 governance-bootstrap completion evidence. It is useful project intelligence, has `authority: none`, and does not create doctrine, architecture decisions, or implementation authority.

## Two-Gate Structure

### Gate A - Plan and inspect only

Gate A may:

1. verify all bound commits, hashes, roots, branches, clean trees, and the empty governance log;
2. invoke `kaizen-promote-live plan` exactly once with the fixed operation ID and destination;
3. create only staging-local immutable operation evidence beneath the fixed operation directory;
4. inspect and report the exact plan, canonical candidate, validation evidence, normalized metadata, and rendered diff;
5. verify that the vault, destination parent, and governance log remain unchanged;
6. create a plan-review audit or addendum for Gate B.

Gate A may not:

- create the live destination parent;
- create approval evidence;
- run `approve`, `execute`, or `recover`;
- append a promotion event;
- modify or commit the vault;
- change the held source artifact;
- reinterpret Gate A approval as Gate B approval.

### Gate B - Approve, execute, verify, and commit

Gate B remains blocked until:

1. Gate A completed successfully;
2. the exact immutable plan and reviewed diff are available;
3. a second security review explicitly passes the generated evidence;
4. the owner explicitly approves Gate B using the exact reviewed operation ID and plan hash.

Only Gate B may authorize:

- creation of the one absent destination parent;
- live approval evidence with explicit basis and human actor;
- one live execute invocation with the fixed confirmation literal;
- recovery only if the approved operation is interrupted;
- event and canonical-file verification;
- staging and committing only the approved vault additions and promotion-log append.

## Bound State

```text
docs commit: 262b20b82ea0578c56909daaffcffe0988eed96f
platform commit: 1a890dd80d022e711f385525c202264b95d5faba
vault commit: 248b26a648142164c30d0e1126b821fce9f36cd3
platform branch: main
vault branch: main
```

All three repositories must be clean before Gate A. Do not reset, stash, restore, clean, or discard unexpected work.

## Fixed Operation Identity

```text
packet id: kz-tp-01KTMHA87Z529QH5ES2GWKV2AG
operation id: kz-prom-01KTMHA8F1HXCDXS4SBPC17VNV
human actor for Gate B: owner.local
execute confirmation literal: PROMOTE-LIVE-EXACTLY-ONCE
```

Do not generate a replacement operation ID if any precondition fails. Stop for steward review.

## Held Source Artifact

```text
path: C:\dev\kaizen\staging\projects\kaizen-platform\research\packet-007-governance-bootstrap-completion.md
note id: kz-ss-01KTME98CJ56110KTKJXKC3JPE
bytes: 2380
sha256: 119788565b83876d833917e6b7c7fdd4f35e9620c0c468202e7628825ad58f1a
staging request id: 66d692e2-0d98-447f-9eb8-e75108d0b4ae
staging intent event: 5a4c3d6a-effb-49a8-b445-256166fb0963
staging committed event: 8cfb5283-06a0-4ccc-a68f-a23d0d2ed29a
staging validation run: b0062053-da53-4099-b6c3-24df4a51a0c8
```

The source must remain byte-for-byte unchanged through both gates.

## Fixed Canonical Destination

```text
projects/kaizen-platform/research/packet-007-governance-bootstrap-completion.md
```

The parent directory is currently absent:

```text
C:\dev\kaizen\vault\projects\kaizen-platform\research
```

Gate A must not create it. Gate B may create exactly this one absent regular non-reparse directory immediately before approval/execution preflight. No other directory may be created.

## Gate A Exact Command

After explicit owner approval of Gate A, invoke exactly the fixed live entrypoint with the fixed source, destination, packet ID, and operation ID:

```powershell
C:\dev\kaizen\platform\.venv\Scripts\kaizen-promote-live.exe --json plan `
  C:\dev\kaizen\staging\projects\kaizen-platform\research\packet-007-governance-bootstrap-completion.md `
  --destination projects/kaizen-platform/research/packet-007-governance-bootstrap-completion.md `
  --packet-id kz-tp-01KTMHA87Z529QH5ES2GWKV2AG `
  --operation-id kz-prom-01KTMHA8F1HXCDXS4SBPC17VNV
```

No wrapper script, alternate Python entrypoint, changed timestamp, alternate destination, replacement operation ID, or root override is authorized.

## Gate A Preflight

Before plan generation, prove:

- docs HEAD equals `262b20b82ea0578c56909daaffcffe0988eed96f` and tree is clean;
- platform HEAD equals `1a890dd80d022e711f385525c202264b95d5faba`, branch is `main`, and tree is clean;
- vault HEAD equals `248b26a648142164c30d0e1126b821fce9f36cd3`, branch is `main`, and tree is clean;
- fixed live roots resolve exactly to `C:\dev\kaizen\vault` and `C:\dev\kaizen\staging`;
- governance README and log are regular non-reparse files;
- promotion log size is zero and SHA-256 is `e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855`;
- held source SHA-256 is `119788565b83876d833917e6b7c7fdd4f35e9620c0c468202e7628825ad58f1a`;
- staging-write log SHA-256 is `07b09195bd04852fd6d2f12dc6a3c878b85f964eec2cff850b496352bd4b8a52`;
- the fixed operation directory does not exist;
- the canonical destination and destination parent are absent;
- no unrelated promotion process is active;
- no prior owner approval of Gate B exists.

## Gate A Expected Mutation

Gate A may create only:

```text
C:\dev\kaizen\staging\_promotion\operations\kz-prom-01KTMHA8F1HXCDXS4SBPC17VNV\canonical-candidate.md
C:\dev\kaizen\staging\_promotion\operations\kz-prom-01KTMHA8F1HXCDXS4SBPC17VNV\validation.json
C:\dev\kaizen\staging\_promotion\operations\kz-prom-01KTMHA8F1HXCDXS4SBPC17VNV\plan.json
```

No `approval.json` may exist after Gate A.

## Gate A Required Inspection

Record and verify:

```text
operation id
plan sha256
source sha256
canonical-candidate sha256
validation-result sha256
validation run id
validation errors/warnings
normalized metadata
prior/new review status
prior/new authority
destination path
live binding version
packet id
canonical root
staging root
vault branch/head
platform head
governance-log sha256
rendered diff text and sha256
operation-directory inventory and hashes
```

The canonical candidate must:

- retain note ID `kz-ss-01KTME98CJ56110KTKJXKC3JPE`;
- retain `authority: none`;
- normalize `review_status` from `pending` to `approved`;
- remove staging-only `validation_status`;
- update only metadata required by the promotion contract;
- preserve the source-summary body meaning and provenance;
- pass canonical validation with zero errors;
- have no unresolved relationships, broken canonical links, duplicate note ID, or duplicate canonical summary.

## Gate A Stop Conditions

Stop without further mutation if:

- explicit Gate A approval is absent;
- any bound repository or hash differs;
- the operation directory already exists;
- the destination parent or destination unexpectedly exists;
- the governance log is nonempty or has a different hash;
- plan generation returns nonzero;
- any file beyond the three allowed operation files appears;
- `approval.json` appears;
- the plan does not hash-bind the exact packet, roots, Git state, source, destination, validation, and governance log;
- canonical validation reports an error;
- the rendered diff contains unreviewed body changes;
- the vault or live governance log changes.

Do not repair, regenerate, delete, or retry automatically. Preserve evidence and stop for steward review.

## Gate A Acceptance Criteria

Gate A passes only when:

- explicit owner approval named Gate A;
- one immutable operation directory exists with exactly candidate, validation, and plan evidence;
- all hashes and bindings verify;
- no approval evidence exists;
- the destination parent remains absent;
- vault Git status remains clean at `248b26a`;
- promotion log remains zero bytes and zero events;
- source and staging-write log hashes remain unchanged;
- a Gate B evidence review is created and presented to the owner.

## Gate B Review Requirements

Before requesting Gate B approval, the steward must present:

- exact operation ID and plan hash;
- exact candidate hash;
- complete normalized frontmatter changes;
- rendered diff and diff hash;
- validation evidence and run ID;
- exact destination-parent creation action;
- expected intent and committed event fields;
- exact vault Git diff and commit scope expected after execution;
- recovery decision tree;
- any limitation or deviation.

Gate B approval must explicitly name:

```text
Packet 008B Gate B
operation id
plan sha256
```

A generic “proceed” or prior Packet 008B Gate A approval is insufficient.

## Gate B Allowed Mutation

After explicit Gate B approval, Gate B may:

1. reverify every bound hash, plan, candidate, validation record, clean repository, and empty log condition;
2. create exactly `C:\dev\kaizen\vault\projects\kaizen-platform\research` as an absent regular non-reparse directory;
3. invoke live `approve` once with actor `owner.local`, matching packet ID, and an explicit basis referencing the owner-approved Gate B plan hash;
4. verify the resulting approval evidence and approval hash;
5. invoke live `execute` once with actor `owner.local`, matching packet ID, and confirmation `PROMOTE-LIVE-EXACTLY-ONCE`;
6. use `recover` only if the approved execution is interrupted and only after recovery preflight classifies all dirty paths as operation-owned;
7. verify exactly one canonical file and exactly one intent plus one committed or reviewed recovered terminal event;
8. review the complete vault Git diff;
9. stage and commit only the destination file and append-only promotion log change with exact commit message:

```text
Promote Packet 007 governance bootstrap evidence
```

No vault remote exists or is required. Do not push or create a remote.

## Gate B Non-Scope

Gate B does not authorize:

- any second promotion;
- amendment, supersedence, correction, overwrite, deletion, or rollback;
- editing the staged source;
- modifying existing canonical notes;
- changing platform or docs code;
- staging operation evidence into Git;
- moving or deleting staging evidence;
- creating any directory other than the exact research parent;
- committing `_governance/README.md` again;
- creating a vault remote or pushing;
- Hermes, MCP, LangGraph, Postgres, Qdrant, Observatory, providers, UI, websites, or deployment work.

## Gate B Acceptance Criteria

Gate B passes only when:

- the second owner approval exactly matches the reviewed operation ID and plan hash;
- the approval evidence hash and packet ID verify;
- the live execution returns committed or a reviewed deterministic recovered-committed result;
- the canonical destination hash equals the approved candidate hash;
- the source remains present and unchanged;
- the promotion log parses and contains exactly the approved operation's intent and terminal evidence with no unrelated event;
- no temporary file remains;
- only the destination file and promotion-log append are staged;
- `git diff --cached --check` passes;
- the exact local vault commit succeeds;
- vault, platform, and docs trees end clean;
- no push or remote creation occurs;
- a completion steward audit records all hashes, events, Git evidence, and deviations.

## Failure and Recovery

### Gate A failure before ready plan

The existing planner may remove an incomplete operation directory before `plan.json` exists. Preserve any remaining evidence and stop. Do not create a replacement operation ID automatically.

### Gate A ready-plan failure

Once `plan.json` exists, operation evidence is immutable. Do not patch or regenerate it. Any defect requires a new separately reviewed packet and operation ID.

### Gate B failure before intent

No canonical mutation or event should exist. Preserve approval and plan evidence, remove the newly created empty research parent only if it is proven packet-owned and still empty, then stop for review.

### Gate B interruption after intent

Do not rerun execute blindly. Run recovery preflight. Recovery may proceed only when dirty paths are limited to:

```text
_governance/promotion-log.jsonl
projects/kaizen-platform/research/packet-007-governance-bootstrap-completion.md
projects/kaizen-platform/research/.kz-prom-01KTMHA8F1HXCDXS4SBPC17VNV.candidate.tmp
```

Any unrelated path blocks recovery without cleanup.

### After vault commit

No automatic rollback is authorized. Reversal requires a separately reviewed human Git and Kaizen correction/supersedence action.

## Completion Reports

### Gate A report

```text
result
owner Gate A approval basis
preflight commits and hashes
operation directory inventory
plan JSON and plan sha256
candidate sha256
validation JSON and hash
validation run and counts
rendered diff and hash
live binding
vault/log/source preservation checks
failures or deviations
Gate B recommendation
```

### Gate B report

```text
result
owner Gate B approval basis
operation id and plan sha256
parent creation evidence
approval JSON and hash
execute/recover result
canonical path/size/hash
event sequence and hashes
source preservation
vault staged paths and diff check
new vault commit
post-commit repository statuses
failures or deviations
```
