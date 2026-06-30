# Packet 027B — Hermes Draft Output Intake Contract v0.1

Status: planned — contract specification only
Date: 2026-06-30
Milestone: 17 — Hermes constrained-clerk integration

## Purpose

Specify the first narrow Hermes Draft Output Intake Contract for Kaizen.

This v0.1 contract covers one draft-output family only:

```text
claim-extraction draft
```

The contract exists so Hermes can produce useful noncanonical claim-extraction drafts while Kaizen preserves evidence traceability, human review authority, and the distinction between draft output and accepted project intelligence.

This packet is design/specification only. It does not authorize implementation code, validators, database tables, runtime APIs, live repository access, Hermes File Operations, staging writes, Postgres access, Qdrant access, Tauri work, or production MCP migration.

## Inputs

```text
03-research-results/440-packet-026g-hermes-pasted-context-claim-extraction-draft-sample-result.md
03-research-results/441-packet-027a-hermes-draft-output-intake-contract-planning.md
```

## Contract name

```text
hermes.draft_output.claim_extraction.v0.1
```

## Contract intent

The contract receives a Hermes-generated draft artifact and makes it reviewable.

It must allow a reviewer to answer:

```text
What was generated?
Who/what generated it?
From what bounded source unit?
What is each candidate claim?
What evidence supports each claim?
What evidence limits or conflicts with each claim?
What caveats and confidence did Hermes assign?
What must a human still review?
What does this draft explicitly not establish?
Did Hermes assert any forbidden authority fields?
What is the human review disposition?
```

## Authority model

Hermes-generated fields are draft-only.

Human review fields are disposition-only unless a later, separate Kaizen process promotes content.

This contract must not treat Hermes output as:

```text
canonical;
approved;
accepted;
promoted;
implementation-ready;
final audit verdict;
project doctrine;
source of truth.
```

## Top-level schema

The v0.1 top-level object should contain these fields.

| Field | Required | Owner | Type | Notes |
| --- | --- | --- | --- | --- |
| `contract_version` | yes | system/intake | string | Must be `0.1` for this version. |
| `contract_name` | yes | system/intake | string | Must be `hermes.draft_output.claim_extraction.v0.1`. |
| `report_id` | yes | system/intake or reviewer | string | Stable local identifier for the draft artifact. |
| `report_type` | yes | Hermes | enum | Must be `claim_extraction_draft`. |
| `source_unit_id` | yes | Hermes | string | Identifier of bounded source unit consumed. |
| `source_unit_title` | yes | Hermes | string | Human-readable source unit title. |
| `source_type` | yes | Hermes | enum | Origin kind of the source material. |
| `source_status` | yes | Hermes | enum | Status of the source material. |
| `agent_name` | yes | Hermes/operator | string | Example: `Hermes Agent`. |
| `agent_profile` | no | operator | string | Optional profile/tool posture description. |
| `model` | yes | operator/Hermes | string | Example: `qwen/qwen3.7-plus`. |
| `provider` | no | operator/Hermes | string | Example: `OpenRouter`. |
| `session_id` | no | operator/Hermes | string | Hermes session identifier if available. |
| `generation_context` | yes | Hermes/operator | string | Short description of prompt/test lane. |
| `created_at` | no | system/intake | string | ISO-8601 timestamp if available. |
| `noncanonical_marker` | yes | Hermes | string | Explicit no-authority declaration. |
| `forbidden_authority_declaration` | yes | Hermes | object | Declares whether forbidden authority fields were asserted. |
| `claims` | yes | Hermes | array | Candidate claims. Must contain at least one item. |
| `evidence_index` | yes | Hermes | array | Evidence ID summaries. Must cover all referenced evidence IDs. |
| `validation_needs` | yes | Hermes | array | Human/validator checks needed before downstream use. |
| `gating_warnings` | yes | Hermes | array | Explicit anti-overread warnings. |
| `next_step_suggestion` | no | Hermes | string | Non-binding clerk suggestion. |
| `review_status` | yes | human/reviewer | enum | Initial value should be `pending_review`. |
| `reviewer_notes` | no | human/reviewer | string | Human notes. |
| `partial_acceptance_notes` | no | human/reviewer | array | Per-claim/section notes. |
| `rejection_reason` | no | human/reviewer | string | Required if `review_status` is `rejected`. |

