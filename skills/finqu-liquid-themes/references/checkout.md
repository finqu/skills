# Checkout Customization

The checkout is a separate theme with its own templates, layouts, assets, and configuration. It works differently from the main store theme.

## Checkout vs Storefront

| Feature                                                              | Storefront theme | Checkout theme              |
| -------------------------------------------------------------------- | ---------------- | --------------------------- |
| Sections and blocks                                                  | Yes              | No — edit templates directly |
| `{% form %}`                                                         | Yes              | No                          |
| `{% section %}`, `{% sections %}`, `{% block %}`, `{% container %}` | Yes              | No                          |
| `{% layout %}`, `{% render %}`, `{% stylesheet %}`, `{% javascript %}` | Yes              | Yes                         |
| Vue checkout components                                              | No               | Required                    |

## Directory Structure

```
checkout/
├── assets/
│   └── checkout.scss.liquid    (optional, may require certain plan)
├── config/
│   ├── settings_schema.json    (supports api_version, default 1.2)
│   └── settings_data.json
├── layout/
│   ├── theme.liquid
│   └── order.liquid
├── locales/
│   ├── en.json
│   ├── fi.json
│   └── sv.json
├── snippets/                   (optional)
└── templates/
    ├── checkout.liquid
    ├── complete.liquid
    ├── download.liquid
    ├── order.liquid
    ├── orders.liquid           (customer order history — replaces deprecated customers/orders.liquid)
    └── return.liquid
```

`orders.liquid` renders customer order history. Use `order.liquid` for a single order detail page.

## Required Libraries

Include Bootstrap in `<head>`:

```html
<link rel="stylesheet" href="https://static.finqu.com/lib-sdk/bootstrap@5.3.8/bootstrap.min.css">
```

Include these scripts before `</body>`:

```html
<script src="https://static.finqu.com/lib-sdk/checkout-vue@1.3.6/vue.global.prod.js"></script>
<script src="https://static.finqu.com/lib-sdk/checkout-vue@1.3.6/vue-i18n.global.prod.js"></script>
<script src="https://static.finqu.com/lib-sdk/checkout-vue@1.3.6/customer-portal.umd.js"></script>
<script src="https://static.finqu.com/lib-sdk/bootstrap@5.3.8/bootstrap.bundle.min.js"></script>
```

## Vue Initialization

```javascript
const checkout = new Checkout();

checkout.initialize(document.querySelector('html').dataset.sessionId).then(() => {
  const app = Vue.createApp({
    inject: ['$checkout'],
    components: checkout.components(),
  });

  checkout.setUpApp(app);

  const messages = {};
  messages[checkout.store.state.order.language] = TRANSLATIONS;

  const i18n = VueI18n.createI18n({
    locale: checkout.store.state.order.language,
    fallbackLocale: checkout.store.state.order.language,
    messages: messages,
  });

  app.use(i18n);
  app.mount('#checkout');
});
```

## Checkout API

| Method                        | Description                                    |
| ----------------------------- | ---------------------------------------------- |
| `components()`                | Returns all available Vue components           |
| `dispatchEvent(event, data)`  | Dispatches a custom event                      |
| `freeze()` / `resume()`       | Block/unblock user interactions during updates |
| `refresh()`                   | Refresh checkout data from the server          |
| `placeOrder()`                | Initiate order placement                       |

## Key Vue Components

- `<checkout-cart>` — Cart contents (items, discounts, shipping, payments)
- `<payment-method-selector>`, `<payment-method-list>`, `<klarna-checkout>`
- `<checkout-contact>`, `<contact-information>`, `<checkout-account>`
- `<code-claim>`, `<coupon-list>`, `<gift-card-list>`
- `<shipping-method-selector>`
- `<loading-button>`, `<checkout-timer>`, `<app-frame>`, `<checkout-offer>`

Listen for events: `window.addEventListener('Checkout.Event.OrderPlaced', ...)`

## Customizing Styles

Add `checkout.scss.liquid` to `assets/` to customize checkout appearance. This may require a specific Finqu plan.

## Full Reference

See the [Finqu checkout documentation](https://developers.finqu.com/build-with-finqu/liquid-themes/checkout.md.txt) for complete details.

For checkout-specific Liquid objects:

- [Checkout objects](https://developers.finqu.com/reference/liquid/objects/checkout/global.md.txt)
- [Order object](https://developers.finqu.com/reference/liquid/objects/checkout/order.md.txt)

Reference implementation: [checkout-default-theme](https://github.com/finqu/checkout-default-theme)
