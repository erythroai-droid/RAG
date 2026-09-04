# Caddy + DNS + Audit Worker (VPS)

Synced from `erythro-ai/docs/infrastructure/caddy-dns-audit-worker.md`.

## DNS (Cloudflare)

| Type | Name | Content | Proxy |
|---|---|---|---|
| A | `n8n` | `46.202.155.56` | **DNS only** |
| A | `agent-api` | `46.202.155.56` | **DNS only** |

## Stack on VPS

`proxy_network`: Caddy → n8n:5678, audit_agent_worker:8080

Repo: `infra/caddy`, `infra/n8n`, `services/audit-agent`

See [[vps-n8n]] · [[ai-audit-architecture]].
