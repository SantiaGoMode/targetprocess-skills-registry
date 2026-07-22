# Targetprocess Developer Hub: exhaustive portal analysis

- Source: [IBM Targetprocess Developer Hub](https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas)
- Snapshot analyzed: 2026-07-22
- Coverage: 243 table-of-contents nodes, comprising 15 structural headings, the non-content Welcome node, and 227 linked pages. All 227 linked pages were fetched; 226 returned article content and the remaining item was the portal root shell.

## Executive assessment

The portal is not a single API manual. It is a mixed developer and administrator knowledge base spanning data access, mutation, automation, formula engines, UI extension, integration middleware, import/export, and governance. Its strongest material is the conceptual API v2 documentation, the automation runtime reference, detailed-view schema examples, and the newer governance guidance. Its weakest aspect is contract clarity: current reference material, old samples, internal-looking client APIs, and version-specific recipes sit at the same navigation level.

The content falls into four stability tiers:

1. **Platform contracts:** API v1/v2 request semantics, authentication, paging, CRUD, historical endpoints, documented limits, validation DSL, and entity references. These are the best basis for reusable agent skills, although authentication and limits still require live verification.
2. **Supported configuration surfaces:** Automation Rules, formulas/metrics, detailed-view JSON, UI controls, CSV profiles, and notification templates. These are useful but account/version/configuration dependent.
3. **Integration-specific APIs:** DevOps/WorkSharing, Zapier, RESTful Storage, conversion, undelete, and webhook recipes. Use only when the corresponding product feature is enabled.
4. **Examples and client-side extensions:** automation examples, mashups, external React components, and older JavaScript samples. Treat these as patterns to adapt, not stable copy-and-run contracts.

The most important cross-portal findings are:

- API v1 and v2 are complementary, not successive versions: v1 is the mutation surface; v2 is the preferred read/query surface.
- Authentication is permission-equivalent to the owning user. Personal access tokens are governable but are not independently scopeable as read-only credentials.
- The portal contains a material documentation conflict: **Actions** says a JavaScript action can currently return at most 50 modification commands, while **Limits and Timeouts** says a JavaScript rule can perform 1,000 actions. A production integration must verify the effective tenant/runtime limit rather than encode either number as universal.
- Automation has tight runtime constraints (including UI-block, interval, HTTP, query, CPU, and execution time limits), one-week error-log retention, and no supported API for bulk rule inventory/export/dependency analysis.
- Several pages expose JSON, React, `tau.mashups`, `componentFactory`, or service objects that behave like extension contracts but lack explicit compatibility/version guarantees. Keep those implementations isolated and regression-tested.
- Examples frequently hard-code state names, IDs, project names, custom fields, or legacy terminology. Translate business terms to real API entity/field names and externalize tenant-specific values.

## Section-by-section analysis

### Targetprocess

The portal landing page correctly frames the hub as a collection of APIs, automation, mashups, formulas, validation, view customization, integrations, import/export, and references. It is an index, not a technical contract. The navigation tree, rather than this page, is the authoritative scope map.

### Targetprocess REST API v1

API v1 is the operational API: it supports reads and CRUD, but its query model is less expressive than v2. It remains necessary for create/update/delete, assignments, attachments, direct access, testing integrations, and several administrative operations.

