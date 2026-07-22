# API v2 domain fields

Use this reference for custom fields, tags, dates, timeboxes, and relations.

## Contents

- [Custom fields](#custom-fields)
- [Tags](#tags)
- [Dates](#dates)
- [Releases, Iterations, and Team Iterations](#releases-iterations-and-team-iterations)
- [Relations](#relations)
- [Source topics](#source-topics)

## Custom fields

Remove spaces from the custom-field display name when using the direct v2 property:

```text
External Link -> ExternalLink
select={id,name,ExternalLink,Cost,Billed,BillingDate}
```

Use comparisons matching the configured type:

```text
Cost>10
Class=="B"
Browser.Contains("Safari")
Billed==true
BillingDate>Today.AddDays(5)
Hotfix.Id==2338
```

- Numeric and Money fields compare numerically.
- Text and Dropdown fields compare as strings.
- Multiple-selection text commonly uses `Contains`.
- Entity custom fields expose entity properties such as `.Id`.
- Prefer direct properties over deprecated `CustomValues["Field"]` and `CustomValues.Text("Field")` forms.
- A field absent from the queried entity produces a bad request rather than null.

## Tags

- `Tags` is one comma-separated string and is unsuitable for exact membership logic.
- `TagObjects` is a collection of tag objects.

```text
TagObjects.Count==0
TagObjects.Count>0
TagObjects.Count(Name=="plugin")>0
Epic.TagObjects.Count(Name=="plugin")>0
```

Use parent traversal only when that relationship exists on the queried type.

## Dates

Add `isoDate` for ISO 8601 output. The API otherwise may return Microsoft JSON dates.

```text
CreateDate>Today.AddDays(-7)
ModifyDate>=DateTime.Parse("2026-07-01")
```

- Treat date-only literals, instants, and account-local dates differently.
- Normalize output before comparison and state the timezone assumption.
- Encode `+` in offsets as `%2B`.

## Releases, Iterations, and Team Iterations

```text
Release.IsCurrent==true
Iteration.IsPrevious==true
TeamIteration.IsNext==true
Release.InPast(2)
TeamIteration.InFuture(3)
```

- `IsCurrent` means the timebox start is past and end is future.
- `InPast(n)` and `InFuture(n)` can return matches per project. Do not interpret `n` as one global sequence across all projects.
- Confirm whether the tenant uses Release, Iteration, Team Iteration, or renamed terms.

## Relations

Prefer current terminology:

- `Inbound`: main entity.
- `Outbound`: secondary entity.
- `RelationType`: commonly `Link`, `Blocker`, `Relation`, or `Duplicate`.

```text
/api/v2/relations?where=(inbound.id==191261)&select={
  inbound:{inbound.id,inbound.name},
  outbound:{outbound.id,outbound.name},
  relationType:relationType.name
}
```

- Relation `id` is the relation record ID, not either connected entity ID.
- Entity collections include `InboundRelations`, `OutboundRelations`, and legacy `MasterRelations`/`SlaveRelations`.
- Entity-specific helpers such as `MasterFeature` work only when the inbound entity is actually that type.
- Assignable relation helpers can expose `InboundAssignables`/`OutboundAssignables` for state filters.

## Source topics

- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=v2-working-custom-fields
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=v2-working-tags
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=v2-working-dates
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=v2-filtering-by-releases-iterations-team-iterations
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=v2-working-relations
