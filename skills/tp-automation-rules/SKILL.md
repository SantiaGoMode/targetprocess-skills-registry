---
name: tp-automation-rules
description: Design, review, optimize, and troubleshoot Apptio Targetprocess Automation Rules, including entity, interval, webhook, filter, JavaScript, HTTP, command, helper, DevOps, and named-trigger behavior. Use for automation-rule JSON, rule examples, failed runs, loops, limits, performance, or reusable rule architecture.
---

# Targetprocess Automation Rules

Treat a rule as an event pipeline: source, filters, and actions. Treat examples as adaptable patterns, never tenant-independent contracts.

## Required Workflow

1. Define trigger, invariants, target entities, side effects, worst-case volume, and retry behavior.
2. Read only the relevant reference below.
3. Prefer UI blocks for simple behavior; use JavaScript only for branching, queries, batching, or external calls.
4. Add idempotency, recursion prevention, null handling, and unchanged-value checks.
5. Test every trigger and code path, including retries and high-volume cases.
6. Review logs after activation and externalize telemetry that must outlive one week.

## Reference Routing

- Sources, field/team/process/JavaScript filters, actions, raw JSON, and setup: read [pipeline-setup.md](references/pipeline-setup.md).
- `args`, `context`, v2 queries, webhook data, async JavaScript, and runtime semantics: read [javascript-runtime.md](references/javascript-runtime.md).
- Command shapes, `utils`, HTTP, Named Triggers, and DevOps service functions: read [commands-and-services.md](references/commands-and-services.md).
- Limits, efficiency, logs, decision-tree troubleshooting, and common errors: read [operations-and-troubleshooting.md](references/operations-and-troubleshooting.md).
- Choosing and adapting a recipe pattern: read [recipe-patterns.md](references/recipe-patterns.md).
- Finding the exact IBM action, GitLab, Slack, Wrike, DevOps, or custom-field example after choosing a pattern: read [recipe-catalog.md](references/recipe-catalog.md).

Do not load recipe material for a runtime or troubleshooting question. Do not load the catalog until an exact example is needed. Combine references only when implementing a cross-cutting rule.

## Non-Negotiable Guardrails

- Query through the read-only v2 service and return explicit commands for mutations.
- Never place invariant API calls inside loops; merge requests and filter locally.
- Prevent direct and transitive cycles, especially inheritance and state propagation.
- Deduplicate webhook retries and creation commands.
- Do not embed secrets in rule JSON or logs.
- Verify the tenant's command limit: current docs conflict between 50 commands and 1000 actions.
- Do not deploy or inventory rules through unsupported internal APIs.

## Routing

- Rule lifecycle, service accounts, ownership, and production review: `tp-governance`.
- Formula calculation or transaction rejection: `tp-formulas-validation`.
- CSV/ADM orchestration: `tp-import-export`.
- Native issue-level mapping and WorkSharing: `tp-integrations`.
