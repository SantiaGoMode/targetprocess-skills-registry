# Automation Rule JavaScript runtime

## Return contracts

- JavaScript filter: return boolean.
- JavaScript action: return one command, an array of commands, or the documented empty result when no mutation is required.
- Use `async`/`await` for Targetprocess and HTTP services.

## Modified Entity arguments

- `args.Author`: modifier identity.
- `args.Current`: current entity snapshot.
- `args.Previous`: previous snapshot; null for Created.
- `args.Modification`: `Created`, `Updated`, or `Deleted`.
- `args.ResourceId`: modified entity ID.
- `args.ResourceType`: real API type such as `UserStory` or `TeamAssignment`.
- `args.ChangedFields`: changed field names.

`args.Current` contains own simple fields and shallow references (normally ID/name). Collections are unavailable and deeper reference fields are not guaranteed. Query v2 for everything else.

Custom fields may be accessed by their event-snapshot names:

```javascript
const category = args.Current.Category;
const risk = args.Current["Expected Risk"];
```

Guard `args.Previous`, optional references, deleted entities, and missing custom fields.

## Webhook arguments

- `args.headers`: request headers.
- `args.body`: parsed request body when compatible.

Validate schema before dereferencing nested properties. Normalize header casing according to actual runtime behavior and never trust sender-provided identity without verification.

## Querying Targetprocess

```javascript
const api = context.getService("targetprocess/api/v2");
const rows = await api.queryAsync("UserStory", {
  where: `feature.id == ${featureId}`,
  select: "{id,name,releaseId:release.id}",
  take: 1000
});
```

The query spec mirrors API v2 parameters such as `where`, `select`, `result`, ordering, and paging.

- Use real entity names, not terms.
- Select only needed fields.
- Page when the service does not guarantee full retrieval.
- A root `result` may be scalar.
- Avoid building expressions from untrusted webhook text; validate numeric IDs and escape controlled strings.

## Runtime coding practices

- Declare services once outside loops.
- Query batches with `in [...]` or a combined predicate, then map locally.
- Compare existing and intended values before returning an update command.
- Bound arrays and command counts.
- Log stable IDs and decision summaries, not secrets or sensitive payloads.
- Use deterministic branch ordering and explicit fallback behavior.
- Treat external calls as unreliable and design safe retries.

## Sources

- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=rules-javascript-automation
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=rules-javascript-reference
