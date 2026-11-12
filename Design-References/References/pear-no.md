---
title: "Pear NO"
slug: "pear-no"
type: website
source: "https://pear.no/"
date: 2026-11-12
tags: ["dark-mode", "bento", "grid", "rounded", "soft-shadows", "tight-typography", "landing", "minimal", "editorial", "typography", "webgl", "agency", "scrollytelling", "revenue-share"]
stack: ["react", "lightningcss", "webgl", "shaders"]
mood: "Editorial neoclassical minimalism, high-craft typography and scrollytelling"
preview: "Assets/pear-no/desktop.jpg"
mobilePreview: "Assets/pear-no/mobile.jpg"
---

# 🎨 Референс: Pear NO

> **Vibe & Mood:** Editorial neoclassical minimalism, high-craft typography and scrollytelling  
> **Источник:** [https://pear.no/](https://pear.no/)  
> **Связи базы знаний:** [[foundations-colors|Цвета и Темы]] · [[foundations-typography|Типографика]] · [[foundations-geometry|Сетки и Отступы]] · [[foundations-animations|Анимации]]

---

## 📸 1. Визуальный слой (Visual Preview)

### Десктопная версия: Главы скроллителлинга (Scrollytelling Chapters)

#### 01. Hero & Вводный экран («Pear makes you appear»)
![[Assets/pear-no/01-hero.jpg]]

#### 02. Ch. 1 The Model (Revenue Share, No Retainers)
![[Assets/pear-no/02-model.jpg]]

#### 03. Ch. 2 The Work — Search Engine Optimization
![[Assets/pear-no/03-work-seo.jpg]]

#### 04. Ch. 2 The Work — Custom Software
![[Assets/pear-no/04-work-software.jpg]]

#### 05. Ch. 3 The Terms (Фильтр партнеров: «We say no more often than yes»)
![[Assets/pear-no/05-terms.jpg]]

#### 06. Ch. 4 Questions (Интерактивный FAQ)
![[Assets/pear-no/06-questions-faq.jpg]]

#### 07. The Application & Footer (Форма заявки с кастомными инпутами)
![[Assets/pear-no/07-application-form.jpg]]

---

### Мобильная версия (Mobile Walkthrough, 390×844)

| 01. Hero | 02. The Model | 03. The Work |
| :---: | :---: | :---: |
| ![[Assets/pear-no/mobile-01-hero.jpg]] | ![[Assets/pear-no/mobile-02-model.jpg]] | ![[Assets/pear-no/mobile-03-work.jpg]] |

| 04. Terms | 05. FAQ | 06. Application |
| :---: | :---: | :---: |
| ![[Assets/pear-no/mobile-04-terms.jpg]] | ![[Assets/pear-no/mobile-05-faq.jpg]] | ![[Assets/pear-no/mobile-06-form.jpg]] |

---

## 🎯 2. Дизайн-токены (Design Tokens)

### Цветовая палитра
| Роль | HEX | Назначение / Примечание |
| :--- | :--- | :--- |
| **Background Main** | `#0B0A09` | Основная подложка страницы |
| **Surface / Card** | `#FFFFFF` | Карточки, bento-модули, всплывающие окна |
| **Text Primary** | `#FFFFFF` | Основные заголовки, максимальный контраст |
| **Text Muted** | `#000000` | Вторичный текст, подзаголовки, описания |
| **Border / Divider** | `#FFFFFF` | Границы элементов, тонкие разделители 1px |
| **Accent / Action** | `#2E200F` | Кнопки, бейджи, свечение, фокус |

> **Все обнаруженные оттенки:** `#0B0A09` `#FFFFFF` `#2E200F` `#F2F1ED` `#000000` `#FFFAEA` `#2E2109` `#1D1C19`

### Типографика
* **Заголовочный шрифт (Heading):** `GT Standard L`
* **Основной шрифт (Body):** `Flecha L`
* **Отрицательный кернинг (Letter Spacing):** `-2.80282px`
* **Шкала размеров:** `12.16px`, `14px`, `16px`, `20px`, `23.903px`, `26.57px`, `77.872px`, `116.784px`
* **Начертания (Weights):** `400`, `300`, `500`

### Геометрия и Отступы
* **Скругления (Border Radius):** `5.25px` (вспомогательные: `999px`, `22px`, `19.5px`, `4px`, `3px`)
* **Контейнеры (Max Width):** `1200px, 1440px`
* **Базовый шаг отступов (Paddings/Gaps):** `4.2512px 0px 5.8454px`, `1px`, `0px 0px 1px`, `10.9021px 0px 15.5744px`
* **Тени и размытия:** `rgba(6, 20, 46, 0.475) 0px 1px 6px 0px inset`<br>`rgba(255, 255, 255, 0.16) 0px 1px 0px 0px inset, rgba(0, 0, 0, 0.6) 0px -1px 0px 0px inset`

---

## 🧠 3. Семантический анализ (Analysis for AI Agents)

### Сетка и композиция (Grid & Composition)
Интерфейс построен на модульной **Bento-сетке (CSS Grid)**. Высокая плотность информации сбалансирована щедрыми вертикальными отступами (секционные паддинги 96–128px), что дает ощущение «дорогого воздуха» и премиального технологичного продукта. Основной фокус концентрируется по центру с плавным расширением до полноразмерных карточек.

### Визуальные приемы и акценты (Visual Accents & Techniques)
* **Микро-границы:** Тонкие контрастные бордеры `1px solid #FFFFFF` с эффектом матового стекла (`backdrop-blur-md`).
* **Амбиентное свечение:** Мягкие радиальные градиенты на фоне (`radial-gradient`), создающие глубину сцены без перегрузки визуала.
* **Иерархия акцентов:** Одиночный яркий акцент (`#2E200F`) зарезервирован строго под целевое действие (CTA) и интерактивные статусы.

### Характер движения и взаимодействия (Motion & Interactions)
* **Кнопки и ссылки:** Микро-масштабирование (`scale: 1.02`) и плавная смена прозрачности (`duration-200`).
* **Карточки:** При ховере граница плавно подсвечивается акцентным цветом с мягким смещением иконки.
* **Рекомендация для анимации:** Идеально подходит для реализации с помощью **GSAP ScrollTrigger** (плавный fade-up секций с каскадной задержкой `stagger: 0.1`, параллакс фонового свечения) или **Framer Motion** (`initial={{ opacity: 0, y: 20 }}`).

---

## 💻 4. Эталонный код верстки (Reference Snippet)

```tsx
import React from 'react';

export function PearNoHero() {
  return (
    <section className="relative w-full min-h-[640px] bg-[#0B0A09] text-[#FFFFFF] overflow-hidden px-6 py-24 md:py-32 flex flex-col items-center justify-center border-b border-[#FFFFFF]">
      {/* Background ambient glow / gradient */}
      <div 
        className="pointer-events-none absolute -top-40 left-1/2 -translate-x-1/2 w-[720px] h-[360px] opacity-25 blur-[120px] rounded-full"
        style={{ background: 'radial-gradient(circle, #2E200F 0%, transparent 70%)' }}
      />

      {/* Main Container */}
      <div className="relative z-10 max-w-[1200px] w-full mx-auto flex flex-col items-center text-center">
        {/* Badge / Pill */}
        <div className="inline-flex items-center gap-2 px-3.5 py-1.5 rounded-full border border-[#FFFFFF] bg-[#FFFFFF]/60 backdrop-blur-md mb-8 text-xs font-medium text-[#000000] tracking-wide shadow-sm">
          <span className="w-1.5 h-1.5 rounded-full bg-[#2E200F] animate-pulse" />
          <span>Стиль: Pear NO</span>
        </div>

        {/* Heading */}
        <h1 className="max-w-4xl text-4xl sm:text-6xl lg:text-7xl font-semibold tracking-[-2.80282px] leading-[1.08] mb-6 font-['GT Standard L']">
          Системный дизайн нового поколения
        </h1>

        {/* Subtitle */}
        <p className="max-w-2xl text-base sm:text-lg text-[#000000] font-normal leading-relaxed mb-10 font-['Flecha L']">
          Pear makes you appear. Data-driven SEO, tailored software and revenue-share partnerships. Grow your traffic and outrun your competitors....
        </p>

        {/* Action Buttons */}
        <div className="flex flex-wrap items-center justify-center gap-4">
          <button className="px-6 py-3 rounded-[5.25px] bg-[#2E200F] text-white font-medium text-sm transition-all duration-200 hover:opacity-90 hover:scale-[1.02] active:scale-[0.98] shadow-lg shadow-[#2E200F]/20">
            Начать работу
          </button>
          <button className="px-6 py-3 rounded-[5.25px] border border-[#FFFFFF] bg-[#FFFFFF]/80 text-[#FFFFFF] font-medium text-sm transition-all duration-200 hover:bg-[#FFFFFF] hover:border-[#000000]/40">
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
              className="p-6 rounded-[5.25px] border border-[#FFFFFF] bg-[#FFFFFF]/70 backdrop-blur-sm transition-all duration-300 hover:border-[#2E200F]/50 group"
            >
              <div className="w-8 h-8 rounded-lg bg-[#2E200F]/10 flex items-center justify-center text-[#2E200F] mb-4 group-hover:scale-110 transition-transform">
                0{idx + 1}
              </div>
              <h3 className="text-lg font-medium text-[#FFFFFF] mb-2 font-['GT Standard L']">{card.title}</h3>
              <p className="text-sm text-[#000000] font-['Flecha L']">{card.desc}</p>
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
При реализации интерфейса в стиле «Pear NO»:
1. Строго соблюдай палитру: фон #0B0A09, карточки #FFFFFF, границы #FFFFFF.
2. Для заголовков используй шрифт 'GT Standard L' с отрицательным трекингом '-2.80282px'.
3. Скругление карточек держи в районе 5.25px.
4. Сохраняй простор между секциями (py-24 или py-32) и модульную Bento-композицию.
```
