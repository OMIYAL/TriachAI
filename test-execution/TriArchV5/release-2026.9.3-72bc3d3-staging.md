# Release 2026.9.3-72bc3d3 — Staging QA Guideline
**Environment:** Staging
**Version:** 2026.9.3-72bc3d3
**Date:** 2026-09-03
**Scope:** PRs merged since 2026-09-03 (previous staging deploy)

## New features to test
None in this deploy — this release contains a single configuration fix (see below).

## Changes & fixes to re-test
**Cross-origin access for tenant subdomains and document viewing (PR #554)**
Staging recently started routing tenants through subdomains, and the document viewer started loading files directly from cloud storage instead of downloading them first. Both changes needed matching cross-origin configuration that hadn't been applied to Staging yet, which could cause tenant logins and document previews to silently fail in the browser.
- Happy path: sign in as a tenant user on a tenant subdomain and confirm login completes without errors.
- Happy path: open a document in the Document Review / Packaging viewer and confirm it loads and previews successfully.
- Edge case: sign in from a tenant subdomain that was not previously tested (if multiple tenants exist on Staging).
- Edge case: open documents of different types/sizes in the viewer (PDF, image) to confirm all preview correctly.
- Edge case: check browser dev tools console for any cross-origin errors during login and document preview flows.
- Edge case: confirm the AuthServer sign-in redirect (OIDC callback) completes correctly when initiated from a tenant subdomain.

## Risk / regression areas
- **Multi-tenancy / subdomain routing:** Exercise sign-in and general navigation across multiple tenant subdomains on Staging, not just the apex domain.
- **Document / file viewing:** Exercise any flow that loads files directly from cloud storage (document review, packaging, take-off) to confirm nothing regressed for existing (non-subdomain) access as well.
- **Cross-browser check:** Some browsers enforce cross-origin rules more strictly — spot-check the above flows in at least two browsers if feasible.

## No QA action
- PR #553 — CI/infrastructure change only (build pipeline disk-space fix). No user-facing impact.

## Bug & Test Tracking
| Bug ID | Test Case ID | Priority | Module | Status | Assigned To | Comments |
|--------|--------------|----------|--------|--------|-------------|----------|
| | | | | | | |
| | | | | | | |
| | | | | | | |
