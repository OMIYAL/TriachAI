# Release 2026.08.11-4bdae0f — Staging QA Guideline
**Environment:** Staging
**Version:** 2026.08.11-4bdae0f
**Date:** 2026-08-11
**Scope:** PRs merged since 2026-08-07

## New features to test

**Auto-detect symbols in Take-Off (BuildRoom) — PR #491**
Page/flow: BuildRoom → Projects → Launch Phase → Take-Off stage → plan viewer.
Happy path: open a plan PDF, select a count-type device in the count log, click the new "Auto-detect symbols" (magnifying glass) button, drag a rectangle around one example of the symbol, click **Confirm detection** — matching symbols on the current page should be placed as markers, and the count log should show a breakdown of manually-placed vs. auto-detected quantities.
Edge cases to check:
- Trying auto-detect before a plan PDF is loaded (should block with a clear message).
- Trying auto-detect with no device selected, or with a linear-measurement device selected (auto-detect should be unavailable for linear runs).
- Drawing a selection, then switching to Pan and back to Select to redraw before confirming.
- Confirming a selection where no matching symbols exist on the page (should say none found, selection stays active for retry).
- Simulating/observing a detection service failure (should show a failure message and keep the current selection active so the user can retry, redraw, or cancel).
- Cancelling an in-progress detection.

**Building Model / BIM (IFC via xbim) — PR #492**
This PR ships a new backend capability only — new Building Models and Building Fact Comments application services, entities, and REST endpoints, plus a new IFC building-import/editing engine. No new screens were added in this PR.
QA action: exercise the new endpoints via Swagger/API client (building model list/get/delete, project lookup, fact-comment list/get/create/update/delete) rather than through a UI flow. Confirm the staging database migration for the new tables applied cleanly and that existing BuildRoom flows (Projects, Launch Phase, Take-Off) are unaffected.

## Changes & fixes to re-test

**Payment Requests table now shows a tracking number and purpose — PR #493**
The Payment Requests admin table gained a "Tracking number" column (linking back to the related service request) and a "Purpose" column (e.g. Intake fee, Activity fee, Credit pack). New intake-fee and activity-fee payment requests generated from a Service Request now carry that request's tracking number.
Regression checks:
- Existing/older payment requests that predate this change should render blank cells for tracking number/purpose, not errors.
- The tracking-number link should navigate to the correct Service Request detail page.
- Confirm both intake-fee and activity-fee payment requests created from a Service Request show the correct tracking number.

## Risk / regression areas

- **Multi-tenancy:** the new Building Models / Building Fact Comments permission groups and endpoints should be checked for correct tenant scoping — data and permissions must not cross tenants.
- **Permission scoping:** verify the new permission groups (create/edit/edit-structure/edit-facts/delete for Building Models; create/edit/delete for Building Fact Comments) are off by default and only usable by roles explicitly granted them. Exercise this at a high level only (confirm access is denied/allowed as expected) rather than probing for bypass techniques.
- **Performance:** the new IFC/BIM import path may be resource-intensive on larger building models — watch response times and server load if exercising it on staging.
- **Notifications:** confirm existing Service Request / payment notifications still fire correctly, since the payment-request flow was touched by PR #493.
- **External dependency resilience:** the Take-Off auto-detect feature depends on an external symbol-detection service — confirm the existing manual device-placement flow still works normally if that service is slow or unavailable.

## No QA action
None — all PRs merged in this window are user- or API-facing.

## Bug & Test Tracking

| Bug ID | Test Case ID | Priority | Module | Status | Assigned To | Comments |
|--------|--------------|----------|--------|--------|-------------|----------|
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
