---
id: kz-tp-01KTHAGE0J9GS1XW4N9EY2TJZG
type: task-packet
status: active
project: kaizen-vault-bootstrap
summary: Bootstrap the canonical Kaizen vault as a separate Git repository with one minimal validated project.
created: 2026-06-07T00:00:00Z
updated: 2026-06-07T00:00:00Z
review_status: approved
authority: accepted
primary_spec: 05-specs/kaizen-validation-gate-spec.md
related_specs:
  - 05-specs/kaizen-validation-gate-spec.md
  - 05-specs/kaizen-field-registry.md
  - 05-specs/kaizen-note-type-registry.md
  - 05-specs/kaizen-id-and-prefix-registry.md
---

# Task Packet 003 - Bootstrap Canonical Kaizen Vault

> Bootstrap status: owner-approved and accepted for execution. The staging and promotion system does not exist yet, so this packet authorizes a human-created canonical bootstrap only and does not claim that promotion has been exercised.

## Objective

Create `C:\dev\kaizen\vault` as a standalone Git repository and minimal Obsidian-compatible canonical Markdown vault containing one project bootstrap that validates under the implemented canonical validator.

## Read First

1. `C:\dev\kaizen-docs\04-design-decisions\0003-raw-markdown-is-canonical.md`
2. `C:\dev\kaizen-docs\04-design-decisions\0008-v0.2-operating-conventions.md`
3. `C:\dev\kaizen-docs\04-design-decisions\0012-first-slice-contract-and-implementation-boundary.md`
4. `C:\dev\kaizen-docs\IMPLEMENTATION_ROADMAP.md`
5. `C:\dev\kaizen-docs\03-research-results\019-deterministic-note-validation-steward-audit.md`
6. `C:\dev\kaizen-docs\05-specs\kaizen-field-registry.md`
7. `C:\dev\kaizen-docs\05-specs\kaizen-note-type-registry.md`
8. `C:\dev\kaizen-docs\05-specs\kaizen-validation-gate-spec.md`
9. `C:\dev\kaizen\platform\README.md`
10. `C:\dev\kaizen\platform\AGENTS.md`

## Context Pack

Available tools:

```text
C:\dev\kaizen\platform\.venv\Scripts\kaizen-id.exe
C:\dev\kaizen\platform\.venv\Scripts\kaizen-validate.exe
```

Task Packets 001 and 002 are complete and audited `pass-with-notes`.

No canonical vault, sibling staging root, or promotion system currently exists.

## Scope

1. Verify `C:\dev\kaizen` is not a Git repository.
2. Verify `C:\dev\kaizen\vault` does not contain unrelated preexisting content.
3. Create `C:\dev\kaizen\vault` and initialize it as a standalone Git repository on branch `main`.
4. Add a root `README.md` explaining vault ownership and source-of-truth boundaries.
5. Add a `.gitignore` that excludes volatile Obsidian workspace/cache state while allowing portable configuration to be added later through governance.
6. Create one project at:

```text
projects/kaizen-platform/
```

7. Create exactly these initial canonical notes:

```text
projects/kaizen-platform/command-center.md
projects/kaizen-platform/overview.md
projects/kaizen-platform/current-state.md
```

8. Generate real IDs through `kaizen-id`.
9. Validate all three notes through `kaizen-validate --mode canonical`.
10. Commit the bootstrap repository only after every validation passes.
11. Return a completion report and exact Git status.

## Non-Scope

Do not create:

- `C:\dev\kaizen\staging`;
- `C:\dev\kaizen\data`;
- `C:\dev\kaizen\scratch`;
- promotion logs or invented promotion events;
- source-summary, claim, decision, spec, audit, or canonical task-packet notes;
- Dataview, Templater, Bases, Canvas, community plugin, or custom plugin configuration;
- Postgres, Qdrant, Hermes, MCP, DataForSEO, marketplace, Tauri, bridge-plugin, or UI files;
- copied doctrine from `kaizen-docs`;
- a remote Git repository unless separately authorized by the owner.

Do not edit `kaizen-docs` or `kaizen\platform` during execution.

## Allowed Changes

Only under:

```text
C:\dev\kaizen\vault
```

Expected shape:

```text
vault/
  .gitignore
  README.md
  projects/
    kaizen-platform/
      command-center.md
      overview.md
      current-state.md
```

An empty `.obsidian/` folder is not required and should not be created merely for appearance.

## Do Not Touch

- `C:\dev\kaizen-docs`
- `C:\dev\kaizen\platform`
- `C:\dev\kaizen` Git state
- global Git or Python configuration
- sibling repositories
- remote Git hosting
- Obsidian global configuration

## Output Location

```text
C:\dev\kaizen\vault
```

## Implementation Requirements

### Vault README

Document:

- this repository is canonical project intelligence Markdown;
- doctrine and planning remain in `C:\dev\kaizen-docs` until governed migration or supersedence;
- implementation code remains in `C:\dev\kaizen\platform`;
- mutable operational data does not belong in this vault;
- agents do not have canonical write authority;
- future staged content must use the sibling staging root and human-controlled promotion workflow.

### Git ignore posture

Ignore at minimum:

```text
.obsidian/workspace.json
.obsidian/workspaces.json
.obsidian/cache/
.trash/
.DS_Store
Thumbs.db
```

Do not ignore all of `.obsidian/`; portable governed configuration may be committed later if earned.

### Bootstrap project content

Use project slug:

