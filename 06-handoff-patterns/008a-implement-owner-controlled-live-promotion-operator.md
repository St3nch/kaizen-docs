---
id: kz-tp-01KTMEKKT77BC2S8Y1E39BXFT3
type: task-packet
status: active
project: kaizen-platform
summary: Implement and hammer-test a fixed-root human-only live promotion operator without performing a live promotion.
created: 2026-06-08T20:30:00Z
updated: 2026-06-08T20:30:00Z
review_status: pending
authority: proposed
primary_spec: 05-specs/staging-write-wrapper-and-promotion-recovery.md
related_specs:
  - 05-specs/staging-write-wrapper-and-promotion-recovery.md
  - 05-specs/staging-and-promotion-workflow.md
  - 05-specs/promotion-event-schema.md
  - 05-specs/kaizen-hammer-test-strategy.md
---

# Task Packet 008A - Implement Owner-Controlled Live Promotion Operator

> Security status: security-audited pass in Result 034; awaiting explicit owner approval. This packet authorizes platform implementation and disposable-root tests only. It does not authorize a live plan, approval, execution, recovery, parent-directory creation, or canonical promotion.

## Objective

Add a separate, fixed-root, human-operated live-promotion entrypoint that can later support an explicitly approved first real promotion while preserving Packet 006B's fail-closed default and disposable sandbox CLI.

Packet 006B intentionally rejects the exact live roots:

```text
C:\dev\kaizen\vault
C:\dev\kaizen\staging
```

and `kaizen-promote` is intentionally pinned to:

```text
C:\dev\kaizen\promotion-sandbox\vault
C:\dev\kaizen\promotion-sandbox\staging
```

Do not bypass or remove those protections. Implement a separate live operator with an explicit environment-binding contract.

## Why This Packet Is Separate

The first real promotion cannot safely be bundled with enabling live-root access. Packet 008A must prove the operator without touching the live vault or creating live promotion evidence. A later Packet 008B will bind the reviewed candidate, immutable plan, approval, destination-parent preparation, execution, events, and Git review.

## Bound Repository State

```text
docs planning commit before Packet 008A drafting: 7315401cd3ce42daed6f73eb42d4d31b9fb8405e
platform implementation commit: 703d5327324b77500d3228e33e18b118c68be046
vault commit: 248b26a648142164c30d0e1126b821fce9f36cd3
platform branch: main
vault branch: main
```

The implementation must begin from a clean platform tree at the exact bound commit. Do not reset, stash, or discard unexpected work.

## Held First-Promotion Candidate

The following staged source-summary was created through the governed create-only staging wrapper and passed staging and canonical rehearsal validation:

```text
path: C:\dev\kaizen\staging\projects\kaizen-platform\research\packet-007-governance-bootstrap-completion.md
note id: kz-ss-01KTME98CJ56110KTKJXKC3JPE
bytes: 2380
sha256: 119788565b83876d833917e6b7c7fdd4f35e9620c0c468202e7628825ad58f1a
staging request id: 66d692e2-0d98-447f-9eb8-e75108d0b4ae
staging intent event: 5a4c3d6a-effb-49a8-b445-256166fb0963
staging committed event: 8cfb5283-06a0-4ccc-a68f-a23d0d2ed29a
staging validation run: b0062053-da53-4099-b6c3-24df4a51a0c8
staging-write log sha256 after creation: 07b09195bd04852fd6d2f12dc6a3c878b85f964eec2cff850b496352bd4b8a52
```

Packet 008A must not modify, normalize, plan, approve, promote, move, delete, or regenerate this candidate. It is held for Packet 008B.

## Required Design

### Preserve the existing sandbox operator

`kaizen-promote` must remain disposable-sandbox only. Its command surface and root selection must not gain a `--live`, root-path, environment-variable, or configuration-file bypass.

### Add a separate live entrypoint

Add a separate console entrypoint:

```text
kaizen-promote-live
```

It must:

- use only `RootConfig.default()` fixed roots;
- accept no canonical-root or staging-root argument;
- reject environment-variable root overrides;
- expose only `plan`, `inspect`, `approve`, `execute`, and `recover`;
- remain human-operated local CLI only;
- have no MCP, Hermes, network, HTTP, queue, scheduler, or agent-tool exposure.

### Explicit live capability, not a generic bypass

Do not replace `ensure_disposable_roots` with a generic `allow_live=True` boolean.

Introduce an explicit typed live-promotion capability or scope object created only after fixed-root and Git preflight verification. Core promotion functions must continue to reject live roots when no verified live capability is supplied.

The capability is an accidental-misuse boundary inside a local Python process, not an authentication system. Document that limitation honestly.

### Immutable live environment binding

A live plan must include hash-covered live-only evidence with a versioned binding containing at least:

```text
live_binding_version: 1.0
promotion_scope: live
packet_id
canonical_root
staging_root
vault_branch
vault_head
platform_head
governance_log_sha256
```

Requirements:

- roots are normalized fixed values, not caller text;
- branch must be `main`;
- vault and platform trees must be clean at planning time;
- `packet_id` must be a valid task-packet ID supplied by the human operator;
- plan hashing covers the entire live binding;
- approval remains bound to the plan hash, source hash, candidate hash, destination, validation run, reviewed diff, and human actor.

### Live command preflight

For `plan`, `inspect`, `approve`, and initial `execute`, verify:

- platform branch and head match the plan binding;
- platform tree is clean;
- vault branch and head match the plan binding;
- vault tree is clean;
- governance paths exist and are regular non-reparse paths;
- the promotion log hash matches the plan binding;
- the staged source hash matches the plan;
- no unrelated promotion process is active.

