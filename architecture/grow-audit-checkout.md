# Grow — оплата аудита

Платный заказ на `/order/audit-*` уходит на hosted-страницу Grow. Карта на сайт не попадает. Код PayPlus в репозитории остаётся только для старого callback.

Три системы, строго по порядку.

1. **Morning** (бывшая חשבונית ירוקה) — учёт бизнеса и кассовый терминал. Тип бизнеса здесь — עוסק פטור.
2. **Grow** (бывший Meshulam) — списание карты. Терминал открывают из Morning. Grow проверяет сайт отдельно от разрешения на платёжные ссылки.
3. **Make** — создаёт ссылку на оплату и подтверждает сделку. Сайт не вызывает ни Morning, ни Meshulam.

Прямой Light API (`GROW_USER_ID` / `GROW_PAGE_CODE`) остаётся запасным, если переменные Make не заданы. Номер счёта из письма Grow — это не `userId`. Приложение Morning в Make (API key) в этот поток не входит: документ после оплаты выпускает сам Morning.

С 2026-10-10 оба сценария (**Integration Grow** → Create Payment Link и **Integration Webhooks, Grow** → Approve Transaction) сидят на подключении **VADYM’s Grow Live**, оба Active. Песочница **VADYM's Grow Sandbox** больше не выбрана. Следующая оплата с сайта списывает деньги. Ключи Vercel те же.

## План реализации

Этот раздел — как повторить подключение с нуля: Morning, затем Grow, затем Make, затем сайт. Цена пакета живёт в админке (**Content → Plans**, поле **Price**), не в Morning, не в Make и не в коде. `src/lib/orderPlans.ts` — только запас, если в CMS цена пустая.

