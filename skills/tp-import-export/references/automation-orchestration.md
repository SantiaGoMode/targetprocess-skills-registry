# Automation Rule orchestration for import/export

Automation Rules can start preconfigured CSV import/export profiles and react to ADM completion events. The profile remains the durable mapping/configuration; the rule orchestrates it.

## Design

1. Create and validate the profile manually.
2. Define the rule trigger and exact profile identity.
3. Pass only supported parameters.
4. Record a correlation/run identifier.
5. Handle completion success and failure distinctly.
6. Reconcile output/counts before downstream actions.

## Cycle prevention

- A completion-triggered rule must not start the same operation recursively.
- Prevent overlapping logical runs even though the importer serializes execution.
- Deduplicate repeated completion events.
- Define what happens when a scheduled run starts while an earlier run remains queued/running.

## Runtime limits

Automation Rule total/JavaScript/query/HTTP limits still apply to orchestration and downstream processing. Do not load or transform the full CSV inside a time-constrained JavaScript action when a profile/service should handle it.

## Failure handling

- Profile not found/disabled.
- Import rejected before processing.
- Partial row errors.
- Completion event without expected correlation.
- Export artifact unavailable.
- Downstream action failure after successful import.

Keep import success separate from downstream-notification success so retries do not repeat the import.

## Source

- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=data-automation-rules-import-export
