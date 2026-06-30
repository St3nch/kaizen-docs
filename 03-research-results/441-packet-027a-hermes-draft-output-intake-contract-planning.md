# Packet 027A — Hermes Draft Output Intake Contract Planning

Status: planned — contract design only
Date: 2026-06-30
Milestone: 17 — Hermes constrained-clerk integration

## Purpose

Define the narrow planning scope for a Hermes Draft Output Intake Contract.

This packet exists because 026G-A produced the first concrete Hermes draft artifact: a noncanonical claim-extraction draft with source metadata, claims, evidence links, limiting evidence, caveats, review needs, and suggested intake fields.

The next useful Kaizen step is to design a small typed intake contract for Hermes draft outputs before any deeper Hermes integration.

This packet is planning only. It does not authorize implementation.

## Inputs

```text
03-research-results/438-packet-026d-hermes-026b-a-no-file-access-orientation-test-result.md
03-research-results/439-packet-026e-026f-hermes-pasted-context-clerk-workflow-results.md
03-research-results/440-packet-026g-hermes-pasted-context-claim-extraction-draft-sample-result.md
```

## Current proven lane

The current Hermes evidence proves only pasted-context clerk behavior.

Proven:

```text
Hermes can preserve no-file-access posture from pasted context.
Hermes can produce useful noncanonical Layer 2 clerk reports from bounded context.
Hermes can produce a concrete claim-extraction draft sample.
Hermes can separate candidate claims from evidence and limiting evidence.
Hermes can declare noncanonical status and forbidden authority exclusions.
Hermes can identify review needs and gating warnings.
```

Not proven:

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
production suitability.
```

## Planning target

Design a narrow Hermes Draft Output Intake Contract for one draft-output family first:

```text
claim-extraction draft
```

The contract should be reusable later, but it must not over-generalize before one reviewed cycle exists.

The contract should describe how Kaizen receives a Hermes draft artifact so a human or future validator can:

```text
identify the source unit;
identify the generation context;
confirm noncanonical status;
inspect candidate claims;
trace each claim to supporting evidence;
see limiting or conflicting evidence;
read caveats and confidence;
record review disposition;
record partial acceptance or rejection notes;
preserve gating warnings;
prevent hidden authority transitions.
```

## Draft contract shape

The initial contract should be small enough to use by hand and later type-check.

Candidate top-level fields:

```text
contract_version
report_id
report_type
source_unit_id
source_unit_title
source_type
source_status
agent_name
agent_profile
model
provider
session_id
generation_context
created_at
noncanonical_marker
forbidden_authority_declaration
claims
evidence_index
validation_needs
gating_warnings
review_status
reviewer_notes
partial_acceptance_notes
rejection_reason
next_step_suggestion
```

Candidate `claims[]` fields:

```text
claim_id
statement
supporting_evidence_ids
conflicting_or_limiting_evidence_ids
confidence
caveat
review_recommendation
review_disposition
review_notes
```

Candidate `evidence_index[]` fields:

```text
evidence_id
summary
source_locator
source_status
```

## Required boundary fields

The intake contract must include explicit boundary fields:

```text
noncanonical_marker
forbidden_authority_declaration
gating_warnings
review_status
```

The first version should require `noncanonical_marker` to state that the draft carries no canonical, approved, accepted, promoted, implementation-ready, final-verdict, or audit-pass status.

The first version should require `forbidden_authority_declaration` to declare whether the draft asserted forbidden authority fields. A valid initial draft should declare none asserted.

The first version should require `gating_warnings` to state what the draft does not prove.

## Forbidden fields

The intake contract must not allow Hermes-generated draft fields that directly confer authority.

Forbidden direct authority fields:

```text
approved
accepted
canonical
promoted
implementation_ready
implementation-ready
final_verdict
audit_pass
audit_result
promotion_status
owner_approved
```

Human review fields may record a reviewer disposition, but those fields must be clearly distinguished from Hermes-generated content.

## Review status values

Initial review status should be one of:

```text
pending_review
accepted_as_draft
partially_accepted_as_draft
rejected
rework_needed
superseded
```

These statuses apply to draft handling only. They do not promote a draft to doctrine, canonical record, implementation-ready status, or audit result.

## Confidence values

Initial confidence values should be constrained to:

```text
low
medium
high
```

Confidence is a clerk confidence indicator, not an authority marker.

## Source status values

Initial source status values should include:

```text
canonical
noncanonical
synthetic_test
external_unreviewed
external_reviewed
unknown
```

The first 027A follow-on design should use `synthetic_test` or `noncanonical` for 026G-A-derived source material unless separately reviewed.

## Stop conditions

Stop the contract design lane if:

```text
the contract begins authorizing Hermes writes;
the contract assumes live repo access;
the contract assumes Postgres, Qdrant, or staging integration;
the contract grants Hermes authority fields;
the contract becomes broad enough to cover every future Hermes workflow before one reviewed cycle exists;
the contract drifts into Kaizen app UI, Tauri, production MCP, or long-term agent architecture;
the contract cannot clearly separate Hermes-generated draft content from human review disposition;
the contract loses evidence traceability from claim to source evidence ID.
```

## Acceptance criteria for the next design artifact

A future 027B design artifact may be accepted for planning if it provides:

```text
one narrow schema for claim-extraction drafts;
clear required and optional fields;
clear forbidden fields;
clear review disposition semantics;
clear separation of Hermes-generated fields from human-review fields;
example instance using the 026G-A draft sample;
validation rules that reject authority-conferring fields;
explicit non-authorizations;
no implementation code;
no live repo access;
no File Operations dependency.
```

## Recommended next packet

Prepare Packet 027B:

```text
Hermes Draft Output Intake Contract v0.1 — Claim-Extraction Draft Schema
```

027B should be a design/spec packet only. It should include:

```text
field definitions;
required/optional markers;
allowed enum values;
forbidden field rules;
example populated draft based on 026G-A;
validation expectations;
review workflow notes;
non-authorizations.
```

## Current non-authorizations

027A does not authorize:

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

## Result

027A defines the next narrow design lane:

```text
Hermes Draft Output Intake Contract v0.1 for claim-extraction drafts.
```

The contract should be designed before any broader Hermes integration and before any live repository access is reconsidered.
