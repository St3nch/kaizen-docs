---
id: kz-tp-01KTH7850B03Z3BW3C6Q29RDZQ
type: task-packet
status: active
project: kaizen-platform
summary: Bootstrap the Kaizen platform repository and implement the Python ID-generation and Markdown/frontmatter parser foundations for Milestone 1.
created: 2026-06-07T00:00:00Z
updated: 2026-06-07T00:00:00Z
review_status: approved
authority: accepted
primary_spec: 05-specs/kaizen-id-and-prefix-registry.md
related_specs:
  - 05-specs/kaizen-id-and-prefix-registry.md
  - 05-specs/kaizen-validation-gate-spec.md
  - 05-specs/kaizen-field-registry.md
  - 05-specs/kaizen-note-type-registry.md
---

# Task Packet 001 - Bootstrap Kaizen Platform and Implement ID/Parser Foundations

> Bootstrap status: owner-approved and accepted for execution. The canonical vault and promotion system do not exist yet, so this packet does not claim a canonical promotion event.

## Objective

Initialize the Kaizen implementation repository at `C:\dev\kaizen\platform` and deliver the first reusable Python 3.11.15 tooling foundation:

- machine-readable note and event prefix registries;
- valid Kaizen prefixed-ULID generation;
- UTF-8 Markdown parsing;
- flat YAML frontmatter parsing;
- duplicate-key detection;
- nested-object rejection;
- a small CLI foundation;
- deterministic tests and fixtures.

This packet implements Milestone 1 foundations only. It does not create the canonical vault, staging root, promotion workflow, database services, retrieval services, provider integrations, or user interface.

## Read First

Read these files in order before changing implementation files:

1. `C:\dev\kaizen-docs\04-design-decisions\0012-first-slice-contract-and-implementation-boundary.md`
2. `C:\dev\kaizen-docs\IMPLEMENTATION_ROADMAP.md`
3. `C:\dev\kaizen-docs\05-specs\kaizen-id-and-prefix-registry.md`
4. `C:\dev\kaizen-docs\05-specs\kaizen-field-registry.md`
5. `C:\dev\kaizen-docs\05-specs\kaizen-note-type-registry.md`
6. `C:\dev\kaizen-docs\05-specs\kaizen-validation-gate-spec.md`
7. `C:\dev\kaizen-docs\04-design-decisions\0003-raw-markdown-is-canonical.md`
8. `C:\dev\kaizen-docs\04-design-decisions\0006-hammer-tests-are-a-hard-gate.md`
9. `C:\dev\kaizen-docs\04-design-decisions\0008-v0.2-operating-conventions.md`

The Markdown documents above are governing inputs. The implementation repository must not silently redefine their contracts.

## Context Pack

Current repository boundaries:

```text
C:\dev\kaizen-docs
= planning and doctrine repository

C:\dev\kaizen
= non-repository umbrella folder

C:\dev\kaizen\platform
= implementation repository authorized by Decision 0012
```

`C:\dev\kaizen\platform` currently exists as an empty directory and is not yet a Git repository.

The first-slice implementation language is Python 3.11.15.

This packet is stored in the planning repository because the canonical vault and promotion system do not yet exist. It is a bootstrap handoff, not a claim that the future canonical task-packet promotion flow has already been exercised.

## Scope

Implement only the following:

1. Initialize `C:\dev\kaizen\platform` as a standalone Git repository.
2. Create a Python 3.11.15 project using `pyproject.toml`.
3. Create the package, CLI, schema, fixture, and test foundations.
4. Add machine-readable note-type and event-prefix registries matching the accepted Markdown registry.
5. Implement `kaizen-id <registered-type-or-event>`.
6. Implement a Markdown/frontmatter parser that:
   - requires UTF-8 input;
   - identifies a leading YAML frontmatter block;
   - rejects duplicate YAML keys;
   - rejects nested YAML mappings/objects;
   - allows scalar values and flat lists required by the accepted contracts;
   - returns parsed frontmatter and Markdown body separately;
   - produces stable machine-readable errors.
7. Add unit tests and fixtures for the required behavior.
8. Document setup, test, and CLI commands.
9. Add an `AGENTS.md` file that defines repository-local coding-agent constraints for this milestone.

## Non-Scope

Do not implement or create:

