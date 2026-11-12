---
title: "Mustang 6663 RT"
slug: "mustang-6663rt"
type: website
source: "https://mustang-ashy.vercel.app/"
date: 2026-11-12
tags: ["dark-mode", "bento", "grid", "rounded", "soft-shadows", "landing", "automotive", "dark-brutalist", "scroll-driven", "gsap", "frames", "luxury", "specs", "canvas", "nextjs15"]
stack: ["nextjs15", "react19", "gsap", "scrolltrigger", "typescript", "tailwind-css"]
mood: "Dark brutalist luxury automotive landing with scroll-scrubbed canvas sheet lift, telemetric cards, dynamic finish re-grading, and AI consultant"
preview: "Assets/Assets/mustang-6663rt/desktop.jpg"
mobilePreview: "Assets/Assets/mustang-6663rt/mobile.jpg"
---

# 🎨 Референс: Mustang 6663 RT

> **Vibe & Mood:** Dark brutalist luxury automotive landing with scroll-scrubbed canvas sheet lift, telemetric cards, dynamic finish re-grading, and AI consultant  
> **Источник:** [https://mustang-ashy.vercel.app/](https://mustang-ashy.vercel.app/)  
> **Связи базы знаний:** [[foundations-colors|Цвета и Темы]] · [[foundations-typography|Типографика]] · [[foundations-geometry|Сетки и Отступы]] · [[foundations-animations|Анимации]]

---

## 📸 1. Визуальный слой (Visual Preview)

![[Assets/mustang-6663rt/desktop.jpg]]

> **Мобильная версия:**
> ![[Assets/mustang-6663rt/mobile.jpg]]

---

## 🎯 2. Дизайн-токены (Design Tokens)

### Цветовая палитра
| Роль | HEX | Назначение / Примечание |
| :--- | :--- | :--- |
| **Background Main** | `#050505` | Основная подложка страницы |
| **Surface / Card** | `#111111` | Карточки, bento-модули, всплывающие окна |
| **Text Primary** | `#F2EFE9` | Основные заголовки, максимальный контраст |
| **Text Muted** | `#D4A24C` | Вторичный текст, подзаголовки, описания |
| **Border / Divider** | `#FFFFFF` | Границы элементов, тонкие разделители 1px |
| **Accent / Action** | `#E2391D` | Кнопки, бейджи, свечение, фокус |

> **Все обнаруженные оттенки:** `#050505` `#111111` `#000000` `#0A0A0A` `#F2EFE9` `#E2391D` `#0B0B0B` `#FFFFFF` `#D4A24C`

### Типографика
* **Заголовочный шрифт (Heading):** `Inter`
* **Основной шрифт (Body):** `Anton`
* **Отрицательный кернинг (Letter Spacing):** `5.4px`
* **Шкала размеров:** `15px`, `16px`, `17.28px`, `18px`, `34.56px`, `158.4px`, `164.736px`, `244.8px`
* **Начертания (Weights):** `400`, `600`, `500`, `300`, `700`

### Геометрия и Отступы
* **Скругления (Border Radius):** `3px` (вспомогательные: `40px`, `50%`, `23px`, `2px`)
* **Контейнеры (Max Width):** `1440px`, `430px`, `420px`, `1100px`
* **Базовый шаг отступов (Paddings/Gaps):** `20px 60.48px`, `6px 2px`, `11px 22px`, `0px 18px`
* **Тени и размытия:** `rgb(226, 57, 29) 0px 0px 12px 0px`

---

## 🧠 3. Семантический анализ (Analysis for AI Agents)

### Сетка и композиция (Grid & Composition)
Интерфейс построен на модульной **Bento-сетке (CSS Grid)**. Высокая плотность информации сбалансирована щедрыми вертикальными отступами (секционные паддинги 96–128px), что дает ощущение «дорогого воздуха» и премиального технологичного продукта. Основной фокус концентрируется по центру с плавным расширением до полноразмерных карточек.

### Визуальные приемы и акценты (Visual Accents & Techniques)
* **Микро-границы:** Тонкие контрастные бордеры `1px solid #FFFFFF` с эффектом матового стекла (`backdrop-blur-md`).
* **Амбиентное свечение:** Мягкие радиальные градиенты на фоне (`radial-gradient`), создающие глубину сцены без перегрузки визуала.
* **Иерархия акцентов:** Одиночный яркий акцент (`#E2391D`) зарезервирован строго под целевое действие (CTA) и интерактивные статусы.

### Характер движения и взаимодействия (Motion & Interactions)
* **Кнопки и ссылки:** Микро-масштабирование (`scale: 1.02`) и плавная смена прозрачности (`duration-200`).
* **Карточки:** При ховере граница плавно подсвечивается акцентным цветом с мягким смещением иконки.
* **Рекомендация для анимации:** Идеально подходит для реализации с помощью **GSAP ScrollTrigger** (плавный fade-up секций с каскадной задержкой `stagger: 0.1`, параллакс фонового свечения) или **Framer Motion** (`initial={{ opacity: 0, y: 20 }}`).

---

## 💻 4. Эталонный код верстки (Reference Snippet)

```tsx
import React from 'react';

export function Mustang6663rtHero() {
  return (
    <section className="relative w-full min-h-[640px] bg-[#050505] text-[#F2EFE9] overflow-hidden px-6 py-24 md:py-32 flex flex-col items-center justify-center border-b border-[#FFFFFF]">
      {/* Background ambient glow / gradient */}
      <div 
        className="pointer-events-none absolute -top-40 left-1/2 -translate-x-1/2 w-[720px] h-[360px] opacity-25 blur-[120px] rounded-full"
        style={{ background: 'radial-gradient(circle, #E2391D 0%, transparent 70%)' }}
      />

      {/* Main Container */}
      <div className="relative z-10 max-w-[1200px] w-full mx-auto flex flex-col items-center text-center">
        {/* Badge / Pill */}
        <div className="inline-flex items-center gap-2 px-3.5 py-1.5 rounded-full border border-[#FFFFFF] bg-[#111111]/60 backdrop-blur-md mb-8 text-xs font-medium text-[#D4A24C] tracking-wide shadow-sm">
          <span className="w-1.5 h-1.5 rounded-full bg-[#E2391D] animate-pulse" />
          <span>Стиль: Mustang 6663 RT</span>
        </div>

        {/* Heading */}
        <h1 className="max-w-4xl text-4xl sm:text-6xl lg:text-7xl font-semibold tracking-[5.4px] leading-[1.08] mb-6 font-['Inter']">
          Системный дизайн нового поколения
        </h1>

        {/* Subtitle */}
        <p className="max-w-2xl text-base sm:text-lg text-[#D4A24C] font-normal leading-relaxed mb-10 font-['Anton']">
          MUSTANG 6663 RT: a hand-built matte black fastback with gold-leaf stripes. Sixty-six cars, then the tooling burns....
        </p>

        {/* Action Buttons */}
        <div className="flex flex-wrap items-center justify-center gap-4">
          <button className="px-6 py-3 rounded-[3px] bg-[#E2391D] text-white font-medium text-sm transition-all duration-200 hover:opacity-90 hover:scale-[1.02] active:scale-[0.98] shadow-lg shadow-[#E2391D]/20">
            Начать работу
          </button>
          <button className="px-6 py-3 rounded-[3px] border border-[#FFFFFF] bg-[#111111]/80 text-[#F2EFE9] font-medium text-sm transition-all duration-200 hover:bg-[#111111] hover:border-[#D4A24C]/40">
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
              className="p-6 rounded-[3px] border border-[#FFFFFF] bg-[#111111]/70 backdrop-blur-sm transition-all duration-300 hover:border-[#E2391D]/50 group"
            >
              <div className="w-8 h-8 rounded-lg bg-[#E2391D]/10 flex items-center justify-center text-[#E2391D] mb-4 group-hover:scale-110 transition-transform">
                0{idx + 1}
              </div>
              <h3 className="text-lg font-medium text-[#F2EFE9] mb-2 font-['Inter']">{card.title}</h3>
              <p className="text-sm text-[#D4A24C] font-['Anton']">{card.desc}</p>
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
При реализации интерфейса в стиле «Mustang 6663 RT»:
1. Строго соблюдай палитру: фон #050505, карточки #111111, границы #FFFFFF.
2. Для заголовков используй шрифт 'Inter' с отрицательным трекингом '5.4px'.
3. Скругление карточек держи в районе 3px.
4. Сохраняй простор между секциями (py-24 или py-32) и модульную Bento-композицию.
```
