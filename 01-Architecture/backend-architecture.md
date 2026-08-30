# ⚙️ Бэкенд-архитектура и CMS (Backend Architecture)

#architecture #backend #payload-cms #postgres #drizzle #nextjs #n8n

> **Назначение:** Архитектурный регламент проектирования серверной логики, реляционных баз данных, CMS-коллекций и API-интеграций в экосистеме Next.js + Payload CMS.

---

## 1. Технологический стек бэкенда

- **Headless CMS:** Payload CMS 3.0+ (Self-hosted внутри Next.js App Router).
- **База данных:** PostgreSQL 16+ (Supabase / Neon / AWS RDS).
- **ORM / DB Adapter:** `@payloadcms/db-postgres` (на базе Drizzle ORM).
- **Хранилище медиа:** `@payloadcms/storage-vercel-blob` (Vercel Blob Storage) + прямой доступ через CDN.
- **Текстовый редактор:** `@payloadcms/richtext-lexical` (JSON-структурированный AST).
- **Agentic API & Автоматизация:** n8n Webhooks, REST API, GraphQL, On-Demand Revalidation.
- **Кэширование:** Next.js `unstable_cache` с тегами (`SITE_CONTENT_TAG`) для мгновенной инвалидации.

---

## 2. Конфигурация Payload CMS (`payload.config.ts`)

Подключение к базе данных осуществляется строго через Drizzle/Postgres adapter. Использование MongoDB запрещено.

```typescript
import { postgresAdapter } from '@payloadcms/db-postgres'
import { lexicalEditor } from '@payloadcms/richtext-lexical'
import { vercelBlobStorage } from '@payloadcms/storage-vercel-blob'
import { buildConfig } from 'payload'
import path from 'path'
import { fileURLToPath } from 'url'

import { Users } from './collections/Users'
import { Pages } from './collections/Pages'
import { Services } from './collections/Services'
import { SolutionPlans } from './collections/SolutionPlans'
import { PortfolioProjects } from './collections/PortfolioProjects'
import { PortfolioCategories } from './collections/PortfolioCategories'
import { Media } from './collections/Media'
import { Partners } from './collections/Partners'
import { ContactSubmissions } from './collections/ContactSubmissions'

const filename = fileURLToPath(import.meta.url)
const dirname = path.dirname(filename)

export default buildConfig({
  admin: {
    user: Users.slug,
    importMap: {
      baseDir: path.resolve(dirname),
    },
  },
  db: postgresAdapter({
    pool: {
      connectionString: process.env.DATABASE_URL || '',
    },
    // В CI и тестах push отключается во избежание интерактивных зависаний
    push: process.env.PAYLOAD_DISABLE_PUSH !== '1' && process.env.NODE_ENV !== 'test',
  }),
  editor: lexicalEditor({}),
  collections: [
    Users,
    SolutionPlans,
    Services,
    Pages,
    PortfolioProjects,
    PortfolioCategories,
    Media,
    Partners,
    ContactSubmissions,
  ],
  localization: {
    locales: [
      { label: 'English', code: 'en' },
      { label: 'Русский', code: 'ru' },
      { label: 'עברית', code: 'he', rtl: true },
    ],
    defaultLocale: 'en',
    fallback: true,
  },
  plugins: [
    vercelBlobStorage({
      enabled: Boolean(process.env.BLOB_READ_WRITE_TOKEN),
      collections: {
        media: {
          // Критично для воспроизведения видео (HTTP 206 Range)
          disablePayloadAccessControl: true,
        },
      },
      token: process.env.BLOB_READ_WRITE_TOKEN,
    }),
  ],
  typescript: {
    outputFile: path.resolve(dirname, 'payload-types.ts'),
  },
})
```

---

## 3. Матрица локализации (Field-level Localization)

Каждое текстовое поле в CMS, предназначенное для отображения пользователю или агенту, настраивается с параметром `localized: true`. Это генерирует отдельные таблицы локалей в PostgreSQL (например, `services_locales`, `portfolio_projects_locales`).

### Правила локализации полей:
1. **Тексты, заголовки, описания:** `localized: true`.
2. **Числа, цены, валюты:** `localized: false` (единый источник правды) либо локализованный `priceDisplay` только при необходимости текстового префикса/суффикса.
3. **Слаги (`slug`):** `localized: false`, `unique: true`, `index: true` (URL-структура одинакова для всех языков, переключение происходит через cookie `NEXT_LOCALE` или заголовок `Accept-Language`).
4. **Медиа-связи:** `localized: false` для общих изображений; при наличии специфичных для языка баннеров создаются отдельные поля (например, `bannerRu`, `bannerEn`).

---

## 4. Архитектура основных коллекций

