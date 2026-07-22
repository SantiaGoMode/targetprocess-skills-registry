---
name: tp-rest-api-v2
description: Construct, review, optimize, and troubleshoot read-only Apptio Targetprocess REST API v2 queries for analytics, selectors, filters, aggregation, paging, streaming, custom fields, tags, dates, timeboxes, and relations. Use for `/api/v2` requests and read-only Targetprocess reporting; route mutations to `tp-rest-api-v1`.
---

# Targetprocess REST API v2

Use API v2 as the default read-only query surface. Never use it to imply or perform mutations.

## Required Workflow

1. Identify the real API entity type, desired rows or aggregate, required fields, scope, and expected volume.
2. Read only the task-specific reference below.
3. Build the smallest query with explicit `select`, paging, deterministic ordering, and `isoDate` where dates appear.
4. Add filters, nesting, or aggregation incrementally and URL-encode query values.
5. Report the final request, assumptions, response shape, paging behavior, and null omission.

## Reference Routing

- Selectors, aliases, nested collections, filters, expressions, casts, ordering, and first/last: read [query-language.md](references/query-language.md).
- Custom fields, tags, dates, releases, iterations, team iterations, and relations: read [domain-fields.md](references/domain-fields.md).
- Paging, root results, aggregation, streaming, response shape, and large exports: read [results-and-streaming.md](references/results-and-streaming.md).
- `401`, `403`, bad requests, incomplete results, encoding, metadata, or query debugging: read [troubleshooting.md](references/troubleshooting.md).

Do not read unrelated references. Combine them only when the query requires both language and domain-specific behavior.

## Core Guardrails

- Keep selectors explicit; omitted nulls and unrequested fields are normal.
- Alias every complex selector and avoid duplicate inferred output names.
- Page beyond the default 25; documented maximum `take` is 1000.
- Encode `+` as `%2B` and never mix API v1 query syntax into v2.
- Use `Inbound.Id`/`Outbound.Id` for connected entities, not relation-record IDs.
- Confirm tenant metadata when business terms differ from API types or custom fields vary.

## Routing

- Mutations or v1-only resources: `tp-rest-api-v1`.
- Point-in-time or change history: `tp-history-api`.
- Formula-engine syntax: `tp-formulas-validation`.
- Credential and service-account policy: `tp-governance`.
