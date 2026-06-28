---
id: kz-result-01KZ1BACKUP022FJ
status: complete
type: research-result
project: kaizen-platform
created: 2026-06-28T16:15:00Z
updated: 2026-06-28T16:15:00Z
review_status: steward-reviewed
summary: "Packet 022F through 022J backup v2 key-custody recovery, transfer verification, restore proof, and plaintext cleanup record."
---

# Research Result 383 - Packet 022F Through 022J Backup v2 Key-Custody and Restore-Proof Record

## Scope

This record documents the owner-approved Packet 022F through Packet 022J recovery sequence after Packet 022E restore-proof execution was blocked by missing private age identity custody for the prior backup generation.

Covered sequence:

- Packet 022F new age identity v2 generation;
- Packet 022G fresh local encrypted backup generation;
- Packet 022H USB and Google Drive transfer verification;
- Packet 022I disposable restore proof;
- Packet 022J source plaintext archive cleanup.

This is a docs-only evidence record. It does not generate a key, create a backup, perform transfer, perform restore, delete backup material, mutate the vault or platform roots, or push to GitHub.

## Prior blocked backup status

Previous backup generation:

```text
backup_id:
kz-backup-20260625T210847Z-cc28e52ba881

encrypted archive SHA-256:
fd8f4e7cfb1f5cdd0594839e0eff3701987220d57f737495634438d51f343829
```

That prior backup remains retained and hash-verified from Result 382, but it is not restore-proven unless the original matching private age identity is recovered.

The blocker was key custody, not encrypted-byte integrity:

```text
encrypted backup: verified
USB transfer: verified
Google Drive re-download: verified
plaintext cleanup: verified
restore proof: blocked on missing private identity custody
```

No deletion of the prior encrypted backup, prior verification files, USB copy, or Google Drive copy is authorized or recorded here.

## Packet 022F - new age identity v2

New public recipient:

```text
age1ke6nag08qht9hzfyl3cf0xlcv3je89vncrelxu8pxfch54xrdsqq20uysk
```

Local private identity path:

```text
C:\dev\kaizen-backup-work\age-v2\kaizen-age-identity-v2.txt
```

Public recipient companion path:

```text
C:\dev\kaizen-backup-work\age-v2\kaizen-age-recipient-v2.txt
```

Owner reported:

- private identity v2 was saved in Bitwarden;
- public recipient v2 was saved in Bitwarden;
- private identity material was not pasted into chat, docs, repositories, manifests, or tool responses.

Local ACL was tightened after an initial `icacls` syntax issue. Final reported ACL for the local private identity file:

```text
DESKTOP-TE3C39I\Stench:(F)
BUILTIN\Administrators:(F)
NT AUTHORITY\SYSTEM:(F)
```

Result: pass.

## Packet 022G - fresh local encrypted backup generation

Backup generation completed with the new v2 public recipient.

```text
backup_id:
kz-backup-20260628T155302Z-b325e6f480ac

work_root:
C:\dev\kaizen-backup-work\kz-backup-20260628T155302Z-b325e6f480ac

plaintext_archive_path:
C:\dev\kaizen-backup-work\kz-backup-20260628T155302Z-b325e6f480ac\kz-backup-20260628T155302Z-b325e6f480ac.tar

plaintext_archive_sha256:
9dc52d3c1643561cd741943af03485a6315cf20d374aa7db3fce42266b2bf1f3

encrypted_archive_path:
C:\dev\kaizen-backup-work\kz-backup-20260628T155302Z-b325e6f480ac\kz-backup-20260628T155302Z-b325e6f480ac.tar.age

encrypted_archive_sha256:
a893453e91b09b2eccb1b3026dfe6afc1f083d68b638a8031a757d84a75927da

verify_json_path:
C:\dev\kaizen-backup-work\kz-backup-20260628T155302Z-b325e6f480ac\kz-backup-20260628T155302Z-b325e6f480ac.verify.json
```

Source root checkpoints:

```text
docs HEAD:
0f16cb0a1de08bf626f4a2bc3e7efdeeb109f6ff

vault HEAD:
ba896a5de5ede45ab1408a8f207c85974e4a6992

platform HEAD:
7daabf3eff0b3b0768e88512ca7d596c94e41140
```

Result: pass.

Packet 022G did not perform transfer, restore, deletion, docs edit, vault mutation, platform mutation, Git push, or secret disclosure.

## Packet 022H - transfer verification

### USB SSD

USB destination:

```text
D:\kaizen-recovery\kz-backup-20260628T155302Z-b325e6f480ac
```

USB encrypted archive:

```text
D:\kaizen-recovery\kz-backup-20260628T155302Z-b325e6f480ac\kz-backup-20260628T155302Z-b325e6f480ac.tar.age
```

