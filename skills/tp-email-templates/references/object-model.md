# Notification template object model

## Root objects

- `entity`: business object passed for the notification event.
- `originalEntity`: prior/original entity where the template/event provides it.
- `user`: recipient of the email.
- `tool`: helper/service object.
- Request/Comment context: event-dependent.

Do not assume every object exists in every template.

## Entity and references

The entity can be User Story, Bug, Task, Feature, Request, or another type. It may expose common fields and references such as Priority, Release, Iteration, Assignments, Roles, Comments, and Custom Fields.

Guard:

- Missing/null reference.
- Entity type without the field.
- Recipient lacking permission to open the linked entity.
- Collection absent versus empty.

## User semantics

`user` is the recipient. The modifier/last editor/assignee must be obtained from the appropriate event/entity field, not inferred from `user`.

## Assignments and roles

When iterating assignments, distinguish Assignment, assigned User, Role, and entity. Avoid duplicate recipients/content when one user has multiple roles.

## Comments and descriptions

Comments/descriptions may contain HTML and mention markup. Route rendering through documented Tool methods; never inject raw HTML as plain trusted content.

## Custom fields

Use the template engine's documented custom-field access form and guard absent field/type. Custom-field names are tenant dependencies.

## Interactive data model

Use the portal's interactive model/reference to confirm fields for the exact template and entity type. Do not generalize a field observed in one notification to all templates.

## Sources

- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=notifications-entity-user-comment
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=notifications-markup-guide
