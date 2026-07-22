# Mashup lifecycle and compatibility

## Prefer supported configuration

Before custom code, check Detailed View configuration, UI Controls, Automation Rules, Metrics, and notification templates. A supported configuration is less coupled to the client runtime.

## Install from library

- Administrators manage Mashups under System Settings.
- Refresh/check for library updates before choosing a package.
- Review source, ownership, license, version, network calls, data access, and placeholders before enablement.

## Manual installation

Preserve exact source in version control. Record:

- Mashup name/version/owner.
- Placeholder(s).
- Enabled state.
- User/default-role targeting.
- External origins and dependencies.
- Targetprocess version last tested.

## Disable and delete

Distinguish:

- Enabled/Disabled switch.
- Disabling/removing a placeholder.
- Deleting the Mashup.
- Temporarily disabling all Mashups for diagnosis.

Use the least destructive rollback first and preserve source/settings before deletion.

## User and cookie scope

The portal supports applying a Mashup to selected user IDs or default roles. Treat display targeting as execution scope, not data authorization.

Where supported, restrict a Mashup from using logged-in user cookies. External code should not inherit Targetprocess session authority without explicit need and review.

## Compatibility review

- Test page navigation without full reload.
- Test multiple Targetprocess processes/entity types.
- Test empty, loading, error, and permission-filtered data.
- Test browser support and CSP/external origins.
- Test with other installed Mashups for selector/component collisions.
- Re-test after every Targetprocess client update.

## Sources

- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=mashups-overview
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=mashups-create-your-first-mashup
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=mashups-board-list-timeline-views
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=views-board-list-timeline
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=views-dashboards
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=views-timesheet
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=views-settings
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=mashups-capacity-mode-pi-planning-board
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=mashups-install-from-library
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=mashups-install-code-mashup-manually
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=mashups-deactivate-delete
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=mashups-customize-settings
