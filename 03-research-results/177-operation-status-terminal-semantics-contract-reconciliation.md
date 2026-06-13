---
id: kz-result-01KTZ6OPSTATCONTRACTRECON
type: research-result
status: complete
project: kaizen-platform
created: 2026-06-14T00:45:00Z
updated: 2026-06-14T00:45:00Z
review_status: steward-reviewed
authority: proposed
summary: "Contract reconciliation for preserving terminal operation history while reporting current-world mismatches separately."
---

# Research Result 177 - Operation-Status Terminal-Semantics Contract Reconciliation

## Purpose

Reconcile the accepted Milestone 6 operation-status contract with live Packet 012E evidence and the current platform implementation before Packet 013C implementation is considered.

This result is a proposed contract correction. It does not change the accepted specification or platform code.

## Verified current contract

The accepted specification at:

```text
05-specs/milestone-6-governed-operator-surface.md
SHA-256:
0474fe139c4daa24260113b008fd9d16012367788faf1e4733b711fc7062d77c
```

defines:

```text
committed:
exactly one successful committed terminal event exists and installed result hash verifies

recovered:
exactly one recovered terminal event exists and recovery evidence verifies

invalid:
contradictory, replayed, drifted, hash-mismatched, root-mismatched, Git-mismatched, or otherwise non-actionable evidence exists
```

The contract also exposes Git state and a structured mismatch list separately from `state`.

## Verified implementation behavior

Current implementation:

```text
C:\dev\kaizen\platform\src\kaizen\operation_status.py
SHA-256:
5f75fb64635b3783a3fe6a629da398bbc4c985856fd5628248d88ffdd6346a74
```

uses this precedence:

```python
if mismatches:
    state = "invalid"
elif terminal:
    state = terminal_phase
```

Git mismatches, including `git_not_clean`, are appended before state selection.

Therefore any mismatch erases the textual terminal state even when:

- a valid committed or recovered terminal event exists;
- installed canonical bytes match terminal evidence;
- execution and recovery are already ineligible;
- the dirty repository is the expected human-commit boundary after canonical mutation.

## Live evidence

Packet 012E executed an approved amendment exactly once. The operation returned:

```text
result: committed
terminal events: intent -> committed
installed/result SHA-256 verified
eligibility.execute: false
eligibility.recover: false
git.clean: false
mismatch: git_not_clean
textual state: invalid
```

The operation was historically committed. Current Git cleanliness was false because the authorized canonical note and governance log awaited a separately authorized vault commit.

This proves a semantic collision:

```text
historical terminal outcome
!=
current-world integrity/eligibility posture
```

## Required contract correction

### Primary state meaning

`state` should identify the strongest verified lifecycle fact about the operation.

Proposed precedence:

1. contradictory or untrustworthy terminal evidence -> `invalid`;
2. one verified successful terminal event -> `committed` or `recovered`;
3. one verified failed terminal event with no accepted successful terminal -> `failed`;
4. no terminal event -> `approved`, `ready`, `incomplete`, or `absent` according to existing rules;
5. nonterminal evidence corruption or unsafe binding -> `invalid`.

### Mismatch meaning

`mismatches` should report current-world, binding, integrity, or repository findings without automatically rewriting a verified terminal historical result.

Not every mismatch has the same effect.

#### Terminal-history-invalidating findings

These may force `state: invalid` even when terminal events exist because the claimed terminal history cannot be trusted:

- malformed or contradictory terminal sequence;
- more than one successful terminal event;
- terminal event binding mismatch;
- installed/result hash mismatch against terminal evidence;
- destination missing for a claimed committed result;
- plan, approval, operation, packet, or event evidence tampering that prevents trustworthy terminal attribution;
- unsupported or contradictory event phases.

#### Terminal-preserving current-world findings

These remain in `mismatches` while preserving `state: committed`, `recovered`, or `failed` when terminal evidence and installed result are trustworthy:

