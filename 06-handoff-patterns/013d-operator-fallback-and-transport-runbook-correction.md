---
id: kz-tp-01KV3D013D0PERATORFALLBACK
type: task-packet
status: draft
project: kaizen-platform
created: 2026-06-14T16:00:00Z
updated: 2026-06-14T16:00:00Z
review_status: pending
authority: proposed
summary: "Consolidate the exact operator fallback and transport-classification runbook before the first governed pilot return."
---

# Task Packet 013D - Operator Fallback and Transport Runbook Correction

## Purpose

Correct and consolidate the existing fallback guidance so an operator can distinguish where a failure occurred, verify whether any side effect happened, switch once to the fixed-root local operator when needed, and verify the result afterward.

This is a documentation-only correction. It does not authorize connector, tunnel, process, canonical, staging, Git, backup, or Milestone 9 mutation.

## Evidence and exact source bindings

```text
03-research-results/167-roadmap-lineage-and-corrective-sequence-reconciliation.md
SHA-256: 0b932bf85ae7ba81196e92c66f4d76b01923c7df6251e05fc87869204bf508fd

03-research-results/174-packet-013b-milestone-9-readiness-result.md
SHA-256: 3d409697de50f7176d962daf52331c0c7781a96d615c9a697b7c8d727f3006ca

C:\dev\kaizen-mcp\docs\UPSTREAM_MUTATION_GATING.md
SHA-256: b294de83149a1b1da22a53e2e672a0ba73608d0da28cace46423ef2bc69448a5

C:\dev\kaizen-mcp\README.md
SHA-256: 139d3557bdd3e629b59631144f1c2c2a5af29b337352836ae370dda579b5b79c

C:\dev\kaizen\platform\README.md
SHA-256: f431db6d4f4daf8f7a5972415dbb256bca0189077569ac6ae8f2d3d8fa805cb2
```

The installed `kaizen-operator` parser exposes exactly:

```text
inspect
approve
execute
recover
```

with deterministic `json` or `text` output.

Any bound source mismatch before editing stops implementation and requires renewed inspection.

## Repository and path scope

### Kaizen MCP proving ground

```text
C:\dev\kaizen-mcp\docs\UPSTREAM_MUTATION_GATING.md
C:\dev\kaizen-mcp\README.md
```

Allowed work:

- consolidate the one-block rule;
- add the exact failure-classification table;
- add exact inspect-before-action and post-action verification sequences;
- cross-link to the platform operator commands.

Kaizen MCP remains non-Git. No source, schema, tool, script, endpoint, port, mutation setting, or lifecycle behavior may change.

### Kaizen platform

```text
C:\dev\kaizen\platform\README.md
```

Allowed work:

- correct and expand the existing minimal operator section only;
- document exact command templates for `inspect`, `approve`, `execute`, and `recover`;
- document expected pre-action and post-action state checks;
- distinguish local operator success from connector or transport outcomes.

No Python source, tests, packaging, console parser, public API, event schema, or platform behavior may change.

### Kaizen docs

Allowed work:

- this Packet 013D record;
- one compact completion/audit record after implementation;
- active orientation updates only if the next gate would otherwise be stale.

## Required failure classification

The corrected runbook must distinguish these classes without collapsing them:

| Class | Meaning | Required response |
|---|---|---|
| Upstream safety block | ChatGPT blocks before the MCP server receives the call | Verify no side effect, retry the exact bounded call once, then switch route |
| Connector failure | Connector cannot dispatch or refresh the registered tool surface | Verify local server health and registry independently; do not claim server rejection |
| Tunnel failure | Public endpoint cannot reach the loopback server | Verify local health first; treat tunnel startup separately from server startup |
| MCP adapter rejection | The MCP server receives the call and rejects schema, mutation state, or adapter binding | Report the exact adapter error; do not reclassify it as a platform failure |
| Platform rejection | The adapter delegates and deterministic platform validation or state checks reject the operation | Preserve the exact platform code and evidence; do not bypass it |
| Local operator success | The fixed-root operator completes the approved action | Verify terminal status, hashes, event chain, and Git scope afterward |
| Git-tool upstream block | ChatGPT blocks staging or commit before local Git runs | Verify staged/working state, retry once, then give the owner the exact minimal local Git command |

## One-block rule

