# Import Project (Erythro.ai)

#cms #portfolio #import #skill

> **Synced from:** `erythro-ai/docs/IMPORT_PROJECT.md`  
> **Sync date:** 2026-08-28  
> Canonical source of truth for this file lives in the project repo; keep both sides updated (two-way sync rule).

---
# Import portfolio projects

Каноническая инструкция и код: [`scripts/import-project/README.md`](../scripts/import-project/README.md).

Кратко: создать slug в админке → `content/imports/<slug>/brief.yaml` → `pnpm import:project -- content/imports/<slug>`. Кэш сайта: Save в прод-админке или `REVALIDATION_TOKEN`.