- `C:\dev\kaizen\vault`;
- `C:\dev\kaizen\staging`;
- `C:\dev\kaizen\data`;
- `C:\dev\kaizen\scratch` unless a disposable test directory is created inside the platform repository and removed or ignored appropriately;
- canonical-note validation beyond the parser and identity primitives explicitly required here;
- staging, canonical, or promotion modes;
- canonical file writes;
- promotion events or recovery;
- Hermes or MCP configuration;
- Postgres;
- Qdrant;
- DataForSEO or another provider;
- marketplace-ranking logic;
- Tauri, web UI, Obsidian plugin, or desktop-shell code;
- packaging for public distribution;
- production deployment;
- migrations from existing project repositories.

Do not add implementation code to `C:\dev\kaizen-docs`.

## Allowed Changes

The implementation may create and modify files only under:

```text
C:\dev\kaizen\platform
```

Expected initial files and directories:

```text
C:\dev\kaizen\platform\
  .gitignore
  AGENTS.md
  README.md
  pyproject.toml
  schemas\
    note-type-prefixes.json
    event-type-prefixes.json
  src\
    kaizen\
      __init__.py
      cli.py
      ids.py
      markdown.py
      registry.py
  tests\
    fixtures\
    test_ids.py
    test_markdown.py
    test_registry.py
```

The coding agent may refine package-internal filenames when the reason is documented in the completion report, but it may not change the repository boundary, language, governing command names, or milestone scope.

## Do Not Touch

Do not modify:

- `C:\dev\kaizen-docs`;
- sibling repositories under `C:\dev`;
- global Python configuration;
- system environment variables;
- Obsidian configuration;
- Git configuration outside `C:\dev\kaizen\platform`;
- network services;
- paid APIs;
- future vault, staging, or data roots.

Do not initialize Git at `C:\dev\kaizen`.

## Output Location

All implementation output belongs in:

```text
C:\dev\kaizen\platform
```

The final implementation report must be returned in the coding-agent response and must not be written into `kaizen-docs` by the coding agent.

## Implementation Requirements

### Python project

- Target Python 3.11.15.
- Use a `src` package layout.
- Provide console-script entry points through `pyproject.toml`.
- Use type hints on public functions and core internal boundaries.
- Use `pytest` for tests.
- Use an isolated virtual environment at `.venv` inside the platform repository.
- Use a YAML library or parser strategy that can deterministically detect duplicate keys; plain permissive loading is not acceptable.
- Keep runtime dependencies minimal and justify each dependency in `README.md` and the completion report.
- Pin or constrain development dependencies sufficiently for reproducible local setup.
- Do not require network access at runtime.

Document PowerShell setup commands equivalent to:

```powershell
uv venv --python 3.11 .venv
.\.venv\Scripts\python.exe -m pip install --upgrade pip
.\.venv\Scripts\python.exe -m pip install -e ".[dev]"
.\.venv\Scripts\python.exe -m pytest
```

The coding agent may adapt the install command to the final `pyproject.toml`, but the resulting README must provide one verified Windows PowerShell path from a clean clone to passing tests.

### Machine-readable registries

Create separate JSON files for note types and operational event types.

The note registry must contain exactly:

```json
{
  "command-center": "cc",
  "overview": "ov",
  "current-state": "cs",
  "source-summary": "ss",
  "claim": "clm",
  "decision": "dec",
  "spec": "spec",
  "audit": "aud",
  "task-packet": "tp"
}
```

The event registry must contain exactly:

```json
{
  "promotion-event": "prom",
  "validation-run": "val",
  "escalation-event": "esc"
}
```

The implementation must load these registries rather than duplicating the mappings across command code.

### Kaizen ID generation

Canonical output format:

```text
kz-<registered-prefix>-<26-character-ULID>
```

Requirements:

- output uses uppercase Crockford Base32;
- output contains exactly one generated ID plus one trailing newline in normal mode;
- ULID contains 26 valid characters;
- ambiguous Crockford characters `I`, `L`, `O`, and `U` never appear;
- unknown types/events fail with a nonzero exit code;
- the command never derives identity from titles or filenames;
- the command never contacts an external service;
- note types and event types share one command interface but remain separate registries internally;
- generation is safe under rapid repeated calls within one process;
- normal command output contains no decorative prose.

Required command examples:

```text
kaizen-id claim
kaizen-id spec
kaizen-id promotion-event
```

