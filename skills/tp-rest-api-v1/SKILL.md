---
name: tp-rest-api-v1
description: Build, review, and troubleshoot Apptio Targetprocess REST API v1 integrations, including authentication, CRUD, assignments, attachments, direct access, testing data, and administrative operations. Use for `/api/v1` requests, any Targetprocess data mutation, v1 filters/includes/appends, or v1 error diagnosis; route read-only analytical queries to `tp-rest-api-v2` when possible.
---

# Targetprocess REST API v1

Use API v1 for mutation and v1-only resources. Prefer API v2 for read-only analytics.

## Required Workflow

1. Classify the operation as read-only, write, destructive, permission-changing, or administrative.
2. Resolve the tenant URL, authentication mode, entity type, target IDs, required fields, and permissions.
3. Read only the reference matching the task below.
4. For writes, display the exact method, endpoint, target, and payload before execution.
5. Re-read affected entities and reconcile every requested ID; never infer success from request acceptance alone.

## Reference Routing

- Authentication, PATs, OpenToken, service tokens, cookies, SSO, and credential errors: read [authentication.md](references/authentication.md).
- Reads, resources, formats, paging, filters, sorting, includes/excludes, appends, and Context: read [querying.md](references/querying.md).
- Create, update, set fields, assignments, attachments, delete, undelete links, conversion, and Direct Access: read [mutations.md](references/mutations.md).
- Users, work items, releases, testing, time reports, and integration use cases: read [domain-workflows.md](references/domain-workflows.md).
- Concrete entity endpoints, required fields, polymorphic resources, and metadata discovery: read [entity-reference.md](references/entity-reference.md).

Do not read unrelated references. Combine references only when the task crosses those boundaries.

## Non-Negotiable Guardrails

- Require explicit intent before create, update, delete, restore, assign, upload, or access changes.
- Treat tokens as the owning user's authority; Targetprocess has no separately scoped API permission model.
- Redact credentials from URLs, logs, examples, and output.
- Page every collection and shape responses explicitly.
- Resolve destructive targets by stable ID and distinguish entity deletion from relationship removal.
- Make retries idempotent with an external key or existence check.
- Query `/api/v1/Index/meta` when tenant terminology or resource shape is uncertain.

## Routing

- Current read-only analytics: `tp-rest-api-v2`.
- Historical state: `tp-history-api`.
- Undelete, conversion lookup, RESTful Storage, Zapier, or WorkSharing: `tp-integrations`.
- Credential and production-readiness policy: `tp-governance`.
