# Metric patterns

## Effort custom formula

Use when standard effort does not express the desired entity estimate. Define:

- Source entity types.
- Role versus total effort.
- Child/parent inclusion.
- Unit and normalization.
- Downstream effect on capacity, progress, forecasts, and reports.

Do not replace standard effort semantics without reviewing every downstream consumer.

## Effort via Relations

Use relation direction and relation type explicitly. Prevent:

- Counting both inbound and outbound paths.
- Duplicate related entities through multiple relations.
- Including deleted/final/out-of-scope entities unintentionally.
- Combining normalized and raw effort.

Verify a hand-calculated example before rollout.

## Custom progress

Portal patterns include progress based on:

- Entity State.
- Checkbox custom fields.
- Completed child count.
- Time Spent versus Effort.
- Dropdown value.
- Mixed User Story and Bug children.

For every progress formula define numerator, denominator, empty denominator, final-state override, cap/floor, rounding, and mixed-child weighting.

## Planned dates

Patterns include:

- Child dates → parent dates.
- Parent dates → children.
- Initial Estimate/Effort → Planned End Date.
- Relation-constrained dates.
- Release/Iteration/Team Iteration dates → work item.

Choose one authoritative direction. Do not combine opposing Metrics/Automation Rules without cycle/conflict analysis. Define timezone, date-only behavior, weekends/holidays, null dates, manual override, and what happens when timebox dates move.

## Verification matrix

- No children/relations.
- One and many children.
- Null/zero effort.
- Mixed final/non-final states.
- Deleted or moved relation.
- Manual target-field edit.
- Source field rename/type change.
- Large hierarchy/relation volume.

## Sources

- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=fields-effort-custom-formula-metric
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=fields-effort-via-relations
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=fields-custom-progress-metrics
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=fields-customize-planned-dates-metrics
