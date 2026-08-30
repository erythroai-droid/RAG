# 📑 Шаблон: CMS Field Map и Brief YAML (CMS Field Map Template)

#templates #cms #brief #yaml #portfolio #mapping

> **Назначение:** Полная матрица соответствия полей между файлом `brief.yaml` и коллекциями Payload CMS / PostgreSQL.

---

## 1. Матрица маппинга полей

| Поле в `brief.yaml` | Тип в Payload CMS | Колонка в PostgreSQL | Локализация | Обязательность |
|---|---|---|---|---|
| `slug` | Text | `slug` | Нет (`unique: true`) | Обязательно |
| `client` | Text | `client` | Нет | Опционально |
| `date` | Text | `date` | Нет | Опционально |
| `category` | Relationship / Text | `category_id` | Нет | Обязательно |
| `link` | Text | `link` | Нет | Опционально |
| `title` | Text | `title` в `_locales` | **Да** (EN/RU/HE) | Обязательно |
| `subtitle` | Text | `subtitle` в `_locales` | **Да** (EN/RU/HE) | Опционально |
| `summary` | Text / Lexical | `summary` в `_locales` | **Да** (EN/RU/HE) | Обязательно |
| `description` | Text / Lexical | `description` в `_locales` | **Да** (EN/RU/HE) | Опционально |
| `stack` | Array of strings | Таблица `_stack` | Нет | Обязательно |
| `tags` | Array of strings | Таблица `_tags` | Нет | Обязательно |
| `cardImage` | Upload (`media`) | `card_image_id` | Нет | Обязательно |
| `heroMedia` | Upload (`media`) | `hero_media_id` | Нет | Обязательно |
| `heroMediaMobile` | Upload (`media`) | `hero_media_mobile_id` | Нет | Опционально |
| `body` | Array of blocks | Таблица `_body` | **Да** (EN/RU/HE) | Опционально |
| `seo.title` | Text | `seo_title` в `_locales` | **Да** (EN/RU/HE) | Опционально |
| `seo.description` | Text | `seo_description` в `_locales` | **Да** (EN/RU/HE) | Опционально |

---

## 2. Эталонный файл `brief.yaml`

```yaml
slug: "fintech-payment-portal"
client: "Apex Financial Solutions"
date: "2025"
category: "development" # ai-automation | design-branding | development | management | enterprise
link: "https://apex-demo.erythro.ai"

tags:
  - "Fintech"
  - "PCI DSS"
  - "Next.js 15"
  - "PostgreSQL"

stack:
  - "Next.js (App Router)"
  - "React 19"
  - "Tailwind CSS"
  - "Payload CMS 3.0"
  - "Stripe ACP"

title:
  en: "Next-Gen Fintech Portal & Secure Checkout"
  ru: "Финтех-портал нового поколения и защищенный чекаут"
  he: "פורטל פינטק מתקדם וסליקה מאובטחת"

subtitle:
  en: "Reduced transaction drop-off by 42% with sub-second checkout latency."
  ru: "Снижение оттока на чекауте на 42% благодаря ускорению загрузки до 0.4с."
  he: "הפחתת נטישת לקוחות ב-42% בעזרת סליקה מהירה במיוחד."

summary:
  en: "A comprehensive digital banking and payment platform engineered with strict PCI-DSS and WCAG 2.1 AA accessibility compliance."
  ru: "Полнофункциональная банковская и платежная платформа, разработанная в строгом соответствии со стандартами PCI-DSS и доступности WCAG 2.1 AA."
  he: "פלטפורמת בנקאות ותשלומים דיגיטלית מלאה בהתאמה לתקני PCI-DSS ונגישות WCAG 2.1 AA."

body:
  - heading:
      en: "01. Architecture & Performance"
      ru: "01. Архитектура и скорость"
      he: "01. ארכיטקטורה וביצועים"
    paragraphs:
      - en: "Engineered on serverless Vercel infrastructure with Supabase connection pooler."
        ru: "Развернуто на бессерверной инфраструктуре Vercel с пулером соединений Supabase."
        he: "נבנה על תשתית Vercel Serverless עם חיבור מותאם ל-Supabase."
    images:
      - "01_gallery/architecture-diagram.webp"
```
