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

Status: implementation complete; remote backup intentionally deferred by owner until the working-project checkpoint

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
- remote backup exists, or the owner has explicitly accepted a temporary local-only development period and recorded the later remote decision checkpoint.

# Milestone 3 - Safe staging and promotion

Status: complete; first real canonical promotion committed at vault `80bc093` and audited in Result 040

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
- staged content is retained as immutable review evidence after committed verification, and duplicate initial promotion is rejected.

# Milestone 4 - One governed project loop

Status: planning gate active

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

Review and explicitly approve or reject Packet 009B Wave 5 audit plan generation:

```text
operation: kz-prom-01KTMNHB6P6QKSHJWG6ANZEP94
staged source: projects/kaizen-platform/audits/governed-amendment-support-audit.md
source sha256: 204982aaa28a0404e6bade93df3ce72a22aa2923fd2a763b9a7af1ad5ad5c4f6
```

Current state:

1. Packet 009A is complete; Result 042 passed.
2. Packet 009B Wave 1 is complete at vault commit `999ccb0`; Result 046 passed.
3. Packet 009B Wave 2 is complete at vault commit `665d2a1`; Result 048 passed.
4. The Wave 2 canonical claim hash is `7729c7935573262c97d90cd7487a1505f2d35736b3b935df17740ab7507ac539`.
5. Exactly one `intent` and one `committed` event exist for Wave 2.
6. The vault is clean and has no remote.
7. Packet 009B Wave 3 is complete at vault commit `8a64d86`; Result 050 passed.
8. The Wave 3 canonical decision hash is `4c230de54ab81e67aa76114c9305c212e5714bac360ff7e8a9df740b2ed5b3d2`.
9. Exactly one `intent` and one `committed` event exist for Wave 3.
10. The vault is clean and has no remote.
11. Packet 009B Wave 4 is complete at vault commit `46e96a0`; Result 052 passed.
12. The Wave 4 canonical spec hash is `1fbf0e994f23320d15c72dba9f25018ff1dc33c21be54ba40c89e4398aae56b8`.
13. Exactly one `intent` and one `committed` event exist for Wave 4.
14. The vault is clean and has no remote.
15. The Wave 5 staged audit exists at the reserved source hash.
16. No Wave 5 operation directory exists.
17. Wave 5 execution and Wave 6 remain prohibited.

Do not generate the Wave 5 plan until the owner explicitly approves the named operation ID.
