# Research Result 014 - Human Interface Technical Verification and Amendment

Status: active evidence summary
Date: 2026-06-06
Source: Claude targeted verification report supplied by the project owner
Amends: `03-research-results/013-kaizen-human-interface-architecture-claude-summary.md`

## Purpose

Preserve the targeted technical verification of Research Result 013 and reconcile corrections before any human-interface direction becomes doctrine.

## Executive finding

The core architecture survives.

The verification identified one material Tauri security-model correction and several implementation details that must be narrowed or deferred.

```text
Result 013 architecture
= substantially sound

Result 013 technical details
= amend before decision use
```

## Adopt

### Correct Tauri command model

Custom application commands registered through Tauri's application command mechanism are not automatically protected merely because Tauri v2 uses capabilities.

Implementation planning must require:

- complete custom-command inventory;
- explicit application-command permissions;
- window and webview scoping;
- explicit production capability selection;
- tests proving administrative or unused capabilities are not shipped accidentally;
- server-side validation inside every authority-bearing command.

Plugin commands and scopes use Tauri capabilities, but capability configuration does not repair unsafe Rust code or an overly broad typed service.

### Obsidian URI boundary

Official Obsidian URI support is suitable for opening vaults, notes, headings, blocks, and searches.

It does not provide arbitrary live active-note queries from an external application.

Therefore:

- Stage 3 does not need a bridge;
- active-note context remains a possible later plugin capability;
- Local REST API is not adopted as the default bridge.

### Workload-shape rationale

Replace arbitrary note-count claims with the durable distinction:

```text
linked documents and project knowledge
-> Obsidian

large longitudinal observations, operational events, charts, and queues
-> databases + typed services + desktop shell
```

### Stage 2 split

Adopt:

```text
2A - canonical governance foundation
2B - minimal client-neutral read model
2C - Postgres prototype and scoped reads
2D - Qdrant retrieval and indexing prototype
```

Only 2A and a small part of 2B should gate the first useful desktop-shell prototype.

### Concurrency controls

Human-only promotion is not sufficient serialization.

Future promotion tooling requires:

- approved staged-content hash;
- expected destination hash;
- changed-since-review detection;
- single promotion lock or transaction;
- idempotency key;
- atomic same-volume replacement;
- recoverable journal and terminal audit state.

## Modify

### Tauri remains leading candidate, not architectural lock-in

Keep Tauri as the leading desktop-shell candidate. Do not freeze plugin names, Rust APIs, frontend libraries, or packaging details into Decision 0011.

### Filesystem plugin interpretation

Tauri's filesystem plugin may canonicalize and resolve links for its own scoped commands. Kaizen cannot rely on that for Rust-side promotion operations.

Kaizen's own final-path, reparse-point, UNC, device-path, extended-length-path, no-overwrite, and recovery controls remain mandatory.

### Current opener terminology

Remove obsolete Tauri v1-style shell-open examples from planning documents. Later prototypes should use then-current Tauri v2-supported opener/deep-link mechanisms.

### Bridge transport

Windows named pipes are a useful prototype candidate if a bridge is earned. They are not selected architecture.

A later comparison must still evaluate:

- ACL and user-session scope;
- authentication and replay protection;
- pipe squatting;
- reconnect behavior;
- multi-vault behavior;
- cross-platform implications;
- support burden.

### Secret storage

Accept only the invariant:

```text
frontend never receives provider, database, signing, or service credentials
```

Defer exact storage until the service-process topology is known.

### Update safety

Signed updates prove artifact origin, not compatibility or correctness.

Future implementation requirements include:

- opt-in/manual updates initially;
- compatibility checks before startup;
- no irreversible unattended data migrations;
- previous installer or recovery path retention;
- safe-mode behavior;
- documented downgrade and recovery runbook;
- vault and database state independent of application binaries.

Do not make a specific updater workflow doctrine now.

## Reject

Reject as doctrine:

- custom commands are denied by default;
- named pipe is the selected bridge transport;
- OS keychain or a named library is the selected secret store;
- Stronghold status from discussion threads is settled product doctrine;
- manual reinstall alone is an adequate rollback design;
- localhost is an authentication boundary;
- a bridge plugin is definitely required;
- Postgres and Qdrant must exist before the first shell appears.

## Exact amendments to Result 013

Retain:

- progressive hybrid architecture;
- Obsidian as primary document workspace;
- typed-service boundary;
- thick-plugin rejection;
- URI-first integration;
- bridge deferral;
- Tauri as leading shell candidate;
- prototype-driven implementation.

Replace or remove:

- default-denied custom-command claim;
- Tauri v1 shell-open terminology;
- arbitrary note-count threshold;
- automatic or easy rollback implication;
- Stronghold or OS-keychain selection language;
- named-pipe selection language;
- one oversized Stage 2.

Add:

- explicit app-command permission and capability-selection tests;
- Rust-side path confinement responsibility;
- changed-since-review and promotion locking;
- client-neutral read model before rich UI;
- bridge transport and secret storage as deferred implementation choices.

## Decision consequence

A narrow proposed Decision 0011 is justified now.

Classification:

```text
ready to propose
not ready to accept
```

The decision may establish the division of responsibility and progressive direction. It should not select transport, process topology, secrets, updater mechanics, frontend libraries, or screens.

## Roadmap consequence

This UI-planning lane does not block Research Batch B.

Carry its prototype and staged implementation requirements into Phase 7 and Phase 8 implementation-planning inputs.

## Source limitation

Claude could not run local Git commands during the verification and used the public GitHub repository read-only. The report disclosed that limitation. This summary preserves its load-bearing technical findings and the steward's reconciliation.