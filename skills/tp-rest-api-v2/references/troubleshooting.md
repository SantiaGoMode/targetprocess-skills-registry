# API v2 troubleshooting

## Diagnostic order

1. Capture the exact account URL, endpoint, decoded parameters, status, and response body with credentials removed.
2. Reduce to `select={id,name}&take=1` on the same entity.
3. Add `where`, nested paths, calculations, and ordering one at a time.
4. Check `/api/v1/Index/meta` or tenant metadata for real entity and field names.

## Status classes

### 401 Unauthorized

Check account hostname, token versus access token, expired/revoked PAT, inactive user, missing PAT role permission, SSO/Frontdoor requirements, and System User setup.

### 403 Forbidden

Authentication succeeded, but the user lacks access to the entity, project/team scope, or operation.

### Bad request / 500-style API error

Common causes:

- Wrong entity or field name.
- Business term used instead of API name.
- Smart quotes or malformed expression.
- Duplicate inferred output names such as `{id,project.id}`.
- Complex selector without an alias.
- Custom field absent on the entity.
- API v1 `where` syntax used in v2.
- `+` decoded as a space instead of `%2B`.
- Cast applied to an extended-domain entity.

## Incomplete or surprising results

- Only first 25 returned: paging omitted.
- Field missing: it was null or not explicitly selected.
- Related object only has ID/name: deeper projection omitted.
- Timebox count unexpected: `InPast`/`InFuture` evaluated per project.
- Tag false positive: combined `Tags` string used instead of `TagObjects`.
- Relation ID mismatch: relation record ID confused with inbound/outbound entity ID.
- Oldest/newest wrong: collection order not made explicit.

## Reporting a diagnosis

Return the minimized failing query, corrected query, decoded expression, tenant metadata assumption, expected response shape, and whether the conclusion was verified live.

## Source topic

- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=v2-troubleshooting-rest-api-issues
