# Milestone 13 — DB Tooling Incident and Correction

Status: incident correction / stewardship note
Date: 2026-06-22
Milestone: 13

## 1. Summary

During Path A live-DB proof preparation for Milestone 13 closure, the steward made an operator error while trying to inspect the database tooling path.

The steward accidentally created:

```text
role: dummy
database: dummy
```

The accidental database was immediately removed by the steward. The owner then removed the accidental role manually and verified the role list.

## 2. Owner correction and instruction

Owner statement:

```text
yes, this is a mistake. But its ok now. Remember and make note: We do things smart and optimal.
```

This statement is accepted as an operational correction and stewardship instruction.

## 3. Cleanup evidence

Owner cleanup command executed successfully:

```text
DROP ROLE IF EXISTS dummy;
```

Owner verification output showed only expected roles:

```text
kaizen_ops_backup
kaizen_ops_migrator
kaizen_ops_owner
kaizen_ops_readonly
kaizen_ops_service
kaizen_ops_test_migrator
postgres
```

Steward follow-up role inspection also showed no `dummy` role.

## 4. Current database/role state after correction

Expected roles present:

```text
kaizen_ops_backup
kaizen_ops_migrator
kaizen_ops_owner
kaizen_ops_readonly
kaizen_ops_service
kaizen_ops_test_migrator
postgres
```

Expected databases present:

```text
kaizen_ops
kaizen_ops_restore_016f
kaizen_ops_service_test
kaizen_ops_test
postgres
```

No `dummy` database remains.

No `dummy` role remains.

## 5. Root cause

The root cause was tool-selection discipline failure:

```text
The steward used a mutation-capable PostgreSQL helper while attempting to inspect capability behavior.
The action was not necessary to configure the DSN path.
The correct path was to use read-only inspection plus manual/explicit owner-approved setup commands.
```

## 6. Corrected rule

For the rest of Milestone 13 and Milestone 14:

```text
Do not probe mutation tools by executing harmless-looking placeholder mutations.
Do not create dummy roles, dummy databases, dummy files, or dummy records unless a task packet explicitly authorizes synthetic objects and names them.
Use dry-run when available.
Prefer read-only inspection.
For database setup, produce the exact command and let the owner approve or run it unless a task packet explicitly authorizes mutation.
Stop on uncertainty instead of experimenting in live local infrastructure.
```

## 7. Smart and Optimal reinforcement

Kaizen operating principle reaffirmed:

```text
Smart and Optimal means exact enough to be safe, light enough to move, and honest enough to correct mistakes immediately.
```

Applied rules:

```text
verify before claim;
read before edit;
search before create;
diff before write;
never infer approval;
never widen scope;
preserve evidence;
stop safely on mismatch;
no speculative dummy mutation;
no fake safety through paperwork.
```

## 8. Effect on Milestone 13

This incident does not invalidate the existing M13 planning package, but it must remain visible in the closure record.

Effect:

```text
M13 Path A can continue after cleanup.
The role/database state is restored.
Future live-DB proof work must avoid mutation-tool probing.
The M13 closure audit should mention that the incident was corrected before closure.
```

## 9. Non-authorization

This note does not authorize:

```text
Milestone 13 closure;
Milestone 14 execution;
platform mutation;
vault mutation;
staging mutation;
database mutation;
SQL migration execution;
new operational record families;
provider/customer-data work;
Observatory / IMI implementation;
Qdrant;
Hermes;
Tauri;
Neon Ronin implementation;
SearchClarity implementation;
Git push.
```
