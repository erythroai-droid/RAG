# product-scroll-landing — шаблон в templates.git

**Репозиторий:** https://github.com/erythroai-droid/templates  
**Папка:** `product-scroll-landing/`  
**Референс-прод:** https://ora60a.erythro.ai (`C:\dev\Watch\site`)  
**Статус:** доступен в `main` (первый root-commit шаблонов).

## Зачем

Один клон → `npm i && npm run dev` → бренд / тексты / кадры → Vercel + `*.erythro.ai` за часы.

## Как взять шаблон

```bash
npx degit erythroai-droid/templates/product-scroll-landing my-client-site
cd my-client-site
npm install
npm run new-site -- <slug> "<Brand>" "<Model>" https://<slug>.erythro.ai
npm run frames -- path/to/video.mov   # затем STUB_MEDIA=false, FRAMES=N
npm run i18n
```

Подробный чеклист клиента: `SITE.md` внутри шаблона.

## Стек (не регрессировать)

| Слой | Решение |
|------|---------|
| App | Next 15 + React 19 + GSAP |
| Конфиг | `lib/site.ts` (`BRAND`, `MODEL`, `FRAMES`, `PARTS`, `STUB_MEDIA`) |
| i18n | `i18n/{en,ru,he,ar}.txt` → `npm run i18n` → `lib/dict.ts` |
| Шрифты | `@fontsource/inter` + `@fontsource/heebo` + local Arabic — **без** `next/font/google` |
| Видео-секции | opaque `--panel` под `.has-vbg` |
| Agent | `robots.txt`, `llms.txt`, `sitemap.ts`, Speed Insights |
| Медиа в git | только stub webp (3–5 кадров × 3 ширины); mp4 не коммитим |

## Деплой

1. Отдельный GitHub repo из папки шаблона (или Root Directory в monorepo).
2. Vercel → Next.js → Production Branch `main`.
3. Cloudflare CNAME `<slug>` → `*.vercel-dns-*.com`, DNS only.
4. Smoke: `/`, `/robots.txt`, `/llms.txt`, `/sitemap.xml`, Speed Insights.

## Pitfalls (коротко)

- `.gitignore`: только `/Video/`, не `Video/` (Windows съедает `public/video`).
- Не возвращать `next/font/google` на Next 15.5 (loader crash).
- После extract-frames: `STUB_MEDIA=false`, выставить `FRAMES` и перетаймить `PARTS[].f`.

См. также [[pitfalls-troubleshooting]] и `docs/PITFALLS.md` в Watch.
