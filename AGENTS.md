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
- Gap detection between networks (per VNet) and auto-fill with `Vnet free space` rows.
- Actions: validate, detect gaps, fill gaps, sort by IP, recalculate, export, reset.

## Editing Guidance For LLMs
- Keep everything in `ip-planner.html` unless explicitly asked to split files.
- If you change the CSV schema, update table headers, export, and any logic that references keys.
- Prefer small, localized edits; the file is large and monolithic by design.
- Avoid adding new build tooling unless the user requests it.

## File Map
- `ip-planner.html`: All UI, logic, and styles.
- `README.md`: Human-facing usage notes.
- `AGENTS.md`: This file.
