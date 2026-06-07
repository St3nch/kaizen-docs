---
id: kz-tp-01KTH99S9FFXVRYY3ADCSVEZ39
type: task-packet
status: active
project: kaizen-platform
summary: Implement deterministic single-note validation using the accepted Kaizen field, note-type, identity, and validation contracts.
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

# Task Packet 002 - Implement Deterministic Note Validation

> Bootstrap status: owner-approved and accepted for execution. The canonical vault, sibling staging root, and promotion system do not exist yet, so this packet authorizes static single-note validation only.

## Objective

Extend `C:\dev\kaizen\platform` with a deterministic `kaizen-validate` CLI and reusable validation library for one Markdown note at a time.

The validator must build on the committed Milestone 1 parser and machine-readable registries. It must validate note structure and intrinsic metadata rules without implementing filesystem confinement, canonical writes, promotion events, cross-vault reference resolution, or authority transitions that require human-event evidence.

## Read First

Read these files in order before changing implementation files:

1. `C:\dev\kaizen-docs\04-design-decisions\0012-first-slice-contract-and-implementation-boundary.md`
2. `C:\dev\kaizen-docs\IMPLEMENTATION_ROADMAP.md`
3. `C:\dev\kaizen-docs\03-research-results\018-milestone-1-tools-foundation-steward-audit.md`
4. `C:\dev\kaizen-docs\05-specs\kaizen-validation-gate-spec.md`
5. `C:\dev\kaizen-docs\05-specs\kaizen-field-registry.md`
6. `C:\dev\kaizen-docs\05-specs\kaizen-note-type-registry.md`
7. `C:\dev\kaizen-docs\05-specs\kaizen-id-and-prefix-registry.md`
8. `C:\dev\kaizen-docs\04-design-decisions\0008-v0.2-operating-conventions.md`
9. `C:\dev\kaizen-docs\04-design-decisions\0006-hammer-tests-are-a-hard-gate.md`
10. `C:\dev\kaizen\platform\AGENTS.md`

The Markdown contracts remain the human-readable doctrine. Machine-readable validator contracts must be reviewed against them and must not silently redefine them.

## Context Pack

Milestone 1 is complete and audited `pass-with-notes`.

Available implementation foundations:

- Python 3.11.15 project at `C:\dev\kaizen\platform`;
- strict UTF-8 Markdown/frontmatter parser;
- duplicate-key and nested-YAML rejection;
- machine-readable note and event prefix registries;
- stable Kaizen exception codes;
- `kaizen-id` CLI;
- twenty-one passing tests;
- clean implementation repository.

This packet is the second tools-foundation packet. It does not authorize Milestone 2 vault creation or Milestone 3 staging/promotion work.

## Scope

Implement only:

1. machine-readable field, enum, note-type, and required-section contracts under `schemas/`;
2. reusable validation result, issue, and normalized-metadata models;
3. deterministic single-note validation for `staging` and `canonical` content modes;
4. a `kaizen-validate <path> --mode staging|canonical` CLI;
5. optional `--json` output for stable CI/agent consumption;
6. human-readable validation output;
7. stable error and warning codes;
8. fixtures and tests covering every rule authorized below;
9. README and AGENTS updates limited to validator usage and scope.

## Non-Scope

Do not implement:

- `promotion` mode;
- canonical or staging root configuration;
- final-path confinement;
- traversal, UNC, device-path, symlink, junction, mount-point, or reparse-point checks;
- canonical writes, moves, deletes, renames, or directory creation;
- promotion event creation or validation;
- approval-event lookup;
- content-hash approval freshness;
- immutable-ID or immutable-`created` comparison against Git history;
- global ID uniqueness scanning across a vault;
- cross-note stable-ID resolution;
- body-link target resolution;
- reciprocal supersedence validation;
- duplicate title, summary, filename, or concept scanning across a corpus;
- raw-data threshold calibration beyond the bounded warnings defined here;
- reviewed false-positive override events;
- Hermes, MCP, Postgres, Qdrant, DataForSEO, marketplace, Tauri, Obsidian plugin, or UI work;
- creation of `C:\dev\kaizen\vault`, `staging`, `data`, or `scratch`.

Do not modify `C:\dev\kaizen-docs` during implementation.

## Allowed Changes

Changes are limited to `C:\dev\kaizen\platform`.

Expected additions or modifications:

