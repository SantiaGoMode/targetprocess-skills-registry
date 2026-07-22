# Dashboard widget contract

The dashboard-widget page includes older version/date cues. Validate every API against the current client before implementation.

## Widget lifecycle

1. Register a widget template.
2. Define settings metadata and settings view.
3. Render using current application/widget context.
4. Respond to settings updates without leaking old resources.
5. Handle resize events.
6. Cancel async work and clean up on widget removal/navigation.

## Template contract

Define stable widget ID/name, render function/component, settings schema, and required data. Avoid global singletons shared across widget instances unless deliberately keyed.

## Settings

- Validate and default every setting.
- Migrate older saved settings when the schema changes.
- Keep secrets out of widget settings.
- Re-render only the affected instance.

## Application context

Treat selected projects/teams/processes and user permissions as runtime inputs. Do not cache one user's/context's data globally or assume dashboard context equals all-account scope.

## Resize and async work

- Subscribe once and unsubscribe on cleanup.
- Debounce expensive layout work.
- Render loading, empty, error, and partial states.
- Cancel/ignore stale requests after settings/context changes.
- Prevent an earlier async response from overwriting newer state.

## React widgets

Mount into a Mashup-owned root and unmount cleanly. Keep the Targetprocess adapter isolated from presentational components. Do not depend on internal React instances or private component APIs.

## Verification

- Multiple instances with different settings.
- Settings edit/reset/migration.
- Resize and dashboard layout changes.
- Navigation and removal cleanup.
- Slow/failing API.
- Permission/project context changes.
- Current Targetprocess release compatibility.

## Source

- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=mashups-dashboard-widgets
