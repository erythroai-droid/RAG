# рџ§  Р‘Р°Р·Р° Р·РЅР°РЅРёР№ Erythro.ai вЂ” RAG Knowledge Base

> **РљРѕСЂРїРѕСЂР°С‚РёРІРЅС‹Р№ РґРёСЃРїРµС‚С‡РµСЂ Р·РЅР°РЅРёР№, Р°СЂС…РёС‚РµРєС‚СѓСЂРЅС‹С… СЃС‚Р°РЅРґР°СЂС‚РѕРІ Рё СЂРµРіР»Р°РјРµРЅС‚РѕРІ РґР»СЏ AI-Р°РіРµРЅС‚РѕРІ Рё СЂР°Р·СЂР°Р±РѕС‚С‡РёРєРѕРІ.**  
> РЎРѕР·РґР°РЅРѕ РґР»СЏ РѕР±РµСЃРїРµС‡РµРЅРёСЏ РїСЂРµРµРјСЃС‚РІРµРЅРЅРѕСЃС‚Рё Р°СЂС…РёС‚РµРєС‚СѓСЂС‹, РїСЂРµРґРѕС‚РІСЂР°С‰РµРЅРёСЏ СЂРµС†РёРґРёРІРѕРІ РѕС€РёР±РѕРє (Pitfalls) Рё Р±С‹СЃС‚СЂРѕРіРѕ СЂР°Р·РІРµСЂС‚С‹РІР°РЅРёСЏ РЅРѕРІС‹С… AI-РїСЂРѕРµРєС‚РѕРІ.
> **Sync (2026-09-06):** Р·РµСЂРєР°Р»Р° `erythro-ai/docs/*` вЂ” Access `/admin*` + DMARC quarantine runbook (PIT-064), R2 media, PLAN-deferred, RAG_INDEX ingest map. РџСЂР°РІРёР»Рѕ РґРІСѓС…СЃС‚РѕСЂРѕРЅРЅРµР№ СЃРёРЅС…СЂРѕРЅРёР·Р°С†РёРё: РїСЂР°РІРєРё docs в†” Knowledge-Base.


---

## рџ—єпёЏ РљР°СЂС‚Р° РєРѕРЅС‚РµРєСЃС‚Р° (Context Map & Navigation)

