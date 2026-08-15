# Release 2026.08.15-b4b32a6 — Staging QA Guideline
**Environment:** Staging
**Version:** 2026.08.15-b4b32a6
**Date:** 2026-08-15
**Scope:** PRs merged since 2026-08-14

## New features to test
None in this release — the single change in scope re-configures and extends existing functionality rather than introducing something new.

## Changes & fixes to re-test
- **ITB drawing extraction picks up sheet scale data.** The extraction engine that reads uploaded ITB drawings was upgraded to a newer version that now also captures scale information for each sheet. Re-run an ITB import and confirm sheet scale data shows up correctly on the Take-Off page afterward. Edge cases: sheets with no scale marked, sheets with more than one scale, and re-importing a drawing set that was already processed before this update (confirm none of the previously-working extracted fields regressed). (PR #503)
- **Symbol search on documents should now actually work.** The service the "search for a symbol" action on a document depends on has been pointed at a live endpoint for staging (it was previously unconfigured). Select a symbol region on a document and confirm the search returns matches instead of failing. Edge cases: a selection with no matches anywhere in the document, a very small selection box, a very large selection box, and running the search with a custom match sensitivity vs. the default. (PR #503)

## Risk / regression areas
- BuildRoom document/ITB extraction pipeline — since the extraction engine version changed, spot-check a few previously-imported projects to confirm existing extracted fields (not just the new scale data) still come through correctly.
- Symbol search depends on an external service call — verify the app shows a clear, user-friendly error (not a crash or blank state) if that service is slow to respond or briefly unavailable.

## No QA action
- Removal of unused, empty placeholder settings on the server. No user-visible effect. (PR #503)

## Bug & Test Tracking
| Bug ID | Test Case ID | Priority | Module | Status | Assigned To | Comments |
|--------|--------------|----------|--------|--------|-------------|----------|
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
