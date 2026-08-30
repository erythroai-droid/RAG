# ✉️ Скилл: Транзакционные HTML-письма с Inline CID (Transactional Emails)

#agent-skills #email #smtp #mime #cid #inline-images #hostinger #javamail #nodemailer

> **Назначение:** Регламент верстки и отправки адаптивных транзакционных писем (подтверждения заказов, лиды, сброс паролей) с гарантированным отображением картинок во всех почтовых клиентах (Gmail, Mail.ru, Outlook).

---

## 1. Фундаментальные правила верстки писем

1. **Табличная верстка:** Почтовые клиенты (особенно Outlook) некорректно поддерживают CSS Grid и Flexbox. Каркас письма строится на вложенных таблицах `<table>`.
2. **Ширина макета:** Фиксированная максимальная ширина контейнера `600px`, выравнивание по центру (`align="center"`).
3. **Инлайновые стили:** Все CSS-свойства передаются через атрибут `style="..."`. Теги `<style>` в `<head>` часто вырезаются спам-фильтрами.
4. **Безопасность картинок (CID vs External URL):**
   - Внешние URL картинок (`<img src="https://..." />`) по умолчанию блокируются Gmail/Mail.ru до клика пользователя «Показать изображения».
   - Вставка через Base64 (`<img src="data:image/png;base64,..." />`) полностью блокируется большинством веб-интерфейсов.
   - **Единственный надежный метод:** Внедрение изображений как Mime-вложений с Content-ID (`cid:xxx`) и ссылкой `<img src="cid:logomontblanc" />`.

---

## 2. Java / Spring Mail реализация с CID

```java
MimeMessage message = javaMailSender.createMimeMessage();
MimeMessageHelper helper = new MimeMessageHelper(message, true, "UTF-8");

helper.setFrom("order@erythro.ai");
helper.setTo(customerEmail);
helper.setSubject("Подтверждение заказа №" + orderId);
helper.setText(htmlContent, true);

// Прикрепление логотипа и картинок товаров
helper.addInline("logomontblanc", new ClassPathResource("static/images/Logo.png"), "image/png");
```

---

## 3. Node.js / Next.js Nodemailer реализация с CID

```typescript
import nodemailer from 'nodemailer'

const transporter = nodemailer.createTransport({
  host: process.env.SMTP_HOST,
  port: Number(process.env.SMTP_PORT) || 465,
  secure: true,
  auth: {
    user: process.env.SMTP_USER,
    pass: process.env.SMTP_PASS,
  },
})

await transporter.sendMail({
  from: '"Erythro.ai" <order@erythro.ai>',
  to: customerEmail,
  replyTo: clientEmail, // Ответ клиента уйдет на его реальный адрес
  subject: `Заявка с сайта №${orderId}`,
  html: `
    <table width="100%" cellpadding="0" cellspacing="0" style="background:#f4f4f4; padding:20px;">
      <tr>
        <td align="center">
          <table width="600" cellpadding="0" cellspacing="0" style="background:#ffffff; border-radius:8px; overflow:hidden;">
            <tr style="background:#0D0D0D;">
              <td style="padding:20px;">
                <img src="cid:brandlogo" width="120" height="40" alt="Erythro.ai" />
              </td>
            </tr>
            <tr>
              <td style="padding:30px; font-family:Arial,sans-serif; color:#1a1a1a;">
                <h2>Спасибо за заказ!</h2>
                <p>Мы получили вашу заявку и свяжемся с вами в течение 15 минут.</p>
              </td>
            </tr>
          </table>
        </td>
      </tr>
    </table>
  `,
  attachments: [
    {
      filename: 'logo.png',
      path: process.cwd() + '/public/images/logo.png',
      cid: 'brandlogo',
    },
  ],
})
```
