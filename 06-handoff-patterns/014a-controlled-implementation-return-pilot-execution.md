---
id: kz-tp-01KV9NORTHSTAR014A0000000
type: task-packet
status: draft
project: kaizen-platform
created: 2026-06-14T17:40:00Z
updated: 2026-06-14T17:40:00Z
review_status: pending
authority: proposed
summary: "Execute the Milestone 9 Northstar Stockroom controlled implementation-return pilot through phased, separately approved gates."
---

# Task Packet 014A - Controlled Implementation-Return Pilot Execution

## Purpose

Execute Milestone 9 against the accepted Northstar Stockroom scenario and collect trustworthy evidence about Kaizen's full governed implementation-return loop, information-lane separation, failure detection, fresh-context resumption, controlled amendment behavior, and operational workload.

This packet is the single execution controller for the pilot. It uses phased stop gates rather than separate ceremonial packets for every harmless artifact. Approval of this packet authorizes only the phase explicitly approved by the owner.

## Governing evidence

```text
Kaizen Project Standard v0.2:
5b441a9e024446d8a9acb3a58a8a24ad020772f4e7666d697c495352bebb6e90

Roadmap v0.3 before this packet:
4a452f261e24957140f1324801202e0bcf1660742c87a951667070659f0bde59

Controlled pilot specification:
4830060301f8110884e5aae430849148592fab717bc1e6fcc0be0b76633373e3

Milestone 9 readiness result:
3d409697de50f7176d962daf52331c0c7781a96d615c9a697b7c8d727f3006ca

Packet 013E closure:
f53ad34558f897f86b8901425a8906ed1ab4b09bcc6ebc3ac09e0596acdd62db
```

## Live implementation checkpoints

```text
kaizen-docs:
08b2ab6a6cb0098758b7fddfdf49a807515cdc2f
clean, synchronized with origin/main

kaizen-platform:
ba3b5feca90e4fb5cb02e34981dc7ed86942962f
clean, no remotes

kaizen-vault:
2487de669bc44ed50e54fd5dbbfdd128ce659dbb
clean, no remotes

Go8:
71e678b484d07a31f93ab216bd6a1b3c15281987
clean, no remotes

canonical current-state SHA-256:
e50fc7d1d88257ecf6a818a47673775a9f8960e999a6a843872306227dd2b8c7
```

Any checkpoint or clean-state mismatch before a consequential phase stops that phase.

## Fixed roots

```text
disposable implementation repository:
C:\dev\kaizen-pilot-northstar

owner-private pilot control root:
C:\dev\kaizen-pilot-control\northstar

noncanonical pilot evidence root:
C:\dev\kaizen\staging\_pilot\northstar
```

The owner-private control root is outside all implementation-lane read paths and outside canonical Kaizen roots. It contains the sealed oracle, failure schedule, evaluator instructions, and private expected hashes.

The disposable repository is local-only and must have no remote.

The noncanonical evidence root may contain reviewed manifests, lane-transfer receipts, workload ledger entries, frozen return bundles, and audit evidence. It is not canonical truth.

No root may be created until Phase 1 is separately approved.

## Information lanes

| Lane | May read | Must not read | Mutation authority |
|---|---|---|---|
| Owner/oracle | all pilot evidence | none | owns oracle and final evaluations |
| Steward/planning | accepted project records, manifests, public fixture structure, implementation returns | must not leak oracle answers into implementation or fresh-context packs | drafts and coordinates only |
| GPT implementation | frozen implementer brief, accepted canonical records, released fixtures, disposable repo | oracle, golden outputs, evaluator instructions, unreleased injection schedule | disposable repo only under approved phase |
| Independent audit | accepted records, frozen implementation returns, tests and outputs; oracle only after return freeze | no pre-freeze oracle coaching | read/audit only |
| Fresh-context R1 | baseline canonical records and baseline repo checkpoint, with one evidence value omitted | oracle, prior chat, failure schedule | read/regenerate only |
| Fresh-context R2 | accepted change records and pre-amendment repo checkpoint | oracle, prior chat, failure schedule | outline only |
| Kaizen platform/tools | exact typed fixed-root inputs | owner-private oracle | deterministic enforcement only |

Any implementation or fresh-context access to oracle answers, expected output hashes, evaluator instructions, or unreleased injections invalidates the affected trial and must be preserved as a pilot failure.

## Artifact inventory

### Owner-private artifacts

```text
sealed-oracle/
  baseline-business-rules.md
  baseline-golden/reorder-report.csv
  baseline-golden/reorder-summary.md
  amended-golden/reorder-report.csv
  amended-golden/reorder-summary.md
  expected-artifact-manifest.json
  evaluator-instructions.md

failure-control/
  injection-schedule.json
  release-log.jsonl
```

Every owner-private file receives SHA-256 and size metadata in a private manifest. Only non-secret manifest structure and approved release receipts may enter staging evidence.