```mermaid
graph TD
    INDEX["рџ“„ index.md (Р“Р»Р°РІРЅС‹Р№ РґРёСЃРїРµС‚С‡РµСЂ)"]
    
    subgraph "01 Architecture"
        ARCH_BE["[[backend-architecture|Р‘СЌРєРµРЅРґ Рё CMS]]"]
        ARCH_FE["[[frontend-architecture|Р¤СЂРѕРЅС‚РµРЅРґ Рё UI]]"]
        ARCH_AI["[[ai-agent-architecture|AI-Р°РіРµРЅС‚С‹ Рё РџСЂРѕС‚РѕРєРѕР»С‹]]"]
        ARCH_OPS["[[devops-infra-architecture|РРЅС„СЂР°СЃС‚СЂСѓРєС‚СѓСЂР° Рё CI/CD]]"]
        ARCH_AUDIT["[[ai-audit-architecture|AI Audit MVP]]"]
        ARCH_DEPLOY["[[deployment|Deployment journal]]"]
        ARCH_PLAY["[[playbook|Playbook РїСЂРѕРґСѓРєС‚Р°]]"]
        ARCH_AIVIS["[[ai-visibility|AI Visibility]]"]
        ARCH_RAG["[[rag-evaluation-guide|РћС†РµРЅРєР° Рё РѕРїС‚РёРјРёР·Р°С†РёСЏ RAG]]"]
        ARCH_SPRING["[[spring-boot-oauth2-architecture|Spring Boot РЎС‚РµРє]]"]
        ARCH_GOOGLE["[[google-oauth2-integration-guide|Google OAuth2 OIDC]]"]
        ARCH_X["[[x-oauth2-integration-guide|X (Twitter) OAuth2 PKCE]]"]
        ARCH_EMAIL["[[email-service-cid-analysis|Email Service CID]]"]
        ARCH_LEGAL["[[compliance-legal-architecture|РљРѕРјРїР»Р°РµРЅСЃ, IS 5568 & GDPR]]"]
        ARCH_TARGO["[[targo-production-plan|Targo & WhatsApp]]"]
        ARCH_RLS["[[supabase-rls|Supabase RLS]]"]
    end
    
    subgraph "02 Design Tokens"
        TOK_FIGMA["[[figma-design-pipeline|Figma Design Pipeline]]"]
        TOK_COL["[[foundations-colors|Р¦РІРµС‚Р° Рё РўРµРјС‹]]"]
        TOK_TYP["[[foundations-typography|РўРёРїРѕРіСЂР°С„РёРєР° Рё RTL]]"]
        TOK_GEO["[[foundations-geometry|РЎРµС‚РєРё Рё РћС‚СЃС‚СѓРїС‹]]"]
        TOK_ANI["[[foundations-animations|GSAP Рё РђРЅРёРјР°С†РёРё]]"]
        TOK_HERO["[[hero-motion|Hero Motion]]"]
        TOK_CMP["[[components-rules|РљРѕРјРїРѕРЅРµРЅС‚С‹ UI]]"]
    end
    
    subgraph "03 Agent Skills"
        SKILL_PROMPT["[[prompt-engineering-rules|РџСЂРѕРјРїС‚-РёРЅР¶РёРЅРёСЂРёРЅРі]]"]
        SKILL_MCP["[[mcp-agent-skill|MCP РџСЂРѕС‚РѕРєРѕР»]]"]
        SKILL_ACP["[[acp-commerce-skill|ACP РљРѕРјРјРµСЂС†РёСЏ]]"]
        SKILL_MD["[[markdown-negotiation-skill|Markdown Negotiation]]"]
        SKILL_QA["[[qa-website-auditor-skill|QA Website Auditor]]"]
        SKILL_L1["[[prompt-add-vercel-agent-readiness|Vercel L1 Agent Checks]]"]
        SKILL_CMS["[[cms-content-agent-skill|CMS РљРѕРЅС‚РµРЅС‚-Р°РіРµРЅС‚С‹]]"]
        SKILL_IMPORT["[[import-project|Import Project]]"]
        SKILL_SPRING["[[spring-oauth2-auth-skill|Spring OAuth2 Р’РЅРµРґСЂРµРЅРёРµ]]"]
        SKILL_EMAIL["[[transactional-email-skill|Email СЃ CID Inline]]"]
        SKILL_SEO["[[seo-analytics-agent-skill|SEO Analytics Agent]]"]
        SKILL_GIT["[[obsidian-git-sync-rule|Obsidian Git Sync]]"]
    end
    
    subgraph "04 Templates & Cases"
        TPL_BRIEF["[[brief-yaml-cms-template|CMS Field Map & Brief]]"]
        TPL_FIELDMAP["[[cms-field-map|CMS Field Map (repo)]]"]
        TPL_SPRING["[[oauth2-spring-config-template|Spring OAuth2 Config]]"]
        TPL_CASE["[[portfolio-case-template|РЁР°Р±Р»РѕРЅ РєРµР№СЃР° YAML]]"]
        TPL_CASEMD["[[case-study-template|РЁР°Р±Р»РѕРЅ РєРµР№СЃР° MD]]"]
        TPL_SERV["[[service-page-template|РЁР°Р±Р»РѕРЅ СѓСЃР»СѓРіРё]]"]
        TPL_ORD["[[solution-order-template|РЁР°Р±Р»РѕРЅ С‚Р°СЂРёС„Р°]]"]
        TPL_SEO["[[seo-metadata-template|РЁР°Р±Р»РѕРЅ SEO/Schema]]"]
        TPL_LEG["[[legal-page-template|РЁР°Р±Р»РѕРЅ Legal/WCAG]]"]
        TPL_CLIENT["[[client-brief-design-dev|РљР»РёРµРЅС‚СЃРєРёР№ Р±СЂРёС„]]"]
        TPL_AGREE["[[software-development-agreement-israel|Р”РѕРіРѕРІРѕСЂ РЅР° СЂР°Р·СЂР°Р±РѕС‚РєСѓ]]"]
        CASE_AUDIT["[[ai-audit-agent|РљРµР№СЃ: AI Audit]]"]
        CASE_CONSULT["[[ai-consultant|РљРµР№СЃ: AI Consultant]]"]
    end
    
    subgraph "05 Glossary & FAQ"
        PIT["[[pitfalls-troubleshooting|РљР°С‚Р°Р»РѕРі С„Р°РєР°РїРѕРІ (PIT-001..064)]]"]
        RAG["[[rag-index|RAG Index / ingest map]]"]
        GLOSS["[[technical-glossary|Р“Р»РѕСЃСЃР°СЂРёР№ С‚РµСЂРјРёРЅРѕРІ]]"]
        FAQ["[[architectural-faq|РђСЂС…РёС‚РµРєС‚СѓСЂРЅС‹Р№ FAQ]]"]
    end

    INDEX --> ARCH_BE & ARCH_FE & ARCH_AI & ARCH_OPS & ARCH_AUDIT & ARCH_DEPLOY & ARCH_PLAY & ARCH_AIVIS & ARCH_RAG & ARCH_SPRING & ARCH_GOOGLE & ARCH_X & ARCH_EMAIL & ARCH_LEGAL & ARCH_TARGO & ARCH_RLS
    INDEX --> TOK_FIGMA & TOK_COL & TOK_TYP & TOK_GEO & TOK_ANI & TOK_HERO & TOK_CMP
    INDEX --> SKILL_PROMPT & SKILL_MCP & SKILL_ACP & SKILL_MD & SKILL_QA & SKILL_L1 & SKILL_CMS & SKILL_IMPORT & SKILL_SPRING & SKILL_EMAIL & SKILL_SEO & SKILL_GIT
    INDEX --> TPL_BRIEF & TPL_FIELDMAP & TPL_SPRING & TPL_CASE & TPL_CASEMD & TPL_SERV & TPL_ORD & TPL_SEO & TPL_LEG & TPL_CLIENT & TPL_AGREE & CASE_AUDIT & CASE_CONSULT
    INDEX --> PIT & RAG & GLOSS & FAQ
```

---

## рџ“‚ РЎС‚СЂСѓРєС‚СѓСЂР° СЂР°Р·РґРµР»РѕРІ

