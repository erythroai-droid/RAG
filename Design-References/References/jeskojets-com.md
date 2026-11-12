---
title: "Jesko Jets"
slug: "jeskojets-com"
type: website
source: "https://jeskojets.com/"
date: 2026-11-12
tags: ["light-mode", "bento", "grid", "tight-typography", "landing"]
stack: ["nextjs", "tailwind", "framer-motion", "lucide-react"]
mood: "Чистый светлый хай-тек, воздух, легкие полупрозрачные границы"
preview: "Assets/jeskojets-com-desktop.jpg"
mobilePreview: "Assets/jeskojets-com-mobile.jpg"
---

# 🎨 Референс: Jesko Jets

> **Vibe & Mood:** Чистый светлый хай-тек, воздух, легкие полупрозрачные границы  
> **Источник:** [https://jeskojets.com/](https://jeskojets.com/)  
> **Связи базы знаний:** [[foundations-colors|Цвета и Темы]] · [[foundations-typography|Типографика]] · [[foundations-geometry|Сетки и Отступы]] · [[foundations-animations|Анимации]]

---

## 📸 1. Визуальный слой (Visual Preview)

![[jeskojets-com-desktop.jpg]]

> **Мобильная версия:**
> ![[jeskojets-com-mobile.jpg]]

---

## 🎯 2. Дизайн-токены (Design Tokens)

### Цветовая палитра
| Роль | HEX | Назначение / Примечание |
| :--- | :--- | :--- |
| **Background Main** | `#FFF8ED` | Основная подложка страницы |
| **Surface / Card** | `#312726` | Карточки, bento-модули, всплывающие окна |
| **Text Primary** | `#312726` | Основные заголовки, максимальный контраст |
| **Text Muted** | `#FFFFFF` | Вторичный текст, подзаголовки, описания |
| **Border / Divider** | `#E4E4E7` | Границы элементов, тонкие разделители 1px |
| **Accent / Action** | `#2563EB` | Кнопки, бейджи, свечение, фокус |

> **Все обнаруженные оттенки:** `#FFF8ED` `#312726` `#FFFFFF` `#DBDBDB`

### Типографика
* **Заголовочный шрифт (Heading):** `GT America Extended`
* **Основной шрифт (Body):** `GT America Expanded`
* **Отрицательный кернинг (Letter Spacing):** `-0.319795px`
* **Шкала размеров:** `14.4px`, `24.0048px`, `47.9952px`, `63.9936px`, `72px`, `103.997px`
* **Начертания (Weights):** `500`, `700`

### Геометрия и Отступы
* **Скругления (Border Radius):** `12px` (вспомогательные: `8px, 16px, 9999px`)
* **Контейнеры (Max Width):** `1200px, 1440px`
* **Базовый шаг отступов (Paddings/Gaps):** `0px 95.9904px`
* **Тени и размытия:** `subtle dark glow / drop-shadow`

---

## 🧠 3. Семантический анализ (Analysis for AI Agents)

### Сетка и композиция (Grid & Composition)
Интерфейс построен на модульной **Bento-сетке (CSS Grid)**. Высокая плотность информации сбалансирована щедрыми вертикальными отступами (секционные паддинги 96–128px), что дает ощущение «дорогого воздуха» и премиального технологичного продукта. Основной фокус концентрируется по центру с плавным расширением до полноразмерных карточек.

### Визуальные приемы и акценты (Visual Accents & Techniques)
* **Микро-границы:** Тонкие контрастные бордеры `1px solid #E4E4E7` с эффектом матового стекла (`backdrop-blur-md`).
* **Амбиентное свечение:** Мягкие радиальные градиенты на фоне (`radial-gradient`), создающие глубину сцены без перегрузки визуала.
* **Иерархия акцентов:** Одиночный яркий акцент (`#2563EB`) зарезервирован строго под целевое действие (CTA) и интерактивные статусы.

### Характер движения и взаимодействия (Motion & Interactions)
* **Кнопки и ссылки:** Микро-масштабирование (`scale: 1.02`) и плавная смена прозрачности (`duration-200`).
* **Карточки:** При ховере граница плавно подсвечивается акцентным цветом с мягким смещением иконки.
* **Рекомендация для анимации:** Идеально подходит для реализации с помощью **GSAP ScrollTrigger** (плавный fade-up секций с каскадной задержкой `stagger: 0.1`, параллакс фонового свечения) или **Framer Motion** (`initial={{ opacity: 0, y: 20 }}`).

---

## 💻 4. Эталонный код верстки (Reference Snippet)

```tsx
import React from 'react';

export function JeskojetsComHero() {
  return (
    <section className="relative w-full min-h-[640px] bg-[#FFF8ED] text-[#312726] overflow-hidden px-6 py-24 md:py-32 flex flex-col items-center justify-center border-b border-[#E4E4E7]">
      {/* Background ambient glow / gradient */}
      <div 
        className="pointer-events-none absolute -top-40 left-1/2 -translate-x-1/2 w-[720px] h-[360px] opacity-25 blur-[120px] rounded-full"
        style={{ background: 'radial-gradient(circle, #2563EB 0%, transparent 70%)' }}
      />

      {/* Main Container */}
      <div className="relative z-10 max-w-[1200px] w-full mx-auto flex flex-col items-center text-center">
        {/* Badge / Pill */}
        <div className="inline-flex items-center gap-2 px-3.5 py-1.5 rounded-full border border-[#E4E4E7] bg-[#312726]/60 backdrop-blur-md mb-8 text-xs font-medium text-[#FFFFFF] tracking-wide shadow-sm">
          <span className="w-1.5 h-1.5 rounded-full bg-[#2563EB] animate-pulse" />
          <span>Стиль: Jesko Jets</span>
        </div>

        {/* Heading */}
        <h1 className="max-w-4xl text-4xl sm:text-6xl lg:text-7xl font-semibold tracking-[-0.319795px] leading-[1.08] mb-6 font-['GT America Extended']">
          Системный дизайн нового поколения
        </h1>

        {/* Subtitle */}
        <p className="max-w-2xl text-base sm:text-lg text-[#FFFFFF] font-normal leading-relaxed mb-10 font-['GT America Expanded']">
          Our aircraft are among the first to deliver clients to the most iconic international events. Among DS Jets’ clients are some of the world’s leading companies in...
        </p>

        {/* Action Buttons */}
        <div className="flex flex-wrap items-center justify-center gap-4">
          <button className="px-6 py-3 rounded-[12px] bg-[#2563EB] text-white font-medium text-sm transition-all duration-200 hover:opacity-90 hover:scale-[1.02] active:scale-[0.98] shadow-lg shadow-[#2563EB]/20">
            Начать работу
          </button>
          <button className="px-6 py-3 rounded-[12px] border border-[#E4E4E7] bg-[#312726]/80 text-[#312726] font-medium text-sm transition-all duration-200 hover:bg-[#312726] hover:border-[#FFFFFF]/40">
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
              className="p-6 rounded-[12px] border border-[#E4E4E7] bg-[#312726]/70 backdrop-blur-sm transition-all duration-300 hover:border-[#2563EB]/50 group"
            >
              <div className="w-8 h-8 rounded-lg bg-[#2563EB]/10 flex items-center justify-center text-[#2563EB] mb-4 group-hover:scale-110 transition-transform">
                0{idx + 1}
              </div>
              <h3 className="text-lg font-medium text-[#312726] mb-2 font-['GT America Extended']">{card.title}</h3>
              <p className="text-sm text-[#FFFFFF] font-['GT America Expanded']">{card.desc}</p>
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
При реализации интерфейса в стиле «Jesko Jets»:
1. Строго соблюдай палитру: фон #FFF8ED, карточки #312726, границы #E4E4E7.
2. Для заголовков используй шрифт 'GT America Extended' с отрицательным трекингом '-0.319795px'.
3. Скругление карточек держи в районе 12px.
4. Сохраняй простор между секциями (py-24 или py-32) и модульную Bento-композицию.
```
