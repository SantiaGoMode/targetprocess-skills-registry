---
name: tp-mashups
description: Build, install, review, migrate, and troubleshoot Apptio Targetprocess client-side Mashups, custom card units, lane headers, dashboard widgets, external React components, placeholders, and UI extensions. Use for `tau.mashups`, DOM/CSS behavior, client runtime context, mashup library code, or compatibility migration; prefer entity-view configuration when it can solve the request.
---

# Targetprocess Mashups

Treat Mashups as version-sensitive client extensions. Prefer supported server-side configuration when it can implement the behavior.

## Required Workflow

1. Identify page/view, placeholder, users/roles, entity types, client context, and supported alternative.
2. Read only the relevant reference below.
3. Preserve current source and enabled/placeholder settings.
4. Implement the smallest isolated extension with explicit cleanup.
5. Test navigation, refresh, async failure, permissions, empty data, and current client versions.

## Reference Routing

- Install/update/disable/delete, placeholder scope, user targeting, cookies, and compatibility: read [lifecycle-and-compatibility.md](references/lifecycle-and-compatibility.md).
- Custom card units and custom lane headers: read [custom-units-and-lanes.md](references/custom-units-and-lanes.md).
- Dashboard widget contract, settings, context, resize, async work, cleanup, and React: read [dashboard-widgets.md](references/dashboard-widgets.md).
- External detailed-view components, component factory, dynamic filtering, CSS/DOM workarounds, and sample components: read [external-components.md](references/external-components.md).

Do not load widget or card-unit material for a lifecycle-only task.

## Guardrails

- Never treat DOM structure, CSS selectors, sibling markers, or private component names as stable contracts.
- Clean up subscriptions, timers, observers, nodes, and async work on navigation/unmount.
- Sanitize rich text, external links, and user-controlled data.
- Avoid global changes when the feature cannot be scoped per view.
- Re-test after Targetprocess client updates.

## Routing

- Supported tabs, fields, lists, Roadmaps, assignments, and dropdowns: `tp-entity-view-config`.
- Read/write API behavior: `tp-rest-api-v2` or `tp-rest-api-v1`.
- Source ownership, review, and rollout: `tp-governance`.
