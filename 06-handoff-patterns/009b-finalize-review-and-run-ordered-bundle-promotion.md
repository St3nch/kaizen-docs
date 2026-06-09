---
id: kz-tp-01KTMNHB6NZ5YBJACWZM92HGJE
type: task-packet
status: active
project: kaizen-platform
summary: Finalize the staged audit verdict and govern ordered promotion of the six-note Milestone 4 planning bundle through explicit plan-hash gates.
created: 2026-06-08T22:28:09Z
updated: 2026-06-09T00:31:20Z
review_status: pending
authority: proposed
primary_spec: 05-specs/staging-and-promotion-workflow.md
related_specs:
  - 05-specs/staging-and-promotion-workflow.md
  - 05-specs/promotion-event-schema.md
  - 05-specs/staging-write-wrapper-and-promotion-recovery.md
---

# Task Packet 009B - Finalize Review and Run Ordered Bundle Promotion

> Wave 1 status: complete at vault commit `999ccb0`; Result 046 passed. Wave 2 claim plan generation awaits explicit owner approval for operation `kz-prom-01KTMNHB6NZ5YBJACWZM92HGJH`. Wave 2 execution and Waves 3 through 6 remain prohibited.

## Objective

Finalize the human verdict in the staged audit and promote the six-note Milestone 4 planning bundle in dependency order without weakening the exact-plan approval model proven by Packet 008B.

## Read First

- `03-research-results/042-packet-009a-completion-steward-audit.md`
- `06-handoff-patterns/009a-generate-milestone-4-governed-planning-bundle.md`
- `05-specs/staging-and-promotion-workflow.md`
- `05-specs/promotion-event-schema.md`
- `05-specs/staging-write-wrapper-and-promotion-recovery.md`
- all six staged bundle notes

## Bound State

```text
docs commit: 09146c5
platform commit: 1a890dd80d022e711f385525c202264b95d5faba
vault commit: 80bc0932c505770a57dd8790f105cca1915f9a23
staging log sha256: 3419bc43e1588c7e97d44626bb3e9e263cd3d109647f79c8bda150c422905059
live promotion log sha256: fd94f460d655f2be7327f0349f175cf4be80973f51b0660fb0c0e339fbeb772a
```

## Phase 0 - Exact Audit Verdict Finalization

The staged audit currently has:

```text
path: projects/kaizen-platform/audits/governed-amendment-support-audit.md
sha256: 746310f46d5b31337ac656653d10b244bbea01686ff802a6f92538b960d8c26c
```

Initial Packet 009B approval may authorize one exact SHA-guarded staging edit only:

1. add `audit_verdict: pass` after `authority: none`;
2. replace the pending `## Human Verdict` text with:

   ```text
   Pass. The owner accepts the audit recommendation for promotion planning. This verdict approves the audit findings only; it does not authorize canonical promotion or platform implementation.
   ```

Expected post-edit SHA-256:

```text
ab78d9e4b9421fcca9e2b39374c62968ab75ee292d04efc787183ae5f6675944
```

After the edit:

- staging validation must pass with zero errors;
- note ID, type, project, created timestamp, findings, recommendation, and all other bytes must remain unchanged;
- the staging-write attempt log must remain unchanged because this is a separately owner-approved human review edit, not a new create-only write;
- no other staged note may change.


## Phase 0B - Exact Lifecycle Activation

Before any promotion plan is generated, renewed owner approval must authorize one exact human lifecycle edit in each staged bundle note:

```text
status: draft -> status: active
```

No other byte may change. Expected active SHA-256 values:

```text
source-summary: 8534805beb26f5002f20c4977be77a5d3afcefc2f0454c729e9323f047a1e1c8
claim: a35f8bd85832c5146774fe6971ab59584b8034b3a2c57ddbb8f71bb195b4ac20
decision: c368ea6bede4cf42e549c50d128ac7bf2ab3767e991684eb7e59704e927670ef
spec: d2886674d06b13a41e0c2c279ad935f8c63d0d304de73d8040ef719146ce9c15
audit: 204982aaa28a0404e6bade93df3ce72a22aa2923fd2a763b9a7af1ad5ad5c4f6
task-packet: a140e5ceede4c681c3d8fdbaa84f0a3ab5f884ffd734ce0a37309d5e758bbcc3
```

After activation, all six notes must pass staging validation with zero errors, relationships must remain unchanged, and the staging-write attempt log must remain at SHA-256 `3419bc43e1588c7e97d44626bb3e9e263cd3d109647f79c8bda150c422905059`.

Wave 1 planning may then be retried using the same unused operation ID `kz-prom-01KTMNHB6NZ5YBJACWZM92HGJG`.

## Ordered Promotion Waves

