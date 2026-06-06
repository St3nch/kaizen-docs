# Research Result 013 - Kaizen Human Interface Architecture Study

Status: active evidence summary
Date: 2026-06-06
Source: Claude research report supplied by the project owner

## Purpose

Preserve and reconcile Claude's broad study of Kaizen's human-interface architecture across Obsidian-native, custom-plugin, Tauri, web, and hybrid approaches.

This file is evidence and steward reconciliation. It does not authorize implementation, select final UI libraries, require a bridge plugin, or make Tauri accepted doctrine.

## Executive finding

The report's central architectural conclusion is adopted:

```text
Obsidian-native workflow proof
-> client-neutral typed Kaizen services
-> minimal desktop control shell
-> intelligence and operational panels as backends mature
-> optional Obsidian bridge only when observed friction justifies it
```

The likely long-term division is:

```text
Obsidian
= canonical document, research, evidence, writing, and deep-review workspace

Desktop control shell
= portfolio, review, intelligence, agent, job, cost, and system-health surfaces

Typed Kaizen services
= shared authority and data-access boundary for all clients
```

Tauri is the leading desktop-shell candidate, but implementation details remain deferred.

## Adopt

### Progressive hybrid direction

Adopt the progressive hybrid over an Obsidian-only, thick-plugin, or Tauri-replaces-Obsidian architecture.

### Workload-shape boundary

Use this practical division:

```text
document-shaped work
-> Obsidian

queue-, chart-, grid-, status-, and operations-shaped work
-> desktop control shell

mixed workflows
-> control-shell surface with one-click evidence handoff to Obsidian
```

### Obsidian remains primary for canonical documents

Keep these Markdown-first and Obsidian-centered:

- project narratives;
- research summaries;
- claims;
- decisions;
- specifications;
- audits;
- task packets;
- experiment interpretations;
- accepted opportunity and strategy briefs;
- durable lessons learned.

### Typed services precede rich UI

All clients should consume Kaizen-owned typed operations for:

- canonical reads;
- validation;
- staging;
- promotion;
- review queues;
- governed Postgres reads and mutations;
- Qdrant retrieval;
- agent and job state;
- audit evidence.

The UI must not become an authority bypass.

### Standard URI integration first

Use supported Obsidian URI and desktop deep-link mechanisms as the first navigation layer.

A custom bridge plugin is not required for the first desktop shell.

### Thick Obsidian plugin rejected

Do not make one large Obsidian plugin responsible for intelligence dashboards, operational state, agent activity, costs, or large database-backed views.

## Modify

### Tauri is leading candidate, not accepted product doctrine

The report strongly favors Tauri. Preserve that as the leading implementation candidate, but do not freeze it into accepted doctrine before focused prototypes and later implementation planning.

### Remove arbitrary vault-size thresholds

Do not rely on forum-derived note-count thresholds. The architecture is justified by workload shape, not a magic number of notes.

### Bridge plugin remains optional

A thin plugin may later provide active-note context, inline status, or governed commands from inside Obsidian. It must be earned by demonstrated workflow friction.

### Stage sequencing must be smaller

Split the service foundation into smaller increments:

```text
2A - canonical reader, validation, staging, promotion, CLI
2B - minimal client-neutral read model
2C - Postgres prototype and scoped database lanes
2D - Qdrant retrieval and indexing prototype
```

The first useful desktop shell should not require all of Postgres and Qdrant to exist.

### Update, secrets, transport, and process topology remain implementation concerns

Do not turn these into architecture doctrine:

- named pipe versus localhost transport;
- OS keychain implementation;
- Stronghold or another encrypted store;
- Rust-only versus sidecar service;
- updater provider and rollback implementation;
- frontend libraries;
- screen designs.

## Reject

Reject:

- replacing Obsidian with a custom Markdown editor;
- embedding or modifying Obsidian itself;
- one thick Kaizen Obsidian plugin;
- direct database or Qdrant credentials in the UI;
- direct broad vault writes from the desktop shell;
- treating client-local or plugin-local state as canonical truth;
- making Postgres and Qdrant prototypes prerequisites for the first UI shell;
- assuming a bridge plugin is inevitable.

## Defer

Defer:

- final desktop framework selection;
- bridge necessity and transport;
- service process lifecycle;
- secret-storage implementation;
- updater and rollback implementation;
- frontend libraries;
- mobile or remote UI;
- multi-vault support;
- final deep-link protocol;
- pixel-level design.

## Recommended target architecture

```text
Obsidian
    |
    | supported URI/deep links
    v
Desktop control shell (Tauri leading candidate)
    |
    | typed client-neutral operations
    v
Kaizen service boundary
    |             |              |
    v             v              v
Markdown      Core Postgres   Intelligence DB / Qdrant
```

The shell presents state. The service boundary enforces authority. Obsidian remains the deep document workspace.

## Workflow placement

### Obsidian

- project command centers;
- current-state notes;
- research, claims, decisions, specs, audits;
- canonical strategy and opportunity documents;
- source summaries and evidence navigation.

### Desktop shell

- portfolio overview;
- review inbox;
- staged-draft and validation queues;
- intelligence charts and ranking history;
- signals, opportunities, experiments, and outcomes;
- agent runs, jobs, schedules, costs, failures, and health.

### Mixed workflows

- Tauri review card opens exact evidence in Obsidian;
- Tauri intelligence row opens its canonical source note;
- Obsidian link opens the matching Kaizen operational context.

## Prototype implications

Later implementation planning should carry small prototypes for:

- Tauri-to-Obsidian navigation;
- shared-vault read and external-change behavior;
- minimal desktop shell over a client-neutral read model;
- review gate and changed-since-review handling;
- intelligence panel pagination and evidence linking;
- optional bridge only if a real feature needs active-note context.

## Steward conclusion

The broad report supports a progressive hybrid direction. Its technical details require the amendments recorded in Research Result 014.

## Source limitation

The raw report was supplied in conversation text after Claude cloned the public repository. This summary preserves its load-bearing conclusions and the steward's reconciliation.