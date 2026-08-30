# 🔌 Скилл: Model Context Protocol (MCP / SEP-1960)

#agent-skills #mcp #model-context-protocol #discovery #tools #api

> **Назначение:** Регламент интеграции и публикации манифеста Model Context Protocol для подключения AI-ассистентов (Claude Desktop, Cursor, Antigravity) к данным и инструментам проекта.

---

## 1. Спецификация манифеста `/.well-known/mcp`

Манифест публикуется в формате JSON со статусом HTTP 200:

```json
{
  "protocol": "mcp",
  "version": "2024-11-05",
  "server": {
    "name": "erythro-ai-brand-mcp",
    "version": "1.0.0",
    "description": "Official Erythro.ai MCP server for company profile, service catalog, and capabilities"
  },
  "endpoints": {
    "facts": "https://erythro.ai/api/mcp",
    "tools": "https://erythro.ai/api/mcp",
    "resources": "https://erythro.ai/api/mcp"
  },
  "documentation_url": "https://erythro.ai/llms.txt"
}
```

---

## 2. Реализация API эндпоинта (`/api/mcp`)

Эндпоинт отдает канонические данные компании:

```typescript
// src/app/api/mcp/route.ts
import { NextResponse } from 'next/server'
import { getCachedSiteContent } from '@/lib/getSiteContent'

export const dynamic = 'force-static'

export async function GET() {
  const content = await getCachedSiteContent('en')

  return NextResponse.json({
    name: 'Erythro.ai',
    url: 'https://erythro.ai',
    description: content.metaDescription,
    location: 'Eilat, Israel',
    email: 'order@erythro.ai',
    phone: '+972 50 530 83 05',
    services: content.services.items.map((s) => s.title),
    sameAs: [
      'https://facebook.com/erythro.ai',
      'https://t.me/erythroai',
    ],
    canonicalPages: {
      about: 'https://erythro.ai/about',
      contacts: 'https://erythro.ai/contacts',
      portfolio: 'https://erythro.ai/portfolio',
      services: 'https://erythro.ai/services',
    },
    correctionContact: 'order@erythro.ai',
  }, {
    headers: {
      'Content-Type': 'application/json; charset=utf-8',
      'Cache-Control': 'public, max-age=3600, s-maxage=86400',
    },
  })
}
```

---

## 3. Чеклист валидации MCP

- [x] Эндпоинт `/.well-known/mcp` отдает статус 200 и валидный JSON.
- [x] Эндпоинт `/api/mcp` возвращает актуальный список услуг и контактов.
- [x] Ссылка на манифест добавлена в `/.well-known/api-catalog` (RFC 9264 linkset) и в `/llms.txt`.
