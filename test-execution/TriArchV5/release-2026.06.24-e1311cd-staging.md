# Release 2026.06.24-e1311cd — Staging QA Guideline

**Environment:** Staging  
**Version:** 2026.06.24-e1311cd  
**Date:** 2026-06-25 (UTC)  
**Scope:** PRs merged since 2026-06-22T06:57:01Z (since previous staging deploy, run #108)

---

## New features to test

### 1. Quick Report Panel & Bugasura Support Integration (PR #227)

**Page/flow:** Toolbar → Quick Report Panel; Settings → Support page (`/Support`)

**Happy path:**
1. In any ControlRoom page, open the toolbar quick-report icon/panel.
2. Fill in the issue description, verify screenshot capture is triggered automatically.
3. Submit → confirm the issue is created in Bugasura and a success notification appears.
4. Navigate to the Support page, verify the support ticket list loads (active/open tickets shown).
5. Create a new ticket from the Support page; verify it appears in the list.

**Edge cases:**
- Submit with an empty description — expect validation error, not a silent failure.
- Submit when the Bugasura API is unreachable — expect a graceful error notification, no unhandled exception.
- Open the quick-report panel as a non-admin user — confirm the panel is accessible and submission still works.
- Verify the screenshot capture does not include sensitive data (passwords, tokens) visible in the captured area.

---

### 2. Document Editor in Storefront Documents (PR #223)

**Page/flow:** Storefront → Permit Project wizard → Step 3/4 Documents → edit existing document

**Happy path:**
1. As a citizen with an existing permit project, go to the Documents step in the wizard.
2. Click the new edit (pencil) button on an uploaded document card.
3. In the off-canvas editor: change the document title, optionally upload a replacement PDF, click Update.
4. Verify the document card in the list reflects the new title/file without a full page reload.

**Edge cases:**
- Upload a non-PDF file in the edit panel — only PDF should be accepted (`.pdf` only); expect a rejection message.
- Click Remove File in the edit panel and submit — expect the document record to be deleted and the card removed from the list.
- Edit a document while an upload is still in progress — the form should show "upload still in progress" warning and block submission.
- Try to access the edit endpoint for a document belonging to another project/user — expect 403 Forbidden.

---

### 3. Conditional Notes in Final Package (PR #214)

**Page/flow:** ControlRoom → Service Request → Packaging work surface → Step 1 (Select files)

**Happy path:**
1. On a service request with at least one activity review assignment containing conditional notes, open the Packaging work surface.
2. In Step 1's tray footer, verify the "Import Conditional Notes" checkbox is visible (only for non-read-only packaging).
3. Check the box, select base and optional additional files, click Merge & Continue.
4. Download the merged PDF and confirm a "Conditional Notes" page has been appended, with notes grouped by activity name in step order.

**Edge cases:**
- Run with no conditional notes on any ARA — verify the checkbox is present but the merged PDF has no extra page (no blank "Conditional Notes" page appended).
- Packaging in read-only mode — checkbox must not appear.
- SR with conditional notes spread across multiple activities — verify all activities' notes appear, in step order, with correct activity name headings.
- Very long conditional note text that overflows a page — verify the PDF rendering paginates correctly rather than clipping.

---

### 4. Invoicing Enhancements — Storefront Customer Inclusion, KPIs & Filtration (PR #209)

**Page/flow:** ControlRoom → Invoices → Invoicing list / by-customer view; Invoice KPI strip

**Happy path:**
1. Open the Invoices list; verify the KPI strip shows `CustomerDetailsKpi` and `InvoicingByCustomerKpi` metrics (collection amounts, customer counts) for current period.
2. Filter by Storefront customer source — confirm only Storefront-submitted SRs appear.
3. Open the by-customer summary view; verify customer name, total outstanding, and ready-to-invoice amounts load correctly.
4. Ready-to-invoice and Outstanding amount columns display values right-aligned with USD currency formatting (e.g., `$1,234.56`).

**Edge cases:**
- No invoices for the selected period — KPI strip should show zeroes, not errors.
- Customer with multiple SRs across jurisdictions — confirm amounts are correctly summed per customer.
- Filter by date range with no matching data — table shows empty state, not a JS error.
- Currency format: verify the `INVOICE_CURRENCY` constant (`USD`) is reflected correctly for all amount cells; check that changing locale in the browser does not break the `toLocaleString` formatting.

---

## Changes & fixes to re-test

### A. SR Closed / Workflow Terminal State Logic Rework (PR #221)

**What changed:** `ActivityCompletedEventHandler` now determines "last activity" at runtime (remaining Pending/Active SRAs check) instead of relying on `IsFinalStep`. `ConditionallyApproved` outcome now transitions to `Closed`. Race-condition guard expanded to also block submission on `Closed` and `CorrectionRequired` SRs.

**Regression checks:**
- Complete the last review activity on a multi-activity SR (all Approved/Pass) → SR must transition to `Approved` or `Closed` as appropriate; no lingering `UnderReview`.
- Complete a non-final activity → SR must stay `UnderReview`; next activity proceeds.
- Submit a reviewer decision after SR has been independently moved to `Closed` by another session → system must reject with a race-condition error, not corrupt state.
- SR with a conditional outcome on the last activity → SR must now transition to `Closed` (not `ConditionallyApproved`).

---

### B. Reviewer-Scoped Service Request List (PR #226)

**What changed:** Users without `ServiceRequests.Launch` permission now receive only SRs they are assigned to as reviewers, via new `GetListForReviewerAsync`/`GetCountForReviewerAsync` repository methods and new DB indexes.

**Regression checks:**
- Log in as a reviewer-only user → SR list must contain only SRs assigned to that user; no others visible.
- Log in as an admin/coordinator (with `Launch` permission) → full SR list unchanged.
- Apply filter (status, date range, tracking number) as reviewer-only → filter must work within their scoped list.
- Pagination as reviewer-only with many assigned SRs → verify paging and sort work correctly.

---

### C. DataTables Column Alignment & Contacts Bulk Delete (PR #220)

**What changed:** All `.ta-list-table` numeric columns globally left-aligned (was broken right-align); currency columns opt-in to right-align with `ta-cell-currency`; phone columns typed as `string` to preserve leading zeros; Contacts list adds bulk-delete checkbox.

**Regression checks:**
- Contacts list: phone numbers with leading zeros (e.g., `09113545861`) display correctly and sort as text, not as a truncated number.
- Invoices list: Ready-to-invoice and Outstanding columns are right-aligned with dollar amounts.
- Any list page with numeric columns (SR list, Invoices, Contacts): column headers and sort arrows align correctly with text columns (no doubled-arrow / floating-arrow rendering).
- Contacts bulk delete: select multiple contacts, click Delete → selected contacts removed; cancel clears selection without deleting.

---

### D. Packaging Work Surface — TriArch Theme & Step 2→3 Performance (PR #218)

**What changed:** Packaging surface reworked to match Document Review experience (TriArch CSS tokens, solid borders, shared `.ta-stage-loading` overlay); Step 1 tray split into two independently-scrollable halves; Step 2→3 transition performance improvements; server-side licensing fix.

**Regression checks:**
- Full 3-step packaging flow (Select → Edit → Finalize): complete all three steps on a real SR without errors.
- Step 1: left and right file trays scroll independently; selected file count updates live.
- Step 2 (editor): navigating to Step 3 after editing completes without hanging (performance regression check).
- Visual: packaging surface uses TriArch theme colors; loading overlay appears during transitions.
- Step progression lock: Step 2 and 3 tabs cannot be clicked if Step 1 is incomplete (locked hint visible).

---

### E. Correction History & Review Cycle UX Fixes (PR #215)

**What changed:** "V" prefix renamed to "R" (revision badge) throughout correction history, document panels, and activity title; review cycle timestamps now display in user's local timezone; PermitProject list removes LockState column and LockedAt filter; output file list filters to show only terminal/on-hold activity reports; supporting document uploads now accept PDF only.

**Regression checks:**
- Open an SR activity with a correction cycle → title badge shows `R2` (not `V2`); correction history drawer shows `R1`, `R2` labels.
- Correction history drawer timestamps: verify times match the user's configured timezone, not UTC.
- Storefront SR detail page correction section: `ReviewedAt` date renders in user timezone.
- Applicant output file list on SR detail: only shows reports from `Completed`, `Closed`, `Conditional`, `Failed`, or `Waived` activities; in-progress reports hidden. When SR is `CorrectionRequired`, only the `OnHold` activity's report appears.
- Storefront document upload: drag-and-drop or browse only accepts `.pdf` — Word/Excel/images rejected.
- Permit Project list: no "Lock State" column; no LockedAt date filter fields.
- Step gate bar: button label reads "Mark All Sections Reviewed" (not "Clear all reviewed").

---

### F. Activity Delete Guard & Workflow Integrity (PR #221 / related fixes)

**What changed:** `DeleteAsync` for `ServiceRequestActivity` now blocks deletion of `Active` or `Completed` activities with error `ControlRoom:00084`; `IsFinalStep` recalculated after activity deletion.

**Regression checks:**
- Attempt to delete an `Active` activity → expect error: "Activity cannot be deleted in current state."
- Attempt to delete a `Completed` activity → same error.
- Delete a `Pending` or `Cancelled` activity → succeeds; `IsFinalStep` updates correctly on remaining activities.

---

### G. General Fixes & Off-Canvas Standardization (PRs #208, #211, #212, #219, #225)

**What changed:** Miscellaneous fixes in the invoicing area (PR #211), review-related fixes (PR #219), general fixes (PR #208, #212), and off-canvas component standardization for reviewer assignment, permit project picker, and checklist form selectors (PR #225).

**Regression checks:**
- Reviewer assignment off-canvas: search for and assign a reviewer to an activity — confirm lookup still returns correct results.
- Permit project picker: select a project in any flow that uses the picker — confirm results load and selection works.
- Checklist / user-input form selector: loading and selecting a form in a service definition or activity step.
- Invoice-related pages: spot-check for any visual or data regressions introduced by the fixes in PR #211.

---

### H. Date Picker Clear Fix (PR #226, SR list)

**What changed:** Date picker "clear" now triggers the UI clear button (not just empty-string the input), fixing cases where date filters remained visually applied after clearing.

**Regression checks:**
- On the SR list, apply a `SubmittedAt` date range filter, then click "Clear Filters" → both date pickers visually reset (no residual date shown); re-applying the filter returns the full list.

---

## Risk / regression areas

- **Workflow state machine integrity:** Three PRs (#221, #226) touched the SR terminal state logic and the ARA quorum evaluation. Exercise full review cycle end-to-end (submit → assign reviewer → approve → check SR status) on both single-activity and multi-activity service definitions. Pay special attention to SRs near terminal state under concurrent edits.
- **Multi-tenancy:** The reviewer-scoped SR list uses `CurrentUser.GetId()` to filter — confirm tenant isolation is preserved; verify a reviewer in Tenant A cannot see Tenant B's SRs.
- **Permission boundaries:** The Bugasura support page, quick report panel, and output-file filtering all touch authorization. Smoke-test each with a minimal-permission user to confirm no unintended data exposure.
- **Notifications:** PR #227 modifies the in-app notification component. Trigger an in-app notification (e.g., SR status change) and confirm the notification bell, count, and panel still render and dismiss correctly.
- **Performance / DB indexes:** Two new indexes were added (`ActivityReviewAssignment.AssignedUserId`, `ServiceRequestActivity.ServiceRequestId`). Under staging load, verify the reviewer-scoped SR list and the Packaging conditional-notes query do not cause slow page loads or timeouts.
- **PDF generation (Syncfusion):** Conditional notes PDF rendering is new. Test with varying note lengths (short, long, multi-line) and many activities to confirm Syncfusion license is active in staging and no rendering exceptions occur.

---

## No QA action

| PR | Title | Reason |
|----|-------|--------|
| #217 | feat/abp skill md updates | SKILL.md developer-guide documentation + GitHub CLI settings config — no runtime change |
| #216 | feat: Enhance migration and PR validation workflows with advisory error handling | CI/CD workflow YAML changes only |
| #213 | feat: Update Claude review model and max turns | CI/CD automation model config only |

---

## Bug & Test Tracking

| Bug ID | Test Case ID | Priority | Module | Status | Assigned To | Comments |
|--------|--------------|----------|--------|--------|-------------|----------|
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