- **Getting Started:** Introduces the v1 URL/query shape and demonstrates read, partial response, append, create, update, and delete flows. Useful orientation, but its prominent Basic Auth examples should not be treated as the preferred modern credential design.
- **Authentication:** The real security contract. It covers Apptio OpenToken, legacy resource tokens, personal access tokens, cookies, Basic Auth, and current-user lookup. Prefer PAT or the applicable Apptio front-door flow; never assume a token is read-only, and remember Requester accounts cannot use the API.
- **Resources and collections:** Defines entity, nested entity, collection, hierarchy, and inner-collection behavior. This is essential before composing `include`, `append`, or nested writes.
- **Response formats:** Covers XML, JSON, and JSONP. New integrations should choose JSON explicitly; JSONP is legacy browser interoperability, not a modern default.
- **Paging:** Establishes the default 25-item page and `take`/`skip`, including inner-collection paging. Every list integration must page deliberately.
- **Sorting and Filters:** Defines `where`, operators, nested fields, custom-field filters, tag filters, inner-collection filters, and sorting. This is v1 syntax and must not be mixed with v2 LINQ/DSL examples.
- **Partial response (includes and excludes):** Explains traffic/CPU reduction through `include` and `exclude`. Use explicit response shaping for integrations; do not depend on default entity payloads.
- **Appends and Calculations:** Adds server-side calculations over nested collections. It is capable, but v2 is generally the clearer read/analytics surface.
- **Operations (CRUD):** Parent index for writable operations and required fields.
  - **Create:** POST semantics, response controls, mandatory fields, batch creation, and nested creation. Validate required/reference fields against tenant metadata.
  - **Update:** POST-based updates, batch updates, assign/unassign behavior. Plan for partial failure and idempotency in batch tooling.
  - **Set Fields and Properties:** Payload forms for primitive fields, rich text, references, assignments, team assignments, effort, custom fields, and tags. This is the central mutation-shape reference.
  - **Upload Attachments:** Attachment creation and cURL multipart examples. Treat file size/type constraints as tenant/runtime concerns unless separately confirmed.
  - **Delete, Unassign:** DELETE semantics, batch delete, unassignment, and project/user recovery links. Deletion is a destructive operation and should always resolve IDs before execution.
  - **Converted Entities:** Uses `GeneralConversions` to trace type conversion. Useful for preserving identity lineage when Bugs, Stories, or Features are converted.
  - **Direct Access to Entities:** Reads, grants, changes, and revokes entity-level access. This is a high-risk authorization surface and should be separated from ordinary CRUD tools.
- **Context:** Returns logged-in user, locale, selected projects/processes, and process context. Useful for tenant-aware clients and terminology/date interpretation.
- **Use Cases:** A mixed cookbook for entity type discovery, relations, state filters, teams, and comments. It is illustrative rather than exhaustive.
  - **Users:** Inactivity lookup, role/user prerequisites, deactivation, project membership, and create-and-assign. Administrative and privacy-sensitive.
  - **Work Items:** Planned dates, rank, and tags. Demonstrates common batch mutations.
  - **Assignments:** Read, set, and delete assignments. Distinguish assignments from team assignments.
  - **Releases:** Project releases, cross-project releases, iteration counts, team-iteration counts, and batch creation. Prefer querying releases directly rather than through projects.
  - **Testing module:** Test cases/plans/runs, result updates, state changes, and performance advice. This is the core external test-run integration path.
  - **Time Reports:** Machine-readable time exports and summary columns. Useful for reporting pipelines, not presentation.
- **Code Samples:** Python, PHP, JavaScript, and VBA examples. These are language-era samples and should be modernized for TLS, dependency, async, and secret-handling practices.
  - **JavaScript:** Same-origin mashup reads/writes and cross-origin Basic/token examples. Prefer modern `fetch` and secure credential transport over direct copying.
  - **Google Apps Script:** Spreadsheet ingestion, update, counts, and filtering. Good low-code pattern, but access tokens must not be embedded in shared sheets/scripts.
- **Troubleshooting REST API issues:** Organizes 401, incomplete results, bad GET/POST, and 403 failures. Diagnose authentication, paging/shape, malformed payload/query, and authorization separately.

### Targetprocess REST API v2

API v2 is the preferred read-only analytics/query API. It offers projection, filtering, nested calculations, aggregation, ordering, paging, and streaming, but it is explicitly not a replacement for v1 writes.

- **Overview:** Defines `/api/v2/{entity}/{id}` and `where`, `select`, `result`, `take`, `skip`, `orderBy`, `filter`, `prettify`, and `isoDate`. Key rules: explicitly alias complex selectors, request full nested paths, expect camelCase JSON, and expect null values to be omitted.
- **Selectors and filters:** The expression-language core: nested projections, calculations, casts, collections, and .NET-like expressions. Validate entity and field names through metadata; duplicate inferred output names and unaliased complex expressions are common errors.
- **Result:** Defines JSON result shape, default 25, maximum documented page size 1,000, root aggregation, and the streaming service. Streaming is for complete large exports, orders by ID, ignores `take`, `skip`, `result`, and `orderBy`, includes `__id`, and is limited to 10 concurrent requests per account.
- **Working with Custom Fields:** Selection, filtering, and ordering across custom-field types. Field access uses space-stripped names in v2; type-specific comparisons matter, and absent fields produce bad requests.
- **Working with Tags:** Distinguishes comma-separated `Tags` from `TagObjects`. Use `TagObjects` for exact matching and counts; avoid substring logic over the combined string.
- **Working with Dates:** Covers Microsoft JSON dates, ISO with timezone, ISO without timezone, and date filters. Prefer `isoDate` and explicit timezone handling in agent outputs.
- **Filtering by Releases, Iterations and Team Iterations:** Defines `IsCurrent`, `IsPrevious`, `IsNext`, `InPast(n)`, and `InFuture(n)`. Results are evaluated per project, which matters in multi-project reporting.
- **Working with Relations:** Defines inbound/outbound direction, relation types, counts/filters, direct `/relations` queries, assignable helpers, and entity-specific fields. Relation record IDs are not endpoint entity IDs; use `Inbound.Id`/`Outbound.Id`.
- **Getting first or last element of a collection:** Shows newest/oldest comment selection. The reliable oldest pattern is ascending order plus `first`; do not assume collection default order.
- **Troubleshooting REST API issues:** Shared with v1 and therefore partly generic. For v2, first check paging, explicit selectors, null omission, aliases, field names, encoding, and v1/v2 syntax mixing.

