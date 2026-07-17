# Release 2026.07.17-0c459bb — Staging QA Guideline

**Environment:** Staging
**Version:** 2026.07.17-0c459bb
**Date:** 2026-07-17
**Scope:** PRs merged since 2026-07-14

## New features to test

**Service Request prepaid credits (#303)**
Tenants can now prepay for Service Request credits and have new Service Requests draw down that balance automatically, instead of paying per-request. A new settings page lets an admin view the credit balance and buy more via a Stripe checkout.
- Happy path: open the new Service Request Credits settings page, buy a credit package, confirm balance increases after Stripe checkout completes, then create a Service Request and confirm the balance decrements.
- Edge cases: attempt a purchase when Stripe has no active price configured for the package; create a Service Request when the credit balance is zero/insufficient; confirm existing (non-credit) invoicing/payment flows for Service Requests still work unaffected; verify the credit purchase settings page respects permission scoping (only authorized roles can view/buy).

**OCR text & drawing-scale extraction API (#301)**
A new backend API can extract text from uploaded documents (images, PDFs, Office files) and detect/normalize drawing scale notations (e.g. `1/4" = 1'-0"`, `1:100`) from architectural drawings. This is an API-level capability with no dedicated UI in this release.
- Happy path: call the extract-text and detect-drawing-scale endpoints (e.g. via Swagger) with a sample PDF/image and confirm text and a normalized "primary scale" are returned.
- Edge cases: multi-page documents with more than one scale notation; a document with no detectable scale; unsupported/corrupt file input; multiple documents in a single request.

**Service Request-scoped AI chat (Service Request RAG) (#300, #304)**
CodeAdvisor chat can now answer questions using a single Service Request's own documents (submittals, corrections, linked project files) in addition to the general jurisdiction code library — with strict isolation so one Service Request's documents never leak into another's or into the general jurisdiction chat.
- Happy path: open a Service Request's chat, ask a question answerable only from that SR's submittal/correction documents, confirm the answer is grounded correctly; confirm the general (non-SR) jurisdiction chat still works and does not surface SR-specific content.
- Edge cases: two different Service Requests in the same jurisdiction — confirm SR A's documents never appear in SR B's chat or the general chat; re-index a Service Request where one document fails to process (e.g. corrupt file) — confirm the rest of the batch still indexes successfully and the failed document is clearly marked as failed rather than silently stuck (regression covered by #304); indexing a group (submittals-only or corrections-only) re-test.

**Bulk delete for in-app notifications, redesigned notifications page (#305, #314)**
The notifications page has a refreshed layout and now supports selecting and deleting notifications individually, in bulk, or all-at-once (respecting current filters).
- Happy path: select several notifications and delete them; delete all notifications matching the current filter; delete a single notification.
- Edge cases: select-all across a paginated/filtered list, then delete; delete with no notifications selected; confirm deletion messaging/localization displays correctly; confirm the new table's length/pagination controls work.

**Delete a Contact (#310)**
Contacts can now be deleted from the Contacts list. If a contact is still referenced elsewhere (e.g. linked to a project/service request), deletion is blocked with a clear message instead of silently failing or corrupting data.
- Happy path: delete a contact with no other references; confirm it disappears from the list.
- Edge cases: attempt to delete a contact that is linked to a project/service request and confirm the specific "contact in use" message appears; confirm the underlying record is not partially deleted when blocked.

**Enforce one Project Owner per project (#313)**
When adding or editing a project contact, the Role dropdown no longer defaults to "Project Owner" (it now requires an explicit selection) and shows a hint that only one Project Owner is allowed per project. Attempting to assign a second Project Owner is blocked with a clear message. This applies both in the internal ControlRoom contact panel and the public-facing project contact panel.
- Happy path: add the first contact as Project Owner on a project — succeeds; add additional contacts with other roles — succeeds.
- Edge cases: attempt to add a second Project Owner to the same project (should be blocked with the new message, both internally and on the public storefront panel); edit an existing non-owner contact's role to Project Owner when one already exists (should be blocked); edit the existing Project Owner's own record without changing role (should still succeed); submit the Add Contact form without selecting a role (should show required-field validation).

**Sidebar visibility permissions decoupled from feature access (#306)**
Whether a sidebar shortcut appears for Service Requests, Contacts, Request Forms, Projects, Documents, and Store Front Settings is now controlled by its own "show in sidebar" permission, separate from the permission that actually grants use of the feature. Existing role sidebars are seeded to match prior behavior.
- Happy path: log in as each seeded agency role (Agency Admin, Operations Manager, Reviewer, Intake Clerk, Read Only) and confirm the same sidebar items appear as before this change.
- Edge cases: grant a role the functional permission for one of these areas without the new "show in sidebar" permission — confirm the feature is still usable by direct URL/API but the sidebar shortcut does not appear; revoke the "show in sidebar" permission from a role that previously had it — confirm only the shortcut disappears, not the underlying access.

## Changes & fixes to re-test

**Invoice customer now resolved from Project Owner, not "primary contact" (#309)**
Contacts are no longer flagged as "primary" from the UI, so invoice/billing logic that used to look for the primary contact has been switched to use the Project Owner role instead. Re-test that invoices, invoice runs, and the storefront customer view all resolve to the correct billing contact on projects created both before and after this change.

**Packaging file checkboxes no longer pre-checked (#307)**
On the document packaging step, the output and submittal file checkboxes used to load pre-checked (and the "selected" counter showed the full count) even though nothing had actually been selected yet. They now load unchecked with a selected count of 0. Confirm you must explicitly select files before packaging/merging, and that the counter accurately reflects your selections.

## Risk / regression areas

- **Permission scoping across roles:** the sidebar visibility change (#306) and the RAG document isolation (#300) both depend on correct permission/scope enforcement — exercise each affected area (Service Requests, Contacts, Request Forms, Projects, Documents, Store Front Settings, and SR-scoped chat) under multiple roles and tenants rather than a single admin account.
- **Multi-tenancy:** confirm the new Service Request Credits balance and purchase flow, and the SR-scoped AI chat, are correctly isolated per tenant/jurisdiction and don't cross tenant boundaries.
- **Payments:** the credit purchase flow (#303) adds new Stripe-driven payment handling alongside the existing invoice payment path — regression-test standard invoice payment end to end, not just the new credit purchase.
- **Notifications performance:** re-test notification list load and bulk delete with a large number of notifications to confirm no timeout or partial-delete issues.

## No QA action

- **#302 — Add ControlRoom Suite customized templates.** Developer code-generation scaffolding only; no runtime or user-facing behavior change.
- **#294 — Swagger UI head content fix.** Restores the API documentation explorer's default script loading; affects the internal API docs page only, not the application.

## Bug & Test Tracking

| Bug ID | Test Case ID | Priority | Module | Status | Assigned To | Comments |
|--------|--------------|----------|--------|--------|-------------|----------|
| | | | | | | |
| | | | | | | |
| | | | | | | |
| | | | | | | |
| | | | | | | |
