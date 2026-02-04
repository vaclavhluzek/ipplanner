# AGENTS.md

## Project Summary
Single-file, browser-only tool for validating and editing IP plans. The UI is built with Vue 3 (CDN) and Tailwind (CDN) and lives entirely in `ip-planner.html`.

## How To Run
Open `ip-planner.html` in a modern browser. No build step, no server.

## Data Format (CSV)
The app expects semicolon-separated CSV with these headers (case-sensitive):
- `Region`
- `Network segment`
- `Management Group`
- `Subscription`
- `VNet name`
- `Subnet type`
- `Start` (CIDR, e.g. `10.0.0.0/24`)
- `Address space` (computed)
- `Subnet purpose`
- `Subnet name` (computed)
- `Available addresses` (computed, Azure-style: total minus 5)
- `Comments`

## Core Behaviors
- Upload CSV or start a blank plan.
- Editable cells (double-click to edit). Computed fields update when `Start` changes.
- Validation detects:
  - Invalid CIDR/IPs
  - Overlaps
  - Subnet not contained in parent scope (segment/MG/subscription/VNet)
  - Incorrect order by `Start`
- Gap detection between networks (per VNet) and auto-fill with `<scope> free space` rows.
- Actions: validate, detect gaps, fill gaps, recalculate, export, reset.

## Editing Guidance For LLMs
- Keep everything in `ip-planner.html` unless explicitly asked to split files.
- If you change the CSV schema, update table headers, export, and any logic that references keys.
- Prefer small, localized edits; the file is large and monolithic by design.
- Avoid adding new build tooling unless the user requests it.

## File Map
- `ip-planner.html`: All UI, logic, and styles.
- `README.md`: Human-facing usage notes.
- `AGENTS.md`: This file.

## Fixed Bugs & New Features (2026-02-04)

### 1. `pendingRows` undefined - FIXED
Removed the undefined `pendingRows.add()` call. The `_pending: true` flag on rows already handles the pending state UI.

### 2. Gap prefix alignment - FIXED
Added `splitGapIntoCIDRs()` function that properly splits gaps into multiple aligned CIDR blocks. Now a gap like `10.0.1.0 - 10.0.2.255` correctly generates two `/24` entries.

### 3. Cascading IP changes - NEW FEATURE
When editing the `Start` column, the app detects all child rows (subnets within the parent range) and offers to update them automatically with the same IP offset.

**Implementation details:**
- `startEdit()` stores the original network when editing Start column (`editOriginalNetwork`)
- `onStartEdit()` compares old vs new IP and calls `cascadeIPChanges()` if different
- `cascadeIPChanges()` finds all child rows (strictly inside old range, smaller prefix), calculates offset, and updates their Start, Address space, and Subnet name

## Remaining Limitations

- Overlaps are only checked between subnets/free-space, not between all allocation levels
- Very large gaps may generate up to 100 CIDR blocks (safety limit)

## Suggested Improvements

### High Value
1. Undo/redo stack (store snapshots of `csvData`)
2. Keyboard shortcuts (Ctrl+Z undo, Ctrl+S export, arrows + Enter for navigation)
3. Sort by IP address (automatic reordering of rows)

### Medium Value
4. Bulk row selection and operations
5. Text search across all columns
6. Column visibility toggle
7. Import Excel .xlsx (SheetJS library)

### Low Priority
8. Dark mode toggle
9. Print-optimized CSS
10. Sticky row numbers column
11. Drag-and-drop row reordering
