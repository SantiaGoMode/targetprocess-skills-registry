# Targetprocess skills registry

Reusable skills for agents working with Apptio Targetprocess. Each canonical skill is a folder containing `SKILL.md` and `agents/openai.yaml` metadata.

Each `SKILL.md` is a compact task router. Detailed contracts, examples, limitations, troubleshooting, and official source links live in narrowly scoped `references/` files, so routine tasks do not load unrelated product areas.

## Skills

| Skill | Scope |
| --- | --- |
| [`tp-rest-api-v1`](skills/tp-rest-api-v1/SKILL.md) | Authentication, CRUD, assignments, attachments, and writable API operations |
| [`tp-rest-api-v2`](skills/tp-rest-api-v2/SKILL.md) | Read-only queries, selectors, filters, aggregation, paging, and streaming |
| [`tp-history-api`](skills/tp-history-api/SKILL.md) | Historical snapshots, past state, changes, and custom-field history |
| [`tp-automation-rules`](skills/tp-automation-rules/SKILL.md) | Rule sources, filters, actions, JavaScript, limits, and troubleshooting |
| [`tp-formulas-validation`](skills/tp-formulas-validation/SKILL.md) | Metrics, Calculated Custom Fields, formulas, and Validation Rules |
| [`tp-entity-view-config`](skills/tp-entity-view-config/SKILL.md) | Detailed views, lists, lookups, roadmaps, assignments, and dropdowns |
| [`tp-mashups`](skills/tp-mashups/SKILL.md) | Client-side Mashups, card units, widgets, and external components |
| [`tp-integrations`](skills/tp-integrations/SKILL.md) | Jira/ADO/WorkSharing, DevOps, Zapier, webhooks, and auxiliary APIs |
| [`tp-import-export`](skills/tp-import-export/SKILL.md) | CSV profiles, mappings, batching, dates, and ADM automation |
| [`tp-email-templates`](skills/tp-email-templates/SKILL.md) | Notification markup, objects, conditions, formatting, and Tool helpers |
| [`tp-governance`](skills/tp-governance/SKILL.md) | Credentials, ownership, rule lifecycle, rollout, and production readiness |

## Routing principles

- Use API v2 for read-only analytics and API v1 only when v1 behavior or mutation is required.
- Treat every write, delete, restore, share, import, or permission change as explicit production authority.
- Treat documentation recipes as adaptable patterns, not tenant-independent contracts.
- Verify tenant metadata, permissions, limits, feature enablement, and version-gated behavior live.
- Use dedicated least-privilege service accounts; tokens inherit their owner's permissions.
- Read only the reference files routed for the current task; combine them only for genuinely cross-cutting work.

## Source analysis

The complete IBM Developer Hub coverage and rationale for this decomposition are in [`docs/targetprocess-developer-hub-analysis.md`](docs/targetprocess-developer-hub-analysis.md).

## Claude Code

This repository is also a Claude Code plugin. The manifest at [`.claude-plugin/plugin.json`](.claude-plugin/plugin.json) exposes the canonical `skills/` tree directly, while each `agents/openai.yaml` remains optional OpenAI-only UI metadata.

Load a local checkout for development:

```bash
claude --plugin-dir /path/to/targetprocess-skills-registry
```

Claude namespaces the installed skills with the plugin name, for example `/targetprocess-skills-registry:tp-entity-view-config`.
