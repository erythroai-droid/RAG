# ИИ-консультант Erythro (xAI API)

Статус: **в работе**. Канон цен — live CMS на `erythro.ai`, снимок в `docs/consultant/erythro-knowledge-base.md` (2026-09-13).

Связанные документы: `RAG_INDEX.md` · `DEPLOYMENT.md` · `AI_VISIBILITY.md`

## 1. Решение

Публичный консультант на сайте вызывает `POST /api/consult`. Мозг — xAI API, ключ `XAI_API_KEY` только server-side (Vercel / `.env`). Браузер ключ не видит.

Grok Bot app **не** встраивается и этот ключ не использует. Remote MCP для Grok Bot — позже.

Цены и пакеты **не** брать из `src/translations` / `src/lib/servicePages.ts`: production CMS их перебивает. Консультант цитирует только knowledge-base файл (целиком в system prompt, без pgvector).

## 2. Live snapshot vs git fallback (2026-09-13)

- Free Start: ₪0 / ₪199/mo — совпадает со статикой
- AI Smart Card: ₪7 000 / ₪400/mo — совпадает со статикой
- Business Automation: прод ₪24 000 / ₪850/mo vs статика ₪14 999 / ₪650/mo
- Enterprise: Custom: прод from ₪72 000 / ₪2 500/mo vs статика from ₪45 000
- Audit Diagnostic: прод ₪149 vs статика ₪99
- Audit Pro: прод ₪890 vs статика ₪490

Третья услуга на главной — `/services/enterprise-engineering` (ILS). `/services/management` с USD — не цитировать.

## 3. Поток

```text
[Виджет ChatButton]
        │
        ▼ POST /api/consult  (Turnstile, IP limit, locale)
[Vercel Node route]
  system prompt = KB markdown + bot rules
  tools: get_knowledge_base, create_lead
        │
        ├── XAI_API_KEY → api.x.ai (grok-4.6)
        └── create_lead → тот же pipeline что POST /api/contact
              source=consultant → contact-submissions
```

Нет ключа → `503`, виджет ведёт на форму.

## 4. Env

- `XAI_API_KEY` — Vercel Production/Preview/Development, `.env`, server-only
- `XAI_CONSULT_MODEL` — опционально, дефолт `grok-4.6`

Запрещено: `NEXT_PUBLIC_XAI_*`, HTML виджета, профиль Grok Bot.

## 5. Комплаенс

- Пометка «это ИИ».
- PII в xAI минимизировать; лид через существующий contact pipeline.
- Не выдумывать цены вне KB.
