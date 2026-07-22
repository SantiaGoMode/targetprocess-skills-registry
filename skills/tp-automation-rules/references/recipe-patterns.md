# Automation Rule recipe patterns

Use this as a design index. Open the named official example only when implementing that pattern, then replace all tenant-specific values.

## Assignment/reference propagation

Examples cover Feature Project → User Story, child assignee → parent, parent assignees → new child, User Story Feature → Bug, current Release/Team Iteration, starter/author assignment, and Project Teams → new item.

Checklist: null parent, multiple teams, role existence, process compatibility, unchanged-value guard, reassignment loop, and stable IDs.

## Lifecycle and state propagation

Examples close parents/children/related Requests, reopen Stories/Requests, move to Planned/Blocked, start parent/child items, and update Team State.

Checklist: real state names per process, multiple final states, default final state, deleted relations, direct/transitive cycles, and manual override policy.

## Creation and templates

Examples create related Bugs, template Tasks, project hierarchies, entity templates, webhook-created Bugs, and multiple templates.

Checklist: idempotency/existence key, required fields, partial creation, template version, duplicate webhook delivery, and cleanup after failure.

## Parent/child inheritance and rollups

Examples inherit Tags, Team, Release/Iteration/Team Iteration, Planned Dates, Portfolio Epic, Relations, and Initial Estimate.

Checklist: direction of truth, conflict precedence, null-clearing behavior, multiple parents/relations, and bidirectional-rule cycles.

## Scheduling, effort, and dates

Examples calculate/end/start dates, close stale items, constrain by relations, reschedule timeboxes, set velocity, severity dates, and timebox dates.

Checklist: account timezone, date-only versus instant, holidays/calendar assumption, null effort, rounding, overdue backlog volume, and same-day reruns.

## Comments, mentions, reminders, and custom fields

Examples add closure comments, follow items, route Requests by recipient email, post split comments, create reminders, mention assigned users, set Found In fields/default effort, recalculate test results, and derive a Request field from email domain.

Checklist: sanitize text, requester privacy, mention identity mapping, notification storm, missing field, and email normalization.

## External integrations

- GitLab: pipeline state and commit/Sources records.
- Slack: Request/comment/assignment/mention notifications, stuck/overdue/closed summaries, slash commands.
- Wrike: create Task from User Story.
- DevOps: state from merge/pull updates and branch/merge request creation.

Checklist: signature/authentication, secrets, delivery ID, retry deduplication, user/repository mapping, payload escaping, rate limits, and one-way versus reconciled sync.

## Applying an example

1. Identify the closest behavior group, then locate the exact official page in `recipe-catalog.md`.
2. Copy the JSON into review, not directly into production.
3. Replace entity types, IDs, names, states, roles, fields, URLs, and parameters.
4. Add explicit idempotency and cycle prevention even when the example omits them.
5. Test negative, retry, null, high-volume, and permission cases.

## Source indexes

- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=rules-examples-actions-in-targetprocess
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=rules-examples-integration-gitlab
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=rules-examples-integration-slack
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=rules-examples-integration-wrike
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=rules-devops-integrations
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=rules-examples-working-custom-fields
