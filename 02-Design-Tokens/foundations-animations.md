# 🎬 Анимации и Интерактивность (Animations & GSAP)

#design-tokens #animations #gsap #canvas #scrolltrigger #microinteractions

> **Назначение:** Стандарты таймингов, физики переходов, GSAP-таймлайнов и интерактивного Canvas Hero рендеринга.

---

## 1. Константы таймингов и физики (Easing & Durations)

```css
/* Стандартные переходы интерфейса */
--ease-standard: cubic-bezier(0.4, 0.0, 0.2, 1); /* Стандартное сглаживание */
--ease-out-expo: cubic-bezier(0.16, 1, 0.3, 1);  /* Премиальный выезд карточек/модалок */
--ease-in-out-smooth: cubic-bezier(0.65, 0, 0.35, 1);

--duration-fast: 150ms;   /* Ховеры кнопок, смена цвета */
--duration-normal: 300ms; /* Раскрытие аккордеонов, переключение табов */
--duration-slow: 500ms;   /* Появление модальных окон, выезд мобильного меню */
```

### Классы быстрых CSS-переходов:
- Кнопки, ссылки: `transition-all duration-300 ease-out`
- Смена темной/светлой темы: `transition-colors duration-300 ease-in-out`

---

## 2. Hero Interactive Canvas Sequence (110 кадров WebP)

Архитектура высокопроизводительного Hero-блока с отрисовкой секвенции 3D-модели (110 кадров) на HTML5 `<canvas>`:

```typescript
// Ключевые принципы оптимизации Hero Canvas:
1. Предзагрузка первых 10 кадров с высоким приоритетом (Instant LCP).
2. Фоновая дозагрузка кадров 11-110 в Web Worker или через Image() pool.
3. Отрисовка через requestAnimationFrame с интерполяцией текущего прогресса скролла.
4. Отключение рендеринга (pause), когда Hero-секция выходит за пределы видимости (IntersectionObserver).
5. Resize-дебаунс: canvas.width / canvas.height пересчитываются с учетом window.devicePixelRatio (но не более 2.0 для экономии GPU памяти).
```

---

## 3. Правила работы с GSAP ScrollTrigger

1. **Изоляция контекста (`gsap.context`):**
   ```typescript
   useEffect(() => {
     const ctx = gsap.context(() => {
       gsap.from('.stagger-card', {
         y: 40,
         opacity: 0,
         duration: 0.8,
         stagger: 0.15,
         ease: 'power3.out',
         scrollTrigger: {
           trigger: '.cards-container',
           start: 'top 80%',
         },
       })
     })
     return () => ctx.revert() // Обязательная очистка памяти при анмаунте
   }, [])
   ```

2. **Защита от Reduced Motion:**
   При наличии системного флага `prefers-reduced-motion: reduce` все тяжелые перемещения и скролл-эффекты должны отключаться, заменяясь на мгновенное или простое `opacity: 1` появление.
