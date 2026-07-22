# Zapier and webhook integrations

## Zapier scope

The portal documents creation-oriented triggers for Iteration, Request, Team Iteration, Build, Feature, Task, Release, Comment, Bug, Time, and User Story. Actions include create Task/User Story/Comment/Build/Feature/Time/Request/Bug, add Requester, delete entity, and change state.

Verify the current Zapier catalog; it is narrower than REST APIs and Automation Rules.

## Choose Zapier when

- Volume is moderate.
- Supported trigger/action is sufficient.
- Human-manageable low-code operations are preferred.
- Eventual consistency and provider retry behavior are acceptable.

Use direct APIs/Automation Rules when mapping, volume, reconciliation, or unsupported operations require more control.

## Webhook contract

- Authenticate/verify signatures where supported.
- Validate content type and schema.
- Record provider delivery/event ID.
- Deduplicate retries.
- Tolerate out-of-order delivery.
- Return quickly; move bounded work to the supported execution model.
- Keep secrets outside URLs/source and redact logs.

## Portal use cases

- Slack → Targetprocess entity creation: validate caller identity, allowed entity/project, required fields, and escaping.
- Toggl: personal browser-extension plus integration flow; not enterprise reconciliation by default.
- Time added → email: consider native notifications/Automation Rules before an external Zap.
- Slack notifications/commands in Automation examples: map users, escape messages, verify Slack signature, rate-limit summaries, and prevent notification storms.

## Verification

- Duplicate and delayed deliveries.
- Invalid signature/schema.
- Provider outage/rate limit.
- Deleted/renamed target project/state.
- Unauthorized user/entity type.
- Partial downstream success and reconciliation.

## Sources

- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=integration-getting-started
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=integration-supported-triggers-actions
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=integration-use-cases
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=cases-create-new-items-in-targetprocess-from-slack
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=cases-integration-personal-toggl-timer
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=cases-send-email-when-time-spent-is-added
