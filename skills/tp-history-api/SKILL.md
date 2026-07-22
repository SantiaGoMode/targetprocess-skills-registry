---
name: tp-history-api
description: Query and interpret Apptio Targetprocess historical records, entity snapshots, past states, changes by user or date, and historical custom-field values. Use for `/api/history`, `*Histories`, audit reconstruction, point-in-time questions, or change lineage; use REST API v2 for current-state analytics.
---

# Targetprocess History API

Use history only when current entity state cannot answer the question.

## Required Workflow

1. Define entity type, stable IDs or project scope, time interval, timezone, and the exact historical claim.
2. Read [history-model-and-queries.md](references/history-model-and-queries.md) for endpoint selection and reconstruction.
3. Also read [custom-fields.md](references/custom-fields.md) only when historical custom values are involved.
4. Page and order chronologically; reconstruct from validity intervals.
5. Report observed facts separately from inferred transitions and disclose gaps.

## Guardrails

- Treat history as snapshots over validity periods, not guaranteed event sourcing.
- Normalize timezone boundaries before filtering or comparing.
- Use numeric IDs; names, terms, and field definitions may change over time.
- Do not infer modifier, deletion, or conversion details absent from the returned records.
- Test a known entity before a bulk audit.

## Routing

- Current data and aggregates: `tp-rest-api-v2`.
- Current v1-only resources or writes: `tp-rest-api-v1`.
- Conversion identity or undelete: `tp-integrations`.
