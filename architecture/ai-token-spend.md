# AI token spend map (Erythro.ai)

Статус: **операционный канон**. Принципы из BRM *How to manage AI token spend* (2026) применены к стеку erythro.ai. Продукт BRM **не** обязателен: карта живёт в этом файле и в консолях провайдеров.

Связанные документы: [[gemini-consultant]] · [[ai-audit-architecture]] · [[deployment]] · [[PLAN-deferred]]

## 1. Насколько гайд корректен

**Корректно (брать как метод):**

- Токен — переменный unit-cost, не seat. Счёт приходит позже, чем usage.
- «Айсберг»: прямой счёт модели — только верхушка. Ниже — cloud, SaaS с внутренним meter, coding agents.
- Инвентаризация **по соглашению / ключу**, не по вендору. Один Google / Anthropic / Cursor = несколько коммерческих моделей сразу.
- COGS vs OPEX решает **чья нагрузка**, не логотип на инвойсе. Один инвойс часто оба.
- Прогноз: отдельно подписки и usage; COGS растёт с клиентами, OPEX — с adoption агентов; диапазон, а не прямая.
- Рычаги: routing дешёвой модели на простой объём, бюджеты/алерты **до** инвойса, cost-per-outcome (ретраи дешёвой модели могут быть дороже одного вызова нормальной).
- Нельзя «заморозить spend»: режутся и ROI-юзкейсы.

**Оговорки (не копировать слепо):**

- Цифры BRM (4.3% контрактов, 18.6× per-developer, цитата FinOps Foundation) — маркетинг вендора. Для Erythro считать **свои** консоли.
- «BRM starts at the agreement» — их продукт. Нам достаточно таблицы ниже + AI Studio / Cursor / Cloudflare billing.
- Cloudflare **Workers AI (Neurons)** у нас **нет**. Счёт Cloudflare — DNS/WAF/R2/Access/Turnstile, не токены модели.
- xAI / Grok / OpenRouter / Bedrock / Azure Foundry / Vertex **в проде консультанта не используются** (канон: Gemini Flash через AI Studio). Не тащить их в карту «на всякий случай».
- PageSpeed и LanguageTool — не LLM-токены. PSI — usage Google API; LanguageTool в QA_Auditor **локальный** JAR, не metered SaaS.

## 2. Consumption exposure map

Классификация: соглашение / ключ, а не бренд. Обновлять при новом ключе, модели или тарифе.

| Линия | Где живёт | Коммерческая модель | Драйвер | COGS / OPEX | Owner |
|---|---|---|---|---|---|
| Сайт-консультант `POST /api/consult` | Vercel → Gemini `GEMINI_API_KEY_AI_CHAT` | usage (токены in/out + cache storage) | посетители чата | **COGS** | сайт |
| Translater (бриф EN/HE) | VPS Docker → свой `GEMINI_API_KEY` | usage | кастомные ТЗ из чата | **COGS** | консультант |
| Funnel review (Pro audit) | QA_Auditor → `GEMINI_API_KEY` | usage, **только** `AUDIT_TIER=PRO` | оплаченные Pro-аудиты | **COGS** | audit-agent |
| Cursor / агенты в IDE | Cursor seats + usage credits | hybrid: seat + credits | разработчик, overnight agents | **OPEX (R&D)** | инженер |
| Gemini в Cursor / других IDE | часто тот же Google-аккаунт или отдельный ключ | usage | агенты в репо | **OPEX (R&D)** | инженер |
| Google AI Studio (общая квота) | console | usage + возможный free tier / billing account | **все** ключи проекта | split | finance + инженер |
| Vercel Functions (`maxDuration` consult 60s) | Vercel invoice | GB-s / invocations | чат, audit API | смесь: consult ≈ COGS, остальное OPEX | сайт |
| PageSpeed Insights | `PAGESPEED_API_KEY` | usage (не токены LLM) | каждый audit crawl | **COGS** на продаваемых аудитах | audit-agent |
| Cloudflare | CF dashboard | подписка + add-ons | трафик, R2, Access | **OPEX** (инфра), не AI-meter | инфра |
| n8n / SMTP / R2 / Supabase | свои счета | в основном подписка / storage | продукт | не token spend | инфра |

