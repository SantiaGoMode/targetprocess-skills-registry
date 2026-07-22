# Metrics versus Calculated Custom Fields

## Decision table

| Requirement | Metric | Calculated Custom Field |
| --- | --- | --- |
| Persist result into custom field | Yes | No |
| REST API visibility | Yes | No |
| Advanced filter/export/report use | Yes | Limited/feature-dependent |
| Recalculate when source data changes | Yes | Evaluated when page loads/refreshes |
| User can edit target field | Yes, but metric may overwrite it | No |
| Feed another formula | Yes | No/limited |
| Time-relative value that changes without data mutation | Poor fit | Better fit |

The portal recommends Metrics when possible, but that does not override time/recalculation requirements.

## Supported result types

Metrics can target custom fields including Number, Text, Date, Check Box, Drop Down List, Template URL, Money, and Targetprocess Entity. Calculated Custom Field support differs; confirm the configured target type.

## Recalculation consequences

- Metric values change when a source dependency triggers recalculation, not simply because the wall clock advances.
- `Now`, `Today`, current/past/future timebox flags, `NumericPriority`, Lead/Cycle time, and Forecast dates can become stale in a Metric if no relevant source changes.
- Calculated Custom Fields evaluate dynamically but are not available through REST API and are less composable.

## Selection method

1. List consumers: UI, API, filters, exports, reports, other formulas.
2. Define freshness: on source mutation, page load, or passage of time.
3. Define edit ownership: calculated-only or user-editable target.
4. Confirm target custom-field type.
5. Identify all recalculation dependencies.
6. Choose engine and document limitations.

## Safety

A Metric targeting a normal custom field can overwrite manual input. Use a dedicated calculated field or make ownership explicit in UI/process documentation.

## Source

- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=fields-metrics-vs-ccfs
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=fields-data-types
