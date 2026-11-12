---
title: "Example Minimal Clean"
slug: "example-minimal-clean"
type: website
source: "https://example.com"
date: 2026-11-12
tags: ["dark-mode", "landing", "minimal", "clean", "light-mode", "typography"]
stack: ["nextjs", "tailwind", "framer-motion", "lucide-react"]
mood: "Премиальный темный минимализм, строгий контраст, неоновые микро-акценты"
preview: "Assets/example-minimal-clean/desktop.jpg"
mobilePreview: "Assets/example-minimal-clean/mobile.jpg"
---

# 🎨 Референс: Example Minimal Clean

> **Vibe & Mood:** Премиальный темный минимализм, строгий контраст, неоновые микро-акценты  
> **Источник:** [https://example.com](https://example.com)  
> **Связи базы знаний:** [[foundations-colors|Цвета и Темы]] · [[foundations-typography|Типографика]] · [[foundations-geometry|Сетки и Отступы]] · [[foundations-animations|Анимации]]

---

## 📸 1. Визуальный слой (Visual Preview)

![[Assets/example-minimal-clean/desktop.jpg]]

> **Мобильная версия:**
> ![[Assets/example-minimal-clean/mobile.jpg]]

---

## 🎯 2. Дизайн-токены (Design Tokens)

### Цветовая палитра
| Роль | HEX | Назначение / Примечание |
| :--- | :--- | :--- |
| **Background Main** | `#0A0A0A` | Основная подложка страницы |
| **Surface / Card** | `#141416` | Карточки, bento-модули, всплывающие окна |
| **Text Primary** | `#000000` | Основные заголовки, максимальный контраст |
| **Text Muted** | `#0000EE` | Вторичный текст, подзаголовки, описания |
| **Border / Divider** | `#27272A` | Границы элементов, тонкие разделители 1px |
| **Accent / Action** | `#3B82F6` | Кнопки, бейджи, свечение, фокус |

> **Все обнаруженные оттенки:** `#000000` `#0000EE`

### Типографика
* **Заголовочный шрифт (Heading):** `system-ui`
* **Основной шрифт (Body):** `system-ui`
* **Отрицательный кернинг (Letter Spacing):** `-0.03em`
* **Шкала размеров:** `14px`, `16px`
* **Начертания (Weights):** `400`

### Геометрия и Отступы
* **Скругления (Border Radius):** `12px` (вспомогательные: `8px, 16px, 9999px`)
* **Контейнеры (Max Width):** `1200px, 1440px`
* **Базовый шаг отступов (Paddings/Gaps):** `16px, 24px, 32px, 64px`
* **Тени и размытия:** `subtle dark glow / drop-shadow`

---

## 🧠 3. Семантический анализ (Analysis for AI Agents)

### Сетка и композиция (Grid & Composition)
Используется выверенная 12-колоночная сетка. Высокая плотность информации сбалансирована щедрыми вертикальными отступами (секционные паддинги 96–128px), что дает ощущение «дорогого воздуха» и премиального технологичного продукта. Основной фокус концентрируется по центру с плавным расширением до полноразмерных карточек.

### Визуальные приемы и акценты (Visual Accents & Techniques)
* **Микро-границы:** Тонкие контрастные бордеры `1px solid #27272A` с эффектом матового стекла (`backdrop-blur-md`).
* **Амбиентное свечение:** Мягкие радиальные градиенты на фоне (`radial-gradient`), создающие глубину сцены без перегрузки визуала.
* **Иерархия акцентов:** Одиночный яркий акцент (`#3B82F6`) зарезервирован строго под целевое действие (CTA) и интерактивные статусы.

### Характер движения и взаимодействия (Motion & Interactions)
* **Кнопки и ссылки:** Микро-масштабирование (`scale: 1.02`) и плавная смена прозрачности (`duration-200`).
* **Карточки:** При ховере граница плавно подсвечивается акцентным цветом с мягким смещением иконки.
* **Рекомендация для анимации:** Идеально подходит для реализации с помощью **GSAP ScrollTrigger** (плавный fade-up секций с каскадной задержкой `stagger: 0.1`, параллакс фонового свечения) или **Framer Motion** (`initial={{ opacity: 0, y: 20 }}`).

---

## 💻 4. Эталонный код верстки (Reference Snippet)

```tsx
import React from 'react';

export function ExampleMinimalCleanHero() {
  return (
    <section className="relative w-full min-h-[640px] bg-[#0A0A0A] text-[#000000] overflow-hidden px-6 py-24 md:py-32 flex flex-col items-center justify-center border-b border-[#27272A]">
      {/* Background ambient glow / gradient */}
      <div 
        className="pointer-events-none absolute -top-40 left-1/2 -translate-x-1/2 w-[720px] h-[360px] opacity-25 blur-[120px] rounded-full"
        style={{ background: 'radial-gradient(circle, #3B82F6 0%, transparent 70%)' }}
      />

      {/* Main Container */}
      <div className="relative z-10 max-w-[1200px] w-full mx-auto flex flex-col items-center text-center">
        {/* Badge / Pill */}
        <div className="inline-flex items-center gap-2 px-3.5 py-1.5 rounded-full border border-[#27272A] bg-[#141416]/60 backdrop-blur-md mb-8 text-xs font-medium text-[#0000EE] tracking-wide shadow-sm">
          <span className="w-1.5 h-1.5 rounded-full bg-[#3B82F6] animate-pulse" />
          <span>Стиль: Example Minimal Clean</span>
        </div>

        {/* Heading */}
        <h1 className="max-w-4xl text-4xl sm:text-6xl lg:text-7xl font-semibold tracking-[-0.03em] leading-[1.08] mb-6 font-['system-ui']">
          Системный дизайн нового поколения
        </h1>

        {/* Subtitle */}
        <p className="max-w-2xl text-base sm:text-lg text-[#0000EE] font-normal leading-relaxed mb-10 font-['system-ui']">
          Референс интерфейса Example Minimal Clean с разобранными токенами, сеткой и эталонной версткой....
        </p>

        {/* Action Buttons */}
        <div className="flex flex-wrap items-center justify-center gap-4">
          <button className="px-6 py-3 rounded-[12px] bg-[#3B82F6] text-white font-medium text-sm transition-all duration-200 hover:opacity-90 hover:scale-[1.02] active:scale-[0.98] shadow-lg shadow-[#3B82F6]/20">
            Начать работу
          </button>
          <button className="px-6 py-3 rounded-[12px] border border-[#27272A] bg-[#141416]/80 text-[#000000] font-medium text-sm transition-all duration-200 hover:bg-[#141416] hover:border-[#0000EE]/40">
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
              className="p-6 rounded-[12px] border border-[#27272A] bg-[#141416]/70 backdrop-blur-sm transition-all duration-300 hover:border-[#3B82F6]/50 group"
            >
              <div className="w-8 h-8 rounded-lg bg-[#3B82F6]/10 flex items-center justify-center text-[#3B82F6] mb-4 group-hover:scale-110 transition-transform">
                0{idx + 1}
              </div>
              <h3 className="text-lg font-medium text-[#000000] mb-2 font-['system-ui']">{card.title}</h3>
              <p className="text-sm text-[#0000EE] font-['system-ui']">{card.desc}</p>
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
При реализации интерфейса в стиле «Example Minimal Clean»:
1. Строго соблюдай палитру: фон #0A0A0A, карточки #141416, границы #27272A.
2. Для заголовков используй шрифт 'system-ui' с отрицательным трекингом '-0.03em'.
3. Скругление карточек держи в районе 12px.
4. Сохраняй простор между секциями (py-24 или py-32) и модульную Bento-композицию.
```