### [[01-Architecture/backend-architecture|рџ“Ѓ 01-Architecture (РђСЂС…РёС‚РµРєС‚СѓСЂР° СЃРёСЃС‚РµРј)]]
- **[[rag-evaluation-guide|rag-evaluation-guide.md]]** вЂ” РљРѕРјРїР»РµРєСЃРЅС‹Р№ СЂРµРіР»Р°РјРµРЅС‚ С‚РµСЃС‚РёСЂРѕРІР°РЅРёСЏ, РјРµС‚СЂРёРє РєР°С‡РµСЃС‚РІР° (Ragas, Phoenix), СѓСЃС‚СЂР°РЅРµРЅРёСЏ СѓР·РєРёС… РјРµСЃС‚ (Lost in the Middle, Chunking, Re-ranking) Рё E2E-РѕС†РµРЅРєРё RAG-РїР°Р№РїР»Р°Р№РЅРѕРІ.
- **[[google-oauth2-integration-guide|google-oauth2-integration-guide.md]]** вЂ” РџРѕР»РЅРѕРµ СЂСѓРєРѕРІРѕРґСЃС‚РІРѕ РїРѕ РЅР°СЃС‚СЂРѕР№РєРµ Google OAuth 2.0 (OIDC, scopes `openid,profile,email`, `DefaultOAuth2UserService`, cookies `same-site=none`).
- **[[x-oauth2-integration-guide|x-oauth2-integration-guide.md]]** вЂ” РџРѕР»РЅРѕРµ СЂСѓРєРѕРІРѕРґСЃС‚РІРѕ РїРѕ РЅР°СЃС‚СЂРѕР№РєРµ X (Twitter) OAuth 2.0 (PKCE S256, Code Verifier, `client_secret_basic`, РєР°СЃС‚РѕРјРЅС‹Р№ `TwitterOAuth2UserService` РґР»СЏ РїР°СЂСЃРёРЅРіР° `{ "data": { ... } }`, РіРµРЅРµСЂР°С†РёСЏ РІРёСЂС‚СѓР°Р»СЊРЅРѕРіРѕ `username@x.local`).
- **[[email-service-cid-analysis|email-service-cid-analysis.md]]** вЂ” Р“Р»СѓР±РѕРєРёР№ Р°РЅР°Р»РёР· РѕС‚РїСЂР°РІРєРё HTML-РїРёСЃРµРј СЃ inline-РІР»РѕР¶РµРЅРёСЏРјРё (Content-ID `cid:xxx`), РїРѕС‡РµРјСѓ РїРѕС‡С‚РѕРІРёРєРё Р±Р»РѕРєРёСЂСѓСЋС‚ base64 Рё РІРЅРµС€РЅРёРµ URL, СЃР±РѕСЂРєР° `emailBody` Рё `inlineImages` РІ `OrderController`.
- **[[spring-boot-oauth2-architecture|spring-boot-oauth2-architecture.md]]** вЂ” РЎРІРѕРґРЅР°СЏ Р°СЂС…РёС‚РµРєС‚СѓСЂР° Spring Boot 3 + Spring Security 6 СЃ PKCE, СЃРµСЃСЃРёСЏРјРё Р·Р° Nginx Рё С‚СЂР°РЅР·Р°РєС†РёРѕРЅРЅС‹Рј РїРѕС‡С‚РѕРІС‹Рј СЃРµСЂРІРёСЃРѕРј.
- **[[backend-architecture|backend-architecture.md]]** вЂ” Payload CMS 3.0, PostgreSQL/Drizzle, РјСѓР»СЊС‚РёСЏР·С‹С‡РЅС‹Рµ СЃС…РµРјС‹ (EN/RU/HE), REST/GraphQL API, On-Demand Revalidation.
- **[[frontend-architecture|frontend-architecture.md]]** вЂ” Next.js 15 App Router, React 19, TypeScript strict, Tailwind CSS, СЃС‚СЂРµСЃСЃ-С‚РµСЃС‚РёСЂРѕРІР°РЅРёРµ РґР»РёРЅС‹ СЃС‚СЂРѕРє (+40%), Р»РѕРіРёС‡РµСЃРєРёРµ СЃРІРѕР№СЃС‚РІР° (RTL).
- **[[ai-agent-architecture|ai-agent-architecture.md]]** вЂ” MCP (Model Context Protocol), ACP (Agentic Commerce Protocol), `llms.txt`, Markdown Content Negotiation, Schema.org.
- **[[devops-infra-architecture|devops-infra-architecture.md]]** вЂ” Vercel Serverless, Supabase Postgres, Cloudflare WAF/DNS/Rate Limiting, Hostinger SMTP, Docker, GitHub Actions CI/CD.
- **[[ai-audit-architecture|ai-audit-architecture.md]]** вЂ” AI Audit MVP: R2, contact-submissions, `/audit/report/[id]`, n8n/worker Р±РµР· РѕРїР»Р°С‚С‹. Synced from `docs/architecture/ai-audit-architecture.md`.
- **[[grow-audit-checkout|grow-audit-checkout.md]]** вЂ” РџР»Р°РЅ РѕРїР»Р°С‚С‹ Р°СѓРґРёС‚Р° СЃ РЅСѓР»СЏ: Morning (С‚РµСЂРјРёРЅР°Р» Рё Ч§Ч‘ЧњЧ”), Grow (VAT Exempt, РїСЂРѕРІРµСЂРєР° СЃР°Р№С‚Р°), Make (РґРІР° СЃС†РµРЅР°СЂРёСЏ), Р·Р°С‚РµРј СЃР°Р№С‚. Synced from `docs/architecture/grow-audit-checkout.md`.
- **[[payplus-audit-checkout|payplus-audit-checkout.md]]** вЂ” Legacy PayPlus Р°РґР°РїС‚РµСЂ РѕРїР»Р°С‚С‹ С‡РµРєР°СѓС‚Р° Р°СѓРґРёС‚Р° (Grow РёСЃРїРѕР»СЊР·СѓРµС‚СЃСЏ РєР°Рє РѕСЃРЅРѕРІРЅРѕР№). Synced from `docs/architecture/payplus-audit-checkout.md`.
- **[[r2-audit-storage|r2-audit-storage.md]]** вЂ” R2 bucket `erythro-audit-reports`, env vars, API token. Synced from `docs/architecture/r2-audit-storage.md`.
- **[[r2-media-storage|r2-media-storage.md]]** вЂ” Payload media on R2 (`erythro-media`), public URL, Blob cutover. Synced from `docs/architecture/r2-media-storage.md`.
- **[[gemini-consultant|gemini-consultant.md]]** вЂ” РР-РєРѕРЅСЃСѓР»СЊС‚Р°РЅС‚ РЅР° Gemini Flash + CMS KB. Synced from `docs/architecture/gemini-consultant.md`.
- **[[grok-bot-consultant|grok-bot-consultant.md]]** вЂ” РџСѓР±Р»РёС‡РЅС‹Р№ РєРѕРЅСЃСѓР»СЊС‚Р°РЅС‚ РЅР° СЃР°Р№С‚Рµ (xAI API / Grok), РїРѕС‚РѕРє РґР°РЅРЅС‹С… Рё С†РµРЅС‹. Synced from `docs/architecture/grok-bot-consultant.md`.
- **[[erythro-knowledge-base|erythro-knowledge-base.md]]** вЂ” РЎРЅРёРјРѕРє Р±Р°Р·С‹ Р·РЅР°РЅРёР№ Р»РёРґРѕРіРµРЅРµСЂР°С†РёРё Рё С†РµРЅ live CMS РґР»СЏ РР-РєРѕРЅСЃСѓР»СЊС‚Р°РЅС‚Р°. Synced from `docs/consultant/erythro-knowledge-base.md`.
- **[[ai-token-spend|ai-token-spend.md]]** вЂ” РљР°СЂС‚Р° AI token spend: COGS/OPEX, РєР»СЋС‡Рё, РјРµСЃСЏС‡РЅС‹Р№ СЂРёС‚СѓР°Р». Synced from `docs/architecture/ai-token-spend.md`.
- **[[audit-rate-limiting|audit-rate-limiting.md]]** вЂ” Р›РёРјРёС‚РёСЂРѕРІР°РЅРёРµ РїРѕРІС‚РѕСЂРЅРѕРіРѕ Р°СѓРґРёС‚Р° (5 РґРЅРµР№) Рё СѓС‡РµС‚ IP. Synced from `docs/architecture/audit-rate-limiting.md`.
- **[[audit-page-cms-schema|audit-page-cms-schema.md]]** вЂ” CMS СЃС…РµРјР° Рё Р»РѕРєР°Р»РёР·Р°С†РёСЏ СЃС‚СЂР°РЅРёС†С‹ Р°СѓРґРёС‚Р°.
- **[[google-ads-conversions|google-ads-conversions.md]]** вЂ” РџРѕСЂСЏРґРѕРє РїРѕРґРєР»СЋС‡РµРЅРёСЏ РєРѕРЅРІРµСЂСЃРёР№ Google Ads (Consent Mode v2 default denied, AnalyticsBootstrap, CSP, СЃРµСЂРІРµСЂРЅС‹Рµ С‚СЂРёРіРіРµСЂС‹ gtag). Synced from `docs/architecture/google-ads-conversions.md`.
- **[[targo-production-plan|targo-production-plan.md]]** вЂ” РџСЂРѕРёР·РІРѕРґСЃС‚РІРµРЅРЅС‹Р№ РїР»Р°РЅ Targo (РјСѓР»СЊС‚РёСЏР·С‹С‡РЅС‹Р№ Р°СЃСЃРёСЃС‚РµРЅС‚ РґР»СЏ Р±РёР·РЅРµСЃР°, WhatsApp РјРѕСЃС‚, FastAPI СЏРґСЂРѕ, SQLite WAL). Synced from `docs/architecture/targo-production-plan.md`.
- **[[whatsapp-data-privacy|whatsapp-data-privacy.md]]** вЂ” РљРѕРЅС„РёРґРµРЅС†РёР°Р»СЊРЅРѕСЃС‚СЊ Рё С…СЂР°РЅРµРЅРёРµ РїРµСЂРµРїРёСЃРєРё WhatsApp (Р—Р°РєРѕРЅ Рѕ Р·Р°С‰РёС‚Рµ С‡Р°СЃС‚РЅРѕР№ Р¶РёР·РЅРё 5741-1981, РџРѕРїСЂР°РІРєР° в„–13, СЂРѕР»Рё РєРѕРЅС‚СЂРѕР»РµСЂР°/РґРµСЂР¶Р°С‚РµР»СЏ Р±Р°Р·С‹, РїСЂР°РІРёР»Р° РїРµСЂРµРґР°С‡Рё РґР°РЅРЅС‹С…).
- **[[whatsapp-translator|whatsapp-translator.md]]** вЂ” РљРѕРЅС‚СѓСЂ WhatsApp Cloud API в†” Telegram / PWA РѕРїРµСЂР°С‚РѕСЂР° (РїРµСЂРµРІРѕРґС‡РёРє РїРµСЂРµРїРёСЃРєРё, Caddy, РІРµР±С…СѓРєРё). Synced from `docs/infrastructure/whatsapp-translator.md`.
- **[[whatsapp-tech-provider|whatsapp-tech-provider.md]]** вЂ” Р§РµРєР»РёСЃС‚ Tech Provider РґР»СЏ РѕРЅР±РѕСЂРґРёРЅРіР° РЅРѕРјРµСЂРѕРІ РєР»РёРµРЅС‚РѕРІ (Embedded Signup v4, РІРµСЂРёС„РёРєР°С†РёСЏ Р±РёР·РЅРµСЃР° РІ Meta). Synced from `docs/infrastructure/whatsapp-tech-provider.md`.
- **[[tech-circuit|tech-circuit.md]]** вЂ” РРЅР¶РµРЅРµСЂРЅС‹Р№ РєРѕРЅС‚СѓСЂ С‚РµС…РїРѕРґРґРµСЂР¶РєРё (`tech.erythro.ai`, С‚РёРєРµС‚С‹ СѓСЃС‚Р°РЅРѕРІРѕРє, TOTP, SQLite master). Synced from `docs/infrastructure/tech-circuit.md`.
- **[[backups|backups.md]]** вЂ” Р РµРіР»Р°РјРµРЅС‚ Рё Р°СЂС…РёС‚РµРєС‚СѓСЂР° С€РёС„СЂРѕРІР°РЅРЅС‹С… Р±СЌРєР°РїРѕРІ SQLite (AES-256-GCM, WAL online backup, СЂРѕС‚Р°С†РёСЏ). Synced from `docs/infrastructure/backups.md`.
- **[[b2bridge-checklist|b2bridge-checklist.md]]** вЂ” Р§РµРєР»РёСЃС‚ РґР°Р»СЊРЅРµР№С€РёС… РґРµР№СЃС‚РІРёР№ Рё РІРЅРµРґСЂРµРЅРёСЏ Targo / B2Bridge. Synced from `docs/infrastructure/b2bridge-checklist.md`.
- **[[vps-n8n|vps-n8n.md]]** вЂ” РҐРѕСЃС‚РёРЅРі n8n РЅР° VPS Р·Р° Caddy, РїРёРЅ РІРµСЂСЃРёРё (2.41.4), cron Рё РІРѕСЂРєС„Р»РѕСѓ.
- **[[n8n-audit-reconcile|n8n-audit-reconcile.md]]** вЂ” Р РµРєРѕРЅСЃРёР»СЏС†РёСЏ Р·Р°РІРёСЃС€РёС… Р°СѓРґРёС‚РѕРІ (Vercel Cron + n8n). Synced from `docs/infrastructure/n8n-audit-reconcile.md`.
- **[[n8n-email-autoresponder|n8n-email-autoresponder.md]]** вЂ” n8n email autoresponder РґР»СЏ order@ Рё team@. Synced from `docs/infrastructure/n8n-email-autoresponder.md`.
- **[[caddy-dns-audit-worker|caddy-dns-audit-worker.md]]** вЂ” Caddy reverse proxy Рё DNS РєРѕРЅС„РёРіСѓСЂР°С†РёСЏ РґР»СЏ Audit Worker.
- **[[brand-entity-promotion|brand-entity-promotion.md]]** вЂ” РЎС‚СЂР°С‚РµРіРёСЏ РїРѕР·РёС†РёРѕРЅРёСЂРѕРІР°РЅРёСЏ Р±СЂРµРЅРґР° Рё СЃСѓС‰РЅРѕСЃС‚РµР№ РІ AI-РїРѕРёСЃРєРµ Рё РєР°С‚Р°Р»РѕРіР°С….
- **[[deployment|deployment.md]]** вЂ” Р–СѓСЂРЅР°Р» РґРµРїР»РѕСЏ erythro.ai (Vercel, Blob, РјРёРіСЂР°С†РёРё, CI, DNS, РїРѕС‡С‚Р°, AI Crawl Control). Synced from `docs/DEPLOYMENT.md`.
- **[[vps-firewall-cloudflare-access|vps-firewall-cloudflare-access.md]]** вЂ” UFW baseline, Cloudflare Access `/admin*`, DMARC quarantineв†’reject checklist. Synced from `docs/infrastructure/vps-firewall-cloudflare-access.md`.
- **[[vps-docker-ports|vps-docker-ports.md]]** вЂ” Docker publish rules (no `0.0.0.0`). Synced from `docs/infrastructure/vps-docker-ports.md`.
- **[[supabase-rls|supabase-rls.md]]** — Postgres RLS: grants vs policies, когда обязательно, режим Payload/server-only vs Data API. Synced from `docs/architecture/supabase-rls.md`.
- **[[supabase-data-api-grants|supabase-data-api-grants.md]]** вЂ” РЎ 2026-10-30 РЅРѕРІС‹Рµ С‚Р°Р±Р»РёС†С‹ `public` Р±РµР· СЏРІРЅРѕРіРѕ GRANT РЅРµРґРѕСЃС‚СѓРїРЅС‹ Data API. РќР° С‚Р°Р±Р»РёС†С‹ Payload СЌС‚Рё GRANT РЅРµ СЃС‚Р°РІРёС‚СЊ. Synced from `docs/infrastructure/supabase-data-api-grants.md`.
- **[[PLAN-deferred|PLAN-deferred.md]]** вЂ” Deferred hardening / perf backlog (DMARC reject ~2026-09-13). Synced from `docs/PLAN-deferred.md`.
- **[[playbook|playbook.md]]** вЂ” РџСЂРѕРґСѓРєС‚РѕРІС‹Р№ playbook Рё С…СЂРѕРЅРѕР»РѕРіРёСЏ v1 (С„Р°Р·С‹ AвЂ“F, i18n, GSAP, CMS). Synced from `docs/PLAYBOOK.md`.
- **[[ai-visibility|ai-visibility.md]]** вЂ” AI visibility pack: llms.txt, MCP, schema, Content Signals, markdown negotiation. Synced from `docs/AI_VISIBILITY.md`.
- **[[meta-ads-campaigns-analysis-2026-10-30|audit/meta-ads-campaigns-analysis-2026-10-30.md]]** — Анализ рекламных кампаний Meta (Facebook / Instagram) и лидогенерации (30.10.2026).
- **[[bigcars-co-il-seo-analytics|audit/bigcars-co-il-seo-analytics.md]]** — Комплексный SEO и маркетинговый аудит: BigCars.co.il.
- **[[meta-pixel|architecture/meta-pixel.md]]** — Архитектура и настройка Meta Pixel, Conversions API, CSP и события Lead.
- **[[audit-ads-landing|architecture/audit-ads-landing.md]]** — Ads landing: AI Audit (fixed locale subdomains для Google Ads и Meta).
- **[[compliance-legal-architecture|compliance-legal-architecture.md]]** вЂ” РџРѕРїСЂР°РІРєР° в„–13 Рє Р·Р°РєРѕРЅСѓ Рѕ Р·Р°С‰РёС‚Рµ РєРѕРЅС„РёРґРµРЅС†РёР°Р»СЊРЅРѕСЃС‚Рё (РР·СЂР°РёР»СЊ), Р”РѕРєСѓРјРµРЅС‚ РѕРїСЂРµРґРµР»РµРЅРёСЏ Р±Р°Р·С‹ РґР°РЅРЅС‹С… (DBD), С€РёС„СЂРѕРІР°РЅРёРµ Р±СЌРєР°РїРѕРІ AES-256-GCM, IS 5568 / WCAG 2.1 AA, GDPR Рё EU AI Act.

