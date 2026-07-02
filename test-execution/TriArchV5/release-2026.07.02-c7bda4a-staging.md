# Release 2026.07.02-c7bda4a — Staging QA Guideline
**Environment:** Staging
**Version:** 2026.07.02-c7bda4a
**Date:** 2026-07-02
**Scope:** PRs merged since 2026-06-26 (last staging deploy)

## New features to test

**CMS Events Calendar & Service Catalog widgets** (#271, #234)
Page/flow: any public page with the Events Calendar or Service Catalog widget embedded (list and tabbed layouts).
- Happy path: widget renders event/service cards with correct icons, dates, and tags; clicking "Register"/"Apply" navigates correctly.
- Edge cases: empty state (no upcoming events/services) shows the empty message, not a blank widget; tabbed layout switches groups without a page reload; unauthenticated user sees the login prompt instead of a register link; long event/service titles wrap without breaking layout.

**CMS storefront settings image upload** (#276)
Page/flow: CMS/storefront settings image upload control.
- Happy path: upload an image, save, confirm it renders in the storefront.
- Edge cases: invalid file type, oversized file, replacing an existing image, removing an image.

**"Message Contractor" citizen chat panel** (#266)
Page/flow: Service Request detail page (storefront), "Message Contractor" action.
- Happy path: applicant opens the chat panel from an SR and can send/receive messages.
- Edge cases: opening chat on an SR with no assigned contractor yet; multiple open/close cycles; chat state after page refresh.

**Reviewer role visibility in Assign Reviewer picker** (#273)
Page/flow: Control Room, Assign Reviewer panel on an activity.
- Happy path: each candidate reviewer row shows their role as a pill next to their name.
- Edge cases: reviewer with multiple roles; reviewer with no role assigned; confirm only users with the correct assignment permission can open this picker at all (permission scope was tightened in this change — see Risk section).

**Project contact email invites** (#267)
Page/flow: Permit Project → Add/Edit Contact.
- Happy path: adding a contact to a project triggers an invite email to that contact.
- Edge cases: contact with no email on file; adding the same contact twice; invite email content/branding renders correctly (see email templating below).

**Branded notification emails** (#235)
Page/flow: any Control Room email notification (SR status changes, reviewer assignment, etc.).
- Happy path: notification emails render using the shared branded layout/header instead of a bare template.
- Edge cases: emails with long titles/body content; verify no template rendering errors on any notification type introduced or touched recently.

## Changes & fixes to re-test

**Public Service Request detail page no longer leaks in-progress reports/certificates** (#278)
Regression check: open an SR that is still under review (not Closed/Rejected/CorrectionRequired) on the public storefront detail page — confirm no report/certificate download links appear. Then move the same SR to Closed and confirm they do appear. Also confirm no crash if a certificate hasn't been issued yet.

**CMS services page aria-label localization** (#277)
Regression check: services carousel/pager on the public CMS services page — confirm accessible labels (pager, page indicators) load correctly and the page doesn't error due to a malformed localization file.

**Storefront UI/UX overhaul — SR detail, Apply wizard, Permit Projects** (#274)
Regression check: full walk of the SR detail page (status tracker, status/correction history panels, document downloads, fee/payment display), the Service Apply wizard (step navigation, document upload, submission), and Permit Project list/wizard (postal code validation, contact off-canvas). Check both light and dark theme.

**Issuance/approval no longer force-closes a Service Request with steps remaining** (#272)
Regression check: for an SR whose workflow has an activity *after* Issuance, approve the Issuance/certificate step and confirm the request stays open and the next activity becomes active (does not incorrectly close). For an SR where Issuance is the last step, confirm approval still closes the request as before. Confirm the Approve button is no longer blocked by not-yet-started later steps. Confirm the public "Download Certificate" button only appears once the request is fully Closed. Confirm an issued certificate shows up as a mergeable output in Packaging.

**Service Request workflow engine refactor (reject/finalize handling)** (#268)
Regression check: reject a Service Request at multiple different stages of its workflow (early activity, mid-workflow, final activity) and confirm each rejection ends the request cleanly and consistently. Confirm normal approve-through-completion flows are unaffected at each stage.

**Storefront/Draft SR fixes: project address, Issuance restriction, inline editing, Packaging decisions** (#270)
Regression check: (1) link a Permit Project to an SR and confirm the Project Address field populates on the SR detail page; (2) attempt to add/retype an activity as "Issuance" on a service whose outcome type is None — confirm it's blocked; (3) open a Draft SR and confirm the Applicant/Property section is directly editable with a single Save button (no separate Edit toggle); (4) on a Packaging activity, confirm only "Approve" is offered as a decision (no Reject/other options).

**Storefront list pages moved to server-side AJAX tables** (#269)
Regression check: Service Requests and Permit Projects list pages — search, sort, paging, and the detail-page link (now a query-string route) all still work; draft requests still show Edit/Discard actions; permit projects list still scopes to the logged-in user's own projects.

**Control Room UI/UX pass — sticky headers, offcanvas flows, filters** (#262)
Regression check: sticky page header/toolbar on scroll across affected list and detail pages; Contacts/Fee Schedules/Request Forms offcanvas edit flows (replacing old modals); filter chips and advanced-search drawers on Service Definitions, Service Requests, Permit Projects, Contacts.

**Contact phone number validation + Service Definition create fix** (#261)
Regression check: enter an invalid phone number (letters, wrong symbols) when adding/editing a contact and confirm a clear validation error instead of silent acceptance. Create a new Service Definition and confirm its Type Code and Description save to the correct fields respectively (a parameter-order bug could previously have swapped them). Also check the submission checklist drawer correctly highlights "No" answers.

**Document upload panel adjustments** (#267)
Regression check: upload and edit documents via the storefront Document upload/edit panels — confirm validation and save behavior are unaffected.

## Risk / regression areas

- **Workflow engine changes** (#268, #272) touch core Service Request activity progression, rejection, and finalization. Exercise a broad matrix of workflow shapes (single-activity, multi-activity, Issuance-first vs. Issuance-last) rather than just one path.
- **Permission scoping**: recent changes tightened an authorization gate on a reviewer-assignment endpoint (#273). Spot-check that users without the relevant permission are correctly denied access to reviewer assignment functionality, and that authorized users are unaffected.
- **Multi-tenancy / data scoping**: the Permit Projects and Service Requests list pages now query via new AJAX endpoints scoped to the current user/tenant (#269). Confirm a user only ever sees their own tenant's and, where applicable, their own records — never another tenant's or another applicant's data.
- **Notifications/email**: the emailing module wiring changed (#235) and a new background email job was added (#267). Confirm notification and invite emails are still delivered and correctly branded, with no failures in the background job queue.
- **Performance**: the storefront list pages now depend on server-side AJAX calls (#269) — check list page load and interaction responsiveness on realistically sized data sets.

## No QA action

- #263 — CI workflow fix for the staging-deploy signal itself (internal automation only).
- #236 — Documentation-only addition (CLAUDE.md release-notes guidelines).
- #224 — Internal Lighthouse/performance-audit tooling scripts (dev-only, not shipped to users).

## Bug & Test Tracking

| Bug ID | Test Case ID | Priority | Module | Status | Assigned To | Comments |
|--------|--------------|----------|--------|--------|-------------|----------|
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