### Implementer-visible planning artifacts

```text
implementer-brief/
  request.md
  source-a.md
  source-b.md
  fixture-contract.md
  implementation-boundary.md
```

The initial implementer brief must preserve the deliberate ambiguity and contradictory threshold statements. It must not contain accepted answers, golden bytes, expected totals, or injection timing.

### Released fixtures

```text
fixtures/baseline/inventory.csv
fixtures/baseline/suppliers.csv
fixtures/invalid/inventory.csv
fixtures/invalid/suppliers.csv
```

The invalid fixture is released only at Injection 2.

### Disposable repository allowed tree

```text
src/
tests/
fixtures/
golden/
README.md
pyproject.toml
```

No other top-level path is allowed without a reviewed packet amendment.

### Pilot evidence

```text
artifact-manifest.json
lane-access-manifest.json
release-receipts.jsonl
workload-observation.jsonl
return-bundles/
audit-results/
resumption-r1/
resumption-r2/
```

The workload ledger is append-only operational evidence. It is not a new canonical note type.

## Frozen implementation shape

```text
language: Python 3.11
runtime dependencies: standard library only
allowed development dependency: pytest
required deterministic argument: --as-of YYYY-MM-DD
frozen pilot value: --as-of 2026-06-14
system-clock business-rule use: prohibited
network: prohibited
database: prohibited
external API: prohibited
GUI/service/background process: prohibited
remote repository: prohibited
```

The implementation must remain deliberately boring: CSV, Decimal, typed functions, deterministic CLI, and tests.

## Phase gates

## Phase 1 - Control artifacts, manifests, and repository bootstrap

Requires separate owner approval bound to this packet SHA-256.

Authorized work:

- create the three fixed roots;
- create and hash owner-private oracle and failure-control artifacts;
- create the ambiguous implementer brief and baseline fixtures;
- create the lane-access and artifact manifests;
- initialize the disposable Git repository with the allowed tree only;
- freeze all created bytes by SHA-256;
- verify implementation lane cannot read the owner-private root;
- create the empty append-only workload ledger;
- commit only the neutral repository scaffold and released inputs.

Stop before:

- accepting any Northstar claim or decision;
- canonical Kaizen mutation;
- implementation coding;
- releasing an injection;
- giving implementation or fresh-context lanes oracle access.

Phase 1 return must include exact paths, hashes, access checks, repository commit, and zero contamination findings.

## Phase 2 - Baseline Kaizen planning loop

Requires separate approval after Phase 1 evidence is reviewed.

Authorized work:

- create source summaries that preserve both contradictory threshold statements;
- create claims and conflict evidence;
- stop for explicit owner resolution of each consequential ambiguity;
- create accepted decisions for equality threshold, reserved inventory, stale pricing, inactive items, sorting, and decimal behavior;
- create the Northstar implementation specification;
- create and audit the baseline implementation task packet;
- create the canonical project overview and current-state candidate through normal governed staging;
- freeze the final implementer brief after accepted decisions and audited packet are available.

Stop before canonical execution and implementation coding unless separately approved.

Injection 1 occurs during this phase. Silent ambiguity resolution is a pilot failure.

## Phase 3 - Baseline implementation and failed-return sequence

Requires separate approval bound to the audited baseline task packet and disposable repository checkpoint.

Authorized work:

- implement the baseline CLI in the disposable repository without oracle access;
- release Injection 2 invalid fixtures at the defined gate;
- deliberately introduce and preserve Injection 3 availability bug in the first return;
- inject the wrong-but-green test under Injection 8;
- run deterministic tests and record exact commands, durations, commits, and outputs;
- create the intentionally incomplete first completion report for Injection 4;
- freeze the first implementation return before independent audit;
- run independent audit against accepted records and, only after return freeze, owner oracle;
- preserve failed code, tests, audit, and return evidence;
- correct implementation and tests in a new commit;
- produce complete return evidence.

No golden file may be edited by the implementation lane. Injection 6 is controlled by the owner/audit lane and must prove hash failure if attempted.

Stop before canonical governed return.

## Phase 4 - Governed baseline return and R1

Requires separate approval after the corrected baseline return passes independent functional and specification audit.

Authorized work:

- prepare, plan, approve, and execute the exact canonical Northstar baseline records through existing gates;
- run Injection 7 with the applicable repository intentionally dirty;
- verify read-only inspection remains structured while mutation fails closed;
- restore clean state only through explicit reviewed action without reset, stash, clean, or evidence destruction;
- commit exact canonical vault paths locally;
- run R1 in a fresh context with one required completion-evidence value omitted;
- freeze R1 response and regenerated outputs before oracle comparison;
- evaluate R1 against canonical truth and owner oracle.

Stop before change-request implementation.

## Phase 5 - Controlled amendment and R2

Requires separate approval after baseline and R1 acceptance.

Authorized work:

