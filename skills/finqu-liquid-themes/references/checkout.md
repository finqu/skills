# Checkout Customization

The checkout is a separate theme with its own templates, layouts, assets, and configuration. It works differently from the main store theme.

## Key Differences from Main Theme

- **No sections or blocks** — All layout is defined directly in Liquid templates
- **Vue.js required** — Dynamic features (cart updates, address validation, payment) use Vue.js
- **Separate directory structure** — Own `templates/`, `layout/`, `assets/`, `config/`, `locales/`
- **Different template set** — `checkout.liquid`, `complete.liquid`, `download.liquid`, `order.liquid`

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
    └── order.liquid
```

## Vue.js Library

The checkout uses Vue.js for interactive features. Include these scripts before `</body>`:

```html
<script src="https://cdn.finqu.com/lib-sdk/checkout-vue@1.3.5/vue.global.prod.js"></script>
<script src="https://cdn.finqu.com/lib-sdk/checkout-vue@1.3.5/vue-i18n.global.prod.js"></script>
<script src="https://cdn.finqu.com/lib-sdk/checkout-vue@1.3.5/components.umd.js"></script>
```

## Vue Initialization

```javascript
const checkout = new Checkout();

checkout.initialize(document.querySelector('html').dataset.sessionId).then(() => {
    const app = Vue.createApp({
        inject: ['$checkout'],
        // Component logic here
    });

    app.use(checkout.plugin);
    app.mount('#checkout-app');
});
```

## Customizing Styles

Add `checkout.scss.liquid` to the `assets/` directory to customize checkout appearance. Note: this may require a specific Finqu plan.

## Checkout Templates

| Template          | Purpose                                              |
| ----------------- | ---------------------------------------------------- |
| `checkout.liquid` | Main checkout flow (shipping, payment, confirmation) |
| `complete.liquid` | Order confirmation page                              |
| `download.liquid` | Digital download page                                |
| `order.liquid`    | Order status/tracking page                           |

## Full Reference

See the [Finqu checkout documentation](https://developers.finqu.com/build-with-finqu/liquid-themes/checkout.md.txt) for complete details.

For checkout-specific Liquid objects, see:

- [Checkout objects](https://developers.finqu.com/reference/liquid/objects/checkout/global.md.txt)
- [Order object](https://developers.finqu.com/reference/liquid/objects/checkout/order.md.txt)
