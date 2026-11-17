---
title: "Searchable"
slug: "searchable"
type: website
source: "https://www.searchable.com/"
date: 2026-11-17
tags: ["light-mode", "bento", "grid", "rounded", "soft-shadows", "tight-typography", "landing"]
stack: ["nextjs", "tailwind", "framer-motion", "lucide-react"]
mood: "Чистый светлый хай-тек, воздух, легкие полупрозрачные границы"
preview: "Assets/Assets/searchable/desktop.jpg"
mobilePreview: "Assets/Assets/searchable/mobile.jpg"
---

# 🎨 Референс: Searchable

> **Vibe & Mood:** Чистый светлый хай-тек, воздух, легкие полупрозрачные границы  
> **Источник:** [https://www.searchable.com/](https://www.searchable.com/)  
> **Связи базы знаний:** [[foundations-colors|Цвета и Темы]] · [[foundations-typography|Типографика]] · [[foundations-geometry|Сетки и Отступы]] · [[foundations-animations|Анимации]]

---

## 📸 1. Визуальный слой (Visual Preview)

![[Assets/searchable/desktop.jpg]]

> **Мобильная версия:**
> ![[Assets/searchable/mobile.jpg]]

---

## 🎯 2. Дизайн-токены (Design Tokens)

### Цветовая палитра
| Роль | HEX | Назначение / Примечание |
| :--- | :--- | :--- |
| **Background Main** | `#FDFDFD` | Основная подложка страницы |
| **Surface / Card** | `#FFFFFF` | Карточки, bento-модули, всплывающие окна |
| **Text Primary** | `#292524` | Основные заголовки, максимальный контраст |
| **Text Muted** | `#57534E` | Вторичный текст, подзаголовки, описания |
| **Border / Divider** | `#E7E5E4` | Границы элементов, тонкие разделители 1px |
| **Accent / Action** | `#FF4D00` | Кнопки, бейджи, свечение, фокус |

> **Все обнаруженные оттенки:** `#FDFDFD` `#FFFFFF` `#FF4D00` `#000000` `#31B77A` `#B2F5EE` `#292524` `#57534E` `#2D6E68` `#6E6E6E`

### Типографика
* **Заголовочный шрифт (Heading):** `Geist`
* **Основной шрифт (Body):** `Geist`
* **Отрицательный кернинг (Letter Spacing):** `-0.64px`
* **Шкала размеров:** `12px`, `12.5px`, `13px`, `14px`, `16px`, `20px`, `28px`, `48px`
* **Начертания (Weights):** `400`, `500`, `600`, `450`

### Геометрия и Отступы
* **Скругления (Border Radius):** `8px` (вспомогательные: `6px`, `3.35544e+07px`, `3px`, `14px`, `50%`)
* **Контейнеры (Max Width):** `1400px`, `896px`, `672px`, `1320px`
* **Базовый шаг отступов (Paddings/Gaps):** `10px 16px`, `0px 32px`, `16px 0px 15px`, `0px 12px`
* **Тени и размытия:** `rgba(255, 255, 255, 0.16) 0px 1px 0px 0px inset, rgba(0, 0, 0, 0.08) 0px -1px 0px 0px inset, rgba(0, 0, 0, 0.12) 0px 1px 2px 0px`<br>`rgba(0, 0, 0, 0.16) 0px 4px 18px 0px`<br>`rgb(255, 255, 255) 0px 1px 0px 0px inset, rgba(0, 0, 0, 0.03) 0px 1px 1px 0px`

---

## 🧠 3. Семантический анализ (Analysis for AI Agents)

### Сетка и композиция (Grid & Composition)
Интерфейс построен на модульной **Bento-сетке (CSS Grid)**. Высокая плотность информации сбалансирована щедрыми вертикальными отступами (секционные паддинги 96–128px), что дает ощущение «дорогого воздуха» и премиального технологичного продукта. Основной фокус концентрируется по центру с плавным расширением до полноразмерных карточек.

### Визуальные приемы и акценты (Visual Accents & Techniques)
* **Микро-границы:** Тонкие контрастные бордеры `1px solid #E7E5E4` с эффектом матового стекла (`backdrop-blur-md`).
* **Амбиентное свечение:** Мягкие радиальные градиенты на фоне (`radial-gradient`), создающие глубину сцены без перегрузки визуала.
* **Иерархия акцентов:** Одиночный яркий акцент (`#FF4D00`) зарезервирован строго под целевое действие (CTA) и интерактивные статусы.

### Характер движения и взаимодействия (Motion & Interactions)
* **Кнопки и ссылки:** Микро-масштабирование (`scale: 1.02`) и плавная смена прозрачности (`duration-200`).
* **Карточки:** При ховере граница плавно подсвечивается акцентным цветом с мягким смещением иконки.
* **Рекомендация для анимации:** Идеально подходит для реализации с помощью **GSAP ScrollTrigger** (плавный fade-up секций с каскадной задержкой `stagger: 0.1`, параллакс фонового свечения) или **Framer Motion** (`initial={{ opacity: 0, y: 20 }}`).

---

## 💻 4. Эталонный код верстки (Reference Snippet)

```tsx
import React from 'react';

export function SearchableHero() {
  return (
    <section className="relative w-full min-h-[640px] bg-[#FDFDFD] text-[#292524] overflow-hidden px-6 py-24 md:py-32 flex flex-col items-center justify-center border-b border-[#E7E5E4]">
      {/* Background ambient glow / gradient */}
      <div 
        className="pointer-events-none absolute -top-40 left-1/2 -translate-x-1/2 w-[720px] h-[360px] opacity-25 blur-[120px] rounded-full"
        style={{ background: 'radial-gradient(circle, #FF4D00 0%, transparent 70%)' }}
      />

      {/* Main Container */}
      <div className="relative z-10 max-w-[1200px] w-full mx-auto flex flex-col items-center text-center">
        {/* Badge / Pill */}
        <div className="inline-flex items-center gap-2 px-3.5 py-1.5 rounded-full border border-[#E7E5E4] bg-[#FFFFFF]/60 backdrop-blur-md mb-8 text-xs font-medium text-[#57534E] tracking-wide shadow-sm">
          <span className="w-1.5 h-1.5 rounded-full bg-[#FF4D00] animate-pulse" />
          <span>Стиль: Searchable</span>
        </div>

        {/* Heading */}
        <h1 className="max-w-4xl text-4xl sm:text-6xl lg:text-7xl font-semibold tracking-[-0.64px] leading-[1.08] mb-6 font-['Geist']">
          Системный дизайн нового поколения
        </h1>

        {/* Subtitle */}
        <p className="max-w-2xl text-base sm:text-lg text-[#57534E] font-normal leading-relaxed mb-10 font-['Geist']">
          Track and optimize your brand visibility across ChatGPT, Claude, Perplexity, and all major AI search engines. Get actionable insights to drive growth....
        </p>

        {/* Action Buttons */}
        <div className="flex flex-wrap items-center justify-center gap-4">
          <button className="px-6 py-3 rounded-[8px] bg-[#FF4D00] text-white font-medium text-sm transition-all duration-200 hover:opacity-90 hover:scale-[1.02] active:scale-[0.98] shadow-lg shadow-[#FF4D00]/20">
            Начать работу
          </button>
          <button className="px-6 py-3 rounded-[8px] border border-[#E7E5E4] bg-[#FFFFFF]/80 text-[#292524] font-medium text-sm transition-all duration-200 hover:bg-[#FFFFFF] hover:border-[#57534E]/40">
            Документация
          </button>
        </div>

        {/* Bento Grid Preview Cards */}
        <div className="grid grid-cols-1 md:grid-cols-3 gap-5 mt-16 w-full text-left">
          {[
            { title: "Точная сетка", desc: "12-колоночная система с выверенными отступами" },
            { title: "Высокая плотность", desc: "Контрастная типографика и микро-акценты" },
            { title: "Плавная физика", desc: "Адаптировано для GSAP ScrollTrigger" }
          ].map((card, idx) => (
            <div 
              key={idx}
              className="p-6 rounded-[8px] border border-[#E7E5E4] bg-[#FFFFFF]/70 backdrop-blur-sm transition-all duration-300 hover:border-[#FF4D00]/50 group"
            >
              <div className="w-8 h-8 rounded-lg bg-[#FF4D00]/10 flex items-center justify-center text-[#FF4D00] mb-4 group-hover:scale-110 transition-transform">
                0{idx + 1}
              </div>
              <h3 className="text-lg font-medium text-[#292524] mb-2 font-['Geist']">{card.title}</h3>
              <p className="text-sm text-[#57534E] font-['Geist']">{card.desc}</p>
            </div>
          ))}
        </div>
      </div>
    </section>
  );
}
```

---

## 🤖 Инструкция для AI-агентов (Prompt Ingestion)

```text
При реализации интерфейса в стиле «Searchable»:
1. Строго соблюдай палитру: фон #FDFDFD, карточки #FFFFFF, границы #E7E5E4.
2. Для заголовков используй шрифт 'Geist' с отрицательным трекингом '-0.64px'.
3. Скругление карточек держи в районе 8px.
4. Сохраняй простор между секциями (py-24 или py-32) и модульную Bento-композицию.
```
