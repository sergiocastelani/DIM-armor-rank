# Copilot Instructions for DIM-armor-rank

## Project Overview

This is a static web application (single HTML file) that analyzes Destiny Item Manager (DIM) armor data exported as CSV. It identifies "obsolete" armor pieces by finding the best item in each stat configuration group and marking lower-stat duplicates for deletion.

## Architecture

**Single-file client-side app**: All logic is contained in `index.html` with inline CSS and JavaScript. No build process, dependencies, or server required.

**Processing flow**:
1. User uploads CSV from DIM Organizer
2. Filters out Exotic items (user keeps all Exotics)
3. Groups remaining armor by some stats
4. Within each group, identifies the highest total-base-stat item as "winner"
5. All other items in the group are marked "obsolete" (candidates for deletion)
6. Displays results in expandable tree view with winner at top, obsolete items nested below

**Key grouping logic** (lines 241-266):
- Extracts base stat values for 6 stats: weapons, health, class, grenade, super, melee
- Sorts them descending to find top 3 stats for that armor piece

## Key Conventions

**No external dependencies**: Vanilla JavaScript only. No npm, no build tools, no frameworks.

**CSV parsing**: Custom parser function (lines 442-464) handles quoted fields with commas and newlines. Do not replace with external CSV library.

**DIM ID format**: When copying to clipboard, uses DIM search syntax: `id:abc123 or id:def456 or id:ghi789`
- This format allows direct paste into DIM's search bar to select multiple items for batch deletion

**Checkbox state**: Each winner node has a checkbox to track which groups have been copied to clipboard. Not persisted (resets on page reload).

## Development

**Testing**: Open `index.html` directly in browser. No local server required.

**CSV test files**: Export armor data from DIM Organizer (https://destinyitemmanager.com) → Go to Organizer → Chosse any class → Download Armor.csv

**No build/lint/test commands**: Static HTML file with no toolchain.

## Common Patterns

When modifying stat grouping logic, remember:
- Exotic items are always excluded (line 236)
- Groups must have 2+ items to be shown (line 275) - no point showing a "winner" with no obsolete items
- Sort order matters: results are sorted by character class (lines 295-299) to group Hunter/Titan/Warlock items together

When adding stat columns:
- Update `statNames` array (line 182)
- Update table headers in `showResults()` (lines 386-394)
- Update table row cells (lines 397-409)