A machine-readable JSON output mode is not required in this packet.

### Markdown/frontmatter parser

The parser must:

- accept a file path or text input through a testable library boundary;
- require a leading `---` frontmatter block for durable-note parsing;
- require a closing `---` delimiter;
- decode UTF-8 strictly;
- reject duplicate keys;
- reject nested mappings/objects;
- reject lists containing nested mappings/objects;
- allow flat scalars and flat scalar lists;
- preserve the Markdown body without rendering it;
- report line-aware errors where practical;
- expose stable error categories suitable for later CLI and CI use;
- avoid Obsidian-specific parsing dependencies.

The parser does not need to enforce all note-type, relationship, lifecycle, authority, folder, or promotion rules in this packet.

### CLI foundation

Provide:

```text
kaizen-id <registered-type-or-event>
```

A parser-facing CLI command may be included for diagnostics, but it is not required for acceptance. The parser must be exposed as importable Python code.

### Repository-local agent guidance

`AGENTS.md` must state at minimum:

- this repository contains implementation code;
- `kaizen-docs` is read-only doctrine during implementation tasks unless a separate task authorizes documentation changes;
- deferred systems must not be introduced silently;
- tests must run before completion;
- completion reports must disclose deviations and unresolved issues;
- secrets and paid API calls are forbidden in this milestone.

## Interfaces and Data

Public library boundaries should make later validator work possible without premature framework choices.

Recommended conceptual interfaces:

```python
def generate_kaizen_id(kind: str) -> str:
    ...


def load_prefix_registries() -> PrefixRegistries:
    ...


def parse_markdown_note(text: str) -> ParsedMarkdownNote:
    ...


def parse_markdown_file(path: Path) -> ParsedMarkdownNote:
    ...
```

Names may differ, but equivalent tested library boundaries must exist.

Errors should use explicit exception classes or structured result types rather than requiring callers to parse human prose.

## Constraints

- The implementation must work on Windows 11.
- Paths in code and tests must use `pathlib` or equivalent platform-safe handling.
- Do not hardcode `C:\dev\kaizen\platform` inside reusable library logic.
- Do not use string-prefix checks as a security boundary.
- Do not implement canonical writes in this packet.
- Do not add a database.
- Do not add a web server.
- Do not add telemetry.
- Do not add authentication.
- Do not add asynchronous background services.
- Do not add plugin dependencies.
- Do not embed generated IDs as fixtures unless they are structurally valid and clearly test data.
- The implementation must remain understandable and maintainable by one owner with AI coding assistance.

## Documentation Updates

Create or update only implementation-repository documentation:

- `README.md` with setup, test, and CLI usage;
- `AGENTS.md` with repository-local constraints;
- concise docstrings where public library behavior is not obvious.

Do not edit planning or doctrine documents as part of implementation.

Any discovered contract issue must be listed in the completion report for steward review.

## Validation

The coding agent must run and report the exact commands used.

Minimum expected validation:

```text
python --version
python -m pytest
```

Also run direct CLI checks equivalent to:

```text
kaizen-id claim
kaizen-id spec
kaizen-id promotion-event
```

And one invalid-input check equivalent to:

```text
kaizen-id unknown-type
```

The invalid command must return a nonzero exit status.

Tests must cover:

- all nine note-type mappings;
- all three event-type mappings;
- valid ID shape;
- prohibited ULID characters;
- repeated generation uniqueness;
- unknown-kind failure;
- valid UTF-8 Markdown/frontmatter parsing;
- missing opening delimiter;
- missing closing delimiter;
- duplicate YAML key;
- nested mapping;
- nested mapping inside a list;
- flat scalar list acceptance;
- invalid UTF-8 file input;
- body preservation.

## Acceptance Criteria

The packet is complete only when all of the following are true:

