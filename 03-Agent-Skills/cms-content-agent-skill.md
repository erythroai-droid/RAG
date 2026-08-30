# 📦 Скилл: Автономный CMS Контент-Агент (CMS Content Agent)

#agent-skills #cms #n8n #import #automation #payload-cms #sharp #lexical

> **Назначение:** Регламент автономного наполнения и обновления контента CMS через агентские пайплайны n8n и CLI-скрипты на базе YAML/JSON брифов.

---

## 1. Пайплайн пакетного импорта кейсов (`scripts/import-project`)

Агент принимает локальную папку с медиафайлами и файлом `brief.yaml`, автоматически конвертирует изображения в WebP, загружает их в медиа-хранилище и создает проект в Payload CMS.

```
case-folder/
├── brief.yaml          # Структурированное описание кейса на 3 языках
├── card.png            # Превью-обложка (будет конвертирована в WebP)
├── hero-desktop.png    # Десктопный баннер
├── hero-mobile.png     # Мобильный вертикальный баннер
└── 01_gallery/         # Изображения для галереи и текстовых блоков
```

---

## 2. Спецификация файла `brief.yaml`

```yaml
slug: "smart-crm-automation"
client: "Global Logistics Ltd"
date: "2025"
category: "ai-automation"
link: "https://example.com"
tags:
  - "AI Agents"
  - "n8n"
  - "PostgreSQL"
stack:
  - "Next.js"
  - "Payload CMS"
  - "OpenAI API"

title:
  en: "AI-Powered CRM Automation Platform"
  ru: "Платформа автоматизации CRM на базе ИИ"
  he: "פלטפורמת אוטומציה של CRM מבוססת AI"

summary:
  en: "End-to-end automation of customer requests with 70% cost reduction."
  ru: "Сквозная автоматизация обработки заявок с сокращением расходов на 70%."
  he: "אוטומציה מקצה לקצה של פניות לקוחות עם חיסכון של 70% בעלויות."

body:
  - heading:
      en: "The Challenge"
      ru: "Задача и контекст"
      he: "האתגר"
    paragraphs:
      - en: "The client faced high manual processing times..."
        ru: "Клиент столкнулся с высокой задержкой ручной обработки..."
        he: "הלקוח התמודד עם זמני עיבוד ידניים ארוכים..."
    images:
      - "01_gallery/architecture-diagram.png"
```

---

## 3. n8n Agentic API Integration

Для добавления или редактирования контента через чат-боты Telegram/Slack агент n8n выполняет:

1. **Авторизация:** Заголовок `Authorization: PayloadAPIKey <token>`.
2. **Запрос на создание:** `POST https://erythro.ai/api/portfolio-projects`.
3. **Сброс кэша:** `POST https://erythro.ai/api/revalidate?secret=<REVALIDATION_TOKEN>&path=/portfolio`.
