# Release 2026-09-04-46fd790 — Staging QA Guideline

**Environment:** Staging
**Version:** 2026-09-04-46fd790
**Date:** 2026-09-04 (UTC)
**Scope:** PRs merged since 2026-09-03 (previous successful staging deploy)

## New features to test

### Units of Measure management (Catalog)
A new "Units of Measure" list is now managed directly in the Catalog module (create, edit, and bulk import/export via Excel), instead of unit values being free-text entries scattered across parts and line items.

- **Page/flow:** Catalog → Units of Measure (new list page, create/edit modals, Excel download/upload)
- **Happy path:** create a new unit of measure, confirm it appears in the list, edit its details, download the list to Excel and re-upload it.
- **Edge cases:**
  - Creating a duplicate unit name/code
  - Editing or attempting to delete a unit that is already referenced by a catalog part, BOM line, quote line, estimation line, or scope item
  - Uploading an Excel file with missing/invalid rows
  - Permission check: confirm only roles with catalog-admin rights see the new menu item and can create/edit/delete

Cite PR #551, #557

### Unit of measure is now a real lookup across Catalog Parts and Build Room
Catalog parts, assemblies, and Build Room line items (BOM lines, estimation lines, quote lines, scope items, component types) now select their unit from the new Units of Measure list instead of typing it in as text.

- **Page/flow:** Catalog Parts create/edit and "Create/Edit Assembly" modals; Build Room BOM Review and Estimate work surfaces (BOM lines, estimation lines, quote lines, scope items, component types)
- **Happy path:** create or edit a catalog part and assembly component, picking a unit from the new dropdown; create/edit a BOM line, estimation line, quote line, and scope item and confirm the chosen unit displays correctly and persists after save.
- **Edge cases:**
  - Records created **before** this deploy — confirm their previous unit value carried over correctly to the new lookup (see migration risk below)
  - Changing a catalog part's default unit and confirming BOM lines that already recorded a different unit are unaffected
  - Assembly components inheriting a unit vs. explicitly overriding it
  - Removing/deactivating a unit that is currently in use elsewhere

Cite PR #551, #557

## Changes & fixes to re-test

### Error pages no longer misreported as server errors (backend telemetry only)
Plain "page not found" or "access denied" pages are no longer logged internally as application errors. This is a monitoring change only — the pages users see are unchanged.

- **Regression check:** navigate to a bad/nonexistent URL (404) and to a page you don't have permission for (403/401) and confirm the same error page renders as before. If there is a way to trigger a genuine server error in a test environment, confirm it still shows the error page correctly.

Cite PR #556

### Document viewer diagnostics (re-test document viewing end to end)
Additional internal diagnostics were added around opening and paging through PDF documents (open, virtual load, per-page rendering, session summary on close). No user-facing behavior is intended to change, but the document viewer touches shared code paths, so it needs a full pass.

- **Regression check:**
  - Open small and large PDF documents in the document review flow
  - Scroll and page through a multi-page document, including scrolling back to previously viewed pages
  - Close/exit a document viewing session and re-open it

Cite PR #556

## Risk / regression areas

- **Data migration risk:** this deploy includes a schema change that removes the old unit-of-measure columns from catalog parts, scope items, quote lines, estimation lines, component types, and BOM lines, replacing them with references into the new Units of Measure table. Spot-check existing staging records (created before this deploy) across all of these areas to confirm their unit values were preserved correctly and nothing displays blank or incorrect.
- **Cross-module impact:** unit of measure now spans Catalog and Build Room. Run a full end-to-end flow — create/update a catalog part, add it to a BOM, generate an estimation and a quote — and confirm the unit is consistent at every step.
- **Permission scoping:** a new permission area was introduced for managing Units of Measure. Verify access is correctly scoped by role and that users without catalog-admin rights cannot reach the new management screens via direct URL.
- **Notification/telemetry volume:** the additional document-viewer logging is expected to be low-volume, but keep an eye on general app responsiveness while using the document viewer during this QA pass.

## No QA action

- A logging-level change (PR #555) was opened during this window but was closed without being merged — it is **not** part of this deploy.

## Bug & Test Tracking

| Bug ID | Test Case ID | Priority | Module | Status | Assigned To | Comments |
|--------|--------------|----------|--------|--------|-------------|----------|
| | | | | | | |
| | | | | | | |
| | | | | | | |
| | | | | | | |
| | | | | | | |
