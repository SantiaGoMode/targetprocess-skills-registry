# Validation Rules

Validation occurs before persistence. A filter expression evaluating to `true` means validation fails and the transaction is rolled back.

## Triggers

- Created: new entity.
- Updated: existing fields change; restrict to relevant changed fields where possible.
- Deleted: version-dependent.

Collection changes are child operations, not parent updates. To validate assigning a User Story to a Feature, validate User Story Updated with `Feature` among changed fields.

## DSL context

- `$Author.Id`
- `$Author.IsAdmin`
- `$Author.ScopedRole.Id` / `.Name`
- `$Previous`
- `$ChangedFields`
- `$Modification`
- `IsBeingDeleted()`

Example failure conditions:

```text
EntityState.IsFinal == true
Budget > $Previous.Budget
$ChangedFields.Contains("Budget") and $Author.IsAdmin == false
```

`$Previous` supports at most two traversal levels and no collections/aggregations. It is unavailable/meaningless for Created as a prior entity state.

Use `IsBeingDeleted()` to permit a reference becoming null because the referenced entity is being deleted while still blocking a direct user reassignment/removal.

Custom fields containing spaces or symbols use square brackets:

```text
[Expected Budget]
Feature.[Total Time]
```

## Author-aware policy

Distinguish:

- User's default/global role.
- Scoped project/team role.
- Assignment to the entity.
- Administrator status.

Avoid ID/name constants when a role can be renamed; if names are unavoidable, register the dependency.

## Enforcement caveat

Standard user PAT operations should respect Validation Rules. System-level Automation Rules, Metrics, sync-v2 integrations, and System User token operations may bypass them by design. Test every privileged path; do not claim universal enforcement.

## Test matrix

- Create and update through UI.
- Same operations through a standard-user PAT.
- Admin and non-admin.
- Allowed and blocked scoped roles.
- Empty and prior values.
- Collection/reference deletion.
- Import, Automation Rule, Metric, sync, and System User path.
- Clear error message returned to the user/integration.

## Sources

- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=rules-validation
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=rules-examples
