---
title: "Jesko Jets"
slug: "jeskojets"
type: website
source: "https://jeskojets.com/"
date: 2026-11-12
tags: ["luxury", "aviation", "private-jets", "editorial", "extended-grotesque", "split-typography", "champagne-cream", "espresso-dark", "lenis", "gsap", "telemetry-clock", "scrollytelling", "3d-globe", "landing"]
stack: ["webflow", "gsap", "scrolltrigger", "lenis", "splittext", "slater", "threejs-globe"]
mood: "Private aviation quiet luxury, Swiss extended typography, warm champagne & deep roasted espresso"
preview: "Assets/jeskojets/desktop.jpg"
mobilePreview: "Assets/jeskojets/mobile.jpg"
---

# ✈️ Референс: Jesko Jets

> **Vibe & Mood:** Премиальная приватная авиация, швейцарская расширенная типографика (GT America Extended), теплая палитра цвета шампанского (`#FFF8ED`) и глубокого жареного эспрессо (`#312726`), микро-телеметрия с живым временем и кинематографичный скроллителлинг.  
> **Источник:** [https://jeskojets.com/](https://jeskojets.com/)  
> **Связи базы знаний:** [[foundations-colors|Цвета и Темы]] · [[foundations-typography|Типографика]] · [[foundations-geometry|Сетки и Отступы]] · [[foundations-animations|Анимации и GSAP]]

---

## 📸 1. Визуальный слой (Visual Preview)

### Десктопная версия: Главы интерфейса (Desktop Chapters)

#### 01. Hero & Вводный экран («We are movement · We are distinction»)
Симметрично-асимметричная типографика, иллюминатор бизнес-джета со шторкой-прелоадером и деликатный призыв к скроллу.
![[Assets/jeskojets/01-hero.jpg]]

#### 02. Виджет телеметрии и Живое время (Live Dubai Telemetry)
Минималистичная статусная плашка: поддерживаемые страны (174), база операций (Dubai, UAE) и живые цифровые часы по местному времени хаба.
![[Assets/jeskojets/02-stats-time.jpg]]

#### 03. Интерактивный аккордеон преимуществ (Benefits & Services)
Кастомный аккордеон с параллаксом и зумом фотографий (Pets on board, 24/7 Availability, Onboard Services, Efficiency).
![[Assets/jeskojets/03-benefits-services.jpg]]

#### 04. 3D Интерактивный глобус и маршрутная сеть («Fly anywhere»)
Картографический модуль планирования трансконтинентальных полетов с динамической отрисовкой ортодромий (дуг полета).
![[Assets/jeskojets/04-globe-destinations.jpg]]

#### 05. Флот джетов (Fleet Showcase)
Каталог воздушных судов: сверхдальние (Ultra Long Range), тяжелые и среднемагистральные джеты с тактико-техническими характеристиками.
![[Assets/jeskojets/05-fleet-showcase.jpg]]

#### 06. Консьерж-форма и Футер (Bespoke Inquiry & Footer)
Премиальная минималистичная форма бронирования с тонкими волосяными разделителями и контрастной типографикой.
![[Assets/jeskojets/06-inquiry-footer.jpg]]

---

### Мобильная версия (Mobile Walkthrough, 390×844)

| 01. Hero | 02. Stats & Time | 03. Benefits |
| :---: | :---: | :---: |
| ![[Assets/jeskojets/mobile-01-hero.jpg]] | ![[Assets/jeskojets/mobile-02-stats.jpg]] | ![[Assets/jeskojets/mobile-03-benefits.jpg]] |

| 04. Globe & Destinations | 05. Fleet & Footer | Полный экран |
| :---: | :---: | :---: |
| ![[Assets/jeskojets/mobile-04-globe.jpg]] | ![[Assets/jeskojets/mobile-05-footer.jpg]] | ![[Assets/jeskojets/mobile.jpg]] |

---

## 🎯 2. Дизайн-токены (Design Tokens)

### Цветовая палитра
Палитра построена на концепции «Quiet Luxury» — отказ от холодного стерильного белого `#FFFFFF` в пользу теплого кремового оттенка дорогой бумаги/шампанского `#FFF8ED`, сбалансированного благородным темным оттенком обжаренного кофейного зерна `#312726`.

| Роль | HEX / RGBA | Назначение / Примечание |
| :--- | :--- | :--- |
| **Background Main** | `#FFF8ED` | Теплый кремовый фон (Champagne Cream / Warm Linen) |
| **Contrast Surface** | `#312726` | Глубокий кофейно-бронзовый оверлей / карточки / секции `bg="color"` |
| **Text Primary (Light BG)** | `#312726` | Основной текст на кремовом фоне с идеальной контрастностью |
| **Text Primary (Dark BG)** | `#FFF8ED` / `#FFFFFF` | Заголовки и маркеры на темных секциях |
| **Text Muted** | `#7A7372` / `#A19E9B` | Вторичный текст, подписи параметров, метки телеметрии |
| **Hairline Dividers** | `#DBDBDB` / `rgba(49, 39, 38, 0.12)` | Тонкие 1px горизонтальные и вертикальные разделители |
| **Dark Hairline** | `rgba(255, 248, 237, 0.14)` | Тонкие 1px разделители внутри темных секций |
| **Accent Glow** | `rgba(49, 39, 38, 0.06)` | Мягкие амбиентные тени без резких краев |

> **Все ключевые оттенки:** `#FFF8ED` `#312726` `#FFFFFF` `#DBDBDB` `#7A7372` `#1A1514`

### Типографика (Typography System)
Используется расширенный неогротеск **GT America** (Extended & Expanded) от швейцарского словолитного дома Grilli Type. За счет широких пропорций букв заголовки приобретают монументальность, премиальность и аэродинамический характер.

* **Заголовочный шрифт (Heading):** `GT America Extended`
* **Основной шрифт (Body & UI):** `GT America Expanded` / `GT America Standard`
* **Кернинг (Letter Spacing):**
  - Отрицательный трекинг для крупных заголовков: `-0.02em` (`-0.32px` до `-1.5px`)
  - Положительный разрядный трекинг для плашек и меток (Uppercase Labels): `+0.08em` (`+1.2px`)
* **Иерархия размеров:**
  - `Display / Hero Title:` `72px` – `104px` (Line-height: `1.02`, Weight: `500` / `Medium`)
  - `H2 Section Title:` `48px` – `64px` (Line-height: `1.08`)
  - `H3 Feature Title:` `24px` – `32px`
  - `Body / Descriptions:` `14px` – `16px` (Line-height: `1.55`)
  - `Telemetry / Data Numbers:` `24px` – `48px` (Monospace / Tabular styling)
  - `Micro Labels / Badges:` `11px` – `12px` (Uppercase, Tracking-wide)

### Геометрия, Сетка и Скругления
* **Модульная сетка:** 12-колоночная адаптивная сетка (`_6-columns` на средних и `grid fill` на десктопе).
* **Скругления (Border Radius):** 
  - Карточки и модальные окна: `0px` или строгие микро-скругления `2px` – `4px` (архитектурный брутализм).
  - Кнопки-таблетки и чипсы меню: `9999px` (полный радиус / pill shape).
* **Отступы и Воздух:**
  - Вертикальные паддинги секций: `96px`, `156px`, `216px` (`unit-96`, `unit-156`, `unit-216`).
  - Горизонтальные поля контейнера: `24px` (мобильные), `48px` – `96px` (десктоп).
* **Волосяные линии (Hairline Borders):** Разделители строго `1px` без псевдо-теней (`border-t border-[#DBDBDB]`).

---

## 🧠 3. Семантический анализ (Analysis for AI Agents)

### Сетка и композиция (Grid & Composition)
1. **Диагональный баланс заголовков (Split Hero Header):**
   Вместо классического центрированного или левостороннего H1, Jesko Jets разделяет месседж на два противоположных угла экрана:
   - Левый блок: `We are movement` (`hero-s_title-l`)
   - Правый блок: `We are distinction` (`hero-s_title-r`, `text-align: right`)
   Это создает ощущение пространственного полета и кинематографической перспективы.
2. **Виджет цифровой телеметрии (Status Bar Telemetry):**
   Прямо под Hero расположен информационный блок операционной готовности: количество доступных стран (174), база флота (Dubai, UAE) и **живые цифровые часы хаба**. Это мощнейший психологический триггер доверия и статуса: клиент видит, что компания бодрствует и управляет рейсами прямо сейчас.
3. **Чередование световых объемов (Light / Dark Rhythm):**
   Страница чередует светлые воздушные секции на бежевом фоне (`bg="light"`) и глубокие монохромные капсулы цвета эспрессо (`bg="color"`). Такой ритм сбрасывает усталость глаз и драматически акцентирует медиа-контент.

### Визуальные приемы и акценты (Visual Accents & Techniques)
* **Прелоадер со шторкой иллюминатора:** При первой загрузке открывается шторка окна бизнес-джета (`gsap.fromTo(knob, { yPercent: 0 }, { yPercent: -92, duration: 1.2, ease: "0.76, 0, 0.24, 1" })`), создавая эффект взлета.
* **Синхронизированный медиа-аккордеон:** Клик по пункту сервиса плавно разворачивает текст, вращает иконку на 90° и плавно зуммирует фотографию с масштаба `2.0` до `1.0` с легким смещением по Y.
* **Двойной перекат текста при ховере (Rolling Text Links):** В меню и кнопках текст дублируется в две строки (`.link_label`), при наведении верхняя строка уезжает вверх, а нижняя занимает ее место (`transform: translateY(-100%)`).

### Анимационный стек (Motion Architecture)
* **Lenis Smooth Scroll:** Плавный инерционный скролл с параметрами `duration: 1.2`, экспоненциальным сглаживанием `t => Math.min(1, 1.001 - Math.pow(2, -10 * t))` и синхронизацией через `gsap.ticker`.
* **SplitText & Custom Ease:** Посимвольный (`chars`) и построчный (`lines`) ревил текста с кривой Безье `CustomEase.create("InOut", "0.76, 0, 0.24, 1")`.
* **3D Globe Visualizer:** Рендеринг земного шара с траекториями полетов и плавным следованием маски за курсором (`initGlobeMaskFollow`).

---

## 💻 4. Эталонный код верстки (Reference Snippet)

Готовый React + Tailwind CSS компонент секции в эстетике Jesko Jets с живыми часами Дубая, расширенным гротеском и волосяными разделителями:

```tsx
import React, { useEffect, useState } from 'react';

export function JeskoJetsHeroSection() {
  const [dubaiTime, setDubaiTime] = useState<string>('--:--');

  useEffect(() => {
    const updateTime = () => {
      const now = new Date();
      // Dubai is UTC+4
      const options: Intl.DateTimeFormatOptions = {
        timeZone: 'Asia/Dubai',
        hour: '2-digit',
        minute: '2-digit',
        hour12: false
      };
      setDubaiTime(new Intl.DateTimeFormat('en-GB', options).format(now));
    };

    updateTime();
    const interval = setInterval(updateTime, 1000);
    return () => clearInterval(interval);
  }, []);

  return (
    <section className="relative w-full bg-[#FFF8ED] text-[#312726] selection:bg-[#312726] selection:text-[#FFF8ED] overflow-hidden">
      {/* Top Navigation Bar */}
      <header className="w-full border-b border-[#DBDBDB]/70 px-6 md:px-12 py-5 flex items-center justify-between">
        <a href="#home" className="text-sm font-semibold tracking-[0.18em] uppercase font-['GT_America_Extended',sans-serif]">
          Jesko Jets
        </a>
        <nav className="hidden md:flex items-center gap-10 text-xs tracking-wider uppercase text-[#312726]/80 font-medium">
          <a href="#about" className="hover:text-[#312726] transition-colors duration-200">About</a>
          <a href="#services" className="hover:text-[#312726] transition-colors duration-200">Services</a>
          <a href="#fleet" className="hover:text-[#312726] transition-colors duration-200">Fleet</a>
          <a href="#destinations" className="hover:text-[#312726] transition-colors duration-200">Destinations</a>
        </nav>
        <button className="px-5 py-2.5 rounded-full border border-[#312726] text-xs font-medium uppercase tracking-wider hover:bg-[#312726] hover:text-[#FFF8ED] transition-all duration-300">
          Inquire Flight
        </button>
      </header>

      {/* Main Hero Container */}
      <div className="max-w-[1440px] mx-auto px-6 md:px-12 pt-20 md:pt-32 pb-16">
        {/* Split Typography Titles */}
        <div className="grid grid-cols-1 md:grid-cols-2 gap-8 items-start mb-24 md:mb-36">
          <div>
            <span className="inline-block text-xs uppercase tracking-[0.2em] text-[#7A7372] mb-4">
              Private Aviation Concierge
            </span>
            <h1 className="text-4xl sm:text-6xl lg:text-7xl font-medium tracking-tight leading-[1.04] font-['GT_America_Extended',sans-serif]">
              We are <br className="hidden sm:inline" />movement
            </h1>
          </div>
          <div className="md:text-right flex flex-col md:items-end justify-between h-full pt-4 md:pt-0">
            <span className="hidden md:inline-block text-xs uppercase tracking-[0.2em] text-[#7A7372] mb-4">
              Bespoke Charters
            </span>
            <h2 className="text-4xl sm:text-6xl lg:text-7xl font-medium tracking-tight leading-[1.04] font-['GT_America_Extended',sans-serif]">
              We are <br className="hidden sm:inline" />distinction
            </h2>
          </div>
        </div>

        {/* Narrative & Scroll Prompt Divider */}
        <div className="border-t border-[#DBDBDB] pt-8 pb-12 grid grid-cols-1 md:grid-cols-12 gap-8 items-center text-sm">
          <div className="md:col-span-5">
            <p className="text-xl md:text-2xl font-normal leading-snug tracking-tight text-[#312726]">
              Your freedom to enjoy life.
            </p>
          </div>
          <div className="md:col-span-4 text-[#7A7372] text-xs sm:text-sm leading-relaxed">
            Every flight is designed around your comfort, time, and ambitions — so you can focus on what truly matters, while our team orchestrates every detail.
          </div>
          <div className="md:col-span-3 flex md:justify-end items-center gap-3">
            <div className="w-2 h-2 rounded-full bg-[#312726] animate-pulse" />
            <span className="text-xs tracking-widest uppercase font-medium text-[#312726]">
              Scroll to explore
            </span>
          </div>
        </div>

        {/* Live Operational Telemetry Bar */}
        <div className="border-t border-[#DBDBDB] pt-8 grid grid-cols-1 sm:grid-cols-3 gap-6 sm:gap-8">
          {/* Countries Supported */}
          <div className="flex flex-col border-l border-[#DBDBDB] pl-4 sm:pl-6">
            <span className="text-xs uppercase tracking-wider text-[#7A7372] mb-1">
              Global Coverage
            </span>
            <span className="text-3xl md:text-4xl font-medium font-['GT_America_Extended',sans-serif]">
              174
            </span>
            <span className="text-[11px] text-[#7A7372] mt-0.5">Destinations worldwide</span>
          </div>

          {/* Hub Base */}
          <div className="flex flex-col border-l border-[#DBDBDB] pl-4 sm:pl-6">
            <span className="text-xs uppercase tracking-wider text-[#7A7372] mb-1">
              Operational Base
            </span>
            <span className="text-3xl md:text-4xl font-medium font-['GT_America_Extended',sans-serif]">
              Dubai, UAE
            </span>
            <span className="text-[11px] text-[#7A7372] mt-0.5">Middle East Hub & Hangar</span>
          </div>

          {/* Local Live Clock */}
          <div className="flex flex-col border-l border-[#DBDBDB] pl-4 sm:pl-6">
            <span className="text-xs uppercase tracking-wider text-[#7A7372] mb-1">
              Hub Local Time
            </span>
            <div className="flex items-baseline gap-2">
              <span className="text-3xl md:text-4xl font-medium font-mono tabular-nums tracking-tight">
                {dubaiTime}
              </span>
              <span className="text-xs text-[#7A7372] font-mono">GST (UTC+4)</span>
            </div>
            <span className="text-[11px] text-emerald-600 font-medium mt-0.5">
              ● 24/7 Flight Operations Active
            </span>
          </div>
        </div>
      </div>
    </section>
  );
}
```

---

## 💡 5. Рекомендации по интеграции в проекты

1. **Для люксовых и FinTech интерфейсов:** Заменяйте стандартный белый фон `#FFFFFF` на теплый шампань `#FFF8ED` с контрастным глубоким кофейным `#312726` — это мгновенно выделяет продукт среди шаблонных SaaS.
2. **Для статусных панелей и дашбордов:** Интегрируйте виджет живого времени операционного хаба (`Intl.DateTimeFormat` с временной зоной сервера или клиента) рядом с ключевыми числовыми метриками.
3. **Для лендингов с длинным скроллом:** Используйте Lenis smooth scroll с длительностью `1.2` и анимации SplitText на заголовках для достижения эффекта кинематографичного журнала.
