# Auxiliary APIs

## RESTful Storage

Client Storage provides custom groups/storages with get/list, put/post, delete, and query operations.

Before use define:

- Owning integration/Mashup.
- Namespace/group and key scheme.
- Value schema/version.
- Maximum expected size/volume.
- Retention/deletion.
- Migration and backup.
- Access/permission expectations.

Do not use it as an undocumented source of truth for core business entities.

## Validate Users and Requesters

The portal notes these endpoints do not require authentication. Treat them as validation/lookup only, never authentication or authorization.

- Minimize returned/exposed personal data.
- Rate-limit and validate input.
- Do not reveal whether arbitrary sensitive identities exist when avoidable.
- Confirm exact User versus Requester semantics.

## Conversion API

Use General Conversions to find:

- New entity ID/type from an old converted entity.
- Old entity ID/type from the resulting entity.

Conversions occur through the supported Targetprocess UI/workflow; the lookup API observes lineage and does not itself authorize conversion.

## Undelete

Availability depends on version/entity:

- Projects and Users have documented deleted-item/restore paths.
- Later versions add some native and extended-domain entities.
- Some entity types cannot be undeleted.
- Newer UI versions may restore work items without direct API use.

Safe recovery:

1. Query deleted items and resolve exact stable ID/type/name/date.
2. Confirm restore support and destination dependencies.
3. Display the exact target and require approval.
4. Restore once.
5. Re-read entity, references, permissions, and automations.

Do not repeatedly retry an ambiguous restore without checking current state.

## Sources

- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=apis-restful-storage
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=apis-validate-users-requesters
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=apis-conversion-api
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=apis-undelete-api
