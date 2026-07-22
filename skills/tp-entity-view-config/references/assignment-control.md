# Assignment control configuration

The detailed-view assignment component is `assignmentsList`.

## Restrict visible roles

```json
{
  "type": "component",
  "component": "assignmentsList",
  "properties": {
    "allowedRoles": ["Developer", "UX/UI Designer"]
  }
}
```

`allowedRoles` can only show roles present in the entity type's workflow. It uses role names, not IDs, so role renames are dependencies.

## Group and order roles

Create multiple `assignmentsList` components with different `allowedRoles`. Place complete components in the desired order and use separators if needed.

Each list displays Total Effort by default. Hide duplicates on all but the intended group:

```json
"properties": {
  "allowedRoles": ["Product Owner"],
  "hideTotalEffort": true
}
```

## Filter assignable users

`userFilter` accepts documented DSL over user/role data:

```json
"userFilter": "user.firstname != 'Bot' or user.lastname != 'Test-One'"
```

```text
role.name != 'Developer'
user.teamMembers.count(team.name == 'Sun Team') > 0
```

- `role` means scoped Team/Project role where applicable; on global entities/direct access it can mean the user's own role.
- Use `user.role` to explicitly target the user's own role.
- Prefer IDs when supported and names are mutable; `allowedRoles` itself requires names.

## Security boundary

Hiding roles/users changes UI selection, not underlying permissions or API capability. Enforce assignment policy with roles/permissions/Validation Rules.

## Verification

- Every entity workflow and process.
- Project, Team, global extended-domain, and Direct Access scopes.
- Role rename.
- Multiple assignment groups and Total Effort display.
- Empty candidate set and permission-restricted users.
- Existing assignments excluded by the new filter.

## Source

- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=configuration-assignments-control
