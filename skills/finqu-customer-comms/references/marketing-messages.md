# Marketing Messages

Creating and sending marketing email campaigns in Finqu.

## Overview

Marketing messages are manually created campaigns sent to customer segments. They use the same MJML + Liquid templating as transactional emails.

## Creating a Campaign

1. Go to store admin → Communications → Marketing
2. Create a new message
3. Set subject line and sender info
4. Design the email content using MJML + Liquid
5. Select target audience (customer segments)
6. Preview and send test
7. Schedule or send immediately

## Customer Segments

Target specific groups:

- All customers
- Customers who purchased in a date range
- Customers with specific tags
- Newsletter subscribers
- Custom segments

## Liquid Variables in Marketing

Available variables:

```liquid
{{ customer.first_name }}
{{ customer.last_name }}
{{ customer.email }}
{{ store.name }}
{{ store.url }}
```

Product recommendations and dynamic content can be included using Liquid loops and conditionals.

## Best Practices

- Always include an unsubscribe link
- Test across email clients before sending
- Personalize with customer name
- Keep subject lines concise (40-60 characters)
- Include a clear call-to-action
- Monitor open rates and click rates

## Full Reference

- [Marketing messages](https://developers.finqu.com/build-with-finqu/customer-communications/marketing-messages.md.txt)
