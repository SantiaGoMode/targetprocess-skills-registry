# Detailed-view basics

## Template discipline

Preserve the current entity/process template before editing. Existing `componentId` values identify elements and should remain stable when editing/moving the same component. Generate/accept new IDs only according to the editor/runtime contract for new elements.

## Tabs

A tab/section combines a title and component:

```json
{
  "title": {"type": "string", "value": "Change Log", "localize": true},
  "component": {
    "type": "component",
    "component": "auditHistory",
    "componentId": "component_uovrhva"
  },
  "componentId": "section_a6ede8w"
}
```

- Rename by changing `title.value`.
- Move by relocating the entire section in the `sections` array.
- Remove by deleting the complete section object.
- Use the documented `selected` property for a default tab.
- Do not copy example component IDs into an unrelated template.

## Embedded page

```json
{
  "title": {"type": "string", "value": "Google Doc"},
  "component": {"type": "embeddedPage", "customFieldName": "Doc"}
}
```

The referenced URL/Template URL custom field must exist and contain an allowed URL. Review external-content security and framing restrictions.

## Custom fields

```json
{
  "type": "property.customField",
  "properties": {"name": "Extra"}
}
```

Rename display label only:

```json
{
  "type": "property.customField",
  "properties": {
    "name": "Custom Field Name",
    "label": {"type": "string", "value": "Updated Name", "localize": true}
  }
}
```

- The custom field must already exist in Settings.
- Targetprocess Multiple Entity custom fields are unsupported here.
- Rich-text custom fields need adequate main-column space.

## Field properties

- Read-only: set `"editable": false`.
- Prevent clearing: set `"allowReset": false`.
- Hiding by deleting a layout object changes UI only, not API visibility or permissions.

## Trigger button

Use the documented `automationRuleTriggerButton` component to manually launch a rule. Resolve the rule/trigger configuration and confirm user-visible failure behavior; the request runs in the background.

## Layout components

Use documented grid, stack, collapsible, separator, spacing, and text components. Preserve type/component structure; do not wrap a component arbitrarily. Test narrow/wide layouts and long labels/content.

## Embedded boards, Roadmaps, Flow, and templates

- Flow on extended-domain entities requires the documented minimum version and `implementationHistory` component.
- Route advanced Roadmap setup to `embedded-roadmaps.md`.
- Verify embedded Board context/filter behavior.
- Task/Test Case template components require the matching process/practice.

## Source

- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=customization-customize-entity-detailed-view
