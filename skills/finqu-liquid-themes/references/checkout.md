# Checkout Customization

The checkout is a separate theme with its own templates, layouts, assets, and configuration. It works differently from the main store theme.

## Key Differences from Main Theme

| Feature | Storefront theme | Checkout theme |
| ------- | ---------------- | -------------- |
| Sections and blocks | Yes | No — edit templates directly |
| `{% form %}` tag | Yes | No |
| `{% section %}`, `{% sections %}`, `{% block %}`, `{% container %}` | Yes | No |
| `{% layout %}`, `{% render %}`, `{% stylesheet %}`, `{% javascript %}` | Yes | Yes |
| Vue checkout components | No | Required |

## Directory Structure

```
checkout/
├── assets/
│   └── checkout.scss.liquid    (optional, may require certain plan)
├── config/
│   ├── settings_schema.json
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
    ├── orders.liquid
    └── return.liquid
```

- `orders.liquid` — Customer order history (replaces deprecated storefront `customers/orders.liquid`)
- `order.liquid` — Single order detail page
- Checkout themes may ship multiple layouts (e.g. `layout/order.liquid` for order confirmation)

## Settings Schema

Checkout themes use `config/settings_schema.json` like storefront themes. You can set `api_version` (default `1.2`) to declare which checkout data shape your templates expect.

## Vue.js Library

The checkout uses Vue.js for interactive features. Include these scripts before `</body>`:

```html
<script src="https://cdn.finqu.com/lib-sdk/checkout-vue@1.3.5/vue.global.prod.js"></script>
<script src="https://cdn.finqu.com/lib-sdk/checkout-vue@1.3.5/vue-i18n.global.prod.js"></script>
<script src="https://cdn.finqu.com/lib-sdk/checkout-vue@1.3.5/components.umd.js"></script>
```

Do not remove or modify the Vue library script tags.

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

| Method                     | Description                                    |
| -------------------------- | ---------------------------------------------- |
| `components()`             | Returns all available Vue components         |
| `dispatchEvent(event, data)` | Dispatches a custom event                    |
| `freeze()`                 | Prevents user interactions during updates    |
| `resume()`                 | Restores checkout to ready state             |
| `refresh()`                | Refreshes checkout data from the server      |
| `placeOrder()`             | Initiates the order placement process          |

## Common Vue Components

- `<checkout-cart>` — Cart contents with slot props
- `<payment-method-selector>` / `<payment-method-list>` — Payment selection
- `<checkout-contact>` / `<contact-information>` — Contact info
- `<shipping-method-selector>` — Shipping selection
- `<code-claim>` / `<coupon-list>` / `<gift-card-list>` — Discounts
- `<loading-button>` — Async action button
- `<checkout-timer>` — Session countdown
- `<app-frame>` — Embedded apps via `@finqu/app-link`

## Customizing Styles

Add `checkout.scss.liquid` to the `assets/` directory to customize checkout appearance. Note: this may require a specific Finqu plan.

## Checkout Templates

| Template          | Purpose                                              |
| ----------------- | ---------------------------------------------------- |
| `checkout.liquid` | Main checkout flow (shipping, payment, confirmation) |
| `complete.liquid` | Order confirmation page                              |
| `download.liquid` | Digital download page                                |
| `order.liquid`    | Single order detail page                             |
| `orders.liquid`   | Customer order history                               |
| `return.liquid`   | Return flow page                                     |

## Full Reference

See the [Finqu checkout documentation](https://developers.finqu.com/build-with-finqu/liquid-themes/checkout.md.txt) for complete details.

For checkout-specific Liquid objects, see:

- [Checkout objects](https://developers.finqu.com/reference/liquid/objects/checkout/global.md.txt)
- [Order object](https://developers.finqu.com/reference/liquid/objects/checkout/order.md.txt)

For a reference implementation, see [checkout-default-theme](https://github.com/finqu/checkout-default-theme).
