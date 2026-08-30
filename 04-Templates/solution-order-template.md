# 📑 Шаблон: Тарифный план и заказ (Solution Order Template)

#templates #pricing #order #solution-plans #checkout

> **Назначение:** Шаблон для тарифов, готовых решений и интерактивного чекаута (`/order/[slug]`).

---

## 1. Схема тарифа (`SolutionPlan`)

```typescript
export interface SolutionPlanData {
  slug: string
  title: LocalizedString
  price: number | string
  currency: 'ILS' | 'USD' | 'EUR'
  pricePrefix?: LocalizedString
  periodDiscountLabel?: LocalizedString
  features: Array<{
    label: LocalizedString
    value?: LocalizedString
    home_only?: boolean
  }>
  periods: Array<{
    periodId: '1m' | '6m' | '12m'
    label: LocalizedString
    months: number
    discountPercent: number
  }>
  addons: Array<{
    addonId: string
    name: LocalizedString
    description: LocalizedString
    priceDisplay: LocalizedString
    discountMonths1?: number
    discountMonths6?: number
    discountMonths12?: number
    recommended?: boolean
    mandatory?: boolean
  }>
}
```

---

## 2. Логика интерактивного калькулятора заказа

1. **Выбор периода:** 1 месяц (базовая цена), 6 месяцев (скидка 10%), 12 месяцев (скидка 20%).
2. **Выбор дополнительных опций (Add-ons):**
   - Индивидуальный AI-агент поддержки (+350 ₪/мес).
   - Выделенный DevOps и 24/7 SLA (+650 ₪/мес).
   - Расширенный аудит безопасности (+1,500 ₪ разово).
3. **Итоговый расчет:** Динамическое обновление итоговой суммы, НДС (VAT 18% для резидентов Израиля) и переход к оплате / генерации коммерческого предложения.
