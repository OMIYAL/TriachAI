# Release 2026.07.24-b8504ac — Staging QA Guideline
**Environment:** Staging
**Version:** 2026.07.24-b8504ac
**Date:** 2026-07-24 (UTC)
**Scope:** PRs merged since 2026-07-18

## New features to test

**BuildRoom — Project Creation (Create + Detail pages)** (#442)
- Flow: full-page **New Project** create flow (replacing the old create-from-modal path) → Project Detail page.
- Happy path: open New Project, pick/search/create a GC via the off-canvas picker, pick a jurisdiction from the searchable dropdown, save, confirm an auto-generated project number (`BR-YYYY-NNNNN`) and at least one initial trade scope in Draft status.
- Edge cases: creating a GC inline vs. selecting an existing one with a slightly different name (normalization should not fork the directory); leaving optional fields blank; verifying Value / Margin / Next owner show as `—` placeholders where data isn't populated yet; confirming the project list and Detail header/metrics/tabs render correctly, and that the old Suite parent/child expand grid is gone from the list.

**BuildRoom — General Contractors, Projects, Contacts, Documents, ITB Extractions** (#434)
- Flow: list/create/edit screens for General Contractors, Projects, and Contacts; Projects support Trade Scopes and Project Milestones.
- Happy path: create a GC, a Project, and a Contact from their respective list pages; attach a Trade Scope and a Milestone to a Project.
- Edge cases: required-field validation on create; editing an existing record; confirming Documents/ITB Extraction records associate correctly with a Project.

**BuildRoom — new Create/Edit/Index page templates** (#439)
- Flow: entity list pages with search, filter chips, bulk-delete context menu, and slide-in Create/Edit modals.
- Happy path: open an entity index page, use search/filters, create a record via the modal, edit it, bulk-select and delete.
- Edge cases: filtering with no results; bulk-delete with a mixed selection; modal validation errors.

**Catalog — Material "Price Book"** (#435)
- Flow: `Catalog` page — category sidebar (Favorites / All parts / Assemblies / per-category), search box, filter chips (All / Parts / Assemblies / Manual override / Stale price), row list, create/edit part panel, create/edit assembly panel.
- Happy path: browse categories, search for a part, create a new part (Manual override price source by default), create an assembly by adding component parts and quantities and confirm cost/labor roll up live, toggle a favorite, deactivate a part.
- Edge cases: page layout/scroll on small screens; price-source selector shows the correct selection on both create and edit; the row actions menu appears for users who should have permission (this was previously hidden by a permission bug); attempting to add an assembly as a component of another assembly (should be blocked); saving an assembly and confirming rollup + component save happen atomically (no half-saved assembly on a mid-save failure).

**Catalog — pricing import & manual-edit consistency** (#445)
- Flow: vendor price import matching/conflict pipeline, manual price edits on the Catalog Part page, discount/netting rules.
- Happy path: import a price file with clean CN matches and confirm rows apply with price history recorded; make a manual price edit on a Catalog Part and confirm it now takes the catalog write-lock and logs price history the same way import does.
- Edge cases: import row with a brand mismatch on an otherwise-matching catalog number (should be held as a conflict, not applied); an import row matching an assembly part (should be skipped and counted in the batch summary, not applied); creating/renaming a catalog part to a Catalog Number that already exists for the tenant (should be rejected); two discount rules of equal specificity overlapping — confirm a warning appears at save time (non-blocking) and netting stays deterministic regardless of rule entry order; override collisions during import should queue as Pending, not auto-apply.

**Contact forms — reCAPTCHA** (#449)
- Flow: public-facing contact form submission.
- Happy path: submit a contact form as a normal user and confirm it goes through.
- Edge cases: automated/bot-like submission behavior is challenged or blocked; confirm the form still works correctly in both Development and Production configurations.

**Feature/Edition & quota gating revamp** (#444)
- Flow: tenant editions now drive feature availability, sidebar menu visibility (BuildRoom, Catalog, ControlRoom), and usage quotas.
- Happy path: as a tenant on each edition/plan tier, confirm the menu items and features that should be available for that tier are visible and usable.
- Edge cases: a feature that should be gated off for a lower tier does not leak into the menu or remain callable via direct URL; quota-limited actions (see `TenantFeatureUsage`) correctly block or warn once a usage cap is hit; changing a tenant's edition assignment updates its available features without requiring a full reseed.

## Changes & fixes to re-test

**Payment menu is now visible to every tenant type** (#446)
Previously the Payment admin menu (plans / payment requests / gateway settings) was hidden entirely for tenants without ControlRoom enabled — i.e., pure contractor tenants couldn't see it at all. It's now shown for Host, Agency, and Contractor tenants alike, with its child items still gated individually by permission and the Payments feature flag. Re-test: log in as a Contractor-only tenant and confirm the Payment menu now appears, and that a tier with payments disabled (e.g. Agency Essentials) still correctly hides the gated children.

**Contacts list and Permit Projects list column changes, Service Request date filters now inclusive of the full day** (#448)
- Contacts list: the separate "Address" and "Company" columns are replaced with a single "Organisation" column. Re-test the Contacts list renders and sorts correctly with the new column.
- Permit Projects list: the city/state text is replaced with the resolved jurisdiction name (loaded asynchronously). Re-test the list still loads correctly, including for projects with no jurisdiction assigned (should show a dash, not an error).
- Service Requests: date-range filters (submitted, expiry, resolved, fee-paid) previously excluded records submitted later in the same calendar day when a date-only "max" filter was used. This is now fixed — filtering by a max date includes the entire selected day. Re-test filtering Service Requests by each of these date ranges with records at different times of day, including near midnight.

**Service request creator name in notifications** (#447)
Notifications and emails for service request events previously could show a missing or incorrect submitter name. The creator's display name is now resolved from Identity before notifications go out. Re-test: submit a service request and confirm the notification/email shows the correct submitter name.

**Notification & email content improvements** (#443, #440)
Email and in-app notifications for service requests and project contact invites now include more context (recipient name, project details, submission date, origin, and distinct messaging for applicants vs. coordinators on status changes; a new notification for correction resubmissions). Re-test the full notification set for: SR submitted, SR status changed (as both applicant and coordinator), SR corrected/resubmitted, project contact invited.

**Jurisdiction seed data — Georgia abbreviation corrected** (#437)
"Georgia" was seeded with the abbreviation "GE" instead of "GA". This is corrected in the seed data. Note per this change: seed-data fixes only apply automatically to newly-provisioned tenants, not tenants already provisioned — verify with a new tenant, and separately confirm whether existing tenants need a manual reseed to pick this up.

**Service request credit-check flow simplified** (#432)
Redundant credit checks and modal pop-ups during service request creation/launch were removed and streamlined; messaging around exhausted credits was updated. Re-test the full SR creation and launch flow for a tenant with credits available, and again for a tenant with credits exhausted, confirming the messaging is clear and no duplicate modals appear.

**Invoicing now requires a closed service request; contact panel label removed** (#433)
Invoicing a service request that isn't yet Closed is now blocked with a clear validation message (previously this wasn't enforced consistently). The "A/E only" label was also removed from contact panels for a cleaner UI. Re-test: attempt to invoice a Submitted/In-Review service request (should be blocked) and a Closed one (should succeed); check contact panels no longer show the removed label.

**Payment return handling after credit purchase** (#318)
A new Return page handles the redirect back from the payment provider after a credit purchase, with tenant-authorization validation on the returned payment request. Re-test purchasing credits end-to-end, including what happens if the return URL is reloaded or revisited after the purchase already completed.

**Draft project status modifier fix; Service Request form styling** (#317)
Fixes an incorrect status modifier applied to draft projects, and improves the visual styling of the Service Requests form. Re-test creating/viewing a Draft project's status, and do a visual pass over the Service Requests form.

## Risk / regression areas

- **Feature/Edition gating is now load-bearing for menu and feature visibility** (#444). Exercise every tenant edition/plan tier and confirm no previously-available feature silently disappeared for existing tenants, and no gated feature leaks through on a lower tier via direct URL navigation, not just the menu.
- **Notification pipeline touched by three separate PRs this cycle** (#443, #440, #447). Do a broad regression pass across service-request lifecycle notifications (submit, status change, correction resubmit, invoice) and project-contact invite emails, watching for duplicate, missing, or mis-addressed notifications.
- **Multi-tenancy / seed-pack propagation.** Several changes this cycle touch seed data and feature/permission definitions. Confirm behavior on both a freshly-provisioned tenant and an existing tenant for each of #444 and #437, since seed-pack changes do not automatically backfill tenants provisioned before the change.
- **Catalog write-locking and price history consistency** (#445). With both import and manual edits now taking the same lock and history path, run a manual edit concurrently with (or immediately after) an import on the same part and confirm no lost updates or missing history rows.
- **Cross-tenant payment menu visibility change** (#446) is a permission-adjacent change — confirm no tenant type gained visibility into payment configuration it shouldn't have access to, beyond the intended contractor-visibility fix.

## No QA action

- #441 — Domain Services Spec / domain-layer documentation updates (docs only).
- #438 — Documentation and model-reference updates to v13.5, including mermaid diagrams and a sample import CSV fixture (docs only, no code changes).
- #431 — Automated developer knowledge-bank documentation append (internal, docs only).
- #430 — Despite the title mentioning an ABP 10.5 upgrade, the actual diff for this PR contains only newly added documentation files; no application code, project file, or dependency changes were included.

## Bug & Test Tracking

| Bug ID | Test Case ID | Priority | Module | Status | Assigned To | Comments |
|--------|--------------|----------|--------|--------|-------------|----------|
| | | | | | | |
| | | | | | | |
| | | | | | | |
| | | | | | | |
| | | | | | | |
