# Packet 026C — Hermes Target Capability Profile

Status: complete — target capability profile recorded
Date: 2026-06-30
Milestone: 17 — Hermes constrained-clerk integration

## Purpose

Record the intended non-gimped Hermes role for Kaizen.

026A established that Hermes Desktop / Hermes Agent should be used first as a controlled proof harness, not as the permanent Kaizen app.

026B established the first read-only tool-inventory and orientation test contract.

026C clarifies that the restricted first test is not the final ambition. Kaizen does not want a crippled Hermes. Kaizen wants a powerful clerk whose useful capabilities are routed through governed boundaries instead of broad raw access.

## Core position

The first Hermes test is intentionally narrow.

The target Hermes role is not narrow.

Kaizen should grow Hermes toward full clerk usefulness after each capability is proven, bounded, observable, reversible, and governed.

The distinction is:

```text
Gimped Hermes = artificially useless, cannot perform meaningful Kaizen clerk work.
Governed Hermes = powerful clerk, but every dangerous capability is routed through typed tools, validation, audit, and human authority.
```

Kaizen wants governed Hermes, not gimped Hermes.

## Target role

Hermes should eventually become Kaizen's high-volume operational and research clerk.

Hermes should help move work through:

```text
capture
-> source intake
-> source summary drafts
-> claim extraction drafts
-> duplicate checks
-> context-pack preparation
-> validation report summaries
-> review queue preparation
-> task-packet draft scaffolding
-> implementation-return intake support
-> post-launch improvement loop support
```

Hermes should be good at doing the repetitive, mechanical, evidence-preserving work that slows down humans and frontier models.

Hermes should not become the authority-bearing reviewer, approver, promoter, auditor of record, source-of-truth owner, database owner, vector-index owner, publisher, or business operator.

## Target capability ladder

Hermes should earn capabilities in layers.

### Layer 0 — Profile and tool inventory

Hermes must prove:

```text
dedicated Kaizen profile;
exact tool inventory capture;
unsafe tools absent or isolated;
persistent memory behavior known;
skill-management behavior known;
schedule and always-on behavior known;
external messaging behavior known;
local versus remote/backend behavior known.
```

### Layer 1 — Read-only orientation

Hermes may:

```text
read approved docs;
search approved docs;
follow the approved entrypoint;
summarize project posture;
summarize Hermes restrictions;
identify stale assumptions;
report open questions;
report no writes.
```

This is the 026B test lane.

### Layer 2 — Read-only clerk workflows

After orientation passes, Hermes should be tested on useful read-only work:

```text
find duplicate or overlapping docs;
produce source-map summaries;
prepare context-pack review reports;
identify stale references;
summarize validation failures;
compare task packet scope against governing specs;
extract candidate claims from supplied evidence;
prepare implementation-return review checklists;
prepare review queue summaries;
find missing links or missing required sections;
identify contradictions across approved docs.
```

Outputs remain chat/session artifacts or human-reviewed drafts. No filesystem writes are required at this layer.

### Layer 3 — Draft generation without direct writes

Hermes may produce complete draft text for human or steward insertion, including:

```text
source-summary drafts;
claim drafts;
audit finding drafts without verdicts;
task-packet draft sections;
context-pack candidate text;
operator checklist drafts;
research-intake summaries;
implementation-return intake summaries;
post-launch improvement candidates.
```

At this layer Hermes still does not write files. It drafts; Kaizen or a human decides what to record.

### Layer 4 — Staging create through Kaizen wrapper

Only after wrapper and hammer proof, Hermes may request creation of one new staged Markdown artifact through a Kaizen-owned typed tool.

Allowed future operation shape:

```text
create_staged_markdown(
  logical_path,
  content,
  note_type,
  project,
  agent_id,
  model_id,
  session_id,
  request_id,
  search_run_id
)
```

Hermes still does not receive raw filesystem write access.

The wrapper owns:

```text
path confinement;
create-new semantics;
no overwrite;
validation;
provenance;
audit evidence;
failure codes;
idempotency;
rejection of traversal, absolute, UNC, device, extended-length, symlink, junction, and sibling-repository escape.
```

### Layer 5 — Governed diff proposals

Hermes may later propose edits to existing staged or canonical material only as diffs.

Canonical changes require:

```text
read-before-edit;
reviewable diff;
validation;
changed-since-review check;
human approval;
human-operated promotion or amendment workflow;
append-only evidence.
```

Hermes may not silently apply canonical edits.

### Layer 6 — Typed operational reads

After Operational Postgres and other typed read services mature, Hermes may use narrow read/result tools such as:

```text
get_project_status;
list_review_queue_items;
get_validation_result;
get_context_pack_manifest;
get_operation_evidence;
get_approved_observatory_result;
get_agent_run_summary.
```

Hermes must not receive arbitrary SQL, raw database credentials, schema browsing, migrations, role management, backup/restore, or admin tools.

### Layer 7 — Typed retrieval reads

After Qdrant retrieval gateway work matures, Hermes may use narrow retrieval tools such as:

```text
search_approved_context;
retrieve_project_evidence;
find_related_approved_notes;
retrieve_source_locator_candidates;
search_supersedence_aware_context.
```

Hermes must not receive direct Qdrant credentials, store/upsert/delete tools, collection administration, scroll/list dump tools, exact-point bypass tools, or arbitrary payload filters.

### Layer 8 — Integrated Kaizen worker

If earlier layers pass, Hermes can become a worker behind Kaizen's app, service, or operator command surface.

Pattern:

```text
Human uses Kaizen
-> Kaizen builds governed context pack
-> Kaizen grants typed tool surface
-> Hermes performs bounded clerk work
-> Kaizen records draft/proposal/evidence
-> validation runs
-> human reviews and approves if authority is needed
```

