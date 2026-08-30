# Pitfalls — QA Auditor

## PIT-027 — Production `/admin` Cloudflare 524 / Vercel 300s timeout (Erythro.ai)

**Seen:** 2026-08-30 `https://erythro.ai/admin` — Cloudflare 524 at 00:46:33 UTC.

**Symptom:** Public pages work. `/admin` hangs then Cloudflare 524. Vercel: `Task timed out after 300 seconds`. Logs show Payload prompt: “It looks like you've run Payload in dev mode… Would you like to proceed? (y/N)”.

**Cause:** Local `next dev` against production `DATABASE_URL` writes `payload_migrations` (`name=dev`, `batch=-1`). Serverless `getPayload()` runs `prodMigrations`, hits that row, and `prompts()` waits on stdin. Public routes often miss this because `getSiteContent` is cached.

**Fix:** `DELETE FROM payload_migrations WHERE batch = -1;` then wait for hung isolates (≤300s) or redeploy. Do not pass `prodMigrations` at Vercel request runtime (migrations run at build via `payload migrate`). Local `pnpm dev` must set `PAYLOAD_DISABLE_PUSH=1`.

**Prevent:** Never Drizzle-push against prod Supabase. Schema changes go through `src/migrations/`.

---

## PIT-028 — Missing solution_plans_features.home_only / features_locales.full (Erythro.ai)

**Seen:** 2026-08-30 API Tests. Migration rows present; columns dropped by local Drizzle push against prod (same incident as PIT-027).

**Fix:** `pnpm db:fix-solution-feature-columns` (ADD COLUMN IF NOT EXISTS for `home_only` boolean and locales `full` jsonb). Wired in CI before API tests.

---

## Protocol block: chips white-on-black, line-height 24, title/body split

- EXTRA_CSS used to paint `.scope-chip` gray (`#f4f4f4` / `#1e1e1e` text) after the template — chips must stay `color: #fff; background: #1e1e1e`.
- Sidebar «протокол / выполненного / аудита» is 3×24px: `.section-scope .toc-bar` must stretch (`min-height: 72px`), not the global 60px bar.
- List items split on ` — ` into `.scope-item-k` / `.scope-item-v`. Lead must not repeat chip numbers (73 types / 229 observations) or the block looks like one dense paragraph.

## Hebrew footer: phone and email bidi

- Do not set `direction: rtl` only on `.footer-contact-label` while `.footer-actions` stays LTR. The colon then jumps to the left of `טלפון` / `אימייל`, and the label sits on the left of the LTR value.
- Put `dir="rtl"` on the whole `.footer-contact` pair and `dir="ltr"` + `unicode-bidi: isolate` on the phone/email value.

## Protocol block must show lab volume, not 5–7 scorecard bullets

- «Протокол выполненного аудита» is the lab coverage, not the 5 executive scales or the 7 AI Visibility score points.
- Numbers come from `audit_data.json` via `ReportScopeOfWork.Stats`: unique signal types, observations (locale × funnel × Lighthouse), sitemap loc count, candidates, pages opened, pack disclosure (Free 1 / Diagnostic 5 / Pro 10 URLs).
- Do not write «детальный протокол остаётся у Erythro.ai». The client PDF must list the check families with counts. Scorecard is a rollup.

## A4-4 diagnostic checklist: 11 summary cards, excellent first

- Diagnostic stays at the aggregated table size (10–11 cards), not the Pro 28-row dump.
- On the page, cards go excellent → needs-attention → critical (same as Pro). Selection may still swap in failing aggregated extras (SEO/OG, a11y, TTFB, locales, agent summary) and drop spare greens so problems stay in the 11.
- Do not add per-URL `agent_*` funnel cards in Diagnostic.

## A4-4 diagnostic: missing «Решение от Erythro.ai:» in top-3 cards

- Diagnostic cards must always include the solution paragraph (`finding-box solution`), same as Free and Pro. Free/Diagnostic use the teaser copy; Pro uses the real fix.
- Do not hide it with `html.tier-diagnostic .finding-box.solution { display: none }`. That rule was meant for locked checklist recs only (`check-acc`), not the top-3 block.

## Agent Readiness / markdown negotiation

- `documentRequest()` шлёт `Accept: */*`. Проверка markdown **обязана** идти отдельным запросом с `Accept: text/markdown`. Иначе CDN/сервер всегда отдаст HTML и L1-5 ложно провалится (или наоборот — не отличить форматы).
- Нет `Vary: Accept` — CDN может отдать markdown браузеру или HTML агенту. Фиксируем `vary_accept=false` (warn), pass/fail по Content-Type и телу.
- Контрольный GET с `Accept: text/html` обязателен: если оба Accept отдают markdown, `browser_broken=true`.
- Cloudflare Managed robots.txt часто ставит `Disallow: /` для GPTBot и вырезает `Content-Signal`. На эталоне erythro.ai Managed должен быть выключен.
- Не требовать URL вида `/page.md`: эталон erythro.ai использует content negotiation на той же URL, что HTML.

---

## PIT-029 — Audit landing copy vs QA_Auditor report tiers (Erythro.ai)

**Seen:** 2026-08-30 `/audit` vs `C:\agents\website-auditor\erythro-ai`

**Symptom:** Marketing claimed ~55–60 checks, “homepage + 5 funnel pages” on Free, and Diagnostic as a full 60+ checklist with a fix plan.

**Cause:** Copy described the lab in the abstract, not `A44Tier` disclosure: Free 1 URL (scorecard, top-3, Lighthouse); Diagnostic 5 URLs + summary cards; Pro full 60+ checklist + recommendations. Scale names in PDFs are Speed & Mobile UX, SEO & Visibility, Lead gen & Forms, Security & Stability, AI Visibility & Brand Discovery.

**Fix:** Align `src/lib/auditPage.ts` (including `/audit` «How it works»), `src/lib/orderPlans.ts`, and order UI with `QA_Auditor` `A44Copy` / `ReportScopeOfWork` / `AGENTS.md`. Client PDFs say “60+”, not “55–60”. Do not brand Agent Readiness as Vercel. Scorecard weights in `AuditCollector` are Speed 27% · Lead 22% · SEO 22% · Security 18% · AI Visibility 11%; L1 is reported, not weighted. The How-it-works categories must follow `ReportScopeOfWork` itemsCore (network, indexing, PSI, per-locale, AI Visibility, Agent Readiness L1, funnel crawl) — not a shortened marketing list.

**Prevent:** When auditor checks or tier unlocks change, update landing + order copy in the same change. Diff against `QA_Auditor/AGENTS.md`, `ReportScopeOfWork.java`, and `A44Tier.pageCap`.

