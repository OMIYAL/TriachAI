# Release 2026.08.14-bd89f59 — Staging QA Guideline
**Environment:** Staging  
**Version:** 2026.08.14-bd89f59  
**Date:** 2026-08-14 UTC  
**Scope:** PRs merged since 2026-08-14 11:41 UTC (previous staging deploy)

## New features to test

### Auto-detect device button on the BuildRoom Take-Off page (PR #501)
A new "auto detect" toggle button has been added next to the existing "Add Manual Device" control on the Take-Off page's device panel.

- **Page/flow:** BuildRoom → Project → Launch Stages → Take-Off, device panel toolbar.
- **Happy path:** Open a project's Take-Off page, confirm the new button (bullseye icon) renders next to "Add Manual Device," and toggling it visibly changes its pressed state.
- **Edge cases:**
  - Verify the button is present and behaves consistently across different screen widths (the Take-Off pane layout is collapsible/resizable).
  - Confirm it does not interfere with the existing manual "Add Device" flow.
  - Check keyboard/screen-reader accessibility (button exposes an aria-label/aria-pressed state).
  - Confirm behavior on a project with no devices yet vs. one with existing devices placed.

## Changes & fixes to re-test

### Custom property kinds on Take-Off (Floor Details / Building) round-trip correctly (PR #501)
Editing custom properties on the Take-Off page's Floor Details and Building panels now preserves the property's type (e.g. Integer, Label, ID) even when the value is left empty, instead of silently coercing everything to a generic text/number type.

- Add a custom property of each kind (Text, Number, Integer, Boolean, Label, ID, Area, Length) with a value, save, and confirm it round-trips with the correct kind and value after a page refresh.
- Add a custom property of Integer or Label kind with an **empty** value, save, and confirm the kind is preserved (previously this could be lost/mis-typed).
- Edit an existing predefined (catalog) property and confirm its kind still saves as expected — the priority between the inspector's kind and the catalog's default kind changed, so re-verify catalog properties (e.g. Area, Length) aren't mis-saved as the wrong kind.
- Cite: PR #501.

### Duplicate-save / duplicate-add protection on Take-Off property inspector (PR #501)
Saving or deleting a property row, and adding a new property, now disables/hides the relevant controls immediately while the request is in flight, instead of only after a delay.

- Rapidly double-click "Save" on a property edit and confirm only one save request goes through (no duplicate rows, no duplicate save toasts).
- Rapidly double-click "Delete" on a property and confirm it isn't submitted twice.
- While adding a new property, confirm the "Add" control is hidden/disabled until the in-flight add completes, and that a second "Add" cannot be triggered mid-save.
- Cite: PR #501.

### ITB document extraction analyzer upgraded to v3 — adds Scales field (PR #502)
The ITB (Invitation to Bid) document extraction pipeline now uses an upgraded analyzer that additionally extracts a "Scales" field under sheet details.

- Run the full "BuildRoom: E2 — Project Creation via ITB Extraction" flow (upload an ITB document, run extraction, confirm project creation) end to end on staging and confirm it still succeeds.
- For a document containing scale information on its sheets, confirm the extracted Scales field is now populated under sheet details and flows through to wherever sheet scale is consumed downstream (e.g. Take-Off plan sheet scale configuration).
- Spot-check a previously-working ITB extraction document to confirm existing extracted fields (project name, addresses, other ITB fields) are unaffected by the analyzer version change.
- Cite: PR #502.

## Risk / regression areas

- **BuildRoom IFC/property persistence:** The property-kind resolution logic on the backend changed (how a saved property's type is inferred/canonicalized). Broadly exercise adding, editing, and deleting Take-Off properties across multiple projects and property kinds to catch any regression in how existing (pre-upgrade) property data is read back.
- **AI/document extraction pipeline:** The ITB analyzer version bump changes what the extraction service returns. Exercise ITB extraction across a variety of document layouts/formats to confirm extraction quality hasn't regressed for existing fields, in addition to validating the new Scales field.
- **Config/secrets housekeeping:** Some unused placeholder configuration entries were removed as part of the analyzer update. Confirm document-intelligence and content-understanding-dependent features (ITB extraction) still authenticate and run correctly on staging after this deploy.

## No QA action

- None — both merged PRs since the last staging deploy contain user-facing or extraction-behavior changes and are covered above.

## Bug & Test Tracking

| Bug ID | Test Case ID | Priority | Module | Status | Assigned To | Comments |
|--------|--------------|----------|--------|--------|-------------|----------|
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
