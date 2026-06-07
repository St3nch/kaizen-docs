---
id: kz-tp-01KTHB7DWY4BNXH7AGJTNZKJJH
type: task-packet
status: active
project: kaizen-platform
summary: Implement sibling staging-root configuration and Windows path-confinement foundations without canonical promotion writes.
created: 2026-06-07T00:00:00Z
updated: 2026-06-07T00:00:00Z
review_status: approved
authority: accepted
primary_spec: 05-specs/staging-write-wrapper-and-promotion-recovery.md
related_specs:
  - 05-specs/staging-write-wrapper-and-promotion-recovery.md
  - 05-specs/staging-and-promotion-workflow.md
  - 05-specs/kaizen-hammer-test-strategy.md
  - 05-specs/kaizen-validation-gate-spec.md
---

# Task Packet 004 - Implement Staging Root and Path Confinement Foundations

> Security status: owner-approved and accepted for execution. This packet does not authorize canonical writes, promotion events, agent write access, or a general filesystem API.

## Objective

Extend `C:\dev\kaizen\platform` with explicit canonical/staging root configuration and a Windows-focused path-confinement library plus CLI that can classify and verify proposed staging paths without writing note content.

Create the sibling staging root at `C:\dev\kaizen\staging` only after the implementation and preflight checks prove that `C:\dev\kaizen\vault` remains canonical and read-only to this milestone.

## Read First

1. `C:\dev\kaizen-docs\04-design-decisions\0001-two-zone-agent-write-model.md`
2. `C:\dev\kaizen-docs\04-design-decisions\0002-search-before-create-and-diff-before-write.md`
3. `C:\dev\kaizen-docs\04-design-decisions\0005-api-only-structured-data-access.md`
4. `C:\dev\kaizen-docs\04-design-decisions\0006-hammer-tests-are-a-hard-gate.md`
5. `C:\dev\kaizen-docs\05-specs\staging-write-wrapper-and-promotion-recovery.md`
6. `C:\dev\kaizen-docs\05-specs\staging-and-promotion-workflow.md`
7. `C:\dev\kaizen-docs\05-specs\kaizen-hammer-test-strategy.md`
8. `C:\dev\kaizen-docs\03-research-results\020-canonical-vault-bootstrap-steward-audit.md`
9. `C:\dev\kaizen\platform\AGENTS.md`
10. `C:\dev\kaizen\vault\README.md`

## Context Pack

Current repositories:

```text
C:\dev\kaizen-docs
C:\dev\kaizen\platform
C:\dev\kaizen\vault
```

The canonical vault is clean at commit `3de6042`. The platform is clean at commit `4e9a145` with sixty-seven passing tests.

The sibling staging root does not exist yet.

## Scope

Implement only:

1. a validated local root-configuration model for canonical and staging roots;
2. explicit defaults:
   - canonical: `C:\dev\kaizen\vault`
   - staging: `C:\dev\kaizen\staging`;
3. a `kaizen-path-check` CLI for dry-run staging-path evaluation;
4. logical-path syntax rejection before filesystem access;
5. Windows path classification and stable failure codes;
6. strict-descendant checks against the resolved staging root;
7. existing-parent traversal inspection;
8. reparse-point, symbolic-link, junction, mount-point, and ambiguous-final-path refusal;
9. target-extension and reserved-name checks;
10. test-only temporary directory trees for safe and hostile path cases;
11. creation of the real sibling staging root only after the path library and hammer tests pass;
12. a minimal staging-root `README.md` stating that content is non-canonical and disposable;
13. documentation and tests.

## Non-Scope

Do not implement:

- note-content writes into staging;
- `create_staged_markdown`;
- overwrite, edit, move, rename, delete, cleanup, or archive operations;
- canonical writes;
- promotion mode or promotion commands;
- promotion logs or promotion events;
- exact-content approval hashes;
- validation-status mutation;
- search-before-create evidence storage;
- Hermes or MCP access;
- database, provider, retrieval, Tauri, Obsidian plugin, or UI work;
- caller-selected roots;
- network paths;
- cross-volume promotion;
- general filesystem browsing.

Do not modify canonical vault content.

## Allowed Changes

Implementation changes are limited to:

```text
C:\dev\kaizen\platform
```

The only authorized new external directory is:

```text
C:\dev\kaizen\staging
```

Expected platform additions may include:

```text
src/kaizen/root_config.py
src/kaizen/path_confinement.py
src/kaizen/path_check_cli.py
tests/test_root_config.py
tests/test_path_confinement.py
tests/test_path_check_cli.py
```

