# WhatsApp translator (VPS)

Operator loop for ErythroAi WhatsApp. A customer writes the business number. The VPS translates the message into a Telegram card. The operator replies in that Telegram chat, and the reply is sent back on WhatsApp.

Source of the service: `C:\dev\Targo` (`api_service.py`, `core/`). This file is the production contract. The consultant translater (`translate.erythro.ai`, `translater_service:8090`) is a different process. Do not replace it with this container. The older IT-translator lab at `C:\agents\Translater` is not this container.

Business number: `+972 50-530-8305`, display name ErythroAi. The number is registered in Meta Cloud API. It must stay off the WhatsApp and WhatsApp Business apps on the phone. Opening it there detaches Cloud API.

## Layout

| | |
|---|---|
| Public URL | `https://wa.erythro.ai` |
| Container | `whatsapp_translator` |
| VPS path | `/home/whatsapp-translator` |
| Network | `proxy_network` (no host `ports:`) |
| Published paths | `/health`, `/webhook/*` |
| Internal only | `/translate/incoming`, `/translate/outgoing`, `/docs` |
| Gateway | Meta Cloud API, Graph version from `GRAPH_API_VERSION` (default `v25.0`, valid until 2028-07-29) |
| Webhook signature | `POST /webhook/whatsapp` checks `X-Hub-Signature-256` with `WHATSAPP_APP_SECRET`; wrong or missing signature → 403. Empty secret = check off (`/health` shows `webhook_signature_check`) |
| `/health` | also returns `last_webhook_at` (UTC of the last accepted webhook, `null` after a restart until the first one), `installation_id` and `database` (`sqlite` or `memory`) |
| Bot | Telegram `Translater_workerBot`, updates only from `TELEGRAM_ADMIN_CHAT_ID` |

DNS: `wa` A record → `46.202.155.56`, **DNS only** (grey cloud). Caddy HTTP-01 cannot issue a certificate through the orange cloud. Create the record with `node scripts/ensure-wa-dns.mjs` once `CLOUDFLARE_API_TOKEN` (Zone DNS Edit) is available.

Caddy block: `infra/caddy/Caddyfile`. Compose and image: `C:\dev\Targo\docker-compose.yml`. Volume `whatsapp_sessions` keeps `/data/app.db` (SQLite, with `-wal` and `-shm` files) and the former `/data/sessions.json`.

Do not publish host port 8000 (PIT-053). `curl -I --max-time 5 http://46.202.155.56:8000` must time out. A `200` means the port was published.

Do not run `n8n_whatsapp_telegram_workflow.json` together with this container. That workflow repeats the same loop. Do not run `whatsapp_orchestrator.py` long-polling while the Telegram webhook is set.

## Operator flow

1. Customer text arrives at `POST /webhook/whatsapp`.
2. The service translates it to Russian and sends one Telegram card to `TELEGRAM_ADMIN_CHAT_ID`.
3. The card always has a line `Оригинал`: the customer's text as received, not a paraphrase. The Russian translation is the next line. Telegram HTML makes the labels bold, the phone number monospace, and a Facebook source URL a link. Reply to that card with a quote.
4. The quote is translated into the customer's language and sent from `+972 50-530-8305`.

A plain Telegram message with no active dialog returns: no active WhatsApp dialog, reply to the customer's card. Searching the bot chat by the customer's phone number collects that person's cards. Each new customer message is a new card. The operator's quote and the "sent" confirmation sit under it.

State lives in `/data/app.db` (SQLite, since stage 1 of the Targo plan). Tables: `installation` (client id), `settings`, `license`, `contacts`, `conversations`, `messages`, `attachments`; migrations in `core/migrations/*.sql` run at start. Only the Python core opens the file (WAL, `synchronous=NORMAL`, `busy_timeout=5000`, `foreign_keys=ON`). Every incoming and outgoing message is a row: original, Russian text, what went to WhatsApp, status (`received`, `translated`, `untranslated`, `sent`, `delivered`, `read`, `failed`) and the Telegram card id. A reply to a card uses that card's message for the language and context.

