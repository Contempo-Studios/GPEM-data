# GPEM-data

Public runtime data for the **GPEM** iOS app (published by ContempoStudios Ltd).

Files in this repo are fetched at runtime by the app from `raw.githubusercontent.com`. Edits to `main` take effect for installed apps on their next launch — no App Store update is required.

## Files

### `practices.json`

A JSON array of UK GP practice names. Used by the onboarding flow's practice picker (`PracticeDirectory.swift` in the GPEM app).

If the app cannot reach this file (offline, first launch on a new device, etc.) it falls back to a hardcoded list inside the binary.

To add/remove a practice: edit the file in the GitHub web UI, commit to `main`.

### `seed-catalog.json`

The default emergency medicines/equipment catalogue seeded into the SwiftData store on first launch. Used by `CatalogSeeder.swift` in the GPEM app.

Each item has:

- `name` / `shortName` — full and abbreviated display names
- `category` — `medicine`, `controlledDrug`, `equipment`, `consumable`, or `diagnostic`
- `drawer` — physical location (`1`–`5` numbered drawers, `10` defib, `11` fridge, `12` basement, `99` other)
- `expectedQuantity` — target stock level
- `sortOrder` — display order within drawer
- Optional flags: `isSiteSpecific`, `isOptional`, `isSiteActive`, `notes`

**Important:** a copy of this file is also bundled inside the iOS app as an offline fallback. When you change this file, the change is live immediately for existing installs. For *new* installs that are offline on first launch, the bundled copy is used — so keep `Stock/Resources/seed-catalog.json` in the private GPEM repo in sync with this one before any App Store release.

## Why a separate public repo?

The GPEM source repo is private. `raw.githubusercontent.com` requires the host repo to be public for unauthenticated access. Splitting data into its own public repo keeps the source private while letting installed apps fetch the data without credentials.
