---
id: kz-aud-01KTMJ37XHX7H2FXEJK34S58HG
type: audit
status: complete
project: kaizen-platform
summary: Correction audit for Packet 008B Gate A after the approved exact command failed before mutation because the global JSON flag was placed after the subcommand.
created: 2026-06-08T21:26:53Z
updated: 2026-06-08T21:26:53Z
review_status: approved
related_specs:
  - 05-specs/staging-write-wrapper-and-promotion-recovery.md
  - 05-specs/staging-and-promotion-workflow.md
---

# Research Result 037 - Packet 008B Gate A Command Correction Audit

## Incident

The owner explicitly approved Packet 008B Gate A. Preflight passed, but the exact command placed the global `--json` option after the `plan` subcommand. Argparse rejected the command before the promotion planner ran.

## Side-effect verification

```text
operation directory: absent
destination parent: absent
promotion log bytes: 0
promotion log sha256: e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855
held candidate sha256: 119788565b83876d833917e6b7c7fdd4f35e9620c0c468202e7628825ad58f1a
docs/platform/vault trees: clean
```

## Corrected command

```powershell
C:\dev\kaizen\platform\.venv\Scripts\kaizen-promote-live.exe --json plan `
  C:\dev\kaizen\staging\projects\kaizen-platform\research\packet-007-governance-bootstrap-completion.md `
  --destination projects/kaizen-platform/research/packet-007-governance-bootstrap-completion.md `
  --packet-id kz-tp-01KTMHA87Z529QH5ES2GWKV2AG `
  --operation-id kz-prom-01KTMHA8F1HXCDXS4SBPC17VNV
```

Only the global option order changes. Packet ID, operation ID, source, destination, hashes, allowed mutation, and Gate B prohibition remain unchanged.

## Authority consequence

The original approval bound an invalid exact command. It must not be silently reinterpreted as approval of the corrected command. Gate A returns to an explicit owner-approval gate.

## Security verdict

**pass for renewed owner review of corrected Gate A command**

Gate B remains prohibited.
