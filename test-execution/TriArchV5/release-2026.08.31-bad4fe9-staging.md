# Release 2026.08.31-bad4fe9 — Staging QA Guideline
**Environment:** Staging
**Version:** 2026.08.31-bad4fe9
**Date:** 2026-08-31
**Scope:** PRs merged since 2026-08-28

## New features to test

### Assign / unassign drawing sheets to building floors (#537)
- **Page/flow:** BuildRoom → Project → Take-Off stage → **Building Floors** panel (side panel opened from the plan viewer).
- **Happy path:** Open the panel and confirm floors are split into **Assigned floors** and **Unassigned floors**. On an unassigned floor, click **Assign to…**, pick a sheet from the dropdown, and confirm the floor moves into Assigned with a checkmark pill showing the sheet number and a success toast. From an assigned floor, click **Unassign** and confirm it moves back to Unassigned floors.
- **Add a floor:** Click **+ Add Floor N** in the panel footer, fill in the nested Add Floor panel, and confirm the new floor appears in Unassigned floors and the floor count/label updates.
- **Edge cases:**
  - User without assign permission: **Assign to…** is disabled with an explanatory tooltip.
  - No unassigned sheets left: **Assign to…** is disabled with a "no sheets available" tooltip instead of an empty dropdown.
  - User without add-floor permission: **+ Add Floor** button is disabled with a tooltip.
  - Try to assign a sheet that's already linked elsewhere, or unassign a floor with no current link — confirm a clear error message rather than a silent failure or crash.
  - Floors flagged as a coverage "gap" (covered by a typical floor) remain read-only as before and are unaffected by the new assign/unassign controls.

### General Contractor & Contacts directory usability upgrades, Catalog list polish (#538)
- **Pages/flow:** BuildRoom → **General Contractors**, BuildRoom → **Contacts**, and Catalog → **Catalog Parts / Category Parts / Discount Structures**.
- **Happy path:** On the General Contractors and Contacts list pages, use the search box — typing should filter after a short pause rather than on every keystroke. Open the new **"What this page is for"** help panel (question-mark icon near the title) and confirm the guidance steps read correctly. Create a new General Contractor or Contact and confirm the new row is visibly highlighted in the list once it reloads, and the list shows newest-first by default.
- **Contacts from a GC panel:** Open a General Contractor's **Contacts** side panel, click **+ New**, and confirm the General Contractor field is pre-filled and locked. Confirm the new contact appears in that panel's list immediately (no manual refresh needed) with a "Contact created" toast, and the empty-state message correctly invites you to create one when the list is empty.
- **Catalog lists:** Create or edit a Catalog Part, Category Part, or Discount Structure and confirm the saved row is highlighted, and that these lists also default to newest-first ordering.
- **Edge cases:**
  - Rapid typing in the search box should not spam requests and should settle on the correct filtered result.
  - Users without create permission should see creation controls hidden/disabled as before — confirm nothing regressed here.
  - Navigating from Project → Create should land on the Projects list with the newly created project highlighted.
  - Row actions (Edit/Delete/export to Excel) on Contacts, General Contractors, and Catalog Parts lists still work correctly after the internal list wiring changes.

## Changes & fixes to re-test

### Take-Off plan viewer toolbar re-ordered (#536)
- **What changed:** The floating tool rail on the Take-Off plan viewer (Select, Pan, Place, Search, Fit, Zoom, etc.) was re-arranged into more logical groups with separators.
- **Regression check:** Open the Take-Off plan viewer and confirm every tool button (Place, Select, Search, Pan, Fit-to-page, and any zoom controls) is present, visually grouped sensibly, still toggles its active/pressed state correctly, and each tool still performs its function on the canvas.

### List sort order changed on several BuildRoom/Catalog pages (#538)
- **What changed:** Projects, Contacts, General Contractors, Catalog Parts, Category Parts, and Discount Structures lists now sort newest-first by default instead of alphabetically, and some lists' internal row-data handling changed.
- **Regression check:** Confirm each list still loads, sorts, paginates, and exports to Excel correctly, and that row actions (Edit/Delete/View) still target the correct row.

## Risk / regression areas

- **Multi-tenancy:** Confirm the Building Floors assign/unassign flow and the GC/Contacts/Catalog list changes stay correctly scoped to the current tenant and project — no cross-tenant or cross-project data should ever appear.
- **Permission scoping:** Exercise the new assign/unassign/add-floor/create-contact controls with a lower-privileged QA role and confirm restricted actions are unavailable both in the UI and if attempted directly against the API.
- **Notifications:** Confirm success/error toasts for assign, unassign, add-floor, and contact-creation flows appear once, with accurate wording, and don't stack or duplicate.
- **Performance:** With a larger data set (many contacts / catalog parts), confirm the new search debounce and highlight-on-save behavior stay smooth with no noticeable lag or layout jump.

## No QA action

- None — all PRs in this release cycle contain user-facing changes.

## Bug & Test Tracking

| Bug ID | Test Case ID | Priority | Module | Status | Assigned To | Comments |
|--------|--------------|----------|--------|--------|-------------|----------|
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
