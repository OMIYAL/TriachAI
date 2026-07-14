# Release 2026.07.14-7bf1952 — Staging QA Guideline

**Environment:** Staging
**Version:** 2026.07.14-7bf1952
**Date:** 2026-07-14 (UTC)
**Scope:** PRs merged since 2026-07-04

## New features to test

**Tenants now come pre-loaded with notification email templates** (#290)
New tenants provisioned through the ControlRoom setup flow now automatically receive a full set of ready-to-use email notification templates (service request created/updated/status changed, fee payment requested, fee waived, review launched, reviewer decision submitted, activity assigned, project/permit created, contact invites, etc.), loaded from a built-in catalog instead of arriving empty.
- Happy path: provision a brand-new tenant end-to-end and confirm the notification templates appear already populated and readable, with correct merge fields/placeholders rendered.
- Edge cases: re-run provisioning against an existing tenant (should not duplicate or corrupt existing templates); trigger at least 2-3 of the listed notification events (e.g. service request created, fee waived) and confirm the actual email sent matches the seeded template; verify templates display correctly for a tenant using a non-default language/locale.

**Broader file support for knowledge-base / workspace document uploads** (#291)
Documents uploaded to the AI-powered knowledge base / workspace area can now be text-extracted from a much wider range of file types — images (JPG, PNG, BMP, TIFF, HEIF), Word/Excel/PowerPoint files, and HTML — in addition to what was supported before, with improved extraction accuracy and configurable chunking of long documents.
- Happy path: upload a PDF, a Word document, and an image containing text to a workspace data source; confirm each is accepted, processed, and searchable/usable by the AI feature afterward.
- Edge cases: upload a very large document near the 100 MB limit; upload a scanned/image-only PDF; upload a file type that is NOT in the supported list and confirm it is rejected with a clear message; upload a file with a very long single block of text and confirm it is still processed without failing.

## Changes & fixes to re-test

**Sign-up now supports an email verification code step** (#289)
The registration form has been updated to support entering an email verification code as part of sign-up, with improved client-side validation and clearer enable/disable states on the submit button. Password strength feedback has also been reworked.
- Retest: complete registration end-to-end for a tenant where email verification is required, including requesting/using the "Resend code" option; complete registration for a tenant where email verification is NOT required and confirm the flow still works as a single step with no leftover verification-code field; check password strength indicator behavior while typing a weak vs. strong password; confirm form validation messages still appear correctly for missing/invalid fields.

**Document editor: dialogs and menus no longer go invisible in full screen** (#287)
Previously, opening a document, then switching the Document Editor to full screen, then opening things like Insert Table, a color picker, a context menu, or a dropdown from the ribbon could render those popups invisible (present but not shown). This is now fixed.
- Retest: open a document, THEN toggle full screen (this specific order was the broken case), and confirm Insert Table, color pickers, context menus, and other ribbon dropdowns are all visible and usable.
- Retest the reverse order too: toggle full screen first, then open a document, and confirm popups still work.
- Confirm Rename, Move, and Share dialogs, and the row menu, still work correctly both in and out of full screen.
- Confirm exiting full screen restores the page layout correctly with nothing left stranded or duplicated.

## Risk / regression areas

- **Underlying platform library upgrade** (#288, #292): the application's core platform packages were upgraded solution-wide. No specific new user-facing behavior is expected, but because this touches nearly every module, a broad smoke pass across core areas (login, Service Requests, Invoicing, Document Editor, Storefront) is recommended to catch anything unexpected.
- **Application startup / configuration on staging** (#291): a new startup-time check was added that requires a specific workflow-related configuration value to be present in non-development environments; if it's missing, the application will fail to start entirely. Confirm staging comes up cleanly after this deploy and that every major area of the app is reachable, not just the areas touched by this release.
- **File upload limits and allowed types** (#291): upload-size and allowed-file-type configuration changed together in this release. Spot-check that the 100 MB upload limit and file-type restrictions still behave consistently across all upload entry points (form attachments, Storefront supporting documents, document editor, knowledge-base/workspace uploads) — not just the new upload paths.

## No QA action

- #293 — Backend build/dependency pin fix to prevent a database migration tooling crash. No user-visible change.
- #288, #292 — Underlying platform/package version upgrade (see Risk section above for the associated regression recommendation).

## Bug & Test Tracking

| Bug ID | Test Case ID | Priority | Module | Status | Assigned To | Comments |
|--------|--------------|----------|--------|--------|-------------|----------|
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
