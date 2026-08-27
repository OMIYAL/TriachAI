# Release 2026.08.27-a8342b4 — Staging QA Guideline
**Environment:** Staging
**Version:** 2026.08.27-a8342b4
**Date:** 2026-08-27 (UTC)
**Scope:** PRs merged since 2026-08-25 (previous staging deploy)

## New features to test

**Primary contact designation for General Contractor contacts** (#528, #530)
Page/flow: Contacts create/edit (standalone Contacts page and the new GC Contacts panel).
- Happy path: pick a General Contractor on a contact, check "Is Primary," save — contact should be marked primary for that GC.
- Edge cases: the checkbox should stay disabled until a GC is selected, and clear if the GC selection is removed before saving; marking a second contact primary for a GC that already has one should prompt a "replace primary contact?" confirmation, and canceling it should leave the original primary contact intact; editing the existing primary contact without changing the flag should not trigger that confirmation; marking primary contacts for two different GCs back-to-back should not cross-affect each other.

**Contacts side panel on the General Contractors list** (#528)
Page/flow: BuildRoom → General Contractors list → new "Contacts" row action.
- Happy path: opens a side panel scoped to that GC's contacts, with search and an Active/Inactive filter, loading more contacts as you scroll.
- Edge cases: a GC with zero contacts (empty state); a GC with many contacts (scroll pagination); combining search text with the Active filter; the "Contacts" action should not appear for users without permission to view contacts.

**GC contact picker on ITB project intake** (#528)
Page/flow: BuildRoom → Projects → Create from ITB.
- Happy path: "GC Contact Name" is now a searchable dropdown of that GC's existing contacts (with a "Primary" badge on the primary one), instead of free text.
- Edge cases: selecting a GC with no contacts yet (dropdown should be empty and not error); switching to a different GC after picking a contact (previous selection should clear); a contact name auto-extracted from an uploaded ITB PDF should still prefill correctly into the dropdown.

**Bulk select & delete on Price Book, Category Parts, and Discount Structures lists** (#526)
Page/flow: BuildRoom → Price Book (parts list), Category Parts list, Discount Structures list.
- Happy path: a checkbox column with "select all" appears; selecting rows shows a bulk-delete action; single-row click still opens Edit.
- Edge cases: bulk delete on a filtered/searched result set; select-all/indeterminate checkbox state; a partially-failed batch delete should refresh the table to reflect what actually deleted; users without delete permission should not see the checkboxes/bulk-delete controls at all.

## Changes & fixes to re-test

**"Company" field removed from GC contacts** (#528, #530)
The Company text field is gone from contact create/edit/filter/list columns and the contacts Excel export (paired with a database schema change dropping the column). Verify the Contacts page still lists, sorts, and exports correctly with the column gone — the default sort/column alignment shifted when the column was removed. Existing contacts created before this deploy should still open and save correctly with no errors from the missing field.

**Manual (non-ITB) project creation removed from the UI** (#528)
The "Manual" project-creation option is no longer reachable from the New Project screen. Confirm the tile/route no longer appears, and that navigating to the old manual-create page directly doesn't throw an unhandled error (should be blocked/redirected gracefully).

**"Save as draft" button removed from project creation** (#528)
Previously a disabled button with a "not available" tooltip; now removed entirely from both the manual and ITB project-creation screens. Confirm it's gone and the remaining Cancel/Create buttons still lay out correctly.

**General Contractor picker list could go stale after rapid actions** (#528)
Fixes a race condition where quickly searching/clearing/reopening the GC picker (used during ITB intake / project creation) could leave a stale or incomplete list on screen. Re-test by rapidly repeating search/clear/reopen several times and confirming the list always settles on the correct final result.

**Price Book part fields now properly validate numbers** (#526)
Labor Units, Trade List Price, and Net Cost on Add/Edit Part became real numeric inputs (decimals allowed, negatives rejected) instead of unvalidated text fields. Try pasting non-numeric text, negative values, very small decimals, and blank values.

**Price Book bulk reprice — manufacturer selection** (#526)
Manufacturer changed from free text to a dropdown of real active brand names, with the price preview updating on selection instead of on every keystroke. Verify the dropdown lists the correct active brands and the "Any manufacturer" default still works.

**Assembly component quantity field no longer snaps while typing** (#526)
Typing a decimal quantity like "0.5" previously got clamped to the minimum value mid-keystroke. Confirm decimals now type smoothly, the field formats to 3 decimals on blur, and the extended price recalculates live. Try clearing the field and pasting negative/huge values.

**Category Parts list filters and layout reworked** (#526)
The old collapsible "Advanced Filters" panel was replaced with All/Active/Inactive chip filters; a duplicate Actions column was removed; the category name is now clickable to edit. Verify chip filtering (alone and combined with search text), name-click-to-edit, and that Excel export respects the active filter.

**Discount Structures — manufacturer dropdown and permission fix** (#526)
Manufacturer on Create/Edit is now a dropdown of brands but still displays/saves correctly for existing rules pointing at an inactive/legacy manufacturer. Also fixes a permission check so edit/delete controls show correctly for authorized users. Verify with both delete-only and edit-only permission roles.

**Price Book column header alignment** (#526)
Visual-only fix so the "Part" header aligns with part names and numeric column headers align with their sort arrows. Spot-check at different browser widths and with the table scrolled horizontally.

**Payment return URLs now resolve per tenant** (#527)
Stripe payment pre-payment and post-payment redirect/callback URLs are now built from the server-resolved tenant instead of a single fixed URL. Verify a payment initiated from a tenant context returns to that tenant's own URL after completion (and cancellation), and that a payment initiated with no tenant context still returns to the base URL. Do not need to test or document any bypass mechanics — just confirm redirects land on the expected, correct host in each case.

## Risk / regression areas

- **Multi-tenancy on payment redirects (#527):** exercise the Stripe payment flow from at least two different tenant contexts plus a non-tenant context, confirming each returns to its own correct domain and that no cross-tenant redirect is possible.
- **Permission scoping:** re-verify that the new Contacts panel, bulk-delete controls (Price Book / Category Parts / Discount Structures), and Discount Structures edit/delete controls all correctly hide for roles that shouldn't have access.
- **Data integrity on the Contacts schema change (#528, #530):** spot-check contacts that existed before this deploy — they should open, display, and save cleanly now that Company is gone and IsPrimary exists.
- **Shared error-handling path (#529 — see "No QA action" below):** this change instruments the exception pipeline across all four application hosts. As a light smoke check only, trigger a couple of ordinary error scenarios (an invalid form submission, a not-found page) on each host and confirm the user-facing error behavior is unchanged.
- **General performance:** with several bulk-selection UIs added this release, spot-check list performance on Price Book / Category Parts with a large number of rows.

## No QA action

- **#529 — Capture server-side exceptions in Application Insights.** Backend observability/telemetry only; no user-facing behavior change. (A brief cross-cutting smoke check is called out under Risk areas above, since it touches shared exception-handling middleware.)

## Bug & Test Tracking

| Bug ID | Test Case ID | Priority | Module | Status | Assigned To | Comments |
|--------|--------------|----------|--------|--------|-------------|----------|
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