Официальные страницы: [подключение סליקה в Morning](https://www.greeninvoice.co.il/help-center/digital-payments-connect/), [переход Grow в прод](https://developers.grow.business/reference/live-environment), [Grow App for Make](https://developers.grow.business/docs/grow-app-for-make). Регион Make: `eu1.make.com`. Списание только в шекелях.

### 1. Morning — учёт и терминал

Morning ведёт книги и после успешной оплаты сам выпускает документ. Сайт документ не создаёт и в запрос оплаты счёт не кладёт.

1. Аккаунт на [morning](https://www.greeninvoice.co.il/) (חשבונית ירוקה). Тип עוסק פטור. Реквизиты бизнеса, банк и мобильный, который потом пойдёт в Grow, заполнить до конца.
2. Тариф **Best** или выше: на нём терминал без отдельной месячной платы, оплата только комиссии со сделки. Точные ставки — на странице Morning, они меняются.
3. Меню **תשלומים → סליקה**, кнопка подключения. Телефон — тот, что привязан к бизнесу.
4. Терминала Grow ещё нет: регистрация нового (מסוף סליקה), все шаги и документы до конца. Иначе заявка не уходит. Терминал уже есть: Morning подключается к нему тем же телефоном, второй терминал не открывать.
5. В той же форме отметить приём оплаты с сайта и вписать `https://erythro.ai`. Разрешение Morning на приложение и платёжные ссылки само по себе торговлю на сайте не включает. Проверку сайта делает Grow. В справке Morning это обычно один рабочий день; письмо об одобрении терминала приходит отдельно и может занять дольше.
6. Дождаться письма, что терминал одобрен. Пока письма нет, боевое списание не включать.
7. В **תשלומים → סליקה** задать тип документа, который Morning выпускает после оплаты: для עוסק פטור это **קבלה**, не חשבונית מס. Там же — максимум платежей: для аудита один платёж.

Сайт к этому моменту уже должен быть живым. Grow смотрит `https://erythro.ai`, не preview. Список требований — в разделе «Что Grow смотрит на сайте» ниже: телефон, адрес, галочка условий до карты, текст условий на странице.

Выплата на банк — 10-го числа за прошлый месяц. Комиссию считает Grow.

### 2. Grow — что следует из типа бизнеса

Отдельного кабинета «сначала Grow, потом Morning» в этом потоке нет: карта живёт на терминале, который открыт из Morning.

- Боевой счёт — עוסק פטור, поэтому на ссылке **VAT Type = VAT Exempt**. `Regular VAT` песочница принимала и показывала מע״מ внутри суммы. Боевой Grow такую ссылку отклоняет, Make отвечает сайту HTTP 500, форма пишет «Оплата временно недоступна».
- На боевой странице сверху название бизнеса (**erythro.ai**), מע״מ ₪0, к оплате та же сумма, что в админке.
- В песочнице сверху было **MAKE**. Тестовая карта `4580458045804580` работает только там. Bit, Apple Pay, Google Pay и PayBox могут списать живые деньги даже в песочнице.
- Боевые `userId` и `pageCode` для Light API Grow выдаёт отдельно, письмом в `support@grow.business`, и только для утверждённого адреса. Пока оплата идёт через Make, эти ключи не нужны. Номер счёта из письма — не `userId`.

### 3. Подключение Grow в Make

1. В модуле Grow нажать **Connection → Add**.
2. Тип **Grow Sandbox** — проверка без списания. Тип **Grow** — боевые деньги. Среду нельзя сменить у уже сохранённого подключения, только создать новое.
3. Номер бизнеса (ת.ז./ח.פ.) и мобильный, привязанный к счёту. **Save**, затем код из SMS.
4. Песочницу назвать отдельно от боевой (**VADYM's Grow Sandbox** и **VADYM’s Grow Live**). Боевую подставлять в оба модуля Grow. Песочницу не удалять.

В песочнице на странице оплаты сверху было **MAKE**. На боевом подключении — название бизнеса (**erythro.ai**). Тестовая карта `4580458045804580` работает только в Sandbox. Bit, Apple Pay, Google Pay и PayBox могут списать живые деньги даже в песочнице.

### 4. Сценарий ссылки — Integration Grow

Порядок модулей: **Webhooks → Custom webhook** → **Grow → Create Payment Link** → **Webhooks → Webhook response**. Сценарий **Active**, запуск сразу по данным. **Run once** для боя не нужен.

Create Payment Link:

- Connection: нужная среда.
- **Sending Mode** `none`. Ссылку клиенту отдаёт сайт, не SMS Grow.
- **Payment Type** payments, один платёж.
- **VAT Type** `VAT Exempt`. Боевой счёт — עוסק פטור. `Regular VAT` песочница принимала, боевой Grow отклоняет, и сайт получает HTTP 500 («Оплата временно недоступна»).
- Поля из вебхука, не константы: имя, телефон, email, название, цена, success URL, notify URL, custom field 1.
- Цена продукта = цена из вебхука. Количество `1`.

Webhook response: статус 200, `Content-Type: application/json`, тело `{"url":"<url из Create Payment Link>"}`. Не отвечать сразу `Accepted`. URL должен быть страница Grow, не erythro.ai и не hook.make.com.

### 5. Сценарий после оплаты — Integration Webhooks, Grow

Порядок: **Custom webhook** → **Grow → Approve Transaction** → **HTTP → Make a request**.

Approve Transaction, то же подключение, что у ссылки. Поля только из notify. Число `0` Make считает пустым, и сценарий сам выключается.

- Transaction ID → `data.transactionId`
- Transaction Token → `data.transactionToken`
- Full Name → `data.fullName`
- Payer Phone → `data.payerPhone`
- Asmachta → `data.asmachta`

`cField1` приходит как `data.customFields.cField1`, не как `data.cField1`.

HTTP:

- `POST https://erythro.ai/api/payment/make-paid`
- Заголовок `x-make-payment-secret` = секрет с Vercel. В чат и в репозиторий его не писать.
- JSON: `submissionId` = `data.customFields.cField1`, `transactionId` = `data.transactionId`, `sum` = `data.sum`, `statusCode` = `data.statusCode`
- Фильтр модуля: `statusCode` = `2`

Успешный Approve: Output `status` 1 и пустой Err. Повтор уже подтверждённой сделки — ошибка Grow `712`, второй аудит из неё не ставить.

### 6. Сайт и Vercel

Три переменные, production и preview, значения не коммитить:

| Переменная | Откуда |
|---|---|
| `MAKE_GROW_WEBHOOK_URL` | URL первого модуля сценария ссылки |
| `MAKE_GROW_NOTIFY_URL` | URL первого модуля сценария после оплаты. Его же сайт кладёт в `notifyUrl` ссылки |
| `MAKE_PAYMENT_SECRET` | Случайная строка. Тот же секрет в заголовке HTTP-модуля |

Пока заданы эти три ключа, Light API (`GROW_USER_ID` / `GROW_PAGE_CODE`) не вызывается.

Поток: форма `/order/audit-diagnostic` или `/order/audit-pro` → `POST /api/payment/create` → заявка `pending` → Make → редирект на Grow. Имя — два слова. Телефон — израильский мобильный `05XXXXXXXX`. Сумма считается на сервере из цены плана. После Approve сайт сверяет секрет, провайдера `grow`, сумму и `statusCode` `2`, затем письма и очередь аудита. Повтор того же вызова аудит второй раз не ставит. Редирект на `/order/success?id=&sig=` сам по себе оплату не доказывает.

Код: `src/lib/payments/makeGrow.ts`, `src/app/api/payment/create/route.ts`, `src/app/api/payment/make-paid/route.ts`, `src/lib/payments/fulfillPayment.ts`.

### 7. Как проверяли

1. Песочница, Диагностика: AUD-160, ₪149, אישור `132104079`, `make-paid`, аудит в очереди.
2. Песочница, Pro: AUD-162, ₪890, אישור `132104828`, отчёт `tier-pro`.
3. Боевое подключение с `Regular VAT`: Make HTTP 500, ссылки нет, списания нет.
4. После `VAT Exempt`: страница Grow открывается, сверху erythro.ai, מע״מ ₪0, сумма как в админке.
5. Живого списания ещё не было, поэтому קבלה в Morning на эту сумму ещё не проверялась. После первой боевой оплаты, которую сайт отметил `paid`, в Morning должна лежать קבלה на ту же сумму.

Лимиты бесплатного аудита (`AUDIT_INTAKE_LIMITS_OPEN_FOR_QA`) возвращать только после первого живого списания, которое сайт отметил `paid`. Возврат — в кабинете Grow, отдельного callback в коде нет. Документ возврата, если он нужен, правят в Morning.

## Make

Форма сайта не вызывает Meshulam. `POST /api/payment/create` создаёт заявку `pending` и шлёт JSON на сценарий Make, который делает **Create Payment Link** и в том же HTTP-ответе возвращает URL страницы оплаты.

Пока нет `MAKE_GROW_WEBHOOK_URL` и `MAKE_GROW_NOTIFY_URL`, этот путь выключен и используется Light API. Если нет и его ключей, `POST /api/payment/create` отвечает `503` до создания заявки. Если Make или Grow отказал уже после создания, заявка остаётся со статусом `failed`.

### Тело запроса сайта → Make

| Поле | Смысл |
|---|---|
| `fullName` | Два слова, уже проверены |
| `phone` | Израильский мобильный `05XXXXXXXX` |
| `email` | Email клиента |
| `title`, `productName` | Локализованное имя плана + `· Erythro.ai` (не slug) |
| `price` | Сумма плана в шекелях, как на сайте (`390.00` для `audit-pro-ads`) |
| `quantity` | `1` |
| `successUrl` | `https://erythro.ai/order/success?id=&sig=` |
| `cancelUrl` | `https://erythro.ai/order/cancel?slug=` |
| `notifyUrl` | Вебхук сценария `grow-notify`, не адрес сайта |
| `cField1` | id заявки |

Ответ сценария должен быть JSON с полем `url` (страница Grow, не erythro.ai и не hook.make.com). Модуль **Webhook response**, не мгновенный `Accepted`.

### Сценарий создания ссылки

1. Триггер: **Webhooks → Custom webhook**. URL этого хука — `MAKE_GROW_WEBHOOK_URL`.
2. **Grow → Create Payment Link**. Поля из шага 1. **Sending Mode** `none`. **Notify URL** = `notifyUrl` из запроса. **Custom Field 1** = `cField1`. **Payment Type** payments, один платёж. Цена продукта = `price`. **VAT Type** = `VAT Exempt`: боевой счёт — עוסק פטור. `Regular VAT` песочница принимала, боевой Grow отвечает ошибкой и сценарий отдаёт сайту HTTP 500.
3. **Webhooks → Webhook response**, тело `{"url":"<url из Create Payment Link>"}`.
4. Сценарий включён.

### Сценарий после оплаты

Уже есть: **Custom webhook `grow-notify` → Approve Transaction**. Его URL — `MAKE_GROW_NOTIFY_URL`.

После успешного Approve (Output `status: 1`) добавь **HTTP → Make a request**:

- `POST https://erythro.ai/api/payment/make-paid`
- Заголовок `x-make-payment-secret`: значение `MAKE_PAYMENT_SECRET`
- JSON: `submissionId` = `data.customFields.cField1`, `transactionId` = `data.transactionId`, `sum` = `data.sum`, `statusCode` = `data.statusCode`
- Фильтр: `statusCode` = `2`. Иначе сайт не ставит `paid`.

Сайт сверяет секрет, id заявки, провайдера `grow` и сумму с `paymentAmount`. Повтор того же вызова не ставит аудит в очередь второй раз. Затем письма и `triggerAuditAgent`.

`0` в полях Approve Make считает пустым значением. Маппить только поля из notify.

### Переменные Make

| Переменная | Назначение |
|---|---|
| `MAKE_GROW_WEBHOOK_URL` | Хук сценария Create Payment Link |
| `MAKE_GROW_NOTIFY_URL` | Хук сценария grow-notify |
| `MAKE_PAYMENT_SECRET` | Секрет заголовка `x-make-payment-secret` |

Те же три ключа на Vercel. Без них форма аудита по-прежнему отвечает `503`, если нет и `GROW_USER_ID` / `GROW_PAGE_CODE`.

## Light API (запасной)

Живых списаний через API нет, пока пустые `GROW_USER_ID` и `GROW_PAGE_CODE`. `userId` и `pageCode` выдаёт Grow после проверки интеграции. Если заданы переменные Make, API не вызывается.

Разрешение Morning на приложение и платёжные ссылки не включает торговлю на `https://erythro.ai`. Адрес сайта передают в Morning (**תשלומים → סליקה**), проверку делает Grow. Боевые идентификаторы Light API — только для утверждённого адреса.

## Поток Light API

1. Форма на `/order/audit-*` (кроме `audit-free`) вызывает `POST /api/payment/create`.
2. Сумма считается на сервере. Имя — два слова. Телефон — израильский мобильный `05XXXXXXXX`. Иначе `400`, заявка не создаётся.
3. Заявка создаётся со статусом `pending`, провайдер `grow`.
4. Сервер шлёт `POST /api/light/server/1.0/createPaymentProcess` как `multipart/form-data` (`chargeType: 1`, `paymentNum: 1`, `saveCardToken: 0`, ILS). Клиент уходит на `data.url`.
5. В заявке хранится `processId|processToken`. Это секрет процесса, не номер транзакции.
6. Grow вызывает `POST /api/payment/webhook` (`notifyUrl`). Тело — form-data, часто под ключом `data[...]`. Подписи нет.
7. Webhook не верит телу. Он находит заявку по сохранённому процессу, читает `getTransactionInfo` и сверяет `statusCode` `2`, сумму в шекелях и `cField1` с id заявки.
8. Затем сервер вызывает `approveTransaction`. Это закрытие цикла у Grow, не доказательство оплаты. Если ack не прошёл, ответ `503`, чтобы Grow повторил вызов. Повтор не ставит аудит в очередь второй раз.
9. Переход в `paid` — условный `UPDATE`. После `paid`: письмо сотрудникам, письмо клиенту, `triggerAuditAgent`.
10. Браузер возвращается на `/order/success?id=&sig=` с дописанным `response=success`. Подпись — HMAC `PAYLOAD_SECRET`. Страница показывает `pending`, пока статус не станет `paid`. Редирект оплату не подтверждает.

Отмена ведёт на `/order/cancel?slug=`.

`statusCode`, отличный от `2` (в том числе отложенная сделка `11`), заявку не переводит в `paid` и не в `failed`.

## Переменные

| Переменная | Назначение |
|---|---|
| `GROW_USER_ID` | Идентификатор бизнеса. Не номер счёта |
| `GROW_PAGE_CODE` | Страница оплаты (карта, Bit и кошельки — тот код, который выдал Grow) |
| `GROW_API_KEY` | Только если Grow выдал ключ платформы. Обычному бизнесу не нужен |
| `GROW_TEST_MODE=true` | Без `GROW_API_URL` ходит в `https://sandbox.meshulam.co.il` |
| `GROW_API_URL` | Явный base URL, если нужен не дефолт |

Прод: те же ключи на Vercel, `GROW_TEST_MODE` не ставить, base URL `https://secure.meshulam.co.il`, `NEXT_PUBLIC_SITE_URL=https://erythro.ai`. Success, cancel и notify должны быть публичным HTTPS. `localhost` Grow не принимает.

## Кабинет

- Документация: [Introduction](https://developers.grow.business/reference/introduction), [процесс](https://developers.grow.business/reference/the-process), [переход в прод](https://developers.grow.business/reference/live-environment).
- Запрос боевых `userId` / `pageCode`: `support@grow.business`, после того как sandbox-поток создаёт процесс, webhook отвечает и уходит `approveTransaction`.
- Возврат на старте — из кабинета Grow. Отдельный callback возврата в этот поток не зашит: аудит из-за возврата сам не останавливается.
- Счёт в этот вызов не включаем. Документ после оплаты выпускает Morning (для עוסק פטור — קבלה), не сайт.

## Что проверить до боевых ключей

1. Sandbox `userId` и `pageCode`, тестовая карта, сумма как на плане.
2. Заявка `paid` только после `getTransactionInfo`, страница success до этого пишет «подтверждаем».
3. Повтор того же notify не создаёт второй прогон аудита.
4. Отмена возвращает на `/order/<slug>` без `paid`.
5. Чужой POST на `/api/payment/webhook` без процесса этой заявки не ставит `paid`.
6. Имя из одного слова и не-израильский номер остаются на форме с текстом ошибки.
7. На форме заказа отмечен чекбокс со ссылками на `/privacy` и `/terms`.

## Что Grow смотрит на сайте

Список с [перехода в прод](https://developers.grow.business/reference/live-environment). Проверяют `https://erythro.ai`, не preview.

| Требование | Где на сайте |
|---|---|
| Живой сайт и услуги с ценой | Главная, `/order/audit-*` |
| Страница покупателя до карты | Кнопка заказа открывает форму: имя, email, телефон, сайт |
| Телефон | Подвал и шапка |
| Адрес | Подвал («Эйлат») и абзац условий |
| Галочка со ссылкой на условия | Форма заказа, не отмечена заранее. Ссылки на `/terms` и `/privacy` |
| Условия на главной: доставка, ответственность, 18+, отмена, конфиденциальность | Абзац `#purchase-terms` в подвале каждой страницы, включая главную. Текст — глобал Footer, поле Purchase terms notice. Полный текст — `/terms`, раздел 7, глобал Legal — Terms of Use |

На живой форме заказа галочка ведёт на `/terms` и `/privacy` и по умолчанию не отмечена. Абзац условий покупки стоит на странице заказа.

Код: `src/lib/payments/makeGrow.ts`, `src/lib/payments/grow.ts`, `src/lib/payments/fulfillPayment.ts`, `src/components/PurchaseTermsNotice.tsx`, `src/app/api/payment/`.
