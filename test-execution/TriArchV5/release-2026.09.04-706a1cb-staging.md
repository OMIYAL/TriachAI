# Release 2026.09.04-706a1cb — Staging QA Guideline
**Environment:** Staging
**Version:** 2026.09.04-706a1cb
**Date:** 2026-09-04
**Scope:** PRs merged since 2026-09-04

## Changes & fixes to re-test

**Document Packaging viewer now renders in the browser instead of on the server** (#558)
The Packaging work surface's Edit stage used to hand PDF rendering off to the server; on staging that path never actually worked (it silently failed to render any page). This release switches the Edit stage to load and render the document in the browser, the same approach already used elsewhere in the app.
- Open a packaging activity and step through **Select → Edit → Organize → Finalize**. Confirm the document now actually loads and paints in the Edit stage (previously reviewers may have seen a stuck/blank viewer or a "Web-service is not listening" style failure).
- On the Edit stage, click into the **Page Organizer**, reorder pages, then use **Save & Next** — confirm the reorganized result is saved and the draft reflects the new page order.
- On the Edit stage *without* opening the organizer, use **Save & Next** - confirm the (unmodified) draft finalizes correctly and no accidental save/overwrite occurs.
- Confirm the **Print** button is available and works on every stage except Select.
- Open a **large package** (tens to hundreds of MB) and watch the loading indicator: it should stay up until the document is actually downloaded and rendered, not disappear early and leave a blank pane with Save & Next already active.
- Confirm the toolbar's page-organizer button correctly navigates from the plain Edit view into Organize mode (rather than opening an organizer that can't save).

## Risk / regression areas

- **Performance on large packages:** the Edit stage now downloads the full document to the browser (previously intended to stream page-by-page from the server). Time how long large packages take to open on staging and note anything that feels materially slower than Document Review's existing large-file handling.
- **Deployment/config changes (infra):** this release's infra changes reconcile several app settings (workflow signing key, message-queue credentials, a support-ticket API key, and a document-extraction API endpoint/key) that were previously only set directly on the live app rather than declared in the deployment template. These are config/plumbing changes, not new UI — but if any value is missing or wrong after a redeploy, watch for: workflow/automation features failing to start, the support/ticket page erroring out, portal messages not flowing through the event bus, or document field-extraction reporting as "not configured." A general smoke test across these areas is worth doing even though no functional UI changed.

## No QA action
- `docs/cicd-production-strategy.md`, `infra/README.md`, `infra/main.bicep`, `infra/environments/prod.bicepparam`, `infra/environments/staging.bicepparam` — infrastructure/deployment-template documentation and configuration reconciliation. Governs how settings are declared for deployment; no direct staging UI or behavior change beyond the config risk area noted above.

## Bug & Test Tracking

| Bug ID | Test Case ID | Priority | Module | Status | Assigned To | Comments |
|--------|--------------|----------|--------|--------|-------------|----------|
| | | | | | | |
| | | | | | | |
| | | | | | | |
