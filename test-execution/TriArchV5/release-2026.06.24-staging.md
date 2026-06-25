# Release 2026.06.24 — Staging QA Guideline

**Environment:** Staging  
**Version:** 2026.06.24  
**Date:** 2026-06-25  
**Scope:** Recent merged PRs (#214–#227)

> What to test on the current staging build. Each item lists where to go, the happy path, and edge cases. Record outcomes in the tracking table at the bottom.

## 🆕 New features to test

### Quick Report / Bugasura support (#227)
- **Where:** Settings toolbar → new **Quick Report** panel; and the new **/support** page (list, create, detail).
- **Happy path:** Open the quick-report panel, capture a screenshot + diagnostics, submit an issue → it appears in the support list; open it to see detail.
- **Edge cases:** Submit with no description; very long description; submit while offline / API down (should fail gracefully, not hang); screenshot-capture denied; confirm a created ticket is scoped to the current user and visible in the list.

### Service Request list page (#226)
- **Where:** Service Requests → list.
- **Happy path:** List loads with correct columns, paging, and sorting.
- **Edge cases:** Empty list; large list paging; filters + sort combined; identifier links navigate to the correct SR.

### Document Editor in Documents (#223)
- **Where:** Documents → open a document → editor.
- **Happy path:** Open, edit, and save a document; reopen and confirm changes persisted.
- **Edge cases:** Large document; unsupported file type; save-failure handling; a user without edit rights.

### Conditional Notes in Final Package (#214)
- **Where:** Packaging → Finalize → Final Package notes.
- **Happy path:** Notes that meet the condition appear in the final package; non-matching notes are excluded.
- **Edge cases:** Boundary conditions for the rule; no notes; all notes excluded.

## 🔧 Changes & fixes to re-test

### Packaging work surface alignment (#218)
- **Where:** ControlRoom → Packaging (Select → Edit → Finalize).
- **Re-test:** 3-step surface matches the Document Review look; Step 1 tray splits into two independently-scrolling halves (Workflow Outputs / Applicant Submittals); theme tokens correct in **light and dark**; the licensing fix in Step 2 (Syncfusion) shows no license error; Step 2→3 performance is acceptable.
- **Edge cases:** Dark mode; large submittal sets (scrolling); slow Step 2→3 with many documents.

### DataTables integration (#220)
- **Where:** Any list using DataTables (e.g. SR list, document lists).
- **Re-test:** Column alignment correct; sorting/paging intact; no layout shift.

### Off-canvas standardization (#225)
- **Where:** Any off-canvas / slide-over panel across the app.
- **Re-test:** Consistent open/close, sizing, headers, and buttons; no regression in the document-upload off-canvas.

### Service Request "Closed" fix (#221)
- **Where:** Service Requests → close an SR.
- **Re-test:** Closing an SR behaves correctly (the previously-broken closed state); status reflects correctly in both list and detail.

## ⚠️ Risk / regression areas
- **Settings toolbar** — #227 adds to it; confirm existing toolbar items still work.
- **Service Requests** — touched by #221 and #226; smoke the full SR lifecycle (create → review → close).
- **Documents / Packaging** — #218, #223, #214; verify document open/edit/save and packaging end-to-end, in light and dark themes.
- **Cross-cutting UI** — #220 (DataTables) and #225 (off-canvas) touch shared components used app-wide; spot-check several pages.
- **Multi-tenancy / permissions** — confirm new support tickets (#227) and document edits respect user scoping.

## ⏭️ No QA action
- #216 (CI / PR-validation workflow advisory errors), #217 (ABP skill docs), #219 (review fixes — verify via the areas above).

## 🐞 Bug & Test Tracking
| Bug ID | Test Case ID | Priority | Module | Status | Assigned To | Comments |
|--------|--------------|----------|--------|--------|-------------|----------|
|        |              |          |        |        |             |          |

---
_Published manually as a pipeline demonstration on 2026-06-25. Once the Claude GitHub App has write access to this repo, the **TriArchV5 Staging QA Guideline** routine generates and updates this automatically on each staging deploy._
