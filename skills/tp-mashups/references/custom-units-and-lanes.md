# Custom Units and Lane Headers

## Custom card units

Custom Units extend card layouts on Board, List, and Timeline views.

Define:

- Supported entity/model types.
- Card sizes (S, M, L, and any documented variants).
- Fields/selectors required by the unit.
- Render output and null/loading state.
- Styles scoped to the unit.
- Whether the unit is editable and on which models/views.

Do not assume a unit valid on one card size or entity type works elsewhere. Test every declared combination.

## Editable units

For editable behavior:

- Confirm field write support and user permission.
- Validate input type and references.
- Handle rejected Validation Rules/API writes visibly.
- Prevent duplicate saves on re-render.
- Keep read-only rendering available when edit is disallowed.

## Custom Lane Headers

Lane-header Mashups can add existing or custom units to Board/Roadmap row headers.

Define:

- Row entity type.
- Target all applicable views or a specific view.
- Required fields, for example assigned Developers or Owner.
- Fallback for no assignment/owner.
- Width/overflow behavior.

View scoping is part of correctness; a global row-header change may affect unrelated boards.

## Styling

- Scope CSS under a Mashup-owned class.
- Do not target transient generated classes or DOM position.
- Support long text, missing icons, dark/light themes where applicable, and responsive layout.

## Verification

- Every entity type and card size.
- Board, List, Timeline, and Roadmap where declared.
- Empty/loading/error/permission states.
- Edit allowed/denied and validation failure.
- View-specific versus global scope.
- Coexistence with other card/lane customizations.

## Sources

- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=mashups-custom-units
- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=mashups-custom-lane-headers
