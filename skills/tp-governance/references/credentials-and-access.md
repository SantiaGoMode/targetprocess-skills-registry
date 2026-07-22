# Credentials and API access governance

## Platform model

- API access mirrors the authenticated user's roles, type, projects/teams, and entity permissions.
- A user with UI edit permission cannot receive an independently read-only PAT.
- PAT governance does not change permission scope.

## Read-only pattern

1. Create a dedicated service account.
2. Assign a read-only Default Role and minimal project/team/entity scope.
3. Generate PATs from that account only.
4. Restrict integration behavior to GET.
5. Monitor use and revoke/rotate according to policy.

Use for reporting, extraction, analytics, AI, and read-only integrations.

## Write-capable pattern

- Dedicated non-personal service account.
- Minimum required write role/scope.
- One integration purpose per credential where practical.
- Explicit owner and production approval.
- Idempotency, audit correlation, rollback, and monitoring.

Treat System User tokens as administrator-equivalent and exceptional.

## PAT inventory

Record owner, token name, integration, issue date, last usage, expiration, environment, permission scope, rotation date, and revocation procedure. Administrators with required PAT permission can inspect/revoke issued tokens according to the current tenant UI.

## Secret hygiene

- Never commit or paste tokens into examples/tickets.
- Redact query parameters and logs.
- Prefer supported headers over query-string credentials where available.
- Rotate after exposure or owner/purpose change.
- Do not reuse personal PATs for automation/integrations.

## Validation enforcement

Standard-user PAT actions should respect Validation Rules. Automation Rules, Metrics, sync-v2 integrations, and System User operations may bypass them. A standard PAT unexpectedly bypassing validation should be treated as a defect.

## Sources

- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=v1-authentication
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=practices-managing-automation-rules-validation-rules-scale
