# Packet 024F — Hammer Tests and Prototype Implementation Return Audit

Status: audit packet draft — not owner accepted
Date: 2026-06-29
Milestone: 16
Packet: 024F

## Purpose

Define the next Milestone 16 packet after Packet 024E implemented the disposable synthetic local retrieval prototype.

Packet 024F is a docs-only audit packet. It should re-check the 024E prototype against the accepted 024A through 024E contracts, re-run the relevant tests, assess hammer coverage, record residuals, and recommend the next M16 step.

024F should not implement new code.

## Authority basis

```text
ROADMAP_V0.4.md
00-entrypoint/LLM_START_HERE.md
03-research-results/410-packet-024e-implementation-return.md
03-research-results/409-packet-024e-disposable-synthetic-local-prototype.md
03-research-results/408-packet-024d-retrieval-gateway-and-typed-tool-contract-result.md
03-research-results/406-packet-024c-chunk-payload-id-and-rebuild-contract-result.md
03-research-results/404-packet-024b-corpus-boundary-and-governance-contract-result.md
03-research-results/402-packet-024a-m16-definition-and-qdrant-evidence-reconciliation-result.md
03-research-results/400-milestone-16-qdrant-research-context-and-hammer-test-direction.md
```

## Starting commits for acceptance

024F audit may be accepted only against these observed commits unless superseded by a later explicit owner acceptance:

```text
docs starting commit for this draft: afe55198110375546d4295cce354ff1c2efcbefb
platform audit target commit: f5c8804df4a1e31f9163395c55c82d11dcbeeb46
vault observed commit: 6fbc8cefe4ed4cd57403648ee3977ee2aeaaba5b
```

If any starting commit changes before acceptance, re-verify and amend the packet before execution.

## 024F objective

Execute a docs-only audit of the 024E synthetic local prototype.

The audit should answer:

```text
Did 024E stay within accepted scope?
Did 024E avoid infrastructure setup and real corpus indexing?
Did 024E preserve vault, staging, and database boundaries?
Did the synthetic fixtures cover the required trap classes?
Did the prototype implement deterministic chunk, payload, ID, source-hash, index-generation, and gateway-filter behavior?
Did the tests prove the required hammer cases?
Were any connector safety blocks encountered and handled transparently?
Are there blocking defects before accepting 024E as the policy-first local proof?
What should the next M16 packet be?
```

## Audit scope

024F audit scope is limited to:

```text
platform commit f5c8804df4a1e31f9163395c55c82d11dcbeeb46;
docs Result 410 implementation return;
accepted Packet 024E scope and tests;
accepted M16 contracts from 024A through 024D;
repository cleanliness and exact path evidence;
test re-run evidence;
residual-risk analysis;
next-packet recommendation.
```

## Required source reads

024F execution should read or inspect:

```text
03-research-results/410-packet-024e-implementation-return.md;
03-research-results/409-packet-024e-disposable-synthetic-local-prototype.md;
03-research-results/408-packet-024d-retrieval-gateway-and-typed-tool-contract-result.md;
src/kaizen/m16_retrieval_prototype.py;
tests/test_m16_retrieval_proto.py;
platform commit details for f5c8804df4a1e31f9163395c55c82d11dcbeeb46.
```

## Required checks

024F should perform these checks where connector safety permits:

```text
verify docs repository state;
verify platform repository state;
verify vault repository state;
inspect platform commit path manifest;
run focused M16 synthetic retrieval tests;
run collateral M15, Markdown, validation, and M16 tests;
run diff/whitespace checks where not blocked;
record any connector safety-blocked checks honestly.
```

## Required test commands

Preferred exact commands:

```text
python -m pytest -k m16_retrieval_proto
python -m pytest -k "m16_retrieval_proto or test_markdown or test_validation or test_m15_read_model"
```

If direct path commands work, they may also be run and recorded.

## Hammer coverage to audit

024F should verify that 024E proves the synthetic coverage families recorded in Result 410:

```text
allowed candidate passes;
wrong-project candidate is blocked;
restricted-class candidate is blocked;
superseded candidate is blocked;
volatile operating notes are excluded by default;
draft/rejected candidates are blocked;
bad metadata fails closed;
retrieved trap text cannot alter policy;
chunk IDs and point IDs are deterministic;
source hash changes are detected;
inactive generation is rejected;
embedding-space mismatch is rejected;
payload-schema mismatch is rejected;
caller backend-control fields are detected;
result projection stays narrow;
context-pack candidate manifest includes safe references and rejection reasons;
source re-resolution accepts matching source and rejects mismatches;
missing source reference is rejected.
```

## Blocking-defect criteria

024F should mark the prototype as not acceptable if any blocking issue is found:

```text
real corpus content was indexed;
infrastructure setup or external-service use occurred;
embedding installation/download occurred;
vault, staging, or database was mutated;
agent-facing retrieval tools were exposed;
required synthetic leakage/rejection tests fail;
logical IDs are nondeterministic;
source mismatch does not reject;
result projection exposes backend control surfaces;
manifest exposes restricted trap content;
required test family fails;
changed paths exceed accepted scope without recorded justification.
```

## Expected 024F deliverable

024F execution should create a docs-only audit result:

```text
03-research-results/412-packet-024f-hammer-tests-and-prototype-audit-result.md
```

The result should include:

```text
owner acceptance line;
starting commits;
source reads;
platform commit/path evidence;
repository state evidence;
test commands and results;
hammer coverage matrix;
connector safety-block notes;
blocking-defect assessment;
residual risks;
recommendation for next packet.
```

## Acceptance criteria for 024F execution

024F execution is complete only if:

```text
no implementation is performed;
no infrastructure setup or external-service use occurs;
no embedding model is installed;
no platform files are changed;
no vault files are changed;
no staging files are changed;
no database state is changed;
no real corpus is indexed;
024E test evidence is re-run or clearly bounded by connector-safe equivalent commands;
hammer coverage is assessed;
blocking defects are explicitly assessed;
read path is refreshed;
docs commit is pushed.
```

## Stop conditions

Stop immediately if 024F requires:

```text
platform mutation;
vault mutation;
staging mutation;
database mutation;
infrastructure setup;
embedding installation;
network access;
real corpus indexing;
new synthetic fixture creation;
production gateway work;
agent-facing retrieval tools;
path expansion beyond docs audit result/read-path updates.
```

## Non-authorization

This draft packet does not authorize 024F execution until explicitly owner accepted.

Even if accepted, it does not authorize:

```text
Qdrant-backed prototype;
infrastructure setup;
embedding model installation;
real corpus indexing;
real docs/vault corpus manifest generation;
platform mutation;
vault mutation;
staging mutation;
database mutation;
production gateway implementation;
agent-facing retrieval tools;
Hermes write authority;
Tauri implementation;
Observatory / IMI implementation;
commercial operations;
backup deletion;
restore work.
```

## Recommended owner acceptance line

Recommended owner action after review:

```text
Accept Packet 024F for docs-only hammer-test and implementation-return audit of the 024E synthetic local retrieval prototype at platform commit f5c8804df4a1e31f9163395c55c82d11dcbeeb46, with exact audit scope, tests, and non-authorization boundaries as listed.
```

After accepted 024F execution, the likely next decision should be one of:

```text
accept 024E as policy-first local proof and plan a Qdrant-backed disposable prototype;
request a narrow repair packet if 024F finds a blocking defect;
amend M16 sequence if Qdrant-backed prototype should wait for another contract layer.
```