### [[02-Design-Tokens/foundations-colors|рџ“Ѓ 02-Design-Tokens (Р”РёР·Р°Р№РЅ-СЃРёСЃС‚РµРјР°)]]
- **[[figma-design-pipeline|figma-design-pipeline.md]]** вЂ” 5-СЌС‚Р°РїРЅС‹Р№ СЂРµРіР»Р°РјРµРЅС‚ СЃРѕР·РґР°РЅРёСЏ РґРёР·Р°Р№РЅР° РІ Figma: РЎРµС‚РєРё (1920/1170/768/390), Р»РёРЅРµР№РєР° РѕС‚СЃС‚СѓРїРѕРІ 4/8px, Primitives, РЎРµРјР°РЅС‚РёРєР° С‚РµРј, РРєРѕРЅРєРё 24Г—24px, Auto-Layout, РР·РѕР»РёСЂРѕРІР°РЅРЅС‹Рµ РѕРІРµСЂР»РµРё, Definition of Done.
- **[[foundations-colors|foundations-colors.md]]** вЂ” РџР°Р»РёС‚СЂР° Primitives (Coal/Erythro/Gold), СЃРµРјР°РЅС‚РёС‡РµСЃРєРёРµ С‚РѕРєРµРЅС‹ (`--bg-primary`, `--bg-surface`), РїСЂРѕС†РµРґСѓСЂРЅС‹Р№ С€СѓРј 5%, С‚РµРјРЅР°СЏ/СЃРІРµС‚Р»Р°СЏ С‚РµРјР°.
- **[[foundations-typography|foundations-typography.md]]** вЂ” РЁСЂРёС„С‚РѕРІР°СЏ РјР°С‚СЂРёС†Р°: Inter (Latin/Cyrillic), Heebo (Hebrew/RTL), Roboto Mono (Data/Tech), С‚РёРїРѕРіСЂР°С„РёС‡РµСЃРєР°СЏ С€РєР°Р»Р°.
- **[[foundations-geometry|foundations-geometry.md]]** вЂ” РђРґР°РїС‚РёРІРЅР°СЏ 12-РєРѕР»РѕРЅРѕС‡РЅР°СЏ СЃРµС‚РєР° 1170px, РєРѕР»РѕРЅРєРё 70px, РіР°С‚С‚РµСЂС‹ 30px, РјР°СЃС€С‚Р°Р± РѕС‚СЃС‚СѓРїРѕРІ `space-xs..xl`, СЂР°РґРёСѓСЃС‹ СЃРєСЂСѓРіР»РµРЅРёР№.
- **[[foundations-animations|foundations-animations.md]]** вЂ” GSAP 3 + ScrollTrigger, Hero Interactive Canvas Sequence (110 РєР°РґСЂРѕРІ WebP), РїР»Р°РІРЅРѕСЃС‚СЊ СЃРѕСЃС‚РѕСЏРЅРёР№ `transition-all duration-300`.
- **[[hero-motion|hero-motion.md]]** вЂ” РЎРїРµС†РёС„РёРєР°С†РёСЏ Hero Motion (РєР°РґСЂС‹, pin, copy). Synced from `docs/HERO_MOTION.md`.
- **[[components-rules|components-rules.md]]** вЂ” РџСЂР°РІРёР»Р° СЃРѕР·РґР°РЅРёСЏ РєРѕРјРїРѕРЅРµРЅС‚РѕРІ: Navbar, Mobile Menu Overlay, Hero, Service Cards, Solutions, Portfolio, Contact Modal, SplashHost.

