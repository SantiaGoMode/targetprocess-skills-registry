# Native issue-level routing and mapping

## Integration contract

Define source/target system, profile, entity types, stable external IDs, direction, source of truth per field, create/update/delete/unlink behavior, and reconciliation schedule.

## JavaScript routing

Routing scripts receive documented source entity/profile context and return the routing decision. Keep decisions deterministic:

- Prefer numeric/stable project/profile IDs.
- If a custom field selects a route, validate it against an allowlist.
- Log decision reason and stable IDs without sensitive payloads.
- Define no-route and multiple-route behavior.
- Treat process/project names as mutable dependencies when unavoidable.

## Field mappings

Mapping scripts transform source modifications into target field changes. For every mapped field define:

- Direction.
- Type conversion.
- Null/clear semantics.
- Unmapped value behavior.
- Ownership/conflict policy.
- Whether updates should be suppressed when equivalent.

Use the documented `sourceEntityModification` and return contract rather than guessing event shape.

## Comparators

Comparators determine whether values are materially equal. Use them to prevent update ping-pong for:

- State/Team State names.
- User identities.
- Worklogs/time values.
- Collections where order is irrelevant.
- Text with normalization rules.

Comparator equality must match mapping normalization.

## Jira worklog → Targetprocess Time

The portal example deletes/recreates target Time records when worklogs change. This is destructive reconciliation. Require stable external worklog IDs, restrict deletion to integration-owned records, preserve user/date/duration mapping, and handle partial failure before recreating.

## Jira Status ↔ Team State

Define an explicit bidirectional map and fallback for unknown/renamed states. Prevent a target update from bouncing back as a new source change. Confirm process/team workflow variations.

## Verification

- Create/update/retry/out-of-order events.
- Source/target rename.
- Null/clear and unsupported value.
- Conflicting concurrent edits.
- Delete/unlink.
- Reconciliation after outage.
- Loop suppression.

## Sources

- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=tp-javascript-routings
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=routings-example-dynamic-routing-filter-by-process
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=routings-example-sync-features-specific-custom-field-value
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=tp-javascript-fields-mappings-comparators
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=comparators-example-map-jira-worklog-targetprocess-time
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=comparators-example-map-jira-status-targetprocess-teamstates
