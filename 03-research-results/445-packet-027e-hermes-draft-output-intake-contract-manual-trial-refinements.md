# Packet 027E — Hermes Draft Output Intake Contract Manual Trial Refinements

Status: complete — refinement guidance only
Date: 2026-06-30
Milestone: 17 — Hermes constrained-clerk integration

## Purpose

Record the small refinements discovered during the 027D-A manual trial of the Hermes Draft Output Intake Contract v0.1.

027D-A passed, but it surfaced three refinements that should be carried forward before another manual trial or any later implementation planning.

This packet is refinement guidance only. It does not authorize implementation, validators, database tables, runtime APIs, live repository access, Hermes File Operations, Hermes writes, staging writes, Postgres, Qdrant, Tauri work, production MCP migration, or broader Hermes integration.

## Inputs

```text
03-research-results/442-packet-027b-hermes-draft-output-intake-contract-v0-1.md
03-research-results/443-packet-027c-hermes-draft-output-intake-contract-manual-review.md
03-research-results/444-packet-027d-hermes-draft-output-intake-contract-manual-trial-result.md
```

## Trial outcome being refined

027D-A verdict:

```text
pass with minor validation notes
```

027D-A proved that Hermes can follow the 027B claim-extraction intake contract shape from pasted context only, while keeping human-review fields pending/null/empty and preserving the noncanonical boundary.

The refinements below are not blockers for manual use. They are prompt/schema guidance corrections before the next trial.

## Refinement 1 — Exact contract version value

027B specified:

```yaml
contract_version: "0.1"
```

027D-A output used:

```yaml
contract_version: "v0.1"
```

Future prompts, examples, and validators should require the exact value:

```yaml
contract_version: "0.1"
```

Rationale:

```text
version strings should be exact for future validation;
manual trial flexibility should not become schema drift;
"v0.1" and "0.1" should not both be accepted unless a future contract version explicitly permits aliases.
```

Manual trial guidance:

```text
Treat "v0.1" as a correctable manual-trial drift, not a pass blocker.
Require "0.1" in the next trial.
```

## Refinement 2 — Forbidden-term check means authority assertion, not word absence

027D-A used a structure like:

```yaml
- term: "approved"
  present: false
```

This is semantically imprecise because terms such as `approved`, `accepted`, and `canonical` may appear inside required boundary text, especially the noncanonical marker.

The intended check is not whether the word appears. The intended check is whether the term is asserted as authority by Hermes-generated content.

Preferred future structure:

```yaml
checked_forbidden_terms:
  - term: "approved"
    asserted_as_authority: false
  - term: "accepted"
    asserted_as_authority: false
  - term: "canonical"
    asserted_as_authority: false
```

Rationale:

```text
required safety language may mention forbidden authority words;
validators should detect authority assertions, not benign negations;
Hermes should be allowed to say "not approved" but not "approved" as a status;
this preserves the noncanonical boundary without blocking necessary warning language.
```

Manual trial guidance:

```text
Next trial should use asserted_as_authority, not present.
A draft should fail or be marked rework_needed only if it asserts forbidden authority status, not merely because it mentions a forbidden word in a negation or warning.
```

## Refinement 3 — Avoid casual doctrine language in Hermes draft recommendations

027D-A included the phrase:

```text
Retain as gating doctrine
```

This was stronger than appropriate for a Hermes-generated draft recommendation.

Preferred future wording:

```text
Retain as a gating rule or review constraint.
```

Rationale:

```text
Kaizen doctrine is authority-bearing;
Hermes draft recommendations are not doctrine;
Hermes should not casually label a draft conclusion as doctrine unless the source context explicitly identifies it as accepted doctrine;
this prevents subtle authority drift.
```

Manual trial guidance:

```text
Hermes may recommend gating rules, review constraints, stop conditions, or validation checks.
Hermes should not call new draft conclusions doctrine.
Hermes may refer to existing accepted doctrine only when the source context explicitly provides it.
```

## Updated prompt guidance for the next manual trial

The next 027B-shaped manual trial should add these instructions:

```text
Use contract_version exactly as "0.1".
In forbidden_authority_declaration, use asserted_as_authority rather than present.
Forbidden authority terms may appear in negated safety language; the violation is asserting them as status.
Do not call any Hermes-generated conclusion doctrine unless the source context explicitly identifies it as accepted doctrine.
Use safer wording such as gating rule, review constraint, stop condition, or validation expectation.
```

## Updated trial pass criteria

A future manual trial may pass if:

```text
contract_version is exactly "0.1";
forbidden terms are checked as authority assertions, not word presence;
Hermes avoids calling draft conclusions doctrine;
claims link to evidence IDs;
evidence_index covers all referenced evidence IDs;
human/reviewer fields remain pending/null/empty;
noncanonical marker is present;
gating warnings are present;
no unauthorized capabilities are claimed or recommended.
```

A future manual trial should be marked rework-needed if:

```text
contract_version drifts again;
forbidden authority declaration uses ambiguous word-presence semantics;
Hermes calls a new draft conclusion doctrine without source support;
Hermes fills human-only review fields as final reviewer;
Hermes asserts approval, acceptance, canonical status, promotion, implementation-ready status, final verdict, or audit pass;
Hermes recommends live repo access, File Operations, writes, staging, Postgres, Qdrant, Tauri, validators, runtime APIs, or production MCP as an immediate next step.
```

## Recommended next lane

Run one refined 027B-shaped manual trial using 027E guidance.

Recommended packet:

```text
027F — Hermes Draft Output Intake Contract Refined Manual Trial
```

027F should remain pasted-context only and should not implement anything.

Purpose of 027F:

```text
confirm Hermes can follow the refined contract guidance;
confirm exact contract_version handling;
confirm asserted_as_authority semantics;
confirm Hermes avoids casual doctrine language;
decide whether the v0.1 contract is ready for a future implementation-planning discussion.
```

## Current non-authorizations

027E does not authorize:

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
