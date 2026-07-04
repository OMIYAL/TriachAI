# Release 2026.07.04-9051ad2 — Staging QA Guideline
**Environment:** Staging
**Version:** 2026.07.04-9051ad2
**Date:** 2026-07-04
**Scope:** PRs merged since 2026-07-03 11:43 UTC (previous staging deploy)

## New features to test
No net-new capabilities in this deploy — all merged PRs are fixes/improvements to existing functionality (see below).

## Changes & fixes to re-test

**1. Document Editor — Ribbon toolbar, keyboard shortcuts, and fullscreen dialogs (#284)**
Page/flow: Documents section → open any document in the built-in editor.
- Happy path: Open a document, use the new tabbed ribbon toolbar (Home/Insert/Layout/...) to apply formatting (bold, italic, font, alignment, etc.), save, and confirm the changes persist.
- Edge cases:
  - Press Ctrl+N and Ctrl+O while the editor has focus — confirm neither blanks the open document nor swaps in an unrelated file.
  - Enter fullscreen mode and confirm the editor now fills the screen correctly (previously rendered undersized/distorted).
  - While in fullscreen, trigger the Rename / Move / Share dialogs and the file row's context menu — confirm they are visible and usable (previously invisible).
  - While in fullscreen, use the ribbon's Insert Table / Insert Image / Insert Hyperlink dialogs — confirm they render visibly.
  - Confirm the file-properties panel that used to appear below the document viewer (Type/Size/Modified) has been removed and the viewer's layout still looks correct with no gap or visual break.
  - Exit fullscreen and confirm all dialogs return to normal in-page rendering.

**2. Service Definition creation with Certificate outcome type (#286)**
Page/flow: ControlRoom → Service Definitions → create or edit a Service Definition with Outcome Type = Certificate.
- Happy path: Create a new Certificate-outcome Service Definition and save — confirm it saves successfully with no error, and its mandatory Issuance step is already present.
- Edge cases:
  - Edit an existing Service Definition and change its outcome type to Certificate — confirm the Issuance step is scaffolded once, with no duplicates on repeated saves.
  - Create/edit Service Definitions using every other outcome type — confirm no regression (these should be unaffected).

**3. Account invite and password-reset email links (#285)**
Flow: Invite a new user, and separately trigger a "forgot password" reset.
- Happy path: Confirm the invite email link and the password-reset email link both point to the correct staging application URL and land on the expected page.
- Edge cases:
  - Confirm the links point to the staging authentication server, not a misconfigured or production URL.
  - Complete an invite acceptance and a password reset end-to-end via the emailed links to confirm no broken redirect or 404.

## Risk / regression areas
- General regression pass on the Document Editor beyond the specific fixes above — formatting, saving, and the mail-merge-fields panel (kept separate from the ribbon) should be exercised broadly given the toolbar rendering change.
- Fullscreen behavior on other document/file types viewed in the Documents section (e.g. read-only PDF/image previews), to confirm the fullscreen dialog-visibility fix doesn't affect other viewers.
- Broader pass across Service Definition / workflow step configuration, since the underlying save-timing change touches a shared code path used for automatic step scaffolding.
- Broad authentication pass: login, registration, invite acceptance, and password reset across different tenants/environments, to confirm the URL configuration change doesn't affect other account flows or tenant-specific redirects.

## No QA action
None — all PRs in this deploy are user-facing and covered above.

## Bug & Test Tracking
| Bug ID | Test Case ID | Priority | Module | Status | Assigned To | Comments |
|--------|--------------|----------|--------|--------|-------------|----------|
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
