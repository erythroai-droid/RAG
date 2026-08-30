# Анализ Email Service — формирование и оформление писем с Inline CID

#architecture #spring-boot #email #mime #cid #inline-images #order-processing

## 1. EmailService — отправка

**Файл реализации:** `src/main/java/com/montblanc/montblanc/EmailService.java`

### Метод 1: `sendMultipartMessage(String to, String subject, String messageBody)`

- **Назначение:** отправить HTML-письмо без inline-изображений.
- **Параметры:**
  - `to` — адрес получателя
  - `subject` — тема письма
  - `messageBody` — тело письма в HTML (готовый HTML-фрагмент)
- **Как отправляет:**
  - создаёт `MimeMessage`;
  - оборачивает в `MimeMessageHelper(message, true)` (true = multipart, можно HTML);
  - выставляет: `setTo(to)`, `setSubject(subject)`, `setText(messageBody, true)` (true = HTML);
  - отправитель: `setFrom("order@erythro.ai")`;
  - отправка: `javaMailSender.send(message)`.
- **Использование:** fallback, когда нет inline-изображений (например, логотип не найден и товары без картинок).

### Метод 2: `sendWithInlineImages(String to, String subject, String htmlBody, Map<String, byte[]> inlineImages)`

- **Назначение:** отправить HTML-письмо с inline-изображениями (логотип, товары) по cid.
- **Параметры:**
  - `to` — адрес получателя
  - `subject` — тема письма
  - `htmlBody` — HTML-тело с ссылками вида `img src="cid:xxx"`
  - `inlineImages` — Map: Content-ID → байты изображения (PNG/JPEG)
- **Как отправляет:**
  - создаёт `MimeMessage`;
  - `MimeMessageHelper(message, true, "UTF-8")`;
  - для каждой пары (cid, bytes) вызывает `addInline(cid, ByteArrayResource, contentType)`;
  - Content-Type определяется по сигнатуре файла (PNG: `89 50 4E 47`, иначе JPEG).
- **Почему cid, а не URL:** Gmail и Mail.ru блокируют или не поддерживают data URI и внешние URL; inline-вложения с cid работают во всех почтовых клиентах.

**EmailService только отправляет** готовый HTML и вложения; он не формирует и не оформляет письмо.

---

## 2. Где и как формируется и оформляется письмо

**Файл:** `OrderController.java`, метод `order()` (POST `/order`).

Письмо **формируется и оформляется только здесь** — в виде HTML-строки `emailBody` и `Map<String, byte[]> inlineImages`, которые передаются в `emailService.sendWithInlineImages(...)` или `sendMultipartMessage(...)`.

### Порядок действий

1. Заказ сохраняется в БД (`ordersRepository.save(orders)`), чтобы был `orders.getId()`.

2. **Подготовка inline-изображений:**
   - **Логотип:** загружается из classpath `static/images/Logo.png`, добавляется в `inlineImages` с cid `logomontblanc`. Если файл не найден — логотип не добавляется, в шапке остаётся только номер заказа.
   - **Товары:** для каждого `OrderProducts` поле `image` (base64) декодируется через `decodeBase64Image()`. Поддерживаются форматы `data:image/png;base64,...` и чистый base64. Добавляется в `inlineImages` с cid `product-0`, `product-1`, … Если изображения нет — в ячейке пусто.

3. **Таблица товаров** (`productRows`): для каждого товара — строка с колонками **Image** (50×50, `cid:product-N`), **Name**, **Qty**, **Price**. Имена экранируются через `HtmlUtils.htmlEscape()`.

4. **Тело письма** (`emailBody`) — полноценный HTML-документ:
   - `<!DOCTYPE html>`, `<html>`, `<head>` (charset UTF-8, viewport, title "Order Confirmation"), `</head>`;
   - `<body>` с фоном `#f4f4f4`;
   - внешняя таблица-обёртка по центру, ширина 600px, фон белый, шрифт Arial/Helvetica;
   - **Шапка (акцентный фон #0D0D0D или #46BB22):** таблица из двух ячеек — слева логотип `cid:logomontblanc` (120×39), справа номер заказа `Order №` + `orders.getId()`;
   - блок приветствия: `Hello, {orders.getName()}` (если имя задано) или `Hello,`; затем "Thank you for your order at MontBlanc...";
   - **Таблица заказа:** заголовки (пустая колонка для картинок) | Name | Qty | Price; затем `productRows`; затем строка **Total:** + `orders.getTotal()` + " ₪";
   - блок **Delivery:** и **Payment:** — значения из `orders.getDelivery()` и `orders.getPayment()`;
   - disclaimer и футер "© MontBlanc. All rights reserved." (серый фон `#f8f8f8`).

5. **Финальная подготовка:**
   - если логотипа нет в `inlineImages`, из тела удаляется тег `<img src='cid:logomontblanc' .../>`;
   - если `inlineImages` не пуст — вызов `sendWithInlineImages(...)`;
   - иначе — вызов `sendMultipartMessage(...)` (без картинок).

6. **Тема письма:** `"Order № " + orders.getId() + " from MontBlanc"`.

Стили — только инлайновые атрибуты `style='...'` (так лучше для почтовых клиентов).

### Вспомогательный метод: `decodeBase64Image(String imageData)`

Декодирует base64 в `byte[]`. Поддерживает:
- полный data URI: `data:image/png;base64,iVBORw0KGgo...`;
- чистый base64: `iVBORw0KGgo...`.

При ошибке или пустых данных возвращает `null`.

---

## 3. Краткая сводка

| Компонент | Роль |
|---|---|
| **EmailService.sendMultipartMessage** | Отправка HTML без изображений. Fallback при отсутствии inline-картинок. |
| **EmailService.sendWithInlineImages** | Отправка HTML с inline-изображениями (logo, товары) по cid. Работает в Gmail, Mail.ru и др. |
| **OrderController.order()** | Формирование письма: шапка с логотипом и номером заказа, приветствие с именем, таблица товаров (Image \| Name \| Qty \| Price), итог, доставка/оплата, футер. Подготовка `inlineImages` (логотип из classpath, товары из base64). |

Шаблонизаторов (Thymeleaf и т.п.) нет — всё собирается в Java.

### Зависимости от фронтенда

- Поле `image` (base64) в каждом товаре заказа должно передаваться при POST `/order`, чтобы в письме отображались картинки товаров.