### Historical Record

This group explains temporal data access. It should be used when current entity state is insufficient and change reconstruction is required.

- **Overview of historical record:** History stores entity snapshots over validity periods. It documents custom-field behavior and limitations; consumers should not assume perfect event-sourcing semantics.
- **Simple history:** Lists supported `*Histories` resources for straightforward snapshot queries.
- **Full history:** Covers native and extendable-domain entities via the history API. Choose this for broader lineage/detail than simple history.
- **Examples:** Queries changes by user/date/project, state at a past date, and post-date updates/closures. These are the best starting patterns for audit questions.
- **Custom Field values in history:** Provides custom-value accessors, `IsChanged`, filtering, and conversion. Custom-field type conversion must be explicit.

### Working with Other APIs

These are feature-specific APIs, not a unified alternative to v1/v2.

- **RESTful Storage:** Custom client storage groups and values with get/put/post/delete/query operations. Useful for mashup/integration state; do not use it as an undocumented business-data store without retention, ownership, and schema rules.
- **Validate Users and Requesters:** Unauthenticated validation endpoints. Minimize exposed input/output and do not mistake validation for authentication or authorization.
- **Undelete API:** Recovery paths for Projects/Users, later native/extendable entities, unsupported entity types, and newer UI recovery. Availability is version- and entity-dependent.
- **Conversion API:** Looks up old/new entity IDs around UI-driven conversions. It observes conversions; it is not the primary conversion command surface.

### Setting up Automation Rules

Automation Rules are the portal's largest area. They form event pipelines: source, zero or more filters, and actions. They can react to entity changes, time intervals, or incoming webhooks; query v2; return mutation commands; and call external services.

- **Automation Rules Overview:** Capability and use-case map. It correctly frames rules as event-driven mutation orchestration.
- **Setup:** Pipeline/block model.
  - **Sources:** Modified Entity, Incoming Webhook, Time Interval, and JSON-only/other sources. Time intervals range from 15 minutes to 90 days; webhook service selection affects suggestions, not the underlying compatibility contract.
  - **Filters:** Team, Process, field-value, and JavaScript filters; all configured filters must pass. Field filters use OR between groups and AND within groups. Create-time filtering on optional fields is a common timing mistake.
  - **Actions:** Update, create, create-related, move state, comment, JavaScript, and HTTP. Reference-field payloads and related-entity targeting need explicit testing. The page's 50-command statement conflicts with the limits page's 1,000-action statement.
  - **Other:** Raw JSON representation for portability and features without a UI block. JSON is valuable for review and source documentation, but the portal does not promise an automated deployment API.
  - **How to apply Raw JSON from Examples:** Copy/import workflow for examples. Replace IDs, state names, custom fields, URLs, and parameters before activation.
- **Diagnostic and troubleshooting:** Parent index.
  - **Automation Rule Logs:** Failed-event access, `console.log`, and one-week retention. Externalize important operational telemetry because the built-in history is short.
  - **How to make rules more efficient:** Remove irrelevant triggers, move invariant calls out of loops, merge API queries, avoid redundant work, and reduce returned commands. This is mandatory design guidance under the tight runtime limits.
  - **Limits and Timeouts:** Public SaaS limits include 60 UI blocks, 15-minute to 90-day intervals, 10-minute total execution, 20-second JavaScript action blocks, 10-second Targetprocess queries, 10-second HTTP requests, and 1 second CPU; it also states 1,000 JavaScript actions. Private cloud may differ.
  - **Troubleshooting: My Automation Rule is not working!:** A useful decision tree: not triggered, filter mismatch, or execution error. It calls out real-entity names versus terms, parent/child ID confusion, process-less entities, create timing, and assignment/team semantics.
