# Agent Readiness Level 1 (Vercel / contentsignals.org)

Сбор в репозитории `website-auditor/erythro-ai` (`QA_Auditor`): объект `ai_visibility.vercel_level1` в `reports/audit_data.json`.

## Зачем в том же аудиторе

Пять HTTP-проверок на том же origin, что и блок AI Visibility (robots.txt, sitemap, Accept: text/markdown). Отдельный агент дублировал бы парсер robots и контур PDF/MD.

## Вес scorecard

Коммерческая шкала `ai_visibility` остаётся на **7 критериях**. L1 markdown / Content-Signal / sitemap для агентов **не входят** в эти 7 и **не меняют** формулу overall (`ai_visibility * 0.11`).

Отдельное число: `vercel_level1.score` и `executive_scorecard.agent_readiness_l1`.

## Пять checks

| ID | Pass |
|---|---|
| `robots_txt` | 200, не HTML, User-agent + Allow/Disallow, у `*` нет глухого `Disallow: /` |
| `sitemap` | директива `Sitemap: https://…` + XML + ≥1 `<loc>` |
| `ai_crawler_rules` | нет `severe_block` и валидный robots |
| `content_signals` | ≥1 директива `Content-Signal:` |
| `markdown_negotiation` | `Accept: text/markdown` → markdown; HTML-контроль |

Источник в репо аудитора: `docs/qa-auditor/vercel-agent-readiness.md`.
