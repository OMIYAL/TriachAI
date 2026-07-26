# Release 2026.7.26.2029 — Staging QA Guideline
**Environment:** Staging
**Version:** 2026.7.26.2029
**Date:** 2026-07-26
**Scope:** PRs merged since 2026-07-24 (previous staging deploy)

## Changes & fixes to re-test

### Notification emails no longer fail to send when a template has a formatting error
Previously, if a saved email template contained a formatting mistake, the notification could fail to send at all, and in a worst case this jammed processing for every tenant's notifications for hours. Now, if a template fails to render, the system falls back to a plain-text version of the same notification and still sends it.

- Happy path: trigger a normal notification (e.g. a service request status change) with a valid, unmodified template and confirm the email looks correct as before.
- Edge case: intentionally introduce a syntax error into a notification template (e.g. via template editing, if exposed) and confirm the notification still sends, using the fallback plain title/description instead of failing silently.
- Edge case: confirm that a broken template for one tenant does not delay or block notifications for other tenants.
- Cite: PR #450

### Storefront payment no longer appears to disappear when a service request can't auto-submit
When a customer pays through the storefront, the system marks the payment as received and then tries to automatically submit the request. Previously, if the auto-submit step failed for a specific request, the payment could effectively look reverted on a retry. Now, the payment is recorded permanently as soon as it completes, independent of whether auto-submit succeeds.

- Happy path: complete a storefront payment for a normal service request and confirm it is marked paid and automatically submitted as expected.
- Edge case: complete a storefront payment for a service request that is missing information needed to submit (e.g. incomplete required fields/attachments) and confirm the payment still shows as paid/recorded, with the request left in Draft for manual resubmission — not appearing unpaid.
- Edge case: retry/resubmit the same request after fixing the missing information and confirm it submits normally without double-charging or duplicate payment records.
- Cite: PR #450

## Risk / regression areas

- **Cross-tenant notification processing.** Run notification-triggering actions (status changes, payment completions, template-driven emails) across multiple tenants in parallel and confirm one tenant's failure or unusual data doesn't delay or block another tenant's notifications.
- **Payment-to-submission flow end to end.** Broadly re-test the full storefront intake path — quote/request creation, payment, and submission/status transitions — since the payment-recording logic in this path was restructured.
- **General service request status transitions.** Spot-check that status changes elsewhere in the service request lifecycle (not just the storefront path) still behave normally, since shared code in this area was touched.

## No QA action

- Internal test-suite and test-infrastructure repairs (fixing seed data, fakes, and module wiring used only by the automated test project). No runtime or user-facing behavior changed by these files.

## Bug & Test Tracking

| Bug ID | Test Case ID | Priority | Module | Status | Assigned To | Comments |
|--------|--------------|----------|--------|--------|-------------|----------|
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
