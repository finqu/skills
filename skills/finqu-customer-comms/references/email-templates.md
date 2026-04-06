# Email Templates

Transactional email templates in Finqu and their available Liquid variables.

## Template Types

### Order Emails

- **Order confirmation** — Sent when order is placed
- **Payment received** — Sent when payment is confirmed
- **Shipping notification** — Sent when order is shipped
- **Refund notification** — Sent when refund is processed

Available variables: `order`, `customer`, `store`, `order.line_items`, `order.shipping_address`, `order.billing_address`

### Account Emails

- **Account creation** — Welcome email for new customers
- **Password reset** — Password reset link

Available variables: `customer`, `store`

### Other Emails

- **Contact form** — Auto-reply for contact submissions
- **Gift card** — Gift card code delivery

## Common Liquid Variables

### Store

```liquid
{{ store.name }}
{{ store.email }}
{{ store.url }}
{{ store.logo }}
{{ store.address }}
```

### Customer

```liquid
{{ customer.first_name }}
{{ customer.last_name }}
{{ customer.email }}
{{ customer.phone }}
```

### Order

```liquid
{{ order.name }}
{{ order.created_at | date: "%d.%m.%Y" }}
{{ order.total_price }}
{{ order.subtotal_price }}
{{ order.shipping_price }}
{{ order.tax_price }}
{{ order.discount_amount }}
{{ order.note }}
```

### Order Line Items

```liquid
{% for item in order.line_items %}
  {{ item.title }}
  {{ item.variant_title }}
  {{ item.quantity }}
  {{ item.price }}
  {{ item.sku }}
{% endfor %}
```

### Shipping Address

```liquid
{{ order.shipping_address.first_name }}
{{ order.shipping_address.last_name }}
{{ order.shipping_address.address1 }}
{{ order.shipping_address.city }}
{{ order.shipping_address.zip }}
{{ order.shipping_address.country }}
```

## Editing Templates

1. Go to store admin → Communications → Email templates
2. Select the template to edit
3. Edit MJML + Liquid content
4. Preview with sample data
5. Save

## Full Reference

- [Email messages](https://developers.finqu.com/build-with-finqu/customer-communications/email-messages.md.txt)
