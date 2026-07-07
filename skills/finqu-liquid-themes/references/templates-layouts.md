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
| `customers/order.liquid`            | Single order detail          |
| `customers/wishlist.liquid`         | Customer wishlist            |
| `customers/edit_account.liquid`     | Edit account                 |
| `customers/change_password.liquid`  | Change password              |
| `customers/recover_password.liquid` | Password recovery            |
| `customers/reset_password.liquid`   | Password reset               |
| `customers/activate_account.liquid` | Account activation           |

> **Deprecated:** `customers/orders.liquid` is no longer part of storefront themes. Customer order history is rendered by the checkout theme using `orders.liquid`.

## Template Variants

Create alternative layouts for specific products or pages:

- `product.campaign.liquid` — Campaign product layout
- `page.landing.liquid` — Landing page layout

The template **type** is the part before the first dot. `product.liquid` and `product.campaign.liquid` both have type `product`.

Merchants assign alternate templates to specific products or pages in the Finqu admin. As a theme author, you only create the template files.

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

- `template_sections` — Sticky sections that are always present (cannot be removed, but can be reordered)
- All sections — sticky and user-added — render at `{{ content_for_index }}`

## Section Ordering Model

| Placement              | How you author it                                      | Merchant can edit?              |
| ---------------------- | ------------------------------------------------------ | ------------------------------- |
| Shared header/footer   | `{% sections 'header-group' %}` in layout              | Yes — section group             |
| Fixed in layout        | `{% section 'announcement-bar' %}` before layout content | No                            |
| Per-template content   | `{{ content_for_index }}` in template                | Yes — add/reorder sections      |
| Fixed in template      | `{% section 'product-details' %}` above/below index    | No                              |
| Sticky defaults        | `template_sections` in template schema                 | Reorderable, not removable      |

**Ordering rule:** Content **above** `{{ content_for_index }}` renders before merchant-added sections; content **below** renders after. Same before/after logic applies in the layout around `{{ content_for_layout }}`.

## Section Groups

Shared, merchant-managed section regions (header, footer) defined in `section-groups/` and rendered with:

```liquid
{% sections 'header-group' %}

{{ content_for_layout }}

{% sections 'footer-group' %}
```

Group section data is stored once and shared across all templates that include the group.

|                          | `{% sections 'group-name' %}`              | `{% section 'section-name' %}`     |
| ------------------------ | ------------------------------------------- | ---------------------------------- |
| Merchant editable        | Yes — add, remove, reorder sections         | No — fixed in code                 |
| Shared across pages      | Yes                                         | No — only where the tag appears    |
| Best for                 | Headers, footers, announcement bars         | Truly fixed, theme-controlled content |

See `references/sections.md` for defining groups and opting sections into them.

## Static Sections

```liquid
{% section 'announcement-bar' %}
```

Static sections are not affected by the theme editor. For editable cross-page regions, prefer section groups.

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

- `{{ content_for_header }}` — **Required.** Scripts, styles, and meta tags injected by the platform.
- `{{ content_for_layout }}` — **Required.** Renders the current template content.

**Using layouts in templates:**

```liquid
{% layout 'theme' %}
```

Use `{% layout 'none' %}` for standalone pages that should not wrap in a layout (e.g. `password.liquid` or minimal landing pages).

For production themes, prefer section groups for merchant-editable headers and footers.

## Full Reference

See the [Finqu templates & layouts documentation](https://developers.finqu.com/build-with-finqu/liquid-themes/templates-layouts.md.txt) for complete details.
