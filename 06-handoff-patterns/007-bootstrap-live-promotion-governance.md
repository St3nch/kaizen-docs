---
id: kz-tp-01KTMD1SS76Y6K6WC7Y7H6NRW6
type: task-packet
status: active
project: kaizen-platform
summary: Bootstrap only the two live canonical promotion-governance files under a human-approved, create-new, Git-reviewed maintenance step.
created: 2026-06-08T20:00:31Z
updated: 2026-06-08T20:00:31Z
review_status: pending
authority: proposed
primary_spec: 05-specs/staging-write-wrapper-and-promotion-recovery.md
related_specs:
  - 05-specs/staging-write-wrapper-and-promotion-recovery.md
  - 05-specs/staging-and-promotion-workflow.md
  - 05-specs/promotion-event-schema.md
  - 05-specs/kaizen-hammer-test-strategy.md
---

# Task Packet 007 - Bootstrap Live Promotion Governance

> Security status: security-audited pass in Research Result 032; awaiting explicit owner approval. This packet authorizes no promotion operation, no promotion event, no staging mutation, and no platform-code change.

## Objective

Perform one owner-controlled live-vault maintenance step that creates exactly:

```text
C:\dev\kaizen\vault\_governance\README.md
C:\dev\kaizen\vault\_governance\promotion-log.jsonl
```

The README bytes are fixed by this packet. The log must be an empty zero-byte regular file. The resulting two-file bootstrap must be verified through the implemented Packet 006B read-only governance parser and committed to the local vault Git repository.

This packet closes Result 025 finding F-14 only. It does not authorize the first real canonical promotion.

## Read First

1. `C:\dev\kaizen-docs\00-entrypoint\LLM_START_HERE.md`
2. `C:\dev\kaizen-docs\03-research-results\025-implementation-checkpoint-audit-steward-reconciliation.md`
3. `C:\dev\kaizen-docs\03-research-results\031-packet-006b-final-implementation-steward-audit.md`
4. `C:\dev\kaizen-docs\05-specs\staging-write-wrapper-and-promotion-recovery.md`
5. `C:\dev\kaizen-docs\05-specs\staging-and-promotion-workflow.md`
6. `C:\dev\kaizen-docs\05-specs\promotion-event-schema.md`
7. `C:\dev\kaizen\platform\AGENTS.md`
8. `C:\dev\kaizen\platform\src\kaizen\promotion_events.py`
9. `C:\dev\kaizen\platform\src\kaizen\promotion_workflow.py`
10. `C:\dev\kaizen\platform\src\kaizen\windows_fs.py`

## Bound Repository State

```text
canonical vault root: C:\dev\kaizen\vault
required vault branch: main
required clean vault commit: 4bd394a9446cab054babfb20b0ca07ad4db690e6
platform repository: C:\dev\kaizen\platform
required clean platform commit: 703d5327324b77500d3228e33e18b118c68be046
docs packet: 06-handoff-patterns/007-bootstrap-live-promotion-governance.md
security audit: 03-research-results/032-packet-007-security-audit.md
```

If either repository is dirty, on a different commit, or on an unexpected branch, stop for steward review. Do not rebase, stash, reset, or discard work.

## Exact Allowed Mutation

Create one previously absent directory:

```text
C:\dev\kaizen\vault\_governance
```

Create exactly two previously absent files beneath it:

```text
README.md
promotion-log.jsonl
```

Then stage and commit exactly those two files in the vault repository with commit message:

```text
Bootstrap canonical promotion governance
```

No remote exists or is required. Do not add or push a vault remote.

## Exact README Content

The UTF-8, no-BOM, LF-terminated README content must be exactly:

```markdown
# Kaizen Canonical Promotion Governance

This directory contains the canonical-vault governance evidence required by the human-operated first-time promotion workflow.

## Files

- `promotion-log.jsonl` is the append-only promotion event log.
- This README documents the bootstrap boundary and operating rules.

## Rules

- The bootstrap creates `promotion-log.jsonl` as an empty zero-byte regular file.
- Promotion events may be appended only by the reviewed human-operated promotion workflow.
- Existing promotion-log bytes must never be edited, replaced, or truncated.
- The directory and log must remain regular non-reparse paths inside the canonical vault.
- Hermes, MCP tools, and other agents have no canonical write or promotion authority.
- Governance bootstrap does not authorize a real promotion; first real execution requires a separate explicit owner go/no-go.
```

Expected SHA-256:

```text
aabbabc49eff189c4b17f2c71e1a875cedc21b65553ae8eff0a1ca8c0c6a205a
```

## Exact Log Content

`promotion-log.jsonl` must contain zero bytes.

Expected size and SHA-256:

```text
size: 0
sha256: e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855
parsed events: []
```

Do not add a newline, header, bootstrap event, comment, schema marker, or byte-order mark.

## Required Execution Method

Use one reviewed local maintenance invocation from the platform virtual environment. The invocation must:

