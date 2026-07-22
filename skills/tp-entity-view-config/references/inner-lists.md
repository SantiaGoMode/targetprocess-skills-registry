# Inner-list customization

Use for lists embedded in an entity detailed view. Do not use multilevel-list syntax unless hierarchy/flexible levels are required.

## Select list model

Identify whether the list contains:

- Native child entities.
- Extended-domain entities through a one-to-many reference.
- Multiple entity types.
- An arbitrary entity query rather than a built-in child collection.

The containing entity, child relationship, and process/entity type determine available fields and actions.

## Column identifiers

Prefer the Targetprocess Diagnostic Report to discover stable column IDs. Browser Inspect Element is a fallback and can expose implementation details that change.

Do not infer a column ID from its visible label.

## Configuration concerns

- Define list component/type and relationship.
- Add native fields with documented column identifiers.
- Add custom fields using the documented custom-field column form and exact configured name.
- Set explicit ordering where sequence matters.
- Configure a tab counter only from a bounded/relevant collection.
- For mixed entity types, include only fields valid across types or define type-specific behavior.

## Search and finder model

The portal documents `finderModel` errors for some custom inner lists. Distinguish:

- Search component/configuration error.
- Empty query result.
- Permission-filtered result.
- Unsupported entity/list combination.

Do not suppress an error and present it as an empty list.

## Verification

- Zero, one, and many children.
- Permissions hiding some children.
- Deleted/moved child.
- Custom field absent/null.
- Column sort and tab count agree.
- Search after navigation and refresh.
- Production-scale row volume.

## Source

- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=customization-advanced-ustomization-customize-inner-lists
