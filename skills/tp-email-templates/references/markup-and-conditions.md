# Notification markup and conditions

## Conditions

Use the documented `if` / `elseif` / `else` syntax for process-, project-, entity-, role-, or recipient-specific content.

Design every conditional with:

- Null guard before nested access.
- Explicit comparison type.
- Fallback content.
- Minimal nesting.

Do not use visibility conditions as a substitute for permissions.

## Subjects

Subject templates must produce one line. Remove/newline-proof dynamic values and keep the result concise. Do not place raw descriptions/comments in the subject.

## Common example intents

- Last editor: use the actual modification/editor field.
- Assigned Users: iterate assignments and format names/roles without duplicates.
- Custom Fields: use exact configured name and type guard.
- Terms: use the Tool term helper for customized process terminology.
- Online URL: use the Tool URL/online-link helper and recipient permissions.

## Safe output

- Escape plain dynamic text.
- Process HTML fields with supported Tool methods.
- Avoid secrets, raw payloads, and internal-only IDs unless explicitly required.
- Keep content meaningful when optional related entities are missing.
- Ensure Requester-facing content does not expose internal fields/links.

## Test matrix

- Each entity/event type using the template.
- Internal User and Requester recipient.
- Missing relation/custom field/comment.
- Different process terms/locales.
- User with/without access to entity link.
- Subject and body in actual delivered email client.

## Sources

- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=notifications-conditions
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=notifications-examples
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=notifications-templates
