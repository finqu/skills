# Sections

Sections are modular components that define flexible, customizable layouts in Finqu themes.

## How Sections Work

- Stored in the `sections/` directory — one file per section
- Rendered via `{{ content_for_index }}` (dynamic) or `{% section 'name' %}` (static)
- Contain Liquid markup, logic, and a `{% schema %}` block
- Can include blocks rendered via `{% container 'id' %}`

## Section Schema

Every section defines its configuration in a `{% schema %}` tag:

```liquid
{% schema %}
{
  "name": { "en": "Featured Products", "fi": "Suositellut tuotteet" },
  "tag": "section",
  "class": "section section-featured-products",
  "category": "theme-featured",
  "keywords": {
    "en": ["featured", "products", "grid"],
    "fi": ["suositellut", "tuotteet"]
  },
  "settings": [
    {
      "id": "title",
      "type": "text",
      "label": { "en": "Title", "fi": "Otsikko" },
      "default": "Featured Products"
    },
    {
      "id": "columns",
      "type": "range",
      "label": { "en": "Columns", "fi": "Sarakkeet" },
      "min": 2,
      "max": 6,
      "default": 4
    }
  ],
  "blocks": [
    {
      "type": "product-card",
      "name": { "en": "Product Card" },
      "settings": []
    }
  ]
}
{% endschema %}
```

## Schema Properties

| Property   | Required | Description                                                        |
| ---------- | -------- | ------------------------------------------------------------------ |
| `name`     | Yes      | Display name (localized object)                                    |
| `tag`      | No       | HTML tag or identifier                                             |
| `class`    | No       | CSS classes applied to the section wrapper                         |
| `category` | Yes      | Must match an entry in `settings_schema.json` `section_categories` |
| `keywords` | No       | Search keywords (localized) for the editor                         |
| `settings` | No       | Array of setting definitions                                       |
| `blocks`   | No       | Array of block type definitions                                    |

## Rendering Blocks in Sections

Use the `container` tag to render blocks assigned to the section:

```liquid
<div class="section-content">
  {% container 'main' %}
</div>
```

## Section Example

```liquid
<div class="featured-products">
  <h2>{{ section.settings.title }}</h2>
  <div class="product-grid columns-{{ section.settings.columns }}">
    {% container 'products' %}
  </div>
</div>

{% schema %}
{
  "name": { "en": "Featured Products" },
  "tag": "section",
  "class": "section section-featured-products",
  "category": "theme-featured",
  "settings": [
    { "id": "title", "type": "text", "label": { "en": "Title" }, "default": "Featured" }
  ]
}
{% endschema %}
```

## Full Reference

See the [Finqu sections documentation](https://developers.finqu.com/build-with-finqu/liquid-themes/sections.md.txt) for complete details.