Expected staging-root contents:

```text
C:\dev\kaizen\staging\README.md
```

The staging root is not a Git repository in this packet.

## Do Not Touch

- `C:\dev\kaizen-docs` during implementation;
- tracked files in `C:\dev\kaizen\vault`;
- vault Git history;
- global Git or Python configuration;
- sibling repositories;
- remote hosting;
- Obsidian configuration;
- ACLs or Windows security policy.

## Output Location

Code and tests:

```text
C:\dev\kaizen\platform
```

Non-canonical staging-root marker:

```text
C:\dev\kaizen\staging\README.md
```

## Implementation Requirements

### Root configuration

Provide an immutable configuration model exposing canonical and staging roots.

Rules:

- roots are supplied by trusted local configuration, not caller request data;
- both roots must be absolute local Windows paths;
- roots must be distinct siblings under `C:\dev\kaizen` for the accepted local profile;
- staging must not be nested in canonical and canonical must not be nested in staging;
- resolved roots must not be identical;
- UNC, device, and extended-length configured roots are rejected in this first profile;
- canonical root must already exist;
- staging root may be created only through an explicit human-run bootstrap command or function;
- configuration loading does not create directories as a side effect.

### Logical path rules

Accept only a relative logical path targeting a child Markdown file.

Reject with stable codes when the proposed logical path:

- is empty;
- is absolute;
- contains `..` as a segment;
- uses drive-relative syntax such as `C:folder`;
- is UNC;
- begins with `\\?\` or `\\.\`;
- contains a null byte;
- names the staging root itself;
- ends with a non-`.md` extension;
- contains a reserved Windows device name in any segment;
- contains an empty segment after normalization;
- exceeds the configured path-length limit;
- contains a trailing space or period in a segment;
- contains characters invalid for ordinary Windows filenames.

Mixed separators may be recognized for rejection, but prohibited input must not be silently rewritten into accepted input.

### Final-path confinement

The library must not claim race-resistant write safety unless it actually holds a verified handle-relative write primitive. This packet authorizes read-only inspection and dry-run confinement evidence only.

For existing parent segments:

1. resolve the configured staging root to a final local filesystem path;
2. require every inspected existing parent to remain a strict descendant of that root;
3. inspect each parent for reparse-point attributes;
4. reject symbolic links, junctions, mount points, or other reparse points;
5. reject failures or ambiguity in final-path resolution;
6. refuse any path whose nearest existing parent escapes staging;
7. return a structured result without creating the target file.

For nonexistent descendants:

- validate syntax and the entire existing parent chain;
- identify the nearest verified existing parent;
- report the unresolved descendant segments;
- do not create directories or files during a path check.

### CLI

Provide:

```text
kaizen-path-check <logical-path>
```

Optional JSON output:

```text
kaizen-path-check <logical-path> --json
```

Exit codes:

- `0` - path is acceptable for later staging-create evaluation;
- `1` - path is rejected by policy or confinement checks;
- `2` - configuration, root availability, usage, or internal failure.

Human output must include:

- accepted/rejected;
- logical path;
- configured staging root;
- nearest verified parent;
- stable failure code and message when rejected;
- no claim that content was written.

### Result model

Expose a structured result conceptually equivalent to:

```python
@dataclass(frozen=True)
class PathCheckResult:
    accepted: bool
    logical_path: str
    staging_root: Path
    nearest_verified_parent: Path | None
    unresolved_segments: tuple[str, ...]
    failure_code: str | None
    message: str
