# 📑 Шаблон: Кейс портфолио (Portfolio Case Template)

#templates #portfolio #case-study #yaml #schema

> **Назначение:** Унифицированный шаблон создания и валидации коммерческих кейсов для портфолио агентства.

---

## 1. Структура `brief.yaml` для нового кейса

```yaml
slug: "project-slug-name"
client: "Client Brand Name"
date: "2025"
category: "ai-automation" # design-branding | development | management | enterprise
link: "https://project-url.com"

tags:
  - "Tag 1"
  - "Tag 2"
  - "Tag 3"

stack:
  - "Next.js 15"
  - "TypeScript"
  - "Tailwind CSS"
  - "PostgreSQL"
  - "OpenAI / Claude API"

title:
  en: "English Case Study Title"
  ru: "Название кейса на русском языке"
  he: "כותרת הפרויקט בעברית"

subtitle:
  en: "Short catchy one-line outcome or metric"
  ru: "Краткий результат или метрика в одну строку"
  he: "שורת תוצאה או מדד מרכזי"

summary:
  en: "Comprehensive 2-3 sentence executive summary of the project."
  ru: "Полноценное резюме проекта в 2-3 предложениях."
  he: "תקציר מנהלים מקיף של הפרויקט ב-2-3 משפטים."

description:
  en: "Detailed narrative describing context, business background, and initial state."
  ru: "Подробное описание контекста, бизнес-задачи и исходного состояния."
  he: "תיאור מפורט של הרקע העסקי והמצב ההתחלתי."

body:
  - heading:
      en: "01. The Problem & Business Context"
      ru: "01. Проблема и бизнес-контекст"
      he: "01. הבעיה וההקשר העסקי"
    paragraphs:
      - en: "Description of the manual bottlenecks and challenges."
        ru: "Описание ручных узких мест и сложностей клиента."
        he: "תיאור צווארי הבקבוק והאתגרים של הלקוח."
    images:
      - "01_gallery/initial-state.webp"

  - heading:
      en: "02. Engineering & AI Architecture"
      ru: "02. Инженерная и AI-архитектура"
      he: "02. ארכיטקטורת פיתוח ו-AI"
    paragraphs:
      - en: "How the solution was designed and implemented."
        ru: "Как было спроектировано и внедрено решение."
        he: "כיצד תוכנן ויושם הפתרון."
    images:
      - "01_gallery/architecture.webp"

  - heading:
      en: "03. Business Results & Metrics"
      ru: "03. Бизнес-результаты и цифры"
      he: "03. תוצאות עסקיות ומדדים"
    paragraphs:
      - en: "Concrete ROI, time saved, and scaling achievements."
        ru: "Конкретный ROI, сэкономленные часы и масштабирование."
        he: "החזר השקעה (ROI), חיסכון בזמן והישגי צמיחה."

seo:
  title:
    en: "Project Name — Case Study | Erythro.ai"
    ru: "Название проекта — Кейс | Erythro.ai"
    he: "שם הפרויקט — מקרה בוחן | Erythro.ai"
  description:
    en: "How Erythro.ai delivered high-impact AI and web architecture for Client."
    ru: "Как Erythro.ai разработала эффективную архитектуру для Клиента."
    he: "כיצד Erythro.ai פיתחה מערכת מתקדמת עבור הלקוח."
```

---

> **Related:** Markdown-шаблон из репо — [[case-study-template]]; field map — [[cms-field-map]].
