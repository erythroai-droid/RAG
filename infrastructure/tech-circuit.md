# Engineer circuit (tech/)

Status 2026-10-11: Steps 1 (backend), 3 (engineer PWA `tech/web`) and 2 (tickets in the installation core, `core/support.py`, contract in `docs/whatsapp-translator.md`) written, not deployed. Decisions of the user, 2026-10-11:
the installation polls the circuit (no inbound port at the client); the engineer signs in with a password
and a one-time TOTP code; the engineer PWA is its own app in `tech/web`.

## What it is

A separate service of Erythro (`uvicorn tech.app:app`, port 8100, not published on the host, later
`tech.erythro.ai` through Caddy). It owns `support_master.db` (SQLite, WAL, migrations in `tech/migrations`)
and is the only writer. It receives tickets from client installations and gives one engineer a live folder per ticket.
A ticket holds only what the client wrote in the support form and the engineer's replies, never the client's
conversations with its customers.

## Tables

`installations` (id = `INSTALLATION_ID`, SHA-256 of the key), `tickets` (`#TICK-0001` = id padded to 4 digits,
unique per `(installation_id, local_id)`), `ticket_messages` (side `client` or `engineer`), `installation_events`
(feed that installations poll), `support_incidents` (`analyzed_by_ai` = 0 until module A reads it), `engineers`
(scrypt hash, TOTP secret, last used step), `engineer_sessions`, `push_subscriptions`.

## Installation API (`Authorization: Bearer tk_...`)

