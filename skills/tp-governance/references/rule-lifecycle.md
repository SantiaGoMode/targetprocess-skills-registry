# Automation and Validation Rule lifecycle

## Unsupported capabilities

Targetprocess does not provide supported APIs for bulk rule listing/export/search/query, dependency analysis, or where-used/impact analysis.

Do not rely on internal/undocumented endpoints; they may change or disappear.

## Source documentation capture

For each rule capture manually:

- Name.
- Description/business purpose.
- Raw JSON.
- Parameter values.
- Tags.
- Active status.
- Owner and environment.
- Dependencies and last review/test.

Use source control for documentation, review, history, and impact analysis—not automated deployment.

## Naming and parameters

- Namespace reusable Named Triggers.
- Use semantic parameters such as current reporting period rather than embedding a specific year.
- Prefer stable IDs/keys; register every unavoidable display-name dependency.
- Tag by solution/use case, customer/custom ownership, environment, and responsible team.

## Solution rules

For large/long-lived implementations:

1. Clone the solution-provided rule.
2. Modify/test the clone in Dev/sandbox.
3. Validate edge cases.
4. Apply an intentional production change.
5. Tag/document customization.

Direct edits risk overwrite during product upgrades, solution enhancements, or fixes.

## Dependency register

Track entity types, fields/custom fields, processes, states, roles, projects/teams, tags, Named Triggers, other rules, APIs, external endpoints, secrets, imports, and expected volume.

## Operations

- Built-in rule change history is basic.
- Automation error detail lasts one week.
- Review failures at least weekly and retain necessary telemetry externally.
- Review dependencies before renaming fields/states/processes or upgrading solutions.

## Source

- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=practices-managing-automation-rules-validation-rules-scale
