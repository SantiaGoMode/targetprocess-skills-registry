# API v1 entity reference and metadata

## Prefer concrete resources

The portal provides dedicated GET/POST/DELETE references for:

- `UserStories`
- `Bugs`
- `Epics`
- `Features`
- `Tasks`
- `Requesters`
- `Users`
- `Projects`

Use `Assignables` only for genuinely polymorphic work-item reads/operations and `Generals` only when the task truly spans the broader base type. Concrete endpoints give clearer fields, required data, and behavior.

## Metadata-first method

Query `/api/v1/Index/meta` or the entity-specific metadata endpoint to determine:

- Real resource/entity type name.
- Native fields and data types.
- Required fields.
- Reference and collection fields.
- Custom fields and configured types.
- Whether create, update, or delete is supported.

Do not infer API fields from UI labels or process terms.

## Endpoint checklist

For the selected entity reference, capture:

1. Collection and item paths.
2. Supported GET variants and query parameters.
3. Required create fields.
4. Writable versus read-only properties.
5. Delete/unassign semantics.
6. Related resources and permissions.
7. Validation or version caveats.

## Generic ID lookup

When only a numeric entity ID is known, use the documented general/entity-type discovery pattern before selecting a concrete endpoint. Never guess the type from an ID range or UI URL label.

## Undelete references

The API Reference includes `/DeletedItems` and `/Restore`, but restore eligibility is narrower and version-dependent. Route actual recovery work through `tp-integrations` and verify the current Undelete documentation.

## Sources

- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=api-reference
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=reference-userstories-api
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=reference-bugs-api
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=reference-epics-api
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=reference-features-api
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=reference-tasks-api
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=reference-requesters-api
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=reference-users-api
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=reference-projects-api
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=reference-assignables-api
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=reference-generals-api
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=undelete-deleteditems
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=undelete-restore
