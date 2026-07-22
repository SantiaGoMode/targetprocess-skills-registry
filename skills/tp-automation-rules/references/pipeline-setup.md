# Automation Rule pipeline setup

## Pipeline model

Every rule contains one source, zero or more filters, and one or more actions. All filters must pass before actions execute.

## Sources

### Modified Entity

Select entity type and Created, Updated, or Deleted. For Updated, restrict modified fields to those that can affect the outcome; unrelated fields create needless runs and loop risk.

Common timing trap: optional fields may not be populated when a Created event fires. Move that condition to a later update or query current data if required.

### Incoming Webhook

Create a webhook endpoint and select GitHub/GitLab only for tailored suggestions; `Other` accepts any compatible sender. Treat suggestions as configuration help, not authentication.

Define signature/authentication, content type, body schema, delivery ID, retry behavior, ordering, and replay window before enabling actions.

### Time Interval

Public SaaS permits intervals from 15 minutes to 90 days. JSON may include `startTimeUtc` using an explicit UTC timestamp. Define timezone/calendar semantics and prevent overlapping logical work.

### Named Trigger

Use a `source:uniqueName` JSON block for reusable internal activation. See commands-and-services for activation rules.

## Filters

### Team and Process

- Team/Process filters apply to Modified Entity sources.
- Global extended-domain entities, Assignments, Team Assignments, and Comments may not have Process.
- Distinguish assigned Team from Responsible Team.

### Field values

- Groups are ORed; conditions inside a group are ANDed.
- Strings support equality/existence/contains forms; numbers support comparison operators.
- Values may be constants or referenced fields.
- Use `Now()` for current date comparison where documented.
- For a Comment on work item 123, filter the parent `General.Id`, not the Comment's own `Id`.

### JavaScript filter

Return only boolean `true` or `false`. Query extra data only when the event snapshot is insufficient.

## Actions

Supported UI actions include:

- Update Entity.
- Create Entity.
- Create Entity and add as Relation.
- Move Entity to State.
- Add Comment.
- Execute JavaScript.
- Send HTTP Request.

Related-entity targeting is available only for documented actions/sources. Resolve reference values and process-specific states explicitly.

## Raw JSON

The Raw JSON view represents the pipeline and enables blocks not yet represented in the UI. Copying JSON between tenants does not translate IDs, processes, states, custom fields, URLs, or parameters.

Before import/activation, replace tenant-specific values and review:

- Source entity and modifications.
- Selected changed fields.
- Team/process filters.
- Every state/role/project/custom-field name or ID.
- Webhook/HTTP endpoints and secrets.
- Active status and parameters.

No supported API exists for bulk rule deployment or inventory. Use JSON as reviewed configuration documentation.

## Sources

- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=rules-setup
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=setup-sources
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=setup-filters
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=setup-actions
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=setup-other
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=setup-how-apply-raw-json-from-examples
