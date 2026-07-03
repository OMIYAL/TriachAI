
# Release 2026.07.03-830a87c — Staging QA Guideline
**Environment:** Staging
**Version:** 2026.07.03-830a87c
**Date:** 2026-07-03 (UTC)
**Scope:** PRs merged since 2026-07-02 15:44 UTC (previous staging deploy)

## New features to test

**Media Library gets a cleaner upload flow** (#279)
The Storefront Media Library page has a redesigned layout — the "Upload Image" button now lives in the page header toolbar, and the page breadcrumb/title are wired into the standard Control Room chrome. Test: open Storefront → Media Library, confirm the page header, breadcrumb, and upload button render correctly. Upload a valid image and confirm the page reloads and shows the new item. Try uploading a file over the configured size limit and confirm you get a clear "file too large" error instead of a silent failure. Try an invalid (non-image) file type. Confirm the broken-thumbnail fallback icon appears if an image fails to load.

**Document upload limit raised to 100 MB on Permit Projects** (#283)
Project document uploads (Create/Edit Permit Project) now accept files up to 100 MB per file, with updated hint text near the upload control. Test: upload a file just under 100 MB (should succeed) and just over (should be rejected with a clear message). Confirm the hint text on the page reflects the new limit.

**Unsaved-changes warning on key forms** (#283)
Permit Project Create/Edit and the Service Apply wizard now warn you before you navigate away with unsaved edits. Test: start editing a Permit Project or a Service Apply form, make a change, then try to navigate away (back button, close tab, click another nav link) — confirm a warning prompt appears. Confirm no warning appears when there are no unsaved changes, and that saving clears the warning state.

## Changes & fixes to re-test

**Document Review no longer jumps to the wrong file** (#282)
Previously, when reviewing a document and stepping through Review → Report → Verify, then navigating back to the Review step, the page could switch to a different (most recently uploaded) file instead of staying on the one you were working on. Re-test: open a submittal with multiple documents, select a specific document in Review, move to Report, then Verify, then back to Review — confirm you land back on the same document you started with, not the newest upload. Also confirm explicitly switching documents from the Review step's document switcher still works normally. Cite: #282

**Service Request detail page simplified for customers** (#283)
The public Service Request detail page now shows a simplified "Request Status" summary strip instead of the full workflow/state-machine graph. Re-test: view a Service Request as a customer at various statuses (submitted, under review, correction required, closed) and confirm the status strip text and styling match the current state, and that the old detailed graph view is gone. Cite: #283

**Permit Project Edit gets a back button** (#283)
A back button was added to the Permit Project Edit page header. Re-test: open a project for editing and confirm the back button returns you to the expected previous page (project list/detail) without losing context. Cite: #283

**Stuck document uploads can now be retried or dismissed** (#283)
Previously a failed document upload could leave the upload panel stuck showing "in progress" indefinitely. Re-test: simulate a failed upload (e.g. network interruption or oversized/invalid file) on a document upload panel and confirm you can now retry the upload or close the panel instead of it being stuck. Cite: #283

**"Issuance" step type restricted to certificate-issuing services** (#280)
This restriction already existed when creating/editing a Service Request; it's now also enforced when creating or editing a Service Definition — you can no longer add or change a step to "Issuance" on a service that isn't configured to produce a certificate. Re-test: on Service Definition Create and Edit, try adding/retyping a step to "Issuance" on (a) a service with a certificate outcome — should be allowed, and (b) a service with no certificate outcome — the option should be hidden/blocked with a clear message. Cite: #280

**Minor landing page cleanup** (#281)
Small unused label elements ("kicker" tags) were removed above the Control Room and Build Room section headings on the public home page, and a metric label color reference was corrected. Re-test: view the public landing page in both light and dark theme and confirm the Control Room / Build Room section headers look clean (no stray label above the heading) and the metric label color renders correctly, not the previous fixed dark shade. Cite: #281

## Risk / regression areas

- **File upload limits across the Storefront** — multiple PRs this cycle touch upload size validation (Media Library, Permit Project documents). Exercise upload flows across all Storefront areas that accept files to confirm size/type limits are enforced consistently and error messaging is clear, not just on the specific pages changed.
- **Service Definition / Service Request configuration rules** — the Issuance step-type restriction now applies in two places (Service Request and Service Definition editors). Exercise both editors together to confirm the rule behaves consistently and doesn't block legitimate certificate-outcome configurations.
- **Document Review multi-document workflows** — re-test navigation across Review/Report/Verify with several documents on the same submittal, including rapid step-switching, to confirm the file-context fix holds under normal and edge-case navigation patterns.
- **Cross-browser/theme check** — this cycle includes CSS/styling changes (landing page, Media Library cards); spot-check in both light and dark theme and at least one non-Chrome browser.

## No QA action

None — every PR merged in this window (#279, #280, #281, #282, #283) contains a user-visible change and is covered above.

## Bug & Test Tracking

| Bug ID | Test Case ID | Priority | Module | Status | Assigned To | Comments |
|--------|--------------|----------|--------|--------|-------------|----------|
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
