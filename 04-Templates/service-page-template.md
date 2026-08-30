# 📑 Шаблон: Страница услуги (Service Page Template)

#templates #services #landing #offerings #pricing

> **Назначение:** Шаблон структуры и контента для страниц услуг агентства (`/services/[slug]`).

---

## 1. Схема данных страницы услуги

```typescript
export interface ServicePageData {
  slug: string
  title: LocalizedString
  metaTitle?: LocalizedString
  metaDescription?: LocalizedString
  heroTitle?: LocalizedString
  heroSubtitle?: LocalizedString
  summary?: LocalizedString
  description?: LocalizedString | LexicalDoc
  featuresTitle?: LocalizedString
  features?: Array<{ feature: LocalizedString }>
  offerings?: Array<{
    name: LocalizedString
    description: LocalizedString
    price?: number
    pricePrefix?: LocalizedString
  }>
  heroMedia?: { url: string; alt?: string }
}
```

---

## 2. Структура контентных блоков

1. **Hero Section:**
   - Заголовок H1 с акцентным тегом (например, `01 / DESIGN & BRANDING`).
   - Подзаголовок и краткое описание (Summary).
   - Кнопки: «Рассчитать проект» (CTA модалка) и «Смотреть кейсы» (скролл к портфолио).
2. **What We Do (Ключевые возможности):**
   - Список 4–6 ключевых фичей с акцентными иконками.
3. **Offerings & Packages (Пакеты и цены):**
   - Таблица или карточки с четким описанием состава работ и стартовой стоимостью.
4. **Relevant Case Studies (Связанные кейсы):**
   - Автоматическая фильтрация проектов из портфолио по тегу категории.
5. **FAQ Block (Вопросы и ответы):**
   - 4–5 частых вопросов с разметкой `FAQPage` JSON-LD.
6. **Final CTA:**
   - Форма быстрого старта с предзаполненным типом услуги.
