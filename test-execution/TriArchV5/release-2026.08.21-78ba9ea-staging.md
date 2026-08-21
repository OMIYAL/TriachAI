# Release 2026.08.21-78ba9ea — Staging QA Guideline

**Environment:** Staging
**Version:** 2026.08.21-78ba9ea
**Date:** 2026-08-21 (UTC)
**Scope:** PRs merged since 2026-08-15 17:12 UTC

## New features to test

### Department-Scoped Estimation & Permission Groups (#504, #508, #510)
BuildRoom permissions are now split into per-launch-step groups, and every trade scope must have a Department (organization unit) assigned before its estimation phase can launch. Only members of that department — or holders of the new, explicitly opt-in cross-department right — can actively work a scope's estimation steps; everyone else sees the workspace read-only.

- **Happy path:** Assign a department to a trade scope → launch the estimation phase → work through Take-Off, BOM, Estimate, Quote Review, Approval, and Submit as a member of that department.
- **Edge cases:**
  - A non-member of a scope's department opens the workspace — confirm it renders **read-only** (visible, actions disabled) rather than a 403 error.
  - A user with zero assignable departments opens the Create/Edit Trade Scope modal — confirm a "no departments available" notice appears instead of a broken or empty picker.
  - Approval step: with every department member required to sign, verify a mid-flight roster change (someone added/removed from the department) surfaces a "Refresh approvers" prompt rather than silently altering the chain, and that previously recorded signatures are untouched.
  - "Return for revision" — confirm the new version restarts at the **BOM** stage (not Estimation), with the BOM back in Draft and approver/approved-at cleared.
  - Cross-department right — confirm it is no longer auto-granted to admin / "Projects.Edit" roles. On tenants upgraded from an earlier release, check that roles which previously picked this up automatically have had it revoked (re-grant manually if a role is genuinely cross-departmental).

### Launch flow: assigning a department is now separate from launching (#510)
Assigning a department to a trade scope and launching its phase used to be one combined action. They are now separate.

- **Happy path:** Assign a department to an un-launched trade scope (no launch happens yet) → separately trigger "Launch phase" as a member of that department.
- **Edge cases:**
  - Confirm the Project Detail page's "Launch phase" control is disabled for scopes whose department the current user is not in, and shows a "belongs to &lt;department&gt;" hint.
  - Confirm a project with many trade scopes loads its launch controls promptly (department access is now resolved in one batched call instead of one per scope).

### Create-from-ITB review: inline PDF viewer with field highlighting (#506)
When creating a project from an ITB document, an inline PDF viewer now sits beside the extracted fields, highlighting where each value was found on the page.

- **Happy path:** Upload an ITB PDF for project creation → confirm extracted fields highlight correctly on the PDF → click a field → confirm the viewer jumps to and zooms into the right page/region.
- **Edge cases:** a field sourced from page 3+ of a multi-page PDF, a field with no matching PDF region, densely packed/small text regions (zoom accuracy).

### Document upload from the Project Details "Documents" tab (#506)
Documents can now be uploaded directly from the Documents tab via an off-canvas panel, with drag-and-drop or browse, document type selection, and an upload progress indicator.

- **Happy path:** Open the Documents tab → open the upload panel → drag in a PDF → pick a document type → confirm it uploads and appears in the list.
- **Edge cases:** browse-to-upload vs. drag-and-drop, a large file's progress indicator, leaving the document type unselected, cancelling mid-upload.

## Changes & fixes to re-test

- **Assembly cost rollup now honors price-basis quantity (#509)** — Assembly costs previously assumed every component was priced per single unit. They now divide by each component's price-basis quantity (e.g. a component priced per 100 units). Re-test: create or edit an assembly containing a component with a non-1 price basis quantity, and confirm the rolled-up net cost/labor is correct both in the live modal preview and after saving.
- **Price import now also tracks list price and labor changes, and cascades correctly (#509)** — Importing a price file, resolving a price conflict, or bulk-repricing now records List Price and Labor changes in price history (previously only Net Cost was recorded), and recalculates every assembly containing a changed component exactly once — even if several changed components share the same assembly. Re-test: import a price file that changes net cost and list price for a part used in multiple assemblies; confirm every affected assembly updates once and its price history shows both the price and labor changes.
- **Manual price edits no longer flip source unless net cost changes (#509)** — Editing only a part's list price or labor units (leaving net cost untouched) should keep its existing price source (e.g. stays "File-sourced") instead of being marked as a manual override. Re-test: edit only list price or labor on a file-sourced catalog part and confirm its price source is unchanged.
- **Several BuildRoom pages were over-restricted for non-department-members and are now correctly read-only (#508)** — re-test as a user who belongs to a *different* department than the scope's:
  - Quote Review — can now *view* the bid document (not edit/send).
  - Take-Off — the canvas now loads and displays existing data (still locked for editing).
  - Trade Scope "assignable departments" picker is now reachable from the **Create** Trade Scope modal for users who only have Create rights (previously required Edit rights and could 403).
- **Agency role seeding refactor (#507)** — On a fresh tenant provisioning or a DbMigrator run, verify the expected agency roles are created with the correct default/public flags, and that no unexpected legacy roles appear.
- **Database migration (#511)** — Adds/changes indexes and columns on price history, trade scope department assignment, and approval step assignee data. No direct UI change to verify beyond confirming the migration completed cleanly (functionally covered by the department-scoped estimation and approval tests above).

## Risk / regression areas

- **Permission regression risk** — The BuildRoom permission tree was restructured into per-step groups, and one broad permission (the department-scoping bypass) was deliberately removed from admin-style roles. Broadly exercise BuildRoom role/permission administration: confirm existing custom roles retain the access they had before the upgrade, and that no user unexpectedly gains or loses access to a build room stage.
- **Multi-tenancy** — The permission cleanup and role-seeding changes run per tenant during migration. Spot-check more than one tenant to confirm each was cleaned up / seeded independently and correctly.
- **Notifications** — BuildRoom's notification-to-permission mapping was touched by the launch-flow change; confirm BuildRoom notifications still fire for the expected roles and departments.
- **Permission scoping, generally** — Broadly exercise department-based access boundaries (view vs. work vs. cross-department) across Take-Off, BOM, Estimation, Quote Review, and Approval, without needing to test specific bypass techniques — the goal is confirming the boundary holds up under normal use, not probing it.

## No QA action

- Internal unit/integration test additions accompanying #507 and #508 — covered by the functional re-tests above; no separate QA action needed.

## Bug & Test Tracking

| Bug ID | Test Case ID | Priority | Module | Status | Assigned To | Comments |
|--------|--------------|----------|--------|--------|-------------|----------|
| | | | | | | |
| | | | | | | |
| | | | | | | |
| | | | | | | |
| | | | | | | |