```text
kaizen-platform
```

The three notes must use valid generated IDs and satisfy the accepted note-type contracts.

`command-center.md` and `current-state.md` use:

```yaml
pipeline_stage: build
review_status: not-required
authority: none
```

`overview.md` uses:

```yaml
review_status: not-required
authority: none
```

Timestamps must be timezone-aware ISO-8601 values and internally consistent.

### Required note intent

`command-center.md`:

- orient a human or LLM to the project;
- link relatively to overview and current-state;
- state that code truth is in the platform repo;
- state that doctrine truth is in the docs repo;
- identify the next action as safe staging/promotion implementation planning.

`overview.md`:

- describe the Kaizen platform implementation project;
- state current scope and non-goals;
- avoid copying long doctrine;
- point to the external repository roles in plain text.

`current-state.md`:

- record that ID/parser and deterministic-validator foundations are complete;
- record 67 passing tests at commit `4e9a145`;
- identify staging/promotion as not yet implemented;
- identify the next move without presenting mutable job state as canonical truth.

## Constraints

- use only generated valid IDs;
- use portable relative Markdown links inside the project;
- no Wikilinks;
- no absolute local paths in Markdown links;
- plain text may name environment paths only as non-identity operational hints;
- no duplicate note IDs;
- no empty required H2 sections;
- no agent provenance fields on these human-authored bootstrap notes;
- no `validation_status` in canonical notes;
- no claim that the vault has passed promotion validation;
- no copied raw data or credentials;
- keep the repository minimal.

## Documentation Updates

Only vault-local `README.md` and the three project notes are authorized.

Any doctrine ambiguity is returned in the completion report rather than edited during execution.

## Validation

Run and report:

```powershell
C:\dev\kaizen\platform\.venv\Scripts\kaizen-validate.exe projects\kaizen-platform\command-center.md --mode canonical
C:\dev\kaizen\platform\.venv\Scripts\kaizen-validate.exe projects\kaizen-platform\overview.md --mode canonical
C:\dev\kaizen\platform\.venv\Scripts\kaizen-validate.exe projects\kaizen-platform\current-state.md --mode canonical
git diff --check
git status --short --branch
```

All validator commands must exit `0` with zero errors.

## Acceptance Criteria

1. `C:\dev\kaizen\vault` is a standalone Git repository on `main`.
2. `C:\dev\kaizen` remains a non-repository umbrella.
3. no staging, data, or scratch roots were created.
4. the vault shape contains only the authorized bootstrap files plus Git metadata.
5. README source-of-truth boundaries are explicit.
6. `.gitignore` preserves portable future Obsidian configuration while excluding volatile state.
7. all three notes use real generated IDs and no duplicate IDs.
8. all three notes pass canonical validation with zero errors.
9. all required H2 sections are nonempty.
10. internal note links are portable relative Markdown links.
11. no agent provenance or staging-only field appears.
12. no doctrine was copied into the vault as a competing authority source.
13. no promotion event or promotion claim was fabricated.
14. the first vault commit is created only after validation.
15. the final vault working tree is clean.
16. the completion report lists IDs, files, commands, validator results, commit, deviations, unresolved issues, and final Git status.

## Dependencies and Blockers

Satisfied:

- Task Packet 001 complete;
- Task Packet 002 complete;
- Milestone validation audit `pass-with-notes`;
- platform tools available and clean;
- vault root does not yet exist as a repository.

Stop when:

- `C:\dev\kaizen\vault` contains unrelated preexisting content;
- `C:\dev\kaizen` is unexpectedly a Git repository;
- a bootstrap note cannot validate without inventing doctrine;
- a remote repository or credentials would be required;
- execution would require staging or promotion behavior.

## Rollback and Recovery

Before the first commit, rollback is deletion of only the newly created authorized vault files after confirming no preexisting user content existed.

After commit, rollback is a normal Git revert or owner-authorized deletion of the new bootstrap repository. Do not touch sibling directories.

## Completion Report

```text
Status: complete
Repository: C:\dev\kaizen\vault
Branch: main
Commit: 3de6042 Bootstrap canonical Kaizen vault
Generated IDs:
- command-center: kz-cc-01KTHB33FVKF5M2QCXD3NNBVKE
- overview: kz-ov-01KTHB33KJ0920C7B0C1WVJMBJ
- current-state: kz-cs-01KTHB33QEDZSSGX56KVMWK8HM
Files created:
- .gitignore
- README.md
- projects/kaizen-platform/command-center.md
- projects/kaizen-platform/overview.md
- projects/kaizen-platform/current-state.md
Commands run:
- git init -b main
- kaizen-id command-center
- kaizen-id overview
- kaizen-id current-state
- kaizen-validate <each bootstrap note> --mode canonical
- git diff --check
- git status --short --branch
- git commit -m "Bootstrap canonical Kaizen vault"
Validation results: all three notes passed before and after commit with zero errors and zero warnings
Acceptance criteria result: all 16 criteria satisfied
Deviations: none
Unresolved issues: no remote repository is configured; staging and promotion remain unimplemented by design
Contract findings for kaizen-docs: canonical bootstrap notes validate without promotion claims; relative Markdown navigation works; current-state remains a durable snapshot rather than operational truth
Recommended next task: implement staging-root configuration and Windows path-confinement foundations without canonical promotion writes
Final git status: clean on main
```

The bootstrap was human-authorized. It does not claim that the future staging and promotion workflow was exercised.