- `git_not_clean` caused by the expected uncommitted canonical/governance result;
- current branch or HEAD drift after the operation completed;
- unrelated later repository changes;
- later-current-state conditions that make further mutation unsafe but do not disprove historical completion.

Some branch/HEAD findings may still invalidate history when evidence proves the operation did not execute under its bound Git checkpoint. Packet 013C must distinguish execution-time binding evidence from later inspection-time drift rather than classify solely by code name.

### Eligibility

Terminal states always remain:

```text
eligibility.execute: false
eligibility.recover: false
```

Current mismatches remain visible and may cause nonzero CLI findings or block follow-up operations. Preserving terminal history must not make a completed operation executable or recoverable again.

### CLI and operator semantics

The local status CLI and operator console should distinguish:

```text
terminal success with current findings
```

from:

```text
invalid or untrustworthy operation evidence
```

A terminal state with mismatches must not silently return the same semantic treatment as an approved clean state. The exact exit-code policy should be frozen by Packet 013C tests and documented in the implementation return.

The smallest safe direction is:

- preserve terminal lifecycle state in structured output;
- preserve all mismatch details;
- keep execute/recover disabled;
- use nonzero exit when current findings require human attention, unless existing machine consumers demonstrably require a compatible alternative;
- ensure operator mutation commands still refuse any pre-action mismatch.

## Proposed specification changes

Packet 013C should amend only the operation-status sections of:

```text
05-specs/milestone-6-governed-operator-surface.md
```

Required changes:

1. revise `invalid` so current-world Git drift does not automatically erase trustworthy terminal history;
2. add explicit state-precedence rules;
3. define terminal-history-invalidating versus terminal-preserving findings;
4. preserve all existing execute and recover safety requirements;
5. document terminal-with-findings CLI/operator behavior;
6. retain backward compatibility with historical event shapes;
7. avoid adding new public status states unless focused implementation evidence proves the existing state-plus-mismatches contract cannot express the distinction.

No event schema change is currently required.

## Required focused tests

At minimum:

1. committed amendment plus expected dirty repository -> `state: committed`, `git.clean: false`, `git_not_clean` retained, no eligibility;
2. recovered operation plus later dirty repository -> `state: recovered`, mismatch retained, no eligibility;
3. failed terminal event plus later Git finding -> `state: failed`, finding retained, no eligibility;
4. committed terminal with destination/result hash corruption -> `state: invalid`;
5. duplicate successful terminal events -> `state: invalid`;
6. approved nonterminal operation with dirty Git -> `state: invalid`, execution disabled;
7. ready nonterminal operation with branch or HEAD mismatch -> `state: invalid`;
8. operator execute/recover still rejects pre-action mismatches;
9. status CLI structured JSON preserves terminal state and mismatch list;
10. CLI exit behavior for terminal-with-findings is explicit and tested;
11. Packet 012E exact amendment scenario is reproduced in a disposable Git repository;
12. promotion and amendment operation types receive equivalent coverage.

## Hammer requirements

Packet 013C should include a focused hammer family covering:

- terminal event plus expected dirty Git;
- terminal event plus unrelated later Git drift;
- terminal event plus installed-result tamper;
- duplicate terminal events;
- nonterminal dirty-state rejection;
- idempotent inspect repeatability;
- no mutation from inspection;
- no terminal-state regression after repository commit returns the tree clean.

The full accepted platform suite and compileall remain required after focused proof.

## Compatibility posture

- No new event phase.
- No history rewrite.
- No second successful terminal event.
- No weakening of pre-execution Git or binding gates.
- No re-enabling of execute or recover after terminal completion.
- No change to canonical prepare/plan/approve/execute sequencing.
- No generalized cleanup, rollback, or correction semantics.

## Conclusion

The accepted contract requires a narrow amendment before code correction. Packet 013C should implement the specification and platform correction together, prove the exact Packet 012E scenario, and stop before any canonical current-state operation.