1. bind the canonical root to the trusted fixed path `C:\dev\kaizen\vault`;
2. verify the vault root is a regular non-reparse directory;
3. verify `_governance` is absent before mutation;
4. acquire the existing canonical-root promotion mutex before directory creation;
5. create `_governance` once and fail if it appears concurrently;
6. create both files with create-new semantics (`xb` or equivalent), never overwrite semantics;
7. write and flush the exact README bytes;
8. create and flush the zero-byte log;
9. verify both paths are regular non-reparse files;
10. recompute exact size and SHA-256 values;
11. call the Packet 006B read-only governance functions and require `read_events(...) == []`;
12. release the mutex only after verification finishes.

Do not create a reusable bootstrap command, CLI flag, agent tool, MCP tool, or platform API in this packet. This is a one-time human maintenance operation.

## Preflight

Before mutation, prove all of the following:

- docs, platform, and vault repositories are clean;
- vault branch is `main` at the exact bound commit;
- platform branch is `main` at the exact bound commit;
- `_governance` does not exist in any form;
- no path component is a reparse point;
- live staging inventory and hashes are recorded but not modified;
- no `kaizen-promote` process or other canonical writer is running;
- the exact README bytes and both expected hashes match this packet;
- the maintenance invocation contains no promotion planning, approval, execution, or recovery call.

## Post-Write Verification

Before Git staging:

1. `git status --short` in the vault shows only:

   ```text
   ?? _governance/
   ```

2. recursive inventory beneath `_governance` contains exactly the two approved files;
3. README hash equals `aabbabc49eff189c4b17f2c71e1a875cedc21b65553ae8eff0a1ca8c0c6a205a`;
4. log size is zero and hash equals `e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855`;
5. the promotion log parses as an empty event list;
6. Packet 006B `require_governance` accepts the directory, README, and log;
7. no promotion operation evidence, event, canonical candidate, or destination file exists;
8. live staging hashes are unchanged.

## Git Review and Commit

1. Review `git diff --no-index /dev/null _governance/README.md` or equivalent for the exact README addition.
2. Verify the empty log is staged despite having zero bytes.
3. Stage only:

   ```text
   _governance/README.md
   _governance/promotion-log.jsonl
   ```

4. Review `git diff --cached --check` and `git diff --cached --stat`.
5. Commit locally using the exact commit message.
6. Require a clean vault working tree after commit.
7. Record the new vault commit SHA for the completion audit and next gate.

## Non-Scope

This packet does not authorize:

- running `kaizen-promote plan`, `approve`, `execute`, or `recover`;
- creating a promotion operation ID or validation run;
- appending any promotion event;
- promoting any staged note;
- modifying or deleting any existing vault note;
- modifying live staging or its evidence;
- changing platform code, tests, schemas, dependencies, or configuration;
- creating a general governance-bootstrap command;
- creating a remote, pushing the vault, or changing Git configuration;
- Hermes, MCP, LangGraph, Postgres, Qdrant, Observatory, provider, UI, or website work;
- amendment, supersedence, correction, rollback execution, overwrite, or deletion.

## Stop Conditions

Stop without mutation if:

- the owner has not explicitly approved Packet 007;
- the vault or platform state differs from the bound state;
- `_governance` or either target path already exists;
- any target or parent resolves through a reparse point;
- another canonical writer or promotion process is active;
- the exact README bytes or expected hashes do not match;
- create-new semantics are unavailable;
- the canonical-root mutex cannot be acquired;
- any unexpected file, Git change, event, or byte appears;
- read-only governance parsing does not return an empty event list;
- staging hashes change;
- the staged Git diff contains anything beyond the two approved files.

Do not weaken the packet, overwrite an existing target, or "repair" unexpected state automatically.

## Failure and Cleanup

### Before either file exists

Return blocked. No cleanup is needed.

### Partial create before Git commit

Cleanup is allowed only when all of the following are proven:

- `_governance` did not exist before this packet;
- every existing target is packet-owned;
- README bytes either match the exact approved content or are absent;
- the log is either zero bytes or absent;
- Git shows no other vault change;
- paths remain regular non-reparse paths.

Under those conditions, remove only the packet-created files and empty directory, then verify the vault returns exactly to commit `4bd394a` with a clean tree. Otherwise preserve evidence and stop for human review.

### After Git commit

No automatic rollback is authorized. Any reversal requires a separate reviewed human Git action. Never truncate or rewrite the promotion log.

## Acceptance Criteria

Packet 007 execution passes only when:

- the owner explicitly approved this exact packet;
- the two approved files were created from an absent state with create-new semantics;
- README and log hashes exactly match the packet;
- the log is zero bytes and parses as `[]`;
- governance paths are regular, confined, and non-reparse;
- only the two approved files changed;
- the vault commit succeeds with the exact message;
- vault, platform, and docs repositories end clean;
- staging inventory and hashes remain unchanged;
- no promotion was planned, approved, executed, recovered, or logged;
- the new vault commit is recorded for the next steward audit.

## Completion Report

Return:

```text
result
owner approval basis
preflight vault/platform commits
created paths
README size and SHA-256
log size and SHA-256
parsed event count
reparse/confinement checks
promotion mutex result
Git staged paths
diff-check result
new vault commit
post-commit repository statuses
staging hash comparison
failures or deviations
```

Any deviation makes the result partial or blocked, never silently successful.
