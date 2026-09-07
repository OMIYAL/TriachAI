# Release 2026.09.07-7e2a2be — Staging QA Guideline

**Environment:** Staging
**Version:** 2026.09.07-7e2a2be
**Date:** 2026-09-07 (UTC)
**Scope:** PRs merged since the previous staging deploy (2026-09-07 03:29 UTC, deploy run #149)

## Changes & fixes to re-test

**Take-off symbol markers reverting after reload (#567)**
On the Take-Off drawing surface, markers placed automatically by device detection are meant to render border-only and count into a row's "auto" total, while manually placed markers render filled and count separately. Previously, saving and reloading a take-off could lose this distinction, causing auto-detected markers to come back filled in and counted as manual.
- Place a mix of auto-detected and manually-added markers on a take-off drawing, save, then reload the page and confirm each marker restores with its original style (border-only vs. filled) and counts in the correct total (auto vs. manual).
- Reload multiple times in a row to confirm the state doesn't drift on repeated save/reload cycles.
- Check a device row that has only auto-detected markers, only manual markers, and a mix of both.
- Confirm restored count/linear-type markers (square, circle, polygon) keep their original shape after reload.

**Tenant provisioning permission & error logging fix (#582)**
The dedicated permission-seeding step for the "Demonstration data" admin menu item was removed in favor of the platform's built-in permission seeding, and failures during a tenant's database migration/seed step are now logged at a much higher severity so they are no longer easy to miss.
- Create a new tenant end-to-end and confirm the tenant admin can see and use the "Demonstration data" menu item (create/remove demo data) without any manual permission grant.
- Confirm an existing tenant's admin still has the "Demonstration data" item after a normal login (no regression from removing the dedicated seeding step).
- From the host/SaaS tenant list, run "Apply database migrations" against an existing tenant and confirm it still completes and the tenant remains usable.
- If a tenant migration is intentionally forced to fail in a test environment, confirm the failure is now clearly logged and the tenant is left in a recognizable, recoverable state (no admin user silently missing without any trace).

## Risk / regression areas

- **Multi-tenancy / tenant provisioning:** exercise the full "create tenant → log in as that tenant's admin → check default menu/permissions" path, since this release changed how a tenant's admin permissions get seeded.
- **BuildRoom Take-Off drawings:** exercise the save/reload cycle broadly on the Take-Off stage (not just the specific marker case above) to catch any related regressions in annotation persistence.
- **Permission scoping:** spot-check a couple of other admin-only menu items (not just Demonstration data) after a fresh tenant/host login to confirm nothing else was affected by the permission-seeding change.

## No QA action

- **#580 — Refactor code structure for improved readability and maintainability.** Despite the title, this PR only adds two internal design/planning documents (a future Labor Cost Model design). No application code changed; nothing to test on staging.

## Bug & Test Tracking

| Bug ID | Test Case ID | Priority | Module | Status | Assigned To | Comments |
|--------|--------------|----------|--------|--------|-------------|----------|
| | | | | | | |
| | | | | | | |
| | | | | | | |
