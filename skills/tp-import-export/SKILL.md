---
name: tp-import-export
description: Plan, configure, validate, and troubleshoot Apptio Targetprocess CSV import/export and Automation Rule orchestration around ADM import/export completion events. Use for CSV mappings, integration users, date/timezone behavior, import limits, batching, reconciliation, or repeatable data migration.
---

# Targetprocess Import and Export

Treat every import as a write operation requiring staging, reconciliation, and rollback planning.

## Required Workflow

1. Define entity types, create/update/upsert behavior, stable identity, relationships, volume, validation, and rollback.
2. Read [csv-profiles-and-mapping.md](references/csv-profiles-and-mapping.md).
3. Also read [automation-orchestration.md](references/automation-orchestration.md) only when Automation Rules or ADM events start/follow the profile.
4. Test a representative small file outside production.
5. Scale in bounded batches and reconcile counts/IDs/errors.

## Guardrails

- Require explicit approval for production imports, overwrites, relationship changes, or deletion behavior.
- Preserve source file, mapping/profile version, execution time, and results.
- Use stable external IDs rather than mutable names.
- Verify stored dates through API, not UI display alone.
- Do not retry malformed files or ambiguous runs without checking current state.

## Routing

- Automation runtime details: `tp-automation-rules`.
- Entity payload/metadata details: `tp-rest-api-v1`.
- Production credentials and change control: `tp-governance`.
