---
description: Rule — every demo / template site shown to prospects ships a "thanks for viewing → WhatsApp" notice that folds into a pulsing WhatsApp button, with click analytics in Vercel Web Analytics.
always_on: true
---

# Демо-уведомление с WhatsApp и статистикой кликов

#agent-skills #rules #landing #whatsapp #analytics #vercel

> **Действует для всех демо-сайтов и шаблонов, которые показываются потенциальным клиентам.**
> Добавлять по умолчанию, без отдельной просьбы. Референс-реализация: **ORA 60A** (`C:\dev\Watch\site`, https://ora60a.erythro.ai).
> Связано: [[product-scroll-landing]], [[translation-worker-rule]].

## 1. Что видит посетитель

**Карточка** (тёмное стекло, 18px радиус, в стиле сайта):

| Ключ | RU (источник) |
|------|---------------|
| `demo_h` | Спасибо за просмотр |
| `demo_p` | Это демонстрационный проект. Чтобы обсудить сайт для вашего бренда, свяжитесь с нами. |
| `demo_wa` | Написать в WhatsApp |
| `demo_open` | Связаться с нами *(aria-label свёрнутой кнопки)* |
| `demo_msg` | Здравствуйте! Хочу обсудить сайт для своего бренда. Пишу после просмотра демо ORA 60A. *(предзаполненное сообщение — заменить название проекта)* |

- Только кнопка WhatsApp. **Номер телефона на странице не показывать** — он живёт только в ссылке `https://wa.me/972505308305?text=…`.
- Переводы EN / HE / AR — только через воркер `C:\agents\Translater` (стиль `UI/Microcopy`), см. [[translation-worker-rule]].

**Когда открывается** — один раз за визит (`sessionStorage`), что наступит раньше:
1. футер в зоне видимости (≥35%) — посмотрел всё;
2. таймер ~40 с;
3. exit intent на десктопе (курсор ушёл за верхний край окна).

**При перезагрузке** (F5 / Ctrl+F5) в том же визите карточка снова открывается **развёрнутой** через ~1.2 с — никогда не стартует свёрнутой кнопкой, чтобы клиент всегда видел текст, а не «просто кнопку WhatsApp».

**Закрытие:** крестик, `Esc`, на телефоне — свайп вниз. После закрытия карточка сворачивается в **круглую зелёную кнопку WhatsApp**, которая снова открывает карточку (до следующей перезагрузки).

**Пульсация свёрнутой кнопки** (обязательно): два зелёных кольца по очереди расходятся (`scale 1 → 1.9`, `opacity .55 → 0`, цикл 2.4 с, второе со сдвигом 1.2 с) + лёгкое «дыхание» самой кнопки (`scale 1.06`). Пауза на hover, выключено при `prefers-reduced-motion`.

**Раскладка:** нижний *начальный* угол (`inset-inline-start`) — напротив остальных плавающих кнопок (AI-консультант, «наверх»); в RTL зеркалится сам. На телефоне (`max-width:900px`) — нижняя панель на всю ширину. Если сайт тёмный под карточкой — кастомный курсор должен переключаться в светлый вариант над `.demo`.

## 2. Статистика кликов (Vercel Web Analytics)

`npm i @vercel/analytics` → `<Analytics />` из `@vercel/analytics/next` в `app/layout.tsx` (рядом с Speed Insights) → `track()` через обёртку `trackDemo` с `try/catch`.

| Событие | Когда | Данные |
|---------|-------|--------|
| `demo_notice_shown` | карточка открылась сама | `trigger`: `footer` / `timer` / `exit` / `reload` |
| `demo_notice_closed` | закрыли | `via`: `close` / `esc` / `swipe` |
| `demo_notice_reopened` | клик по пульсирующей кнопке | — |
| `demo_whatsapp_click` | **конверсия** — клик «Написать в WhatsApp» | `opened_by` (trigger или `button`), `lang` |

Отчёт: Vercel → проект → **Analytics → Events**. В dev события только пишутся в консоль (`[Vercel Web Analytics] [event] …`) — так и проверять.

> ⚠️ Web Analytics нужно **включить в проекте** (Analytics → Enable). Пользовательские события (custom events) по документации Vercel доступны на **Pro**; на Hobby будут только просмотры страниц.

## 3. Файлы и конфиг

| Файл | Что внутри |
|------|-----------|
| `lib/demo.ts` | `DEMO = { enabled, phone, delayMs }`, `waLink(text)`, `trackDemo(name, data)` |
| `components/DemoNotice.tsx` | карточка + свёрнутая кнопка, триггеры, события |
| `app/globals.css` | `.demo*`, `.demo-mini`, `@keyframes waPulse / waBreath` |
| `app/page.tsx` | `<DemoNotice />` рядом с `<AiChat />` |
| `i18n/*.txt` | ключи `demo_*` → `npm run i18n` |

**Выключить на сданном клиенту сайте:** Vercel → Environment Variables → `NEXT_PUBLIC_DEMO_NOTICE=0` → Redeploy. Код не трогать.

## 4. Чеклист для агента

- [ ] `lib/demo.ts`, `DemoNotice.tsx`, стили скопированы из Watch; в `demo_msg` подставлено имя проекта.
- [ ] Строки `demo_*` добавлены в `ru.txt` и переведены воркером; `npm run i18n`.
- [ ] `@vercel/analytics` установлен, `<Analytics />` в layout.
- [ ] Проверено в браузере: открытие по футеру, закрытие → пульсирующая кнопка, повторное открытие, ссылка `wa.me` с текстом, 4 события в консоли; десктоп + 375px + RTL.
- [ ] Пользователю напомнить включить Web Analytics в Vercel.
