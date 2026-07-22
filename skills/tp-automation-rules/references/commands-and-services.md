# Automation Rule commands and services

## Prefer helper builders

Load helpers with:

```javascript
const utils = require("utils");
```

Documented helpers include command builders for:

```javascript
utils.createResource("Task", fields)
utils.updateResource("Feature", id, fields)
utils.cloneResource("UserStory", id, overrides)
utils.deleteResource("TeamAssignment", id)
utils.moveToState("Epic", id, "Final")
utils.moveToStateByName("UserStory", id, "In Progress")
```

The helper returns commands; it does not prove the referenced entity/state/field is valid. Resolve IDs and process state names first.

## Reference fields

Use actual entity references rather than arbitrary nested objects:

```javascript
return utils.updateResource("Feature", args.ResourceId, {
  Release: releaseId == null ? null : { Id: releaseId }
});
```

Check the documentation/runtime's accepted `Id` casing and payload schema. A reference generally must be a numeric ID or actual entity reference.

## HTTP requests

Use the documented helper/service to send HTTP with explicit method, headers, content type, and body. Apply the public 10-second request timeout as a design constraint.

- Keep secrets in supported parameters/secret storage, not source.
- Escape JSON and user-controlled text.
- Validate non-2xx responses.
- Do not return duplicate side effects after an ambiguous retry.

## Named Triggers

Define the reusable rule source in Raw JSON:

```json
{
  "type": "source:uniqueName",
  "name": "company.feature.recalculate"
}
```

Activate from another rule:

```javascript
return {
  command: "ActivateNamedTrigger",
  payload: {
    name: "company.feature.recalculate",
    data: { featureId: args.ResourceId }
  }
};
```

Read passed data from the documented named-trigger argument. Names are shared contracts: namespace them, document payload schema, and do not rename casually.

Nested named triggers are supported, but repeated activation is not: A→A and A→B→A are invalid. Design an acyclic trigger graph.

## DevOps service

```javascript
const devOps = context.getService("devOps");
```

The documented service includes branch, merge-request, repository, and attached DevOps-data operations. Validate integration enablement and repository IDs/names before use. Write operations require explicit authority and idempotency.

Example shape:

```javascript
await devOps.createBranch({name: "feature/us12345", ref: "main"}, repositoryId);
await devOps.createMergeRequest({
  source: {branch: "feature/us12345", repositoryId},
  target: {branch: "main", repositoryId},
  title: "US#12345"
});
```

Do not copy documentation typos such as misspelled branch names.

## Sources

- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=rules-javascript-helper-functions
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=rules-javascript-devops-functions
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=rules-reusable-automation-via-named-triggers
