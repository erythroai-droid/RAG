---
title: "Регламент синхронизации базы знаний с GitHub (Obsidian Git Sync)"
description: "Автоматический и агентский протокол синхронизации заметок, шаблонов и дизайн-токенов с удаленным репозиторием GitHub."
category: "agent-skills"
scope: "global"
language: "ru"
tags:
  - "git"
  - "github"
  - "obsidian"
  - "sync"
  - "automation"
  - "ci-cd"
last_updated: "2026-08-31"
---

# 🔄 Регламент синхронизации базы знаний с GitHub (Obsidian Git Sync)

#agent-skills #git #github #obsidian #automation #sync #rules

> **Назначение:** Обязательное правило и инструкция по поддержанию актуальности удаленного репозитория базы знаний при любых модификациях, создании новых заметок и рефакторинге.  
> **Удаленный репозиторий:** `https://github.com/erythroai-droid/RAG.git` (ветка `main`).

---

## 1. Абсолютное правило для AI-агентов (AI Agent Rule)

При выполнении любых действий в базе знаний Obsidian (`d:\Obsidian\Knowledge-Base`):
1. **Любая модификация** (создание, редактирование, переименование, удаление заметок, обновление `index.md`) **ОБЯЗАНА** завершаться фиксацией изменений и отправкой в репозиторий GitHub.
2. **Команда синхронизации:**
   ```bash
   git add .
   git commit -m "<type>(<scope>): <краткое описание изменений>"
   git push origin main
   ```
3. **Стандарты коммитов:**
   * `feat(arch): ...` — добавление новых архитектурных документов.
   * `feat(skills): ...` — добавление новых навыков или правил агентов.
   * `docs(glossary): ...` — обновление глоссария, PITFALLS или FAQ.
   * `fix(index): ...` — исправление связей в Mermaid или опечаток.
   * `refactor(tokens): ...` — обновление токенов или структуры каталогов.

---

## 2. Автоматическая синхронизация при ручном редактировании в Obsidian

Для автоматического пуша при работе непосредственно в приложении Obsidian используется плагин **Obsidian Git**:

### 2.1. Установка и настройка плагина
1. В Obsidian открыть **Settings (Настройки) $\to$ Community plugins (Сторонние плагины)**.
2. Найти и установить плагин **Obsidian Git**. Включить его.
3. В настройках **Obsidian Git**:
   * `Vault backup interval (minutes)`: установить `5` (или `10` минут).
   * `Auto backup after file change`: включить (при необходимости мгновенного сохранения).
   * `Commit message on auto backup`: `vault backup: {{date}}`.
   * `Auto pull interval (minutes)`: установить `10` минут для синхронизации с другими устройствами.
   * `Push on backup`: включить (`ON`).

---

## 3. Git Hooks (Автоматический pre-push / post-commit)

В локальном репозитории настроен `.gitignore`, исключающий временные файлы воркспейса Obsidian (`.obsidian/workspace.json`, `.obsidian/cache/`, `.trash/`), что предотвращает конфликты слияния.
