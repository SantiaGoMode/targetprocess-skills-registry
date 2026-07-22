# `portfolioAssignments`

`portfolioAssignments` displays related entity groupings such as Teams, Agile Release Trains, and Solution Trains, with optional filtering and read-only behavior.

## Schema

Customize through `schemaConfig` with `entityListMetadata` and `selectorConfig`.

Example metadata shape:

```json
{
  "agileReleaseTrains": {
    "entityTypeName": "agilereleasetrain",
    "query": "agileReleaseTrain?select={id,name}&orderby=name"
  },
  "teams": {
    "entityTypeName": "team",
    "filterBy": [{
      "selected": "agileReleaseTrains",
      "on": "agileReleaseTrainId",
      "allowUnassigned": true
    }],
    "query": "team?select={id,name,agileReleaseTrainId:agileReleaseTrain.id,icon:emojiicon}&where=isActive==true&orderby=name",
    "iconComponent": "TeamIcon"
  }
}
```

## Attribute contracts

- Metadata key: stable identifier referenced by other configuration.
- `entityTypeName`: lowercase API entity type.
- `query`: API v2 path/query. Select at least `id` and `name`, plus every filter/display field.
- `filterBy`: array of cross-group rules.
- `selected`: metadata key supplying selected entity ID.
- `on`: selected property compared with that ID.
- `allowUnassigned`: include candidates whose `on` property is null.
- `allowClickable`: make displayed entity names openable.
- `displayName`: custom label; otherwise terms are used.
- `displayOrder`: lower values display earlier; default is 0.
- `selectorConfig`: controls how assignments are selected/stored; verify the exact portal schema for nonstandard entities.

## Design rules

- Keep metadata keys stable.
- Ensure query aliases exactly match every `filterBy.on`.
- Define what happens when a parent selection changes and a child becomes invalid.
- For nonstandard entities, confirm connection multiplicity and auto-assignment semantics.
- Use read-only mode when the view should display but not mutate assignments.

## Verification

- Teams only, Teams+ART, and Teams+ART+Solution Train variants.
- Parent selected/unselected/changed.
- `allowUnassigned` true/false.
- Inactive and permission-restricted entities.
- Nonstandard extended-domain entity.
- Click behavior and display order.
- API v2 query cost.

## Source

- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=customization-setting-up-portfolioassignments
