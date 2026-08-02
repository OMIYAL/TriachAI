# Release 2026.08.02-8a5f04e — Staging QA Guideline

**Environment:** Staging
**Version:** 2026.08.02-8a5f04e
**Date:** 2026-08-02 (UTC)
**Scope:** PRs merged since 2026-07-29 (previous staging deploy)

## New features to test

### Project creation from an ITB/SoW PDF, with AI field extraction (#473)
- **Page/flow:** New-project flow in the Build Room. Choosing "From ITB" lets you upload an ITB/SoW PDF; the system extracts project fields with AI, you review/confirm the extracted values (low-confidence fields must be explicitly confirmed before the project can be created), and the project is created with the source drawing sheets attached.
- **Happy path:** Upload a clean, well-formatted ITB/SoW PDF → verify extracted fields are accurate → confirm → project is created with drawing sheets linked under its Documents tab.
- **Edge cases:**
  - Close/cancel the extraction flow mid-process — you should be prompted to confirm before losing the in-progress extraction.
  - A PDF with low-confidence/ambiguous fields should force manual confirmation before Create is enabled.
  - Compare the "Manual" project-creation path still works unchanged alongside the new "From ITB" path.
  - A malformed or non-ITB PDF should fail gracefully with a clear error, not a silent hang.

### Take-Off workspace for estimation (#473)
- **Page/flow:** From a project's Launch Phase, "Estimation workspace" opens Take-Off, which displays the drawing PDF with an "Active plan sheet" picker and a building-floors panel showing floor coverage/sheet gaps.
- **Happy path:** Open Take-Off for a project created via the ITB flow → switch active plan sheets → confirm the floors panel reflects sheet coverage correctly.
- **Edge cases:** A project with sheets missing for one or more floors should surface a visible gap, not fail silently; verify large drawing sets still load and page correctly.

### Host platform dashboard — live data (#472)
- **Page/flow:** Host admin dashboard KPI tiles (Total Tenants, New Tenants·30D, Total Users, Active Tenants·7D), tenant growth chart, edition-mix chart, recent-tenants panel, and a new date-range filter (Last 7 Days / Last 30 Days [default] / This Month / Last Month).
- **Happy path:** Load as host admin → confirm KPI counts, deltas, and sparklines reflect real tenant/user data → switch each date-range option and confirm charts and the recent-tenants list update accordingly.
- **Edge cases:**
  - Select "This Month" on the 1st of a month — should not render blank/broken.
  - Onboard a new tenant and confirm it appears in "recent tenants" with an accurate user count.
  - Confirm the "Near-quota tenants" and "Security feed" panels show a genuine empty state rather than sample/placeholder rows when there is no real data yet (previously these showed fabricated rows that looked like real alerts).
  - Confirm a non-host-admin user cannot reach the dashboard or its data endpoints.

### System health panel (#472)
- **Page/flow:** New live "System health" panel on the host dashboard covering API host, Auth server, Database, Redis, Message broker, Background jobs, Email provider, and Storage, each showing status plus diagnostic detail (short cache window, ~15s).
- **Happy path:** Confirm each row shows a sensible current status.
- **Edge cases:** Simulate/observe a dependency outage (e.g., email provider misconfigured) and confirm it shows degraded/unhealthy without crashing the page or restarting the app. Use the new "Refresh" button on the Email provider row to trigger a live test-send and confirm both success and failure states render correctly.

