# Release 2026.7.29.0856 — Staging QA Guideline
**Environment:** Staging
**Version:** 2026.7.29.0856
**Date:** 2026-07-29 (UTC)
**Scope:** PRs merged since 2026-07-28 (previous staging deploy)

## New features to test

### Organizations get a real display name for email branding
Tenant admins can now set an "Display name" on an organization/tenant, separate from its short internal code. That name is what recipients see in the header/branding area of outbound emails, instead of a generic placeholder. (PR #462, #463)

- **Page/flow:** Host-level admin → Tenant management (create/edit tenant, tenant list grid).
- **Happy path:**
  1. As host/tenant admin, open a tenant (or create a new one) and set Display Name (e.g., "Acme Corp").
  2. Save, and confirm the name shows correctly in the tenant list.
  3. Trigger any outbound notification email for that tenant (an invite, a status update, a password reset, or the public "Contact us" form).
  4. Confirm the email header/branding shows "Acme Corp" — not a placeholder, not blank, not the internal short code.
- **Edge cases:**
  - Leave Display Name blank on create — form should require it; if a name is genuinely absent, emails should fall back to the short tenant name, then a generic default.
  - A tenant created before this release — confirm it was auto-backfilled with its short code as a default display name, and that an admin can still edit it.
  - Enter a very long name (at/over the field's character limit) — should be rejected or truncated cleanly, not error.
  - Send a notification batch to recipients across multiple tenants at once — confirm each recipient sees their own tenant's name, with no cross-tenant mix-up.

## Changes & fixes to re-test

### "Contact us" and other notification emails no longer show the wrong or missing organization name
Previously, some transactional emails — most notably the public "Contact us" form email — either failed to render properly or showed an unrelated placeholder name in the branding area, regardless of which organization the message was for. (PR #462, #463)

- **Regression check:** Submit the public "Contact us" form as an anonymous visitor and confirm the resulting email sends successfully and shows the correct organization name, with no rendering errors and all form fields (name, email, message) intact.
- Re-test with a tenant that has no display name set — the email should still render cleanly using the fallback name, not throw an error.
- Resubmit a couple of times to rule out an intermittent template rendering failure.

## Risk / regression areas

- **Catalog / assemblies:** A database change in this release removes an existing link between an assembly's component parts and the catalog part they came from. Spot-check any screen or report in the material catalog area that shows assembly components — confirm nothing that used to display a catalog-part link on a component now shows blank or errors. (PR #464)
- **Multi-tenancy:** With organization display names now driving email branding, verify there's no cross-tenant bleed when a batch of notifications goes out to recipients in different organizations at the same time. (PR #462, #463)
- **Data integrity:** This release also tightens uniqueness rules so a general contractor name or project number can no longer be duplicated within the same organization. Try creating a contractor or project with a name/number that already exists and confirm it's rejected with a clear message rather than a raw error. (PR #464)
- **Deployment health:** Run the standard smoke test to confirm the database migration for this release applies cleanly with no errors on the staging environment.

## No QA action

- Minor email template HTML cleanup and a footer copy tweak — cosmetic only, no behavior change. (PR #463)
- A build/dependency fix in the database migrator project — internal tooling, not user-facing. (PR #463)
- New database tables for discount structures, price import batches, price conflicts, and price history — schema only in this release, with no screen or workflow wired up to them yet. Nothing to click through until a follow-up release adds the UI. (PR #464)

## Bug & Test Tracking

| Bug ID | Test Case ID | Priority | Module | Status | Assigned To | Comments |
|--------|--------------|----------|--------|--------|-------------|----------|
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
|        |              |          |        |        |             |          |
