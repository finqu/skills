---
name: finqu-customer-comms
description: 'Finqu customer communications — email templates, marketing messages, print templates, and MJML/Liquid templating'
---

# Finqu Customer Communications

## When to use

Use this skill when:

- Creating or editing transactional email templates (order confirmation, shipping, etc.)
- Building marketing email campaigns
- Customizing print templates (invoices, packing slips)
- Working with MJML for responsive email layouts
- Using Liquid variables in email/print templates

## Inputs required

- **Template type**: Email (transactional or marketing) or print
- **Store admin access**: For editing templates
- **Content requirements**: What the template should contain

## Procedure

### 0) Understand the template system

Finqu uses:

- **MJML** — For responsive email HTML structure
- **Liquid** — For dynamic content (variables, loops, conditionals)
- **HTML/CSS** — For print templates

Templates are edited in the store admin under the communications section.

Read: `references/email-structure.md`

### 1) Choose your template type

**Transactional emails** — Triggered automatically by events:

- Order confirmation
- Shipping notification
- Payment receipt
- Account creation
- Password reset
- Refund notification

**Marketing messages** — Sent manually or scheduled:

- Promotional campaigns
- Newsletter
- Product announcements
- Seasonal offers

**Print templates** — Generated for physical outputs:

- Invoices
- Packing slips
- Delivery notes

### 2) Edit email templates

Transactional email templates use MJML + Liquid:

```mjml
<mjml>
    <mj-body>
        <mj-section>
            <mj-column>
                <mj-text> Hi {{ customer.first_name }}, Your order #{{ order.name }} has been confirmed. </mj-text>
            </mj-column>
        </mj-section>
    </mj-body>
</mjml>
```

Read: `references/email-templates.md`

### 3) Use Liquid variables

Common variables available in templates:

- `{{ store.name }}` — Store name
- `{{ customer.first_name }}` — Customer first name
- `{{ customer.email }}` — Customer email
- `{{ order.name }}` — Order number
- `{{ order.total_price }}` — Order total
- `{% for item in order.line_items %}` — Loop through order items

Variable availability depends on the template type (order emails have order data, account emails have customer data, etc.).

### 4) Create marketing messages

1. Open the communications section in admin
2. Create a new marketing message
3. Design using MJML + Liquid
4. Use customer segments for targeting
5. Preview and test before sending
6. Schedule or send immediately

Read: `references/marketing-messages.md`

### 5) Customize print templates

Print templates use HTML/CSS with Liquid:

```html
<div class="invoice">
    <h1>Invoice #{{ order.name }}</h1>
    <p>Date: {{ order.created_at | date: "%Y-%m-%d" }}</p>
    {% for item in order.line_items %}
    <div class="line-item">{{ item.title }} × {{ item.quantity }} — {{ item.price }}</div>
    {% endfor %}
    <div class="total">Total: {{ order.total_price }}</div>
</div>
```

Read: `references/prints.md`

### 6) Test and preview

1. Use the preview function in admin to see rendered output
2. Send test emails to yourself
3. Test across email clients (Gmail, Outlook, Apple Mail)
4. Verify Liquid variables render correctly with real data
5. Check responsive behavior on mobile

## Verification

- MJML compiles without errors
- Liquid variables render with correct data
- Emails display correctly across major email clients
- Print templates produce clean, printable output
- Marketing messages target the correct customer segments
- Transactional emails trigger on the correct events

## Failure modes / debugging

- **Liquid variable shows nothing**: Variable name is wrong or data isn't available for that template type
- **MJML compilation error**: Check MJML syntax — must have valid structure (`<mjml>` → `<mj-body>` → `<mj-section>` → `<mj-column>`)
- **Email looks broken in Outlook**: Use MJML components instead of raw HTML — MJML handles email client quirks
- **Print template formatting issues**: Check CSS print styles, test with browser print preview

## Escalation

- See [Customer communications overview](https://developers.finqu.com/build-with-finqu/customer-communications/overview.md.txt)
- See [Email messages](https://developers.finqu.com/build-with-finqu/customer-communications/email-messages.md.txt)
- See [Marketing messages](https://developers.finqu.com/build-with-finqu/customer-communications/marketing-messages.md.txt)
- See [Prints](https://developers.finqu.com/build-with-finqu/customer-communications/prints.md.txt)
