# Release 2026.08.03-a67146f — Staging QA Guideline
**Environment:** Staging
**Version:** 2026.08.03-a67146f
**Date:** 2026-08-03
**Scope:** PRs merged since 2026-08-02

## New features to test

**BuildRoom Estimation Workspace (PR #480)**
The Take-Off → BOM Review → Estimate → Quote Review → Approval → Submit flow is now one continuous workspace instead of separate list pages.
- Happy path: walk a trade scope through all six stages end to end, confirming each stage only unlocks after the previous one is approved/locked.
- Confirm the stage ribbon shows done/active/locked correctly and that a user without edit permission for a stage sees the action disabled with a reason.
- Confirm both URL forms work: a bare link to a trade scope auto-redirects to its active step, and a deep link with an explicit stage opens that surface directly even if it isn't the active step.
- Test "Return for revision": confirm it seals the current estimate version, forks a new editable version with the BOM/estimate copied forward, and reopens at the Estimate stage.
- Test the Approval stage sign-off chain (PM → Principal) — confirm signing advances to the next signer, and full sign-off advances to Submit.
- Test the generated quote document/PDF: confirm the quote's pricing table is locked from editing while surrounding text remains editable, and that "Send for approval" produces a PDF that matches the locked estimate.
- Edge cases: attempt to skip ahead to a locked stage via URL; attempt a gated action (Approve BOM, Lock estimate, Send for approval, Sign) without the required permission; revisit an already-submitted trade scope.

**BuildRoom device selection for counting (PR #479)**
You can now add catalog devices into the Take-Off count log and place markers for them directly on the plan.
- Happy path: add a device from the catalog, turn on "Place on plan," click the plan multiple times, and confirm the count log quantity increases with each placed marker.
- Confirm each device's marker color/shape/size can be configured from the device panel and that the marker updates on the plan to match.
- Confirm removing a device from the count log also removes all of its markers from the plan.
- Edge cases: add the same device twice; place markers for two different devices in the same session and confirm counts don't cross-contaminate; remove a device mid-placement (with "Place on plan" still active).

**Service request credit purchase history (PR #477)**
Tenants can now view their past credit top-ups and download the invoice for each, from a new "Payment history" panel in Settings → Review credits.
- Happy path: make a credit purchase, open Payment history, confirm the new purchase appears with correct credit count, amount/currency, and date, and confirm the invoice download link opens a valid PDF.
- Confirm the list only shows purchases for the current tenant (no cross-tenant data), and that scrolling to the bottom loads more history (infinite scroll) without duplicating rows.
- Edge cases: a tenant with zero purchases sees the "no purchases yet" empty state; a purchase without an invoice yet shows no broken download link; the panel handles the history endpoint being temporarily unavailable by showing a retry message rather than a blank/broken list.

## Changes & fixes to re-test

**SLA limits on activity and service definitions (PR #478)**
Creating or editing an activity or service definition now checks that the combined SLA hours for its activities don't exceed the number of days the application stays valid, and shows a hint/error in the UI when they would.
- Regression check: create/edit a definition where SLA hours are within the allowed limit — confirm it still saves normally with no false-positive error.
- Regression check: create/edit a definition where SLA hours exceed the allowed limit — confirm it's blocked with a clear on-screen message (not a generic failure).
- Check the SLA validity hint updates live as hours or validity days are changed on the form.

**Credit purchase invoicing reliability (part of PR #477)**
Stripe invoice creation and PDF/receipt linkage for credit purchases was added, with completion recorded before the invoice is fetched.
- Regression check: complete a credit purchase normally — confirm credits land on the balance and the purchase shows up with a working invoice link.
- Regression check (if simulatable in staging): if invoice retrieval from the payment provider fails or is slow, confirm the purchase still completes and credits are still applied — only the invoice link may be temporarily missing.

## Risk / regression areas

- **Multi-tenancy:** exercise the new BuildRoom workspace and the credit purchase history panel from more than one tenant account to confirm scoping — no tenant should see another tenant's projects, trade scopes, purchases, or invoices.
- **Permission scoping:** re-check that BuildRoom stage actions (Approve BOM, Lock estimate, Send for approval, Sign) and the credit purchase history panel correctly hide/disable for users without the relevant permission, rather than just hiding the entry point while the action remains reachable directly.
- **Performance:** the BuildRoom estimation workspace loads several linked entities (BOM, estimate, quote, approval) per stage — watch load times on trade scopes with a large BOM. The purchase-history panel uses infinite scroll — watch behavior on tenants with a long purchase history.
- **Notifications:** no notification-related changes in this window; a light smoke check of existing service-request notifications is still worthwhile given the batch also touched shared payment/domain code paths.

## No QA action

- None — every PR merged since the previous deploy in this window touches user-facing behavior.

## Bug & Test Tracking

| Bug ID | Test Case ID | Priority | Module | Status | Assigned To | Comments |
|--------|--------------|----------|--------|--------|-------------|----------|
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
