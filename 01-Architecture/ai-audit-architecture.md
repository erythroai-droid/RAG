# Архитектурный регламент: Сервис AI-Аудита (MVP)

См. проектную копию: `docs/architecture/ai-audit-architecture.md`.

### Управление в Payload Admin

| Элемент | Где |
|---|---|
| Тексты `/audit` (форма, How it works, Pricing cards) | Global **Pages → Audit Page** (`audit-page`) |
| Заказы `/order/audit-*` | Collection **Plans** с `kind: AI Audit` (sidebar **Audit Orders**) |
| Счётчики pipeline + re-queue | Dashboard + Contact Submissions / AI Audits |
| Staff re-queue API | `POST /api/audit/admin/requeue` |

Статический фолбэк: `src/lib/auditPage.ts`, `AUDIT_ORDER_PLANS` в `orderPlans.ts`. После seed CMS — источник правды.

Проверка адреса сайта на форме: DNS A/AAAA на публичный IP (`POST /api/audit/check-website`, `src/lib/checkWebsite.ts`). Не n8n и не HTTP-fetch пользовательского URL (PIT-041).
