# History model and query patterns

## Model

A historical record is an entity snapshot associated with a validity period. Reconstruct state by selecting the snapshot whose interval contains the requested instant.

Do not assume:

- One record equals one user action.
- Every current entity type has identical history coverage.
- Deletion, conversion, relations, and custom fields behave like current-state API data.

## Endpoint choice

- Simple History uses supported resources such as `UserStoryHistories`, `BugHistories`, and other documented `*Histories` resources.
- Full History covers native and extendable-domain entities through the history service.
- Query current state with API v2 instead of history.

Inspect the current portal or tenant metadata for the exact resource name before constructing a query.

## Common questions

### Changes by user

Filter historical rows by modifier/user ID and the required entity or time range. Use numeric IDs because names may change.

### Changes since a date

Normalize the date to the tenant/API timezone and filter the history interval or modification field specified by the selected endpoint.

### State at an instant

Select snapshots whose validity interval includes the instant, then project state ID/name. State names may have changed; preserve IDs when available.

### Items updated or closed during a period

Define whether the question means a modification timestamp, entry into a final state, or an end-date change. These are not interchangeable.

### Project-scoped history

Filter through stable project ID. Account for items that moved projects during the period.

## Reconstruction checklist

- Define inclusive/exclusive interval boundaries.
- Sort chronologically using the endpoint's validity/modification field.
- Detect gaps and overlaps.
- Preserve entity, project, state, and modifier IDs.
- Report missing history rather than interpolating.
- Separate direct returned values from derived transitions.

## Sources

- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=record-overview-historical
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=record-simple-history
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=record-full-history
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=record-examples
