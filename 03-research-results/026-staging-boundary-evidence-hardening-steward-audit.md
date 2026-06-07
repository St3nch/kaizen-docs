---
id: kz-aud-01KTHRP1F2JHB5FB0R58FW81S4
type: audit
status: complete
project: kaizen-platform
summary: Steward audit of subprocess contention, abrupt process-exit recovery, malformed JSONL handling, and governed-rejection side-effect evidence.
created: 2026-06-07T19:24:53Z
updated: 2026-06-07T19:24:53Z
review_status: approved
related_specs:
  - 05-specs/staging-write-wrapper-and-promotion-recovery.md
  - 05-specs/kaizen-hammer-test-strategy.md
---

# Research Result 026 - Staging Boundary Evidence Hardening Steward Audit

## Scope

Audit the tightly bounded R2 test-only change at platform commit `f377f53`.

The change was authorized to strengthen evidence for the existing create-only staging boundary. It did not authorize or introduce production behavior, canonical writes, promotion, Hermes, MCP, overwrite/edit/delete operations, or live-root mutation.

## Change reviewed

Only this implementation-repository file changed:

```text
tests/test_staging_write.py
```

Production source remained unchanged.

## Evidence executed

```text
Targeted staging-write suite: 20 passed in 0.86 seconds
Full platform suite: 130 passed in 2.32 seconds
Failed: 0
Skipped: 0 reported
Diff check: clean
Backup/temp sweep: clean
```

All destructive, malformed-log, concurrency, and interruption cases used pytest disposable roots. The live canonical vault and live staging root were not test targets.

## Finding 1 - independent-process contention is proved

Two independently launched Python processes were released against the same staged destination. Each process constructed its own interpreter state and therefore did not share the module-level `_PROCESS_LOCK`.

Observed governed result:

```text
one process: created
one process: destination_exists
one physical destination file
append-only sequence: intent -> committed -> intent -> rejected
```

This proves the Windows named mutex and create-new boundary operate across independent processes for the current single-user desktop threat model. The prior thread-only test remains useful but is no longer the only automated concurrency evidence.

## Finding 2 - actual process death after durable intent is recoverable

A child process executed the real staging-write flow and called `os._exit(91)` at the existing `after_intent` fault point. This was not an in-process exception and did not run Python cleanup handlers.

Observed governed result:

```text
child exit: 91
persisted evidence before death: one intent event
staged destination after death: absent
next process: deterministic recovery and successful create
final sequence: intent -> recovery -> intent -> committed
```

This proves recovery after abrupt child-process termination at the durable-intent boundary. It does not claim power-loss durability, storage-controller guarantees, or recovery from every later write phase.

## Finding 3 - malformed or truncated JSONL fails closed

The test appended a deliberately truncated final JSON object to a disposable attempt log, then attempted a second staged write.

Observed governed result:

```text
failure code: recovery_required
exit code: 2
existing staged artifact: byte-for-byte unchanged
new destination: absent
malformed log: byte-for-byte unchanged
```

The implementation detects the malformed tail before any new destination mutation and requires human recovery rather than guessing or silently discarding evidence.

## Finding 4 - governed rejection limits side effects to allowed audit evidence

A request that attempted forbidden accepted/approved authority was rejected before destination creation.

Observed governed result:

```text
failure code: agent_authority_forbidden
README.md: byte-for-byte unchanged
projects directory: absent
only new filesystem object: staging-write-attempts.jsonl
recorded phase: rejected
```

This proves the selected governed rejection changes only the explicitly allowed audit evidence in a clean disposable staging root.

## Contract and implementation assessment

The new evidence supports the existing design rather than requiring production-code changes.

- `_PROCESS_LOCK` remains an in-process serialization aid.
- The named Windows mutex is now directly proved as the cross-process boundary.
- Existing recovery logic handles a genuine abandoned-process intent.
- Invalid JSONL remains visible and fail-closed.
- The create-only authority boundary remains intact.

No accepted decision or specification contradiction was found.

## Limitations preserved

This audit does not prove:

- machine power-loss durability;
- multiple Windows user sessions or services;
- network filesystems or ReFS behavior;
- antivirus behavior beyond later dedicated sharing-violation tests;
- canonical first-time atomic installation;
- promotion event or recovery behavior.

Those claims remain outside R2 or belong to Packet 006A/006B.

## Result 025 disposition

The following findings are closed by commit `f377f53` and this audit:

```text
F-07 - independent-process contention automation
F-08 - abrupt child-process termination after durable intent
F-22 - named mutex proved as cross-process boundary while _PROCESS_LOCK remains local aid
```

The adopted malformed/truncated-JSONL and governed-rejection no-side-effect evidence items are also complete for the create-only staging boundary.

## Steward verdict

**pass**

R2 staging-boundary evidence hardening is complete. The platform remains safe to continue into promotion-packet restructuring. Canonical promotion is still not implemented or authorized.

## Next action

Draft Packet 006A as a narrowly bounded research-and-implementation packet for proving the Windows first-time atomic canonical-install primitive. It must not include the full promotion workflow, real canonical promotion, plan persistence, approval semantics, or agent exposure.
