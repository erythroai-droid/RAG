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

### Именование PDF (клиентское имя файла)

Скачивание / email-вложение / `Content-Disposition` в R2:

```text
Erythro_Audit_[AUD-N]_[site]_[YYYY-MM-DD]_[LANG].pdf
```

| Часть | Правило | Пример |
|---|---|---|
| Префикс | фиксированный | `Erythro_Audit` |
| Номер аудита | `AUD-{id}` | `AUD-110` |
| Сайт | hostname без протокола; точки и прочие символы → `_` | `example_com` |
| Дата | UTC `YYYY-MM-DD` (момент генерации) | `2026-09-10` |
| Язык | `RU` / `EN` / `HE` | `RU` |

Пример: `Erythro_Audit_AUD-110_example_com_2026-09-10_RU.pdf`

Реализация: `services/audit-agent/src/pdfFilename.js` (`buildAuditPdfFilename`).

### Мобильный HTML-отчёт

На `max-width: 720px` A4-4 HTML складывается по Figma **iPhone 17 - 1** (402): лого (тёмный) → title (красный) → tier strip `#f3f3f3` / чёрный текст → URL (красный) → meta `#f3f3f3` → gauge на `#f3f3f3` с красным кругом балла → scorecard (подписи в 2 строки: EN/RU `108rem`, HE `58rem`, полосы `1fr`) → секции → footer. На мобайле в футере только WhatsApp (email/телефон скрыты), `cta-title` 18rem. Подписи шкалы EN/RU/HE с явным `<br>` (на десктопе скрыт). CTA разблокировки Free/Diagnostic → `https://erythro.ai/order/audit-pro`.

Код: `MOBILE_SCREEN_CSS` в `A44ReportGenerator.java` (после rem→A4). PDF не меняется. Старые `htmlResult` — только после нового прогона.