```

### Staging bootstrap

After all path and configuration tests pass:

- create `C:\dev\kaizen\staging` if it does not exist;
- refuse creation if unrelated content already exists;
- do not initialize Git;
- add only `README.md`;
- state that staging is non-canonical, disposable, and not an authority source;
- state that no agent write access is authorized yet;
- state that later writes must go through the approved create-only wrapper.

## Constraints

- Python 3.11.15;
- Windows 11 compatible;
- no runtime network access;
- no write to canonical vault;
- no staged note creation;
- no path-string prefix test presented as final confinement proof;
- no silent normalization of forbidden input;
- fail closed on ambiguous Windows filesystem behavior;
- use synthetic temporary directories for hostile-path tests;
- do not alter machine ACLs;
- preserve all existing platform tests.

## Documentation Updates

Update only platform-local documentation and the new staging `README.md`.

Document clearly that this packet proves path evaluation and root boundaries, not safe content writing or promotion.

## Validation

Run and report at minimum:

```powershell
.\.venv\Scripts\python.exe -m pytest
.\.venv\Scripts\python.exe -m compileall -q src tests
.\.venv\Scripts\python.exe -m pip check
.\.venv\Scripts\kaizen-path-check.exe projects\kaizen-platform\claims\example.md
.\.venv\Scripts\kaizen-path-check.exe ..\vault\escape.md
.\.venv\Scripts\kaizen-path-check.exe C:\dev\kaizen\vault\escape.md
.\.venv\Scripts\kaizen-path-check.exe \\server\share\escape.md
.\.venv\Scripts\kaizen-path-check.exe \\?\C:\escape.md
```

Expected CLI exits:

- safe relative Markdown path: `0`;
- traversal, absolute, UNC, device, and extended-length forms: `1`;
- unavailable or invalid trusted root configuration: `2`.

## Required Hammer Tests

At minimum:

- ordinary nested relative Markdown path;
- mixed-separator traversal;
- leading traversal;
- embedded traversal;
- absolute drive path;
- drive-relative path;
- UNC path;
- device path;
- extended-length path;
- null byte;
- reserved names including `CON`, `PRN`, `AUX`, `NUL`, `COM1`, and `LPT1`;
- trailing period and trailing space;
- invalid Windows filename characters;
- non-Markdown extension;
- path-length overflow;
- staging-root self-target;
- canonical-root overlap;
- staging nested under canonical;
- canonical nested under staging;
- existing symlink escape where supported;
- existing junction/reparse-point escape on Windows;
- nearest existing parent inside staging with nonexistent safe descendants;
- ambiguous final-path resolution;
- staging root containing unrelated preexisting content;
- proof that path checks create no files or directories.

Platform-specific tests may skip only when the operating system or privileges make the fixture impossible; every skip must be explicit and explained in the completion report.

## Acceptance Criteria

1. immutable root configuration exists and rejects overlapping or untrusted root forms.
2. `kaizen-path-check` exists with human and JSON output.
3. exit codes `0`, `1`, and `2` behave as specified.
4. logical-path rejection uses stable failure codes.
5. traversal, absolute, drive-relative, UNC, device, extended-length, null-byte, reserved-name, invalid-character, extension, and length tests pass.
6. existing parent chains are inspected beneath the resolved staging root.
7. reparse-point, symlink, junction, mount-point, and ambiguous-resolution cases fail closed.
8. no path check creates a file or directory.
9. no canonical vault file or Git state changes.
10. the real staging root contains only its authorized `README.md` and is not a Git repository.
11. no agent write access, staged-note write, or promotion behavior is introduced.
12. all existing platform tests remain green.
13. new tests, compilation, and dependency checks pass.
14. the platform implementation is committed with a clean tree.
15. the completion report lists stable failure codes, commands, hammer-test results, skips, deviations, and exact repository statuses.

## Dependencies and Blockers

Satisfied:

- canonical vault bootstrap completed and audited `pass`;
- platform validator completed and audited `pass-with-notes`;
- canonical vault is clean at `3de6042`;
- staging root is absent.

Stop and report when:

- staging contains unrelated preexisting content;
- the canonical vault is dirty before implementation begins;
- Windows final-path or reparse inspection cannot be implemented reliably in Python without a narrower native helper;
- tests would require elevated ACL or security-policy changes;
- the implementation would need to create staged note content;
- any canonical write becomes necessary.

Do not downgrade the path boundary to string-prefix checking to avoid a blocker.

## Rollback and Recovery

Platform code rolls back through a normal Git revert.

If this packet creates `C:\dev\kaizen\staging`, rollback may remove only the packet-created `README.md` and empty staging directory after confirming no user or agent content exists.

Never delete or modify canonical vault content during rollback.

## Completion Report

```text
Status: complete
Platform commit: b39b3a6
Vault: clean at 3de6042
Staging: README.md only; not a Git repository
Tests: 105 passed, 2 skipped
Skipped: two directory-symlink fixtures; current account lacks symlink privilege
Junction/reparse test: passed
CLI exits: safe 0; traversal/absolute/UNC/device 1; config failure 2 covered by tests
Acceptance criteria: all satisfied
Deviations: symlink fixtures unavailable; equivalent junction refusal verified
Unresolved: no race-resistant content-write primitive yet; path checks remain dry-run only
Next: create-only staging-write wrapper prototype; canonical promotion remains out of scope
Final status: platform and vault clean
```

This milestone proves root boundaries and dry-run confinement only. It does not authorize staged-note writes or canonical promotion.