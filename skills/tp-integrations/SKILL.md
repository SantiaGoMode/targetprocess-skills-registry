---
name: tp-integrations
description: Design, review, and troubleshoot Apptio Targetprocess native issue-level integrations, Jira or Azure DevOps routing and field mapping, WorkSharing API, DevOps services, Zapier, webhooks, RESTful Storage, user/requester validation, conversion lookup, and undelete. Use for cross-system identity, synchronization, reconciliation, proxy, share/unshare, or auxiliary Targetprocess API workflows.
---

# Targetprocess Integrations

Design every integration around stable identity, deterministic mapping, idempotency, and explicit source-of-truth ownership.

## Required Workflow

1. Define systems, direction, entities, source of truth, identity mapping, events, volume, latency, conflicts, and deletion policy.
2. Read only the matching reference below.
3. Confirm connector/profile/service enablement and least-privilege service accounts.
4. Test create, update, retry, rename, delete, unlink, unmapped values, conflicts, and outages.
5. Add reconciliation and operational correlation IDs.

## Reference Routing

- Native Jira/ADO/TP JavaScript routing, field mapping, comparators, worklogs, and states: read [native-routing-and-mapping.md](references/native-routing-and-mapping.md).
- WorkSharing API proxy/share/unshare/profile operations and Automation DevOps services: read [worksharing-and-devops.md](references/worksharing-and-devops.md).
- Zapier triggers/actions, Slack/Toggl/email use cases, incoming webhooks, signatures, and retries: read [zapier-and-webhooks.md](references/zapier-and-webhooks.md).
- RESTful Storage, unauthenticated user/requester validation, conversion lookup, and undelete: read [auxiliary-apis.md](references/auxiliary-apis.md).

Do not load auxiliary API material for native issue synchronization or native mapping details for a Zapier task.

## Guardrails

- Store stable cross-system IDs; mutable names are not sufficient identity.
- Deduplicate retries and tolerate out-of-order events.
- Use comparators to prevent update ping-pong.
- Never expose WorkSharing proxy as unrestricted HTTP.
- Treat share/unshare, branch, merge, restore, and destructive reconciliation as explicit writes.
- Keep secrets outside source/payloads and redact logs.

## Routing

- Rule-hosted webhook/DevOps JavaScript: `tp-automation-rules`.
- Ordinary Targetprocess CRUD/query payloads: `tp-rest-api-v1` or `tp-rest-api-v2`.
- CSV/ADM integration: `tp-import-export`.
- Credential and production policy: `tp-governance`.
