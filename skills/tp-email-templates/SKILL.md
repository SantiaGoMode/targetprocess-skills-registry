---
name: tp-email-templates
description: Create, review, and troubleshoot Apptio Targetprocess email notification templates, markup, root objects, conditions, entity/user/comment fields, Tool methods, HTML and date formatting, subjects, and process-specific notification behavior. Use for notification-template customization rather than Slack, webhook, or Automation Rule messaging.
---

# Targetprocess Email Notification Templates

Preserve the current template and test actual delivered messages for representative events and recipients.

## Required Workflow

1. Identify event, editable template, process scope, recipients, and subject/body outcome.
2. Read only the matching references below.
3. Add the smallest field access or condition and guard missing data.
4. Verify recipient permissions, HTML/date formatting, locale/timezone, and actual delivery.

## Reference Routing

- Root `entity`, `originalEntity`, `user`, Comment, Request, Assignment, Role, and related-object availability: read [object-model.md](references/object-model.md).
- Template markup, `if`/`elseif`/`else`, subject rules, null handling, and reusable examples: read [markup-and-conditions.md](references/markup-and-conditions.md).
- `Tool` methods/fields, URLs, terms, HTML descriptions, mentions, and dates: read [tool-and-formatting.md](references/tool-and-formatting.md).

Do not load the full object model for a formatting-only task.

## Guardrails

- `user` is the recipient, not necessarily modifier or assignee.
- Do not expose data the recipient cannot normally view.
- Process raw HTML descriptions with supported helpers.
- Keep subject output single-line.
- Avoid secrets, internal payloads, and notification storms.

## Routing

- Slack/webhook/custom event messaging: `tp-automation-rules` or `tp-integrations`.
- Template ownership and rollout: `tp-governance`.
