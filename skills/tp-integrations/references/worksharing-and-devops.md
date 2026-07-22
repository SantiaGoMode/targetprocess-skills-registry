# WorkSharing and DevOps services

## WorkSharing service

The documented API includes:

- `getProxy`: access the configured external tool's HTTP API.
- `getEntityShares`: inspect shares for Targetprocess entities.
- `shareEntity` / `shareEntities`: create shares.
- `deleteEntitySharing`: unlink/remove sharing.
- `getProfiles`: inspect configured profiles.

Use the exact runtime signatures documented for the enabled integration version.

## Safe workflow

1. Resolve the integration profile and source/target identities.
2. Inspect existing shares before creating/removing one.
3. Display exact entities/profile/direction for write approval.
4. Use idempotent create/share behavior.
5. Re-read shares and reconcile both systems.

## Proxy guardrails

- Scope requests to the configured integration service and allowed endpoint.
- Never expose a generic user-controlled proxy.
- Validate method/path/body and redact authentication.
- Apply external timeout, rate-limit, and retry policy.
- Do not assume a successful proxy call updated Targetprocess sharing state.

## Automation DevOps service

Automation Rules expose a DevOps service for branch, merge-request, repository, and attached-data operations. Validate integration enablement, repository internal/external ID, branch/ref, permissions, and existing object before writes.

Branch/MR creation and merge are external writes. Use stable naming and existence checks; reconcile ambiguous timeouts before retrying.

## Verification

- No profile/multiple profiles.
- Existing/no share.
- Bulk share partial failure.
- External API non-2xx/rate limit.
- Branch/MR already exists.
- Unlink without deleting external work.
- Permission and repository mapping changes.

## Sources

- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=tp-worksharing-api
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=rules-javascript-devops-functions