## Allowed enum values

### `report_type`

```text
claim_extraction_draft
```

### `source_type`

```text
canonical_project_note
noncanonical_project_note
synthetic_internal_test
external_source
operator_supplied_context
unknown
```

### `source_status`

```text
canonical
noncanonical
synthetic_test
external_unreviewed
external_reviewed
unknown
```

### `review_status`

```text
pending_review
accepted_as_draft
partially_accepted_as_draft
rejected
rework_needed
superseded
```

These review statuses apply only to draft handling. They do not promote the draft to accepted doctrine, canonical intelligence, implementation-ready status, or audit result.

### `confidence`

```text
low
medium
high
```

Confidence is a clerk confidence indicator only. It is not an authority marker.

## Claim item schema

Each `claims[]` item should contain:

| Field | Required | Owner | Type | Notes |
| --- | --- | --- | --- | --- |
| `claim_id` | yes | Hermes | string | Stable within report. Example: `C1`. |
| `statement` | yes | Hermes | string | One core assertion only. |
| `supporting_evidence_ids` | yes | Hermes | array | Must reference IDs in `evidence_index`. |
| `conflicting_or_limiting_evidence_ids` | yes | Hermes | array | May be empty, but should be explicit. |
| `confidence` | yes | Hermes | enum | `low`, `medium`, or `high`. |
| `caveat` | yes | Hermes | string | Required anti-overread text. |
| `review_recommendation` | yes | Hermes | string | Non-binding clerk recommendation. |
| `review_disposition` | yes | human/reviewer | enum | Initial value should be `pending_review`. |
| `review_notes` | no | human/reviewer | string | Per-claim human notes. |

Allowed `review_disposition` values match `review_status` values:

```text
pending_review
accepted_as_draft
partially_accepted_as_draft
rejected
rework_needed
superseded
```

## Evidence index item schema

Each `evidence_index[]` item should contain:

| Field | Required | Owner | Type | Notes |
| --- | --- | --- | --- | --- |
| `evidence_id` | yes | Hermes/operator | string | Example: `E1`. |
| `summary` | yes | Hermes | string | Short summary of the evidence item. |
| `source_locator` | no | Hermes/operator | string | File path, pasted-bundle section, URL, or other locator if available. |
| `source_status` | yes | Hermes/operator | enum | Same enum as top-level `source_status`. |

The evidence index must include every evidence ID referenced by every claim.

## Forbidden authority fields

Hermes-generated draft content must not include fields that directly confer authority.

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
implementation_ready_status
canonical_status
approval_status
```

The contract may include human review/disposition fields, but those must remain clearly separate from Hermes-generated draft content.

## Required boundary declarations

### `noncanonical_marker`

Must explicitly state that the draft carries no canonical, approved, accepted, promoted, implementation-ready, final-verdict, or audit-pass status.

Example:

```text
This draft is noncanonical. It is not approved, accepted, canonical, promoted, implementation-ready, a final verdict, or an audit pass. It requires human/Kaizen review before any downstream use.
```

### `forbidden_authority_declaration`

Should be an object with at least:

```text
asserted_forbidden_authority: false
forbidden_terms_checked:
- approved
- accepted
- canonical
- promoted
- implementation_ready
- implementation-ready
- final_verdict
- audit_pass
- audit_result
- promotion_status
- owner_approved
notes: "No forbidden authority fields asserted by Hermes-generated content."
```

If a forbidden authority assertion is detected, the draft should be rejected or marked `rework_needed` before any downstream use.

### `gating_warnings`

Must include explicit anti-overread warnings.

For the current Hermes lane, expected warnings include:

```text
This draft does not prove live repository read behavior.
This draft does not prove live repository search behavior.
This draft does not prove root confinement.
This draft does not prove File Operations safety.
This draft does not authorize Hermes writes.
This draft does not authorize staging writes.
This draft does not authorize Postgres, Qdrant, Tauri, or production MCP work.
```

## Minimal validation expectations

A future validator for this contract should reject or flag a draft if:

```text
contract_name is not hermes.draft_output.claim_extraction.v0.1;
contract_version is not 0.1;
report_type is not claim_extraction_draft;
required top-level fields are missing;
claims is empty;
any claim lacks a statement;
any claim references evidence IDs absent from evidence_index;
confidence contains a value outside low/medium/high;
review_status contains a value outside the allowed enum;
noncanonical_marker is missing or weak;
forbidden_authority_declaration is missing;
Hermes-generated content asserts forbidden authority fields;
gating_warnings are missing;
Hermes-generated content claims live repo access, writes, production readiness, or authorization not present in the source context.
```

Validator design is not authorized by this packet. These are planning expectations only.

## Example instance based on 026G-A

This example is illustrative and noncanonical.

```yaml
contract_version: "0.1"
contract_name: "hermes.draft_output.claim_extraction.v0.1"
report_id: "sample-026g-a-claim-extraction-draft"
report_type: "claim_extraction_draft"
source_unit_id: "sample-source-001"
source_unit_title: "Hermes Pasted-Context Clerk Workflow Test Notes"
source_type: "synthetic_internal_test"
source_status: "synthetic_test"
agent_name: "Hermes Agent"
agent_profile: "skills disabled; File Operations disabled; Clarifying Questions enabled; chat-only output"
model: "qwen/qwen3.7-plus"
provider: "OpenRouter"
session_id: null
generation_context: "026G-A pasted-context draft-sample clerk test"
created_at: null
noncanonical_marker: "This draft is noncanonical. It is not approved, accepted, canonical, promoted, implementation-ready, a final verdict, or an audit pass. It requires human/Kaizen review before any downstream use."
forbidden_authority_declaration:
  asserted_forbidden_authority: false
  forbidden_terms_checked:
    - approved
    - accepted
    - canonical
    - promoted
    - implementation_ready
    - implementation-ready
    - final_verdict
    - audit_pass
    - audit_result
    - promotion_status
    - owner_approved
  notes: "No forbidden authority fields asserted by Hermes-generated content."
