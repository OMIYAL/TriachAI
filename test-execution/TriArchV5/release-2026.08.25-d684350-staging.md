# Release 2026.08.25-d684350 — Staging QA Guideline

**Environment:** Staging
**Version:** 2026.08.25-d684350
**Date:** 2026-08-25 (UTC)
**Scope:** PRs merged since 2026-08-21

## New features to test

**Overhead rate control on the Estimate stage** (#525)
The estimator can now set an overhead percentage (10–12% band, enforced client- and server-side) on the Estimate work surface, separate from the existing margin control. Test: open Estimate stage with `Stages.Estimate.SetOverhead` permission, adjust the overhead input within the band, verify the input rejects/clamps values outside 10–12%, verify the cost breakdown (direct + contingency + overhead) updates live and persists on reload. Re-test without the permission — the control should be read-only/disabled.

**Revision history panel** (#525)
A new panel lists every estimation version a trade scope has produced, newest first, showing which version is current, which are sealed, and any approver "return for revision" notes with reason text and approver name. Test: open the panel from a trade scope with at least one returned/reopened version; verify entries show creation date, "revised from vN", sealed date where applicable, and return notes (including the "no reason given" and "unknown approver" fallback cases). Test a trade scope that has never been launched — should show the empty state.

**Tenant subdomain routing** (#520)
Tenants can now be reached via a dedicated subdomain (e.g. `acme.stg-portal.triarch.ai`) instead of only the shared host. Test: verify an existing tenant resolves correctly by subdomain on both the portal and storefront apps, that `?__tenant=` query-string override still works (used by test tooling), and that a stale tenant-selector cookie picked up on the host portal does not override the subdomain. Creating or renaming a tenant to an invalid subdomain label (uppercase, spaces, punctuation, too long, or a reserved word like `www`/`admin`/`api`) should be rejected with a clear error and a suggested fixed name.

**Tenant login session gate** (#512)
New sign-in safeguards at the AuthServer around tenant session resolution. Test the full login flow for a tenant user end-to-end (sign in, land in the correct tenant workspace, sign out, sign back in) on staging; verify a host-admin login is unaffected.

**BuildRoom email notifications** (#513)
BuildRoom now sends email notifications for: a trade scope assigned to a department, take-off complete/BOM generated, BOM approved, estimate locked, estimation phase launched, quote sent for approval, an approver's turn to sign, all approvals signed, and bid submitted. Test each event fires exactly once, to the correct recipient roster, with correct subject/body content and working links back into the app.

**Notification & template admin scoped by tenant type** (#513)
In Administration, an Agency (ControlRoom) tenant now sees only ControlRoom notification events and email templates; a Contractor (BuildRoom) tenant sees only BuildRoom ones. Host admin still sees everything. Test: log in as an Agency tenant admin and a Contractor tenant admin and confirm each *Notification settings* and *Text templates* list is filtered correctly; confirm host admin's lists are unfiltered.

## Changes & fixes to re-test

**Contingency buffer toggle showed the wrong state on reload** (#525)
The BOM review "contingency buffer" switch always rendered as checked/on regardless of the saved value. Re-test: turn the buffer off, save, reload the page — the switch should now show off, and the summary line should read "switched off" rather than a percentage.

**Trade scope status badge could show the wrong step** (#525)
The status shown on a trade scope (Draft/ScopeValidated/BOMApproved/Estimated/CompliancePassed/QuoteReady) is corrected to match the actual stage the estimation version has reached. Re-test the status badge at each stage transition (Take-off → BOM → Estimate → Quote → Approval → Submit) and confirm it never regresses or shows a stage the scope hasn't reached.

**Take-off autosave timing changed** (#524)
Autosave debounce increased from 1.5s to 3s, with a 10s hard cap so edits are never held indefinitely, and only sheets whose annotations actually changed are sent. Re-test: count devices rapidly on a take-off sheet, confirm work is saved within ~10s even under continuous editing, confirm closing the tab mid-edit still flushes via the existing beforeunload path, and confirm switching estimation version mid-session doesn't drop a sheet's first save.

**BOM generation on large take-offs** (#521)
Mapping and catalog-part lookups during "Generate BOM" are now batched instead of one query per counted device — this was the cause of "Generate BOM" hanging/timing out on large take-offs. Re-test generating a BOM from a take-off with a large device count (hundreds+) and confirm it completes promptly and produces the same line-for-line result as before (including parts removed since count time still degrading to an "unmapped" line).

**Estimate cost rollup now computed in the database** (#518)
The Estimate stage's material/labor totals are now aggregated server-side instead of summing a paged list of BOM lines in the web tier. This is also a correctness fix: the old page size (1000) silently truncated — and understated — the total on any BOM with more lines than that. Re-test the Estimate metric cards on a BOM with more than 1000 lines and confirm the total now matches the full BOM, and spot-check a normal-size BOM for an unchanged total.

**Quote PDF re-render and caching** (#522)
The Quote/bid PDF is now cached instead of being regenerated by the document engine on every view; the download also supports browser revalidation (304) instead of a full re-fetch. Re-test: open the Quote Review / Approval PDF viewer, navigate away and back, refresh — content should be identical and load faster. Edit and save the quote doc, then reopen the PDF — the new content must appear (not a stale cached copy).

**Approval roster and department-access caching** (#523)
"Who can approve" and department-scope checks are now cached briefly (roster ~5 minutes) or per-request. Re-test: change a user's department/approval permission, then use the "Resync approvers" action on an affected trade scope and confirm the roster and staleness flags update immediately rather than waiting out the cache.

**Estimation workspace entry no longer double-loads** (#517)
Opening a trade scope's estimation workspace without an explicit `?stage=` now redirects to the correct step without loading project documents twice. Re-test opening a workspace both with and without a `stage` query parameter, confirm the correct step opens each time and page-load feels faster; also re-test the "workflow faulted" banner still appears correctly on non-submitted bids.

**Document upload limit raised 100 MB → 200 MB** (#516)
Applies to the main app (Kestrel/IIS request limits and web.config) across the portal, storefront, and API host. Re-test uploading a document between 100 MB and 200 MB (should now succeed) and one over 200 MB (should still be rejected with a clear error).

**Service Request Activity / Document Review / Packaging / Report page performance** (#516)
Repeated calls for the same activity output within one page load are now shared instead of re-fetched. Also adds a "document took too long to load" message. Re-test these Service Request pages load correctly and that the new timeout message appears (rather than a blank/broken viewer) if a document load is slow.

## Risk / regression areas

**Multi-tenancy.** With subdomain routing and the AuthServer session gate both landing in this release, exercise sign-in and workspace access for at least one Agency tenant, one Contractor tenant, and host admin, on both the shared host URL and a tenant subdomain, and confirm each only ever sees and reaches its own tenant's data. Confirm an already-live tenant whose name is not a valid subdomain (spaces, punctuation, uppercase, a reserved word) is *not* silently broken by this release — check with the operations team whether the pre-release tenant-name audit has been run in staging.

**Notification correctness.** Because this release adds a full set of new BuildRoom email events plus tenant-based filtering of the notification/template admin screens, watch for notifications firing for the wrong tenant type, duplicate sends, or an admin screen showing another workspace's items.

**Caching staleness.** Several read paths (approval roster, department access, Quote PDF, per-request activity data) are newly cached this release. Regression-test that a change a user just made (permission change, document re-save, department reassignment) is reflected promptly and not masked by a stale cache entry.

**Financial totals.** Contingency, overhead, and the Estimate rollup all changed calculation or aggregation this release. Cross-check a full Take-off → BOM → Estimate → Quote run's final numbers against a manual calculation, on both a small and a large (1000+ line) BOM.

**Performance-sensitive surfaces under load.** BOM generation batching and the estimation workspace's double-load fix are targeted at surfaces that previously hung or felt slow. Re-test on a large/complex project rather than only a small demo project, since the previous bugs did not reproduce on small data.

## No QA action

- #515 — Enterprise Architecture Review artifact pack (documentation only, no code changes)
- Documentation-only portions of #517, #518, #521, #523, #520 (performance fix plan, domain-review decision records, read-model catalog, tenant-name audit runbook — operational/reference docs, not app behavior)

## Bug & Test Tracking

| Bug ID | Test Case ID | Priority | Module | Status | Assigned To | Comments |
|--------|--------------|----------|--------|--------|-------------|----------|
| | | | | | | |
| | | | | | | |
| | | | | | | |
| | | | | | | |
| | | | | | | |
