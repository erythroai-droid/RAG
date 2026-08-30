# ✍️ Типографические токены (Typography Foundations)

#design-tokens #typography #fonts #inter #heebo #roboto-mono #rtl

> **Назначение:** Регламент типографики, шрифтовых семейств, иерархии заголовков и правил верстки мультиязычного текста (латиница, кириллица, иврит).

---

## 1. Шрифтовые семейства

1. **`Inter` (Latin & Cyrillic):** Основной шрифт для всего контента на английском и русском языках.
2. **`Heebo` (Hebrew):** Специализированный шрифт для иврита с идеальным хинтингом огласовок и пропорций букв в режиме `dir="rtl"`.
3. **`Roboto Mono` (Tech & Numbers):** Моноширинный шрифт для кода, чисел, таймлайнов, цен и системных идентификаторов.

---

## 2. Типографическая шкала (Type Scale)

| Уровень | Размер (Desktop) | Размер (Mobile) | Line-Height | Font Weight | Классы Tailwind |
|---|---|---|---|---|---|
| **H1 (Hero Display)** | 64px–72px | 36px–42px | 1.1 | 700 (Bold) | `text-4xl md:text-6xl font-bold leading-tight` |
| **H2 (Section Title)** | 40px–48px | 28px–32px | 1.2 | 700 (Bold) | `text-3xl md:text-5xl font-bold leading-tight` |
| **H3 (Card Title)** | 24px–28px | 20px–22px | 1.3 | 600 (Semibold) | `text-xl md:text-2xl font-semibold` |
| **H4 (Subheading)** | 18px–20px | 16px–18px | 1.4 | 600 (Semibold) | `text-lg md:text-xl font-semibold` |
| **Body Large (Lead)** | 18px | 16px | 1.6 | 400 (Regular) | `text-base md:text-lg leading-relaxed` |
| **Body (Default)** | 15px–16px | 14px–15px | 1.6 | 400 (Regular) | `text-sm md:text-base leading-normal` |
| **Caption / Meta** | 12px–13px | 11px–12px | 1.4 | 500 (Medium) | `text-xs font-medium text-muted` |
| **Mono / Badge** | 12px–14px | 11px–12px | 1.0 | 500 (Medium) | `font-mono text-xs tracking-wider uppercase` |

---

## 3. Правила верстки мультиязычного текста

1. **Hebrew BiDi & Numbers:**
   - Номера телефонов, коды и цены на иврите должны рендериться в LTR-режиме внутри RTL-потока: `<span dir="ltr" className="inline-block">+972 50 530 83 05</span>`.
2. **Переносы строк (Hyphens):**
   - Запрещено использовать жесткие дефисы в заголовках.
   - Используйте `text-balance` для заголовков H1-H3, чтобы избежать висячих строк.
3. **Трекинг (Letter-spacing):**
   - Заголовки H1-H2: `tracking-tight` (`-0.02em`).
   - Моноширинные бейджи и верхний регистр: `tracking-widest` (`+0.08em`).