```text
pyproject.toml
README.md
AGENTS.md
schemas/
  fields.json
  enums.json
  note-types.json
src/kaizen/
  __init__.py
  cli.py or a dedicated validate_cli.py
  validation.py
  contracts.py or equivalent
  errors.py when new stable error types are needed
tests/
  fixtures/validation/
  test_validation.py
  test_validation_cli.py
  existing tests when required for compatible integration
```

Internal filenames may differ when documented in the completion report. Repository ownership, command names, validation modes, and scope may not change.

## Do Not Touch

Do not modify:

- `C:\dev\kaizen-docs`;
- `C:\dev\kaizen` as a Git root;
- sibling repositories;
- global Python or Git configuration;
- Obsidian configuration;
- paid or network services;
- future vault, staging, data, or scratch roots.

## Output Location

All implementation output belongs in:

```text
C:\dev\kaizen\platform
```

The completion report is returned to the steward. The coding agent does not write it into `kaizen-docs`.

## Implementation Requirements

### Validation command

Provide:

```text
kaizen-validate <path> --mode staging|canonical
```

Optional stable JSON output:

```text
kaizen-validate <path> --mode canonical --json
```

Exit behavior:

- `0` when no errors exist, including warning-only results;
- `1` when validation errors exist;
- `2` for command usage, unreadable input path, invalid mode, registry/configuration failure, or internal execution failure.

Human-readable normal output must identify:

- pass/fail;
- validated path;
- mode;
- note ID and type when parseable;
- each error and warning code with a message;
- error/warning totals.

JSON output must contain stable keys and must not include decorative prose.

### Validation result model

Provide tested library boundaries conceptually equivalent to:

```python
class ValidationSeverity(str, Enum):
    ERROR = "error"
    WARNING = "warning"

@dataclass(frozen=True)
class ValidationIssue:
    code: str
    severity: ValidationSeverity
    message: str
    field: str | None = None
    section: str | None = None
    line: int | None = None

@dataclass(frozen=True)
class ValidationResult:
    path: Path | None
    mode: str
    passed: bool
    issues: tuple[ValidationIssue, ...]
    normalized_metadata: Mapping[str, object]
```

Names may differ, but callers must not parse free-form console text to consume validation results.

### Machine-readable contracts

Create reviewed machine-readable contracts for:

- the seven universal fields;
- allowed known fields;
- enum values;
- note-type required fields;
- note-type allowed authority values;
- note-type required H2 sections;
- conditional relationship rules needed in this packet;
- the accepted six-value `pipeline_stage` enum.

The validator must load these contracts rather than duplicating them across validation branches.

### Universal fields

Every durable note requires non-empty:

```text
id
type
status
project
summary
created
updated
```

Rules:

- `project` is lowercase kebab-case;
- `summary` is non-empty plain scalar text;
- summary length over 200 characters is warning `summary_too_long`, not an error in this packet;
- unknown fields produce warning `unknown_field`;
- nested YAML remains a parser error from Milestone 1.

### ID and type validation

- `type` must be one of the nine registered note types;
- `id` must match `kz-<registered-prefix>-<26-character-ULID>`;
- the ID prefix must match the note type;
- ULID characters must be uppercase valid Crockford Base32;
- malformed ID and type-prefix mismatch use distinct stable error codes;
- global uniqueness and historical reuse checks remain deferred.

### Enum validation

Enforce:

```text
status: draft | active | blocked | complete | archived
review_status: not-required | pending | approved | rejected
authority: none | proposed | accepted
confidence: low | medium | high
validation_status: pending | passed | failed
pipeline_stage: capture | research | spec | audit | handoff | build
```

Unknown enum values are errors.

### Date validation

- `created` and `updated` must parse as ISO-8601 date-times with explicit timezone offsets;
- `updated` must be greater than or equal to `created`;
- timestamps more than five minutes in the future relative to validator time are errors;
- immutability across revisions remains deferred.

### Type-specific fields

Enforce the accepted note-type registry, including:

- `command-center`: `pipeline_stage`, `review_status: not-required`, authority absent or `none`;
- `overview`: `review_status: not-required`, authority absent or `none` unless a later governed rule says otherwise;
- `current-state`: `pipeline_stage`, `review_status: not-required`, authority absent or `none`;
- `source-summary`: `review_status`, `authority: none`, and at least one non-empty `source_docs` or `source_urls` list;
- `claim`: `review_status`, `authority`, `confidence`, and non-empty `source_docs`;
- `decision`: `review_status` and `authority: accepted|accepted`;
- `spec`: `review_status`, `authority: accepted|accepted`, and non-empty `related_decisions`;
- `audit`: `review_status`, authority absent or `none`, and at least one non-empty governed target list among `related_claims`, `related_decisions`, or `related_specs`;
- `task-packet`: `review_status`, `authority: accepted|accepted`, non-empty `related_specs`, and exactly one non-empty `primary_spec` that also appears in `related_specs`.

### Required sections

- enforce exact registered H2 headings;
- headings must be Markdown ATX H2 headings beginning with exactly `## `;
- heading order is not enforced in this packet;
- every required section must contain non-whitespace content before the next H2 or end of file;
- conditional `## Supersedence rationale` is required when `supersedes` or `superseded_by` is present;
- task-packet `## Completion Report` may contain an explicit pre-implementation placeholder such as `Pending implementation.` while `status` is `draft` or `active`; it must not be structurally empty.

### Relationship and source syntax

For known stable-ID relationship fields:

- values must be a stable Kaizen ID or flat list of stable Kaizen IDs as registered;
- no filenames, paths, or Wikilinks are allowed in relationship frontmatter;
- reference resolution remains deferred.

For `source_urls`:

- require absolute `https://` or `http://` URLs;
- reject credentials embedded in URLs;
- liveness checks remain deferred.

For body links:

- canonical mode: Wikilinks are error `wikilink_forbidden`;
- staging mode: Wikilinks are warning `wikilink_requires_normalization`;
- canonical Markdown links with absolute local filesystem targets are errors;
- link target existence remains deferred.

### Agent provenance and staging fields

- human-authored notes omit `agent`, `model`, and `session`;
- if any agent-provenance field is present, all three must be present and non-empty;
- canonical mode rejects `validation_status`;
- staging mode allows `validation_status`;
- when staging content has agent provenance, `validation_status` is required;
- this packet does not infer whether a note was human- or agent-authored when all provenance fields are absent.

### Intrinsic lifecycle and authority combinations

Enforce combinations that require no external event lookup:

- `status: draft` with `review_status: approved` is error;
- `authority: accepted` requires `review_status: approved`;
- `authority: accepted` is error on note types that cannot carry authority;
- `audit_verdict` is allowed only on `audit`;
- `audit_verdict` values use the accepted enum;
- `audit_verdict: pass-with-notes` event/finding materiality checks remain human-review only;
- proof of human approval and promotion events remains deferred;
- task-packet governing-spec acceptance lookup remains deferred.

### Bounded content-safety checks

Errors:

- PEM/OpenSSH private-key block markers;
- obvious bearer-token or API-key assignments with non-placeholder values;
- embedded base64-like payload blocks longer than 2,000 characters when labeled as raw payload or binary content.

Warnings:

- file larger than 100 KB;
- Markdown table with more than 100 data rows;
- fenced JSON/code block longer than 200 lines;
- document longer than 1,000 lines;
- repeated email- or phone-like matches above a conservative threshold.

Tests must use synthetic values only. Do not place real credentials or customer data in fixtures.

### Determinism

For identical content, mode, validator version, and clock input:

- issue codes, severities, fields/sections, and order are stable;
- tests inject or freeze validator time rather than depending on wall-clock timing;
- warnings do not change exit status from `0`;
- errors produce exit status `1`.

## Interfaces and Data

Machine-readable schema files remain implementation artifacts reviewed against Markdown doctrine.

The validator should expose a library entry point conceptually equivalent to:

```python
def validate_note(
    note: ParsedMarkdownNote,
    *,
    mode: Literal["staging", "canonical"],
    path: Path | None = None,
    now: datetime | None = None,
) -> ValidationResult:
    ...
```

The public API may parse a path through a convenience wrapper, but parsing and validation remain separately testable.

## Constraints

- Python 3.11.15;
- Windows 11 compatible;
- use existing parser and registry modules rather than replacing them;
- no network access at runtime or in tests;
- no canonical writes;
- no root-confinement claims;
- no database;
- no background service;
- no hidden plugin dependency;
- no silent mutation or normalization of the validated note;
- no broad refactor unrelated to validation;
- stable error/warning codes require tests.

