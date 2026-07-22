# API v2 query language

Use this reference for selectors, expressions, filters, ordering, casts, and collection traversal.

## Request shape

```http
GET /api/v2/{entity}/{id}?
  where={where}&select={select}&result={result}&
  take={take}&skip={skip}&orderBy={orderBy}&filter={filter}&isoDate
```

- `{entity}` is the real singular API type such as `UserStory`, `Bug`, or `Assignable`.
- `{id}` is optional and still returns an `items` collection for ordinary entity queries.
- `where` is a .NET-like boolean expression.
- `filter` is Targetprocess board-filter DSL; `filter` and `where` are combined with `and`.
- `select` projects items; `result` calculates over the root collection.

## Selectors and aliases

```text
select={id,name,project:{project.id,project.name}}
select={id,storyName:name,taskCount:tasks.count()}
```

- Request the full property path for related fields.
- Use a valid alphanumeric/underscore alias before `:` for every complex expression.
- Do not emit ambiguous names such as `{id,project.id}`; both infer `id`.
- An entity requested as a field normally contains only its basic identity fields; explicitly project deeper data.

## Nested collections

```text
select={id,name,bugs:bugs.select({id,name,effort})}
select={id,name,openBugs:bugs.where(endDate==null).select({id,name})}
select={id,name,bugEffort:bugs.sum(effort)}
select={id,name,openRequestCount:requests.count(entityState.isInitial==true)}
```

Apply `where`, `select`, `count`, `sum`, `average`, `min`, or `max` to collections. Keep the returned projection small even when the filter traverses deeply.

## Filtering expressions

- Use `==`, `!=`, `<`, `>`, `<=`, `>=`, boolean operators, null checks, and supported string/collection methods.
- Use `it` when filtering a sequence of scalar values:

```text
select={id,name,largeEfforts:userStories.select(effort).where(it>5)}
```

- Use real API names rather than process terms.
- Encode quotes, braces, spaces, plus signs, and operators as URL query values.

## Ordering

```text
orderBy=effort desc,name
select={id,name,epics:epics.orderByDescending(name).select(name)}
```

- Root order defaults ascending unless `desc` is specified.
- Separate secondary sort keys with commas.
- Collection items default to descending ID; call `orderBy(...)` or `orderByDescending(...)` when order matters.

## First and last collection items

The documentation uses `first` for the first projected item. Make ordering explicit:

```text
recent:comments.select({createDate,owner,description}).orderByDescending(createDate).first
oldest:comments.select({createDate,owner,description}).orderBy(createDate).first
```

Do not rely on collection default order for oldest/newest semantics.

## Casts

Use casts only for native polymorphic entities:

```text
assignable.as<UserStory>.feature.name
```

Do not use casts for extended-domain entities. Query the extended entity type directly or use its actual exposed properties.

## Output semantics

- Response properties are camelCase.
- Null properties are omitted; an empty related projection may appear as `{}`.
- Explicitly alias calculations so response parsing is stable.

## Source topics

- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=v2-overview
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=v2-selectors-filters
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=v2-getting-first-last-element-collection
