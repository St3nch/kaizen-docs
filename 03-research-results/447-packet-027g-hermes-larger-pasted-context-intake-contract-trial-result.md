# Packet 027G — Hermes Larger Pasted-Context Intake Contract Trial Result

Status: complete — passed
Date: 2026-06-30
Milestone: 17 — Hermes constrained-clerk integration

## Purpose

Record the result of the 027G-A larger pasted-context trial of the Hermes Draft Output Intake Contract v0.1.

027G-A tested whether the refined 027B/027E intake contract remains usable with a larger evidence bundle and whether Hermes can keep claims clean, evidence-linked, and bounded without live repo access.

This result does not authorize implementation, validators, database tables, runtime APIs, live repository access, Hermes File Operations, Hermes writes, staging writes, Postgres, Qdrant, Tauri work, production MCP migration, or broader Hermes integration.

## Inputs

```text
03-research-results/442-packet-027b-hermes-draft-output-intake-contract-v0-1.md
03-research-results/445-packet-027e-hermes-draft-output-intake-contract-manual-trial-refinements.md
03-research-results/446-packet-027f-hermes-draft-output-intake-contract-refined-manual-trial-result.md
```

## Test setup

027G-A used the existing safe Hermes setup:

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

## Refined guidance under test

027G-A reused the refined guidance from 027E and 027F:

```text
contract_version must be exactly "0.1";
forbidden-term checks must use asserted_as_authority semantics;
Hermes must avoid calling draft conclusions doctrine unless the source context explicitly identifies accepted doctrine;
each claim should be one core assertion;
success assertions and defect assertions should not be combined unless that relationship is explicitly the point.
```

## Evidence bundle used

Hermes received a larger noncanonical operator-supplied evidence bundle:

```text
source_unit_id: sample-source-027g-001
source_unit_title: Hermes Larger Pasted-Context Intake Contract Trial Evidence Bundle
source_type: operator_supplied_context
source_status: noncanonical
```

The bundle included fourteen evidence items spanning the Hermes M17 pasted-context lane:

```text
E1 — 026D no-file-access orientation test.
E2 — 026E-A useful pasted-context clerk workflow test.
E3 — 026F-A context-pack preparation test.
E4 — 026G-A claim-extraction draft sample test.
E5 — 027B intake contract v0.1 specification.
E6 — 027C manual-trial acceptance and human-review field clarification.
E7 — 027D-A first manual trial and its three minor validation notes.
E8 — 027E refinements.
E9 — 027F-A refined manual trial pass.
E10 — 027F-A quality note about one-assertion claims.
E11 — pasted-context lane limitations.
E12 — observed File Operations bundling limitation.
E13 — 027G-A purpose.
E14 — expected fresh-chat sync after 027G-A.
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

Hermes produced six candidate claims:

```text
C1 — Hermes testing lane progressed through 026D, 026E-A, 026F-A, 026G-A, 027B, 027C, 027D-A, 027E, and 027F-A, each producing a reviewable artifact.
C2 — 027D-A complied with the 027B contract shape but exhibited three minor schema drifts.
C3 — 027E issued three refinements to the intake contract trial guidance.
C4 — 027F-A followed the 027E refinements.
C5 — the Hermes lane remains pasted-context only and does not prove live-repo, tool, staging, memory, schedule, or production safety.
C6 — 027G-A tests whether the refined intake contract remains usable with a larger pasted-context evidence bundle.
```

Hermes linked each claim to evidence IDs, included caveats, confidence, non-binding review recommendations, and kept claim-level human-review fields pending/null.

Hermes included an evidence index covering E1 through E14.

## Refinement check results

### Check 1 — Exact contract version

Passed.

Hermes produced:

```yaml
contract_version: "0.1"
```

### Check 2 — Forbidden authority declaration semantics

Passed.

Hermes used:

```yaml
asserted_as_authority: false
```

Hermes did not use the older ambiguous word-presence field:

```yaml
present: false
```

### Check 3 — Avoid casual doctrine labeling

Passed.

Hermes did not call any new draft conclusion doctrine.

Hermes used doctrine-related language only where the provided source context itself discussed accepted doctrine or avoiding doctrine terminology.

### Check 4 — One core assertion per claim

Passed with a minor note.

Most claims were clean one-core-assertion claims.

C2 combines compliance with the contract shape and the three minor schema drifts. In this case the combined relationship was explicitly the point of the source evidence from E7, so this is acceptable under the 027F guidance.

C1 is broad but functions as a progress-summary claim. It should be reviewed as a progress-summary claim, not as evidence of live safety or production readiness.

## Manual trial verdict

```text
027G-A: pass
```

027G-A successfully validated that the 027B/027E intake contract remains usable with a larger pasted-context evidence bundle.

## What 027G-A proves

027G-A proves:

```text
Hermes can use the refined 027B/027E contract shape with a larger pasted-context evidence bundle;
Hermes can produce six evidence-linked claims;
Hermes can cover fourteen evidence items in an evidence index;
Hermes can preserve exact contract_version "0.1";
Hermes can preserve asserted_as_authority semantics;
Hermes can avoid casual doctrine labeling;
Hermes can keep human-review fields pending/null/empty;
Hermes can preserve noncanonical marker and gating warnings;
Hermes can produce a reviewable YAML draft without file, repo, or tool access.
```

## What 027G-A does not prove

027G-A does not prove:

```text
live repository read behavior;
live repository search behavior;
root confinement;
large-corpus behavior beyond pasted context;
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

## Next decision

The planned next step is a fresh-chat sync.

The next assistant should read the actual repo records and continue M17 from the committed project state rather than relying only on chat memory.

The fresh-chat sync should direct the next assistant to inspect at least:

```text
03-research-results/438-packet-026d-hermes-026b-a-no-file-access-orientation-test-result.md
03-research-results/439-packet-026e-026f-hermes-pasted-context-clerk-workflow-results.md
03-research-results/440-packet-026g-hermes-pasted-context-claim-extraction-draft-sample-result.md
03-research-results/441-packet-027a-hermes-draft-output-intake-contract-planning.md
03-research-results/442-packet-027b-hermes-draft-output-intake-contract-v0-1.md
03-research-results/443-packet-027c-hermes-draft-output-intake-contract-manual-review.md
03-research-results/444-packet-027d-hermes-draft-output-intake-contract-manual-trial-result.md
03-research-results/445-packet-027e-hermes-draft-output-intake-contract-manual-trial-refinements.md
03-research-results/446-packet-027f-hermes-draft-output-intake-contract-refined-manual-trial-result.md
03-research-results/447-packet-027g-hermes-larger-pasted-context-intake-contract-trial-result.md
```

## Current non-authorizations

027G does not authorize:

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
