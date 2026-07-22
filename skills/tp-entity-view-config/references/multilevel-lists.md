# Multilevel-list customization

Use flexible multilevel lists when a detailed view must render multiple hierarchy levels with type-specific resources, filters, ordering, or visual encoding.

## Contents

- Prerequisites
- Configuration model
- Level design
- Filters and ordering
- Visual encoding
- Performance and correctness

## Prerequisites

- Confirm the feature/version is available.
- Define the root context entity and every level relationship.
- Bound maximum depth and expected branching.
- Decide whether repeated entities/cycles can occur through relations.

## Configuration model

The portal separates:

- Flexible list definition.
- `levels` describing hierarchy/query transitions.
- Resource item definition describing each rendered entity.
- Filter.
- Ordering.
- Visual encoding.

Keep each level explicit. Do not use a broad polymorphic query when a concrete entity type is known.

Base component:

```json
{
  "type": "component",
  "component": "embedded.flexible.list",
  "properties": {
    "name": "User Stories (flexible)",
    "origin": "feature/user-stories-flexible",
    "listDefinition": {
      "levels": [{
        "id": "level-0",
        "buckets": [{
          "items": [{
            "id": "userstory",
            "type": "Resource",
            "resourceType": "UserStory"
          }],
          "filter": "?Feature.Id==${{Id}}$"
        }]
      }]
    }
  }
}
```

- `component` is always `embedded.flexible.list`.
- `name` identifies the list in diagnostics.
- `origin` is the global Comet-notification/log identity; keep it unique even though duplicates do not directly control flow.
- `listDefinition.levels` describes hierarchy. The documented current maximum is 10 levels.
- Every level requires unique `id` and `buckets`.
- Every bucket requires `items`; optional behavior includes `itemPaths`/back paths, `filter`, `ordering`, and color.

## Level design

For each level document:

1. Source entity type.
2. Child/reference selector.
3. Fields required for display, filter, order, and encoding.
4. Allowed actions/click behavior.
5. Empty and permission-filtered behavior.
6. Whether an entity may appear more than once.

## Filters and ordering

- Use actual entity fields and custom-field names.
- Keep filter scope at the level it logically applies to.
- Define deterministic secondary ordering.
- Test null sort keys and mixed entity types.

Filters are Board/List DSL and must begin with `?`; otherwise the value is treated as name text search. The bucket target type is the common base of its resource items. A field absent on one concrete type evaluates false for that type.

Ordering shape:

```json
{"name": "Creation Date", "direction": "Desc"}
```

`direction` defaults to `Asc`. A predefined ordering or simple non-reference field must be valid for every item type in the bucket.

Resource items require `resourceType`; native and extended-domain types are supported. Multiple item types must share a common base type. Use `/api/v1/index/meta` to verify inheritance. Optional `cardUnits` contains unit IDs such as `entity_name_1line` and `general_entity_id`.

## Visual encoding

Keep color/encoding rules orthogonal to access and filtering. Provide an accessible text/state cue in addition to color. Define a default for unmatched/null data.

```json
"color": {
  "combine": true,
  "cases": [
    {"filter": "?Name.Contains(':(')", "value": "\"fbe0e0\""},
    {"filter": "?Name.Contains(':)')", "value": "\"e3f5e6\""}
  ]
}
```

Use separate buckets plus documented item back paths when the child types depend on the parent type.

## Performance and correctness

- Estimate worst-case nodes as branching raised across levels.
- Avoid deep selectors that load unused fields.
- Verify lazy/expanded behavior in the actual UI.
- Test cycles and duplicate paths when relations drive levels.
- Confirm permissions at every level; parent visibility must not imply child access.

## Source

- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=evc-advanced-ustomization-multilevel-list-customization-entity-detailed-view
