# Release 2026.07.28-4f8f6b0 — Staging QA Guideline
**Environment:** Staging
**Version:** 2026.07.28-4f8f6b0
**Date:** 2026-07-28
**Scope:** PRs merged since 2026-07-26 (PR #451, #461)

## New features to test

### Low-balance guidance on the Launch Review flow
**Where:** Service Request Detail page (ControlRoom), a request in `Pending Intake` with all reviewers assigned.

Coordinators now see the review-credit balance reflected before they commit to launching a review, instead of finding out only after confirming.

- Happy path: tenant with credits remaining sees the normal "Launch review" button; the confirmation dialog now also states how many credits will be left after this launch (e.g. "2 will be left on your balance"). Confirm and verify the review launches and the balance decrements by one.
- Edge case: tenant with **zero** credits — the "Launch review" button is replaced by a "Top up" button with a tooltip explaining why. Clicking it should open the credits-exhausted modal, not attempt to launch.
- Edge case: a coordinator who can launch reviews but does **not** hold the "buy credits" permission — the next-action card and the exhausted modal should say "ask an administrator to top up" and must not show a dead/non-functional buy button.
- Edge case: a coordinator who **can** buy credits — same states should show a working "Top up" link that deep-links to Settings → Review credits.
- Edge case (race): two coordinators both have the page open on a request when only one credit remains; the first launches successfully, the second should still hit the server-side block cleanly (exhausted modal) rather than a raw error.
- Regression check: on a **host** (non-tenant) account, none of the credit UI (balance, blocked button, next-action "needs credits" card) should appear at all — hosts have no credit wallet.

### Review credits settings panel redesign
**Where:** Settings → Review credits (renamed from "Service request credits").

- Happy path: open the panel, confirm the balance line reads "Credits available N" with a sub-line like "Enough for N more reviews" (singular wording at exactly 1 credit).
- Edge case: balance is 0 — sub-line should read "You're out of credits — top up to launch your next review" and the number should render in a warning/red color.
- Buy flow: enter a quantity, confirm the "Total" line updates and matches quantity × unit price exactly (label was previously "Estimated total" — it should now read as an exact total, and the button reads "Continue to payment" rather than "Buy credits").
- Post-purchase return: complete a Stripe test purchase and get redirected back — verify a one-time "Payment received — your credits are on your balance" banner appears. Refresh the page and confirm the banner does **not** reappear (it should show exactly once, not persist or replay on refresh/bookmark).
- Edge case: if the async credit top-up hasn't landed by the time you're redirected back, verify the "Payment received. Your credits are still being added — refresh in a moment" message shows instead, and that refreshing shortly after shows the updated balance.

## Changes & fixes to re-test

### Storefront intake-fee confirmation page (PR #451)
**Where:** Public storefront → apply for a service that has an intake fee → pay → land back on the application page.

This fixes a real staging/production bug: the confirmation page could say "Application submitted" with no tracking number after a card was charged, because the request record is created in a separate step after the payment is recorded.

- Happy path: pay the intake fee, land on the confirmation page — it should show a "Payment received" stage with the amount and a payment reference, then automatically upgrade in place (no manual refresh) to "Application submitted" with a tracking number once the request finishes creating, typically within a few seconds.
- Edge case: simulate a slow backend (or just watch on a loaded staging environment) — after ~90 seconds without a tracking number, the page should switch to a "still finalizing" message with the payment reference, and continue polling slowly rather than spinning forever or showing an error.
- Edge case: if the gateway payment lookup fails/times out on hand-back, the page should show "We couldn't confirm your payment" messaging that explicitly says **not to pay again**, rather than re-showing the "Pay intake fee" button (which would risk a double charge).
- Regression check: submit a **no-fee** application (immediate submit, no payment step) — confirm it still shows the tracking number immediately, unaffected by the new payment-confirmed staging logic.
- Regression check: pay for one application, then in the same browser session start and no-fee-submit a second application — confirm the second application's confirmation page shows its own tracking number, not a stale one carried over from the first (this was a specific bug the fix targets).
- Timestamp check: for a new storefront-created request, confirm submission/expiry timestamps are correct in the local timezone display (a bug meant these could be off by several hours on some server configurations — only newly created rows are fixed, pre-existing rows are not expected to change).

## Risk / regression areas

- **Payment hand-back flows generally.** Both the storefront intake-fee flow and the ControlRoom review-credits purchase flow changed how they interpret a "just returned from payment" state. Exercise both a successful payment and a payment that appears to fail/time out on return, and confirm neither flow ever re-offers to charge the card for something already paid.
- **Multi-tenancy scoping.** Review-credit balance and related UI must only appear for tenant users; confirm host-level accounts see no trace of it anywhere the feature was touched (Service Request Detail page, next-action card, Settings).
- **Permission scoping.** Anywhere a "top up" or "buy credits" action is offered, confirm it's gated correctly — a user without the buy permission should see explanatory copy pointing them to an administrator, never a button that leads nowhere or errors.
- **Theming.** Several UI elements (informational banners, notes) were restyled to use theme-aware tokens instead of fixed-color Bootstrap classes. Spot-check the Settings → Review credits panel and the Service Request Detail next-action card in both light and dark mode.

## No QA action

- Internal AI code-review tooling/skill updates (`.claude/`, `.github/agents/`, `.github/skills/`) — process documentation only, no runtime effect.

## Bug & Test Tracking

| Bug ID | Test Case ID | Priority | Module | Status | Assigned To | Comments |
|--------|--------------|----------|--------|--------|-------------|----------|
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
