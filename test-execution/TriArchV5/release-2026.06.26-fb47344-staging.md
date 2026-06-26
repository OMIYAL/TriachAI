# Release 2026.06.26-fb47344 — Staging QA Guideline

**Environment:** Staging  
**Version:** 2026.06.26-fb47344  
**Date:** 2026-06-26 UTC  
**Scope:** PRs merged since 2026-06-26T10:58:38 UTC (previous staging deploy — run 28233767852)

---

## New features to test

### Reject handling in ReviewerBlockingActivity — PR [#233](https://github.com/OMIYAL/TriArchV5/pull/233)

`ReviewerBlockingActivity` in ControlRoom Elsa workflows now has a proper reject branch, allowing a reviewer to explicitly reject a service request at the blocking step rather than only approve/forward it.

**Page / flow:** ControlRoom → Service Requests → open a request currently in the *Reviewer Blocking* workflow step.

**Happy path:**
1. Log in as a user with reviewer permissions.
2. Open a service request sitting at the *ReviewerBlocking* activity.
3. Click **Reject** (new action).
4. Confirm the workflow transitions to the rejected terminal state: status badge updates, the activity timeline records the rejection with actor + timestamp.
5. Confirm the applicant's Storefront "My Requests" view reflects the new status.
6. Confirm any configured rejection notification (in-app / email) fires correctly.

**Edge cases:**
1. **Concurrent rejection:** Two reviewer sessions attempt to reject the same request simultaneously — expect only one succeeds; the second should receive a clear conflict/already-actioned message, not a silent 500 or a double-state flip.
2. **Reject then attempt re-submission:** After rejection, verify that attempting to reactivate or re-submit the request from the Storefront does not leave the workflow in a broken/zombie state.
3. **Reject with no optional comment/reason:** If the rejection reason field is optional, submit without filling it — the workflow should transition cleanly without a validation error.
4. **Audit trail integrity:** Inspect the service request activity log after rejection — confirm the entry shows the correct action ("Rejected"), the reviewer's display name, and an accurate UTC timestamp.

---

## Changes & fixes to re-test

No additional functional changes or standalone bug fixes landed between the previous staging deploy and this one beyond PR #233 above.

*Smoke-test the approve/forward path on the same `ReviewerBlockingActivity` to confirm the new reject branch did not disturb the existing happy path.*

---

## Risk / regression areas

- **Workflow state-machine integrity:** The reject branch is a new code path inside `ReviewerBlockingActivity`. Run the full approve path alongside the new reject path to rule out any shared-state side effects or Elsa bookmark conflicts.
- **Notifications pipeline:** If rejection publishes a domain event that triggers a notification, confirm delivery does not interfere with other notification types (fee-waived, status-update, etc.) — particularly under load or when multiple requests are rejected in quick succession.
- **Multi-tenancy:** Exercise the reject flow under a non-default tenant configuration to confirm the activity correctly scopes all reads and writes to the current tenant.
- **Permission scoping:** Verify that only roles carrying the appropriate reviewer grant can invoke the reject action. Staff or applicant-level users should not see the reject control, and a direct API call without the grant should be refused.
- **Storefront applicant view:** After a staff-side rejection, the applicant's "My Requests" list and detail view should reflect the rejected status without a hard reload; verify the SignalR real-time update fires correctly.
- **Performance:** The `ReviewerBlockingActivity` SLA clock (introduced in a prior deploy) should still stop correctly on rejection — verify `SlaDueAt` is not advanced or recalculated on the reject path.

---

## No QA action

| Item | Reason |
|------|--------|
| `Elsa_Workflow_Improvement_Diagrams.html` (PR #233) | Documentation artefact — no runtime behaviour |
| `Elsa_Workflow_Improvement_Handoff.md` (PR #233) | Documentation artefact — no runtime behaviour |

---

## Bug & Test Tracking

| Bug ID | Test Case ID | Priority | Module | Status | Assigned To | Comments |
|--------|--------------|----------|--------|--------|-------------|----------|
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
