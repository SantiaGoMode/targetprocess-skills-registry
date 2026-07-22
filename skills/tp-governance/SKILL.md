---
name: tp-governance
description: Review and establish governance for Apptio Targetprocess APIs, Personal Access Tokens, service accounts, Automation Rules, Validation Rules, Metrics, integrations, view configurations, Mashups, ownership, source documentation, testing, rollout, and upgrade safety. Use for security boundaries, read-only access design, rule inventory, change management, naming/tagging, or production-readiness reviews.
---

# Targetprocess Governance

Treat API write access and system automation as production privileges. Base governance on actual permission and lifecycle constraints.

## Required Workflow

1. Identify the asset and decision: credential/access, rule lifecycle, or production readiness.
2. Read only the matching reference below.
3. Record owner, business purpose, data/write scope, environments, dependencies, monitoring, rollback, and review date.
4. Verify current tenant controls and obtain the required approval before production change.

## Reference Routing

- PATs, service accounts, read-only design, System User risk, token review, and permissions: read [credentials-and-access.md](references/credentials-and-access.md).
- Automation/Validation Rule inventory, JSON capture, tags, naming, dependencies, cloning, and upgrades: read [rule-lifecycle.md](references/rule-lifecycle.md).
- Cross-capability implementation readiness, testing, monitoring, rollback, and unsupported surfaces: read [production-readiness.md](references/production-readiness.md).

Do not load rule-lifecycle material for a credential-only question.

## Known Constraints

- API permissions mirror user permissions; no separate API-only scope exists.
- No supported API exists for bulk Automation/Validation Rule inventory/export/search.
- No built-in dependency or where-used analysis exists for rule logic.
- Source control documents/reviews configuration; it is not a supported automated deployment mechanism.
- System operations may bypass Validation Rules.

## Routing

Use the relevant capability skill for implementation details, then return here for readiness review.
