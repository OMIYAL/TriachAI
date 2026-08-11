# Release 2026.08.11-83b2ec0 — Staging QA Guideline
**Environment:** Staging
**Version:** 2026.08.11-83b2ec0
**Date:** 2026-08-11 (UTC)
**Scope:** PRs merged since 2026-08-11 02:11 UTC (previous staging deploy)

## New features to test

**Service Requests list now shows who submitted each request** (#495)
- Page/flow: ControlRoom → Service Requests list (main grid)
- Happy path: Open the Service Requests list. Each row's tracking-number cell should now show a second line reading "Submitted by – <name>" underneath the tracking number, with the "Updated <date>" line below that.
- Edge cases:
  - A service request whose creator can't be resolved (no creator id) — should show a "—" placeholder rather than a blank or broken line.
  - Hover/keyboard-focus a row — only the tracking-number title should change to the brand color; the "Submitted by" and "Updated" sub-lines should stay the normal text color.
  - Load the list as a reviewer-scoped user (someone without permission to launch/review all requests) — confirm the list still returns the correct rows and the submitted-by name still populates correctly for that scoped view.
  - Load a page with many rows and confirm names resolve for all of them without a noticeable slowdown (names are now looked up in a single batch per page). (#495)

## Changes & fixes to re-test

**You can no longer create or submit a service request against a deactivated service** (#494)
- Regression check: Create a new service request for an active service — should still succeed as before.
- Regression check: Attempt to create or submit a service request against a service that has been deactivated — should be blocked with a clear "this service is deactivated" message instead of silently succeeding or failing with a generic error.
- Regression check: Open the "Service Definition" picker when creating a service request — deactivated services should no longer appear in the list at all.
- Regression check: Existing service requests that were created earlier under a service that has since been deactivated should still be viewable and manageable normally — only new creation/submission is blocked.

## Risk / regression areas

- Multi-tenancy: confirm the submitted-by name lookup and the "active services only" filtering behave correctly per tenant, with no names or service definitions leaking across tenants.
- Permission scoping: the Service Requests list logic was restructured internally to add the submitted-by name; re-verify both the full-access and reviewer-scoped list views return the same rows and counts as before this change.
- Performance: for tenants with large Service Request volumes, confirm list pages still load at normal speed with the added name lookup.

## No QA action

None — both PRs in this scope are user-facing.

## Bug & Test Tracking

| Bug ID | Test Case ID | Priority | Module | Status | Assigned To | Comments |
|--------|--------------|----------|--------|--------|-------------|----------|
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
