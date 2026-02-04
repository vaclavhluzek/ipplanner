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
- Use **Seradit dle IP** before validation if rows are out of order.
- **Hledat mezery** finds gaps within each VNet scope.
- **Doplnit** inserts free-space rows marked `Vnet free space`.

## Development
There is no build system. All UI, styles, and logic are in `ip-planner.html`.