`INSTALLATION_ID` (demo: `erythro-demo`, compose default) is written into the first row of `installation`. A different value refuses to start the core, so one client's `.env` cannot open another client's database. On first start `/data/sessions.json` is imported once (`settings.legacy_sessions_imported`); the file stays as the rollback path, but is no longer written.

## Languages

Incoming text is translated to Russian for the operator. The reply language follows the customer message and is only Hebrew, English, Arabic, or Russian. The list lives in `core/languages.py`.

- Hebrew original → operator writes Russian → customer receives Hebrew.
- English original → operator writes Russian → customer receives English.
- Arabic original → operator writes Russian → customer receives simple Modern Standard Arabic (added 2026-10-04).
- Russian original → the operator's text is sent unchanged.

Hebrew and Arabic replies get right-to-left marks at the end of lines that end in punctuation. The card shows the code as `HE`, `EN`, `AR` or `RU`.

Latin-script languages other than English (French, Spanish, German) are treated as English, so the reply goes out in English. Chinese and other scripts without Hebrew, Arabic, Cyrillic, or Latin are also labeled English. The original line on the card still shows the customer's text.

## Verbatim tag

A reply that starts with `!!` is sent exactly as written after the tag, in any language. The tag is stripped. `!!` alone is rejected with a short Telegram notice. Without the tag, Hebrew, English and Arabic customers always get a translation from Russian. Writing Hebrew, English or Arabic yourself, without `!!`, is passed through the translator as if it were Russian and can come out wrong.

The same tag works as a caption on a photo or file.

## Media

Customer voice note (`audio`) is downloaded and sent into the operator chat in the original, untranslated (OGG/Opus as a Telegram voice note, other audio as audio), then the card says «Голосовое сообщение, оригинал без перевода». In the PWA the message has a player (the file is fetched from Meta through `/attachments/{id}` on first press). Transcription and translation of voice are planned later. Customer video, document, and sticker become a Telegram card whose original line is a short Russian notice («Видео», «Документ», «Стикер»). The file itself is not attached. The operator can still quote that card; the reply is a text message.

A customer photo is uploaded into Telegram, and the text card is a reply to that photo. Quote the card, not the photo. A caption on the photo is translated like ordinary text.

A photo or file the operator attaches to a quote is delivered to the customer. Caption rules match text replies, including `!!`. WhatsApp accepts photos up to 5 MB and files up to 20 MB (Telegram bot download limit). Voice notes and video from Telegram are not forwarded. If delivery fails, the bot says so in the reply.

## Facebook post or ad

A message opened from a Facebook post, ad, or catalog product keeps, on the Telegram card, the source link and a Russian summary of the post (`🔗 Публикация` / `Реклама` / `Товар`, then `О чём пост`). The post image is uploaded above the card when Meta sends `image_url`. The customer's own sentence stays in `Оригинал`. «Подробнее об этом» without this block is only the customer's sentence. An already delivered card is not rewritten.

## First greeting

On the first text (or a non-text message) from a customer number the service sends one greeting in the language of that first message (he, ar, en, ru; anything else gets English), whatever the package. Welcome: thanks for contacting `BUSINESS_NAME`, message received, the team will be in touch soon. Text in `core/welcome.py`, business name from `BUSINESS_NAME`. Later messages in that chat are not greeted again (`contacts.greeted_at`). A greeting that failed (for example, an expired access token) does not set the flag, so the next message tries again. **Data notice (Stage 4, part B):** after two welcome lines and a blank line the greeting carries a notice in the same language (he, en, ru, ar): who stores the messages and why (`BUSINESS_NAME`), that Google translates them, that there is no end-to-end encryption, that writing is optional, and where to ask for a copy or deletion (`BUSINESS_CONTACT`, a phone or e-mail; without it the sentence ends at the business name). Texts in `core/welcome.py`, wording from `docs/compliance/whatsapp-data-privacy.md`; the Arabic text is a translation made for this project and has not been checked by a native speaker.

