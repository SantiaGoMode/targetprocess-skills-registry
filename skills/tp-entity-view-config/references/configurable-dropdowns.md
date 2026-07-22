# Configurable Dropdowns

Configurable Dropdowns are registered by Mashup code and can then be placed in Detailed Views, Lists, and Quick Add.

## Registration

```javascript
tau.mashups
  .addDependency('tau/api/configurable-controls/controls/v1')
  .addMashup((controlsApiV1) => {
    controlsApiV1.addConfiguration('dropdown', {
      id: 'program_increment_for_art',
      name: 'Program Increments of the assigned ART',
      label: 'Program Increment',
      supportedEntityTypes: ['feature'],
      requiredEntityFields: ['id', 'name', 'release'],
      sampleData: {release: {name: 'Program Increment'}},
      entityClickBehavior: 'openEntity',
      field: 'release',
      dropdownItemsSource: {
        type: 'interpolatedQuery',
        query: 'release?where=(agileReleaseTrain.id==${{agileReleaseTrain.id}}$)&orderBy=name desc'
      }
    });
  });
```

Use a global placeholder such as the documented footer placeholder for registration; placement of the resulting control is configured separately.

## Attributes

- `id` (required): unique lowercase-oriented ID among dropdown controls.
- `name`: administrator-friendly description.
- `label` (required): Detailed View label/List column header.
- `supportedEntityTypes`: lowercase target types; omitted means universal.
- `allowedLocations`: booleans for `listUnit`, `detailedView`, and `quickAdd`.
- `requiredEntityFields`: fields/aliased v2 selectors needed to render; Entity custom fields require this.
- `sampleData`: matching dummy shape for editors/previews.
- `isEnabled`: conditional enablement according to documented config context.
- `entityClickBehavior`: for example `openEntity`.
- `field`: native/custom field being edited.
- `allowReset`: whether value can be cleared.
- `dropdownItemsSource`: item provider, commonly interpolated API v2 query.
- `showSearch`: search control behavior.

## Required fields

Fields are combined into one API v2 selector and may include aliases/nested collections:

```javascript
requiredEntityFields: [
  'id',
  'name',
  'feature:{id:feature.id,name:feature.name,projectId:feature.project.id}',
  'tasks:tasks.select({id,name})'
]
```

`sampleData` must mirror that shape so editors render meaningful previews.

## Interpolated queries

- Validate every interpolation field is requested.
- Treat interpolation as data, not arbitrary expression injection.
- Use stable IDs for relationships.
- Select fields needed for display/click behavior.
- Bound result count and define ordering/search behavior.

## Placement and verification

Test the same configuration separately in Detailed View, List, and Quick Add. Verify editor preview, actual entity data, empty source, current value excluded from candidates, permissions, reset, click behavior, search, and query performance.

Because registration is Mashup code, use `tp-mashups` for lifecycle/compatibility and this reference for the control contract.

## Source

- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=configuration-configurable-dropdown
