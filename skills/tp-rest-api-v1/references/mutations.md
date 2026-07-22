# API v1 mutations

## Preflight

Resolve the exact entity type, IDs, required fields, reference shapes, permissions, validation rules, and idempotency key. Display method, endpoint, and payload before executing.

## Create

POST a JSON representation to the collection URL:

```http
POST /api/v1/Projects
Content-Type: application/json

{"Name":"Example"}
```

- Required fields vary by resource; verify the operations table or metadata.
- Batch create and nested creation are resource-dependent.
- Use an external key or pre-query to prevent duplicate creates on retry.

## Update

API v1 uses POST for updates. Include the resource ID in the URL or documented payload form and send only intentional changes.

- Re-read before writing when concurrent updates matter.
- Avoid writing unchanged fields because it can trigger Automation/Validation Rules.
- For batch updates, reconcile every item and do not assume atomicity.

## Field/reference payloads

- Primitive: send the type-correct value.
- Rich text: preserve/sanitize documented HTML form.
- Reference: send the documented entity reference, generally including stable ID.
- Assignment and Team Assignment: use their resources/shapes; they are not ordinary scalar fields.
- Effort: respect role versus total effort semantics.
- Custom fields: use v1 custom-field payload syntax and configured type.
- Tags: distinguish replacement from append behavior.

## Attachments

Use the documented attachment resource/multipart flow. Verify local file, size/type, destination entity ID, filename, and upload response. Re-read the entity's attachment collection.

## Delete and unassign

```http
DELETE /api/v1/{Resources}/{id}
```

- Resolve and show the entity before deletion.
- Distinguish deleting an entity from deleting Assignment/TeamAssignment/relationship resources.
- A successful empty response does not prove all dependent effects; re-query.
- Batch deletes require per-ID reconciliation.

## Converted entities

Use `/api/v1/GeneralConversions` to observe conversion lineage. Do not assume this endpoint performs conversion.

## Direct Access

Direct Access operations read, grant, change, and revoke entity-level access. Treat them as authorization changes:

- Resolve user/entity IDs.
- Show old and proposed access mode.
- Require explicit approval.
- Verify effective access afterward with the intended identity.

## Failure handling

- Bad create/update: required field, reference shape, content type, immutable resource, or Validation Rule.
- `403`: write permission or object access.
- Timeout/ambiguous response: query by stable ID/external key before retrying.

## Sources

- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=crud-create
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=crud-update
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=crud-set-fields-properties
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=crud-upload-attachments
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=crud-delete-unassign
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=crud-direct-access-entities
