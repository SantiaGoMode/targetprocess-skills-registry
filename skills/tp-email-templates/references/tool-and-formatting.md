# Tool helpers and formatting

## Important methods

- `Tool.Url(entity, user)`: permission-aware entity view/edit URL for the recipient.
- `Tool.Term(entity_name)`: convert an API/default entity name to customized Default-process terminology.
- HTML processing/sanitizing method: convert stored HTML descriptions to email-safe absolute links/paths.
- Mention-style method: transform Targetprocess mention markup into styled output.
- `Tool.ShowOnlineLink(user)`: Comment-created-template check for Requester/Service Desk link availability.

Use the exact method names/casing from the current template runtime; the portal's rendered table contains the authoritative contract.

## Fields

The Tool object exposes environment paths/settings such as application and Service Desk paths. Some legacy fields, including company-name storage, are deprecated; prefer current supported methods/settings or explicit approved text.

## Descriptions

Descriptions are HTML, not safe plain text. Process them so relative Targetprocess paths become absolute and only supported markup reaches the email. Do not double-escape already processed output.

## Mentions

Stored mention tags require the Tool mention helper for readable/styled email output. Define fallback text for unresolved users.

## Dates

Format dates using the documented template formatting and recipient/account locale expectations. State timezone for absolute instants; distinguish date-only fields.

## Links

- Build entity links through `Tool.Url` rather than concatenating account paths.
- Use recipient identity for permission-aware result.
- For Requesters, check Service Desk availability before showing an online link.
- Sanitize external custom-field URLs.

## Sources

- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=notifications-tool
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=notifications-formatting-values
