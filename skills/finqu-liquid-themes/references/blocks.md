# Blocks

Blocks are modular content elements that live inside sections. Most blocks render content directly; some are **layout blocks** that contain child blocks in named containers (nesting limited to a single level).

## How Blocks Work

- Stored in the `blocks/` directory — one file per block type
- Rendered inside sections using the `{% container %}` tag
- Each block has its own `{% schema %}` with settings
- Layout blocks declare `containers` in their schema and render child blocks via `{% container 'id' %}`
- Order is set by the theme editor or default config

## Block Schema

```liquid
{% schema %}
{
  "name": { "en": "FAQ Item", "fi": "UKK-kohta" },
  "description": { "en": "A single question and answer pair." },
  "tag": "div",
  "class": "block block-faq-item",
  "category": "content",
  "settings": [ ... ]
}
{% endschema %}
```

## Schema Properties

| Property         | Required | Description                                                              |
| ---------------- | -------- | ------------------------------------------------------------------------ |
| `name`           | Yes      | Display name (localized object or `t:` key)                              |
| `description`    | No       | Description shown in the editor                                          |
| `category`       | Yes      | Must match an entry in `settings_schema.json` `block_categories`         |
| `tag`            | No       | HTML tag (default: `div`)                                                |
| `class`          | No       | CSS classes applied to the block wrapper                                 |
| `templates`      | No       | Template types the block is available on. Empty/absent = all templates   |
| `private`        | No       | If `true`, hidden from general picker; only addable where whitelisted     |
| `display`        | No       | Editor rendering mode. Currently only `"inline"` (side-by-side layout)   |
| `allowed_blocks` | No       | Whitelist of child blocks for layout blocks                              |
| `containers`     | No       | Declares layout block drop zones                                         |
| `settings`       | No       | Setting definitions                                                      |

See the [block schema definition](https://schemas.finqu.com/liquid/block.schema.json) for the authoritative shape.

## Rendering Blocks

**Inside sections** via container:

```liquid
{% container 'main' %}
```

**Static block slots** (theme-owned, merchant can configure but not drag):

```liquid
{% block 'product-title' %}
{% block 'app/part-payment' id: 'product-part-payment' %}
```

**Inline display** for side-by-side layout:

```liquid
{% block 'product-price', display: 'inline' %}
```

Or in block schema: `"display": "inline"`

## Layout Blocks and Containers

A layout block declares `containers` — drop zones for child blocks (single level only):

```liquid
<div class="product-card">
  <div class="product-card__media">{% container 'media' %}</div>
  <div class="product-card__details">{% container 'details' %}</div>
</div>

{% schema %}
{
  "name": { "en": "Product card" },
  "category": "content",
  "templates": ["product"],
  "containers": [
    { "id": "media", "allowed_blocks": ["product-image"] },
    { "id": "details", "allowed_blocks": ["product-title", "product-price"] }
  ]
}
{% endschema %}
```

Container `preset.blocks` seeds read-only fallback blocks when a statically imported block has no stored configuration. Bare block-type name strings are only valid inside `preset.blocks`.

## Private Blocks

Set `private: true` to remove a block from the general picker. Private blocks can only be added where explicitly listed in an `allowed_blocks` whitelist:

```json
{ "name": { "en": "Price" }, "category": "content", "private": true }
```

Use `private` for child-only blocks that only make sense inside a specific parent layout block.

## `allowed_blocks` Whitelist

Restricts which blocks a target accepts. Can be declared on:

- Section schema (top-level blocks)
- Layout block schema (child blocks in all containers)
- Container definition (overrides layout block schema for that container)

A block may be added when **all** apply:

1. **Template** — block's `templates` is empty or includes current template type
2. **Privacy** — if target has no whitelist, block must not be `private`
3. **Whitelist** — if target declares `allowed_blocks`, block must be in that list

Empty/absent `allowed_blocks` means all public, template-compatible blocks are accepted.

## Template Scope

Template **type** is the part before the first dot. `product` and `product.custom` both have type `product`.

```json
{ "templates": ["product"] }
```

## Third-Party App Blocks

App blocks use the same schema format and respect `templates`, `private`, and `allowed_blocks`. Themes constrain app blocks via section/container whitelists. For fixed integrations, use a static block slot:

```liquid
{% block 'app/part-payment' id: 'product-part-payment' %}
```

## Best Practices

- Keep block logic simple and focused on a single purpose
- Use `private` and `allowed_blocks` to enforce intentional placement
- Use layout blocks for composite components (product cards, etc.)
- Test blocks across sections and template types

## Full Reference

See the [Finqu blocks documentation](https://developers.finqu.com/build-with-finqu/liquid-themes/blocks.md.txt) for complete details.
