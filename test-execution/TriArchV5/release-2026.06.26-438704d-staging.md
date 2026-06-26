# Release 2026.06.26-438704d — Staging QA Guideline

**Environment:** Staging  
**Version:** 2026.06.26-438704d  
**Date:** 2026-06-26 (UTC)  
**Scope:** PRs merged since 2026-06-25T21:02:57Z (previous successful deploy)

---

## New features to test

### Tenant Provisioning System — Seed Packs (PR [#230](https://github.com/OMIYAL/TriArchV5/pull/230))

**Page / flow:** Admin area → Tenant Provisioning (TenantProvisioningAppService) / Saas → Editions seed

**Happy path:**
1. As a super-admin, trigger tenant provisioning for a freshly created tenant.
2. Verify that the expected Agency Roles, Contractor Roles, Form templates, Checklist templates, and Fee Schedule entries are present under the tenant.
3. Confirm the Saas → Editions page lists the new editions seeded by `TriArchEditionDataSeedContributor`.

**Edge cases:**
- Re-running provisioning on an already-provisioned tenant — confirm idempotency (no duplicate roles, forms, or fee schedule rows).
- Provisioning with a CSV that contains missing optional columns — should complete gracefully without error.
- Provisioning a tenant with no edition assigned — verify defined fallback or a clear error message (not an unhandled exception).
- Attempting provisioning as a tenant-level admin (non-host) — confirm the endpoint correctly restricts access.

### Feature Flag / Feature Gating System (PR [#230](https://github.com/OMIYAL/TriArchV5/pull/230))

**Page / flow:** Settings → Features (ABP feature management) and Saas → Editions → Feature values

**Happy path:**
1. Enable or disable a feature for a tenant edition in the host management panel.
2. Log in as a tenant user; verify gated menus appear or disappear as expected.
3. Confirm that `MenuVisibilityPermissions` entries are visible in the permissions assignment tree.

**Edge cases:**
- Feature enabled at edition level but the user has no explicit permission — confirm correct merge behavior (gated vs permitted).
- Switch a tenant's edition mid-session — verify features reload without requiring re-login.
- Host-level feature value vs tenant-level override — confirm tenant setting takes precedence where expected.
- Verify no orphaned menu items appear for features that are disabled.

---

## Changes & fixes to re-test

### SLA Clock — Activation-Anchored with Stop-the-Clock on Hold (PR [#232](https://github.com/OMIYAL/TriArchV5/pull/232))

**What changed:** SLA clock now starts on first *activation* (was anchored at submission for every step). Clock pauses when a step goes OnHold (fee, correction, or manual) and `SlaDueAt` is pushed forward by the held duration on resume. Operations Dashboard SLA buckets exclude untracked steps (SlaHours ≤ 0) and no longer drop resumed steps; `SlaBreached` is derived (not persisted); CorrectionRequired is excluded from on-hold-expired.

**Regression checks:**
- Submit a new service request; confirm the SLA countdown does **not** start until the first activation action is performed.
- Place a step on hold (fee payment); verify the SLA timer pauses; release the hold; confirm `SlaDueAt` advances by approximately the held duration.
- Repeat the hold/resume cycle for correction hold and manual release — verify each credits time correctly.
- Open the Operations Dashboard → SLA Breached / Adherence panels; confirm CorrectionRequired steps do not appear in "on-hold expired"; untracked steps (SlaHours ≤ 0) should not appear in any SLA bucket.
- Verify existing in-flight service requests show sensible (non-regressed) SLA values after the deploy.

### Storefront Applicant "My Requests" List (PR [#232](https://github.com/OMIYAL/TriArchV5/pull/232))

**What changed:** `GetListAsync` became reviewer-scoped and broke applicant access. A new `GetListForApplicantAsync` endpoint (repo + app service + explicit HTTP API controller, AsNoTracking) was added. "My requests" now calls this endpoint. Payment-return uses `FindByPaymentRequestAsync` + `GetAsync` instead of `GetListAsync`.

