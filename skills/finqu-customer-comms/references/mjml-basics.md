# MJML Basics

Quick reference for MJML email markup used in Finqu templates.

## What is MJML?

MJML is a markup language that compiles to responsive HTML email. It handles the complex table-based layouts required by email clients.

## Core Components

### Layout

```mjml
<mjml>
    <mj-body>
        <mj-section>
            <!-- Row -->
            <mj-column>
                <!-- Column (auto-width) -->
                <!-- Content -->
            </mj-column>
        </mj-section>
    </mj-body>
</mjml>
```

### Multi-column Layout

```mjml
<mj-section>
    <mj-column width="60%">
        <mj-text>Main content</mj-text>
    </mj-column>
    <mj-column width="40%">
        <mj-text>Sidebar</mj-text>
    </mj-column>
</mj-section>
```

### Text

```mjml
<mj-text font-size="16px" color="#333" line-height="1.5"> Hello {{ customer.first_name }}, </mj-text>
```

### Image

```mjml
<mj-image src="https://example.com/logo.png" alt="Logo" width="200px" />
```

### Button

```mjml
<mj-button background-color="#007bff" color="#ffffff" href="{{ order.status_url }}"> Track Your Order </mj-button>
```

### Table

```mjml
<mj-table>
    <tr>
        <th>Product</th>
        <th>Qty</th>
        <th>Price</th>
    </tr>
    {% for item in order.line_items %}
    <tr>
        <td>{{ item.title }}</td>
        <td>{{ item.quantity }}</td>
        <td>{{ item.price }}</td>
    </tr>
    {% endfor %}
</mj-table>
```

### Divider

```mjml
<mj-divider border-color="#eeeeee" />
```

### Spacer

```mjml
<mj-spacer height="20px" />
```

## Head Section

Global styles and attributes:

```mjml
<mj-head>
    <mj-attributes>
        <mj-all font-family="Arial, sans-serif" />
        <mj-text font-size="14px" color="#333333" />
        <mj-section background-color="#ffffff" padding="20px" />
    </mj-attributes>
    <mj-style>
        .highlight {
            color: #007bff;
        }
    </mj-style>
</mj-head>
```

## Full Reference

- [MJML documentation](https://mjml.io/documentation/)
- [MJML components](https://mjml.io/documentation/#components)
