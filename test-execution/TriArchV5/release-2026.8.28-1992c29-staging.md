# Release 2026.8.28-1992c29 — Staging QA Guideline
**Environment:** Staging
**Version:** 2026.8.28-1992c29
**Date:** 2026-08-28 (UTC)
**Scope:** PRs merged since 2026-08-27 (previous staging deploy, PR #530)

## New features to test

### Take-Off count log — sheet-scoped device counts (PR #533)
- **Page/flow:** BuildRoom → Project → Take-Off stage → left rail Count Log.
- **Happy path:** Place devices on markers across multiple sheets in a plan. In the Count Log header, toggle between "All sheets" and "Active plan." Confirm quantities switch to show only the counts present on the currently active sheet.
- **Edge cases:**
  - Switch the active sheet while "Active plan" is selected — counts should update automatically, no manual refresh needed.
  - A device with zero placements on the active sheet should show 0, not an error.
  - Linear (footage) devices should read "X m on this sheet," not the all-sheets wording, while in sheet scope.
  - Switching back to "All sheets" should restore the persisted total — the sheet toggle is display-only and must not alter saved quantities.

### Take-Off fullscreen viewer (PR #533)
- **Happy path:** In the Take-Off stage, use the new fullscreen icon on the canvas toolbar (moved from the viewer toolbar to sit next to the building-floors button). Confirm fullscreen covers the plan canvas and the left count-log rail, hides count-log config/legend/remove controls, and disables the floors button while active.
- **Edge cases:**
  - Enter fullscreen while a symbol-selection is in progress — the fullscreen toggle itself should stay clickable even though other toolbar controls are locked.
  - Exit both via the in-app toggle and via the browser's native Escape/exit-fullscreen — the icon should correctly reflect state either way.
  - Toast notifications firing while in fullscreen should still be visible and interactive.

### Panel/menu default-collapsed state (PR #533)
- **Happy path:** Load the app in a fresh session (no existing cookies) — the left side menu should start collapsed. Expand it and reload — it should stay expanded (persisted via cookie).
- The Take-Off right details panel should also start collapsed by default on first load.
- **Edge case:** Clear cookies/local storage and reload to confirm the collapsed default returns correctly rather than sticking to a previous state.

### Catalog — Category Parts advanced filters (PR #534)
- **Page/flow:** Catalog module → Category Parts list.
- **Happy path:** Open the new "Filters" panel, set Name/Description text and an Active/Inactive value, click Apply — the grid narrows accordingly. Click Clear — filters and the quick-filter chips reset to "All."
- **Edge cases:**
  - Type in the quick-search box (now labeled "Search name, description…") and confirm the list auto-refreshes after a short pause without clicking anything.
  - Combine the quick-filter chips (Active/Inactive) with the advanced filter panel and confirm they don't produce conflicting results.
  - Open the new help panel ("What this page is for") from the page and confirm all steps and the footer tip render.
  - A filter combination matching zero rows should show the normal empty-state, not an error.

### Catalog Parts & Discount Structures list styling (PR #534)
- **Happy path:** Confirm the Catalog Parts and Discount Structures list tables render with striped rows, and that the Catalog Parts "Catalog #," "M/U," "Mat Cost," and "Labor" columns are left-aligned under their headers rather than center/right-aligned.
- **Edge case:** On Discount Structures, the page-title banner above the list was removed — confirm the page still reads clearly and that removing the row hover-highlight hasn't made rows feel unclickable.

## Changes & fixes to re-test

### Large package save no longer times out (PR #535) — HIGH PRIORITY
- **What changed:** Finalizing a large packaging output previously failed outright above roughly 100 seconds of processing (a real-world case: a ~291 MB package consistently failed). Finalize now hands off to a background job and the client polls for completion; uploads are sent in smaller chunks instead of one large request.
- **Regression checks:**
  - Finalize the largest package you can produce (target well above what used to fail) and confirm it completes and reports success rather than failing partway through.
  - Confirm the UI shows real progress/waiting state during a long finalize instead of appearing frozen or timing out client-side.
  - Confirm a normal, small package still finalizes quickly with no change in behavior.
  - Force both an "expired" wait and a genuine failure and confirm the two now show distinct, correct messages (previously an expired-but-not-yet-failed state could be misreported as failed).
  - Confirm the finished output isn't shown as available/downloadable until the background job has actually finished writing it.

## Risk / regression areas
- **Performance under load:** Finalize moved from a synchronous request to a background job. Verify concurrent finalizes from multiple users/projects don't back up indefinitely, and that an in-progress finalize survives a page refresh (the poll should pick back up and reflect the correct state).
- **Session/cookie migration:** The side-menu default is now driven by a cookie the app sets itself. Check that users with an existing session from before this release don't see unexpected menu state on their next visit.
- **Multi-tenancy:** Spot-check the sheet-scoped count log and the package finalize flow across at least two tenants to confirm no cross-tenant bleed in any cached or polled status.
- **Notifications:** Confirm success/failure toasts still appear correctly for the async Finalize flow, and that toast visibility is unaffected by the Take-Off fullscreen changes.

## No QA action
- **PR #531 — staging telemetry fixes.** Server-side exception/telemetry logging changes only (excludes a noisy expected-flow exception from the error table; fixes a missing service-name tag on trace rows). No user-visible behavior changed; nothing to functionally test.

## Bug & Test Tracking
| Bug ID | Test Case ID | Priority | Module | Status | Assigned To | Comments |
|--------|--------------|----------|--------|--------|-------------|----------|
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