- **JavaScript for Automation Rules:** Establishes boolean-return filters and command-return actions, asynchronous v2 querying, external HTTP, and event context.
  - **JavaScript Reference:** Defines `args.Author`, `Current`, `Previous`, `Modification`, `ResourceId`, `ResourceType`, `ChangedFields`, webhook headers/body, services, query specs, and command shapes. Related objects in `Current` are shallow and collections are unavailable; query v2 for more data.
  - **JavaScript Helper Functions:** `require("utils")` helpers for create, update, clone, delete, state moves, HTTP, dates, and related commands. Prefer these builders over hand-written payloads when they cover the use case.
  - **JavaScript DevOps functions:** Branch, merge-request, repository, and attached DevOps data operations through `context.getService("devOps")`. Feature enablement and repository identifiers must be validated.
- **Examples: Actions in Targetprocess:** A large pattern library. It is best analyzed by behavior, because every recipe embeds tenant assumptions.
  - **Assignment propagation:** *Assign a Feature's Project to a User Story*; *Assign users to a parent when they are working on its child*; *Assign a User Story's Feature to a Bug*; *Assign a bug to a current Release/Team Iteration*; *Assign an item to the Person who started it*; *Assign the people working on a parent to a newly created child*; *Assign Project's Teams to a newly created item*. These demonstrate reference/assignment propagation; guard against loops, multiple-team workflow rules, null parents, and reassignment churn.
  - **Date, estimation, and scheduling:** *Calculate Planned End Date based on Initial Estimate or Effort*; *Close an item on its Planned End Date*; *Close User Stories that stayed more than 10 days in a particular state*; *Limit Planned Dates based on Relations*; *Reschedule work items in a Release / Iteration / Team Iteration*; *Set Velocity for new Team Iterations from effort of previous Team Iterations of the Team*; *Set Planned End Date based on Severity*; *Set Planned Dates based on Iteration/Release/Team Iteration dates*; *Start items on their Planned Start date*. These require timezone, calendar, state-name, null-date, and batch-volume testing.
  - **Lifecycle/state propagation:** *Close a request when all related items are closed*; *Close all children when the parent was closed*; *Close an item when all its children are closed*; *Move an item to Planned when its timebox is set*; *Move an item to Planned when its Planned Start Date is set*; *Move an item to Blocked state*; *Open User Story when one of its Bugs becomes open*; *Reopen a Request when replied by requester*; *Start the parent item when a child item has been started*; *Start an item when time is added on it*; *Update Team State when a Team is assigned*. These need loop prevention, final-state selection, and process-specific state resolution.
  - **Creation/templates:** *Create a Related Bug for new Requests*; *Create a set of Tasks under User Story based on a template*; *Create pre-defined hierarchy for new Projects*; *Entity Templates*; *Create Bug and attach to User Story from incoming webhook*; *Apply several templates*. These need idempotency keys or existence checks to prevent duplicates on retries.
  - **Inheritance/rollup:** *Inherit Tags from a parent to its children*; *Inherit Team from parent to all its children*; *Inherit Team Iteration/Release/Iteration from parent to its children*; *Inherit Planned Dates from children to parents*; *Inherit Planned Dates from parents to children*; *Inherit Release/Iteration/Team Iteration from children to its parent*; *Inherit Portfolio Epic from Epic to its Features*; *Inherit relations from child to parents*; *Inherit Initial Estimate from Epic to Portfolio Epic*. Define conflict precedence and avoid bidirectional rule cycles.
  - **Notifications and miscellaneous behavior:** *Add an automatic comment when a Request is closed*; *Follow items automatically*; *Move requests to a project based on recipient email*; *Post a comment when an item is split*; *Reminders in comments*; *Set Found In Release/Team Iteration fields for Bugs*; *Set default effort*; *Mention Assigned Users on Status Change*; *Mention Assigned Users on a field update*; *Recalculate last run result when Test Case Run is deleted*; *Set custom field for Request based on requester's domain*. Sanitize external text, handle missing users/fields, and consider notification storms.
