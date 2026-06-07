# Kaizen Implementation Roadmap v0.1

Status: active planning artifact; implementation begins only through approved task packets
Date: 2026-06-07
Owner: Kaizen project steward
Governing decision:
- `04-design-decisions/0012-first-slice-contract-and-implementation-boundary.md`

## Purpose

Define the shortest governed path from the accepted Kaizen planning foundation to one working vertical slice.

This roadmap covers only the first implementation slice and the retrospective needed to decide what comes next.

## Repository boundaries

```text
C:\dev\kaizen-docs
= doctrine, decisions, specifications, research, and task-packet source material

C:\dev\kaizen
= non-repository umbrella folder

C:\dev\kaizen\platform
= implementation and code repository
```

Current first-slice roots exist at `C:\dev\kaizen\vault` and `C:\dev\kaizen\staging`. Future `data` and `scratch` roots remain deferred until an approved milestone requires them.

## First-slice objective

```text
canonical Markdown
-> ID generation
-> deterministic validation
-> sibling staging
-> safe human promotion
-> implementation task packet
-> completion evidence
-> governed current-state update
-> retrospective audit
```

## First-slice exclusions

- Hermes integration
- Operational Postgres
- Internet Marketing Intelligence database
- Qdrant
- DataForSEO or another paid provider
- marketplace-ranking collection
- Tauri or another desktop shell
- Obsidian bridge plugin
- production deployment
- final backup and disaster-recovery architecture
- advanced knowledge-quality scoring
- multi-project semantic retrieval

## Implementation principles

- Production code belongs in `C:\dev\kaizen\platform`, never in `kaizen-docs`.
- Every milestone is authorized by one or more reviewed task packets.
- Each task packet identifies exact governing decisions and specifications.
- High-risk write boundaries require hammer tests before use.
- No task packet may silently expand scope into a deferred system.
- Completion reports are evidence, not authority.
- Contract changes discovered during implementation return through governed documentation updates.

# Milestone 1 - Kaizen tools foundation

Status: complete

## Objective

Create the Python 3.11.15 project foundation and implement the reusable primitives needed by later first-slice tools.

## Included

- initialize the `C:\dev\kaizen\platform` Git repository;
- create `pyproject.toml`;
- establish package and test structure;
- implement the machine-readable type-prefix registry;
- implement valid Kaizen ID generation;
- implement UTF-8 Markdown and flat-YAML frontmatter parsing;
- implement a minimal CLI framework;
- add deterministic unit tests and fixtures;
- document local development commands.

## Expected initial repository shape

```text
platform/
  pyproject.toml
  README.md
  AGENTS.md
  src/
    kaizen/
  tests/
  schemas/
```

## Required outputs

- `kaizen-id <registered-type-or-event>`;
- valid uppercase Crockford Base32 ULID output;
- failure on unknown types;
- machine-readable note and event prefix mappings;
- Markdown/frontmatter parser with duplicate-key and nested-object detection;
- passing unit tests;
- completion report.

## Required tests

- every registered note type generates the correct prefix;
- every registered event type generates the correct prefix;
- unknown types fail with a nonzero exit code;
- generated ULIDs are valid and unique across a meaningful test batch;
- parser rejects duplicate YAML keys;
- parser rejects nested YAML objects;
- parser handles valid UTF-8 Markdown fixtures;
- tools do not require network access.

## Exit criteria

- repository exists and is committed;
- tests pass locally;
- CLI commands run from the documented environment;
- machine-readable registries match the accepted Markdown contracts;
- completion report records files, tests, deviations, and follow-up work.

# Milestone 2 - Canonical vault bootstrap

Status: implementation complete; remote-backup exit criterion open

## Objective

Create the first canonical vault root and one minimal project bootstrap after Milestone 1 tools exist.

## Included

