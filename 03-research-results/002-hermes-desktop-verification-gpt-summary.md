# Research Summary 002 — Hermes Desktop Verification

Status: evidence summary
Source: GPT Deep Research report pasted by user
Date captured: 2026-06-03

## Purpose

Summarize the GPT Deep Research report on Hermes Desktop / Hermes Agent by Nous Research and extract Kaizen-relevant verified implications.

This file is evidence. It is not accepted doctrine by itself.

## Relationship to Research Summary 001

Research Summary 001 captured Claude's broad Hermes/agent-boundary analysis.

This summary captures GPT's Hermes-specific verification pass. It is more useful for confirming actual Hermes Desktop / Hermes Agent capabilities, tool behavior, skill behavior, and integration risks.

## High-confidence findings

1. Hermes Desktop is a native UI over the same Hermes Agent core used by CLI/gateway workflows, not a separate lightweight product.
2. Hermes is mixed local/cloud. Local app state can exist locally, but model providers, tool gateway features, hosted MCP, browser automation, and remote backend options can involve cloud services.
3. Hermes has enough tooling for Kaizen clerk work: file reading, search, patching, writing, sessions, memory, skills, terminal, browser/web actions, and MCP integration.
4. Hermes can likely be configured toward read-only use by disabling write-capable tools, but there is no single magic switch documented as a complete Kaizen-safe read-only mode.
5. Hermes skills are procedural instructions, not hard enforcement. They can tell Hermes what to do, but they are not an access-control system.
6. Hermes skills are mutable by the agent unless tool access such as `skill_manage` is restricted. Kaizen governance skills must be human-maintained.
7. Shell command approval does not equal file-write approval. File tools such as write/patch behavior need separate testing and external guardrails.
8. `patch` is safer than full-file write because it can emit diffs, but Kaizen should not assume mandatory diff-before-write enforcement exists natively.
9. The official Obsidian skill is not sufficient for Kaizen. It does not enforce duplicate detection, schema validation, strict read-before-write, or status governance.
10. Hermes is moving quickly. Kaizen should treat integration as a governed subsystem with repeatable tests, not as a one-time setup.

## Confirmed strategic posture

The GPT report strongly supports the posture already emerging from Claude's report:

```text
Hermes may search, draft, lint, and propose.
Hermes must not approve, promote, finalize, mutate doctrine, or touch source repos.
```

This confirms that the current Kaizen policy drafts are directionally correct:

- `07-hermes/hermes-permission-matrix.md`
- `07-hermes/hermes-write-access-preconditions.md`
- `04-design-decisions/0001-two-zone-agent-write-model.md`
- `04-design-decisions/0002-search-before-create-and-diff-before-write.md`
- `05-specs/kaizen-validation-gate-spec.md`

## Capability implications for Kaizen

### Hermes can support

- vault search and retrieval
- raw source draft creation
- source-summary draft creation
- task-packet draft creation from approved specs
- log append proposals
- index regeneration proposals
- validation/lint execution
- broken-link and missing-frontmatter reporting
- MCP-assisted research or file access, if scoped safely

### Hermes should not control

- accepted specs
- decisions
- source-of-truth rules
- pipeline status promotion
- implementation-ready labels
- deletes/moves of canonical content
- commits/pushes
- source-code repositories
- external publishing
- private/customer data workflows

## Controls recommended by the report

### 1. Dedicated Hermes profile

Kaizen should use its own Hermes profile so config, skills, sessions, memories, and state do not mix with general Hermes usage.

### 2. Minimal tool surface

Initial Kaizen profile should disable everything not required.

Candidate first safe surface:

- `read_file`
- `search_files`
- possibly `session_search`

Candidate disabled tools until proven safe:

- `write_file`
- `patch`
- `terminal`
- `execute_code`
- `skill_manage`
- code-repo integrations
- broad browser/web automation unless needed

### 3. Human-maintained Kaizen skill

The Kaizen governance skill should be short, version-controlled, and human-maintained.

Hermes should not be allowed to edit the governance skill as accepted policy.

### 4. Staging-only writes

If writes are enabled, they should first be enabled only in a disposable staging area or staging clone.

Canonical areas should require validation and promotion.

### 5. External validation

Kaizen should not depend on Hermes' built-in diagnostics for frontmatter/schema correctness.

Kaizen needs its own deterministic validation gate.

### 6. Audit ledger

Every Hermes write or proposed write should produce a human-readable audit entry recording:

- requested action
- target path
- tool used
- model/profile/session where available
- validation result
- promotion status

## Findings that update or sharpen existing docs

### Permission matrix

The existing permission matrix is aligned with this report, but should eventually add more explicit Hermes-specific controls:

- dedicated profile required
- `skill_manage` disabled for governance workflows
- `write_file` disabled on canonical content
- file writes allowed only in staging
- no remote backend exposure without explicit network safety review

### Write-access preconditions

The write-access preconditions should add:

- verify per-tool disabling in the exact Desktop/CLI surface used
- verify whether local file tools can write outside intended cwd
- verify Obsidian vault path resolution
- verify behavior with spaces in paths
- verify checkpoint/rollback behavior
- verify the difference between `patch` and `write_file` in a sandbox

### Validation gate spec

The validation spec remains necessary. Hermes does not provide Kaizen-specific frontmatter schema enforcement, duplicate detection, folder-placement validation, status-governance enforcement, or source-of-truth checks.

## Open questions requiring hands-on testing

1. Can the deployed Hermes Desktop/CLI profile preserve `read_file` and `search_files` while disabling `write_file` and `patch`?
2. Can local Hermes file tools write to arbitrary absolute paths reachable by the process, or are they confined by cwd/profile settings?
3. Does Hermes Desktop expose enough tool configuration for the Kaizen profile without dropping to CLI config?
4. Does Hermes post-write checking catch malformed YAML frontmatter relevant to Kaizen?
5. How does Hermes behave when `OBSIDIAN_VAULT_PATH` is missing, wrong, or contains spaces?
6. Is `patch` reliable enough for staged edits, and does it always produce reviewable diffs?
7. Can checkpoint/rollback be made mandatory enough for Kaizen's first write tests?
8. What is the safest staging design: staging folder, shadow vault, or staging repo?
9. How should Hermes model/provider routing be configured for cheap clerk work vs stronger synthesis?
10. Can remote Desktop/backend mode be avoided entirely for early Kaizen work?

## Proposed next docs/actions

1. Update `07-hermes/hermes-write-access-preconditions.md` with the Hermes-specific verification checklist.
2. Create `07-hermes/hermes-first-read-only-test.md`.
3. Create `07-hermes/hermes-first-staging-write-test.md`.
4. Create `07-hermes/kaizen-hermes-skill-draft.md` as a human-maintained proposed skill.
5. Add a future decision on dedicated Hermes profiles and minimal tool surfaces.

## Steward note

This report confirms the most important Kaizen safety rule: Hermes can be useful only when treated as a constrained clerk. It should be isolated by profile, limited by tool configuration, restricted to staging for writes, and checked by external validation before anything becomes canonical.
