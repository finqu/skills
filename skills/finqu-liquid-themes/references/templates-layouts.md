# Templates & Layouts

Templates control the layout and content of store pages. Layouts define shared structure across templates.

## Common Templates

| Template                            | Page                         |
| ----------------------------------- | ---------------------------- |
| `frontpage.liquid`                  | Store front page             |
| `product.liquid`                    | Product detail page          |
| `category.liquid`                   | Category / product listing   |
| `catalog.liquid`                    | Catalog overview             |
| `cart.liquid`                       | Shopping cart                |
| `blog.liquid`                       | Blog overview                |
| `article.liquid`                    | Individual article           |
| `page.liquid`                       | Generic content page         |
| `search.liquid`                     | Search results               |
| `manufacturer.liquid`               | Manufacturer product listing |
| `password.liquid`                   | Password-protected store     |
| `404.liquid`                        | Not found page               |
| `customers/login.liquid`            | Customer login               |
| `customers/register.liquid`         | Customer registration        |
| `customers/account.liquid`          | Account summary              |
| `customers/orders.liquid`           | Customer orders list         |
| `customers/order.liquid`            | Single order detail          |
| `customers/wishlist.liquid`         | Customer wishlist            |
| `customers/edit_account.liquid`     | Edit account                 |
| `customers/change_password.liquid`  | Change password              |
| `customers/recover_password.liquid` | Password recovery            |
| `customers/reset_password.liquid`   | Password reset               |
| `customers/activate_account.liquid` | Account activation           |

## Template Rendering

Templates include sections in two ways:

**Dynamic sections** — managed by the theme editor, reorderable:

```liquid
{{ content_for_index }}
```

**Static sections** — fixed position, not affected by editor:

```liquid
{% section 'announcement-bar' %}
```

## Template Schema

Templates can define metadata and required sections:

```liquid
{{ content_for_index }}

{% schema %}
{
  "name": { "en": "Product Campaign" },
  "template_sections": [
    { "name": "product" }
  ]
}
{% endschema %}
```

- `template_sections` — Sticky sections that are always present (cannot be removed by the user)
- Users can still add, reorder, and remove non-sticky sections via the editor

## Template Variants

Create alternative layouts for specific products or pages:

- `product.campaign.liquid` — Campaign product layout
- `page.landing.liquid` — Landing page layout

Assign different templates to specific products/pages in the Finqu admin.

## Layouts

Layouts define the common structure (HTML shell, header, footer) shared across templates.

**Required layout elements:**

```liquid
<!DOCTYPE html>
<html lang="{{ request.locale.iso_code }}">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>{{ page_title }}</title>
    {{ content_for_header }}
  </head>
  <body>
    {{ content_for_layout }}
  </body>
</html>
```

- `{{ content_for_header }}` — **Required.** Outputs scripts, styles, and meta tags injected by the platform.
- `{{ content_for_layout }}` — **Required.** Renders the current template content.

**Using layouts in templates:**

```liquid
{% layout 'theme' %}
```

Create multiple layouts for different purposes (e.g., `layout/campaign.liquid`).

## Full Reference

See the [Finqu templates & layouts documentation](https://developers.finqu.com/build-with-finqu/liquid-themes/templates-layouts.md.txt) for complete details.