- introduce the accepted stale-price/inactive-supplier change request;
- perform impact analysis;
- amend the affected decision and specification through governed controls;
- version amended golden outputs without changing baseline evidence;
- create and audit a separate amended implementation packet;
- run R2 in a fresh context before amended implementation;
- implement, test, audit, return, and govern the amended slice;
- preserve baseline history and all failed evidence.

Stop before Milestone 9 closure acceptance.

## Phase 6 - Closure and workload reconciliation handoff

Requires separate closure authorization.

Authorized work:

- complete the workload ledger and evidence pointers;
- calculate recurrence counts;
- record concrete Markdown/file friction;
- apply the accepted Postgres nomination bar;
- list rejected database candidates and reasons;
- produce the implementation-return effectiveness assessment;
- produce the Milestone 9 closure audit;
- recommend a real-project pilot, controlled research/report pilot, or direct Milestone 10 reconciliation;
- obtain explicit owner closure acceptance.

No physical schema or production infrastructure may be designed.

## Eight injections and release controls

| ID | Controller | Earliest release | Expected proof |
|---|---|---|---|
| 1 contradictory source | steward/owner | Phase 2 source review | conflict preserved and human decision required |
| 2 invalid fixture | owner/audit | Phase 3 validation gate | row-level deterministic failure and no partial output |
| 3 deliberate availability bug | implementation under packet instruction | first Phase 3 return | tests/audit block acceptance; failed evidence retained |
| 4 incomplete completion report | owner/audit | first Phase 3 return | missing command/commit detected; no return acceptance |
| 5 rule-modifying change | owner | Phase 5 | governed amendment and versioned golden outputs |
| 6 silent golden edit attempt | owner/audit controller | after first output freeze | oracle hash failure; implementation correction required |
| 7 dirty governed return | steward/owner | Phase 4 | mutation blocked, read-only status preserved, no destructive cleanup |
| 8 wrong-but-green test | implementation under packet instruction | first Phase 3 return | independent audit catches specification violation despite green suite |

The full private timing schedule remains owner-private. Implementation receives only the injection currently released.

## Resumption contracts

### R1

Input pack:

- canonical baseline records;
- accepted baseline repository commit;
- no oracle, prior chat, or failure schedule;
- one required completion-evidence value intentionally omitted from the pack.

Pass requires correct current stage, last decision, unresolved work, next valid action, refusal to invent the omitted value, and byte-identical regenerated baseline outputs before oracle comparison.

### R2

Input pack:

- baseline canonical history;
- accepted change request and governing decision/packet;
- pre-amendment repository checkpoint;
- no oracle, prior chat, or failure schedule.

Pass requires complete impact identification, baseline immutability, versioned amended golden requirement, amendment-packet outline only, and missing-evidence detection.

## Workload ledger contract

Each entry records:

```text
record_name
evidence_pointer
producer
consumer
write_frequency
recurrence_count
read_pattern
lifecycle
immutability
project_scope
canonical_or_operational
markdown_or_file_friction
retention_observed_or_unknown
privacy_observed_or_unknown
transaction_boundary_observed_or_unknown
query_or_aggregation_need
```

A later Postgres nomination requires all three:

```text
recurrence_count >= 3 or appearance in both baseline and amendment
concrete Markdown/file friction
real query or aggregation need files cannot reasonably serve
```

## Deterministic verification

At each phase:

- verify exact repo heads and clean states;
- verify owner-private and implementation lane manifests;
- hash every frozen artifact;
- verify exact changed paths before every commit;
- scan changed text integrity;
- preserve failed evidence;
- record all commands, commits, tests, output hashes, and durations;
- verify no prohibited remote, network dependency, database, service, or extra top-level path;
- run independent audit where specified;
- stop on contamination, mismatch, or unauthorized scope expansion.

## Completion criteria

Milestone 9 succeeds only when all accepted specification criteria pass, including:

- baseline and amended oracle matches;
- all eight injections caught at expected gates;
- two complete implementation-return loops;
- R1 and R2 pass;
- canonical history preserved;
- workload ledger complete;
- no premature infrastructure or schema;
- explicit owner closure acceptance.

## Authorization boundary

Drafting, deterministic audit, and docs commit of this packet are authorized under the active Tier 1 workflow.

No pilot root, oracle, fixture, golden output, workload ledger, disposable repository, staging record, canonical record, source implementation, injection, fresh-context trial, or Milestone 9 execution is authorized until the owner separately approves the exact Packet 014A SHA-256 and named phase.

## Explicit exclusions

- production MCP migration or packaging;
- Postgres or Observatory schema;
- Qdrant, LangGraph, Hermes, broad UI, or provider integrations;
- customer data, spending, deployment, or external publication;
- new note types without proven need;
- implementation-lane oracle access;
- remote creation or push for platform, vault, Go8, or disposable pilot repo;
- cleanup or deletion;
- changing Kaizen doctrine merely to make the pilot pass.
