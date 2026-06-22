# Packet 020K — Backup / Restore Proof Plan and Evidence Request

Status: backup/restore proof plan / owner evidence required
Date: 2026-06-22
Packet: 020K
Milestone: 14

## 1. Purpose

Define the backup/restore proof required for M14 after the successful Stage B Neon Ronin implementation-return cycle.

This packet does not perform backup, restore, push, upload, deletion, or mutation.

## 2. Current checkpoint

Kaizen docs repository:

```text
root: docs
branch: main
HEAD: 3be4c8a70f66113744cf7932dddea9f8fc3d8b31
working tree: clean
upstream: origin/main
ahead/behind: 34 / 0
```

Kaizen platform repository:

```text
root: platform
branch: main
HEAD: b7593c5ee90fd32c1e2a86572cc570d307de2be6
working tree: clean
remotes: none
```

Kaizen vault repository:

```text
root: vault
branch: main
HEAD: c898f261c0b341eb8419125247c8bd53ef567d6c
working tree: clean
remotes: none
```

Neon Ronin downstream repository, from owner-supplied Stage B return:

```text
path: C:\dev\neon-ronin
branch: main
final HEAD: 7bbe64f2aec80cd7498e7623f59e68dd2493cb80
status: main...origin/main [ahead 2]
```

## 3. Why backup / restore proof is required now

M14 exit criteria require:

```text
backup and restore proof for selected project data is completed
```

M14 has now produced selected-project data in two categories:

```text
Kaizen governance/proof records in C:\dev\kaizen-docs
Neon Ronin downstream Stage B implementation commits in C:\dev\neon-ronin
```

Because Neon Ronin is currently ahead of origin by two local commits, a restore proof must account for those commits. It is not enough to rely on the remote.

## 4. Backup scope

Minimum backup scope for M14 020K:

```text
C:\dev\kaizen-docs
C:\dev\neon-ronin
```

Optional / context backup scope:

```text
C:\dev\kaizen\platform
C:\dev\kaizen\vault
```

Reason:

```text
Kaizen docs contain the M14 proof trail.
Neon Ronin contains the actual Stage B implementation commits.
Platform and vault are clean and unchanged in this M14 slice, but may be included as baseline recoverability context.
```

## 5. Preferred proof method

Use Git bundle files for Git repositories.

Reason:

```text
Git bundles preserve commits and can be verified by cloning without relying on a network remote.
They are local-file backup artifacts suitable for restore proof.
They avoid provider/customer/private-data expansion beyond the Git-tracked repo contents.
```

## 6. Required owner-side backup command

Run from PowerShell:

```powershell
$stamp = Get-Date -Format "yyyyMMdd-HHmmss"
$backupRoot = "C:\dev\kaizen-backup-work\m14-$stamp"
New-Item -ItemType Directory -Force -Path $backupRoot | Out-Null

$repos = @(
  @{ name = "kaizen-docs"; path = "C:\dev\kaizen-docs"; expectedHead = "3be4c8a70f66113744cf7932dddea9f8fc3d8b31" },
  @{ name = "neon-ronin"; path = "C:\dev\neon-ronin"; expectedHead = "7bbe64f2aec80cd7498e7623f59e68dd2493cb80" }
)

foreach ($repo in $repos) {
  Push-Location $repo.path

  Write-Host "===== BACKUP SOURCE: $($repo.name) ====="
  git status --short --branch
  $head = (git rev-parse HEAD).Trim()
  Write-Host "HEAD: $head"

  if ($head -ne $repo.expectedHead) {
    throw "HEAD mismatch for $($repo.name). Expected $($repo.expectedHead) got $head"
  }

  $dirty = git status --short
  if ($dirty) {
    throw "Repo $($repo.name) is dirty. Stop before backup."
  }

  $bundle = Join-Path $backupRoot "$($repo.name)-$head.bundle"
  git bundle create $bundle --all
  git bundle verify $bundle
  Get-FileHash $bundle -Algorithm SHA256 | Format-List

  Pop-Location
}

Write-Host "===== BACKUP ROOT ====="
Write-Host $backupRoot
Get-ChildItem $backupRoot | Select-Object Name,Length,FullName
```

## 7. Required owner-side restore proof command

After backup bundle creation, run:

```powershell
$backupRoot = "PASTE_BACKUP_ROOT_FROM_PREVIOUS_OUTPUT"
$restoreRoot = Join-Path $backupRoot "restore-proof"
New-Item -ItemType Directory -Force -Path $restoreRoot | Out-Null

$bundles = Get-ChildItem $backupRoot -Filter *.bundle

foreach ($bundle in $bundles) {
  $name = $bundle.BaseName
  $target = Join-Path $restoreRoot $name

  Write-Host "===== RESTORE: $($bundle.Name) ====="
  git clone $bundle.FullName $target
  Push-Location $target
  git status --short --branch
  git log --oneline -5
  git fsck --strict
  Pop-Location
}

Write-Host "===== RESTORE ROOT ====="
Write-Host $restoreRoot
Get-ChildItem $restoreRoot | Select-Object Name,FullName
```

## 8. Evidence required for 020K proof record

Paste the full output from both commands.

The proof record must capture:

```text
backup root path;
repo names;
source HEADs;
source clean states;
bundle paths;
bundle SHA-256 hashes;
git bundle verify results;
restore root path;
restored repo HEAD/log evidence;
git fsck --strict results;
whether any network remote was required;
whether any Git push occurred.
```

## 9. Pass criteria

020K passes only if:

```text
kaizen-docs source HEAD equals 3be4c8a70f66113744cf7932dddea9f8fc3d8b31;
neon-ronin source HEAD equals 7bbe64f2aec80cd7498e7623f59e68dd2493cb80;
both source repos are clean before bundle creation;
Git bundles are created for both repos;
Git bundle verify passes for both bundles;
SHA-256 hashes are recorded for both bundles;
restore clone succeeds for both bundles;
git fsck --strict passes or emits only known non-fatal warnings that are explicitly recorded;
restored logs show the expected recent commits;
no Git push is performed as part of 020K.
```

## 10. Fail / stop criteria

Stop if:

```text
repo HEAD does not match expected value;
repo is dirty;
bundle creation fails;
bundle verification fails;
restore clone fails;
git fsck reports corruption;
secret values appear in output;
command attempts to push or upload;
backup root is outside C:\dev\kaizen-backup-work unless owner explicitly chooses another path.
```

## 11. Current 020K verdict

```text
PENDING — BACKUP / RESTORE PROOF PLAN RECORDED; OWNER-SIDE BUNDLE AND RESTORE EVIDENCE REQUIRED
```

## 12. Non-authorization

This packet does not authorize:

```text
Git push;
cloud upload;
remote deletion;
repo mutation;
platform mutation;
vault mutation;
staging mutation;
database mutation;
provider/customer/private data work;
Observatory / IMI implementation.
```
