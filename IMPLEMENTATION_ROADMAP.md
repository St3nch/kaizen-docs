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

Status: complete

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

Status: contract reconciliation complete; v0.2 owner-acceptance gate active; next implementation milestone not authorized

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

Review and explicitly accept or reject the Kaizen Project Standard v0.2 candidate at SHA-256 `e5749b3a228dc2aaaed634395ed81edd0626511d30ce44ea6f6a4f80684fc95e`.

Milestone 4 closure evidence:

```text
platform implementation: 845d65f356bd684c2f858f36aef54d0344791e43
task-packet completion amendment: 9c5dcdb6a5c25fa0e17248df7a7a66e4f6c36305
current-state amendment: e5e4eec1adc4ef26f9e735333dbb229b7bb59368
closure audit: 03-research-results/062-milestone-4-implementation-return-closure-audit.md
```

Milestone 5 retrospective, Decision 0013 acceptance, and first-slice specification reconciliation are complete. The v0.2 candidate remains non-authoritative until explicit owner acceptance.

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
15. Packet 009B Wave 5 is complete at vault commit `37a4bbd`; Result 054 passed.
16. The Wave 5 canonical audit hash is `f0a617936fda65afb8060690e85571a441c3bf0b71dc4ffe2281e2a809371dd8`.
17. Exactly one `intent` and one `committed` event exist for Wave 5.
18. The vault is clean and has no remote.
19. Packet 009B Wave 6 is complete at vault commit `a306b2c`; Result 056 passed.
20. The Wave 6 canonical task-packet hash is `f42549c6f4805c9d4da3cfe558c23596ec48fd159ddb9c2bcec84e5541a6380d`.
21. Exactly one `intent` and one `committed` event exist for Wave 6.
22. All six ordered Packet 009B notes are canonical.
23. The vault is clean and has no remote.
24. Governed amendment implementation is complete at platform commit `845d65f356bd684c2f858f36aef54d0344791e43`; Result 057 passed.
25. The full platform suite passes 230 tests, including focused amendment, recovery, contention, CLI, and first-time promotion regression coverage.
26. Completion-report amendment operation `kz-prom-01KTP9CTDRDG6KH8R2GRNNHHN7` completed at vault commit `9c5dcdb`; Result 059 passed.
27. The amended task-packet SHA-256 is `4e8c6081b1cfa6442f4c777abdf4eb4b1e883e0e1e1eb5d3f4b8838be971ce84`.
28. Exactly one `intent` and one `committed` event exist for the amendment.
29. The vault is clean and has no remote.
30. `current-state.md` amendment operation `kz-prom-01KTP9Y3Q9KMFVPWVXHS7SQQGK` completed at vault commit `e5e4eec`; Result 061 passed.
31. The amended current-state SHA-256 is `8ad7233e70d7194cc5bd93999f3eebbaa98168a39735de2e1d405c1cd2385d17`.
32. Exactly one `intent` and one `committed` event exist for the amendment.
33. The vault is clean at `e5e4eec`; no push occurred and no remote was created.
34. The governed implementation-return path is complete through canonical task-packet and current-state updates.
35. Milestone 4 closure audit Result 062 passed.
36. All nine required canonical chain artifacts validate with zero errors and zero warnings.
37. All six promotion operations and both amendment operations have exactly `intent -> committed`.
38. The full platform suite passes 230 tests at closure.
39. Platform, docs, and vault repositories are clean; the vault has no remote.
40. Milestone 4 is complete.
41. Milestone 5 retrospective Result 063 passed.
42. The non-authoritative v0.2 draft exists at `01-project-standard/kaizen-project-standard-v0.2-draft.md`.
43. Result 064 passes the draft for governed review but blocks authoritative acceptance.
44. Decision 0013 was explicitly accepted on 2026-06-09.
45. The event, workflow, recovery, validation, registry, ID, hammer, and spec-index contracts are reconciled as implemented baselines.
46. Result 066 passes the v0.2 candidate for explicit owner acceptance.
47. The v0.2 candidate SHA-256 is `e5749b3a228dc2aaaed634395ed81edd0626511d30ce44ea6f6a4f80684fc95e`.
48. No next implementation milestone is authorized.

Do not declare v0.2 authoritative or start the next implementation milestone until the owner explicitly accepts the exact candidate hash.
