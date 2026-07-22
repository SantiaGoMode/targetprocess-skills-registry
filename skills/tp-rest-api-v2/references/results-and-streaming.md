# API v2 results, paging, aggregation, and streaming

## Ordinary result shape

```json
{
  "next": "/api/v2/UserStory?...&take=25&skip=25",
  "items": [{"id": 1, "name": "Example"}]
}
```

- Default page size: 25.
- Documented maximum `take`: 1000.
- Use `take` and `skip` or follow `next`/`prev` until complete.
- Add deterministic `orderBy` when paged results must be reproducible.
- Null fields are omitted.

For a changing collection, ordinary offset paging can duplicate or miss records when entities are created/deleted between requests. Use a stable filter/order or the streaming endpoint for full exports.

## Root aggregation

`result` applies after root filtering and returns a scalar or projected aggregate object, not an `items` envelope.

```text
/api/v2/UserStory?result=count
/api/v2/UserStory?where=(tasks.count()!=0)&result=sum(effort)
/api/v2/UserStory?result={
  sum:sum(effort),
  average:average(effort),
  min:min(effort),
  max:max(effort)
}
```

Handle numeric precision and the empty-set behavior in the calling integration.

## Streaming endpoint

```text
/svc/tp-apiv2-streaming-service/stream/UserStory?where=(Effort>0)&select={id,name,effort}
```

Use only when the entire large collection is required.

- `take` and `skip` are ignored.
- `result` is ignored; use ordinary v2 for aggregation.
- `orderBy` is ignored; records are ordered by entity ID.
- Every item includes `__id`; consumers may ignore it.
- The documentation states at most 10 concurrent streaming requests per account.
- Streaming supports access-token authentication through the documented parameter.

Process the JSON stream incrementally when the client supports it. Apply backpressure, timeouts, partial-output handling, and restart/reconciliation logic.

## Export decision

- Small/interactive result: ordinary v2 with explicit page size.
- Aggregate only: ordinary v2 with `result`.
- Large but bounded stable result: ordinary deterministic paging can suffice.
- Entire large changing set: streaming, if supported and operationally acceptable.

## Source topic

- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=v2-result
