# Lookup and Relations-tab columns

## Surfaces

The same column concepts appear in:

- Relations-tab lookup.
- Native reference fields.
- Entity custom fields.
- Embedded-list lookup.
- Existing related-items list on the Relations tab.

Identify the surface before applying configuration; schemas are similar but placement differs.

## Column design

1. Start from the current lookup/list configuration.
2. Add a column using the documented field/property selector.
3. Give it a display label only when needed.
4. Reorder by moving complete column objects.
5. Add an entity filter at the lookup source, not as visual hiding after selection.
6. Preserve identity/name columns so users can distinguish candidates.

For `relations.container`, lookup columns are configured separately for inbound and outbound relations:

```json
"inbound": {
  "lookup": {
    "setup": [{
      "title": "Assigned Team",
      "columnId": "assignedTeam",
      "selector": "assignedTeam.name"
    }],
    "layout": ["entityType", "name", "assignedTeam"]
  }
}
```

- Use `outbound` for the other direction.
- Configure each relation type/direction separately.
- `setup` defines new/custom columns.
- `layout` defines visible columns and order.
- The ID column is always first and should not be listed.
- If `layout` is omitted, a new column can append while defaults remain; when `layout` is supplied, list every default column you want to keep.

Default lookup columns include type, name, state, project, release, and team where applicable.

## Entity filters

- Use API/entity fields supported by every candidate type.
- Prefer stable IDs for project/team/state restrictions.
- Verify that permissions are still enforced by the underlying lookup.
- Define behavior for already-selected entities that no longer match.

## Relations-specific concerns

- Confirm inbound/outbound direction and relation type.
- A relation's ID differs from the related entity ID.
- Columns available on one related type may be absent on another.
- Avoid filters that hide an existing relation and make it impossible to understand/remove.

Columns displayed in the Relations-tab lists use `columns` rather than `lookup`:

```json
"inbound": {
  "columns": {
    "setup": [{"columnId": "userstory", "selector": "userstory.name"}],
    "layout": ["userstory", "entityType", "name", "project", "entityState"]
  }
}
```

Relations-list column setup has no `title` because titles are not displayed there. Dependency Type and ID remain present regardless of configuration; other defaults must be included in `layout` to remain visible.

The portal warns that some example JSON contains whitespace unsuitable for direct paste in the current editor; normalize/validate through the editor rather than copying blindly.

## Limitations

The portal documents surface-specific limitations; verify current support for custom fields, multiple entity types, ordering, and embedded lists. Do not assume a column configuration valid in a native lookup is valid on the Relations tab.

## Verification

- Add/edit/reorder columns.
- Search and selection.
- Existing and new relations.
- Mixed related types.
- Permission-restricted entities.
- Renamed/custom fields.
- Empty and large candidate sets.

## Source

- https://www.ibm.com/docs/en/targetprocess/tp-dev-hub/saas?topic=customization-advanced-columns-in-lookups-relation-tab