After one blocked mutation call:

1. inspect the operation and repository state to prove whether any side effect occurred;
2. retry the exact bounded call once only when no side effect occurred;
3. do not invent cosmetic variants or broaden the route;
4. switch to the narrow typed alternative or `kaizen-operator`;
5. if upstream blocking also prevents the local tool call, provide the exact minimal owner-run command;
6. resume read-only verification immediately after the owner runs it.

## Exact operator sequence

All commands run from:

```powershell
cd C:\dev\kaizen\platform
```

### Inspect before approval, execution, or recovery

```powershell
.\.venv\Scripts\kaizen-operator.exe inspect <operation-id> `
  --packet-id <packet-id> --format json
```

Required review:

```text
operation ID and packet ID match
state matches the intended gate
plan SHA-256 matches the reviewed immutable plan
prior, candidate, and reviewed-diff hashes match
approved actor is exact when approval exists
Git branch and HEAD match the plan
mismatches are empty for a nonterminal action
eligibility for the intended action is true
```

### Approve

```powershell
.\.venv\Scripts\kaizen-operator.exe approve <operation-id> `
  --packet-id <packet-id> --plan-sha256 <exact-plan-sha256> `
  --human-actor owner.local --approval-basis "<exact reviewed basis>" `
  --format json
```

Then inspect again. Approval must create approval evidence only.

### Execute exactly once

```powershell
.\.venv\Scripts\kaizen-operator.exe execute <operation-id> `
  --packet-id <packet-id> --plan-sha256 <exact-plan-sha256> `
  --human-actor owner.local --confirm PROMOTE-LIVE-EXACTLY-ONCE `
  --format json
```

Then inspect again. A trustworthy terminal state remains `committed` even while the expected uncommitted Git changes produce `git_not_clean`.

### Recover an interrupted reviewed operation

```powershell
.\.venv\Scripts\kaizen-operator.exe recover <operation-id> `
  --packet-id <packet-id> --plan-sha256 <exact-plan-sha256> `
  --human-actor owner.local --confirm RECOVER-LIVE-EXACTLY-ONCE `
  --format json
```

Recovery is valid only when inspection reports recovery eligibility. It is not a generic retry or repair route.

## Post-action verification

After execution or recovery:

1. inspect operation status;
2. verify one intent and exactly one successful terminal event;
3. hash the installed canonical destination;
4. hash the governance log;
5. verify exact changed paths;
6. scan changed text integrity;
7. stage only the reviewed paths;
8. verify staged scope;
9. create the local commit;
10. verify the repository is clean and the terminal state remains preserved.

No operator command performs Git staging, commit, push, reset, clean, stash, remote creation, or cleanup.

## Acceptance checks

Implementation passes when deterministic review proves:

- all seven failure classes are present and non-overlapping;
- the one-block rule appears once in authoritative operational guidance;
- exact operator commands match installed parser behavior;
- inspect-before-action and post-action verification are complete;
- connector refresh, server startup, tunnel startup, and process termination remain separate concerns;
- no command kills or replaces a process;
- no endpoint, port, tool, source, test, package, schema, or runtime behavior changes;
- Kaizen MCP remains non-Git;
- platform and vault remain local-only with no remote creation or push;
- exact changed paths and final clean states are recorded.

## Verification method

```text
source-hash recheck
exact text search for required classifications and literals
installed parser --help comparison
diff and whitespace check
hidden-Unicode scan
exact changed-path verification
Kaizen MCP non-Git verification
platform and docs Git status verification
```

No platform test suite is required because no executable code may change. If executable code changes, stop: the packet scope is wrong.

## Authorization boundary

Drafting, deterministic auditing, and committing this documentation-only packet in `kaizen-docs` are authorized under the active Tier 1 workflow.

Implementation in `kaizen-mcp` and `kaizen-platform` requires separate owner approval bound to the exact audited Packet 013D SHA-256.

## Explicit exclusions

- any canonical or staging mutation;
- connector refresh or lifecycle action;
- server or tunnel start, stop, or restart;
- process termination;
- source code or test changes;
- new tools, scripts, endpoints, ports, or environment overrides;
- Packet 013E backup action;
- Milestone 9 artifact or repository creation;
- platform or vault remote creation or push;
- cleanup or deletion;
- deferred systems.
