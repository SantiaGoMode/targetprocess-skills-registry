# API v1 querying

## Resource model

- Entity: Targetprocess object such as User Story, Bug, Project, or Iteration.
- Nested entity: referenced object included inside another resource.
- Collection: endpoint returning entities.
- Inner collection: child collection such as Tasks or Assignments.

Use real API resource names rather than customized process terms.

## Format and response shaping

- API v1 supports XML and JSON; request JSON explicitly with `format=json` or the documented Accept header.
- JSONP exists for legacy browser interoperability; do not choose it for modern integrations.
- `include` adds fields/related data and `exclude` removes defaults.

```http
GET /api/v1/UserStories?format=json&include=[Name,Project]&exclude=[Description]
```

Default resource fields are not a stable integration contract. Request what the client needs.

## Paging

Collections default to 25 items:

```http
GET /api/v1/Bugs?format=json&take=20&skip=10
```

Page root and inner collections deliberately. Stop only when the page/result semantics prove completion.

## Filters and sorting

```text
where=EntityState.Name eq 'Done'
```

API v1 filtering supports fields, nested fields, custom fields, tags, and documented operators. It is not API v2 expression syntax.

- Quote strings exactly.
- Use IDs when names may be duplicated or renamed.
- Combine only operators supported by v1.
- Apply sorting explicitly when paging or rank matters.

## Appends and calculations

`append` adds calculated values, including counts/aggregates over nested collections:

```text
append=[Tasks-Count]
```

For new analytical work, prefer API v2 selectors/results unless a v1 append is specifically required.

## Context

Use the Context resource for:

- Logged-in user and culture.
- Date, time, and number formats.
- Selected projects/processes.
- Process details for an entity.

Do not assume browser-selected context equals the intended integration scope.

## Query review checklist

- Correct singular/plural resource endpoint.
- Explicit JSON format and fields.
- Complete paging.
- Stable sort for page traversal.
- v1 filter syntax only.
- Custom-field and tag form verified against tenant metadata.
- Permissions considered when counts differ from UI expectations.

## Sources

- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=v1-resources-collections
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=v1-response-formats
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=v1-paging
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=v1-sorting-filters
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=v1-partial-response-includes-excludes
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=v1-appends-calculations
