# Release 2026.09.04-8df3e23 — Staging QA Guideline

**Environment:** Staging
**Version:** 2026.09.04-8df3e23
**Date:** 2026-09-04
**Scope:** PRs merged since 2026-09-04 07:05 UTC (previous successful staging deploy)

## New features to test

**Project Activity Log** (#559)
Page/flow: Project Detail → Activity tab.
Happy path: perform a range of project actions (approve a quote, seal/reopen a version, submit/return a quote, set code-add mode, lock an estimate, create a new project) and confirm each shows up in the day-grouped log with a plain-English description rather than a raw code.
Edge cases:
- Filter the log by each category (All / Gates / Seals & versions / Documents) and confirm the filtered set and counts are correct.
- Create a brand-new project and confirm it immediately shows a baseline "project created" entry.
- View the log as a user without access to the project — confirm it shows a distinct "you may not have access" state, not an empty log.
- Trigger an action whose detail payload can't be parsed and confirm the row falls back to a readable action label instead of a blank/raw value.

**Reworked assembly builder for Catalog Parts** (#560)
Page/flow: Catalog → Catalog Parts → Create/Edit Assembly.
Happy path: create a new assembly, search and add several component parts, set quantities per row, confirm the assembly total and per-unit total update live, save.
Edge cases:
- Try to save with a missing name, catalog number, unit, category, or zero components — confirm each shows its own specific inline message.
- Add and remove several rows in different orders — confirm row numbers and totals stay correct throughout.
- Try to add an assembly as a component of another assembly — confirm it's rejected (no nesting).
- Edit an existing assembly's components and confirm labor/net totals reflect the change after saving and reloading.

**Full-screen quote document viewer** (#560)
Page/flow: Estimation → Quote Review.
Happy path: open a quote document, click "Full screen," confirm the viewer expands and stays usable, then "Exit full screen" restores the normal layout.
Edge cases: toggle full screen on documents of different page counts/sizes; confirm sign/return actions on the quote still work correctly while in and after leaving full-screen mode.

## Changes & fixes to re-test

**Units of Measure bulk-delete removed** (#562)
What changed: the bulk-select/checkbox/"delete all matching" controls were removed from the Units of Measure list, because every seeded (built-in) or in-use unit always refused deletion anyway — the controls could only end in a refusal after a confirmation prompt.
Regression check: confirm the Units of Measure list no longer shows bulk-select checkboxes or a select-all banner; deleting a single built-in unit via its row action still shows a clear "this is a built-in unit and can't be deleted — deactivate it instead" message (distinct from the message shown when trying to *edit* a built-in unit's code/factor); deactivating a unit still hides it from pickers while parts that already use it keep working.

**Duplicate floor/room names now rejected** (#563)
What changed: building setups — including floors auto-created from ITB plan-page extraction — now reject two floors or two rooms sharing the same name (case-insensitive) instead of silently creating ambiguous duplicates.
Regression check: import/seed a building from ITB plans where two plan pages share the same title — confirm they collapse onto one shared floor when their sheet numbers differ, or are kept as separate floors with a disambiguating "(2)" suffix when there's no sheet number to distinguish them; manually create two storeys or two spaces with the same name and confirm a clear "two floors/rooms share the name…" error appears before anything is saved.

**Location / time-zone lookup account corrected** (#561, #564)
What changed: the third-party service account used for city and time-zone lookups on Staging and Production was pointed at the correct Triarch account after being briefly misconfigured with an unrelated one.
Regression check: in ITB creation, confirm the bid-deadline time zone picker loads and lists time zones correctly on staging, with no lookup errors in the flow.

## Risk / regression areas

- **Building/floor seeding paths**: the floor-name matching and de-duplication logic is shared across manual building setup, ITB-plan-based seeding, and drawing-sheet floor assignment. Exercise all three paths, not just the one most directly touched, since a change to the shared logic can surface differently in each.
- **Shared list-page bulk-actions component**: the mechanism used to turn off bulk delete for Units of Measure (#562) lives in a component shared by other list pages. Spot-check bulk delete on a couple of unrelated list pages to confirm their checkboxes/select-all/delete-all behavior is unaffected.
- **Multi-tenancy / permission scoping on the new Activity Log**: the log surfaces who did what and when at the project level. Exercise it as users with different project-level access and confirm no cross-project or cross-tenant data is visible, and that "you may not have access" never leaks whether activity exists.
- **Assembly totals accuracy**: spot-check that a saved assembly's net cost and labor total match a manual calculation from its component rows and quantities, across a couple of different units of measure, since totals are computed before save.

## No QA action

No CI-only, infrastructure-only, or documentation-only PRs fell in this window — every merged change listed above has at least one user-visible surface to verify.

## Bug & Test Tracking

| Bug ID | Test Case ID | Priority | Module | Status | Assigned To | Comments |
|--------|--------------|----------|--------|--------|-------------|----------|
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
