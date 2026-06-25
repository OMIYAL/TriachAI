# Release 2026.6.25.2103 — Staging QA Guideline

**Environment:** Staging  
**Version:** 2026.6.25.2103  
**Date:** 2026-06-25 UTC  
**Scope:** PRs merged since 2026-06-25T19:52:49Z (previous successful deploy — run #110)

---

## New features to test

No new user-facing features were introduced in this deploy window. The single PR merged between run #110 and run #111 is a CI/infrastructure change (see §No QA action). A broader smoke-test of features delivered in prior deploys is recommended in the §Risk / regression areas section.

---

## Changes & fixes to re-test

No user-facing code changes were introduced between the previous staging deploy (run #110, 2026-06-25 19:52 UTC) and this one (run #111, 2026-06-25 21:02 UTC). The deploy itself represents a clean re-cut of the same product codebase against the updated CI pipeline.

---

## Risk / regression areas

Although no new product code shipped in this window, a fresh Azure deployment warrants a structured smoke-test. The items below reflect cross-cutting concerns based on features active in staging:

### Deployment health
- Verify the application starts without errors; confirm health-check endpoint returns HTTP 200.
- Check the staging error log (Application Insights / Azure Log Stream) for startup exceptions.

### Authentication & sessions
- Log in as a tenant admin and as a standard portal user; confirm tokens are issued, persisted across requests, and session expiry/refresh behaves correctly.
- Attempt login with invalid credentials; confirm error messaging is correct and no stack traces are exposed.

### Multi-tenancy
- Switch between at least two distinct tenants; confirm data isolation is enforced — no cross-tenant data leaks in lists, searches, or detail views.
- Create an entity under Tenant A and confirm it does not appear under Tenant B's scoped views.

### Notifications & SignalR (Redis backplane)
- Trigger a service-request lifecycle event (e.g. submit a new SR, change status); confirm the in-app notification arrives in near real-time.
- Confirm the SignalR Redis backplane connection is healthy (check logs for backplane connection errors; the configuration was introduced in a prior deploy and is environment-specific).

### Packaging work surface
- Open a packaging work surface and advance through all three steps (Select → Edit → Finalize).
- Verify the Syncfusion PDF viewer loads without a trial watermark (license is registered server-side before merge).
- Confirm "Save & Next" uploads the edited PDF correctly and Finalize loads from the warm Redis cache.
- Edge cases: empty document set; a document with 0 pages; navigate backward and forward between steps.

### Service Requests (SR)
- Open the SR list page; verify column alignment, filters, and pagination render correctly (DataTables integration).
- Close an SR; put one on hold; resume an on-hold SR — confirm activity log entries appear for each transition.
- Verify conditional notes appear in the final package where applicable.
- Edge case: attempt to close an already-closed SR and confirm a graceful error is shown rather than a silent failure.

### Invoicing
- Open the invoice list; verify KPI tiles load and figures are tenant-scoped.
- Confirm Stripe invoice integration surfaces correctly (invoice status, line items).
- Verify Store Front customer inclusion in invoice records where applicable.
- Edge cases: invoice list with zero records; filter by date range that returns no results.

### Document Editor
- Open an existing document record and confirm the editor launches (Syncfusion EJ2 toolbar).
- Save an edit and verify the change persists on re-open.
- Edge case: open a document with no existing content; attempt to save without changes.

### Bugasura quick report panel
- Verify the settings toolbar shows the quick-report trigger icon.
- Open the panel; confirm screenshot capture UI is present and the form renders correctly.
- Edge case: submit with required fields empty and confirm validation fires.
- Note: actual API submission to Bugasura requires staging API key configuration; confirm the key is set or skip submission if not configured.

### Forms & Checklists
- Open the Forms/Checklists administration page; confirm Purpose filter is visible (note: filter query may not function until ExtraProperty is mapped to a separate column).
- Open a checklist in a service request activity; confirm radio-button rendering (not checkboxes) for checklist items.
- Submit a form response through the StoreFront portal; verify it appears in the activity checklist panel in the ControlRoom.

### Permission scoping
- Log in as a role with restricted permissions; confirm restricted menu items are hidden.
- Attempt to access a restricted page via direct URL; confirm access is denied with an appropriate response, not a server error.
- Verify the Payment menu is not visible to roles lacking the Payment permission.

---

## No QA action

| PR | Title | Reason |
|----|-------|--------|
| [#229](https://github.com/OMIYAL/TriArchV5/pull/229) | Refactor CI workflows: Remove AI-drafted release notes and QA report; integrate QA guideline routine for staging deploys | CI/infrastructure only — no product code changed |

---

## Bug & Test Tracking

| Bug ID | Test Case ID | Priority | Module | Status | Assigned To | Comments |
|--------|--------------|----------|--------|--------|-------------|----------|
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