### [[03-Agent-Skills/prompt-engineering-rules|рџ“Ѓ 03-Agent-Skills (Р РµРіР»Р°РјРµРЅС‚С‹ AI-Р°РіРµРЅС‚РѕРІ)]]
- **[[prompt-engineering-rules|prompt-engineering-rules.md]]** вЂ” РЎС‚Р°РЅРґР°СЂС‚С‹ РїСЂРѕРјРїС‚РѕРІ РґР»СЏ РєРѕРґРёРЅРі-Р°СЃСЃРёСЃС‚РµРЅС‚РѕРІ (Antigravity, Cursor, Claude 3.7, GPT-4o), РїСЂРµРґРѕС‚РІСЂР°С‰РµРЅРёРµ РіР°Р»Р»СЋС†РёРЅР°С†РёР№.
- **[[HANDOFF|context/HANDOFF.md]]** вЂ” РџСЂРѕС‚РѕРєРѕР» СЌСЃС‚Р°С„РµС‚С‹ РјРµР¶РґСѓ Cursor, Claude, Gemini Рё Grok. РљР°РЅРѕРЅ: `erythro-ai/docs/context/HANDOFF.md`. Р¤Р°Р№Р» РїРѕСЃР»РµРґРЅРёС… РёР·РјРµРЅРµРЅРёР№ `NOW.md` РІ Р±Р°Р·Сѓ РЅРµ Р·РµСЂРєР°Р»РёС‚СЃСЏ.
- **[[mcp-agent-skill|mcp-agent-skill.md]]** вЂ” РЎРїРµС†РёС„РёРєР°С†РёСЏ SEP-1960 MCP, Discovery Manifest `/.well-known/mcp`, СЌРЅРґРїРѕРёРЅС‚ brand facts.
- **[[acp-commerce-skill|acp-commerce-skill.md]]** вЂ” РЎРїРµС†РёС„РёРєР°С†РёСЏ Agentic Commerce Protocol, `/.well-known/acp.json`, capability negotiation, Р·Р°РєР°Р·С‹ Рё С‡РµРєР°СѓС‚.
- **[[markdown-negotiation-skill|markdown-negotiation-skill.md]]** вЂ” Р РµР°Р»РёР·Р°С†РёСЏ `Accept: text/markdown`, РїРѕРґСЃС‡РµС‚ С‚РѕРєРµРЅРѕРІ `x-markdown-tokens`, РѕС‚РґР°С‡Р° С‡РёСЃС‚РѕРіРѕ С‚РµРєСЃС‚Р° РґР»СЏ LLM.
- **[[qa-website-auditor-skill|qa-website-auditor-skill.md]]** вЂ” РђРІС‚РѕРЅРѕРјРЅС‹Р№ QA-Р°СѓРґРёС‚РѕСЂ: ~60 РїСЂРѕРІРµСЂРѕРє (Mobile UX, RTL, Forms, SEO, Speed, Security, AI Readiness), РіРµРЅРµСЂР°С†РёСЏ РјРЅРѕРіРѕСЏР·С‹С‡РЅС‹С… PDF.
- **[[prompt-add-vercel-agent-readiness|prompt-add-vercel-agent-readiness.md]]** вЂ” РџСЂРѕРјРїС‚ РґР»СЏ Р°РіРµРЅС‚Р°: РґРѕР±Р°РІРёС‚СЊ РІ QA_Auditor РїСЂРѕРІРµСЂРєРё Vercel Level 1 (robots.txt, Sitemap, AI Crawler Rules, Content Signals, Markdown Negotiation).
- **[[cms-content-agent-skill|cms-content-agent-skill.md]]** вЂ” РђРІС‚РѕРЅРѕРјРЅС‹Р№ РєРѕРЅС‚РµРЅС‚-РјРµРЅРµРґР¶РјРµРЅС‚ С‡РµСЂРµР· n8n Рё CLI-СЃРєСЂРёРїС‚С‹, РёРјРїРѕСЂС‚ РєРµР№СЃРѕРІ РёР· РїР°РїРѕРє, СЃРµСЂРёР°Р»РёР·Р°С†РёСЏ Lexical.
- **[[import-project|import-project.md]]** вЂ” Pointer РЅР° `scripts/import-project` (brief.yaml в†’ CMS). Synced from `docs/IMPORT_PROJECT.md`.
- **[[spring-oauth2-auth-skill|spring-oauth2-auth-skill.md]]** вЂ” РџРѕС€Р°РіРѕРІР°СЏ РёРЅСЃС‚СЂСѓРєС†РёСЏ РїРѕ РЅР°СЃС‚СЂРѕР№РєРµ Spring Boot СЃ Google Рё X (Twitter) OAuth2 PKCE.
- **[[transactional-email-skill|transactional-email-skill.md]]** вЂ” Р РµРіР»Р°РјРµРЅС‚ РІРµСЂСЃС‚РєРё С‚СЂР°РЅР·Р°РєС†РёРѕРЅРЅС‹С… РїРёСЃРµРј СЃ РіР°СЂР°РЅС‚РёСЂРѕРІР°РЅРЅРѕР№ РїРѕРґРґРµСЂР¶РєРѕР№ РєР°СЂС‚РёРЅРѕРє С‡РµСЂРµР· CID (JavaMailSender / Nodemailer).
- **[[seo-analytics-agent-skill|seo-analytics-agent-skill.md]]** вЂ” РђРІС‚РѕРЅРѕРјРЅС‹Р№ Java SEO Analytics Agent: SerpApi, 24h РєСЌС€ (Redis+Caffeine), СЃС‚СЂРѕРіР°СЏ Р»РѕРєР°Р»РёР·Р°С†РёСЏ (RU/EN/HE), РїСЂРµРґРѕР±СЂР°Р±РѕС‚РєР° РґР°РЅРЅС‹С… РґР»СЏ LLM (-85% С‚РѕРєРµРЅРѕРІ), Gemini 3.6 Flash.
- **[[obsidian-git-sync-rule|obsidian-git-sync-rule.md]]** вЂ” РћР±СЏР·Р°С‚РµР»СЊРЅРѕРµ РїСЂР°РІРёР»Рѕ Рё РїСЂРѕС‚РѕРєРѕР» СЃРёРЅС…СЂРѕРЅРёР·Р°С†РёРё Р±Р°Р·С‹ Р·РЅР°РЅРёР№ СЃ GitHub СЂРµРїРѕР·РёС‚РѕСЂРёРµРј РїСЂРё Р»СЋР±С‹С… РёР·РјРµРЅРµРЅРёСЏС… (Agent rules + Obsidian Git plugin).