`plan` captures current clean heads into the binding. `approve` and `execute` must not silently refresh stale state.

### Explicit human controls

Live `approve` must require:

```text
operation id
human actor
packet id matching the plan
non-empty approval basis
```

Live `execute` must additionally require an exact fixed confirmation literal. The literal is an accidental-invocation guard, not a substitute for Packet 008B owner approval.

### Recovery-aware Git checks

Initial live execution requires a clean vault. Recovery cannot require a clean vault because a crashed operation may have appended intent or installed the candidate.

Live recovery must:

- require the original bound vault head and branch;
- require the bound platform head and a clean platform tree;
- parse and validate the promotion log;
- allow only dirty paths explainable by the target operation, its approved canonical destination, and the append-only promotion log;
- reject unrelated tracked or untracked vault changes;
- preserve unknown temporary files and conflicting canonical content;
- never run `git reset`, `checkout`, `restore`, `clean`, `stash`, commit, or push.

### No Git mutation in the operator

The live operator may read Git branch, head, and status. It must not create branches, stage files, commit, push, create remotes, alter Git configuration, reset, restore, clean, or stash.

Packet 008B will govern post-promotion Git review and commit separately.

### No destination-parent creation

The live operator must not create arbitrary destination parent directories. Packet 008B must explicitly prepare and verify the one approved parent when needed.

### Stable failure codes

Add stable failure codes for at least:

```text
live_scope_required
live_root_mismatch
live_packet_mismatch
live_confirmation_required
live_git_branch_mismatch
live_git_head_mismatch
live_git_dirty
live_platform_drift
live_vault_drift
live_recovery_unrelated_changes
```

Failures before intent must cause no canonical mutation and no promotion event.

## Required Tests

Use disposable temporary roots and temporary Git repositories. Do not invoke a mutating live command against the actual live roots.

At minimum prove:

1. existing `kaizen-promote` remains sandbox-only;
2. default core calls still reject exact live roots;
3. no generic boolean or caller-selected-root bypass exists;
4. the live CLI exposes no root arguments;
5. only exact fixed live roots can create a live capability;
6. wrong roots fail before mutation;
7. live plans hash-bind packet, roots, Git heads, branch, and log state;
8. invalid packet IDs are rejected;
9. dirty or drifted platform/vault state blocks plan, approve, and execute;
10. source, candidate, or governance-log drift blocks approve and execute;
11. approval requires a human actor and explicit basis;
12. execute requires the exact confirmation literal;
13. every pre-intent failure leaves canonical content and log unchanged;
14. a complete live-mode mirror operation produces one intent and one terminal event;
15. same-operation and competing-operation races converge safely;
16. interrupted live-mode mirror execution recovers deterministically;
17. recovery accepts only operation-explainable dirty paths;
18. unrelated dirty paths block recovery without cleanup;
19. the operator never invokes a mutating Git command;
20. the held live candidate and live governance log remain byte-for-byte unchanged;
21. the full platform suite passes with zero failures, skips, or xfails reported.

## Documentation Updates

Update only the platform README and command help needed to explain:

- sandbox versus live entrypoints;
- fixed roots;
- owner-packet requirement;
- explicit confirmation;
- no Git commit/push behavior;
- no agent or MCP authority;
- recovery limitations.

Do not claim live promotion is approved or complete.

## Non-Scope

Packet 008A does not authorize:

- invoking `kaizen-promote-live` against the live roots;
- generating a live promotion plan or validation run;
- approving or executing the held candidate;
- creating `projects/kaizen-platform/research` in the live vault;
- appending a live promotion event;
- modifying the live vault or staging root;
- changing the held candidate or staging evidence;
- Hermes, MCP, LangGraph, Postgres, Qdrant, Observatory, UI, provider, website, or deployment work;
- amendment, supersedence, correction, overwrite, delete, or generalized rollback.

## Stop Conditions

Stop as blocked if:

- Packet 008A lacks explicit owner approval;
- the bound platform state differs;
- the implementation weakens the existing sandbox or disposable-root refusal;
- the design requires caller-selected roots or a generic live boolean;
- Git binding cannot be included under the immutable plan hash;
- recovery cannot distinguish operation-owned dirty paths from unrelated changes;
- tests require mutation of the real vault or staging root;
- any live promotion plan, approval, event, candidate, or canonical file is created;
- the held candidate or staging-write log changes;
- the full test suite regresses.

## Acceptance Criteria

Packet 008A passes only when:

- the dedicated fixed-root live CLI exists;
- existing sandbox behavior is unchanged;
- default core calls still fail closed on live roots;
- live plans bind packet, root, Git, and governance-log state immutably;
- approval and execution verify stale-state conditions;
- initial execute and recovery use different correct Git-dirty rules;
- explicit human basis and confirmation are required;
- no mutating Git command exists in the operator;
- all required hammers pass repeatedly;
- full platform tests pass with zero failures, skips, or xfails reported;
- the live vault, governance log, staging candidate, and staging log are unchanged;
- a steward completion audit confirms no live command was invoked.

## Completion Report

Return:

```text
result
platform commits before and after
implemented files and entrypoints
live capability and binding contract
CLI help output
new failure codes
focused tests and repeated race counts
full-suite results
Git-command surface review
live candidate hash before and after
staging-log hash before and after
live governance-log size/hash before and after
vault/platform/docs statuses
limitations and deviations
```
