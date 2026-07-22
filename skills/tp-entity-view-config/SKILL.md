---
name: tp-entity-view-config
description: Design, edit, review, and troubleshoot Apptio Targetprocess entity detailed-view JSON, tabs, blocks, fields, layouts, inner or multilevel lists, lookup and relation columns, embedded roadmaps, portfolio assignments, assignment controls, and configurable dropdowns. Use for supported server-side UI configuration rather than custom mashup code.
---

# Targetprocess Entity View Configuration

Load only the reference matching the requested UI surface. Do not read the entire configuration catalog for a simple field or tab edit.

## Required Workflow

1. Identify the exact surface, entity type, process scope, roles, current template/configuration, and desired behavior.
2. Preserve the working JSON/source and choose one reference below.
3. Make one structural change at a time and validate component IDs, selectors, fields, and syntax.
4. Test relevant roles, processes, permissions, data volumes, and navigation states.
5. Keep rollback JSON and record version/tenant assumptions.

## Reference Routing

- Tabs, blocks, fields, layouts, embedded pages/boards, trigger buttons, and basic components: read [detailed-view-basics.md](references/detailed-view-basics.md).
- Conditional visibility, security boundary, unsupported fields, localization, and mashup compatibility: read [visibility-and-limitations.md](references/visibility-and-limitations.md).
- Native/extended/mixed inner lists, column IDs, counters, custom columns, ordering, and search issues: read [inner-lists.md](references/inner-lists.md).
- Flexible multilevel lists, levels, resource items, filters, ordering, and visual encoding: read [multilevel-lists.md](references/multilevel-lists.md).
- Lookup and Relations-tab columns, filters, embedded-list lookups, and limitations: read [lookups-and-relations.md](references/lookups-and-relations.md).
- Embedded Roadmap periods, cards, filters, swimlanes, templates, milestones, and encoding: read [embedded-roadmaps.md](references/embedded-roadmaps.md).
- `portfolioAssignments`, `entityListMetadata`, queries, cross-group filters, and selectors: read [portfolio-assignments.md](references/portfolio-assignments.md).
- Assignment role visibility/grouping/order, user filters, and effort display: read [assignment-control.md](references/assignment-control.md).
- Configurable Dropdown registration, attributes, API v2 source, and view placement: read [configurable-dropdowns.md](references/configurable-dropdowns.md).

Do not load unrelated references. If external React, `tau.mashups`, DOM, or CSS is required, route to `tp-mashups` instead.

## Guardrails

- Hiding UI elements is not access control; fields remain available through APIs and other views.
- Prefer documented configuration/component IDs over DOM-derived selectors.
- Bound list/query depth and test production-scale data.
- Custom labels are not automatically localized.
- Confirm compatibility with installed Mashups before rollout.

## Routing

- External/client code: `tp-mashups`.
- API v2 selector design used by controls: `tp-rest-api-v2`.
- Rollout, ownership, and upgrade review: `tp-governance`.