### [[04-Templates/portfolio-case-template|рџ“Ѓ 04-Templates (РЁР°Р±Р»РѕРЅС‹ Р°СЂС‚РµС„Р°РєС‚РѕРІ)]]
- **[[client-brief-design-dev|client-brief-design-dev.md]]** вЂ” РџРѕР»РЅС‹Р№ РєР»РёРµРЅС‚СЃРєРёР№ Р±СЂРёС„ РЅР° СЂР°Р·СЂР°Р±РѕС‚РєСѓ Рё UI/UX РґРёР·Р°Р№РЅ (РїР°СЃРїРѕСЂС‚ РїСЂРѕРµРєС‚Р°, С†РµР»Рё, Р¦Рђ, С„СѓРЅРєС†РёРѕРЅР°Р»СЊРЅС‹Рµ С‚СЂРµР±РѕРІР°РЅРёСЏ, РёРЅС‚РµРіСЂР°С†РёРё).
- **[[software-development-agreement-israel|software-development-agreement-israel.md]]** вЂ” Р”РѕРіРѕРІРѕСЂ РЅР° СЂР°Р·СЂР°Р±РѕС‚РєСѓ РџРћ (РР·СЂР°РёР»СЊ / РґРІСѓСЏР·С‹С‡РЅС‹Р№ EN-RU / Fixed Price / Milestones / IS 5568 / РџРѕРїСЂР°РІРєР° 13).
- **[[software-development-agreement-israel-ru|software-development-agreement-israel-ru.md]]** вЂ” Р”РѕРіРѕРІРѕСЂ РЅР° СЂР°Р·СЂР°Р±РѕС‚РєСѓ РџРћ (СЂСѓСЃСЃРєР°СЏ СЂРµРґР°РєС†РёСЏ РґР»СЏ Osek Patur / Eilat).
- **[[brief-yaml-cms-template|brief-yaml-cms-template.md]]** вЂ” РџРѕР»РЅР°СЏ РјР°С‚СЂРёС†Р° СЃРѕРѕС‚РІРµС‚СЃС‚РІРёСЏ РїРѕР»РµР№ РјРµР¶РґСѓ `brief.yaml` Рё PostgreSQL/Payload CMS.
- **[[cms-field-map|cms-field-map.md]]** вЂ” Field map РєРµР№СЃ в†’ Portfolio Projects (РёР· СЂРµРїРѕ). Synced from `docs/cases/CMS_FIELD_MAP.md`.
- **[[case-study-template|case-study-template.md]]** вЂ” Markdown-С€Р°Р±Р»РѕРЅ РєРµР№СЃР° СЃ РїР»РµР№СЃС…РѕР»РґРµСЂР°РјРё. Synced from `docs/cases/case-study-template.md`.
- **[[oauth2-spring-config-template|oauth2-spring-config-template.md]]** вЂ” Р“РѕС‚РѕРІС‹Рµ С€Р°Р±Р»РѕРЅС‹ `application.properties` Рё `SecurityConfig.java` РґР»СЏ Spring Boot OAuth2.
- **[[portfolio-case-template|portfolio-case-template.md]]** вЂ” РџРѕР»РЅС‹Р№ С€Р°Р±Р»РѕРЅ РєРµР№СЃР° РїРѕСЂС‚С„РѕР»РёРѕ (`brief.yaml`), СЃС‚СЂСѓРєС‚СѓСЂР° РјРµРґРёР°, СЃС‚РµРє, С‚РµР»Рѕ СЃС‚Р°С‚СЊРё.
- **[[service-page-template|service-page-template.md]]** вЂ” РЁР°Р±Р»РѕРЅ СЃС‚СЂР°РЅРёС†С‹ СѓСЃР»СѓРіРё (Hero, Summary, Offerings, РўР°СЂРёС„С‹, Р’РѕР·РјРѕР¶РЅРѕСЃС‚Рё, SEO).
- **[[solution-order-template|solution-order-template.md]]** вЂ” РЁР°Р±Р»РѕРЅ С‚Р°СЂРёС„РЅРѕРіРѕ РїР»Р°РЅР° Р·Р°РєР°Р·Р° (Р±Р°Р·РѕРІР°СЏ С†РµРЅР°, РїРµСЂРёРѕРґС‹, СЃРєРёРґРєРё, Р°РґРґРѕРЅС‹, РЅР°Р»РѕРіРё).
- **[[seo-metadata-template|seo-metadata-template.md]]** вЂ” РЁР°Р±Р»РѕРЅС‹ generateMetadata, OpenGraph, Twitter Cards, Canonical alternates, JSON-LD (Organization, WebSite, FAQPage).
- **[[legal-page-template|legal-page-template.md]]** вЂ” РЁР°Р±Р»РѕРЅС‹ СЋСЂРёРґРёС‡РµСЃРєРёС… РґРѕРєСѓРјРµРЅС‚РѕРІ: Privacy Policy, Terms of Service, Accessibility Statement (IS 5568 / WCAG 2.1 AA).