**Regression checks:**
- Log in as a storefront applicant; navigate to "My Requests" — list must show the applicant's own submissions (must not be empty or throw an error).
- Complete a payment via the payment wizard; on payment-return confirm the wizard resolves correctly (no 404).
- Staff/reviewer "All Requests" view must continue returning all requests for the tenant.
- Confirm that one applicant cannot retrieve another applicant's requests through the new endpoint.

### "Add Step" — Hidden Step Types (PR [#232](https://github.com/OMIYAL/TriArchV5/pull/232))

**What changed:** "Add step" action now hides *Correction window* and *Physical inspection* to match the Service Definition editor.

**Regression check:**
- Open an active service request → Add Step panel; verify "Correction window" and "Physical inspection" are absent from the step-type picker. All other step types should remain present.

### General Review — Hold Label (PR [#232](https://github.com/OMIYAL/TriArchV5/pull/232))

**What changed:** Label changed from "Hold — request payment" to "Hold".

**Regression check:**
- Navigate to a service request in General Review stage and initiate a hold; confirm the UI label reads "Hold" (not "Hold — request payment").

### ControlRoom Page Help & Table Refresh Elements (PR [#231](https://github.com/OMIYAL/TriArchV5/pull/231))

**What changed:** Page Help panels and Table Refresh buttons added across ControlRoom list pages.

**Regression checks:**
- Visit at least three ControlRoom list pages (e.g. Dashboard, Service Requests, Documents); confirm Page Help panel opens and displays guidance; confirm Table Refresh button reloads the grid without a full page reload.
- Verify Page Help icon does not appear on pages that were not updated.

### "Assign to All" Activity Off-Canvas Fix (PR [#231](https://github.com/OMIYAL/TriArchV5/pull/231))

**What changed:** Bug fix for the Assign-to-All off-canvas panel.

**Regression checks:**
- Open a service request activity; click "Assign to all" — confirm the off-canvas panel opens and renders correctly; submit and confirm the assignment persists.
- Verify the assignment is reflected in the activity list without a page refresh.

### Submission Guidance Document Off-Canvas (PR [#231](https://github.com/OMIYAL/TriArchV5/pull/231))

**What changed:** Title input is now hidden; the uploaded file's name is used as the document title automatically.

**Regression checks:**
- Upload a submission guidance document; confirm the title input is absent from the form; confirm the saved document uses the file name as its title.
- Test with a file whose name contains special characters or is very long — verify graceful display.

---

## Risk / regression areas

- **Multi-tenancy & provisioning isolation:** The seed pack system writes roles, forms, and fee schedules scoped per-tenant. Validate that data seeded for Tenant A does not appear under Tenant B. Pay attention to ABP `ICurrentTenant` scoping throughout the provisioning pipeline.
- **SLA correctness across hold types:** Three hold modes (fee, correction, manual) now each influence `SlaDueAt`. Test each type independently, then in sequence on the same service request.
- **Storefront applicant endpoint scoping:** The new `GetListForApplicantAsync` must enforce tenant isolation — an applicant in Tenant A must not retrieve service requests from Tenant B even via direct API calls.
- **Notifications on hold/resume transitions:** With SLA and SR status changes in #232, verify in-app notifications (SignalR) fire correctly when a step enters or exits an OnHold state.
- **Permission tree loading performance:** PR #230 adds `MenuVisibilityPermissions` and `TriArchFeatureGatingPermissionDefinitionProvider`. Exercise the ABP permission management UI to ensure it loads within acceptable time and all newly seeded role permissions are present.
- **Operations Dashboard under load:** The SLA bucket query logic was modified. Exercise the dashboard with a realistic volume of service requests to confirm no performance regression or N+1 queries.

---

## No QA action

| PR | Title | Reason |
|----|-------|--------|
| [#229](https://github.com/OMIYAL/TriArchV5/pull/229) | Refactor CI workflows: Remove AI-drafted release notes and QA report jobs; integrate QA guideline routine for staging deploys | CI / workflow-only change — no application behaviour affected |

---

## Bug & Test Tracking

| Bug ID | Test Case ID | Priority | Module | Status | Assigned To | Comments |
|--------|--------------|----------|--------|--------|-------------|----------|
| | | | | | | |
| | | | | | | |
| | | | | | | |
| | | | | | | |
| | | | | | | |
