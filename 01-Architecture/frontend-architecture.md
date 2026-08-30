# 🎨 Фронтенд-архитектура и UI (Frontend Architecture)

#architecture #frontend #nextjs #react #tailwind #gsap #i18n #rtl

> **Назначение:** Архитектурный регламент для разработки клиентской части веб-приложений. Определяет стандарты чистой верстки, адаптивности, RTL-поддержки, анимаций GSAP и производительности (Core Web Vitals).

---

## 1. Технологический стек фронтенда

- **Фреймворк:** Next.js 15+ (App Router, Server & Client Components).
- **Библиотека:** React 19 (Hooks, Suspense, Transitions).
- **Язык:** TypeScript 5+ (Strict Mode).
- **Стилизация:** Tailwind CSS 4 / 3.4 (с семантическими токенами и логическими свойствами).
- **Анимации:** GSAP 3 (ScrollTrigger, Flip, Observer) + CSS transitions.
- **Шрифты:** `next/font/google` (Inter, Inter Cyrillic, Heebo для иврита, Roboto Mono для данных).
- **Иконки:** SVG Components с поддержкой `currentColor` и `rtl:rotate-180`.

---

## 2. Разделение Server и Client компонентов

Для достижения максимального показателя скорости (PageSpeed 95+):
- **Server Components (по умолчанию):** Сборка данных из CMS (`getCachedSiteContent`, `getServicePageBySlug`), генерация метатегов (`generateMetadata`), серверный рендеринг структуры.
- **Client Components (`'use client'`):** Только интерактивные модули (переключатель тем, языковой селектор, модальные окна, GSAP-анимации, слайдеры, формы валидации).

```
src/app/(frontend)/services/[slug]/
├── page.tsx            # Server Component: загрузка данных, generateMetadata
└── ServiceClient.tsx   # Client Component: интерактивность, табы, аккордеоны
```

---

## 3. Два фундаментальных закона локализации

### Закон 1: Стресс-тест длины текста (+40%)
1. Тексты на русском языке в среднем на 30–40% длиннее английских оригиналов.
2. **КАТЕГОРИЧЕСКИ ЗАПРЕЩЕНО** задавать фиксированную ширину (`w-[200px]`, `w-48`) для кнопок, табов, заголовков и карточек.
3. **ОБЯЗАТЕЛЬНО** использовать эластичные контейнеры: `w-full`, `max-w-*`, `flex-1`, `gap-*`, `min-w-0`.

### Закон 2: Логические свойства Tailwind (RTL-Ready)
Для полной поддержки иврита (`dir="rtl"`) интерфейс должен автоматически зеркалироваться без дублирования стилей.

| Запрещено (направленные) | Обязательно (логические) | Назначение |
|---|---|---|
| `pl-*`, `pr-*` | `ps-*`, `pe-*` | Padding Start / End |
| `ml-*`, `mr-*` | `ms-*`, `me-*` | Margin Start / End |
| `left-*`, `right-*` | `start-*`, `end-*` | Позиционирование Absolute/Fixed |
| `text-left`, `text-right` | `text-start`, `text-end` | Выравнивание текста |
| `border-l-*`, `border-r-*` | `border-s-*`, `border-e-*` | Границы Start / End |
| `rounded-l-*`, `rounded-r-*` | `rounded-s-*`, `rounded-e-*` | Радиусы скругления |

> **Правило иконок:** Все направленные стрелки (например, стрелка перехода `➔`) обязаны иметь класс `rtl:rotate-180`.

---

## 4. Шрифтовая матрица и оптимизация LCP

Подключение шрифтов настраивается через `src/lib/fonts.ts`:

```typescript
import { Inter, Heebo, Roboto_Mono } from 'next/font/google'

export const inter = Inter({
  subsets: ['latin'],
  variable: '--font-inter',
  display: 'swap',
})

export const interCyrillic = Inter({
  subsets: ['cyrillic'],
  variable: '--font-inter-cyrillic',
  display: 'swap',
})

export const heebo = Heebo({
  subsets: ['hebrew'],
  variable: '--font-heebo',
  display: 'swap',
})

export const robotoMono = Roboto_Mono({
  subsets: ['latin'],
  variable: '--font-roboto-mono',
  display: 'swap',
})
```

В корневом `layout.tsx`:
```tsx
<html
  lang={locale}
  dir={locale === 'he' ? 'rtl' : 'ltr'}
  className={`${inter.variable} ${locale === 'ru' ? interCyrillic.variable : ''} ${locale === 'he' ? heebo.variable : ''} ${robotoMono.variable}`}
>
```

---

## 5. LCP & Hero Image Preloading

Для устранения задержек при загрузке Hero-изображений используется декларативный `<link rel="preload">` с `fetchpriority="high"` и `imageSrcSet`:

```tsx
// В page.tsx (Server Component)
{mobileHero && (
  <>
    <link
      rel="preload"
      as="image"
      imageSrcSet={heroImageSrcSet(mobileHero, 80)}
      imageSizes="(max-width: 1023px) 100vw, 1200px"
      fetchPriority="high"
      media="(max-width: 1023px)"
    />
    <link
      rel="preload"
      as="image"
      imageSrcSet={heroImageSrcSet(desktopHero, 80)}
      imageSizes="(min-width: 1024px) 100vw, 1920px"
      fetchPriority="high"
      media="(min-width: 1024px)"
    />
  </>
)}
```

---

## 6. Анимации GSAP и ScrollTrigger

1. Все ScrollTrigger-триггеры регистрируются внутри хуков `useLayoutEffect` или `useEffect` с обязательной очисткой в `return () => ctx.revert()`.
2. Запрещено использовать `window.scrollTo` без проверки наличия объекта `window`.
3. Для плавной прокрутки используется специализированный помощник `src/lib/ctaNav.ts`.

---

## 7. Фронтенд-чеклист качества

- [x] **Отсутствие Layout Shift (CLS < 0.05):** Все медиа и видео имеют фиксированные пропорции `aspect-ratio` или размеры `width`/`height`.
- [x] **Идеальный RTL:** При переключении на иврит ни один элемент не вылезает за пределы экрана, отступы зеркальны.
- [x] **Плавность переходов:** Интерактивные элементы имеют `transition-all duration-300` или `transition-colors duration-200`.
- [x] **Доступность (A11y / IS 5568):** Наличие `SkipToContent`, `aria-label` на кнопках-иконках, поддержка клавиатурной навигации (`focus-visible:outline-2`).
