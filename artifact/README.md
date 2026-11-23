# Marcele Records Gear — live artifact copy

`marcele-records-gear.html` is a copy of the live Claude artifact "Marcele Records Gear"
(https://claude.ai/artifact/24ViWkTQWRneHwnXpULYsX, version 4, 2026-11-23). The artifact is the
canonical version; its data lives in the artifact's own database and is not in this repository.
`index.html` and `index 4.html` at the repo root are older copies and are left as they were.

## What version 4 adds: the Film Planner bridge

Settings gets two buttons (code only; no data was changed):

- **Export for Film Planner** saves `marcele-inventory-YYYY-MM-DD.json`: every item (code, name,
  category, status, due back, holder when out) and every shoot (dates, status, item codes).
  Film Planner's Shoot → Gear reads it to show availability "as of" the export and to confirm
  reservations.
- **Import shoot from Film Planner** reads the file Film Planner's "Send to inventory" saves and,
  after a confirmation that lists what will change, creates or updates one **prep** shoot per shoot
  day. The shoot is found again by its `externalId`; only the items Film Planner added before
  (`fpItemIds`) are replaced, gear added by hand stays, kits/packing/returns are untouched, wrapped
  shoots are skipped, unknown item codes are listed and ignored. Nothing is deleted.

`film-planner-bridge.diff` is the change against version 3 (lines split at `;` so the diff is readable).
The file formats are documented in the Film Planner repository, `docs/inventory-bridge.md`.

Validation: the patched page was run in jsdom with a seeded inventory: export → 4 items, 1 shoot;
import of a one-day bundle → 1 prep shoot dated that day holding 2 items, unknown code reported,
items unchanged; re-import with fewer codes → same shoot updated, no shoot or item deleted.
