# API v1 domain workflows

Use these patterns only after reading the query or mutation reference for the underlying method.

## Users and membership

- Query inactive/never-recently-logged users with explicit date criteria.
- Resolve User and Role IDs before project membership changes.
- Deactivation, user creation, and membership assignment are separate operations.
- Never expose personal details beyond the task's scope.

## Work items

- Planned dates: normalize timezone/date-only semantics and update both dates intentionally.
- Rank: confirm the endpoint's ordering context and concurrent changes.
- Tags: distinguish appending a tag from replacing the tag set.

## Assignments

- Retrieve Assignment resources to identify User, Role, and owning entity.
- Create/set assignments with explicit entity, user, and role IDs.
- Delete the Assignment resource to unassign; do not delete the user or work item.
- Team Assignments use a distinct resource and may interact with Team Workflow.

## Releases and iterations

- Prefer querying Releases directly rather than only through Project inner collections.
- Cross-project releases require explicit project membership/scope handling.
- Iteration and Team Iteration counts are different dimensions.
- Batch release creation must validate dates, projects, naming, and duplicates.

## Testing module

Typical external QA flow:

1. Read Test Cases/Test Plans and linked work items.
2. Create a Test Plan Run with the required references.
3. Update each Test Case Run result.
4. Move/update the Test Plan Run state when complete.

Optimize by paging/batching and retrieving only required fields. Preserve stable mapping between the external test and Targetprocess run/case IDs.

## Time reports

REST time reports are machine-readable XML/JSON inputs for downstream reporting. Define source entities, date range, users/projects, grouping, and summary columns. Reconcile API totals with timezone and permissions.

## Code samples

Portal Python, PHP, JavaScript, VBA, and Google Apps Script examples demonstrate request shape but may use old dependencies, Basic Auth, or browser patterns. Modernize TLS, async behavior, secret storage, retries, paging, and error handling before reuse.

## Sources

- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=cases-users
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=cases-work-items
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=cases-assignments
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=cases-releases
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=cases-testing-module
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=cases-time-reports
