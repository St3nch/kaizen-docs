# Spec - Staging and Governed Canonical Mutation Workflow

Status: implemented baseline
Date: 2026-06-04
Updated: 2026-06-09
Related decisions:
- `04-design-decisions/0001-two-zone-agent-write-model.md`
- `04-design-decisions/0002-search-before-create-and-diff-before-write.md`
- `04-design-decisions/0007-foundation-resolution-for-v0.2.md`
- `04-design-decisions/0008-v0.2-operating-conventions.md`
- `04-design-decisions/0012-first-slice-contract-and-implementation-boundary.md`
- `04-design-decisions/0013-v0.2-first-slice-contract-reconciliation.md`

## Purpose

Define the safe path from staged Markdown candidates to canonical Kaizen notes through human-controlled first-time promotion or bounded amendment.

Prompt instructions are not access control. Canonical mutation is enforced by fixed roots, deterministic validation, immutable evidence, explicit owner gates, filesystem controls, event evidence, and scoped Git review.

## Roots

```text
C:\dev\kaizen\vault
C:\dev\kaizen\staging
```

Rules:

- the vault is canonical Markdown and governance evidence;
- staging is sibling, non-canonical preparation and immutable operation evidence;
- canonical notes never link into staging;
- callers do not select arbitrary roots;
- Qdrant, Postgres, Hermes, and plugins are not required for this workflow.

## Staged note requirements

Staged agent-created notes use the accepted field and note-type registries and include durable provenance:

```yaml
agent:
model:
session:
validation_status: pending | passed | failed
```

`validation_status` is staging-only and is removed from the canonical candidate. Agents may not set human-only approval, rejection, accepted authority, or audit verdicts.

## Validation layers

### Intrinsic note validation

Checks note syntax, fields, enums, IDs, sections, lifecycle, provenance, and prohibited content.

### Operation-context validation

Checks fixed roots, destination placement, canonical dependencies, relationship and local-link resolution, exact hashes, packet and operation binding, Git state, governance-log state, approval freshness, and filesystem safety.

Intrinsic canonical validation alone does not authorize mutation.

## Governed operation evidence

Every planned mutation has an immutable directory:

```text
staging/_promotion/operations/{operation-id}/
```

A reviewed plan contains or binds:

- `plan.json`;
- `validation.json`;
- normalized `canonical-candidate.md`;
- exact source or original staged candidate evidence;
- prior canonical bytes for amendment;
- reviewed diff for amendment;
- plan SHA-256;
- operation-context Git and governance-log bindings.

Plan generation creates no `approval.json`, canonical file, event, or Git commit.

## Human gates

### Gate 1 - plan generation

The owner explicitly approves one operation ID and destination for immutable plan generation.

### Gate 2 - execution

The owner explicitly approves:

- exact operation ID;
- exact plan SHA-256;
- exact destination and action.

Execution requires:

```text
PROMOTE-LIVE-EXACTLY-ONCE
```

Approval for one operation or plan does not authorize another.

## First-time promotion

Promotion creates one canonical note at a destination that does not already exist.

The plan binds:

- exact staged source;
- normalized canonical candidate;
- validation evidence;
- destination;
- note identity and type;
- review and authority transition;
- canonical dependencies;
- Git and governance-log state.

Execution:

1. revalidates all immutable evidence and live bindings;
2. appends and flushes one `action: promote` intent;
3. writes and flushes an owned sibling temporary file;
4. verifies its hash;
5. installs the destination using the proven same-volume no-replacement primitive;
6. verifies installed identity, size, and SHA-256;
7. appends exactly one successful terminal event;
8. leaves staged and operation evidence preserved;
9. allows a separately reviewed two-path local Git commit.

Existing destinations fail closed.

## Bounded amendment

Amendment changes one existing canonical Markdown note at the same destination.

The plan additionally preserves and binds:

- exact prior canonical bytes;
- original staged candidate bytes;
- normalized canonical candidate bytes;
- reviewed unified diff;
- prior, candidate, diff, validation, plan, and approval hashes;
- concise change summary;
- continuity fields.

The first amendment slice preserves:

```text
id
type
project
created
```

Review status and authority remain unchanged unless a separately accepted transition is explicitly supported and reviewed.

Execution:

1. revalidates immutable evidence and live state;
2. appends and flushes one `action: amend` intent;
3. creates and verifies one owned sibling temporary file;
4. opens and verifies the prior canonical destination through the proven handle-relative delete-sharing path;
5. performs native same-directory same-path replacement;
6. verifies installed identity, size, and SHA-256;
7. appends exactly one successful terminal event;
8. preserves all operation evidence;
9. allows a separately reviewed two-path local Git commit.

Amendment does not authorize delete, move, rename, generalized overwrite, supersedence, correction, rollback execution, or multi-note transactions.

## Completion reports and current-state return

Task-packet completion evidence returns through a governed amendment.

The completion report records:

- deliverables;
- tests and validation;
- deviations;
- failures and unresolved issues;
- discoveries;
- follow-up recommendations.

After the completion-report amendment is committed and the vault is clean, a separate current-state amendment may record the resulting project position.

Each amendment requires its own plan-generation and execution approvals.

## Event evidence

Events are appended to:

```text
_governance/promotion-log.jsonl
```

Each governed mutation has:

```text
intent -> committed
```

or, after deterministic recovery, one allowed recovered terminal record under the accepted event specification.

The file write and event append are separate operations and are never described as one atomic transaction.

## Recovery

Recovery uses immutable operation evidence to classify exact states.

For amendment, only exact approved prior bytes or exact approved candidate bytes are accepted canonical states. Unknown bytes fail closed.

For first-time promotion, destination absence, exact approved candidate presence, temporary evidence, and event state are reconciled under the recovery specification.

Recovery appends evidence; it never edits prior JSONL lines.

## Git boundary

Before mutation:

- platform and vault branches and HEADs match the plan;
- required trees are clean except for approved recovery-owned paths;
- the governance-log hash matches.

After mutation:

- verify exact changed paths;
- stage only the canonical destination and governance log;
- commit locally with a reviewed message;
- do not push or create a vault remote without separate approval.

## Connector and local operator boundary

Typed connector tools are preferred when accepted by the upstream platform.

Connector mutation is not guaranteed. After one upstream block:

1. verify no approval, event, or canonical mutation occurred;
2. do not repeatedly retry equivalent routes;
3. use the fixed-root local human operator;
4. inspect and verify resulting evidence afterward.

The local operator uses the same platform enforcement code and is not a bypass of Kaizen controls.

## Human actor limitation

The first slice accepts a trusted local actor string such as `owner.local`. This is not authentication. Agents may not select the owner actor. Multi-user or remote operation requires later identity design.

## Implemented acceptance evidence

- create-only staging and path confinement implemented and hammered;
- deterministic validation implemented;
- first-time promotion implemented and hammered;
- six ordered live promotions completed;
- bounded amendment implemented and hammered;
- task-packet and current-state return amendments completed;
- 230 platform tests passed at Milestone 4 closure;
- Results 023, 031, 040, 057, 059, 061, 062, and 065 passed.

## Deferred capabilities

- Hermes live write integration;
- production MCP;
- generalized edit/delete/move tools;
- supersedence, correction, and rollback execution;
- multi-note transactions;
- Postgres and Qdrant integration;
- remote or multi-user actor authentication.

## Related files

- `05-specs/promotion-event-schema.md`
- `05-specs/staging-write-wrapper-and-promotion-recovery.md`
- `05-specs/kaizen-validation-gate-spec.md`
- `05-specs/kaizen-hammer-test-strategy.md`