## Documentation Updates

Update only platform-repository documentation:

- `README.md` with `kaizen-validate` examples and exit codes;
- `AGENTS.md` with the new validator scope and explicit deferred boundaries;
- concise docstrings or generated schema notes as needed.

Any doctrine ambiguity is reported in the completion report for steward action.

## Validation

Run and report at minimum:

```powershell
.\.venv\Scripts\python.exe -m pytest
.\.venv\Scripts\python.exe -m compileall -q src tests
.\.venv\Scripts\python.exe -m pip check
.\.venv\Scripts\kaizen-validate.exe tests\fixtures\validation\valid-claim.md --mode canonical
.\.venv\Scripts\kaizen-validate.exe tests\fixtures\validation\valid-staged-agent-claim.md --mode staging --json
.\.venv\Scripts\kaizen-validate.exe tests\fixtures\validation\invalid-missing-field.md --mode canonical
```

The invalid note must return exit code `1`.

Tests must cover:

- every universal required field missing individually;
- project slug validation;
- summary warning threshold;
- all registered note types;
- ID shape and prefix mismatch;
- every enum;
- timezone-required timestamps;
- updated-before-created;
- future timestamp with injected clock;
- each note type's required fields;
- each note type's required sections and empty-section failure;
- conditional supersedence section;
- primary-spec membership in related specs;
- stable-ID relationship syntax;
- URL syntax and embedded credentials;
- provenance all-or-none behavior;
- canonical versus staging `validation_status` behavior;
- Wikilink warning/error mode difference;
- intrinsic lifecycle/authority combinations;
- bounded secret and bulk-content checks;
- deterministic issue ordering;
- human output;
- JSON output;
- exit codes `0`, `1`, and `2`.

## Acceptance Criteria

The packet is complete only when:

1. `kaizen-validate <path> --mode staging|canonical` exists and is documented.
2. stable human and JSON outputs exist.
3. exit codes `0`, `1`, and `2` behave as specified.
4. machine-readable field, enum, and note-type contracts match the accepted Markdown registries.
5. all seven universal fields are enforced.
6. IDs, prefixes, enums, project slugs, summaries, and timestamps are validated.
7. all nine note types enforce their required fields and H2 sections.
8. relationship and source syntax checks pass their test matrix.
9. provenance and staging-only field behavior is enforced without fabricating authorship.
10. intrinsic lifecycle and authority combinations are enforced without claiming promotion-event verification.
11. bounded content-safety errors and warnings are implemented with synthetic fixtures.
12. the complete prior Milestone 1 test suite remains green.
13. all new tests pass, compilation succeeds, and dependency health is clean.
14. no filesystem confinement, write, promotion, database, provider, Hermes, retrieval, or UI scope is added.
15. `C:\dev\kaizen-docs` remains unchanged by the coding agent.
16. the platform working tree is clean after the implementation commit.
17. the completion report lists commands, tests, issue codes, deviations, unresolved contract questions, and recommended next work.

## Dependencies and Blockers

Satisfied:

- Task Packet 001 completed;
- Milestone 1 audit verdict `pass-with-notes`;
- Python 3.11.15 environment exists;
- parser, registries, errors, CLI pattern, fixtures, and tests exist;
- platform repository is clean.

Stop and report when:

- accepted field and note-type registries contradict each other materially;
- a rule requires promotion-event or human-identity proof;
- implementation would require path confinement or canonical writes;
- a content-safety rule cannot avoid obvious false positives within the bounded scope;
- changes would break the committed Milestone 1 library interfaces without an explicit migration rationale.

Do not invent doctrine to bypass a blocker.

## Rollback and Recovery

This packet changes only committed implementation code and fixtures in `C:\dev\kaizen\platform`.

Rollback is a normal Git revert of the Task Packet 002 implementation commit. No canonical vault or staged project content exists yet.

## Completion Report

Return:

```text
Status: complete | partial | blocked
Repository:
Branch:
Commit:
Files created:
Files modified:
Machine-readable contracts added:
Validation issue codes added:
Commands run:
Tests run:
Test results:
CLI checks and exit codes:
Acceptance criteria result:
Deviations:
Unresolved issues:
Contract findings for kaizen-docs:
Recommended next task:
Final git status:
```

Do not mark complete with failing tests, a dirty tree, unresolved acceptance criteria, or deferred scope introduced.