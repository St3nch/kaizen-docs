# Packet 027F — Hermes Draft Output Intake Contract Refined Manual Trial Result

Status: complete — passed
Date: 2026-06-30
Milestone: 17 — Hermes constrained-clerk integration

## Purpose

Record the result of the 027F-A refined manual trial of the Hermes Draft Output Intake Contract v0.1.

027F-A tested whether Hermes could follow the 027E refinements while producing a 027B-shaped claim-extraction draft from pasted context only.

This result does not authorize implementation, validators, database tables, runtime APIs, live repository access, Hermes File Operations, Hermes writes, staging writes, Postgres, Qdrant, Tauri work, production MCP migration, or broader Hermes integration.

## Inputs

```text
03-research-results/442-packet-027b-hermes-draft-output-intake-contract-v0-1.md
03-research-results/444-packet-027d-hermes-draft-output-intake-contract-manual-trial-result.md
03-research-results/445-packet-027e-hermes-draft-output-intake-contract-manual-trial-refinements.md
```

## Test setup

027F-A used the existing safe Hermes setup:

```text
Skills: all off
Toolsets:
- Clarifying Questions: on
- File Operations: off
- everything else: off
Model intended: qwen/qwen3.7-plus through OpenRouter
```

Hermes received pasted context only.

No filesystem access, repo access, write access, terminal, code execution, browser automation, messaging, schedules, memory mutation, skill mutation, Postgres, Qdrant, staging, or production action was authorized.

## Refined guidance tested

027F-A tested the three refinements from 027E:

```text
1. contract_version must be exactly "0.1", not "v0.1".
2. Forbidden-term checks must use asserted_as_authority semantics, not word-presence semantics.
3. Hermes draft recommendations must avoid calling draft conclusions doctrine unless the source context explicitly identifies them as accepted doctrine.
```

## Evidence bundle used

Hermes received a noncanonical operator-supplied evidence bundle:

```text
source_unit_id: sample-source-027f-001
source_unit_title: Hermes Refined Intake Contract Manual Trial Evidence Bundle
source_type: operator_supplied_context
source_status: noncanonical
```

The bundle included eight evidence items:

```text
E1 — 027D-A passed the first manual trial and produced the required contract shape.
E2 — 027D-A drifted by using contract_version "v0.1" instead of "0.1".
E3 — 027D-A used present:false; 027E clarified asserted_as_authority semantics.
E4 — 027D-A used "Retain as gating doctrine"; 027E clarified Hermes should avoid casual doctrine language.
E5 — 027E refined guidance for exact version, asserted_as_authority, and safer language.
E6 — Hermes lane remains pasted-context only and does not prove live-read/search or production suitability.
E7 — File Operations remains off because observed setup bundles read/search with write/patch.
E8 — 027F-A tests whether Hermes can follow refined 027E guidance in a reviewable 027B-shaped draft.
```

## Hermes output summary

Hermes returned YAML only.

Hermes included the expected 027B-shaped fields:

```text
contract_version;
contract_name;
report_id;
report_type;
source metadata;
agent/model/provider metadata;
generation_context;
noncanonical_marker;
forbidden_authority_declaration;
claims;
evidence_index;
validation_needs;
gating_warnings;
next_step_suggestion;
review_status;
reviewer_notes;
partial_acceptance_notes;
rejection_reason.
```

Hermes produced four candidate claims:

```text
C1 — 027D-A passed the first manual trial but had minor contract_version drift.
C2 — 027E refined guidance for exact version, authority-assertion semantics, and avoiding casual doctrine language.
C3 — Hermes testing lane remains pasted-context only and does not prove live repository, tool, staging, or production safety.
C4 — 027F-A tests whether Hermes can follow refined 027E guidance while producing a 027B-shaped draft from pasted context only.
```

Hermes linked each claim to evidence IDs, included confidence values, caveats, review recommendations, and kept claim-level review fields pending/null.

## Refinement check results

### Check 1 — Exact contract version

Passed.

Hermes produced:

```yaml
contract_version: "0.1"
```

No `v0.1` drift was observed.

### Check 2 — Forbidden authority declaration semantics

Passed.

Hermes used:

```yaml
asserted_as_authority: false
```

Hermes did not use the previous ambiguous word-presence structure:

```yaml
present: false
```

This confirms Hermes can follow the intended semantics: forbidden terms may appear in negated safety language, but must not be asserted as authority.

### Check 3 — Avoid casual doctrine language

Passed.

Hermes mentioned `doctrine` only while describing the previous refinement itself and the instruction to avoid doctrine language unless source context explicitly identifies accepted doctrine.

Hermes did not call any new draft conclusion doctrine.

## Manual trial verdict

```text
027F-A: pass
```

027F-A successfully validated the 027E refinements in a pasted-context manual trial.

## Minor quality note

One non-blocking quality note was observed.

Claim C1 combined a success assertion and a defect assertion:

```text
027D-A passed the first manual trial ... but exhibited minor schema drift.
```

This is acceptable in this specific context because the relationship between pass and minor drift was part of the evidence bundle.

However, future claim-extraction guidance should prefer one core assertion per claim and should avoid combining a success assertion with a defect assertion unless the combined relationship is the point.

Suggested future guidance:

```text
Each claim should avoid combining a success assertion and a defect assertion unless the combined relationship is the point.
```

## What 027F-A proves

027F-A proves:

```text
Hermes can follow the refined 027B/027E claim-extraction intake guidance from pasted context;
Hermes can use exact contract_version "0.1";
Hermes can use asserted_as_authority semantics;
Hermes can avoid casual doctrine labeling;
Hermes can produce YAML-only output;
Hermes can link claims to evidence IDs;
Hermes can leave human-review fields pending/null/empty;
Hermes can preserve gating warnings and noncanonical status.
```

## What 027F-A does not prove

027F-A does not prove:

```text
live repository read behavior;
live repository search behavior;
root confinement;
large-corpus behavior;
File Operations safety;
ability to separate read/search from write/patch;
staging write safety;
skill-management safety;
memory safety;
schedule safety;
browser or messaging safety;
typed-tool integration;
production suitability;
validator implementation readiness;
database schema readiness;
runtime API readiness.
```

## Recommended next lane

The contract lane is now strong enough to choose between two next paths:

```text
Option A — run a larger pasted-context bundle trial using the refined 027B/027E guidance.
Option B — pause and sync a fresh chat before continuing M17.
```

If continuing immediately, the next useful test should remain no-file-access and pasted-context only, but should use a larger or more complex evidence bundle to test whether the contract stays usable with more evidence and more claims.

No implementation packet should be opened until a later explicit decision accepts the contract for implementation planning.

## Current non-authorizations

027F does not authorize:

```text
Hermes filesystem access;
Hermes File Operations;
Hermes writes;
Hermes staging writes;
Hermes canonical writes;
Hermes source-repo access;
Hermes terminal access;
Hermes code execution;
Hermes browser automation;
Hermes external messaging;
Hermes schedules or always-on behavior;
Hermes memory mutation;
Hermes skill mutation;
Postgres access;
Qdrant access;
Tauri implementation;
Kaizen app implementation;
custom chatbox implementation;
platform mutation;
vault mutation;
staging mutation;
database mutation;
Qdrant runtime start;
production MCP migration;
commercial or production use;
implementation of validators;
creation of database tables;
creation of runtime APIs.
```
