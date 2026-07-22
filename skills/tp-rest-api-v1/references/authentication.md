# API v1 authentication

## Authority model

- REST access requires an active Targetprocess User or System User. Requester accounts cannot use the API.
- API requests inherit the authenticated user's project, team, role, and entity permissions.
- A PAT is not independently scopeable as read-only. Use a dedicated read-only service account when read-only authority is required.

## Apptio OpenToken

For Frontdoor-managed environments:

1. Create an API key with access to the required environment.
2. POST the key access/secret to the regional Frontdoor `/service/apikeylogin` endpoint.
3. Read the returned `apptio-opentoken` response header and its expiry.
4. Send that value in the `apptio-opentoken` header to API v1 or v2.

Never log the API key secret or returned token.

## Personal Access Tokens

- Users create PATs in **Settings → Authentication and Security → Personal Access Tokens**.
- The user's Default Role must allow creating and using access tokens.
- Administrators with that permission can review issued-token owner, name, issue date, and last usage and can revoke tokens; they cannot create a PAT for another user.
- PATs are password-independent and can be application-specific.
- The portal documents `access_token={token}` as a query parameter. Redact full URLs and prefer a supported header-based credential mechanism when the environment provides one.

## Legacy service/resource tokens

- Retrieve through `GET /api/v1/Authentication` for the logged-in user.
- An administrator can request another user or System User token with `?login=`.
- They are tied to the login/password and do not provide multiple application-specific tokens.
- The System User has administrator-equivalent authority; use only as an explicit exception.

## Cookie and Basic Authentication

- Browser/Mashup same-origin requests may use the logged-in user's cookie.
- Basic Authentication is not recommended for SSO/Frontdoor instances and is blocked when the login form is disabled.
- Never use Basic Authentication without HTTPS. Base64 is encoding, not encryption.

## Verification and errors

- `GET /api/v1/users/LoggedUser/` verifies the authenticated identity and role.
- `401`: wrong account, missing/expired/revoked token, inactive user, PAT role permission, SSO mode, or unset System User password.
- `403`: identity is valid but lacks data/operation permission.

## Sources

- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=v1-authentication
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=practices-managing-automation-rules-validation-rules-scale