- **Examples: Integration with GitLab:** Parent setup plus *pipeline passes → move state* and *commit pushed → Sources record*. Validate webhook signatures, delivery retries, repository mapping, and event deduplication.
- **Examples: Integration with Slack:** App/webhook setup plus Request messages, comment forwarding, personal assignment notifications, mention routing, stuck/overdue/closed summaries, and slash-command Request creation. Protect webhook/token secrets, map users explicitly, escape JSON/text, and verify Slack retries/signatures.
- **Examples: Integration with Wrike:** Task creation from a Targetprocess User Story. Treat it as one-way synchronization unless identity and update reconciliation are added.
- **DevOps Integrations:** State changes from merge/pull updates, branch/merge-request creation from a repository custom field, and Model Definition. Repository and entity mappings are the central contract; examples are feature-specific.
- **Examples: Working with custom fields:** Trigger-on-change, filter, UI action, JavaScript update, and HTTP templating. Respect custom-field types and escape HTTP JSON with the documented conversion helper.
- **Reusable automation rules via Named Triggers:** Extracts reusable logic and parameter passing without webhooks. Nested triggers are supported, but direct or transitive recursion is not; trigger names are effectively shared contracts.
- **Automation Rules Best Practices:** Require descriptions, minimize API calls, avoid calls in loops, test all branches/empty fields/high-volume cases, and review failures at least weekly.

### Mashups

Mashups are client-side custom add-ins. They are powerful but sit closest to the UI implementation and therefore carry the highest compatibility risk.

- **Overview:** Defines mashups as controls, widgets, reports, field hiding, and workflow/UI tweaks. Prefer supported configuration before custom code.
- **Install mashups from library:** Admin activation and update discovery. Review third-party/library source before enabling.
- **Install code of a mashup manually:** Manual source installation. Keep the exact source and version in source control.
- **Create your first mashup:** Placeholder selection, source composition, save, and verification. Placeholder choice is part of runtime behavior.
- **Deactivate and delete mashups:** Enabled switch, placeholder disablement, deletion, and global temporary disable. Maintain a rollback path.
- **Board, list and timeline views:** Catalog of whole-view mashups.
  - **Board, list and timeline views:** Duplicate/cross-linked catalog entry, not a separate contract.
  - **Dashboards:** Dashboard-specific mashups and widgets; embedded lists may need list-view techniques instead.
  - **Timesheet:** Timesheet page extensions.
  - **Settings:** Settings-screen extensions.
- **Capacity Mode for PI Planning Board:** Global board-setting change with no per-board selection. Scope is the principal risk.
- **Custom Units:** Card unit registration, entity types, card sizes, styles, editability, and model types. Test all supported sizes and entity shapes.
- **Custom Lane Headers:** Row-header units, entity types, existing/custom units, view scoping, and limitations. Keep selectors resilient to missing assignments/owners.
- **Dashboard widgets:** Widget template contract, settings, in-place updates, application context, resize, async cleanup, and React. The page carries old version/date cues; validate against the current client runtime.
- **Customize Settings for Mashups:** User/default-role targeting and cookie isolation. Use least-privilege targeting and avoid depending on logged-in cookies for external code.

### Metrics and Calculated Custom Fields

This is the formula subsystem. Metrics are generally preferred because values are persisted and API-visible; calculated custom fields are evaluated dynamically but have different trigger and composition behavior.

- **Metrics vs CCFs:** Decision table for recalculation, API/report visibility, editability, schedules, and formula differences. The portal explicitly recommends Metrics when possible; warn that metrics can overwrite manual custom-field values.
- **Data Types:** Supported output types and conversion behavior for Metrics and CCFs. Choose output types before writing formulas.
- **Custom Formulas Syntax:** Values/properties, arithmetic, conditionals, aggregates, and conversions. This is the grammar reference.
- **Entity Fields:** Simple fields, reference objects, and entity-specific field lists. Business labels may differ from real property names.
- **Parent and Linked Entities:** Dot traversal into parent/linked objects. Null-safe reasoning is required.
- **Custom Fields:** Accessing same-entity and parent custom fields, historically using `CustomValues` forms. Distinguish this formula syntax from API v2's newer direct custom-field properties.
- **Effort custom formula metric:** Custom effort definitions for entity types. Validate downstream progress/capacity semantics when replacing standard effort.
- **Effort via Relations:** Relation-based effort, settings, verification, and normalization. Relation direction and duplicate paths can distort totals.
- **Custom Progress Metrics:** State-, checkbox-, child-count-, time/effort-, dropdown-, and mixed-child progress patterns. Define denominators and empty-set behavior explicitly.
- **Customize Planned Dates with Metrics:** Parent/child inheritance, effort-based calculation, relation constraints, and timebox dates. Avoid creating feedback loops with date automation rules.