| Call | Meaning |
|---|---|
| `POST /v1/tickets` `{local_id, subject, body, contact_name, contact_phone, language, app_version}` | New ticket with its first message. Resending the same `local_id` returns the stored ticket. 10 new tickets per hour per installation. -> `{number, status}` |
| `POST /v1/tickets/{ticket_local_id}/messages` `{local_id, body}` | Client message on its ticket; stored once per `local_id`; reopens a closed ticket. 30 per minute. 404 for an unknown ticket (also another installation's). |
| `GET /v1/events?after=<cursor>&wait=<0..25>` | Long poll: engineer replies and status changes after the cursor, oldest first. -> `{events: [{id, kind: message\|status, ticket_local_id, number, message?, status?}], cursor}`. The installation stores the cursor and sends it next time. |

Keys are issued by `python -m tech.admin add-installation <INSTALLATION_ID> --name ...` and shown once; only the hash is kept.
`disable-installation` revokes a key.

## Engineer API (`/internal/v1`, from the tech web container only)

`X-Internal-Token` (`TECH_INTERNAL_TOKEN`) on every call; a request with `X-Forwarded-For` (came through Caddy) gets 404.
Engineer calls also carry `Authorization: Bearer <session token>`; the web app keeps the token in an httpOnly cookie.

- `POST /auth/login {login, password, code}`: password and TOTP code, one error for any failure, 10 attempts per 10 minutes per login and per `X-Client-IP`. A code works once (time step stored, compare-and-set). Session 30 days, sliding. `POST /auth/logout`, `GET /auth/me`.
- `GET /tickets?status=&installation=&q=`, `GET /tickets/{id}` (opening reads it), `POST /tickets/{id}/messages {body}`, `POST /tickets/{id}/status {status: open|closed}`, `GET /installations`.
- `GET /events` SSE (`ready`, `ticket {ticket_id}`, heartbeat comment every 20 s).
- `GET /push/key`, `POST /push/subscribe`, `POST /push/unsubscribe`: Web Push to the engineer's phone (VAPID keys as in the core, `python -m core.admin vapid-keys`). A new ticket or client message sends a push.

## Administration

`python -m tech.admin set-engineer <login> [--name ...] [--new-totp]` (password from the terminal; the TOTP secret and `otpauth://` URI are shown once, at creation or with `--new-totp`),
`add-installation`, `disable-installation`, `list-installations`. Settings: `TECH_DB_PATH`, `TECH_INTERNAL_TOKEN`, `TECH_PUBLIC_URL`.

## Diagnostics from a client

A client may attach a diagnostics report to a ticket (see "Diagnostics report" in `docs/whatsapp-translator.md`). It arrives as a message with `kind: "diagnostics"` (`POST /v1/tickets/{id}/messages`, `kind` is `text` or `diagnostics`, anything else is 422; migration `tech/migrations/0002_message_kind.sql`). The engineer PWA shows it as a folded block "Client diagnostics" with the text in monospace. The report is already cleaned by the installation's core.

## Engineer PWA (tech/web)

Next.js 15 / React 19 / TypeScript strict, own app and container (port 3100, not published; later `tech.erythro.ai` through Caddy). The browser talks only to `/api/v1/*` of the Next server (allow-list proxy, same-origin check on writes), which calls the service with `TECH_URL` and `TECH_INTERNAL_TOKEN`; the session token lives in the httpOnly cookie `tech_session`. Screens: sign-in (e-mail, password, one-time code), ticket list (Open/Closed, search, unread badge), ticket folder (client facts, messages, reply with Ctrl+Enter, close/reopen, call button `tel:` to the number in the ticket). Live: SSE refetches the list and the open folder; a slow poll is the safety net. A new client message plays a chime when sound is on (one tap unlocks audio, setting in `localStorage` `tech-sound`) and sends Web Push when the app is not in focus (`sw.js`; a tap opens `/?t=<id>`). Interface Russian or English by the browser language, no switcher. Local run: `scripts/dev_tech.py` (sample tickets, `--code` prints the current TOTP code) and `tech/web` (`.claude/launch.json`: `tech-dev`, `tech-web-dev`).

## Deployment (written 2026-10-11, NOT deployed)

Files: `tech/Dockerfile` (service; build context is the repository root), `tech/web/Dockerfile` (engineer PWA), `docker-compose.tech.yml` (own compose project `targo_tech`: containers `targo_tech` on :8100 and `targo_tech_web` on :3100, volume `targo_tech_data`, network `proxy_network`, no host ports), `.env.tech.example`, Caddy site `tech.erythro.ai` in `infra/Caddyfile.snippet`. `.env.tech` lives only on the VPS (gitignored, not shipped by `scripts/deploy_whatsapp_translator.py`) and holds only `TECH_INTERNAL_TOKEN`, `TECH_PUBLIC_URL` and the VAPID keys; the translator tokens never reach these containers. The deploy script ships `tech/` and `docker-compose.tech.yml` and, when the server already has `.env.tech`, also runs `docker compose -f docker-compose.tech.yml --env-file .env.tech up -d --build`; the Caddy site is added by hand.

Order on the VPS (see CHECKLIST, Stage 3): DNS record `tech` A 46.202.155.56 (DNS only); create `.env.tech`; append the Caddy block and reload; `docker compose -f docker-compose.tech.yml --env-file .env.tech up -d --build`; `docker exec -it targo_tech python -m tech.admin set-engineer <login>` (scan the TOTP secret); `docker exec targo_tech python -m tech.admin add-installation erythro-demo --name "Erythro demo"`; put `TECH_URL=http://targo_tech:8100` and the printed `TECH_INSTALLATION_KEY` into the translator `.env` and recreate it (`docker compose up -d`); check `https://tech.erythro.ai/login` and that ports 8100 and 3100 are not published. `support_master.db` must be part of the Stage 4 backups.

Checked locally in Docker on 2026-10-11 (a throwaway network and volume, removed afterwards): both images build (`tech/Dockerfile` from the repository root and `tech/web/Dockerfile`), the compose project starts and both containers become healthy; no host ports are published (8100 and 3100 do not answer from the host); the web container reaches the service by name (`targo_tech:8100`, a wrong internal token gives 403); the engineer is created with `tech.admin`, signs in through the web container with password and TOTP; `caddy validate` of `infra/Caddyfile.snippet` says "Valid configuration"; the core image still builds after the `.dockerignore` change. Not checked: Caddy in front with a real certificate, DNS, Web Push to a real phone, the image on the VPS (other CPU and Docker versions).

## Not yet (later steps of Stage 3)

Steps 4 and 5 are written (support panel in the client PWA; Dockerfile, compose, Caddy).
Magic Link, access log and cleaning of diagnostics belong to Stage 4.