claims:
  - claim_id: "C1"
    statement: "Hermes can be run in a mode with no filesystem or repo access and still correctly restate Kaizen identity, accepted architecture, M17 posture, Hermes constrained-clerk role, and safety posture."
    supporting_evidence_ids: ["E1", "E2", "E5", "E7"]
    conflicting_or_limiting_evidence_ids: ["E8"]
    confidence: "medium"
    caveat: "Demonstrates bounded-context compliance only; does not establish capability on the live corpus."
    review_recommendation: "Confirm this claim does not over-attribute pasted-context tests."
    review_disposition: "pending_review"
    review_notes: null
  - claim_id: "C2"
    statement: "The Layer 2 read-only clerk workflow is executable today with zero repository surface risk, without granting Hermes filesystem or tool access."
    supporting_evidence_ids: ["E5", "E7"]
    conflicting_or_limiting_evidence_ids: ["E6", "E8"]
    confidence: "medium"
    caveat: "Viability is observed for the pasted-context lane; it does not imply live-corpus retrieval behaves the same way."
    review_recommendation: "Treat as evidence for pasted-context clerk usefulness only."
    review_disposition: "pending_review"
    review_notes: null
  - claim_id: "C3"
    statement: "In the observed Hermes Desktop setup, enabling per-tool read-only access is not achievable because File Operations bundles patch, read_file, search_files, and write_file into a single toggle."
    supporting_evidence_ids: ["E3", "E4"]
    conflicting_or_limiting_evidence_ids: []
    confidence: "medium"
    caveat: "Observation is specific to the observed setup and does not rule out another read-only tooling path."
    review_recommendation: "Verify against current Hermes UI before carrying forward as a live constraint."
    review_disposition: "pending_review"
    review_notes: null
  - claim_id: "C4"
    statement: "Passing pasted-context tests does not, by itself, demonstrate that Hermes can safely operate over the live Kaizen corpus or handle write-adjacent operations safely."
    supporting_evidence_ids: ["E6", "E8"]
    conflicting_or_limiting_evidence_ids: ["E1", "E2", "E5", "E7"]
    confidence: "high"
    caveat: "High confidence reflects explicit limitation in the evidence bundle, not live-corpus testing."
    review_recommendation: "Carry forward as a gating rule before any live repo, staging, or production step."
    review_disposition: "pending_review"
    review_notes: null
