# Release 2026.09.07-a3318d4 — Staging QA Guideline
**Environment:** Staging
**Version:** 2026.09.07-a3318d4
**Date:** 2026-09-07
**Scope:** PRs merged since 2026-09-04

## New features to test

**Approval surface rebuild (#566)**
Page/flow: the Approval work surface an approver uses to sign off on a bid.
- Happy path: open a project in the Approval stage → the pinned verdict strip shows the bid, the blocking condition, and the Sign/Return actions → review the cost build-up, quoted scope, and site conditions → open the quote from its document card, which opens full-size in a new tab → complete the attestation in the confirm drawer → submit.
- Edge cases:
  - A bonded project priced below the margin floor: confirm the shortfall is shown as a clear negative/below-floor indicator, not a misleadingly positive one.
  - An unbonded project: confirm no bonded-floor language appears at all.
  - A revision re-open: confirm the messaging says the project reopens at BOM, consistently with the rest of the screen.
  - A project where prevailing wage or union local was never captured during intake: confirm the field is hidden, not shown as "No".
  - Compliance & exposure "Code adds" figure reflects real recorded values, not always $0.

**ITB intake data now flows into the project (#566)**
Page/flow: ITB extraction → review/confirm → create project.
- Happy path: extract an ITB with prevailing wage / union local detected → confirm on the review screen → create the project → confirm those values carry through.
- Edge cases:
  - Manually created project: set "Bond Required" after creation and confirm the bonded margin floor then applies on Approval.
  - Generate a quote document, save it, and confirm the Scope Narrative / Exclusions sections are populated (not blank) in the generated document.
  - Rename a section heading in a saved quote and re-save: confirm the existing narrative/exclusions text is left untouched (no crash or data loss), per the documented best-effort behavior.

**Contractor workspace: My Work, Estimates, Quotes & Awards (#565)**
Page/flow: a contractor's cross-project work views. My Work is also the new default landing page for non-admin contractors.
- Happy path: log in as a contractor → land on My Work → see approvals awaiting signature and active trade scopes → (Estimates and Quotes & Awards are not yet in the sidebar, so reach them by direct link) confirm the pipeline and submissions/outcomes views show accurate data.
- Edge cases:
  - A trade scope that was never launched still appears in Estimates.
  - A bid that has been Awarded no longer sits in the "live scopes" queue on My Work (previously it stayed forever).
  - Scopes in Lost/Declined/Closed/HandedOff remain excluded from the live queue.
  - Load these pages against a large/realistic data set and confirm no errors or timeouts.
  - A bonded project below the margin floor is flagged in Estimates; an equivalent unbonded one is not.

**Sidebar reorganization (#565)**
- Happy path: log in as a contractor and confirm daily-work, pricing, directory, and setup items now sort above Administration.
- Edge cases:
  - The BIM Viewer link is gone from the sidebar; confirm Take-Off (which shares the same underlying data) still works.
  - Payment now lives under Administration for contractors — confirm it's reachable there.
  - Compare admin vs. contractor sidebars to confirm nothing that should still be visible was accidentally hidden.

**Demo Data tool (#565)**
Page/flow: Administration > Demo Data (admin-only).
- Happy path: create demo data → confirm a full 7-project bid pipeline appears covering every state, including a bonded project below the margin floor and an unbonded one that is correctly not flagged → remove demo data → confirm only the seeded records are gone.
- Edge cases:
  - Create twice in a row without removing in between: confirm no duplicate or broken records.
  - Remove then re-create: confirm a clean pipeline reappears.
  - Confirm the page and its permission are visible only to the intended admin role(s), and behave correctly in both host and tenant contexts.

## Changes & fixes to re-test

- **Take-Off crash on manually created projects (#565):** previously errored with "No building model exists for this project." Open Take-Off for a manually created project and confirm it now shows a proper empty state instead of an error.
- **Approval margin sign/color (#566):** confirm a below-floor bid now shows a correctly signed, correctly colored indicator.
- **Compliance & exposure "Code adds" (#566):** confirm the figure reflects actual data instead of always reading $0.
- **Revision reopen messaging (#566):** confirm consistent wording about where a revision reopens.
- **Quote viewing performance (#566):** the embedded PDF viewer was replaced with an "open in new tab" document card; confirm the Approval page loads noticeably faster and the quote still opens and displays correctly.
- **My Work queue for Awarded bids (#565):** confirm an awarded bid drops out of the contractor's live-scopes queue.

## Risk / regression areas

- **Permission scoping:** a new permission was added for the Demo Data feature and existing trade-scope/approval checks were touched. Exercise the Approval surface, the new workspace pages, and Demo Data as different roles (admin, contractor, approver) and confirm each sees only what they should.
- **Multi-tenancy:** Demo Data is available on both host and tenant sides. Verify creating or removing demo data in one tenant does not affect other tenants or the host.
- **Performance:** the Approval surface dropped a large client-side viewer bundle; spot-check load time there and on the new workspace pages, which aggregate data across many projects.
- **Cross-cutting data queries:** the new workspace views combine data from multiple sources in a single pass — watch for missing or duplicated rows in My Work, Estimates, and Quotes & Awards.
- **Localization:** new and renamed display strings shipped in this release — confirm no missing-translation placeholders appear on any touched screen.

## No QA action

None — both PRs in this release (#565, #566) contain user-facing changes.

## Bug & Test Tracking

| Bug ID | Test Case ID | Priority | Module | Status | Assigned To | Comments |
|--------|--------------|----------|--------|--------|-------------|----------|
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
