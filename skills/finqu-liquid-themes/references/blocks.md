# Blocks

Blocks are modular content elements that live inside sections. Most blocks render content directly; some are **layout blocks** that contain child blocks in named containers (nesting is limited to a single level).

All placement fields are **opt-in and backward compatible**. A block schema with none of them behaves exactly as before.

## How Blocks Work

- Stored in the `blocks/` directory — one file per block type
- Rendered inside sections using the `{% container %}` tag
- Each block has its own `{% schema %}` with settings
- Order is set by the theme editor or default config

## Block Schema

```liquid
{% schema %}
{
  "name": { "en": "FAQ Item", "fi": "UKK-kohta" },
  "tag": "div",
  "class": "block block-faq-item",
  "category": "content",
  "settings": [
    {
      "id": "question",
      "type": "text",
      "label": { "en": "Question", "fi": "Kysymys" }
    },
    {
      "id": "answer",
      "type": "richtext",
      "label": { "en": "Answer", "fi": "Vastaus" }
    }
  ]
}
{% endschema %}
```

## Schema Properties

| Property     | Required | Description                                                      |
| ------------ | -------- | ---------------------------------------------------------------- |
| `name`       | Yes      | Display name (localized object)                                  |
| `tag`        | No       | HTML tag (default: `div`)                                        |
| `class`      | No       | CSS classes applied to the block wrapper                         |
| `category`   | Yes      | Must match an entry in `settings_schema.json` `block_categories` |
| `settings`   | No       | Array of setting definitions                                     |
| `templates`  | No       | Template types the block is available on. Empty = all templates  |
| `private`    | No       | If `true`, hidden from general picker; only addable where whitelisted |
| `display`    | No       | Set to `"inline"` for side-by-side layout in the designer        |
| `containers` | No       | Declares this block as a layout block with child drop zones      |

## Rendering Blocks

Blocks are rendered inside sections using the `container` tag:

```liquid
{% container 'main' %}
```

Or rendered statically with the `block` tag:

```liquid
{% block 'faq-item' %}
{% block 'app/part-payment' id: 'product-part-payment' %}
{% block 'product-price', display: 'inline' %}
```

## Layout Blocks and Containers

A **layout block** declares one or more `containers` in its schema. Each container is a drop zone for **child blocks**. Nesting is limited to a **single level**.

`blocks/product-card.liquid`:

```liquid
<div class="product-card">
  <div class="product-card__media">{% container 'media' %}</div>
  <div class="product-card__details">{% container 'details' %}</div>
  <div class="product-card__actions">{% container 'actions' %}</div>
</div>

{% schema %}
{
  "name": { "en": "Product card" },
  "templates": ["product", "category", "frontpage"],
  "containers": [
    {
      "id": "media",
      "title": { "en": "Media" },
      "allowed_blocks": ["product-image", "product-badge"]
    },
    {
      "id": "details",
      "title": { "en": "Details" },
      "allowed_blocks": ["product-title", "product-price", "product-rating"]
    },
    {
      "id": "actions",
      "title": { "en": "Actions" },
      "allowed_blocks": ["buy-button", "wishlist-button"]
    }
  ]
}
{% endschema %}
```

## Private Blocks

Set `private: true` to remove a block from the general picker. A private block can only be added where it is **explicitly listed** in an `allowed_blocks` whitelist:

```liquid
{% schema %}
{
  "name": { "en": "Price" },
  "private": true
}
{% endschema %}
```

Use `private` for child-only blocks that only make sense inside a specific parent layout block.

## allowed_blocks Whitelist

Restricts which blocks a target accepts. Can be declared on:

- A **layout block container** — restricts blocks in that container
- A **section schema** — restricts blocks at the section's top level

A block may be added when **all** apply:

1. **Template** — `B.templates` is empty or includes the current template type
2. **Privacy** — if the target has no whitelist, `B` must not be `private`
3. **Whitelist** — if the target declares `allowed_blocks`, `B` must be in that list

Empty/absent `allowed_blocks` means all public, template-compatible blocks are accepted.

## Static Block Slots

For integrations that must appear in an exact spot, use a static block slot:

```liquid
{% block 'app/part-payment' id: 'product-part-payment' %}
```

The merchant can configure it but cannot drag it elsewhere.

## Best Practices

- Keep block logic simple and focused on a single purpose
- Use `private` and `allowed_blocks` to enforce intentional placement
- Use `templates` to scope blocks to relevant page types
- Test blocks in different sections for compatibility

## Full Reference

See the [Finqu blocks documentation](https://developers.finqu.com/build-with-finqu/liquid-themes/blocks.md.txt) for complete details.