### 4.1. `Services` (Услуги)
- `title` (Text, `localized: true`, `required: true`)
- `slug` (Text, `required: true`, `unique: true`)
- `summary` (Textarea / Lexical, `localized: true`)
- `description` (Array / Lexical, `localized: true`)
- `features` (Array of objects with `feature: { localized: true }`)
- `offerings` (Array: `name`, `description`, `price`, `pricePrefix`)
- `image` / `heroMedia` (Upload: `media`)
- `order` (Number, `index: true` для сортировки)

### 4.2. `PortfolioProjects` (Кейсы портфолио)
- `title` (Text, `localized: true`)
- `slug` (Text, `unique: true`, `index: true`)
- `category` (Relationship to `portfolio-categories` or Text)
- `summary` (Text / Lexical, `localized: true`)
- `subtitle` (Text / Lexical, `localized: true`)
- `description` (Text, `localized: true`)
- `stack` (Array of items: `item` string)
- `tags` (Array of tags: `tag` string)
- `client` (Text), `date` (Text, e.g. "2025")
- `link` (URL string)
- `cardImage`, `heroMedia`, `heroMediaMobile` (Upload: `media`)
- `body` (Array of Sections: `heading` localized, `paragraphs` array, `images` array of uploads)
- `seo` (`title`, `description` localized)

### 4.3. `SolutionPlans` (Тарифные планы и готовые решения)
- `title` (Text, `localized: true`)
- `slug` (Text, `unique: true`)
- `price` (Text/Number), `currency` (Select: `ILS`, `USD`, `EUR`)
- `pricePrefix` (Text, `localized: true`)
- `features` (Array: `label`, `value`, `full` Lexical, `home_only` boolean)
- `periods` (Array: `periodId`, `label`, `months`, `discountPercent`)
- `addons` (Array: `addonId`, `name`, `description`, `priceDisplay`, `discountMonths1`, `discountMonths6`, `discountMonths12`, `recommended`, `mandatory`, `full`)

---

## 5. Lexical RichText — Сериализация и Конвертация

В Next.js клиенту передаются как чистый текст (для SSR/SEO/RAG), так и AST (для интерактивного рендеринга).

```typescript
// src/lib/lexical.ts
export function isLexicalDoc(value: unknown): boolean {
  return Boolean(
    value &&
      typeof value === 'object' &&
      'root' in value &&
      typeof (value as { root?: unknown }).root === 'object'
  )
}

export function lexicalToPlain(doc: unknown): string {
  if (!isLexicalDoc(doc)) return typeof doc === 'string' ? doc : ''
  const root = (doc as { root: { children?: unknown[] } }).root
  return extractText(root.children).trim()
}

function extractText(nodes?: unknown[]): string {
  if (!Array.isArray(nodes)) return ''
  return nodes
    .map((node) => {
      if (node && typeof node === 'object') {
        if ('text' in node && typeof node.text === 'string') return node.text
        if ('children' in node && Array.isArray(node.children)) {
          return extractText(node.children)
        }
      }
      return ''
    })
    .join(' ')
}
```

---

## 6. On-Demand Revalidation & Инвалидация кэша

При редактировании данных в админке или импорте через n8n/скрипты, сервер мгновенно сбрасывает кэш через теги `SITE_CONTENT_TAG`:

```typescript
// src/app/api/revalidate/route.ts
import { NextRequest, NextResponse } from 'next/server'
import { revalidateTag, revalidatePath } from 'next/cache'
import { SITE_CONTENT_TAG } from '@/lib/revalidate'

export async function POST(request: NextRequest) {
  const secret = request.nextUrl.searchParams.get('secret')

  if (secret !== process.env.REVALIDATION_TOKEN) {
    return NextResponse.json({ message: 'Invalid token' }, { status: 401 })
  }

  try {
    revalidateTag(SITE_CONTENT_TAG)
    revalidatePath('/', 'layout')
    return NextResponse.json({ revalidated: true, now: Date.now() })
  } catch (err) {
    return NextResponse.json({ message: 'Revalidation error', err }, { status: 500 })
  }
}
```

---

## 7. Чеклист предотвращения сбоев (Backend Best Practices)

- [x] **Импорт-карта:** После изменения плагинов/компонентов всегда запускать `pnpm generate:importmap` ([[pitfalls-troubleshooting#PIT-001|PIT-001]]).
- [x] **Видео-стриминг:** Всегда ставить `disablePayloadAccessControl: true` для коллекции `media` ([[pitfalls-troubleshooting#PIT-003|PIT-003]]).
- [x] **Миграции:** Любое изменение схемы сопровождается файлом миграции в `src/migrations/` и идемпотентным скриптом исправления ([[pitfalls-troubleshooting#PIT-004|PIT-004]]).
- [x] **Защита от спама:** Honeypot + Rate Limit по IP на эндпоинтах контактов и лид-форм.
- [x] **Обработка сбоев БД:** Все функции чтения (`getCachedSiteContent`) содержат fallback на `defaultContent.ts`, чтобы сайт не падал при недоступности базы.