### Validation Rules

Validation Rules reject transactions before persistence. They are governance controls, not post-save automation.

- **Validation Rules:** Created/Updated/Deleted triggers and a DSL shared with v2/Metrics plus `$Author`, `$Previous`, `$ChangedFields`, `$Modification`, and `IsBeingDeleted()`. A `true` expression means validation failure. `$Previous` is limited to two levels and cannot access collections/aggregations.
- **Examples:** Final-state locks, user/project constraints, planned-date rules, assignment permissions, Definition of Done, team-only transitions, and time-entry windows. Examples need exception/admin and API-import testing because validations apply through APIs as well as UI actions.

### Entity View Customization

This area describes JSON-driven detailed-view composition and external components. It is a supported customization surface, but configurations are coupled to entity types, processes, component IDs, and client behavior.

- **Customize Entity Detailed View:** Tabs, embedded views/pages, blocks, fields, read-only/reset/trigger controls, tooltips, grid/stack/collapsible layout, and conditional visibility. Start from a copied working template and make small verified changes.
- **Limitations of Entity detailed view customization:** No localization for custom labels, no multiple-entity custom fields, Toggl constraints, and a migration/compatibility matrix for mashups. Check this before porting legacy mashups.
- **Advanced Customization: Customize Inner Lists:** Native and extendable lists, column ID discovery, hierarchical/heterogeneous lists, counters, custom columns, ordering, and search caveats. Avoid DOM-inspection-derived IDs when a diagnostic/config identifier exists.
- **Advanced Customization: Multilevel list customization:** Flexible levels, resource definitions, filters, ordering, visual encoding, and advanced topics. Deep lists can create expensive queries and confusing permissions.
- **Advanced Customization: Columns in Lookups and Relation Tab:** Lookup/relation columns, filters, ordering, embedded lists, configuration steps, examples, and limitations. Relation direction and entity-specific fields must be tested.
- **External mashup support:** Registers external React components with the layout renderer. Isolate the component API behind a small adapter.
  - **Feature state on User Story; Copy User Story details to clipboard; Epic on UserStory; External link; Rich text; Progress by Custom Field:** Concrete footer-placeholder component examples. Useful as skeletons, but IDs, fields, links, sanitization, and styling are tenant/client dependent.
  - **Sibling marker workaround:** CSS/markup workaround for stack-element class targeting. Treat as fragile implementation detail.
  - **Using componentFactory for dynamic filtering:** Uses current entity context for visibility. Handle navigation/context refresh and missing entity data.
- **Embedded Roadmap view customization:** Full roadmap-in-tab configuration: period, cards, filters, navigation span, swimlanes, size, card/hover templates, milestones, and visual encoding. Query and render complexity should be tested with production-scale data.
- **Setting up portfolioAssignments:** Dynamic grouping component, optional read-only behavior, `entityListMetadata`, query/filter relationships, ordering, selection, and nonstandard entities. Validate cross-group filters and permissions.

### UI Controls Configuration

- **Assignments Control Configuration:** Hide/group/order roles, filter users, and hide total effort. These changes affect discoverability and staffing workflow, so test all relevant roles.
- **Configurable Dropdown:** Defines custom dropdowns backed by API v2 queries, placement in detailed/list/quick-add views, required fields, sample data, enablement, click behavior, reset, source, and search. Query cost, permission filtering, and empty/error states are key concerns.

### Zapier integration

- **Getting started:** Targetprocess as trigger/action app in Zapier. Suitable for low-code, moderate-volume integrations.
- **Supported Triggers and Actions:** Enumerates creation triggers and create/delete/change-state actions. The catalog is limited compared with APIs and Automation Rules.
- **Use Cases:** Integration ideas index.
  - **Create new items from Slack:** Command/message-driven entity creation; validate requester identity and input.
  - **Personal Toggl timer:** Browser extension plus Zapier workflow; user-specific rather than enterprise-grade time integration.
  - **Send email when Time Spent is added:** Webhook and mail action pattern; consider native notifications/automation first.

### Email Notifications

