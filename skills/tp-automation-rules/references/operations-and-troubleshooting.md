# Automation Rule operations and troubleshooting

## Public SaaS limits

| Limit | Documented value |
| --- | --- |
| UI blocks per rule | 60 |
| Time interval | 15 minutes to 90 days |
| Total execution | 10 minutes |
| One JavaScript action block | 20 seconds |
| Targetprocess query in JavaScript | 10 seconds |
| External HTTP request | 10 seconds |
| CPU time | 1 second |

Private cloud can differ.

The portal conflicts on mutation count: **Actions** says a JavaScript action currently returns at most 50 modification commands, while **Limits and Timeouts** says 1000 JavaScript actions per rule. Verify the effective tenant/runtime limit before designing a batch.

## Efficiency rules

1. Trigger only on fields that affect the result.
2. Move invariant service calls and queries outside loops.
3. Merge N per-ID queries into one query and group locally.
4. Select only required fields.
5. Do not return updates when the value is unchanged.
6. Bound children/relations/commands and define overflow behavior.
7. Split work across safe events/time windows when one execution cannot fit.

## Log behavior

- Recent failed events are linked from the rule list/details.
- Built-in detailed logs are retained for one week.
- `console.log(...)` writes diagnostic output.

Log rule/version, event/resource ID, branch decision, counts, and external correlation ID. Never log tokens or full sensitive payloads. Export operational evidence elsewhere when longer retention is required.

## Troubleshooting tree

### No log/run

- Rule inactive.
- Wrong source entity/modification.
- Updated field not selected.
- Expected event did not occur.

### Triggered, filter did not match

- State/name typo (`InProgress` versus `In Progress`).
- Process filter on an entity without Process.
- Comment/Assignment ID confused with parent work-item ID.
- Optional create-time field not yet populated.
- Responsible Team confused with assigned Team.
- OR-group/AND-condition logic misunderstood.

### Execution error

- Timeout: inefficient queries/loops or slow external service.
- v2 bad request: wrong entity/field name, term instead of API name, invalid selector/filter.
- Deserialization error: invalid mutation command payload.
- Reference error: object supplied where numeric ID/entity reference is required.
- HTTP non-2xx: inspect sanitized status/body and retry contract.

### Succeeds but produces wrong/duplicate effects

- Rule retriggered itself.
- Webhook/provider retried delivery.
- Bidirectional parent/child rules formed a cycle.
- Query read stale or broader data than intended.
- Creation lacked an existence/idempotency check.
- Multiple process states or relationships matched.

## Sources

- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=rules-automation-best-practices
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=troubleshooting-automation-rule-logs
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=troubleshooting-how-make-rules-more-efficient
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=troubleshooting-limits-timeouts
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=troubleshooting-my-automation-rule-is-not-working
