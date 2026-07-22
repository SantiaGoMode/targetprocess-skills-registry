# External detailed-view components

## Registration boundary

The layout renderer exposes a client-side registration API for React components implemented by external Mashups/integrations. Keep registration and Targetprocess context access in a small adapter so client changes do not spread through business logic.

## Context and dynamic visibility

`context.entity` includes the opened entity's identity. Use it to load/render the correct data and to drive documented `visibilityConfig` behavior.

- React to in-app navigation where the entity changes without a full reload.
- Guard missing/stale entity context.
- Cancel old async work.
- Never use UI visibility as authorization.

## Sample component families

Portal examples cover Feature state on User Story, copying User Story details, Epic on User Story, external links, rich text, and progress by custom field. Use them as skeletons only:

- Replace hard-coded field names/IDs/URLs.
- Sanitize rich text and links.
- Verify clipboard/browser permission and fallback.
- Define null/unsupported entity behavior.
- Avoid assuming a custom progress field has 0–100 valid data.

## CSS and sibling-marker workaround

The documented sibling-marker/classes workaround depends on rendered markup. Treat it as a last resort:

- Scope selectors narrowly.
- Feature-detect the expected structure.
- Fail without hiding/breaking native UI.
- Add a regression test after client upgrades.

## Cleanup

Unmount React roots; remove event listeners, timers, observers, DOM nodes, and subscriptions; abort/ignore outstanding requests; clear instance-scoped caches.

## Verification

- Direct open and in-app navigation.
- Supported and unsupported entity types.
- Empty/null/custom-field data.
- Role/permission differences.
- Multiple component instances.
- Client upgrade and coexistence with detailed-view JSON changes.

## Sources

- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=customization-external-mashup-support
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=support-using-componentfactory-dynamic-filtering
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=support-sibling-markerworkaround-classes-stack-element
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=support-mashup-feature-state-user-story
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=support-mashup-copy-user-story-details-clipboard
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=support-mashup-epic-userstory
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=support-mashup-external-link
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=support-mashup-rich-text
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=support-mashup-progress-by-custom-field
