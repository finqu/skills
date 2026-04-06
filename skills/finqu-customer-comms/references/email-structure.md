# Email Structure

How Finqu email templates are structured using MJML and Liquid.

## MJML Overview

MJML (Mailjet Markup Language) is a framework for responsive email HTML. It abstracts away email client compatibility issues.

## Basic Structure

```mjml
<mjml>
    <mj-head>
        <mj-attributes>
            <mj-all font-family="Arial, sans-serif" />
            <mj-text font-size="14px" color="#333333" />
        </mj-attributes>
    </mj-head>
    <mj-body background-color="#f4f4f4">
        <mj-section background-color="#ffffff">
            <mj-column>
                <mj-image src="{{ store.logo }}" alt="{{ store.name }}" width="150px" />
                <mj-text>
                    <!-- Liquid template content here -->
                </mj-text>
            </mj-column>
        </mj-section>
    </mj-body>
</mjml>
```

## Key MJML Components

| Component      | Purpose                 |
| -------------- | ----------------------- |
| `<mj-section>` | Row container           |
| `<mj-column>`  | Column within a section |
| `<mj-text>`    | Text content            |
| `<mj-image>`   | Image                   |
| `<mj-button>`  | CTA button              |
| `<mj-divider>` | Horizontal line         |
| `<mj-table>`   | Data table              |
| `<mj-spacer>`  | Vertical spacing        |

## Liquid Integration

Inside MJML components, use Liquid for dynamic content:

```mjml
<mj-text> Hi {{ customer.first_name | default: "there" }}, {% if order %} Your order #{{ order.name }} has been confirmed. {% endif %} </mj-text>
```

## Why MJML?

- Handles email client quirks (Outlook, Gmail, Yahoo, etc.)
- Responsive by default — works on mobile
- Cleaner syntax than raw HTML email tables
- Compiles to battle-tested HTML

## Full Reference

- [Email messages](https://developers.finqu.com/build-with-finqu/customer-communications/email-messages.md.txt)
- [MJML documentation](https://mjml.io/documentation/)
