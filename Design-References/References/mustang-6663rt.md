---
title: "MUSTANG 6663 RT"
slug: "mustang-6663rt"
type: website
source: "https://mustang-ashy.vercel.app/"
repo: "C:\\dev\\Mustang_6663RT"
date: 2026-11-13
tags: ["automotive", "dark-brutalist", "scroll-driven", "gsap", "canvas-assembly", "frame-sequence", "luxury", "specs-counter", "video-bg", "i18n-rtl", "ai-chat", "nextjs15", "react19", "tailwind", "landing"]
stack: ["nextjs-15", "react-19", "gsap", "scrolltrigger", "lenis", "typescript", "tailwind-css", "canvas", "fontsource"]
mood: "Dark brutalist luxury automotive fastback, sheet-lift scroll scrub on HTML5 Canvas, high-contrast typography (Anton + Instrument Serif italic), telemetric salt flat video layer, dynamic CSS filter color regrading, and embedded AI consultant"
preview: "Assets/mustang-6663rt/01-hero-revealed.jpg"
mobilePreview: "Assets/mustang-6663rt/mobile-01-hero.jpg"
---

# 🏎️ Референс: MUSTANG 6663 RT

> **Vibe & Mood:** Темный бруталистский премиум-ленд маслкара ручной сборки. Скролл-скраббинг сдергивания защитного чехла на HTML5 Canvas (121 кадр), строгая контрастная типографика (монументальный гротеск Anton + изящный курсивный Instrument Serif), кинематографичное фоновое видео дрифта по солончаку с телеметрическими плашками, мгновенная смена расцветки кузовных полос через CSS Filter-грейдинг и интерактивный AI-консультант.  
> **Источник (Live):** [https://mustang-ashy.vercel.app/](https://mustang-ashy.vercel.app/)  
> **Исходный код проекта:** `C:\dev\Mustang_6663RT`  
> **Связи базы знаний:** [[foundations-colors|Цвета и Темы]] · [[foundations-typography|Типографика]] · [[foundations-geometry|Сетки и Отступы]] · [[foundations-animations|GSAP и Анимации]]

---

## 📸 1. Визуальный слой (Visual Preview)

### Десктопная версия: Главы интерфейса (Desktop Chapters)

#### 01. Hero & Сдергивание чехла («Sheet comes off · The cold metal remains»)
Полноэкранный пиннинг через GSAP ScrollTrigger (`500vh` трек). По мере прокрутки 121 кадр на холсте высокого разрешения (DPR до 2x) плавно сдергивает ткань с кузова, затем в центре экрана из контура проявляется и заливается титан-белым логотип `MUSTANG`. Статусные плашки: номер шасси `001/066` и точный таймкод `00:05.00`.
![[Assets/mustang-6663rt/01-hero-revealed.jpg]]
*Исходный закрытый кадр прелоадера:*
![[Assets/mustang-6663rt/01-hero.jpg]]

---

#### 02. Спецификации и динамика («Numbers we / never rounded»)
Секция технических характеристик с живым счетчиком значений (CountUp с поддержкой локализованных десятичных знаков): 780 л.с., 3.4 сек до 100 км/ч, 298 км/ч «максималка» и 850 Нм момента. Минималистичные разделители и моноширинная верстка цифр.
![[Assets/mustang-6663rt/02-specs.jpg]]

---

#### 03. Заезд по солончаку (Salt Flat Drive & Telemetry)
Фоновое зацикленное видео скоростного заезда (`public/video/v2.mp4`) с гигантским полупрозрачным вотермарком `MUSTANG` на всю ширину экрана. Поверх видео при скролле каскадом проезжают карточки телеметрии шасси и активной аэродинамики.
![[Assets/mustang-6663rt/03-drive.jpg]]

---

#### 04. Макро-галерея деталей (Crafted to the Millimeter)
Инновационная оптимизация: одна мастер-фотография высокого разрешения динамически трансформируется через `object-position` и `scale`, фокусируя внимание зрителя на заднем колесе (2.6x), фонарях (1.6x), бортовом номере «6663 RT» (2.1x) и ночном световом силуэте (1.0x).
![[Assets/mustang-6663rt/04-detail.jpg]]

---

#### 05. Конфигуратор полос кузова (Stripe Package Selector)
Мгновенное переключение ливреи без повторной загрузки 4 тяжелых картинок: CSS-фильтры (`hue-rotate`, `saturate`, `brightness`, `contrast`) на лету перекрашивают акцентные полосы на оригинальном рендере: Gold (`#C9A24E`), Red (`#C8321E`), Silver (`#B9BCC2`) и Black (`#3A3A3A`).
![[Assets/mustang-6663rt/05-finish.jpg]]

---

#### 06. Финал заезда («Unbreakable / Bred in Detroit»)
Атмосферный кинематографичный видеоряд (`v3.mp4`) с уходящими вдаль габаритными огнями в ночной пустыне и историческим манифестом маслкара.
![[Assets/mustang-6663rt/06-outro.jpg]]

---

#### 07. Аллокация, бронирование и AI Консультант («Reserve your chassis»)
Счетчик оставшихся машин лимитированной серии (41 из 66 доступно), прогресс-бар квоты, модальное окно бронирования, мультиязычный переключатель (EN / RU / HE / AR с честным RTL) и плавающий диалоговый виджет AI Consultant с заготовленными сценариями вопросов и ответов.
![[Assets/mustang-6663rt/07-reserve.jpg]]

---

### Мобильная версия (Mobile Walkthrough, 390×844)

| 01. Hero & Canvas | 02. Specs Numbers | 03. Salt Flat Drive |
| :---: | :---: | :---: |
| ![[Assets/mustang-6663rt/mobile-01-hero.jpg]] | ![[Assets/mustang-6663rt/mobile-02-specs.jpg]] | ![[Assets/mustang-6663rt/mobile-03-drive.jpg]] |

| 04. Stripe Finishes | 05. Allocation & AI Chat | Полный экран |
| :---: | :---: | :---: |
| ![[Assets/mustang-6663rt/mobile-04-finish.jpg]] | ![[Assets/mustang-6663rt/mobile-05-reserve.jpg]] | ![[Assets/mustang-6663rt/mobile.jpg]] |

---

## 🎯 2. Дизайн-токены (Design Tokens)

### Цветовая палитра
Палитра выстроена на строгой монохромной базе глубокого темного чугуна (`#050505`), теплого антибликового белого (`#F2EFE9`), благородного металлического золота сусального листа (`#D4A24C`) и сигнального стоп-красного (`#E2391D`).

| Роль токена | CSS переменная / HEX | Назначение / Примечание |
| :--- | :--- | :--- |
| **Background Deep** | `var(--bg)`: `#050505` | Основной фон всего сайта, отсутствие паразитной засветки |
| **Surface Panel** | `var(--panel)`: `#0B0B0B` | Непрозрачные плашки поверх видео, модальные окна, карточки |
| **Text Primary** | `var(--fg)`: `#F2EFE9` | Теплый молочно-белый контрастный цвет заголовков и цен |
| **Text Muted** | `var(--mute)`: `rgba(242, 239, 233, 0.58)` | Вторичные подписи, описания характеристик, лейблы |
| **Text Dim** | `var(--dim)`: `rgba(242, 239, 233, 0.34)` | Таймкоды, индексы спецификаций, мелкие маркеры |
| **Divider Hairline** | `var(--line)`: `rgba(255, 255, 255, 0.14)` | Тонкие 1px границы кнопок, карточек и разделителей секций |
| **Divider Subtle** | `var(--line2)`: `rgba(255, 255, 255, 0.07)` | Фоновые направляющие сетки |
| **Accent Gold** | `var(--accent)`: `#D4A24C` | Золотой металл: полосы ливреи, курсивные акценты `<em>` в заголовках, суп-индексы |
| **Accent Red** | `var(--red)`: `#E2391D` | Ярко-красный стоп-сигнал: пульсирующие точки статуса (`.dot`), полоса прогресса скролла |

> **HEX-сводка токенов:** `#050505` · `#0B0B0B` · `#F2EFE9` · `#D4A24C` · `#E2391D` · `rgba(255,255,255,0.14)`

---

### Типографическая система (Typography System)
Архитектура шрифтов использует намеренный стилистический контраст: тяжелый плотный американский плакатный гротеск дополняется утонченной итальянской курсивной антиквой.

* **Display Heading Font:** `Anton`, `Oswald`, `Impact`, sans-serif (`--f-display`)
  - Всегда `text-transform: uppercase`, экстремально плотный межстрочный интервал (`line-height: 0.92`), плотный кернинг (`letter-spacing: -0.005em` до `-0.01em`).
* **Italic Serif Accent Font:** `Instrument Serif`, `Cormorant Garamond`, Georgia, serif (`--f-serif`)
  - Применяется внутри заглавий через тег `<em>word</em>`: `font-style: italic`, `text-transform: lowercase`, цвет `var(--accent)`. Добавляет премиальный редакторский лоск.
* **Body / UI Font:** `Inter`, `Heebo`, Arial, sans-serif (`--f-text`)
  - Нейтральный, сбалансированный читаемый гротеск.
* **Многоязычность и RTL (Zero Font Flickering):**
  - Поддерживаются 4 языка: английский (`en`), русский (`ru`), иврит (`he`, RTL) и арабский (`ar`, RTL).
  - Специфические гарнитуры (`Heebo 800` для иврита, `TrueArabic` для арабского) подключены через `@font-face` с жесткими диапазонами `unicode-range`. Латинские и кириллические символы всегда рендерятся в `Anton`/`Inter`, предотвращая скачки макета при переключении языков.

#### Шкала размеров (Type Scale):
* `Hero Display:` `clamp(46px, 7.6vw, 140px)` (Line-height: `0.92`)
* `Big Telemetry Numbers:` `clamp(64px, 7vw, 128px)` (Tabular numbers, Line-height: `1.0`)
* `Section Title:` `clamp(36px, 4.8vw, 72px)`
* `Body Lead:` `15px` (Line-height: `1.65`, Color: `var(--mute)`)
* `Telemetry / Micro Labels:` `10px – 11px` (`letter-spacing: 0.26em – 0.28em`, Uppercase)

---

### Геометрия, Сетки и Микро-взаимодействия

* **Боковые поля (Gutter):** `padding-inline: var(--gx)` где `--gx: 4.2vw` (адаптивный отступ, идеально выравнивающий контент на 1440px+ и смартфонах).
* **Скругления (Border Radius):**
  - Кнопки-пилюли: `border-radius: 40px`
  - Модальные окна и AI-панель: `border-radius: 14px`
  - Меню выбора языков: `border-radius: 10px`
  - Карточки и секции: строгие прямые углы `0px` с волосяным бордером `1px solid var(--line)`.
* **Custom Cursor (Кастомный курсор):**
  - Инверсный курсор с `mix-blend-mode: difference`.
  - При наведении на интерактивные ссылки/кнопки внешний круг сужается до 16px с белой заливкой и масштабированной диагональной стрелкой (`.cur[data-mode="link"]`).
  - При наведении на текстовые инпуты курсор исчезает (`.cur[data-mode="text"]`), уступая место системному кареточному курсору.

---

## 🧠 3. Архитектурный и сценарный анализ (Engineering & UX Patterns)

### 1. Canvas Scroll Scrubbing (Сдергивание чехла)
* **Проблема:** Полноэкранное видео скраббить через `video.currentTime` в браузерах нельзя — это вызывает лаги декодера ключевых кадров (I-frame) и падение FPS до 10-15 кадров/сек.
* **Решение:** Видео преобразуется в последовательность оптимизированных WebP-кадров (121 кадр, от 12 КБ до 25 КБ на кадр) под 3 брейкпоинта: `768px`, `1280px`, `1920px`.
* Отрисовка идет на элементе `<canvas>` с ручным масштабированием через `ctx.drawImage` с логикой `cover` и сохранением нижней трети под отражение пола.
* При быстром скролле алгоритм `nearest()` находит ближайший уже загруженный кадр, исключая белые мерцания, пока фоновый пул подгружает целевой кадр.

### 2. Pinned Video with Text Underlay (Сквозной вотермарк)
* В секции `Drive` видео дрифта закрепляется (`pin: true`), а гигантский бренд `MUSTANG` располагается в абсолютном слое.
* Текстовые плашки телеметрии прокручиваются поверх зафиксированного видео, создавая глубокий 3D-эффект многослойного пространства без WebGL.

### 3. Dynamic Single-Image Detail Zoom
* Вместо загрузки 4 разных макро-снимков высокого разрешения галерея `Detail` оперирует одной фотографией.
* Параметры переходов задаются в виде легких метаданных `{ photo: 'profile', pos: '30% 72%', zoom: 2.6 }`, что экономит ~5 МБ сетевого трафика на мобильных устройствах.

### 4. Zero-Asset CSS Filter Grading
* Для 4 вариантов кастомных полос (`Gold`, `Red`, `Silver`, `Black`) не рендерятся 4 отдельных 4K-изображения автомобиля.
* Графика берется из одного рендера с нейтральной полосой, а цвет меняется мгновенно инлайновым CSS-фильтром `filter: hue-rotate(-38deg) saturate(2.2)`.

### 5. Интегрированный AI Консультант (Ferrari AiChat Pattern)
* Плавающий виджет `AiChat` в правом нижнем углу интерфейса.
* Поддерживает предзаготовленные быстрые вопросы (мощность, сроки поставки, кастомизация, бронирование), авто-прокрутку к новым сообщениям и интеграцию с общей локализацией.

---

## 💻 4. Эталонный код компонентов (Production-Ready Snippets)

### Сниппет 1: Hero Canvas Sequence Scrubber (React + GSAP ScrollTrigger)

```tsx
'use client';
import { useEffect, useRef, useState } from 'react';
import gsap from 'gsap';
import { ScrollTrigger } from 'gsap/ScrollTrigger';

gsap.registerPlugin(ScrollTrigger);

interface CanvasHeroProps {
  totalFrames?: number;
  framePattern?: (idx: number) => string;
}

export function CanvasHeroScrubber({
  totalFrames = 121,
  framePattern = (i) => `/frames/assembly/1280/frame_${String(i).padStart(4, '0')}.webp`
}: CanvasHeroProps) {
  const containerRef = useRef<HTMLDivElement>(null);
  const canvasRef = useRef<HTMLCanvasElement>(null);
  const [progress, setProgress] = useState(0);

  useEffect(() => {
    const canvas = canvasRef.current;
    const container = containerRef.current;
    if (!canvas || !container) return;

    const ctx = canvas.getContext('2d');
    if (!ctx) return;

    // Кэш изображений
    const images: HTMLImageElement[] = [];
    let loadedCount = 0;

    for (let i = 1; i <= totalFrames; i++) {
      const img = new Image();
      img.src = framePattern(i);
      img.onload = () => {
        loadedCount++;
        if (loadedCount === 1) renderFrame(1);
      };
      images.push(img);
    }

    const renderFrame = (index: number) => {
      const img = images[Math.min(totalFrames - 1, Math.max(0, index - 1))];
      if (!img || !img.complete) return;

      const dpr = window.devicePixelRatio || 1;
      canvas.width = canvas.clientWidth * dpr;
      canvas.height = canvas.clientHeight * dpr;
      ctx.scale(dpr, dpr);

      // Масштабирование 'cover'
      const scale = Math.max(canvas.clientWidth / img.naturalWidth, canvas.clientHeight / img.naturalHeight);
      const w = img.naturalWidth * scale;
      const h = img.naturalHeight * scale;
      const x = (canvas.clientWidth - w) / 2;
      const y = (canvas.clientHeight - h) * 0.6; // Фиксация с акцентом на отражение колес

      ctx.fillStyle = '#050505';
      ctx.fillRect(0, 0, canvas.clientWidth, canvas.clientHeight);
      ctx.drawImage(img, x, y, w, h);
    };

    const trigger = ScrollTrigger.create({
      trigger: container,
      start: 'top top',
      end: '+=400%',
      pin: true,
      scrub: 0.5,
      onUpdate: (self) => {
        setProgress(self.progress);
        const frameIndex = Math.floor(self.progress * (totalFrames - 1)) + 1;
        renderFrame(frameIndex);
      }
    });

    return () => {
      trigger.kill();
    };
  }, [totalFrames, framePattern]);

  return (
    <div ref={containerRef} className="relative w-full h-screen bg-[#050505] overflow-hidden">
      <canvas ref={canvasRef} className="absolute inset-0 w-full h-full object-cover" />
      
      {/* HUD Telemetry Overlay */}
      <div className="absolute top-8 left-8 text-xs font-mono uppercase tracking-widest text-[#F2EFE9]/60 flex items-center gap-3 z-10">
        <span className="w-2 h-2 rounded-full bg-[#E2391D] animate-ping" />
        <span>REVEAL SEQ // {Math.round(progress * 100)}%</span>
      </div>

      <div className="absolute bottom-12 left-1/2 -translate-x-1/2 text-center pointer-events-none z-10">
        <h1 className="font-['Anton'] text-7xl md:text-9xl tracking-tight text-[#F2EFE9] uppercase">
          MUSTANG <span className="font-['Instrument_Serif'] lowercase italic text-[#D4A24C]">6663</span> RT
        </h1>
      </div>
    </div>
  );
}
```

---

### Сниппет 2: Stripe Color Grading Selector (CSS Filter Switcher)

```tsx
'use client';
import React, { useState } from 'react';

interface FinishOption {
  id: string;
  name: string;
  code: string;
  hex: string;
  filter: string;
}

const FINISHES: FinishOption[] = [
  { id: 'gld', name: 'Raw Gold Leaf', code: 'GLD', hex: '#C9A24E', filter: 'none' },
  { id: 'red', name: 'Competition Red', code: 'RED', hex: '#C8321E', filter: 'hue-rotate(-38deg) saturate(2.2)' },
  { id: 'slv', name: 'Brushed Silver', code: 'SLV', hex: '#B9BCC2', filter: 'saturate(0) brightness(1.08)' },
  { id: 'blk', name: 'Matte Shadow', code: 'BLK', hex: '#3A3A3A', filter: 'saturate(0) brightness(.62) contrast(1.15)' }
];

export function StripeConfigurator() {
  const [selected, setSelected] = useState<FinishOption>(FINISHES[0]);

  return (
    <section className="relative w-full bg-[#050505] text-[#F2EFE9] py-24 px-6 border-t border-white/10">
      <div className="max-w-6xl mx-auto flex flex-col items-center">
        <span className="text-[11px] font-mono tracking-[0.28em] text-[#E2391D] uppercase mb-3 flex items-center gap-2">
          <span className="w-1.5 h-1.5 rounded-full bg-[#E2391D]" /> 05 // LIVERY FINISH
        </span>
        <h2 className="font-['Anton'] text-4xl md:text-6xl uppercase tracking-tight mb-12">
          SELECT <em className="font-['Instrument_Serif'] text-[#D4A24C] lowercase italic">stripe</em> PACKAGE
        </h2>

        {/* Viewport with CSS Filter application */}
        <div className="relative w-full max-w-4xl aspect-[16/9] bg-[#0B0B0B] border border-white/10 rounded-lg overflow-hidden mb-8 shadow-2xl">
          <img
            src="/img/profile-1280.webp"
            alt="Mustang 6663 RT Profile"
            className="w-full h-full object-cover transition-all duration-500 ease-out"
            style={{ filter: selected.filter }}
          />
          <div className="absolute bottom-4 left-6 font-mono text-xs text-[#F2EFE9]/50">
            SPEC: {selected.code} // {selected.name.toUpperCase()}
          </div>
        </div>

        {/* Swatch Controls */}
        <div className="flex gap-4">
          {FINISHES.map((f) => (
            <button
              key={f.id}
              onClick={() => setSelected(f)}
              className={`flex items-center gap-3 px-5 py-2.5 rounded-full border transition-all text-xs font-semibold tracking-wider uppercase ${
                selected.id === f.id
                  ? 'border-[#D4A24C] bg-white/10 text-white'
                  : 'border-white/20 text-white/60 hover:border-white/40'
              }`}
            >
              <span className="w-3.5 h-3.5 rounded-full border border-white/20" style={{ backgroundColor: f.hex }} />
              {f.code}
            </button>
          ))}
        </div>
      </div>
    </section>
  );
}
```

---

## 🤖 5. Инструкция для AI-агентов (Cursor, Claude Code, Antigravity)

При создании сайтов для люксовых автомобильных марок, high-end техники, мужских брендов или сложных технологических продуктов:

1. **Цветовая дисциплина:** Не используйте чистый черный `#000000` под фон — берите глубокий угольный `#050505` с панелями `#0B0B0B`. Для текста используйте не стерильный белый, а антибликовый молочный `#F2EFE9`.
2. **Типографический прием «Editorial Brutalism»:**
   - Заголовки пишите прописным сжатым гротеском (`Anton`, `Impact`, `Oswald`) с line-height `0.92`.
   - Одно ключевое эмоциональное слово в заголовке оборачивайте в курсивную классическую антикву строчными буквами (`Instrument Serif`, `Cormorant Garamond`) золотого оттенка `#D4A24C`.
3. **Оптимизация скролл-анимаций:**
   - Если требуется раскрытие продукта со скроллом — используйте покадровый экспорт в WebP и рендер на HTML5 Canvas с интерполяцией `nearest()`. Никогда не скрабьте нативное видео по `currentTime`.
   - Если требуется кастомизация цвета/материала — используйте CSS Filters на одном слое вместо 4 тяжелых картинок.
4. **Микро-детали:**
   - Используйте красные индикаторы `#E2391D` для живых статусов (`pulse`) и бейджей аллокации.
   - Делайте моноширинные подписи (`font-mono`) с трекингом `0.28em` для инженерного контекста.