| Wave | Type | Source SHA-256 before canonical normalization | Operation ID |
|---|---|---|---|
| 1 | source-summary | `8534805beb26f5002f20c4977be77a5d3afcefc2f0454c729e9323f047a1e1c8` | `kz-prom-01KTMNHB6NZ5YBJACWZM92HGJG` |
| 2 | claim | `a35f8bd85832c5146774fe6971ab59584b8034b3a2c57ddbb8f71bb195b4ac20` | `kz-prom-01KTMNHB6NZ5YBJACWZM92HGJH` |
| 3 | decision | `c368ea6bede4cf42e549c50d128ac7bf2ab3767e991684eb7e59704e927670ef` | `kz-prom-01KTMNHB6P6QKSHJWG6ANZEP92` |
| 4 | spec | `d2886674d06b13a41e0c2c279ad935f8c63d0d304de73d8040ef719146ce9c15` | `kz-prom-01KTMNHB6P6QKSHJWG6ANZEP93` |
| 5 | audit | `204982aaa28a0404e6bade93df3ce72a22aa2923fd2a763b9a7af1ad5ad5c4f6` | `kz-prom-01KTMNHB6P6QKSHJWG6ANZEP94` |
| 6 | task-packet | `a140e5ceede4c681c3d8fdbaa84f0a3ab5f884ffd734ce0a37309d5e758bbcc3` | `kz-prom-01KTMNHB6P6QKSHJWG6ANZEP95` |

Canonical destinations:

```text
projects/kaizen-platform/source-summaries/milestone-4-amendment-gap.md
projects/kaizen-platform/claims/governed-amendment-required.md
projects/kaizen-platform/decisions/governed-amendment-slice.md
projects/kaizen-platform/specs/governed-amendment-support.md
projects/kaizen-platform/audits/governed-amendment-support-audit.md
projects/kaizen-platform/handoffs/implement-governed-amendment-support.md
```

## Per-Wave Two-Gate Rule

For every wave:

### Plan gate

The owner must explicitly authorize plan generation for the named wave and operation ID. Plan generation may create only immutable operation evidence in staging.

The steward must then present:

- exact plan SHA-256;
- canonical candidate SHA-256;
- validation evidence and run ID;
- complete normalized metadata and rendered diff;
- required earned-directory creation;
- prior and new review/authority values;
- expected event and Git scope.

### Execute gate

Execution remains prohibited until the owner explicitly approves:

```text
wave number
operation ID
exact plan SHA-256
```

After approval, that wave may create only its exact earned directory if absent, approval evidence, one intent event, one terminal event, one canonical file, and the scoped vault Git commit.

No approval for one wave authorizes another wave.

## Promotion Order and Authority Transitions

1. **source-summary:** `pending -> approved`; authority remains `none`.
2. **claim:** `pending -> approved`; `proposed -> accepted`.
3. **decision:** `pending -> approved`; `proposed -> accepted`.
4. **spec:** `pending -> approved`; `proposed -> accepted`.
5. **audit:** `pending -> approved`; authority remains `none`; audit verdict remains `pass`.
6. **task-packet:** `pending -> approved`; `proposed -> accepted`.

A later wave may be planned only after all canonical dependencies required by its frontmatter exist and the prior wave has a clean committed vault state.

## Scope

- exact audit verdict finalization;
- six immutable plan generations;
- six separately approved executions;
- earned-directory creation only when required by the accepted placement map;
- exact event verification and one scoped local vault commit per wave;
- completion audit after all six waves.

## Non-Scope

- platform implementation;
- live amendment execution;
- editing any other staged or canonical note;
- changing note IDs, summaries, evidence, decisions, spec requirements, or task-packet scope;
- batch approval without exact plan hashes;
- combining waves into one Git commit;
- push or vault remote creation;
- Hermes, LangGraph, Postgres, Qdrant, providers, UI, websites, or deployment.

## Constraints

- All repositories must be clean at each plan and execute gate.
- Existing staged source bytes must match the bound hash for that wave.
- Existing canonical destination must be absent before first promotion.
- Staging evidence remains retained after promotion.
- Source-summary historical placement under `research/` does not override the accepted `source-summaries/` mapping.
- Any plan, candidate, log, Git, source, destination, or dependency drift stops the wave without cleanup or regeneration.

## Validation

Packet 009B completes only when:

- the exact audit edit validates and matches `ab78d9e4b9421fcca9e2b39374c62968ab75ee292d04efc787183ae5f6675944`;
- each wave has a reviewed plan and separate exact owner approval;
- all six canonical notes validate and retain stable IDs;
- all relationships resolve canonically;
- each operation has exactly one intent and one successful terminal event;
- vault Git history contains six scoped commits or a separately reviewed equivalent if later evidence proves fewer commits safer;
- docs/platform remain unchanged during promotion execution;
- no deferred system or implementation work begins.

## Acceptance Criteria

- The six-note chain is canonical in dependency order.
- Governing notes have approved review status and the intended authority.
- Audit verdict is `pass` in both body/frontmatter and consistent with promotion evidence.
- Task packet is canonical, approved, accepted, and implementation-ready.
- Vault, platform, and docs end clean.
- A completion steward audit records every plan, operation, event, hash, destination, and commit.

## Completion Report

Pending multi-wave execution.