USB encrypted SHA-256:

```text
a893453e91b09b2eccb1b3026dfe6afc1f083d68b638a8031a757d84a75927da
```

Verify JSON copied:

```text
True
```

Result: pass.

### Private Google Drive

Google Drive re-download path:

```text
C:\dev\kaizen-backup-work\google-redownload\kz-backup-20260628T155302Z-b325e6f480ac.tar.age
```

Google Drive re-download SHA-256:

```text
a893453e91b09b2eccb1b3026dfe6afc1f083d68b638a8031a757d84a75927da
```

Result: pass.

Only the encrypted `.tar.age` archive and non-sensitive `.verify.json` companion were approved for transfer. No plaintext archive transfer is authorized or recorded.

## Packet 022I - disposable restore proof

Restore proof root:

```text
C:\dev\kaizen-restore-proof\kz-backup-20260628T155302Z-b325e6f480ac\v2-local
```

Restore result path:

```text
C:\dev\kaizen-restore-proof\kz-backup-20260628T155302Z-b325e6f480ac\v2-local\packet-022i-restore-result.json
```

Verified encrypted SHA-256:

```text
a893453e91b09b2eccb1b3026dfe6afc1f083d68b638a8031a757d84a75927da
```

Verified plaintext archive SHA-256:

```text
9dc52d3c1643561cd741943af03485a6315cf20d374aa7db3fce42266b2bf1f3
```

Restored Git HEADs:

```text
docs:
0f16cb0a1de08bf626f4a2bc3e7efdeeb109f6ff

vault:
ba896a5de5ede45ab1408a8f207c85974e4a6992

platform:
7daabf3eff0b3b0768e88512ca7d596c94e41140
```

Manifest rows verified:

```text
docs: 527
vault: 17
platform: 151
```

Result: pass.

Packet 022I did not mutate live roots, edit docs, mutate vault, mutate platform, push Git, delete backups, delete Google Drive material, delete USB material, or disclose secret material.

## Packet 022J - source plaintext cleanup

Deleted exact source local plaintext archive:

```text
C:\dev\kaizen-backup-work\kz-backup-20260628T155302Z-b325e6f480ac\kz-backup-20260628T155302Z-b325e6f480ac.tar
```

Cleanup verification:

```text
plaintext_archive_still_exists:
False

encrypted_archive_retained:
True

verify_json_retained:
True

restore_result_retained:
True
```

Result: pass.

Packet 022J did not delete the encrypted backup, verify JSON, restore-proof result, USB copy, Google Drive copy, vault material, platform material, or secret material.

## Current recovery posture

Current restore-proven backup generation:

```text
backup_id:
kz-backup-20260628T155302Z-b325e6f480ac

encrypted archive SHA-256:
a893453e91b09b2eccb1b3026dfe6afc1f083d68b638a8031a757d84a75927da

plaintext archive SHA-256:
9dc52d3c1643561cd741943af03485a6315cf20d374aa7db3fce42266b2bf1f3
```

Current source checkpoints covered by the restore-proven backup:

```text
docs:
0f16cb0a1de08bf626f4a2bc3e7efdeeb109f6ff

vault:
ba896a5de5ede45ab1408a8f207c85974e4a6992

platform:
7daabf3eff0b3b0768e88512ca7d596c94e41140
```

Current conclusion:

```text
Packet 022F through Packet 022J repaired the post-v1 backup key-custody failure by creating a new age identity v2, generating a fresh encrypted backup, verifying USB and Google Drive copies, proving restore in a disposable root, and cleaning up the source plaintext archive while retaining encrypted and evidence material.
```

## Retained material

Retained by design:

- local encrypted `.tar.age` source for backup `kz-backup-20260628T155302Z-b325e6f480ac`;
- local `.verify.json` companion;
- backup work-root payload, manifests, and recovery metadata;
- USB encrypted archive and verify JSON copy;
- private Google Drive encrypted archive and verify JSON copy;
- Google Drive re-download verification copy;
- disposable restore-proof root and `packet-022i-restore-result.json`;
- local private age identity v2 file;
- Bitwarden/offline custody of the private identity and public recipient, owner-reported;
- prior backup `kz-backup-20260625T210847Z-cc28e52ba881`, retained but not restore-proven unless the original private identity is recovered.

Not authorized by this sequence:

- disclosure of private identity contents;
- deletion of encrypted backups;
- deletion from USB;
- deletion from Google Drive;
- deletion of restore-proof evidence;
- vault mutation;
- platform mutation;
- database mutation;
- Git push;
- Observatory / IMI implementation;
- downstream project mutation.