### Health-check endpoint enhancements (#472)
- **Page/flow:** `/health-ui` now polls on a configurable interval (default 60s) and adds checks for event processing, blob storage, background job retries, pending DB migrations, email, and message broker.
- **What to test:** Confirm `/health-ui` reports all new checks; confirm a simulated external dependency outage is reported without recycling the app instance (checks that justify a restart, like the API host or database, remain on the app's own liveness probe).

## Changes & fixes to re-test

### Final report / certificate file naming (#472, #475)
- **What changed:** Generated final reports and certificates are now named `FR_{ActivityStepName}_{ServiceRequestID}.pdf` and `CR_{ServiceRequestID}.pdf` respectively, replacing the old generic `package.pdf` / `Certificate_*` naming. Both the reviewer Packaging workspace and the public Storefront Service Request detail page were updated to recognize both the new and legacy naming.
- **Regression check:** Finalize a packaged set of outputs and confirm the downloaded file uses the new `FR_...` name; confirm an older service request that already has a legacy `package.pdf` / `*_Final.pdf` file still displays and downloads correctly on both the reviewer and applicant (Storefront) sides; confirm the certificate file name shown on the Storefront detail page matches the new `CR_...` convention.

### Service Requests list — reviewer-scoped KPI counts (#472, #475)
- **What changed:** The "In queue" KPI tile on the Service Requests list now shows counts scoped to the current reviewer's own assignments (with an updated subtitle, "Open requests assigned to you") for users without launch/administrative visibility, while users with that broader permission continue to see the count across all reviewers.
- **Regression check:** Compare the tile as a regular reviewer (should show only their assigned open requests) versus as a user with full visibility (should show the org-wide count, unchanged subtitle).

### Service Requests list — project name column (#476)
- **What changed:** Both the internal Control Room Service Requests list and the public Storefront Service Requests list now show the resolved project name (falling back to project address, then an em dash) alongside the jurisdiction, instead of just the raw address. The project column also got narrower to make room.
- **Regression check:** Confirm project names resolve and display correctly on both lists; confirm the list search box also matches on project name now; confirm a service request with a deleted/inaccessible project still shows a sensible fallback (address or em dash) instead of an error; confirm a reviewer without project-list visibility still sees a reasonable fallback rather than a failed page load.

### Additional Service Requests / Packaging / notifications fixes (#467)
- **Contractor column removed** from the Service Requests list — confirm the list still renders correctly and the SLA/Project/Service Definition columns are unaffected.
- **Project contact invitation emails** now use the resolved tenant display name instead of the raw tenant name — confirm invite emails show the correct From/tenant name.
- **Packaging output files now default to checked**, and the selected-file count in the tray reflects the real starting selection instead of showing "0" — confirm both on opening the Packaging workspace.
- **Notification dropdown on the global panels toolbar** no longer gets blocked from opening/closing correctly — confirm clicking the bell opens the dropdown and clicking outside it closes it.

### Authorization page-path fix (#468)
- **What changed:** A module configuration bug meant several Control Room pages were not correctly gated by login/permission checks, and one Catalog page's permission mapping pointed at the wrong path. Both have been corrected, and invoicing list pages now have an explicit permission gate that had previously been missing entirely.
- **Regression check:** Confirm normal, authorized navigation to Service Definitions, Contacts, Fee Schedules, Service Requests (list/detail/report/pending-intake/document-review/activity), Dashboards, Permit Projects, Request Forms, Invoicing, and the Catalog discount-structures page all still work for users who should have access. Separately, confirm that users without the right permission — or without being logged in — cannot reach any of these pages. Do not test or document specific bypass techniques; just confirm the standard access-control behavior holds across this list.

## Risk / regression areas

- **Permission scoping:** This release touches page-level authorization wiring (#468) and reviewer-scoped data visibility (#472, #475). Exercise standard role-based access across Control Room modules broadly, not just the specific pages called out above — a wiring mistake in one area of this file can plausibly affect sibling pages.
- **Multi-tenancy:** The host dashboard (#472) aggregates data across tenants — confirm no tenant's data leaks into another tenant's view, and that per-tenant health/quota data stays correctly scoped.
- **Performance:** The Service Requests list now does client-side batch lookups for project and jurisdiction names (#476) and the host dashboard runs several live queries per load (#472) — watch list/page load times, especially with large data sets (project lists near or above 1000 items).
- **Notifications:** Both the notification dropdown fix (#467) and the tenant-email-display-name change (#467) touch notification/email delivery paths — spot-check that in-app notifications and outbound emails still render and send correctly across a few different flows.

## No QA action

- **#469** — Added a Playwright BDD suite trigger to the CI/CD pipeline for automated QA after deployment. Pipeline/infrastructure only, no user-facing change.

## Bug & Test Tracking

| Bug ID | Test Case ID | Priority | Module | Status | Assigned To | Comments |
|--------|--------------|----------|--------|--------|-------------|----------|
| | | | | | | |
| | | | | | | |
| | | | | | | |
| | | | | | | |
| | | | | | | |
