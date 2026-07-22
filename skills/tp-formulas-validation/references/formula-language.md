# Formula language

## Build incrementally

1. Return one native field.
2. Add parent/reference traversal.
3. Add null/conditional handling.
4. Add collection aggregation.
5. Add type conversion and custom fields.

## Values and calculations

Formula expressions support native values/properties, arithmetic, string operations, conditions, aggregates, and conversions. Use the exact grammar documented for the selected engine; do not substitute JavaScript or API v2 object-selector syntax.

## Native and linked fields

Use dots for reference traversal:

```text
EntityState.Name
Feature.Project.Name
```

Guard missing parents/references. Confirm depth and entity-type availability in the tenant.

## Collections and aggregates

Use collection aggregates such as `SUM`, `COUNT`, `MIN`, `MAX`, or the documented engine form. Define behavior for an empty collection and null child values.

Examples of intent:

- Sum effort from child User Stories.
- Count completed children.
- Aggregate custom values from related entities.
- Select a date or value conditionally by state.

## Custom fields

Formula pages use `CustomValues` accessors appropriate to type, for example a numeric accessor for a numeric custom field. This differs from API v2, where many custom fields are direct space-stripped properties.

- Use the accessor matching Text, Number, Date, Check Box, Money, Dropdown, or Entity.
- Preserve exact display name inside the accessor.
- Confirm the field exists on every entity type receiving the formula.
- Do not silently convert malformed text to numbers/dates.

## Relations

Relation-based formulas historically use Master/Slave collections. Confirm direction using current inbound/outbound meaning and prevent double counting when multiple paths reach the same entity.

## Type discipline

- Ensure every conditional branch returns a compatible type.
- Normalize decimals/rounding for effort and money.
- Normalize timezone/date-only semantics for dates.
- Preserve entity ID/type for entity-valued results.
- Define division-by-zero and empty denominator behavior.

## Sources

- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=fields-custom-formulas-syntax
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=fields-entity
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=fields-parent-linked-entities
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=fields-custom