- **Markup Guide:** Root objects (`entity`, `user`, `tool`, request/original entity), conditions, and advanced template topics. Admin-only and shared across recipients.
- **Templates:** Which workflow/system notification templates can be customized. Preserve originals and test representative events.
- **Entity, User, Comment:** Template object model and interactive data model. Related object availability varies by event.
- **Tool:** URL, term translation, HTML sanitization/normalization, mention styling, online-link checks, and settings fields. Some fields are deprecated; prefer supported methods/current settings.
- **Conditions:** `if`/`elseif`/`else` for project/process/entity-specific output. Keep conditions simple and include a fallback.
- **Formatting Values:** HTML description and date formatting. Raw descriptions must not be inserted unprocessed.
- **Examples:** Last editor, assignments, custom fields, terms, and links. Subject templates must be single-line.

### Native Issue Level Integrations (Jira, ADO, TP)

- **JavaScript Routings:** Dynamic sharing/routing, arguments, return contract, and recommendations. Routing must be deterministic and should log why an entity did or did not share.
  - **Dynamic routing by process:** Maps Jira state/custom project selection to a Targetprocess project/process. Names are fragile identifiers.
  - **Sync Features with a custom-field value:** Shares a Feature once a Jira project key is set. Treat the custom field as an external-routing contract.
- **JavaScript Fields Mappings and Comparators:** Mapping scripts, `sourceEntityModification`, return objects, and comparator options. Comparators are important for avoiding update ping-pong.
  - **Map Jira Worklog to Targetprocess time:** Rebuilds Targetprocess time from Jira worklogs. Destructive reconciliation requires stable external IDs and careful deletion scope.
  - **Map Jira Status to Targetprocess TeamStates:** Bidirectional state/team-state mapping. Define unmapped and renamed states explicitly.
- **WorkSharing API:** Proxy HTTP, entity shares, sharing/unsharing, and profile access. This is feature-specific and should not be exposed as a generic unrestricted proxy.

### Import and Export data

- **CSV Import:** Admin profile, integration-user mapping, timezone/date behavior, and limits: 10 relation mappings (9 assigned), 15 MB, about 240 records/minute, and one import at a time. Prefer date-only inputs when time is not needed.
- **Automation Rules for import and export:** Rules can trigger preconfigured CSV profiles and react to ADM completion events. Profiles remain the durable configuration; rules orchestrate them.

### API Reference

The entity references provide GET/POST/DELETE contracts and schemas for **UserStories, Bugs, Epics, Features, Tasks, Requesters, Users, Assignables, Generals, and Projects**. Use specific entity references when the type is known; use Assignables/Generals only for genuinely polymorphic operations. The **UNDELETE** subtree documents **`/DeletedItems`** and **`/Restore`** examples, but restore eligibility is narrower and version-dependent—cross-check the newer Undelete API page.

### Development Best Practices

- **Development Best Practices:** Index page.
- **Automation Rules Best Practices:** Duplicate/cross-link of the automation guidance; the recommendations remain descriptions, low API-call counts, complete branch/volume testing, and weekly failure review.
- **Managing automation rules and validation rules at scale:** The portal's most important governance page. It states there is no supported API for bulk rule listing/export/search, no dependency/impact analysis, and no separately scoped read-only API credential. Recommended controls are manual JSON capture plus parameters/status/tags, source control as documentation (not automated deployment), semantic parameters, consistent tags/names, cloned solution rules, sandbox validation, service accounts, and PAT review/revocation/expiration.

## Documentation quality and consistency audit

### High-confidence material

- API v1/v2 separation, query/mutation responsibilities, and paging behavior.
- API v2 projection, aggregation, custom-field, tag, date, timebox, and relation semantics.
- Automation event/filter/action model and JavaScript context shape.
- Validation DSL semantics and `$Author`/`$Previous` limitations.
- Current governance limitations for rules and credentials.

### Material requiring runtime or tenant verification

- JavaScript command/action maximum (50 versus 1,000 conflict).
- Authentication method availability and preferred Apptio front-door flow by environment.
- Private-cloud limits and version-gated undelete/delete validation behavior.
- DevOps/WorkSharing services, configurable dropdowns, portfolio assignments, and external components.
- Entity/custom-field/process terminology and metadata in the target tenant.
- Mashup/client component compatibility after UI updates.

### Content hygiene issues

- Duplicated/cross-linked pages (REST troubleshooting, Automation Best Practices, Board/List/Timeline).
- Legacy terminology such as master/slave relations remains alongside inbound/outbound terminology.
- Old sample technologies and dates coexist with pages updated in 2026.
- Typographical errors and malformed titles appear in navigation and code examples.
- Stability, deprecation, minimum-version, and permission requirements are inconsistently surfaced.
- Several examples show credentials in query strings; production integrations should prefer secure headers where the supported authentication mode permits it and must redact URLs/logs.