Hermes can be powerful here because Kaizen owns the context, tools, state, audit, and authority boundaries.

## Full useful capability categories

A fully useful Hermes clerk should eventually support these categories.

### Research and source work

```text
intake raw sources;
summarize bounded source units;
extract evidence with locators;
separate evidence from interpretation;
identify caveats;
identify conflicting evidence;
prepare source-summary drafts;
prepare claim candidates.
```

### Project intelligence hygiene

```text
search before create;
detect duplicates and near-duplicates;
find stale path references;
find missing required sections;
find broken links;
find conflicting status claims;
find outdated milestone references;
regenerate or propose index/report updates from scans.
```

### Governance support

```text
prepare audit finding drafts;
prepare contradiction and gap reports;
prepare readiness checklists;
compare packets against specs;
compare implementation returns against acceptance criteria;
prepare owner-decision option summaries;
prepare non-authorizations and stop-condition summaries.
```

Hermes may draft governance evidence. Hermes may not issue governance authority.

### Context-pack and retrieval support

```text
assemble candidate context-pack manifests;
explain why files were included;
flag stale or missing inputs;
summarize retrieved evidence;
cross-check source locators;
identify retrieval gaps;
prepare resumption summaries.
```

Hermes should not choose authority by vibes. It must use approved manifests, typed retrieval, and visible evidence.

### Implementation-return support

```text
read implementation return;
compare changed paths to task scope;
compare validation evidence to required commands;
identify deviations;
identify residual risks;
prepare review checklist;
prepare handoff summary.
```

Hermes may not accept the return, mark complete, or promote results.

### Operator support

```text
summarize current project posture;
prepare next-step checklists;
explain blocked gates;
surface missing evidence;
prepare daily/weekly project status drafts;
summarize agent-run results.
```

Notifications, messaging, schedules, or always-on behaviors remain separate later gates.

## Capabilities that should be powerful but controlled

Hermes should eventually be able to use these capabilities when Kaizen has a safe surface for them:

```text
read/search across approved canonical Markdown;
Obsidian-aware read/navigation where useful;
project-scoped typed operational reads;
project-scoped semantic retrieval;
validation and lint report retrieval;
context-pack manifest inspection;
staged artifact creation through wrapper;
diff proposal generation;
agent-run/session provenance reporting;
review queue preparation;
operator summary generation.
```

These are not gimped capabilities. They are strong capabilities with the sharp edges covered.

## Capabilities Hermes should never receive raw

Hermes should not receive raw versions of these capabilities:

```text
broad filesystem write;
canonical write;
delete/move/rename;
overwrite;
terminal or arbitrary command execution for clerk workflows;
source repository modification;
Git commit/push;
arbitrary SQL;
direct Postgres credentials;
database migrations or DDL;
direct Qdrant credentials;
Qdrant store/upsert/delete/admin;
external publishing;
customer-facing action;
spend/submission authority;
private/customer/provider-data access outside approved lanes;
self-approval;
self-promotion;
audit verdict authority;
implementation-ready authority;
accepted decision/spec mutation;
unsupervised skill or memory governance mutation;
always-on/scheduled behavior without a separate gate.
```

This is not gimping Hermes. This is preventing Hermes from becoming a drunk forklift in the vault.

## What not to confuse

Do not confuse:

```text
first test restrictions
```

with:

```text
final Hermes capability target
```

The first test is deliberately narrow because it proves the harness.

The final target is broad clerk usefulness through governed operations.

## Relationship to Kaizen app / Tauri direction

A future Kaizen app or Tauri-style control shell should not replace Hermes, and Hermes should not replace the app.

The intended relationship is:

```text
Kaizen app = human control room
Hermes = clerk worker
Obsidian = canonical document workspace
Typed services = law
Humans = authority
```

The future Kaizen app may expose a chat/command panel, but the panel should call Kaizen services and route clerk tasks to Hermes or other agents through approved context packs and tool contracts.

## Capability design principle

For every desired Hermes capability, ask:

```text
What useful work should Hermes perform?
What context does it need?
What typed tool gives it the least dangerous sufficient capability?
What evidence should be logged?
What validation checks the output?
Who approves authority-bearing changes?
How do we prove it cannot escape its lane?
```

Do not ask only:

```text
Can Hermes do this if we give it broad access?
```

Broad access is not the design target.

Useful governed work is the design target.

## Next implication for 026B and later gates

026B should remain strict. It is a harness proof.

After 026B passes, the next test should not jump straight to writes. The better next lane is:

```text
read-only useful clerk workflow proof
```

Candidate next proof:

```text
Hermes reads approved docs and prepares a noncanonical context-pack review report or stale-reference report in chat/session output only.
```

That proves usefulness without granting mutation.

Only after useful read-only workflows pass should Kaizen consider wrapper-mediated staging creation.

## Current non-authorizations

026C does not authorize:

```text
Hermes execution;
Hermes write access;
Hermes staging writes;
Hermes canonical writes;
Hermes source-repo access;
Hermes terminal access;
Hermes external messaging;
Hermes scheduling or always-on behavior;
Hermes self-managed governance skills or memory;
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
commercial or production use.
```

## Related files

```text
03-research-results/435-packet-026a-hermes-desktop-integration-direction.md
03-research-results/436-packet-026b-hermes-read-only-tool-inventory-and-orientation-test-contract.md
07-hermes/hermes-permission-matrix.md
07-hermes/hermes-write-access-preconditions.md
05-specs/staging-write-wrapper-and-promotion-recovery.md
03-research-results/001-hermes-agent-boundaries-claude-summary.md
03-research-results/002-hermes-desktop-verification-gpt-summary.md
03-research-results/008-agent-access-and-write-safety-claude-summary.md
03-research-results/012-step-5a-composed-tool-capability-map.md
```