- create `C:\dev\kaizen\vault` as a separate Git repository;
- create only the required governance and project-root structure;
- add one realistic first-slice project;
- create command-center, overview, and current-state notes;
- validate those notes with Milestone 1 tooling;
- define the initial `.gitignore` posture;
- push the canonical vault to an approved remote.

## Exit criteria

- canonical vault is a separate clean Git repository;
- bootstrap notes validate;
- portable path rules hold;
- `.obsidian` is not required for operation;
- remote backup exists.

# Milestone 3 - Safe staging and promotion

Status: split; path confinement and create-only staging writes complete, audit-remediation and canonical-promotion design open

## Objective

Implement the human-operated promotion path from a sibling staging root into the canonical vault.

## Included

- create `C:\dev\kaizen\staging` only when this milestone begins;
- configure canonical and staging roots;
- implement staging, canonical, and promotion validation modes;
- implement final-path confinement;
- implement type-to-folder destination checks;
- implement exact-content hashes and stale-approval checks;
- implement recoverable `intent` and terminal promotion events;
- implement safe earned-folder creation;
- implement collision refusal and rollback/recovery behavior;
- add required hammer tests.

## Required security tests

- relative traversal escape;
- absolute path escape;
- drive-relative path;
- UNC path;
- device and extended-length path;
- symlink escape;
- junction/reparse-point escape;
- destination collision;
- stale approved content;
- interrupted file/event sequence;
- unauthorized authority transition.

## Exit criteria

- one human-approved staged fixture is promoted safely;
- one interruption scenario is recovered or marked recovery-required;
- escape attempts fail closed;
- promotion evidence is append-only and verifiable;
- staged content is removed or archived only after committed verification.

# Milestone 4 - One governed project loop

Status: planned

## Objective

Exercise the accepted Kaizen document pipeline on one realistic project using the implemented tools.

## Required chain

```text
command-center
overview
current-state
source-summary
claim
decision
spec
audit
task-packet
completion report
current-state amendment
```

## Exit criteria

- every required artifact validates;
- governed artifacts have promotion evidence;
- one task packet is executed;
- the completion report is populated through the governed return loop;
- current-state reflects the implementation result;
- no deferred system was required to complete the chain.

# Milestone 5 - Slice retrospective and v0.2 consolidation

Status: planned

## Objective

Use implementation evidence to improve the contracts and determine the next implementation roadmap version.

## Included

- audit Milestones 1 through 4;
- list contract ambiguities, false positives, false negatives, and maintenance friction;
- identify required compatibility or migration changes;
- update decisions and specs through normal governance;
- consolidate Project Standard v0.2 where implementation evidence justifies it;
- choose the next implementation milestone family.

## Exit criteria

- one promoted retrospective audit exists;
- accepted contract changes are documented;
- first-slice repositories are clean and recoverable;
- Implementation Roadmap v0.2 has an evidence-backed next milestone.

# Task-packet sequence

1. bootstrap the Kaizen platform repository and implement ID/parser foundations;
2. implement first-slice deterministic validation;
3. bootstrap the canonical vault;
4. implement safe staging and promotion;
5. execute the governed project loop;
6. audit the slice.

Packets may be combined only when scope remains reviewable and acceptance criteria stay independently testable.

# Immediate next action

Close the implementation-checkpoint remediation gate defined by:

```text
03-research-results/024-implementation-checkpoint-red-team-audit-claude-summary.md
03-research-results/025-implementation-checkpoint-audit-steward-reconciliation.md
```

Immediate priorities:

1. correct stale entrypoint, roadmap, and canonical bootstrap-note claims;
2. configure and push approved private remotes for the platform and vault repositories;
3. add the adopted subprocess, abrupt-termination, malformed-log, and no-side-effect hammer tests;
4. retire the current combined Packet 006 from approval consideration;
5. draft and audit Packet 006A for the Windows first-time atomic-install primitive;
6. draft Packet 006B only after 006A passes.

Canonical-promotion approval and implementation remain blocked until Result 025 Gate A and Gate B requirements close.