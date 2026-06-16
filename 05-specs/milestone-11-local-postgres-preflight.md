---
id: kz-spec-01KV6LLQ8M2N7R5C3Y9P4T6BW0
type: specification
status: proposed
project: kaizen-platform
created: 2026-06-16T02:30:00Z
updated: 2026-06-16T02:30:00Z
review_status: pending
authority: proposed
summary: "Define the exact local Windows PostgreSQL and secret-injection preflight required before Packet 016C implementation approval."
---

# Milestone 11 Local PostgreSQL Preflight

## Purpose

Capture exact local evidence that Packet 016C can use PostgreSQL 18 safely without guessing service state, client version, port, database identity, timezone, or secret handling.

This preflight does not create a database, role, schema, table, migration, or source file.

## Required operator posture

Run in PowerShell 7 from a normal owner account. Do not paste passwords, connection strings, tokens, or secret values into the report.

The operator may redact usernames if desired, but must preserve service names, versions, ports, database names, result codes, and timestamps.

## Exact preflight commands

### 1. Service inventory

```powershell
Get-Service -Name 'postgresql*' |
    Select-Object Name, Status, StartType |
    Sort-Object Name
```

Required result:

- at least one PostgreSQL 18 service exists;
- the selected PostgreSQL 18 service is `Running`;
- the selected service identity is recorded exactly.

### 2. Client binary identity

Preferred expected binary:

```text
C:\Program Files\PostgreSQL\18\bin\psql.exe
```

Run:

```powershell
$Psql = 'C:\Program Files\PostgreSQL\18\bin\psql.exe'
if (-not (Test-Path -LiteralPath $Psql)) {
    throw "PostgreSQL 18 psql not found at expected path: $Psql"
}
& $Psql --version
Get-FileHash -Algorithm SHA256 -LiteralPath $Psql |
    Select-Object Path, Algorithm, Hash
```

Required result:

- `psql` reports major version 18;
- binary SHA-256 is recorded;
- no PATH-based ambiguous binary is used.

### 3. Server connection and identity

Use the owner-controlled secret source to inject only the current PowerShell process environment. Do not write `.env`, `.pgpass`, plaintext scripts, or repository files.

Conceptual example only:

```powershell
$env:KAIZEN_OPS_DSN = '<injected from owner-controlled secret store>'
```

Then run:

```powershell
if ([string]::IsNullOrWhiteSpace($env:KAIZEN_OPS_DSN)) {
    throw 'KAIZEN_OPS_DSN is not set in this process.'
}

& $Psql $env:KAIZEN_OPS_DSN `
    --no-psqlrc `
    --set ON_ERROR_STOP=1 `
    --tuples-only `
    --no-align `
    --command "select current_setting('server_version'), current_setting('server_version_num'), current_database(), current_setting('TimeZone'), current_setting('server_encoding'), current_setting('data_checksums'), current_setting('password_encryption');"
```

Required result:

```text
server major version: 18
expected proving-ground database: owner-selected disposable preflight database, not kaizen_ops unless separately created later
TimeZone: UTC or explicitly identified for later session override
server_encoding: UTF8
data_checksums: on
password_encryption: scram-sha-256
```

The preflight may use an existing disposable database. It must not create `kaizen_ops` under readiness-only authority.

### 4. Port and server address

```powershell
& $Psql $env:KAIZEN_OPS_DSN `
    --no-psqlrc `
    --set ON_ERROR_STOP=1 `
    --tuples-only `
    --no-align `
    --command "select inet_server_addr(), inet_server_port();"
```

Record the exact port. Packet 016C must bind to the verified port through secret injection rather than hard-code it in source.

### 5. Role privilege inspection

```powershell
& $Psql $env:KAIZEN_OPS_DSN `
    --no-psqlrc `
    --set ON_ERROR_STOP=1 `
    --tuples-only `
    --no-align `
    --command "select current_user, session_user, rolsuper, rolcreatedb, rolcreaterole, rolreplication, rolbypassrls from pg_roles where rolname = current_user;"
```

Required finding:

- readiness report identifies whether the preflight role is administrative or least-privilege;
- Packet 016C must not use a superuser as the runtime service role;
- any owner/admin role is used only for explicit database/role bootstrap steps.

### 6. Extension inventory

```powershell
& $Psql $env:KAIZEN_OPS_DSN `
    --no-psqlrc `
    --set ON_ERROR_STOP=1 `
    --tuples-only `
    --no-align `
    --command "select extname, extversion from pg_extension order by extname;"
```

No extension is required by the first slice. Unexpected extensions do not automatically block readiness, but the report must state that Packet 016C does not depend on them.

### 7. Python runtime identity

Packet 016C is bound to Python 3.11 by `pyproject.toml`.

Run from `C:\dev\kaizen\platform`:

```powershell
python --version
python -c "import sys; print(sys.executable); print(sys.version)"
```

Required result:

- Python major/minor is 3.11;
- executable path is recorded;
- no dependency is installed under readiness-only authority.

### 8. Secret leakage checks

After clearing the process variable:

```powershell
Remove-Item Env:KAIZEN_OPS_DSN -ErrorAction SilentlyContinue

Set-Location C:\dev\kaizen\platform
git status --short

git grep -n -I -E "postgres(ql)?://|KAIZEN_OPS_DSN|PGPASSWORD|password=" -- . `
    ':(exclude).git' `
    ':(exclude)*.lock'
```

Required result:

- platform working tree remains clean;
- no connection string, password, or secret value entered tracked or untracked repository files;
- variable is removed from the current process after use.

## Secret-source decision

Before Packet 016C approval, the owner must select one exact local secret source:

```text
A. Windows Credential Manager, injected into a short-lived process environment by an owner-run launcher
B. Bitwarden owner-managed secret, manually injected into a short-lived process environment
C. another owner-controlled Windows secret store with equivalent no-repository and no-log guarantees
```

Steward recommendation:

```text
A. Windows Credential Manager for local runtime credentials
Bitwarden remains appropriate for disaster-recovery secrets and offline records
```

The repository and application code see only the short-lived `KAIZEN_OPS_DSN` process variable. They do not retrieve or manage the owner's master secret store.

## Required frozen readiness evidence

Create one non-secret report containing:

```text
preflight timestamp
PowerShell version
service inventory
selected PostgreSQL service
psql version and binary SHA-256
server version and version number
server address and port
preflight database name
server timezone and encoding
data-checksum state
password-encryption setting
preflight role privilege flags
extension inventory
Python version and executable
selected secret-source option
confirmation that KAIZEN_OPS_DSN was removed
platform Git status
redaction statement
```

The report must not contain the DSN, password, host credentials, or secret-store contents.

## Stop conditions

Stop Packet 016C approval if:

- PostgreSQL 18 service or client cannot be verified;
- server major version is not 18;
- data checksums are not enabled;
- password encryption is not `scram-sha-256`;
- Python is not 3.11;
- secret injection requires a repository file;
- platform working tree becomes dirty;
- runtime service design would require superuser credentials;
- the owner cannot identify a bounded disposable test database posture;
- output contains secret material that cannot be safely redacted and re-created.
