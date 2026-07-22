# Embedded Roadmap configuration

Use when adding/configuring a Roadmap inside an entity detailed-view tab.

## Contents

- Configuration dimensions
- Workflow
- Query and UI rules
- Performance
- Verification

## Configuration dimensions

- Tab/component placement.
- Display period.
- Card entity types.
- Card filter.
- Navigation time span.
- Vertical and horizontal swimlanes.
- Card size.
- Card template.
- Hover template.
- Milestones.
- Visual encoding.

Core properties:

```json
{
  "cardTypes": ["userstory", "bug"],
  "startDateSelector": "PlannedStartDate",
  "endDateSelector": "PlannedEndDate",
  "cardFilter": "?Feature.Id is ${entity.id}",
  "dateScale": "quarter",
  "horizontalLane": "priority",
  "hideEmptyLanes": true,
  "zoomLevel": "s",
  "globalDateRangeCalculation": {
    "daysBeforePlannedStartDate": 40,
    "daysAfterPlannedEndDate": 75
  },
  "showProjectMilestone": true,
  "milestoneProviders": []
}
```

Embedded Roadmaps show no cards by default; `cardTypes` and related properties must be configured.

## Workflow

1. Add the documented Roadmap component/tab to the entity template.
2. Define the entity context relationship, for example Features belonging to the opened Epic.
3. Set a bounded default period and navigation period.
4. Select card types and required fields.
5. Add filter and swimlane selectors.
6. Configure card/hover content and visual encoding.
7. Add milestones only with a clear query/source.

## Query and UI rules

- Use stable IDs/context selectors rather than display-name matching.
- Request fields required by filters, lanes, cards, hover, milestones, and encoding.
- Keep card/hover templates concise; missing fields need fallbacks.
- Provide a default lane/encoding for null/unmatched values.
- Color cannot be the only state indicator.

Date selectors use API v2-style expressions and default to the current entity's Planned Start/End dates when omitted:

```json
{
  "startDateSelector": "StartDate.AddMonths(-1)",
  "endDateSelector": "EndDate.AddDays(10)"
}
```

Collection-derived example:

```json
{
  "startDateSelector": "Bugs.OrderBy(PlannedStartDate).First.PlannedStartDate",
  "endDateSelector": "Bugs.OrderByDescending(PlannedEndDate).First.PlannedEndDate"
}
```

Supported documented `cardTypes` include bug, userstory, epic, feature, impediment, iteration, portfolioepic, project, release, request, task, teamiteration, testplan, and testplanrun.

`dateScale` values include none, halfyear, iteration, month, plannedenddate, quarter, release, teamiteration, and week. `horizontalLane` has a larger documented catalog including entity type, owner, assigned user, project, release, state, team, priority, and time periods; use the current UI/network payload to confirm exact supported value.

Card layout uses per-type rows of unit IDs:

```json
"cardSettings": {
  "userstory": [
    [{"id": "general_entity_id", "alignment": "base"},
     {"id": "project_abbr", "alignment": "alt"}],
    [{"id": "entity_name_small_sizes"}]
  ]
}
```

Use `hintCardSettings` for hover cards and discover unit IDs through the supported card/inner-list tooling.

## Performance

Bound period, entity types, filter scope, lanes, and card count. Test the real production data distribution; a small demo Epic is not representative.

## Verification

- Open different parent entities without full refresh.
- Empty and high-volume Roadmaps.
- Cards crossing period boundaries.
- Permission-filtered cards/milestones.
- Drag/drop or editing behavior if enabled.
- Navigation period and timezone/date rendering.
- Card/hover content with null custom fields.

## Source

- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=customization-embedded-roadmap-view
