# 🛒 Скилл: Agentic Commerce Protocol (ACP)

#agent-skills #acp #commerce #agentic-commerce #stripe #openai #checkout

> **Назначение:** Стандарт Agentic Commerce Protocol ([agenticcommerce.dev](https://agenticcommerce.dev)) для предоставления автономным торговым AI-агентам доступа к каталогу товаров, тарифам и чекауту.

---

## 1. Спецификация discovery-документа `/.well-known/acp.json`

AI-агенты (например, ChatGPT Commerce Agent) обращаются к этому файлу для автоматического определения возможностей мерчанта.

### Обязательные требования:
1. `protocol.name` = `"acp"` и `protocol.version`.
2. `api_base_url` — абсолютный HTTP(S) URL.
3. `transports` — непустой массив поддерживаемых транспортов (`["http", "rest", "mcp"]`).
4. `capabilities.services` — непустой массив доступных сервисов (`["catalog", "checkout", "orders", "delegate_payment"]`).

```json
{
  "protocol": {
    "name": "acp",
    "version": "2025-01",
    "supported_versions": ["2025-01", "1.0.0", "0.1"]
  },
  "api_base_url": "https://erythro.ai/api/acp",
  "transports": ["http", "rest", "mcp"],
  "capabilities": {
    "services": [
      "catalog",
      "checkout",
      "orders",
      "delegate_payment"
    ],
    "extensions": [
      "currency_conversion",
      "localized_content"
    ]
  },
  "merchant": {
    "name": "Erythro.ai",
    "url": "https://erythro.ai",
    "contact": {
      "email": "order@erythro.ai",
      "phone": "+972 50 530 83 05"
    }
  },
  "endpoints": {
    "catalog": "https://erythro.ai/order",
    "contact": "https://erythro.ai/contacts",
    "mcp": "https://erythro.ai/api/mcp"
  },
  "documentation_url": "https://erythro.ai/llms.txt"
}
```

---

## 2. Валидация через IsItAgentReady Scanner

Для проверки готовности сайта отправьте запрос:
```http
POST https://isitagentready.com/api/scan
Content-Type: application/json

{
  "url": "https://erythro.ai"
}
```

**Ожидаемый результат:** `checks.commerce.acp.status: "pass"`.
