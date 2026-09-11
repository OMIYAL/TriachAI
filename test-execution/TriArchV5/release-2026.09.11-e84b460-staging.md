# Release 2026.09.11-e84b460 — Staging QA Guideline

**Environment:** Staging
**Version:** 2026.09.11-e84b460
**Date:** 2026-09-11
**Scope:** PRs merged since 2026-09-07

## New features to test

**Trade scope creation with a configurable drawing source** (#591)
A new "Add trade scope" panel lets you create a trade scope by picking its trade type, and — either now or later from a settings panel — assign the department that can launch it and choose exactly which drawing set it should read: a specific uploaded PDF and a specific page range (e.g. "1-5, 8, 12-15"), instead of always using the whole project drawing set. Extraction of that page range runs in the background.
- Happy path: create a trade scope, leave drawings unset (should default to the project's intake document, all pages), then later open settings and assign a specific PDF + a custom page range; confirm the take-off/estimate pipeline for that trade only reads the chosen pages.
- Edge cases: only one scope per trade type per project (creating a duplicate should be blocked); entering an invalid page selection (out of order, non-numeric, page 0); a project with no PDFs uploaded yet; department and drawing choices should lock once the trade has been launched; confirm background extraction eventually completes (or reports failure) and the scope reflects it.

**Read-only "assembly components" view in BOM and Take-Off** (#588)
Any line item that is a catalog assembly (a kit of parts) now shows an expandable panel listing the individual parts it's built from, on both the Bill of Materials review screen and the Take-Off work surface.
- Happy path: open a BOM or Take-Off line that references an assembly and confirm its component parts display correctly (name, quantity, unit).
- Edge cases: a line item that is a single part, not an assembly, should not show the panel; an assembly whose underlying catalog part was later deactivated or deleted; an assembly with a large number of components (scrolling/layout).

**Price import and import history** (#584, #592, #595, #598, #590)
The price-file import flow (upload → review → apply) and its history log got a substantial round of work: the review screen now maps unknown file categories/units to existing catalog ones (via "use existing" banners, including a choice when a spelling is ambiguous), blocks assigning to inactive categories/units with a clear message, enforces unique category names, and shows a unit column with a warning pill for unrecognized units. Rows the parser can't read (blank catalog number, bad price, over-long fields) are now called out with a count and a downloadable plain-text error report. The Import History list gained summary cards, date-range/source/status filters, and the detail screen gained filters by catalog number/outcome/brand/description plus better handling for long text and date-range edge cases.
- Happy path: import a price file with a mix of matched updates, new parts, and a couple of unknown category/unit spellings; map the unknowns to existing catalog entries via "use existing"; apply; confirm the batch appears correctly in Import History with accurate counts, and its detail page shows every row's outcome.
- Edge cases: a file with rows that have blank catalog numbers or non-numeric prices — confirm they're counted separately as errors (not silently dropped) and the error report downloads and lists the right rows/line numbers; two or more unknown spellings for the same category — confirm the create button lets you pick which one to create; trying to map to a category/unit that is inactive — should be blocked with an explanation; filtering Import History by an "imported to" date should include that whole day; a long description/brand value in the detail table should clamp/truncate rather than break the layout.

**Labor rates and labor categories** (#586, #589, #593, #599)
A new setup area (now under Catalog, having moved there partway through this window) lets admins define labor categories and time-boxed labor rates (with a fully-loaded rate formula and effective-date windows that can be "closed" or a scheduled rate cancelled). Catalog parts that carry labor units can now be assigned a labor category (required once a part has labor hours; optional on assemblies). From an estimate, you can add a manual labor line against a category, and a supervisor-level action lets you override the resolved rate for a line (with the original resolved rate shown alongside the override so it's auditable), plus edit or remove a manually-added labor line.
- Happy path: create a labor category, create a labor rate for it with an effective date, assign that category to a catalog part, then from an estimate add a manual labor line using that category and confirm it prices using the rate; edit and then remove the line.
- Edge cases: try to add a labor line when no labor categories exist yet (should show guidance and a link to create one, not a broken picker); try to delete a labor rate (should be refused — rates are closed with an end date, never deleted, since past bids were priced on them); try to close/cancel a rate that has already taken effect vs. one that's only scheduled for the future; override a line's rate and confirm the original resolved rate still displays; attempt to create a duplicate labor category name (should be blocked, including near-duplicates).

**ArchAI document indexing (Service Requests and jurisdiction code)** (#596)
The AI indexing controls on a Service Request's documents panel were reworked: each document now shows its own status (Not indexed / Indexing / Searchable / Failed) with per-document index, re-index and "remove from index" actions, and a banner if AI indexing is switched off for the workspace. Separately, a new admin page lets you select a jurisdiction and make its adopted code amendment documents searchable by the AI assistant — tracking whether each document has had its text extracted (a billed, per-page step) and whether it's indexed (free once text is stored), with bulk "read all" and "re-index everything" actions.
- Happy path: on a service request, index an individual document and confirm its status moves from Not indexed → Indexing → Searchable; remove it from the index and confirm it reverts. On the jurisdiction admin page, select a jurisdiction, index its amendments, and confirm status per document.
- Edge cases: indexing while the workspace has document intelligence switched off should show the disabled banner and not silently fail; indexing a jurisdiction whose AI workspace isn't allocated/configured yet should be blocked with a clear message; the bulk "read all documents" and "re-index everything" actions should show their cost-confirmation prompts before running; indexing a document whose text was already extracted should not re-run the billed extraction step.

**Support ticket routing by product area** (#594)
Submitting a support ticket from BuildRoom now routes to a different backing project/queue than one submitted from ControlRoom, and "My tickets" now scans across all configured areas so a user sees their tickets regardless of where they were filed.
- Happy path: file a ticket from within BuildRoom and one from ControlRoom; confirm both appear correctly in "My tickets" and open correctly.
- Edge cases: a ticket filed from an area with no specific routing configured should still land in the default queue rather than erroring; opening a specific ticket by its number should find it regardless of which area's queue it lives in; the ticket list should still cap its scan sensibly and indicate when results were truncated.

## Changes & fixes to re-test

**A deleted reference record no longer breaks pages for the whole company** (#581)
Previously, deleting a general contractor, component type, or a few other shared/reference records that other records still pointed to (e.g. projects, scope items) could make entire list pages error out for every user. Deleting one now correctly shows "still in use" and blocks it; the underlying record can be deactivated instead. Regression check: try to delete a general contractor and a component type that are in use (should be refused with a clear message naming what's using it); delete one that's unused (should succeed); confirm the general contractor list's bulk "select all and delete" is no longer available; confirm project and quote lists still render normally even for old data that may already reference a removed record.

**Take-Off plan viewer loads and re-opens faster** (#583)
The plan/drawing viewer on the Take-Off screen now streams the PDF instead of loading it fully into memory first, avoids re-downloading an unchanged plan on repeat opens or stage switching, and opens fit-to-page instead of at 100% zoom. Regression check: open a large multi-page plan set and confirm it loads noticeably faster than before, especially on a second open or when switching between stages/plans; confirm the viewer opens at a fit-to-page zoom; confirm a user without permission to view the plan is still correctly denied.

**Catalog part and assembly forms now require a unit of measure** (#585, #587)
Creating or editing a catalog part or assembly no longer allows saving without a unit of measure — both are needed to compute per-unit labor and assembly totals correctly. Regression check: try to save a new part/assembly without picking a unit (should be blocked); confirm the assembly builder shows a clear "choose a unit above" message instead of a broken "per unit" label when no unit is set yet.

**Import review lists are paginated instead of capped/sampled** (#585)
The "matched updates" and "new part candidates" tables on the price import review screen now page through all matching rows rather than showing only a capped sample while claiming "every row is applied." Regression check: import a file with more matched/candidate rows than one page holds and confirm you can page through all of them and the counts shown match reality.

**List and search behavior across several list pages** (#587)
The Projects list now searches as you type (debounced) instead of requiring an explicit filter action. The General Contractors list search now also matches on contractor name. Several ControlRoom list pages (Contacts, Fee Schedules, Permit Projects, Request Forms, Service Requests) switched from hiding columns behind an expander on narrow screens to letting the table scroll sideways instead. Regression check: confirm typing in the Projects search updates results without clicking Apply; confirm searching contractors by name works; on a narrow window, confirm all columns are reachable by scrolling and that row-action menus on the last row still open fully on-screen rather than being cut off.

**Manual price overrides are called out with a warning** (#585)
Editing a catalog part's price by hand now shows a warning that future imported prices will be held for review rather than silently applied. Regression check: manually edit a part's net cost, then run an import that includes that part's catalog number, and confirm the import holds it as a conflict rather than overwriting it.

## Risk / regression areas

**Performance** — Re-verify the Take-Off plan loading improvements (#583) under realistic conditions: very large drawing sets, slow connections, and rapid switching between plans/stages, watching for regressions in load time or memory use on both the browser and server side.

**Data integrity across a broad set of record types** — The deleted-reference fix (#581) touches the delete and read paths of many linked record types across BuildRoom (addenda, bid requirements, bill of materials, building models, estimation versions, quotes, vendor quotes, and more). Exercise delete flows broadly, not just on the two or three record types called out above, and confirm existing data that predates the fix still displays correctly.

**Permission and access scoping around the labor-rate move** — Labor rate and labor category management moved from one module's permission set to another's during this window (#593). Re-verify that existing user roles that could manage labor rates/categories before still can, that roles without that access are still correctly blocked, and that the "add a labor category" links and hints point users to the right place.

**Cross-module dependencies for labor pricing** — Estimating labor cost now pulls rate and crew-mix data from a different module than it used to. Walk an estimate end-to-end (BOM → labor line → estimate total → customer-facing quote) and confirm the labor figures agree everywhere they're printed.

**Background/async jobs and their notifications** — Trade scope drawing extraction (#591) and ArchAI document indexing (#596) both run as background jobs with status that updates asynchronously. Confirm status transitions (queued → processing → done/failed) are reflected promptly and accurately, that failures surface a usable error rather than hanging at "processing," and that a user isn't notified multiple times for one job.

**Cost-bearing bulk actions** — The ArchAI "read all documents" and "re-index everything" actions (#596) are explicitly billed operations. Confirm their confirmation prompts are clear about cost/scope before proceeding, and that a cancelled confirmation doesn't still trigger the action.

**Multi-tenant / area isolation for support tickets** — With ticket routing now varying by product area (#594), confirm a user only ever sees their own tickets in "My tickets" regardless of which area or tenant filed them, and that ticket detail lookups don't leak another user's ticket.

## No QA action

- **#599** — Database schema changes only (new columns/tables backing the labor rate, labor category, and crew-mix features). No independent UI; covered indirectly by testing the labor rate/category feature area above.
- **#597** — A large migration-only pull request was closed without merging and is **not** part of this deploy; no related schema or behavior shipped from it.
- Internal design/spec documentation added alongside the labor-rate and ArchAI work — no functional change.
- Local development tooling/config adjustments bundled into a couple of the above PRs — no effect on the deployed application.

## Bug & Test Tracking

| Bug ID | Test Case ID | Priority | Module | Status | Assigned To | Comments |
|--------|--------------|----------|--------|--------|-------------|----------|
| | | | | | | |
| | | | | | | |
| | | | | | | |
| | | | | | | |
| | | | | | | |
