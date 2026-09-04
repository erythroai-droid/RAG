# Audit Page CMS Schema & Fetch Layer

**Date:** 2026-09-01  
**Project:** erythro-ai  
**Status:** Implemented

## Overview

Payload CMS global `audit-page` that manages all copy for the `/audit` route.  
Audit plans (free / diagnostic / pro / delegate) live in the existing `solution-plans` collection, differentiated by a new `kind` field.

## Files

| File | Purpose |
|---|---|
| `src/fields/auditPageFields.ts` | Payload Field[] schema with 4 tabs: Meta / Form / How it works / Pricing |
| `src/globals/AuditPage.ts` | Global config, slug `audit-page`, admin group `Pages` |
| `src/lib/auditPage.server.ts` | Server-only fetch + cache layer, mirrors `legalPages.server.ts` |
| `src/migrations/20260901_010000_audit_page_cms.ts` | DB migration: audit_page tables + solution_plans.kind column |

## CMS Global: `audit-page`

### Tab structure

- **Meta** — title (localized, required), metaDescription, tabs group (audit/how/pricing labels)
- **Form** — `form` group with all audit-request form copy + `auditLanguageOptions` sub-group
- **How it works** — `how` group with kicker/heroTitle/heroIntro + 5 arrays: stats, steps, pillars, categories, principles
- **Pricing** — `pricing` group with scalars + `plans` array (nested `features` array inside each plan)

### DB table layout (Payload postgres adapter naming)

```
audit_page                           — root row (id, updated_at, created_at)
audit_page_locales                   — all localized scalars, prefixed by group

audit_page_how_stats / _locales      — how.stats array
audit_page_how_steps / _locales      — how.steps array
audit_page_how_pillars / _locales    — how.pillars array (non-loc: weight)
audit_page_how_categories / _locales
audit_page_how_principles / _locales

audit_page_pricing_plans / _locales  — non-loc: plan_id, featured, cta_href
audit_page_pricing_plans_features / _locales  — nested features array
```

### Key column naming conventions

- Groups flatten: `form_heading`, `how_kicker`, `pricing_agency_cta`
- camelCase → snake_case: `heroTitle` → `how_hero_title`, `auditLanguageOptions` → `form_audit_language_options_*`
- Array rows: varchar `id` PK, integer `_order`, integer `_parent_id`
- Nested array (features): varchar `_parent_id` pointing to plans varchar PK
- Locales tables: `_locale` uses `_locales` enum, `_parent_id` matches parent PK type

## solution_plans `kind` field

New non-localized `varchar` column on `solution_plans`:
- `'solution'` — homepage Solutions section + `/order`
- `'audit'` — `/order/audit-*` only (hidden from homepage Solutions)

Back-fill rule in migration: rows with `slug LIKE 'audit-%'` → `kind = 'audit'`.

## Fetch Layer Pattern

```ts
// server-only
import { getCachedAuditPage } from '@/lib/auditPage.server'

const page = await getCachedAuditPage()  // AuditPageContent (any)
```

- `fetchAuditPage()` — calls `payload.findGlobal({ slug: 'audit-page', locale: 'all', depth: 0 })`
- Merges over static `auditPage` fallback from `src/lib/auditPage.ts`
- Returns static fallback on any error
- `getCachedAuditPage()` — `unstable_cache` with `SITE_CONTENT_TAG`
- Return type `AuditPageContent = any` to avoid fighting `as const` tuple narrowing

## Caveats

1. **Migration table names** are hand-crafted to match Payload's naming. If Payload generates types via `payload generate:types`, it may rename or add index tables. Run `payload migrate` to apply and verify.
2. **Nested array `_parent_id` type**: `audit_page_pricing_plans_features._parent_id` is `varchar` (not integer) because the parent `audit_page_pricing_plans.id` is varchar. This matches the legal_privacy_sections_locales pattern.
3. **`AuditPageContent = any`** — intentional. The static `auditPage as const` infers narrow literal/tuple types that make the merged return type un-expressible without a full duplicate interface. Consumer code uses `tAudit(field, locale)` which accepts `Localized`.
4. **seed.ts** — not updated. TODO: add audit plan seeds when seeding is needed.