## Exact-title coverage ledger for condensed example groups

The analysis above groups repetitive recipes by behavior. The following ledger preserves the exact portal titles that were shortened in those group descriptions and records how each was assessed:

- **Move an item to “Planned” state when its Release / Iteration / Team Iteration is set** and **Move an item to “Planned” state when its Planned Start Date is set:** state-transition recipes; resolve process-specific state names and prevent trigger loops.
- **Move requests to particular project based on recipient email:** routing recipe; normalize domains/addresses, define a fallback, and avoid exposing requester data.
- **Update Team State of the Entity when a Team is assigned to it:** team-workflow recipe; verify that the entity/process supports team states and handle multiple assignments.
- **When GitLab Pipeline passes → move to state** and **When GitLab Commit pushed - Add new record to Sources tab on Entity View:** webhook recipes; authenticate, deduplicate, map repositories, and tolerate out-of-order delivery.
- **Message to Slack when Request created**, **Send comments to Slack**, **Send personal Slack notifications about assignments**, **Send mentions to Slack directly**, **Send stuck stories to Slack**, **Send overdue features to Slack**, **Send closed stories to Slack**, and **Slack Slash commands to create Request in Targetprocess:** notification/command recipes; secure secrets and signatures, escape payloads, map identities, deduplicate retries, and rate-limit summaries.
- **Create Task in Wrike when User Story is create in Targetprocess:** one-way creation recipe; add external identity storage and reconciliation if later updates are expected.
- **Example: Change state in Targetprocess, when merge/pull request updates** and **Example: When repositories custom field value changes → create branch/merge request:** DevOps orchestration; repository/entity mappings and retry idempotency are the key contracts.
- **Check the custom field value in a filter**, **Set custom field in UI action**, **Set custom field in Javascript action**, and **Use custom field value in HTTP request action:** custom-field recipes; preserve field type, tenant name, missing-value behavior, and JSON escaping.
- **Advanced Сustomization: Customize Inner Lists** and **Advanced Сustomization: Multilevel list customization on Entity Detailed view:** schema-driven list composition; prefer documented IDs, bound query depth, and test search/permissions/large data.
- **Mashup: Feature state on User Story**, **Mashup: Copy User Story details to clipboard**, **Mashup: Epic on UserStory**, **Mashup: External link**, **Mashup: Rich text**, and **Mashup: Progress by Custom Field:** external-component examples; adapt fields/IDs and test sanitization, navigation updates, and client compatibility.
- **Sibling marker(Workaround for Classes for Stack element):** CSS workaround; highly coupled to rendered markup and therefore fragile.
- **Create new items in Targetprocess from Slack**, **Integration with personal Toggl timer**, and **Send an email when Time Spent is added:** Zapier use cases; appropriate for low/moderate volume, with identity, credential, and retry caveats.
- **Example: Dynamic routing with the filter by process.** and **Example: Sync Features with specific Custom Field value:** WorkSharing routing recipes; avoid using mutable display names as the only identity.
- **Example: Map Jira Worklog to Targetprocess time** and **Example: Map Jira Status Targetprocess TeamStates:** reconciliation/mapping recipes; require stable external IDs, non-destructive scope checks, comparator logic, and unmapped-state behavior.
- **UserStories API Reference**, **Bugs API Reference**, **Epics API Reference**, **Features API Reference**, **Tasks API Reference**, **Requesters API Reference**, **Users API Reference**, **Assignables API Reference**, **Generals API Reference**, and **Projects API Reference:** entity-specific GET/POST/DELETE references; prefer the narrowest concrete endpoint and check required fields/permissions before writes.

## Implications for this skills registry

The portal should not become one giant skill. The clean capability boundaries are:

1. REST API v2 query and analytics (already represented in this repository).
2. REST API v1 authentication and CRUD.
3. Historical record queries.
4. Automation Rules design/runtime/troubleshooting.
5. Metrics, calculated fields, and Validation DSL.
6. Entity view and UI-control configuration.
7. Mashups and client extensions.
8. Native issue-level/DevOps integrations.
9. Import/export and CSV automation.
10. Notification templates.
11. Governance and safe operating practices.

Each skill should distinguish normative contracts from recipes, declare read/write/destructive scope, route users to tenant metadata, and include a "verify live" checklist for limits, permissions, entity names, custom fields, version-gated behavior, and external integration enablement.
