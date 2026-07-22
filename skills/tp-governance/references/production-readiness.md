# Targetprocess production-readiness review

## Scope and authority

- Owner and approver named.
- Business purpose and affected users/data defined.
- Read, write, destructive, permission, and external side effects explicit.
- Service account/token scope verified live.

## Contract fidelity

- Real entity/field/custom-field/process/state/role names verified.
- Version-gated feature and tenant configuration verified.
- Documentation example IDs/names removed.
- Unsupported/internal API or client dependency explicitly labeled.

## Correctness

- Stable identity and idempotency.
- Paging/volume and partial-result handling.
- Null/empty/deleted/moved data.
- Concurrency and stale reads.
- Retry and out-of-order event handling.
- Rule/mapping cycle prevention.
- Timezone/date-only/rounding behavior.

## Security and privacy

- Secrets externally managed and logs redacted.
- UI hiding not treated as authorization.
- Recipient/user data minimized.
- External input authenticated, validated, and escaped.
- WorkSharing proxy and Direct Access tightly scoped.

## Testing

- Sandbox/non-production test.
- Positive, negative, permission, retry, high-volume, and failure cases.
- UI and standard PAT path.
- Automation, Metric, sync, import, and System User path where relevant.
- Production-representative data volume.

## Rollout and rollback

- Source/config snapshot and parameters captured.
- Bounded rollout or pilot defined.
- Rollback/disable/reconcile procedure tested.
- Monitoring, alert owner, correlation IDs, and review window defined.
- Post-change verification queries specified.

## Upgrade safety

- Mashup/client APIs and CSS isolated.
- Solution rules cloned/documented.
- Configuration IDs and field/state dependencies registered.
- Re-test trigger defined for Targetprocess upgrades or external API changes.

## Acceptance output

Report Ready, Ready with conditions, or Not ready. List verified evidence, unverified assumptions, residual risks, owner, and required next action.

## Source

- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=development-best-practices
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=practices-automation-rules-best
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=practices-managing-automation-rules-validation-rules-scale
