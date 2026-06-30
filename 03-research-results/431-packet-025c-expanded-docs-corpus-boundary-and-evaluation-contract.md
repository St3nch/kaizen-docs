# Packet 025C — Expanded Docs Corpus Boundary and Evaluation Contract

Status: complete — docs-only contract
Date: 2026-06-30
Milestone: post-M16 retrieval expansion gate

## Purpose

Define the expanded docs corpus boundary and evaluation contract selected by 025B.

025C is docs-only. It does not authorize implementation.

## Inputs

```text
03-research-results/430-packet-025b-retrieval-expansion-options-and-gate-contract.md
03-research-results/427-packet-024r-milestone-16-closure-record.md
03-research-results/425-packet-024p-retrieval-vector-baseline-hammer-audit.md
```

## Corpus decision

The first expanded docs corpus should include the M15 and M16 governed research-result records, plus the immediate post-M16 retrieval handoff records.

This corpus expands beyond the 024J M16-only pilot while staying in the docs repository and avoiding vault/private-data risk.

## Inclusion rule

Allowed root:

```text
docs
```

Allowed directory:

```text
03-research-results/
```

Allowed file pattern:

```text
03-research-results/<number>-*.md
```

Allowed numeric range:

```text
387 through 430 inclusive
```

In plain terms, the corpus is:

```text
M15 packet/result/closure records;
M16 packet/result/closure records;
024S handoff correction;
025A roadmap reconciliation;
025B retrieval expansion lane selection.
```

## Required manifest behavior

A later implementation packet must not use a loose recursive crawl as authority.

It must build or declare a deterministic manifest from the 025C inclusion rule and then validate every selected path before indexing.

The manifest must reject:

```text
paths outside 03-research-results/;
paths outside numeric range 387..430;
non-Markdown files;
absolute paths;
parent-directory traversal;
missing files;
duplicate paths;
files with unreadable UTF-8;
files whose source hash changes after manifest assembly.
```

## Excluded paths

025C excludes:

```text
00-entrypoint/;
01-project-standard/;
02-decisions/;
04-task-packets/;
05-audits/;
06-roadmaps/;
platform source;
vault;
staging;
chatgpt_mcp;
kaizen_mcp;
Northstar pilot repos;
backup artifacts;
private/customer/provider data.
```

## Source metadata contract

Every indexed source must retain:

```text
source_root_id = docs;
canonical_source_kind = kaizen-docs-markdown;
source_path;
source_hash;
source_commit_or_snapshot;
corpus_boundary;
chunker_version;
retrieval vector space id;
payload schema version;
index generation id;
traceability status.
```

## Corpus boundary id

Recommended boundary id for later implementation:

```text
post-m16-expanded-docs-results-387-430
```

The boundary id must be distinct from the 024J/024K M16 pilot boundary.

## Evaluation query families

A later implementation packet must define fixed query strings before running tests.

Minimum query families:

```text
M15 parser and typed read model;
M15 context-pack assembly;
M16 Qdrant derivative boundary;
M16 source hash and re-resolution;
M16 retrieval vector baseline;
M16 closure and retained non-authorizations;
post-M16 roadmap reconciliation;
retrieval expansion lane selection.
```

Each query must declare:

```text
query id;
query text;
expected source number range or source family;
minimum accepted candidate count;
negative expectations;
reason for inclusion.
```

## Negative checks

A later implementation proof must fail closed for:

```text
unlisted docs paths;
files outside numeric range 387..430;
wrong corpus boundary;
stale index generation;
stale vector space;
missing source hash;
changed source hash;
wrong source root;
wrong note type;
vault paths;
entrypoint paths;
roadmap paths;
platform paths;
private-data paths.
```

## Quality expectations

The expanded corpus proof should demonstrate that:

```text
M15 queries retrieve M15 source families;
M16 queries retrieve M16 source families;
post-M16 reconciliation queries retrieve 024S/025A/025B families;
results remain source-resolvable;
context-pack candidate manifests remain human-review-required;
retrieved Markdown remains untrusted evidence, not instruction;
quality failures are recorded without weakening governance filters.
```

## Stop conditions

Stop before implementation completion if:

```text
manifest assembly requires broad crawling;
manifest selection is ambiguous;
retrieval requires vault or private paths;
source re-resolution cannot be preserved;
quality scoring weakens policy filtering;
implementation requires provider-backed embeddings;
agent-facing tooling becomes necessary;
production gateway assumptions appear.
```

## Recommended next packet

Recommended next packet:

```text
025D — Expanded Docs Corpus Implementation
```

025D should require separate owner acceptance before platform mutation.

## Non-authorization

025C does not authorize:

```text
platform mutation;
vault mutation;
staging mutation;
database mutation;
Qdrant writes;
expanded corpus implementation;
vault indexing;
platform source indexing;
private-data indexing;
external embedding dependency;
agent-facing retrieval tools;
production gateway;
Hermes implementation;
Tauri implementation;
roadmap milestone renumbering.
```