**Export and deletion of one number** (the client passes a customer's written request to Erythro): `python -m core.admin export-contact <number> [--out file]` (JSON: contact, all messages with original, Russian text and what was sent, attachment records by type and size; internal ids of WhatsApp and Telegram are left out), `delete-contact <number>` (shows the counts and changes nothing) and `delete-contact <number> --yes` (removes the contact, conversation, messages and attachment records in one transaction), `privacy-log`. Every export and deletion is logged in `privacy_log` (migration 0005) with an HMAC tag of the number and a per-installation salt, not the number. The command says what it cannot reach: encrypted nightly copies (they rotate out within `BACKUP_KEEP` days), Telegram cards of the demo bridge, the former `sessions.json` (named when it still contains the number) and WhatsApp itself. A deleted number that writes again is greeted again.

## Language package

`core/package.py`, stored in the `license` row: `package` (`starter` = one pair, `multi`), `operator_langs`, `client_langs`, `operator_lang` (the chosen one). First start takes `PACKAGE_NAME`, `PACKAGE_OPERATOR_LANGS`, `PACKAGE_CLIENT_LANGS` (demo compose: `multi`, `ru,he,en,ar`, `he,en,ar,ru`); later `python -m core.admin set-package multi --operator ru,he --clients he,en` or `show-package`.

- The operator language is the PWA interface language and the language incoming messages are translated into. Russian keeps the original prompts; Hebrew, English and Arabic use generic prompts (`GENERIC_*` in `core/whatsapp_translator.py`). The JSON key `translated_ru` and the column `text_ru` now mean "operator language".
- In `multi` with several operator languages the PWA asks at first sign-in (suggestion: the first device language offered, else English) and the top bar switches it (`POST /internal/v1/settings/operator-lang`). In `starter` it is fixed.
- A message in a language outside the package (not a client language and not the operator language) is not translated: card intent and `messages.note = out_of_package`. A reply to such a customer is refused (PWA 422 `language_out_of_package`, Telegram explains); `!!` still goes out as written.

## Tokens, webhooks, limits

Live `.env` is only on the VPS (`/home/whatsapp-translator/.env`, mode 600). Required: `GEMINI_API_KEY`, `TELEGRAM_BOT_TOKEN`, `TELEGRAM_ADMIN_CHAT_ID`, `WHATSAPP_PHONE_NUMBER_ID`, `WHATSAPP_ACCESS_TOKEN`. When `TELEGRAM_WEBHOOK_SECRET` is set, Telegram updates without `X-Telegram-Bot-Api-Secret-Token` get HTTP 403.

Webhook URL: `https://wa.erythro.ai/webhook/whatsapp`. Verify token: `whatsapp_translator_verify_token`. Telegram webhook: `https://wa.erythro.ai/webhook/telegram`.

The access token in production is a system-user token for `admin_erythro.ai` on app CustomerTranslater. It does not expire (`expires_at` 0). Scopes: `whatsapp_business_messaging`, `whatsapp_business_management`. A dashboard user token (`EAA…`, about 24 hours) stops both greeting and operator replies with HTTP 401 while incoming webhooks still arrive. Replacing the token requires recreating the container so it reloads `.env`.

A temporary user token cannot be generated until the system user has the app and the WhatsApp account assigned. Subscribing the app to the WABA (`subscribed_apps`) is required; the Meta webhook verify alone does not deliver customer messages.

Inbound customer messages are not charged. Through 30 Sep 2026, replies inside the 24-hour customer-service window are free and unlimited. From 1 Oct 2026 each business number gets 1,000 free service messages per calendar month; further service replies to Israeli recipients are about $0.0053 each. The greeting counts. Unused allowance does not roll over. After 24 hours without a customer message, only an approved template can be sent. An Israeli marketing template is about $0.0353 and needs a payment method in WhatsApp Manager. The 250 unique numbers per day cap applies only to business-initiated messages, not to replies inside an open window.

The server itself has no message quota. Gemini translation is a separate API cost.

Webhook handling: each Meta `wamid` is saved before translation; a replayed webhook with the same `wamid` is skipped (no second card, no second greeting). Events whose `metadata.phone_number_id` differs from `WHATSAPP_PHONE_NUMBER_ID` are ignored. `statuses` (`sent`, `delivered`, `read`, `failed`) update outgoing messages and never move a status back.

In-process limits (`core/limits.py`): LLM calls `LLM_RATE_PER_MIN` (25) per minute and `LLM_MAX_CONCURRENT` (2) at once per installation; per customer number 20 messages per minute to translation, 20 forwarded images per hour, 30 operator replies per minute. Text over `TRANSLATE_MAX_CHARS` (3000) is cut before translation, the card says so, the database keeps the whole original. Over the limit the incoming message is still stored and the card carries the original with a limit note; an operator reply over the limit is not sent and Telegram says to wait a minute. File sizes stay: photo up to 5 MB, file up to 20 MB.

Webhook and translation failures (2026-10-14): `POST /webhook/whatsapp` stores each new message by `wamid` and answers 200 at once; translation, the Telegram card and push run after the reply, one webhook at a time, in arrival order. A replayed `wamid` is skipped. Incoming translation (also the referral text and the manual retry) makes **one** model attempt: no automatic retries and no fallback model, so a failure never repeats anything on its own (decision 2026-10-14). `core/engine.py` keeps retries (up to 3 attempts, pauses 1.5 s and 3 s, the last one on `GEMINI_FALLBACK_MODEL`, default `gemini-3.5-flash`) only for the operator's own calls such as the reply translation. Main model `GEMINI_MODEL`, default `gemini-3.6-flash`, thinking level `GEMINI_THINKING_LEVEL=minimal` (chosen 2026-10-13). If the translation does not arrive, the card carries the original and a button "Перевести ещё раз" (Telegram `callback_data` `retr:<message id>`; PWA `POST /internal/v1/conversations/{id}/messages/{mid}/retranslate`, errors 404 `not_found`, 409 `not_retranslatable`, 429 `translation_limit`, 502 `translation_failed`). A successful retry stores the translation and rewrites the Telegram card without the button. No button for a language outside the package, for translated messages and for attachments.

## Operator PWA API (internal)

`operator_api.py`, prefix `/internal/v1`. Not published: Caddy on `wa.erythro.ai` passes only `/health` and `/webhook/*`, and any request with `X-Forwarded-For` gets 404. Every call needs `X-Internal-Token` = `CORE_INTERNAL_TOKEN` (empty token = API off, 503). Operator calls also need `Authorization: Bearer <session token>`; the web container keeps it in an httpOnly cookie, the browser never talks to the core.

| Method and path | Purpose |
|---|---|
| `POST /auth/login` | login (e-mail) + password → session token, 30 days, extended on use. 10 attempts per 10 minutes per login and per `X-Client-IP` |
| `POST /auth/logout`, `GET /auth/me` | end session, current operator |
| `POST /auth/password-reset/request` | e-mails a one-time link (30 minutes) to `PUBLIC_APP_URL/reset-password`; always 200, so the form does not reveal which e-mails exist; 3 requests per 15 minutes per login and per address |
| `POST /auth/password-reset/confirm` | token + new password (10+ characters); ends every session of the operator |
| `GET /status` | installation id, licence state, WhatsApp configured, `last_webhook_at` |
| `GET /conversations?unread=` | dialogs, newest first: contact (`is_new` for a stranger who wrote first), unread count, reply window (`window_open`, `window_until`), last message (the automatic greeting is skipped) |
| `GET /conversations/{id}`, `GET /conversations/{id}/messages?before_id=&limit=` | one dialog; messages oldest first with attachments |
| `POST /conversations/{id}/read` | unread to 0 |
| `POST /conversations/{id}/preview` | what would go out: translation into the reply language, or the text after `!!` |
| `POST /conversations/{id}/send` | sends; with `outgoing` from the preview the customer gets exactly that text. 409 outside the 24-hour window, 429 over 30 replies per minute |
| `POST /contacts`, `PATCH /contacts/{id}` | add a contact (E.164), name or language; naming clears `is_new` |
| `GET /attachments/{id}` | customer media, fetched from Meta on demand (files are not stored) |
| `GET /events` | SSE: `message`, `status`, `conversation` events with ids; the client refetches |
| `POST /push/subscribe`, `POST /push/unsubscribe` | Web Push subscription of this device (`endpoint`, `keys.p256dh`, `keys.auth`); 503 when VAPID keys are not set |

Passwords: `hashlib.scrypt` (N=2^14), tokens stored as SHA-256 only. A new password ends all sessions of that operator. Engineer commands in the container: `python -m core.admin set-password <login> --name "<name>"` (password asked, never an argument) and `python -m core.admin reset-link <login>` (one-time link to `PUBLIC_APP_URL/reset-password`, 30 minutes).

Outgoing messages carry `source` (`pwa`, `telegram`, `system`) and `kind` (`text`, `greeting`). Only operator replies mark a dialog read.

Reset e-mail: `core/mailer.py` over SMTP (`SMTP_HOST`, `SMTP_PORT` 465 SSL or 587 STARTTLS, `SMTP_USER`, `SMTP_PASSWORD`, `SMTP_FROM`), text in the operator's interface language (RU/EN/HE/AR). Without SMTP the engineer gives the link with `core.admin reset-link`.

Web Push (`core/push.py`, `pywebpush`): every new customer message is pushed to all subscribed devices (title: contact name or number, body: Russian translation up to 140 characters; the payload is end-to-end encrypted by the Web Push protocol, push services see only ciphertext). 404 or 410 from a push service removes the subscription. Keys: `VAPID_PUBLIC_KEY`, `VAPID_PRIVATE_KEY` (`python -m core.admin vapid-keys`), `VAPID_SUBJECT`. The service worker skips the notification when the app is open and focused. On iPhone push works only for the PWA added to the Home Screen (iOS 16.4+).

## Support tickets to the engineer circuit (internal API)

The operator can write to the engineer of Erythro (circuit: `docs/tech-circuit.md`). Off unless `TECH_URL` and `TECH_INSTALLATION_KEY` are set (key from `python -m tech.admin add-installation`); `APP_VERSION` is sent with a ticket. Tickets live in app.db (`support_tickets`, `support_messages`, migration 0004); only the text of the form and the follow-ups leave the installation, never customer conversations.

| Call | Meaning |
|---|---|
| `GET /support/status` | `{enabled, pending, unread, last_ok_at, last_error}`: pending = items not yet accepted by the circuit, unread = engineer replies not opened |
| `GET /support/tickets`, `GET /support/tickets/{id}` | list with the last message; the ticket with its messages (opening it reads the replies) |
| `POST /support/tickets` `{subject, body, contact_name?, contact_phone?}` | new ticket. Phone E.164 (422 `phone_invalid`), name defaults to the operator's, 5 per hour per operator (429), 503 `support_disabled` when off |
| `POST /support/tickets/{id}/messages` `{body}` | follow-up (reopens a closed ticket); 20 per minute per operator |

Delivery: this installation opens every connection (no inbound port). Two daemon threads started on app startup (`core/support.py`): the outbox sends queued tickets and messages in order (retry 5 s doubling to 5 min; wakes at once when something is written) and the poller long-polls `GET /v1/events?after=<cursor>&wait=20`. Replies and status changes are applied with the cursor (`settings.support_events_cursor`) in one transaction, so none is applied twice. A ticket or message the circuit refuses for good (400/404/409/413/422) is parked with its error and not retried; a wrong key, 429 and 5xx are retried. A reply publishes an SSE `support` event and a Web Push (`ticket_id` in the payload).

### Diagnostics report (Stage 4, part C)

The engineer never signs in to an installation. Instead the client chooses to send a technical report with a ticket. The PWA collects what the browser knows (the last console errors and warnings of the page, Service Worker state, push permission and subscription, network and live-feed state, user agent, installed or not, language, time zone, screen), the core adds what only it knows (installation id, `APP_VERSION`, schema version, licence state, package and operator language, WhatsApp configured, last webhook and its age, push configured on the server, support sync state) and `core/diagnostics.py` renders one text of at most 3800 characters. No message texts and no customer data.

| Call | Meaning |
|---|---|
| `POST /support/diagnostics/preview` `{client}` | the report exactly as it would be sent; nothing is stored or sent |
| `POST /support/tickets/{id}/diagnostics` `{client}` | stores the report as a message of the ticket with `kind: "diagnostics"` (rebuilt by the core, not taken from the browser) and queues it; 5 per hour per operator; 404 for an unknown ticket, 503 when support is off |

The browser is not trusted: only known fields of known types are kept; free text (console lines) loses phone numbers (7+ digits), e-mails, token-like runs of 24+ characters, URL queries and fragments, and quoted pieces of 13+ characters (a message text in "Unexpected token '…'"), and every line is cut to 200 characters; at most 20 lines. The panel shows the cleaned text first and sends only after the client presses the button; the sent report stays in the ticket thread, so the client can always see what was sent. Messages carry `kind` (`text` or `diagnostics`) in the core (migration 0006), in the circuit (migration 0002) and in the `/v1/tickets/{id}/messages` call. The panel also has "Reset the app cache": it deletes Cache Storage and the Service Worker and reloads (sign-in and dialogs stay; push may have to be turned on again, because a subscription belongs to the Service Worker registration).

Magic Link (a limited engineer session in the client PWA) is not built: decided 2026-10-11 to start with this report and return to Magic Link only if the report proves too little.

## Not implemented yet

A first message to a number that has not written yet. Meta rejects free text. It needs an approved marketing template, a payment method, and prior consent. Parked in `docs/PLAN-deferred.md` § WhatsApp first outbound until that traffic appears. The Telegram command does not exist yet.

Selling the bot to other companies is a Tech Provider rollout: each client gets their own number, Meta bill, and Telegram chat. Checklist: `docs/infrastructure/whatsapp-tech-provider.md`.

## Incident 2026-10-02: Graph API answers "API access blocked"

Status: resolved on 2026-10-02, root cause not confirmed. Customer messages had stopped reaching Telegram.

**Symptoms**
- No `POST /webhook/whatsapp` in the container log (the last 300 lines, up to 2026-10-01 22:44 UTC, hold only `/health`). Meta stopped delivering webhooks.
- Every Graph API call (`/me`, the phone number, `subscribed_apps`) with the production system-user token returned `400 OAuthException code 200 "API access blocked."`. A freshly generated token for the same system user returned the same error, so the block sits on the app CustomerTranslater, not on one token.

**What is fine (checked 2026-10-02)**
- Server: container `whatsapp_translator` healthy, `https://wa.erythro.ai/health` online, Caddy certificate valid, no errors in the Caddy log, host port 8000 closed.
- WhatsApp Manager: number `+972 50-530-8305` (phone number ID `1414060945113608`, matches `.env`) has no restriction. For 4 Sep – 1 Oct: 13 sent, 13 delivered, 23 received.
- Business Settings: system user `admin_erythro.ai` (ID `61595081334202`) has full access to the app CustomerTranslater (ID `4115238332107482`) and to the WhatsApp account ErythroAi. The app belongs to the business portfolio Erythro.ai (ID `1620861999525835`).

**What was found**
- WhatsApp Manager showed the warning "No valid payment method" (free-tier conversations can be started only by customers; replies are blocked). A payment method was added on 2026-10-02. The API stayed blocked afterwards.
- Business verification of Erythro.ai is not done. Documents were submitted on 2026-10-02; the decision is pending.
- The operator is shown as not an admin of the app ("You are not an admin of this app" in the token dialog). Opening `developers.facebook.com` asks to verify the account and the confirmation code never arrives, so the app status and App Review pages were not seen.
- Meta's business-support AI assistant (beta) said the app is in development mode with its WhatsApp review "not approved", and that API access is restricted because of it. This is unconfirmed: the checklist in `whatsapp-tech-provider.md` shows App Review was never submitted, and the assistant gave no evidence. A human agent was not available for this account status.
- Three WhatsApp accounts exist in the portfolio: ErythroAi `28743423441941303` (the one in WhatsApp Manager and billing), ErythroAi `1850783479281040` (duplicate, purpose unknown) and a Test account `854956307641254`. The old `.env` key `ACCOUNT_ID=2066297714089660` matches none of them; the bridge does not use it.

**Resolution**
- Later on 2026-10-02 the Graph API answered again with the unchanged production token: `/me`, the phone number and WABA `28743423441941303` returned HTTP 200. The number is `CONNECTED`, `LIVE`, quality GREEN, and `subscribed_apps` lists CustomerTranslater. The operator then confirmed that messages flow again.
- WABA `health_status` after recovery: WABA and APP `AVAILABLE`, BUSINESS `LIMITED` (error 141010, business verification pending).
- What lifted the block is not established, and Meta never gave a reason. Changes made the same day: a payment method added, business verification documents submitted, operator access to the developer console restored.
- Why the console login failed: the primary contact number of the operator's Facebook profile was `+972 50-530-8305`, the Cloud API number of the bridge. Verification codes (SMS and WhatsApp) went to a number that no phone receives. Fixed by adding and confirming a second number, making it primary, and removing the bridge number from the Facebook profile. This does not touch the Cloud API registration kept in WhatsApp Manager.

**App Dashboard (read after console access returned, 2026-10-02)**
- The app is unpublished ("Not published", development mode). The dashboard shows no block, rejection or App Review banner.
- The use case "Communicate with customers on WhatsApp" is configured. "Test use cases" and "Publish" are still open. The dashboard only suggests becoming a Tech Provider, which is required to submit App Review and to request access to other companies' data.
- So no App Review was ever submitted or rejected. The AI assistant's claim of a rejected review was wrong. The bridge runs on its own WABA in development mode, which is fine for one business.

**Open items**
1. Wait for the business verification decision. The BUSINESS `LIMITED` status should clear with it.
2. If the block comes back, look at the "Required actions" and "Notifications" pages of the App Dashboard. For selling the bot to other companies the app needs Tech Provider status, App Review and publishing (see `whatsapp-tech-provider.md`).
3. Do not delete WABA `1850783479281040`. Graph API on 2026-10-04 showed it lists the same live number (`+972 50-530-8305`, phone ID `1414060945113608`) as `28743423441941303`, with no subscribed app and no currency. It looks like a second record of the same account; removing it may detach the live number. Clean up only the unused `.env` key `ACCOUNT_ID` (old value `2066297714089660`) and the extra `PHONE_NUMBER_ID`.
4. Add an alert when no `POST /webhook/whatsapp` arrives for several hours during working time.
5. Never use the bridge number as a contact or 2FA number on any Facebook account.

## Deploy

From the erythro repo (needs `VPS_PASSWORD` in `.env.local`):

```bash
py -3 scripts/deploy_whatsapp_translator.py
```

The script packs `C:\dev\Targo` (only `core/`, `web/` without `node_modules` and `.next`, and the build files) without `.venv` or `.env`, keeps the remote `.env`, rebuilds both containers (`whatsapp_translator` and `targo_web`), and appends the `wa` Caddy site if it is missing. The `demo.targo.erythro.ai` site from `infra/Caddyfile.snippet` is added by hand. `docker compose` refuses to start without `CORE_INTERNAL_TOKEN` in `.env`; the web container gets only that secret and `CORE_URL`, not the Meta, Gemini or Telegram tokens. It does not import the n8n workflow. After changing only `WHATSAPP_PHONE_NUMBER_ID` or `WHATSAPP_ACCESS_TOKEN`, recreate with `docker compose up -d --force-recreate --no-build` from `/home/whatsapp-translator`.

## Checks

```bash
docker exec whatsapp_translator python -c "import urllib.request; print(urllib.request.urlopen('http://127.0.0.1:8000/health').read())"
curl -fsS https://wa.erythro.ai/health
curl -I --max-time 5 http://46.202.155.56:8000
```

Health JSON reports `whatsapp_gateway: cloud_api` and pairs `he/en → ru`, `ru → he/en`. The last command must time out or refuse the connection.
