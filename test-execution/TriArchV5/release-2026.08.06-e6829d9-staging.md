# Release 2026.08.06-e6829d9 — Staging QA Guideline
**Environment:** Staging
**Version:** 2026.08.06-e6829d9
**Date:** 2026-08-06
**Scope:** PRs merged since 2026-08-03 (previous staging deploy)

## New features to test

**BuildRoom Take-Off: Linear device measurement with scale configuration** (#485)
- Page/flow: BuildRoom → Project → Launch Phase → Take-Off stage → add a linear-type device from the catalog to the count log → toggle "Place on plan" → draw a run (polyline) on the sheet.
- Happy path: set a scale (detected on the sheet, a common scale, or by measuring a known length) → confirm the scale → draw a linear run for a device → count log shows the total length (e.g. "42 m total").
- Edge cases:
  - Try to place a linear device before any scale is set — should be blocked with a "set a scale first" prompt.
  - Switch scale source between "From sheet" (auto-detected), "Common scales", and "Measure known length" — confirm lengths already drawn recalculate.
  - Edit a linear device's line thickness/transparency in device config while it already has runs on the plan — confirm existing runs update live.
  - Try to give two linear devices the same line color — should be blocked with a conflict message.

**Storefront: downloadable fee receipts** (#486)
- Page/flow: public Storefront → My Service Requests → a request's Detail page → Fees card.
- Happy path: after paying an intake fee or an activity fee, a "Download receipt" button appears next to that line item and downloads a PDF.
- Edge cases:
  - A fee paid before this release (no stored receipt link) — no broken/blank download button should appear.
  - Try to load another applicant's fee-receipt download link directly — should be blocked.
  - Receipt temporarily unreachable — should return to the Detail page, not show an error page.

## Changes & fixes to re-test

**Credit purchase history downloads a receipt instead of an invoice** (#482)
- Settings → Service Request Credits → Payment history panel: each past purchase with a receipt should show a working "Download receipt" button that downloads a PDF (not a hosted invoice page).
- Re-test the redesigned panel itself: empty state, error state, and infinite-scroll loading of older purchases all still render correctly.

**Login / Register password field redesign** (#483)
- Show/hide password (eye icon) still works on both the Login and Register pages.
- Register: the password-strength meter still renders correctly under the field.
- Register: submitting with an empty verification code no longer shows a premature "required" error before the first submit attempt.
- The footer on Login/Register now shows the live application version instead of a fixed placeholder — confirm it displays a real version string.

**Operations Dashboard: "Issued" renamed to "Closed", funnel simplified** (#489, #487)
- The submissions chart now compares "Submitted" vs "Closed" (previously "Issued") — confirm the chart legend, KPI tile, and date-range totals reflect closed requests.
- The workflow funnel now shows 4 stages (Intake, Review, Approved, Closed) instead of 7 — confirm requests in Incomplete/OnHold now count toward Intake/Review and the Open Requests KPI.
- Confirm chart date labels line up with the browser's local calendar across different date-range filters (this week, this month, last 30 days), including near a DST boundary if possible.

**Dashboard responsiveness — donut chart and host system health card** (#484)
- On a narrow browser width, confirm the edition-breakdown donut and host system health cards scroll horizontally instead of clipping or overlapping neighboring cards.

## Risk / regression areas
- Payment/receipt handling changed for both ControlRoom (credit purchases) and Storefront (service request fees) on top of the same payment provider integration — re-verify checkout completion and receipt availability end-to-end, including for payments made before this release.
- Operations Dashboard KPI/funnel math changed (closed-timestamp fallback logic, funnel bucket regrouping, timezone-aware charting) — spot-check totals against a manual count for at least one tenant and one date range to catch off-by-one or timezone edge cases.
- Multi-tenancy: confirm dashboard changes and the new fee-receipt download flow stay scoped to the correct tenant, with no cross-tenant data exposure.
- Access control: confirm the new receipt-download and take-off scale endpoints only allow the owning applicant or an authorized staff member to access the data.

## No QA action
None this cycle — every merged PR in this window touches a user-facing surface (BuildRoom take-off, Storefront fees, ControlRoom credits, Operations dashboard, or auth pages).

## Bug & Test Tracking
| Bug ID | Test Case ID | Priority | Module | Status | Assigned To | Comments |
|--------|--------------|----------|--------|--------|-------------|----------|
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
