# Historical custom fields

Historical custom values use dedicated accessors rather than current API v2 direct custom-field properties.

## Rules

- Select the accessor matching the historical field type, such as the documented string accessor for text values.
- Use the documented `IsChanged` accessor/flag when the question is whether the custom value changed.
- Convert historical values explicitly to text, number, date, checkbox, or entity identity.
- Confirm the field name and type at the historical time; administrators may rename or recreate fields.
- Distinguish missing field, null value, and unsupported historical coverage.

## Query method

1. Confirm the entity type supports the field historically.
2. Query a known entity and narrow time range.
3. Select the historical field value and change flag.
4. Verify type conversion and timezone for date fields.
5. Scale only after comparing a known UI/history example.

## Interpretation

- Snapshot presence does not itself prove the field changed.
- A string representation is not proof of the original field type.
- Entity custom values should retain stable referenced IDs where available.

## Source

- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=record-custom-field-values-in-history
