# CSV profiles and mapping

## Preflight contract

Define:

- Target entity type.
- Create, update, or upsert behavior.
- Stable external identity column.
- Required/native/custom fields.
- Reference and relation mappings.
- Validation behavior.
- Source encoding, delimiter, quoting, decimal/date formats.
- Batch size, rollback, and reconciliation.

The profile is configured under Settings → Connections → Integrations and requires administrative access.

## Identity and idempotency

- Use a dedicated external key when available.
- Define duplicate-key behavior.
- Define missing-reference behavior.
- Do not use display names as the only key if IDs/external keys exist.
- Preserve a mapping of source row to resulting Targetprocess ID/error.

## Integration users

The portal documents integration-user mapping with fields including `IsIntegration` and `skipTpValidation: true`. Use validation bypass only for the documented integration-user requirement and review the security/business-rule consequences.

## Dates and timezones

- Prefer date-only input when time is not needed.
- Timestamp without timezone is interpreted using the Targetprocess server/account timezone, not the user's browser timezone.
- Timestamp with `Z` or explicit offset is converted to the account timezone.
- Normalize to ISO 8601 with explicit offset when time matters.
- Test daylight-saving boundaries and verify stored values through API.

## Documented SaaS limits

| Constraint | Documented value |
| --- | --- |
| Relations in mapping | 10 maximum; 9 assignable to entity |
| CSV file size | 15 MB |
| Approximate rate | 240 create/update records per minute |
| Concurrent imports | One; operations are serialized |

Verify the current tenant before scheduling.

## Staged execution

1. Validate headers/types/references without broad production scope.
2. Import a small set containing ordinary, null, Unicode, custom-field, reference, and invalid rows.
3. Re-read entities and compare every mapped field.
4. Run bounded batches.
5. Reconcile source rows, created/updated IDs, errors, duplicates, and totals.

## Source

- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=data-csv-import
