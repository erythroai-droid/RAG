# Архитектурный регламент: Сервис AI-Аудита (MVP)

См. каноническую копию: [[01-Architecture/ai-audit-architecture]].

### Управление в Payload Admin

Отдельная коллекция `audits` не нужна. Ops-интерфейс поверх `contact-submissions`:

| Элемент | Где |
|---|---|
| Счётчики New / In progress / Failed / Report sent | Dashboard (`AuditsDashboard`) |
| Пункт **AI Audits** в сайдбаре | `afterNavLinks` → фильтр `source=audit` |
| Re-queue, ссылки на status page / report | UI-поле `auditActions` + list cell |
| Staff API | `POST /api/audit/admin/requeue` (сессия Payload, не `AGENT_SECRET`) |

Ручной re-queue: `auditStatus → new`, очистка `errorLast`, сброс `retryCount`, вызов `triggerAuditAgent`.

Проверка адреса сайта на форме: DNS на публичный IP (`POST /api/audit/check-website`). Не n8n (PIT-041).
