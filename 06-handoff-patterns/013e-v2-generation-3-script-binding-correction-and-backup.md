---
id: kz-tp-01KV3EGEN3V2BINDING00000
type: task-packet
status: draft
project: kaizen-platform
created: 2026-06-14T17:05:00Z
updated: 2026-06-14T17:05:00Z
review_status: pending
authority: proposed
summary: "Correct Generation 3 script bindings, then create and independently restore backup Generation 3."
---

# Task Packet 013E v2 - Generation 3 Script-Binding Correction and Backup

## Purpose

Correct only the stale Generation 2-era source-state bindings in the accepted backup and restore scripts, prove the corrected fail-closed behavior, then execute the previously defined Generation 3 capture, transfer, restore, and bounded regression sequence.

This packet supersedes Packet 013E SHA-256 `3bcf6ffae2f72c4c12835812a779a2bb1c8ce25a364e1b86d8efb71f4bada096` for execution.

## Evidence basis

```text
Result 192 preflight reconciliation:
03-research-results/192-packet-013e-preflight-failure-and-reconciliation.md
SHA-256: 83ae95a6109b381f5289ee7f821391ad854987d8f3092437546bfe765065879e

Prepare script before SHA-256:
5d811b3d86b7201279fed352f662a1271f751eac428717f705b16f24cc8cd7fe

Restore script before SHA-256:
6740978c9e4ca16a7fce79df7562e6a37f07f306da82a0465f045821fb0d406f
```

## Current execution bindings

```text
docs HEAD:
7bb8a1c344b30f486a0b5548c39f4ad486f803bc

platform HEAD:
ba3b5feca90e4fb5cb02e34981dc7ed86942962f

vault HEAD:
2487de669bc44ed50e54fd5dbbfdd128ce659dbb

Go8 HEAD before script correction:
312704a8b8505bdb64f28cc557171c10de8bd5bc

canonical current-state SHA-256:
e50fc7d1d88257ecf6a818a47673775a9f8960e999a6a843872306227dd2b8c7

USB root:
D:\kaizen-recovery\

previous_backup_id:
kz-backup-20260611T181735Z-64f1ccd3800c
```

## Source-change scope

Repository:

```text
C:\dev\chatgpt-mcp
```

Allowed implementation paths:

```text
scripts/packet011c/Prepare-KaizenRecovery.ps1
scripts/packet011d/Restore-KaizenRecovery.ps1
tests/test_backup_tools.py or one new narrowly named recovery-script binding test file only when required
```

No server, MCP tool, public schema, generic execution, cloud API, scheduler, or lifecycle change is authorized.

## Required correction

### Prepare script

Replace hard-coded Generation 2 source-state bindings with explicit mandatory parameters or one deterministic Generation 3 profile that binds:

```text
ExpectedVaultHead
ExpectedPlatformHead
ExpectedDocsHead or equivalent docs evidence binding
ExpectedCurrentStateSha256
ExpectedPromotionLogSha256
ExpectedRoadmapSha256
ExpectedRecoveryContractSha256
```

The chosen method must avoid silently accepting arbitrary live state. Exact values must be supplied by the approved execution command or frozen in an audited Generation 3 profile.

### Restore script

Apply equivalent exact bindings for restored verification. Existing checks for archive safety, encrypted/plaintext hashes, predecessor ID, manifests, repository cleanliness, remotes, current-state, governance log, required evidence, and disposable-root confinement remain mandatory.

### Compatibility

Generation 1 and 2 evidence must remain restorable under their recorded bindings. Do not rewrite or delete historical backup evidence.

## Focused verification

Required minimum:

1. current Generation 3 bindings pass preflight;
2. old vault HEAD fails before workspace creation;
3. old platform HEAD fails before workspace creation;
4. wrong current-state hash fails restore verification;
5. wrong promotion-log hash fails restore verification;
6. wrong predecessor ID fails;
7. nonempty restore root fails;
8. no source mutation occurs on failed preflight;
9. no prior USB generation is overwritten;
10. parser/help or parameter inspection proves exact required bindings.

Run relevant Go8 tests and compile/static checks. Exact changed paths must match the allowlist.

## Git boundary

Create one local Go8 commit for the script correction and focused tests. Go8 has no remote; do not create one or push.

The script-correction commit becomes a new protected checkpoint and must be recorded before backup capture.

## Backup execution sequence

After source correction passes and the owner separately approves the exact corrected packet and Go8 commit:

1. rerun all live clean-state, remote, secret, age, predecessor, and USB preflight checks;
2. create a unique Generation 3 backup workspace;
3. capture and encrypt the protected roots;
4. copy encrypted bytes and verification companion to `D:\kaizen-recovery\<generation-3-id>`;
5. manually upload to the private Google Drive recovery folder;
6. re-download and verify Drive hash and size;
7. restore the Drive copy into the approved disposable root;
8. verify manifests, Git states, canonical hashes, and staging evidence;
9. reconstruct the platform and run the complete current suite;
10. run the seven Generation 3 regressions from the original Packet 013E;
11. record one compact completion/audit result;
12. retain all evidence; perform no cleanup.

## Stop conditions

Stop on any source/hash mismatch, failed focused test, broader script change, unresolved secret finding, dirty repository, unavailable age identity, missing prior generation, USB ambiguity, Google Drive verification failure, restore mismatch, or platform test failure.

## Authorization boundary

Drafting and auditing this revised packet are authorized.

Script implementation requires separate owner approval bound to the exact audited Packet 013E v2 SHA-256.

Backup execution remains a later separate gate bound to the corrected Go8 commit, exact current checkpoints, predecessor ID, and USB root. Approval of the source correction does not itself authorize backup creation.

## Explicit exclusions

- cleanup or deletion;
- prior-generation mutation;
- broad recovery redesign;
- cloud API or automatic sync;
- generic MCP execution;
- canonical or staging mutation;
- vault/platform remote creation or push;
- Milestone 9 artifacts or execution;
- deferred systems.
