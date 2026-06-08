---
name: tp_rest-api-v2
description: Use when working with Apptio Targetprocess REST API v2, read-only agile metrics, API query construction, selectors, filters, custom fields, tags, dates, releases, iterations, relations, paging, or troubleshooting Targetprocess API responses.
---

# Targetprocess REST API v2

Use this reference when building, reviewing, or querying Apptio Targetprocess integrations, especially read-only tools backed by Targetprocess REST API v2.

## Core Rules

- Prefer REST API v2 for analytics because it is read-only and supports advanced query expressions.
- Never design Targetprocess REST API v2 flows that mutate data; use API v1 only when the user explicitly asks for write behavior.
- Build requests as `GET /api/v2/{entity}` with query parameters such as `where`, `select`, `result`, `take`, `skip`, `orderBy`, `filter`, and date format flags.
- Use entity type names in Targetprocess API form, such as `UserStory`, `Bug`, `Assignable`, `Release`, `TeamIteration`, `Relation`, or account-specific custom entity names.
- Keep selectors explicit. API v2 omits null fields and only returns requested projected fields reliably.
- Name complex selector expressions, for example `lastCommented:Comments.Where(Owner.Kind="User").Max(CreateDate)`, to avoid missing alias errors.
- URL-encode query values. Remember `+` is treated as a space unless encoded as `%2b`.

## Query Patterns

- Projection: `select={id,name,project:{project.id,project.name}}`
- Nested collection projection: `select={id,name,bugs:bugs.where(endDate==null).select({id,name})}`
- Root aggregation: `result={sum:sum(effort),average:average(effort),min:min(effort),max:max(effort)}`
- Nested aggregation: `select={id,name,openedRequests:requests.count(entityState.isInitial==true)}`
- Ordering: `orderBy=effort desc,name`
- Casts for native entities: `assignable.as<UserStory>.feature.name`; do not use casts for extended domain entities.
- First collection item: `comments.select({createDate,owner,description}).first`
- Oldest item pattern: order ascending then use `first`, for example `comments.select({createDate,owner}).orderBy(createDate).first`.

## Paging And Results

- API v2 defaults to 25 items.
- Use `take` and `skip`; documented maximum page size is `take=1000`.
- If responses include `next` or `prev`, treat them as pagination links.
- `result` can return scalar JSON rather than an object with `items`.
- For very large exports, consider the streaming endpoint only if the user specifically needs full-set retrieval: `/svc/tp-apiv2-streaming-service/stream/{entity}`.

## Dates And Timeboxes

- Prefer ISO dates for agent-readable responses.
- Date filters can use `Today`, `Today.AddDays(n)`, and `DateTime.Parse("YYYY-MM-DD")`.
- Current timeboxes use `IsCurrent`, for example `Release.IsCurrent=true`, `Iteration.IsCurrent=true`, or `TeamIteration.IsCurrent=true`.
- Past timeboxes use `IsPrevious` or `InPast(n)`.
- Future timeboxes use `IsNext` or `InFuture(n)`.
- `InPast(n)` and `InFuture(n)` return matches per project when releases or iterations span multiple projects.

## Custom Fields

- Select custom fields by their space-stripped names, for example `External Link` becomes `ExternalLink`.
- Filter custom fields by type-appropriate comparisons:
  - numbers and money: `Cost>10`
  - text and dropdowns: `Class="B"`
  - multi-select text: `Browser.Contains("Safari")`
  - checkbox: `Billed=true`
  - date: `BillingDate>Today.AddDays(5)`
  - entity field: `Hotfix.Id=2338`
- Avoid deprecated `CustomValues["Field"]` and `CustomValues.Text("Field")` unless needed for legacy compatibility.
- If a custom field does not exist on an entity, expect a `BadRequest` stating the property does not exist.

## Tags

- `Tags` is a comma-separated string.
- `TagObjects` is a collection of tag objects.
- Filter untagged entities with `TagObjects.Count==0`.
- Filter tagged entities with `TagObjects.Count>0`.
- Filter by exact tag name with `TagObjects.Count(Name=='plugin')>0`.
- Parent tags can be traversed, for example `Epic.TagObjects.Count(Name=='plugin')>0` on `Feature`.

## Relations

- Relations expose `Inbound`, `Outbound`, and `RelationType`.
- Common relation type names include `Link`, `Blocker`, `Relation`, and `Duplicate`.
- Use relation collections on entities, such as `InboundRelations`, `OutboundRelations`, `MasterRelations`, and `SlaveRelations`.
- Use `/api/v2/relations` when querying relation records directly.
- Relation IDs are not the same as the inbound or outbound entity IDs; use `Inbound.Id` and `Outbound.Id` for connected entities.
- Entity-specific relation helpers may use `Master{EntityType}` and `Slave{EntityType}`, for example `MasterUserStory.Feature`.

## Troubleshooting

- 401 usually means wrong account URL, wrong token, token/access-token mixup, or SSO/API auth constraints.
- 403 means the authenticated user lacks permission for the requested data/action.
- Incomplete API v2 results often come from missing paging, omitted null fields, missing explicit `select`, or mixing API v1 and API v2 syntax.
- Bad requests commonly come from invalid field/entity names, smart quotes, duplicate output names like `select={id,project.id}`, or complex expressions without aliases.
- Check Targetprocess metadata when unsure about real entity and field names; business terminology may differ from API names.

## Official References

- API v2 overview: https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=v2-overview
- Selectors and filters: https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=v2-selectors-filters
- Result, paging, aggregation, streaming: https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=v2-result
- Custom fields: https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=v2-working-custom-fields
- Tags: https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=v2-working-tags
- Dates: https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=v2-working-dates
- Releases, iterations, team iterations: https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=v2-filtering-by-releases-iterations-team-iterations
- Relations: https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=v2-working-relations
- First/last collection element: https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=v2-getting-first-last-element-collection
- REST API troubleshooting: https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=v2-troubleshooting-rest-api-issues
