# IP Plan Validator

Single-file browser app for validating and editing IP plans. Built with Vue 3 and Tailwind via CDNs.

## Quick Start
1. Open `ip-planner.html` in a modern browser.
2. Upload a semicolon-separated CSV or click **Vytvorit novy plan**.
3. Edit cells by double-clicking.
4. Use the toolbar to validate, detect gaps, sort, and export.

## Features
- CSV import/export (semicolon `;` delimiter)
- Inline editing with calculated fields
- Validation for invalid CIDR/IPs, overlaps, and ordering
- Gap detection and auto-fill for free space
- Visual grouping by Region/Segment/MG/Subscription/VNet
- Side panel with errors, warnings, and gaps

## CSV Schema
Headers must match exactly:
- `Region`
- `Network segment`
- `Management Group`
- `Subscription`
- `VNet name`
- `Subnet type`
- `Start`
- `Address space`
- `Subnet purpose`
- `Subnet name`
- `Available addresses`
- `Comments`

Notes:
- `Start` is a CIDR (e.g. `10.0.0.0/24`).
- `Address space`, `Subnet name`, and `Available addresses` are computed in the UI.
- Available addresses assume Azure behavior (total minus 5).

## Tips
- **Hledat mezery** finds gaps within each VNet scope.
- **Doplnit** inserts free-space rows marked `<scope> free space`.
- Use arrow buttons in the Actions column to reorder rows manually.

## Development
There is no build system. All UI, styles, and logic are in `ip-planner.html`.

## Recent Fixes (2026-02-04)

1. **Fixed `pendingRows` bug** - Removed undefined reference that caused error when filling gaps
2. **Improved gap filling** - Gaps are now split into multiple properly aligned CIDR blocks for complete coverage
3. **Cascading IP changes** - When editing a parent IP (e.g. VNet), all child subnets are automatically updated with the same offset
4. **Cascading delete** - When deleting a parent row (e.g. Subscription), option to delete all child subnets within its IP range
5. **Customizable subnet naming** - Double-click "SUBNET NAME" header to configure naming pattern (default: `{purpose}-subnet-{region}-{start}`)

## Known Limitations

### Validation
- Overlaps are only checked between subnets and free-space rows, not between all allocation levels

### Gap Filling
- Very large gaps may generate many small CIDR blocks (up to 100 max)

## Potential Improvements

### Medium Priority
1. **Undo/Redo** - Track changes and allow reverting
2. **Sort by IP** - Automatic reordering of rows by IP address
3. **Bulk operations** - Select multiple rows for delete/move
4. **Search/filter by text** - Find rows containing specific text
5. **Keyboard navigation** - Arrow keys, Tab for editing, Ctrl+S for export
6. **Import from Excel** - Parse .xlsx directly (using SheetJS)

### Low Priority
7. **Dark mode** - User preference toggle
8. **Column resizing** - Drag to resize columns
9. **Sticky first column** - Keep row numbers visible while scrolling
10. **Print view** - Optimized CSS for printing
