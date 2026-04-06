# Prints

Print templates for invoices, packing slips, and other physical documents in Finqu.

## Overview

Print templates produce documents for physical output (PDF/print). They use HTML/CSS with Liquid for dynamic content.

## Template Types

- **Invoice** — Billing document with order details and amounts
- **Packing slip** — Item list for warehouse fulfillment
- **Delivery note** — Shipping document for the carrier/customer

## Structure

```html
<!DOCTYPE html>
<html>
    <head>
        <style>
            body {
                font-family: Arial, sans-serif;
                font-size: 12px;
            }
            .header {
                display: flex;
                justify-content: space-between;
            }
            .line-items {
                width: 100%;
                border-collapse: collapse;
            }
            .line-items th,
            .line-items td {
                border: 1px solid #ddd;
                padding: 8px;
            }
            .total {
                text-align: right;
                font-weight: bold;
            }
        </style>
    </head>
    <body>
        <div class="header">
            <div>
                <h1>{{ store.name }}</h1>
                <p>{{ store.address }}</p>
            </div>
            <div>
                <h2>Invoice #{{ order.name }}</h2>
                <p>Date: {{ order.created_at | date: "%d.%m.%Y" }}</p>
            </div>
        </div>

        <table class="line-items">
            <thead>
                <tr>
                    <th>Product</th>
                    <th>Qty</th>
                    <th>Price</th>
                    <th>Total</th>
                </tr>
            </thead>
            <tbody>
                {% for item in order.line_items %}
                <tr>
                    <td>{{ item.title }}</td>
                    <td>{{ item.quantity }}</td>
                    <td>{{ item.price }}</td>
                    <td>{{ item.line_price }}</td>
                </tr>
                {% endfor %}
            </tbody>
        </table>

        <div class="total">
            <p>Subtotal: {{ order.subtotal_price }}</p>
            <p>Shipping: {{ order.shipping_price }}</p>
            <p>Tax: {{ order.tax_price }}</p>
            <p><strong>Total: {{ order.total_price }}</strong></p>
        </div>
    </body>
</html>
```

## Available Variables

Same order/customer/store variables as email templates, plus print-specific fields.

## Best Practices

- Use CSS `@media print` for print-specific styles
- Set fixed widths for consistent PDF output
- Test with browser print preview
- Include all legally required information (VAT number, company details, etc.)
- Use `page-break-before` / `page-break-after` for multi-page documents

## Full Reference

- [Prints](https://developers.finqu.com/build-with-finqu/customer-communications/prints.md.txt)
