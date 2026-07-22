---
name: tp-formulas-validation
description: Design, review, and troubleshoot Apptio Targetprocess Metrics, Calculated Custom Fields, custom formulas, progress or effort metrics, planned-date calculations, and Validation Rules. Use for formula syntax, aggregation, custom-field calculations, `$Author`, `$Previous`, `$ChangedFields`, transaction rejection, or policy enforcement.
---

# Targetprocess Formulas and Validation

Choose the engine first: Metrics persist calculated values, Calculated Custom Fields compute dynamically, and Validation Rules reject invalid transactions.

## Required Workflow

1. Define whether the outcome is a stored calculation, dynamic calculation, transaction rejection, or post-save side effect.
2. Read only the matching reference below.
3. Confirm entity types, source/target field types, recalculation triggers, null/empty behavior, and API/report needs.
4. Test every branch, empty collection, missing parent, type conversion, and privileged/system execution path.

## Reference Routing

- Metric versus Calculated Custom Field selection, data types, recalculation, and API/report behavior: read [engine-selection.md](references/engine-selection.md).
- Formula grammar, fields, parents, linked entities, collections, custom fields, conversions, and aggregations: read [formula-language.md](references/formula-language.md).
- Effort, relation effort, progress, and planned-date metric patterns: read [metric-patterns.md](references/metric-patterns.md).
- Created/Updated/Deleted Validation Rules, `$Author`, `$Previous`, `$ChangedFields`, and `IsBeingDeleted()`: read [validation-rules.md](references/validation-rules.md).

Do not load Validation Rules for a calculation-only task or metric patterns for a policy-only task.

## Guardrails

- Metrics may overwrite manual values in their target custom fields.
- Time-relative formulas are unsafe when the engine does not recalculate merely because time passes.
- Formula custom-field syntax differs from API v2 syntax.
- A Validation Rule expression returning `true` rejects and rolls back the transaction.
- Automation, Metrics, sync integrations, and System User actions may bypass Validation Rules by design.
- Avoid cycles when Automation Rules and Metrics write each other's inputs.

## Routing

- Post-save side effects: `tp-automation-rules`.
- API v2 selectors rather than formula grammar: `tp-rest-api-v2`.
- Production enforcement and privileged bypass review: `tp-governance`.
