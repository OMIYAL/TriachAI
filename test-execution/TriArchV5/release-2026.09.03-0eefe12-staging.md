# Release 2026.09.03-0eefe12 — Staging QA Guideline

**Environment:** Staging
**Version:** 2026.09.03-0eefe12
**Date:** 2026-09-03
**Scope:** PRs merged since 2026-08-31

## New features to test

**In-browser document viewing (BuildRoom project documents)** — PR #541, #545
The "Download" action on a project's Documents tab is now "View" and opens the PDF inline in a new browser tab instead of forcing a download.
- Happy path: open a project with an uploaded document, click **View**, confirm the PDF renders inline in a new tab with a sensible filename (not a GUID).
- Edge cases: a document whose file is missing/unavailable (button should be disabled with an explanatory tooltip, not error); a non-PDF/corrupt file; opening the same document twice in quick succession; a user without document-view permission; confirm the viewer tab cannot be used to navigate elsewhere in the app (should behave as a sandboxed view).

**Faster, more reliable ITB intake processing** — PR #544, #548
ITB document analysis now hands the analyzer a direct, short-lived link to the uploaded file instead of always uploading the full file through the app; if the link approach fails, it automatically falls back to the old upload method. This release also ships an updated extraction analyzer (v4.1) and a refreshed Take-Off stage page.
- Happy path: upload a typical ITB PDF and confirm it reaches "Parsing" and completes extraction as before.
- Edge cases: a very large ITB file; an ITB upload during a transient storage hiccup (should still complete via fallback, just possibly slower); confirm extracted field accuracy on a document you've tested before (analyzer version changed); visually check the Take-Off stage page for layout regressions (count log, symbol detection panel).

**Time zone suggestion for bid deadlines** — PR #546
When creating a project from an ITB, the app now suggests the bid deadline's time zone based on the project ZIP/postal code, using IANA time zone names.
- Happy path: create a project from ITB intake with a US ZIP code and confirm a sensible time zone is pre-filled/suggested.
- Edge cases: an invalid or non-US postal code (should degrade gracefully, not error); manually overriding the suggested zone; a ZIP near a time zone boundary; confirm floor-detail labels now correctly describe sheets as linked to floors (not storeys).

**Price import conflict review panel** — PR #539
The price import review screen can now show a conflicts banner/panel highlighting rows that couldn't be auto-matched (e.g. brand mismatches) before the import is applied.
- Happy path: import a price file with at least one ambiguous/conflicting row and confirm the conflicts panel lists it clearly, separate from clean matches.
- Edge cases: a file with zero conflicts (panel should not appear); a file that is entirely conflicts; resolving a conflict and re-previewing before apply.

**Clearer price import errors & offline-friendly storage** — PR #532
Failed price import operations now show specific, friendly error messages instead of generic failures, and import files are stored via the database rather than Azure blob storage (reduces dependence on internet connectivity for this workflow).
- Happy path: run a normal price import end-to-end and confirm step-by-step progress/guidance is clear.
- Edge cases: force an import failure (e.g. malformed file) and confirm a specific, actionable error appears rather than a generic failure banner; import while offline/degraded network, if testable in staging.

**Catalog price book: part "Name" field and clearer bulk repricing** — PR #549
Price import now accepts an optional short "Name" column (separate from the longer description) when creating new catalog parts, and the bulk reprice control on the Catalog Parts page now clearly labels "% off trade list" vs "multiplier on trade list."
- Happy path: import a price file that includes a Name column and confirm new parts show that name in the price book, not the full description.
- Edge cases: a file with no Name column (should fall back to Description, then catalog number); a Name or Description far longer than the field limit (should trim, not fail the import); check the price book table with long descriptions no longer stretches/breaks column widths; toggle the bulk reprice mode between percent and multiplier and confirm the label above the input updates correctly.

**Contacts / General Contractors panel enhancements** — PR #540
UI enhancements to the BuildRoom Contacts and General Contractors create/edit panels and listings.
- Happy path: create, edit, and list a contact and a general contractor and confirm the forms and grid behave as expected.
- Edge cases: validation messages on required fields; editing an existing contact/general contractor tied to multiple projects.

## Changes & fixes to re-test

**Bid deadline could be wrongly rejected as "in the past"** — PR #550
Creating a project with a bid deadline was comparing the raw stored value against UTC time without first converting it from the project's own time zone, so a deadline that was genuinely hours in the future could be rejected as invalid depending on the time zone selected.
- Regression check: create a project with a bid deadline set several hours ahead in a non-UTC time zone (especially one behind UTC) and confirm it saves successfully. Also try a deadline just a few minutes in the future and confirm it is still correctly rejected as too soon/past where applicable.

**Submit stage "Submitted To" field mislabeled when switching methods** — PR #550
On the Submit work-surface, toggling between "Portal" and "Email" submission methods could leave the wrong label/placeholder text on the "Submitted To" field.
- Regression check: on the Submit stage, switch between Portal and Email methods a few times and confirm the field label and placeholder always match the selected method.

**Large document packages could time out or fail in Document Review** — PR #542
Generating or reviewing large document packages in Document Review could stall or fail outright; packaging drafts and large in-review payloads now use chunked/cached storage so bigger packages complete reliably.
- Regression check: run Document Review / packaging finalize on a project with a large set of documents (as large as staging data allows) and confirm it completes with a clear success state rather than hanging or erroring out partway through.

## Risk / regression areas

- **ITB processing pipeline**: the new direct-storage-link dispatch path for ITB analysis is a meaningful architecture change — watch for any ITB uploads that get stuck in "Parsing" longer than usual, and compare extraction accuracy against a few previously-known-good ITB documents given the analyzer version bump.
- **Document access flows**: the new inline document-viewing endpoint and the ITB storage-link dispatch both mint short-lived access to stored files. Exercise document viewing and ITB upload across more than one project/tenant to confirm each surface only ever exposes files belonging to the project or tenant it's opened from.
- **Large-payload / packaging performance**: retest packaging and document review on the largest realistic document sets available in staging; watch timing, not just success/failure.
- **Price import surfaces**: three separate PRs touched the price import flow this release (conflict panel, friendlier errors, Name field). Run a couple of full end-to-end import passes (clean file, and one with conflicts/errors) rather than testing each PR's slice in isolation.
- **Permission scoping**: spot-check that document viewing, ITB creation, and contact/general-contractor edits still correctly respect the existing role/permission boundaries for each area — no permission changes were intentionally made, but multiple UI surfaces changed this release.

## No QA action

- **#552** — Database-only migration removing an unused column/foreign key from the BuildRoom contacts table. No application behavior change.
- **#553** — CI pipeline fix for the deploy workflow running out of disk space during builds. No application change.
- **#543** — Internal developer tooling: added reference documentation for a PDF-viewer skill used during development.

## Bug & Test Tracking

| Bug ID | Test Case ID | Priority | Module | Status | Assigned To | Comments |
|--------|--------------|----------|--------|--------|-------------|----------|
| | | | | | | |
| | | | | | | |
| | | | | | | |
| | | | | | | |
| | | | | | | |
