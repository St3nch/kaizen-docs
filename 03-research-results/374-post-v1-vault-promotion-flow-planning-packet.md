# Post-v1 Vault Promotion Flow Planning Packet

Status: proposed planning packet
Date: 2026-06-22
Packet: 021A candidate

## Purpose

Define the first post-v1 planning lane for turning Kaizen from a proven governance/build workbench into a usable governed project-intelligence system backed by the canonical Obsidian vault.

This packet is planning-only.

It does not authorize vault mutation, platform mutation, staging mutation, database mutation, Git push, downstream project mutation, or automated promotion.

## Trigger

Kaizen v1 / Milestone 14 is owner-accepted in:

```text
03-research-results/368-milestone-14-kaizen-v1-owner-acceptance.md
```

The post-v1 Claude adversarial audit returned:

```text
PASS WITH REQUIRED FIXES
```

The required reading-path fixes RF-01, RF-02, and RF-03 were closed in docs commit:

```text
862493f5b71b3a4fad82072980522ae63d782438
Refresh post-v1 reading path
```

The audit disposition is recorded in:

```text
03-research-results/373-post-v1-claude-audit-disposition-and-reading-path-refresh-closure.md
```

## Problem

Kaizen has now proven that the governed loop can work, but the canonical vault is not yet the active living project-intelligence layer for the current project state.

Current reality:

```text
kaizen-docs = Kaizen construction, governance, proof, and audit workbench;
kaizen vault = intended canonical living project-intelligence layer;
downstream repositories = implementation truth for their projects;
platform / Postgres = operational and structured governed records;
```

The vault currently contains historical bootstrap/project-state material that predates the accepted v1 state. Its Kaizen platform current-state is materially stale relative to Milestone 14 and post-v1 acceptance.

That stale vault state is not a v1 acceptance blocker because the caveat was recorded, but it is the next major usability blocker.

## Core doctrine to preserve

```text
Kaizen docs prove the governance.
The vault remembers the project.
The repo implements the project.
```

## Scope

This planning packet covers only the design of the vault promotion flow for post-v1 Kaizen project memory.

In scope:

```text
identify which project truth belongs in vault;
separate proof records from project current-state truth;
define minimum project-note structure for post-v1 use;
define human-controlled promotion flow;
define validation requirements before vault mutation;
define first candidate project-memory updates;
define stop conditions and non-authorizations.
```

Out of scope:

```text
writing vault files;
editing vault files;
creating staging candidates;
executing promotions;
platform code changes;
database changes;
Qdrant;
Hermes write authority;
Tauri;
Observatory / IMI implementation;
Neon Ronin implementation;
SearchClarity implementation;
Git push;
cloud sync;
provider/customer-data work.
```

## Required first design questions

Before any vault mutation, answer these:

1. What is the minimum useful post-v1 Kaizen project-memory set?
2. Which truths belong in `projects/kaizen-platform/` now that v1 is accepted?
3. Should Neon Ronin and SearchClarity appear in the vault now, and if yes, as what type of project memory?
4. What content remains in `kaizen-docs` only as proof/governance evidence?
5. What current-state fields must be stable enough for future fresh-context agents?
6. What frontmatter status/authority/review fields should be used for post-v1 vault updates?
7. What validation must run before promotion?
8. What exact owner approval phrase should be required before the first vault mutation?

## Candidate minimum vault project-memory set

A governed project should eventually have:

```text
command-center.md
overview.md
current-state.md
decisions.md or decisions/
roadmap.md or roadmap/
risks-and-bugs.md
improvement-queue.md
implementation-returns.md or implementation-returns/
repo-map.md
```

For the first post-v1 slice, the minimum should likely be:

```text
projects/kaizen-platform/command-center.md
projects/kaizen-platform/overview.md
projects/kaizen-platform/current-state.md
```

Optional only if needed:

```text
projects/neon-ronin/overview.md
projects/neon-ronin/current-state.md
projects/searchclarity/overview.md
```

## Initial observed vault state

A bounded vault manifest shows existing project folders:

```text
projects/kaizen-platform/
projects/northstar-stockroom/
```

The vault README states:

```text
Project intelligence belongs in this vault.
Kaizen doctrine and planning remain in kaizen-docs until a governed migration or supersedence decision changes that boundary.
Implementation code belongs in the separate kaizen/platform repository.
Mutable operational records do not belong in canonical Markdown.
Agents do not have canonical write authority.
```

The current `projects/kaizen-platform/current-state.md` still describes a pre-Milestone-9 world and is stale relative to Milestone 14 / Kaizen v1 acceptance.

## Proposed promotion model

Use a staged human-controlled flow:

```text
1. discover current vault state;
2. define exact target notes and fields;
3. draft candidate replacement content in docs or staging;
4. validate frontmatter, note type, links, boundaries, and stale-claim removal;
5. produce exact diff and SHA-bound plan;
6. owner approves exact plan;
7. execute vault mutation exactly once;
8. verify path scope and content hashes;
9. commit vault changes locally;
10. record promotion / implementation-return in kaizen-docs.
```

No agent should directly rewrite canonical vault files without a reviewed and owner-approved exact plan.

## First promotion candidate

The first useful candidate should probably be:

```text
Update projects/kaizen-platform/current-state.md to the post-v1 accepted state.
```

Potential companion updates:

```text
Update projects/kaizen-platform/command-center.md to point to current post-v1 lane;
Update projects/kaizen-platform/overview.md to clarify Kaizen v1 vs full-system completion;
```

The first candidate should not attempt to migrate all of `kaizen-docs` or all M14 proof records into the vault.

## Validation requirements

Before a vault update is approved, validate:

```text
exact target path list;
current SHA-256 of each target file;
frontmatter required fields;
note type matches registry expectations;
no stale M9/M13/M14-active language remains in updated current-state;
no downstream project truth is overclaimed;
no SearchClarity / Neon Ronin / Kaizen boundary collapse;
no secret values;
no invisible Unicode / bidi controls;
diff is bounded and reviewable;
no unapproved vault paths changed;
```

## Stop conditions

Stop before mutation if:

```text
vault working tree is dirty;
expected target SHA mismatch occurs;
project truth cannot be separated from proof clutter;
Neon Ronin/SearchClarity boundary becomes ambiguous;
owner has not approved exact target paths and replacement content;
secrets or DSNs appear in candidate content;
frontmatter validation fails;
any tool attempts broad or implicit staging;
```

## Non-authorization

This packet does not authorize:

```text
vault mutation;
staging mutation;
platform mutation;
database mutation;
Git push;
remote creation;
Qdrant;
Hermes write authority;
Tauri;
Observatory / IMI implementation;
Neon Ronin mutation;
SearchClarity mutation;
provider/customer-data work.
```

## Recommended next action

Create a focused audit/design record for the first vault promotion candidate:

```text
021B — Kaizen Platform Post-v1 Current-State Vault Promotion Candidate
```

021B should read the existing `projects/kaizen-platform/current-state.md`, define the exact post-v1 replacement content, validate the target path scope, and stop before vault mutation pending owner approval.

## Related files

```text
00-entrypoint/LLM_START_HERE.md
ROADMAP_V0.4.md
03-research-results/368-milestone-14-kaizen-v1-owner-acceptance.md
03-research-results/373-post-v1-claude-audit-disposition-and-reading-path-refresh-closure.md
projects/kaizen-platform/current-state.md
projects/kaizen-platform/command-center.md
projects/kaizen-platform/overview.md
```
