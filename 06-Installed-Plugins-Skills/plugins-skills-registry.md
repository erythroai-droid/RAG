---
title: "Реестр установленных плагинов и скиллов AI-агентов"
description: "Единый каталог всех установленных плагинов, скиллов и MCP-связок для Antigravity, Cursor, Claude Code и общих директорий агентов с версиями, источниками и статусом обновлений."
category: "installed-plugins-skills"
scope: "global"
language: "ru"
tags:
  - "plugins"
  - "skills"
  - "agents"
  - "registry"
  - "updates"
  - "antigravity"
  - "cursor"
  - "claude"
last_updated: "2026-11-19"
---

# 🧩 Реестр установленных плагинов и скиллов (все агенты)

#plugins #skills #agents #registry #updates

> **Назначение:** единая точка учёта всех плагинов и скиллов, установленных для всех AI-агентов (Antigravity, Cursor, Claude Code, Codex, Grok и общие `.agents`).  
> **Связанный регламент:** [[plugin-update-policy|Регламент проверки и установки обновлений плагинов и скиллов]].

> [!IMPORTANT]
> **Обязательное правило для всех агентов:** при установке, обновлении или удалении любого плагина или скилла (глобально или в проекте) **немедленно обновить этот реестр** — указать агента, версию/коммит, источник, путь установки и дату проверки — и синхронизировать базу знаний с GitHub по [[obsidian-git-sync-rule]].

---

## 1. Antigravity (`~/.gemini/config/plugins/`)

Путь на Windows: `C:\Users\Вадим\.gemini\config\plugins\`

| Плагин (dir) | Название | Версия / коммит | Источник | Скиллы / возможности | Проверка обновлений |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `modern-web-guidance-plugin` | Modern Web Guidance | `1.0.1` (`64a6d70`) | [GoogleChrome/modern-web-guidance](https://github.com/GoogleChrome/modern-web-guidance) | `modern-web-guidance` (+210 гайдов), `chrome-extensions` | **Автоматизация** `modern-web-guidance-update` (Пн 11:00 `Asia/Jerusalem`) · проверено `2026-11-19` |
| `vercel-plugin` | Vercel | `0.54.1-agy.4b94cb5a` | [vercel/vercel-plugin](https://github.com/vercel/vercel-plugin) | 37 скиллов (`ai-sdk`, `ai-gateway`, `vercel-cli`, `deployments-cicd`, `react-best-practices`, `workflow`, `flags-sdk`, `vercel-functions`, `vercel-storage` и др.) | Marketplace / ручная проверка · зафиксировано `2026-11-19` |
| `figma` | Figma | `2.2.174-agy.82ddeba0` | [figma/mcp-server-guide](https://github.com/figma/mcp-server-guide) | 16 скиллов (`figma-design-to-code`, `figma-use`, `figma-generate-design`, `figma-generate-library`, `figma-code-connect` и др.) + Figma MCP | Marketplace / ручная проверка · зафиксировано `2026-11-19` |
| `supabase` | Supabase | `0.1.17-agy.a224d9f1` | [supabase-community/supabase-plugin](https://github.com/supabase-community/supabase-plugin) | `supabase`, `supabase-postgres-best-practices` + Supabase MCP | Marketplace / ручная проверка · зафиксировано `2026-11-19` |
| `mobbin` | Mobbin | `1.5.0` | [mobbin/skills](https://github.com/mobbin/skills) | `best-practices`, `plain-writing`, `redesign-screen`, `search` + Mobbin MCP | Marketplace / ручная проверка · зафиксировано `2026-11-19` |
| `chrome-devtools-plugin` | Chrome DevTools | `0.21.0` | [ChromeDevTools/chrome-devtools-mcp](https://github.com/ChromeDevTools/chrome-devtools-mcp) | Chrome DevTools MCP | Marketplace / ручная проверка · зафиксировано `2026-11-19` |
| `android-cli-plugin` | Android CLI | `1.0.16625370` | Antigravity Marketplace | `android-cli`, `compose-preview`, `device-hub` | Marketplace / ручная проверка · зафиксировано `2026-11-19` |
| `google-antigravity-sdk` | Google Antigravity SDK | standalone `SKILL.md` | Локальный / Google Antigravity SDK | `google-antigravity-sdk` | Ручная проверка · зафиксировано `2026-11-19` |

---

## 2. Claude Code (`~/.claude/plugins/` и проектные `.claude/skills/`)

| Плагин / скилл | Область | Версия / коммит | Путь | Назначение | Проверено |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `hostinger@claude-plugins-official` | user | `1.2.0` (`569880c`) | `~/.claude/plugins/cache/claude-plugins-official/hostinger/1.2.0` | Интеграция и управление Hostinger | `2026-10-01` (записано `2026-11-19`) |
| `impeccable` | project (`erythro-ai`) | `4.8.0` | `c:\dev\erythro-ai\.claude\skills\impeccable` | UI/UX дизайн, аудит, полировка и дизайн-системы фронтенда | `2026-11-19` |

---

## 3. Cursor (`~/.cursor/plugins/cache/cursor-public/`)

| Плагин | Версия / коммит | Источник | Зафиксировано |
| :--- | :--- | :--- | :--- |
| `cloudflare` | `41e0d19` | `cursor-public` | `2026-11-19` |
| `deploy-on-aws` | `e32b05b` | `cursor-public` | `2026-11-19` |
| `figma` | `490a73c` | `cursor-public` | `2026-11-19` |
| `github` | `e58dd04` | `cursor-public` | `2026-11-19` |
| `gmail` | `e58dd04` | `cursor-public` | `2026-11-19` |
| `hostinger-cursor-plugin` | `0a3d814` | `cursor-public` | `2026-11-19` |
| `make` | `c65bccc` | `cursor-public` | `2026-11-19` |
| `runway-api` | `3dcbb5b` | `cursor-public` | `2026-11-19` |
| `ryze-ai` | `fc9dec1` | `cursor-public` | `2026-11-19` |
| `slack` | `64c3f33` | `cursor-public` | `2026-11-19` |
| `stripe` | `e4856d5` | `cursor-public` | `2026-11-19` |
| `supabase` | `release_v0.1.4` | `cursor-public` | `2026-11-19` |
| `vercel` | `3b47264` | `cursor-public` | `2026-11-19` |

---

## 4. Общие кросс-агентные скиллы (`~/.agents/skills/`)

| Скилл | Путь | Назначение | Зафиксировано |
| :--- | :--- | :--- | :--- |
| `find-skills` | `~/.agents/skills/find-skills/SKILL.md` | Поиск и установка скиллов из открытой экосистемы агентских скиллов | `2026-11-19` |

---

## 5. Журнал изменений реестра

- **`2026-11-19`**: Создана ветка `06-Installed-Plugins-Skills`. Проведена полная инвентаризация плагинов и скиллов (Antigravity, Claude Code, Cursor, `.agents`). Плагин `modern-web-guidance-plugin` обновлён с `1.0.0` → `1.0.1` (`64a6d70`), настроена еженедельная автоматизация проверки обновлений (`modern-web-guidance-update`).
