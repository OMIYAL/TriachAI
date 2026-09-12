# Release 2026.09.12-dd43b12 — Staging QA Guideline

**Environment:** Staging
**Version:** 2026.09.12-dd43b12
**Date:** 2026-09-12
**Scope:** PRs merged since 2026-09-11 (deploy run #151)

## New features to test

**Widen/narrow toggle and consistent sizing for side panels** (#600)
Side panels (the ArchAI/Code Advisor assistant panel and others built on the shared panel component) now support a "widen" toggle that expands the panel and remembers the reader's choice for next time.
- Open the ArchAI/Code Advisor assistant panel from the top-right toolbar; confirm a widen icon appears in the header, and clicking it expands the panel and swaps the icon to a "narrow" state.
- Reload the page (or reopen the panel) and confirm the widened/narrowed state persists.
- Resize the browser to tablet and mobile widths — the widen toggle should disappear below a certain width, and the panel should still render at full width on small screens.
- Check other panels across the app (e.g. Catalog, BuildRoom panels) still open at their expected widths — nothing should visually shrink or grow unexpectedly from before this release.
- Verify a panel opened from a plain link/button (not the newer panel component) still shows the correct width and doesn't silently fall back to a narrow default.

**Clearer status labels while a document is being read for ArchAI** (#600)
Documents attached to a Service Request now show "Reading document…" while the file is first being processed, before the previous "X of Y sections" progress counter appears.
- Upload a new document to a Service Request and watch its AI status badge: it should show "Reading document…" first, then switch to the "X of Y sections" counter, then "Indexed" (or "Failed").
- Confirm a document that was never indexed (e.g., uploaded before AI indexing existed, or skipped because it was too large) shows an "Index for ArchAI Assistant" action, distinct from "Retry indexing" (shown only for failed documents) and "Reindex" (shown only for already-indexed documents).

## Changes & fixes to re-test

**Coordinators can now use the ArchAI assistant on any Service Request in their tenant** (#600)
Previously, a coordinator (someone who routes and manages Service Requests) could see every request in the list, but if they were not the original applicant or an assigned reviewer, opening the ArchAI assistant on that request returned a bare "Forbidden" error.
- As a coordinator/routing user, open a Service Request you did not create and are not assigned to review. Confirm you can now open its ArchAI-scoped chat, see its indexing status, and trigger document indexing without an error.
- As a non-coordinator staff user who is neither the creator nor an assigned reviewer of a given request, confirm you still see a clear, friendly message explaining you don't have access ("You don't have access to this service request…") rather than a raw error page, when attempting the same actions.
- As the original applicant, and separately as an assigned reviewer, confirm normal access still works unchanged.

**Some previously-indexed Service Request documents will show as "Not indexed" after this deploy** (#600)
A one-time cleanup runs on this deploy that removes older documents' AI data from a shared, previously-shared search index (part of retiring an old, less secure filtering mechanism). Affected documents' status will read "Not indexed" even though they showed "Searchable"/"Indexed" before.
- Spot-check a few Service Requests that had documents indexed before this release; confirm their AI status now shows "Not indexed" (this is expected, not a regression).
- Confirm re-indexing those documents from the Documents panel ("Index for ArchAI Assistant") works and brings them back to "Indexed"/"Searchable".
- Confirm the general jurisdiction chat (not tied to a specific Service Request) does not surface any private Service Request document content, before or after re-indexing.

**Deleting a Service Request document is now blocked if its AI data can't be fully removed** (#600)
Previously, if the system failed to clean up a document's AI search data during deletion, the deletion would still silently succeed. Now the deletion is refused and the document remains, so no orphaned private content can be left searchable.
- Delete a normal, already-indexed Service Request document and confirm deletion still works and its AI status goes away with it.
- If a deletion cannot be tested against a simulated backend failure, at minimum confirm delete/undo flows for documents behave normally end-to-end (upload → index → delete) with no new errors surfaced to the user under normal conditions.

**Failed session list loads in the ArchAI assistant panel now show a retry option** (#600)
Previously, if the list of past chat sessions failed to load, the panel appeared blank with only a background error notification. Now a clear "Something went wrong" message with a "Try again" button is shown in the panel itself.
- Simulate or wait for a failed session-list load (e.g., via network throttling/blocking the request in dev tools) and confirm the panel shows the error state with a working "Try again" button instead of a blank list.

**Background AI indexing reliability fix** (#600)
A background job responsible for indexing documents into the AI assistant could previously fail immediately due to an internal permission check that doesn't apply to background jobs, and two document trackers were not being registered correctly, which could leave documents stuck at "Indexing…" forever.
- Upload and index documents in a brand-new jurisdiction (one that has never had an AI workspace provisioned before) and confirm indexing completes successfully rather than immediately failing.
- Upload and index both a Submittal document and a Project-linked document under the same Service Request; confirm both correctly progress through Pending → Indexed (or Failed with a clear reason), and neither gets stuck indefinitely at "Indexing…".

## Risk / regression areas

- **Permission scoping around Service Requests and ArchAI**: exercise the assistant, document indexing, and session listing as several different role combinations (applicant/creator, assigned reviewer, coordinator, unrelated staff member, and a user from a different tenant) to confirm each sees exactly the access level appropriate to their role, and that no role can view another tenant's or another Service Request's private content through the assistant.
- **Cross-tenant / cross-jurisdiction data isolation**: with the shared AI search index cleanup in this release, verify the general jurisdiction chat only ever returns jurisdiction code content and never any applicant's private documents, across at least two different tenants/jurisdictions if available in staging.
- **UI layout regressions from the panel-sizing change**: broadly click through panels across ControlRoom, BuildRoom, and Catalog modules to confirm none rendered at an unexpected width, and that panel open/close animations still look correct.
- **Performance**: watch page load and AI response times for regressions, particularly around jurisdiction chat search (its underlying retrieval logic was simplified in this release) and document upload/indexing throughput.
- **Notifications**: confirm any existing toast/notification behavior around AI chat and document indexing errors still fires appropriately where expected (some error notifications were intentionally removed in favor of in-panel messaging — confirm this isn't perceived as a silent failure).

## No QA action

- CI/deployment pipeline change adding staging-specific AI embedding configuration environment variables (internal deploy config, not user-facing).
- Internal documentation updates describing the retired shared-workspace design and the new offcanvas panel sizing guidance for developers.
- Internal package/dependency cleanup (removing an unused vector-store package reference) and EF Core change-tracking correctness fix for AI embeddings (invisible to end users; covered by automated tests).
- New/updated automated test coverage for the access-control rule, background job dependency isolation, and document tracker registration.

## Bug & Test Tracking

| Bug ID | Test Case ID | Priority | Module | Status | Assigned To | Comments |
|--------|--------------|----------|--------|--------|-------------|----------|
| | | | | | | |
| | | | | | | |
| | | | | | | |
| | | | | | | |
| | | | | | | |
