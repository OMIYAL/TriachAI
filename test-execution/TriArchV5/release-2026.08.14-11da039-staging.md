# Release 2026.08.14-11da039 — Staging QA Guideline

**Environment:** Staging
**Version:** 2026.08.14-11da039
**Date:** 2026-08-14 (UTC)
**Scope:** PRs merged since 2026-08-11

## New features to test

### BIM Viewer module (#496)
Page/flow: new BIM Viewer menu entry → upload an IFC file → embedded 3D model viewer.
- Happy path: upload a valid, moderate-size IFC file and confirm it renders in the embedded viewer, correctly associated with the project.
- Edge cases:
  - Upload a large/complex IFC file and check load time and viewer stability.
  - Upload an invalid file (wrong extension, corrupted IFC) and confirm a clear error instead of a silent failure.
  - Access the BIM Viewer page/menu as a user without BIM Viewer authorization — confirm it is hidden/blocked.
  - Confirm IFC content streams correctly for larger files (no truncation or timeout).

### BuildRoom estimation approval workflow (#497)
Page/flow: BuildRoom project → Launch Phase → estimation workflow stages (Takeoff → Estimate → BOM Review → Quote Review → Approval → Submit), plus a new Workflow Templates admin page.
- Happy path: pick/run an estimation workflow template end-to-end through all stages and submit successfully.
- Edge cases:
  - Create, edit, and delete a workflow template from the new admin page; verify validation on bad input.
  - Try to skip a stage or revisit a completed stage — confirm the workflow enforces the expected order/state.
  - Run two estimation workflow instances concurrently on the same project/version.
  - Check the BuildRoom estimation workflow feature toggle at the tenant level (enable/disable) behaves as expected for tenants with and without the feature.

### Building model creation with IFC / Take-Off property editing (#499)
Page/flow: Take-Off stage — new Floor Details and Building property inspectors backed by the real API (mock removed).
- Happy path: open Take-Off on a project seeded from an IFC building model, edit a floor/building property, save, and confirm it persists after reload; add a new property; delete a property.
- Edge cases:
  - Delete a default catalog property, reopen Take-Off, and confirm it does **not** silently reappear (this is the specific regression this PR fixes).
  - Switch between plan sheets and confirm the Floor Details panel follows the active sheet's storey correctly.
  - Resize/collapse the new Take-Off workspace layout panels and confirm the layout persists sensibly.
  - Confirm the ITB project-confirmation flow correctly seeds the building model and maps plan sheets to IFC storeys.
  - Save an empty-value property and confirm it persists/clears as expected rather than erroring.

## Changes & fixes to re-test

### Document Review — baked-in PDF annotations no longer treated as reviewer markup (#498)
Before: PDFs arriving with markup already baked into the file (e.g. AutoCAD export markup, prior review cycles — sometimes 800+ on real plan sets) were loaded as live annotation objects. That caused the comment export to try to clip an image per pre-existing annotation, which could hit the export limit before the reviewer had added anything of their own.
After: baked-in markup is flattened into the page itself — still visible, but no longer counted as an annotation, so exports only include the reviewer's own markup.
Regression checks:
- Open a document with heavy pre-existing/baked-in markup (e.g. AutoCAD export) — confirm the markup is still visible, but only the reviewer's own new annotations are saved/exported.
- Add your own markup on top of a flattened document and confirm it saves and exports correctly.
- Try exporting comments on a document that would exceed the clipped-image export limit — confirm you get an immediate, clear message instead of waiting through processing and then getting a raw/unfriendly error.
- Re-open a document via the "share with imported markup" (merge) path and confirm existing annotations aren't duplicated.
- Reload a document after it's been re-processed and confirm you get the current version, not a stale cached copy.
- Try a document that's encrypted or otherwise can't be re-saved and confirm it still opens showing the original content.

## Risk / regression areas

- **Multi-tenancy:** confirm new BuildRoom workflow templates, BIM Viewer content, and Take-Off building-model data stay scoped to the owning tenant/project and are not visible across tenants.
- **Permission scoping:** exercise the BuildRoom estimation workflow, workflow template admin page, and BIM Viewer with users who lack the relevant permissions — confirm access is denied at a high level (no need to detail bypass steps here).
- **Performance:** large IFC files (BIM Viewer, Take-Off) and PDFs with heavy annotation history (Document Review) — watch for slow loads or timeouts.
- **General regression sweep:** since BuildRoom received a large set of related changes this cycle, re-check core BuildRoom project/service-request flows that aren't listed above still work end-to-end.

## No QA action

- **#500 — Refactor code structure for improved readability and maintainability.** Diff review shows this PR only adds an EF Core database migration for the new BuildRoom tables (migration + designer files + model snapshot); no application or UI logic changed. No dedicated QA action beyond the general BuildRoom regression sweep above.

## Bug & Test Tracking

| Bug ID | Test Case ID | Priority | Module | Status | Assigned To | Comments |
|--------|--------------|----------|--------|--------|-------------|----------|
| | | | | | | |
| | | | | | | |
| | | | | | | |
| | | | | | | |
| | | | | | | |