1. `C:\dev\kaizen\platform` is a standalone Git repository and `C:\dev\kaizen` is not a Git repository.
2. Python 3.11.15 project metadata is present and usable.
3. A documented local setup creates an isolated development environment.
4. Machine-readable note and event prefix registries exactly match the accepted registry.
5. `kaizen-id` generates valid prefixed ULIDs for all registered note and event kinds.
6. Unknown kinds fail with a nonzero exit code and a useful stderr message.
7. The parser accepts valid UTF-8 Markdown with flat YAML frontmatter.
8. The parser rejects duplicate keys and nested mappings deterministically.
9. The parser preserves Markdown body text.
10. Tests cover every requirement listed under `## Validation` and pass locally.
11. No vault, staging, database, provider, Hermes, MCP, Qdrant, Postgres, or UI scope was added.
12. No file in `C:\dev\kaizen-docs` was changed by the coding agent.
13. The repository contains no credentials, tokens, or private data.
14. The completion report lists changed files, commands run, test results, deviations, unresolved questions, and recommended next work.
15. The final Git working tree in `C:\dev\kaizen\platform` is clean after the implementation commit.

## Dependencies and Blockers

Environment preflight on 2026-06-07:

- Python 3.14.2 is installed system-wide.
- Python 3.11.15 is installed and managed by `uv 0.11.18`.
- The first slice will use the existing `uv`-managed Python 3.11.15 runtime.
- `C:\dev\kaizen` is not a Git repository.
- `C:\dev\kaizen\platform` exists, is empty, and is not yet a Git repository.

Dependencies already satisfied:

- Decision 0008 is accepted.
- Decision 0012 is accepted.
- `C:\dev\kaizen\platform` exists and is empty.
- Milestone 1 is defined in `IMPLEMENTATION_ROADMAP.md`.

Blockers that must stop implementation:

- the approved `uv`-managed Python 3.11.15 runtime is unavailable or fails environment creation;
- `C:\dev\kaizen` is unexpectedly a Git repository;
- `C:\dev\kaizen\platform` contains unreviewed files before initialization;
- a governing registry contradicts another accepted contract;
- implementation requires changing a deferred architecture boundary;
- tests reveal that a required contract cannot be implemented without steward clarification.

When blocked, stop and report the exact issue. Do not invent doctrine or broaden scope.

## Rollback and Recovery

This packet creates only a new implementation repository and local source files.

Before the first commit, rollback is deletion of the newly created implementation files inside `C:\dev\kaizen\platform` after verifying no unrelated preexisting file exists.

After the first commit, rollback is a normal Git revert or branch reset authorized by the owner. Do not delete `C:\dev\kaizen` or touch sibling directories.

No canonical vault or staged project data exists in this milestone.

## Completion Report

```text
Status: complete
Repository: C:\dev\kaizen\platform
Branch: main
Commit: 31d59db Bootstrap Kaizen platform ID and parser foundations
Follow-up documentation commit: setup-path correction committed in platform repository
Files created: 21 implementation files across project metadata, registries, package modules, fixtures, and tests
Files modified after first commit: README.md setup instructions only
Commands run:
- git init -b main
- uv venv --python 3.11 .venv
- .\.venv\Scripts\python.exe -m ensurepip --upgrade
- .\.venv\Scripts\python.exe -m pip install pytest PyYAML
- .\.venv\Scripts\python.exe -m pip install -e .
- .\.venv\Scripts\python.exe -m pip install "pytest>=8.3,<9"
- .\.venv\Scripts\python.exe -m pip install -e ".[dev]"
- .\.venv\Scripts\python.exe -m pytest
- .\.venv\Scripts\python.exe -m compileall -q src tests
- .\.venv\Scripts\python.exe -m pip check
- .\.venv\Scripts\kaizen-id.exe claim
- .\.venv\Scripts\kaizen-id.exe spec
- .\.venv\Scripts\kaizen-id.exe promotion-event
- .\.venv\Scripts\kaizen-id.exe unknown-type
Tests run: 21
Test results: 21 passed; compileall passed; pip check reported no broken requirements
CLI checks: valid claim/spec/promotion-event IDs generated; unknown kind returned exit code 2 with stable error code
Acceptance criteria result: all 15 criteria satisfied
Deviations: Python 3.12 was unavailable; the accepted baseline was amended before implementation to the installed uv-managed Python 3.11.15 runtime
Unresolved issues: no functional blocker; future packaging should decide whether to adopt a lockfile and a single preferred bootstrap command
Contract findings for kaizen-docs: parser and registries are implementable as written; setup documentation should distinguish environment creation from dependency installation; validator work can now build on stable library interfaces
Recommended next task: implement deterministic note validation without path-confinement, promotion, canonical writes, or cross-root resolution
Final git status: clean after implementation commit and post-commit verification
```

The completion report is implementation evidence. It does not alter accepted doctrine by itself.