evidence_index:
  - evidence_id: "E1"
    summary: "026B-A pasted-context orientation succeeded."
    source_locator: "026G-A pasted bundle"
    source_status: "synthetic_test"
  - evidence_id: "E2"
    summary: "026B-A safety-posture confirmation recorded."
    source_locator: "026G-A pasted bundle"
    source_status: "synthetic_test"
  - evidence_id: "E3"
    summary: "Observed File Operations bundling: patch, read_file, search_files, write_file."
    source_locator: "026G-A pasted bundle"
    source_status: "synthetic_test"
  - evidence_id: "E4"
    summary: "Kaizen kept File Operations disabled because of the bundle."
    source_locator: "026G-A pasted bundle"
    source_status: "synthetic_test"
  - evidence_id: "E5"
    summary: "026E-A observed Layer 2 viability on pasted context."
    source_locator: "026G-A pasted bundle"
    source_status: "synthetic_test"
  - evidence_id: "E6"
    summary: "026E-A warned that pasted-context success is not live-corpus safety."
    source_locator: "026G-A pasted bundle"
    source_status: "synthetic_test"
  - evidence_id: "E7"
    summary: "026F-A recommended grounding the draft-output intake contract in at least one real draft sample."
    source_locator: "026G-A pasted bundle"
    source_status: "synthetic_test"
  - evidence_id: "E8"
    summary: "Current tests do not prove live repository read/search, root confinement, large-corpus behavior, File Operations safety, staging-write safety, typed-tool integration, memory safety, schedule safety, or production suitability."
    source_locator: "026G-A pasted bundle"
    source_status: "synthetic_test"
validation_needs:
  - "Confirm C1 and C2 do not over-attribute pasted-context tests."
  - "Verify File Operations bundling against the current observed Hermes UI before treating it as persistent."
  - "Carry C4 forward as a gating rule before live repo access or typed-tool integration."
gating_warnings:
  - "This draft does not prove live repository read behavior."
  - "This draft does not prove live repository search behavior."
  - "This draft does not prove root confinement."
  - "This draft does not prove File Operations safety."
  - "This draft does not authorize Hermes writes."
  - "This draft does not authorize staging writes."
  - "This draft does not authorize Postgres, Qdrant, Tauri, or production MCP work."
next_step_suggestion: "Use this example to design a minimal review cycle before implementing validators."
review_status: "pending_review"
reviewer_notes: null
partial_acceptance_notes: []
rejection_reason: null
```

## Review workflow notes

The intended manual workflow for v0.1 is:

```text
1. Hermes produces draft output in the contract shape.
2. Intake/reviewer confirms required fields are present.
3. Intake/reviewer checks evidence IDs are traceable within the bounded source unit.
4. Intake/reviewer checks noncanonical marker and forbidden authority declaration.
5. Intake/reviewer reviews each claim independently.
6. Intake/reviewer records pending, accepted-as-draft, partially-accepted-as-draft, rejected, rework-needed, or superseded disposition.
7. Any future promotion or canonicalization happens only through a separate Kaizen-governed process.
```

## Stop conditions

Stop or mark rework-needed if:

```text
Hermes asserts approval, acceptance, canonical status, promotion, implementation-ready status, final verdict, or audit pass;
Hermes claims it read live repo files when the run was pasted-context only;
Hermes claims it wrote files or changed state;
Hermes omits evidence IDs for candidate claims;
Hermes fails to include limiting evidence or caveats;
Hermes recommends enabling File Operations, live repo access, staging writes, Postgres, Qdrant, Tauri, or production work as an immediate next step;
the draft cannot be reviewed claim-by-claim;
human review fields are mixed into Hermes-generated content without separation.
```

## Acceptance criteria for this packet

027B is acceptable as a design/spec packet if it:

```text
specifies one narrow schema for claim-extraction drafts;
identifies required and optional fields;
identifies allowed enum values;
identifies forbidden authority fields;
separates Hermes-generated fields from human-review fields;
includes an example instance grounded in 026G-A;
defines validation expectations without implementing validators;
defines review workflow notes;
preserves non-authorizations.
```

## Current non-authorizations

027B does not authorize:

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

## Recommended next lane

Run one manual review of the 027B example instance.

The next packet should not implement code. It should decide whether this schema is:

```text
accepted for manual v0.1 trial;
needs simplification;
needs field changes;
needs an additional pasted-context sample;
or should be rejected and redesigned.
```

A later implementation packet may only be considered after a manual review cycle validates that the contract is useful and not overbuilt.