**Уже сделанное разделение ключей** — главное правило карты: публичный чат **не** должен есть квоту Funnel review. `GEMINI_API_KEY_AI_CHAT` ≠ аудиторский `GEMINI_API_KEY`. Локальный fallback на один ключ — только для dev.

## 3. Что в коде уже является spend-control

Не ждать BRM: рычаги из гайда частично уже в репо.

| Рычаг гайда | Как у нас |
|---|---|
| Model routing | Консультант и Funnel review пинятся на `gemini-3.6-flash`. Frontier не для FAQ. Override только `GEMINI_CONSULT_MODEL`. |
| Не гонять модель на бесплатном объёме | Gemini Funnel review **нет** на Free/Diagnostic. |
| Per-workload keys | Отдельный ключ чата vs аудита vs Translater. |
| Caps до инвойса | Turnstile + IP burst + anon quota + daily verified limit (`Consultant Settings`). Kill switch `enabled` прячет launcher (PIT-089). |
| Cost-per-outcome / prefix | Explicit Gemini prompt cache (`CONSULT_PROMPT_CACHE`, TTL 1 ч) — одинаковый system prompt не тарифицируется на каждое сообщение. |
| Не смешивать meter | Chat **не** наследует `AUDIT_INTAKE_LIMITS_OPEN_FOR_QA`. |

## 4. Ежемесячный ритуал (без нового SaaS)

1. **Google AI Studio / Cloud billing** — usage по **ключу** и модели. Три ряда: chat, audit, translater. Если один ключ на всё — это дыра в карте, чинить ключи, не таблицу.
2. **Cursor** — seats vs consumed premium/usage. Агенты overnight — главный кандидат на сюрприз OPEX (тезис гайда здесь самый сильный).
3. **Vercel** — всплеск Function duration на `/api/consult` и audit worker.
4. Любая линия **> порога** (например ±30% к прошлому месяцу) — кто owner, какой драйвер (трафик чата / Pro-аудиты / сессия агента).
5. Credits / free tier Google: сколько осталось до billing, **до** overage.
6. Новые ключи: grep по репо `API_KEY`, Vercel env, VPS `.env`. Карта в этом файле должна совпасть со списком.

Прогноз: не складывать Cursor seat и Gemini COGS в одну «цифру AI». Две кривые. Диапазон best / likely / worst на квартал: (а) чат выключен, (б) текущий run-rate, (в) чат включён + рост Pro-аудитов + интенсивные агенты.

## 5. Вопросы вендору при продлении

Короткий чеклист гайда, в формулировках Erythro:

- Это seat, usage, credits или гибрид? По **каждому** продукту (Cursor, Google, Vercel).
- Единица: token / request / neuron / GB-s?
- Что входит в committed, что overage, есть ли hard cap?
- Есть ли порог, после которого цена прыгает без роста usage (как Copilot metered в гайде)?
- Usage export **per key / per project**, не только итог.
- Алерты на их стороне (Google budget alert, Cursor spend limit).
- Счёт при 2× и 5× текущего объёма чата / Pro-аудитов.

## 6. Что не делать

- Не включать Funnel review на Free «чтобы отчёт был умнее» — это прямой рост COGS без цены.
- Не сливать `GEMINI_API_KEY_AI_CHAT` с аудиторским ключом в production.
- Не класть API keys в git (PIT-039). Spend-консоль ≠ секрет в репозитории.
- Не считать счёт Cloudflare «AI token spend».
- Не покупать spend-платформу, пока карта и месячный ритуал не ведутся вручную. Гайд это допускает: visibility step one.

## 7. База знаний

Этот файл — канон для агентов на вопросы «сколько стоит чат / какой ключ / COGS или Cursor». Цены пакетов клиенту — **не** отсюда, а из CMS / [[erythro-knowledge-base]].
