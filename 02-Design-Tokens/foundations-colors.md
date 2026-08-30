# 🎨 Цветовые токены и Темы (Color Foundations)

#design-tokens #colors #dark-mode #tailwind #theme

> **Назначение:** Стандарт палитры, семантических цветов, темной/светлой темы и визуальных эффектов (Noise Texture) премиум-уровня.

---

## 1. Примитивы палитры (Color Primitives)

```css
/* Базовые цветовые константы */
--color-coal-900: #0D0D0D; /* Глубокий премиальный черный */
--color-coal-800: #1A1A1A; /* Темный антрацит (поверхности, карточки) */
--color-coal-700: #262626; /* Разделители, границы в темной теме */
--color-coal-100: #F5F5F5; /* Светлый оф-вайт */

--color-erythro:  #E50000; /* Фирменный акцентный алый */
--color-gold:     #D4AF37; /* Акцентное приглушенное золото */

--color-white:    #FFFFFF;
--color-cream:    #FDFBF7; /* Фоновая подложка светлой темы */
```

---

## 2. Семантические токены тем (Semantic Mapping)

Семантические переменные связывают физические цвета с их ролью в интерфейсе. Переключение темы происходит добавлением класса `.dark` к корневому тегу `<html>`.

| Семантический токен | Светлая тема (`:root`) | Темная тема (`.dark`) | Назначение |
|---|---|---|---|
| `--bg-primary` | `#FFFFFF` | `#0D0D0D` (Coal 900) | Основной фон страницы |
| `--bg-surface` | `#FDFBF7` (Cream) | `#1A1A1A` (Coal 800) | Карточки, модальные окна, плашки |
| `--bg-elevated` | `#F5F5F5` | `#262626` | Всплывающие подсказки, ховеры |
| `--text-main` | `#0D0D0D` | `#F5F5F5` (Coal 100) | Основной текст, заголовки H1-H6 |
| `--text-muted` | `#737373` | `#A3A3A3` | Второстепенный текст, даты, подписи |
| `--border-color` | `rgba(0,0,0,0.08)` | `rgba(255,255,255,0.10)` | Тонкие границы блоков |
| `--brand-accent` | `#E50000` | `#E50000` | Кнопки действия, ключевые акценты |
| `--brand-gold` | `#D4AF37` | `#D4AF37` | Бейджи, премиальные статусы |

---

## 3. Процедурная текстура шума (5% Noise Background)

Для создания эффекта «тихой роскоши» (Quiet Luxury) на темных и светлых фонах используется процедурный SVG-шум:

```css
.bg-noise {
  position: relative;
}

.bg-noise::before {
  content: "";
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  opacity: 0.04;
  pointer-events: none;
  background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noiseFilter'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.65' numOctaves='3' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noiseFilter)'/%3E%3C/svg%3E");
}
```

---

## 4. Bootstrap скрипт темы (Предотвращение мигания FOUC)

Для исключения мигания белым экраном при загрузке темной темы в `<head>` внедряется синхронный скрипт `THEME_BOOTSTRAP_SCRIPT`:

```html
<script id="theme-bootstrap">
  (function() {
    try {
      var match = document.cookie.match(/(?:^|; )theme=([^;]*)/);
      var theme = match ? decodeURIComponent(match[1]) : null;
      if (!theme) {
        theme = window.matchMedia('(prefers-color-scheme: dark)').matches ? 'dark' : 'light';
      }
      if (theme === 'dark') {
        document.documentElement.classList.add('dark');
      } else {
        document.documentElement.classList.remove('dark');
      }
    } catch(e) {}
  })();
</script>
```
