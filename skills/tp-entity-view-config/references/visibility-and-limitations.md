# Visibility and detailed-view limitations

## Visibility is not security

Hidden fields/tabs remain accessible through APIs and can appear on boards or other views. Use permissions, roles, Direct Access, and Validation Rules for enforcement.

## Project and user roles

```json
"visibilityConfig": {
  "showForProjectRoles": ["Support Person", "Top Manager"]
}
```

```json
"visibilityConfig": {
  "showForUserRoles": ["Developer", "QA Engineer"]
}
```

Project roles and default user roles are different scopes. Register role-name dependencies because names can change.

## Entity condition

`entityQuerySelector` accepts an API v2 selector expression:

```json
"visibilityConfig": {"entityQuerySelector": "checkboxCF==true"}
```

```text
assignedTeams.where(team.id==44).count()>0
entityState.isFinal==false
```

Select only fields/relationships available for the configured entity type. Verify custom-field API name.

## Required practices

```json
"visibilityConfig": {
  "requiredPractices": ["Test Cases"]
}
```

Documented practice names include Times, Bugs, Help Desk, Test Cases, Source Control, Iterations, Features, Epics, and Portfolio Epics. Use actual supported values.

Multiple visibility conditions combine with AND; a user must satisfy every configured condition.

## Limitations

- Custom labels are one-language values; default terms localize automatically, custom labels do not.
- Multiple-Targetprocess-entity custom fields are unsupported.
- Toggl time tracking may not display with detailed-view customization.
- Some old Mashups must migrate to supported components; others remain compatible.
- Renaming a native field only in the layout creates inconsistent naming across other views and is discouraged.

## Mashup migration decisions

Before recreating a legacy Mashup, check whether the new template supports:

- `embeddedPage`.
- Automation Rule Entity Templates.
- Task/Test Case Template component.
- Conditional visibility instead of Hider Mashups.
- External registered React component.

Route external component implementation to `tp-mashups`.

## Sources

- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=customization-customize-entity-detailed-view
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=customization-limitations-entity-detailed-view