### рџ“Ѓ Cases (Р‘РѕРµРІС‹Рµ РєРµР№СЃС‹ Рё РІРЅРµРґСЂРµРЅРёСЏ)
- **[[ai-audit-agent|cases/ai-audit-agent.md]]** вЂ” Р‘РѕРµРІРѕР№ РєРµР№СЃ: РР-Р°СѓРґРёС‚РѕСЂ СЃР°Р№С‚Р° РЅР° erythro.ai (QA-Р°РіРµРЅС‚, Playwright, PageSpeed, Gemini, R2, A44 РѕС‚С‡С‘С‚).
- **[[ai-consultant|cases/ai-consultant.md]]** вЂ” Р‘РѕРµРІРѕР№ РєРµР№СЃ: РР-РєРѕРЅСЃСѓР»СЊС‚Р°РЅС‚ РЅР° erythro.ai (Gemini 3.6 Flash, Vercel AI SDK, live CMS KB).

### [[05-Glossary-FAQ/pitfalls-troubleshooting|рџ“Ѓ 05-Glossary-FAQ (Р‘Р°Р·Р° РёРЅС†РёРґРµРЅС‚РѕРІ Рё FAQ)]]
- **[[pitfalls-troubleshooting|pitfalls-troubleshooting.md]]** вЂ” РљР°С‚Р°Р»РѕРі РёР·РІРµСЃС‚РЅС‹С… С„Р°РєР°РїРѕРІ (`PIT-001` вЂ“ `PIT-064`): СЃРёРјРїС‚РѕРјС‹, РїСЂРёС‡РёРЅС‹, РїСЂРѕРІРµСЂРµРЅРЅС‹Рµ СЂРµС€РµРЅРёСЏ Рё РїСЂР°РІРёР»Р° РїСЂРѕС„РёР»Р°РєС‚РёРєРё. Synced from `docs/PITFALLS.md`.
- **[[rag-index|rag-index.md]]** вЂ” Ingest map РґР»СЏ RAG (chunking, tags, routing). Synced from `docs/RAG_INDEX.md`.
- **[[technical-glossary|technical-glossary.md]]** вЂ” РђР»С„Р°РІРёС‚РЅС‹Р№ РіР»РѕСЃСЃР°СЂРёР№ С‚РµСЂРјРёРЅРѕРІ: MCP, ACP, RAG, ISR, Drizzle, Lexical, GSAP, TTFB, CSP, HSTS, WCAG.
- **[[architectural-faq|architectural-faq.md]]** вЂ” РћС‚РІРµС‚С‹ РЅР° С‡Р°СЃС‚С‹Рµ Р°СЂС…РёС‚РµРєС‚СѓСЂРЅС‹Рµ РІРѕРїСЂРѕСЃС‹ РґР»СЏ Р±С‹СЃС‚СЂРѕРіРѕ СЃС‚Р°СЂС‚Р° РЅРѕРІС‹С… РєР»РёРµРЅС‚СЃРєРёС… РїСЂРѕРµРєС‚РѕРІ.
- **[[vercel-agent-readiness|vercel-agent-readiness.md]]** вЂ” Р РµРіР»Р°РјРµРЅС‚ Рё С‡РµРє-Р»РёСЃС‚ РіРѕС‚РѕРІРЅРѕСЃС‚Рё РїСЂРѕРµРєС‚Р° Рє РІР·Р°РёРјРѕРґРµР№СЃС‚РІРёСЋ СЃ AI-Р°РіРµРЅС‚Р°РјРё РїРѕ СЃС‚Р°РЅРґР°СЂС‚Р°Рј Vercel Level 1.
