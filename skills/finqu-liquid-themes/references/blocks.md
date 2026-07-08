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

| Property                    | Required | Description                                                                                       |
| --------------------------- | -------- | ------------------------------------------------------------------------------------------------- |
| `name`                      | Yes      | Display name (localized object or `t:` key)                                                       |
| `description`               | No       | Description shown in the editor                                                                   |
| `category`                  | Yes      | Must match an entry in `settings_schema.json` `block_categories`                                  |
| `tag`                       | No       | HTML tag (default: `div`)                                                                         |
| `class`                     | No       | CSS classes applied to the block wrapper                                                            |
| `templates`                 | No       | Template types the block is available on. Empty/absent = all templates                            |
| `private`                   | No       | If `true`, hidden from general picker; only addable where whitelisted                             |
| `display`                   | No       | Editor rendering mode. Currently only `"inline"` (side-by-side layout)                            |
| `allowed_blocks`            | No       | Whitelist of child blocks for layout blocks. Container-level lists take precedence                  |
| `containers`                | No       | Declares layout block drop zones                                                                  |
| `containers[].id`           | Yes      | Container id used by `{% container 'id' %}`                                                       |
| `containers[].title`        | No       | Container label in the designer                                                                   |
| `containers[].type`         | No       | Container type, e.g. `"static"` for a fixed container                                           |
| `containers[].allowed_blocks` | No     | Whitelist of blocks accepted by the container. Empty = all public blocks                          |
| `containers[].preset`       | No       | Fallback blocks for static imports with no stored config. See [Container preset blocks](#container-preset-blocks) |
| `presets`                   | No       | Predefined variants shown in the add-block picker. See [Block presets](#block-presets)            |
| `settings`                  | No       | Setting definitions                                                                               |

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
  <div class="product-card__actions">{% container 'actions' %}</div>
</div>

{% schema %}
{
  "name": { "en": "Product card" },
  "category": "content",
  "templates": ["product", "collection", "index"],
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

An unnamed `{% container %}` holds blocks that have no container assignment.

## Container Preset Blocks

Each `containers[].preset.blocks` array is a **fallback** for statically imported blocks (`{% block %}` in theme markup) that have **no stored configuration**. Preset blocks render **read-only** in the designer until the merchant makes the first change, then they are persisted as real block instances.

```json
{
  "id": "actions",
  "title": { "en": "Actions" },
  "preset": {
    "blocks": [
      "buy-button",
      { "name": "wishlist-button", "settings": { "style": "outline" } }
    ]
  }
}
```

This is the **only place** a bare block-type name string (shorthand for `{ "name": "..." }`) is allowed. Settings not specified default to the block's own schema defaults. A preset block that is itself a layout block can nest further preset blocks via its own `blocks` array.

## Block Presets

The `presets` property defines ready-made variants shown in the **add-block picker**. Unlike [container preset blocks](#container-preset-blocks), which are a read-only fallback for static imports, a block preset is applied **eagerly** — when a merchant picks it, its settings and nested child blocks are written into the block's stored configuration immediately.

`presets` accepts a plain array or a locale-keyed map (same `schemaPresetList` shape as [section presets](sections.md#section-presets)):

**Layout block** — preset seeds child blocks into containers:

```json
{
  "name": { "en": "Product card" },
  "category": "content",
  "presets": [
    {
      "name": { "en": "Default" },
      "default": true,
      "blocks": [
        { "name": "product-image", "container": "media" },
        { "name": "product-title", "container": "details" },
        { "name": "product-price", "container": "details" },
        { "name": "buy-button", "container": "actions" }
      ]
    }
  ],
  "containers": [
    { "id": "media", "title": { "en": "Media" } },
    { "id": "details", "title": { "en": "Details" } },
    { "id": "actions", "title": { "en": "Actions" } }
  ]
}
```

**Simple block** — preset sets `settings` only:

```json
{
  "name": { "en": "Rich text" },
  "category": "content",
  "presets": [
    {
      "name": { "en": "Intro paragraph" },
      "settings": {
        "body": "<p>Welcome to our store.</p>"
      }
    }
  ]
}
```

| Field       | Type                      | Description                                                                                  |
| ----------- | ------------------------- | -------------------------------------------------------------------------------------------- |
| `id`        | string                    | Optional stable identifier for this preset variant                                           |
| `name`      | string \| localized object | Label shown for this preset in the add-block picker                                         |
| `default`   | boolean                   | If `true`, this is the default preset offered                                                |
| `settings`  | object                    | Setting id → value pairs applied to the block on add                                         |
| `blocks`    | array                     | Nested child block instances for layout blocks. When present, container presets are **not** used |

Each entry in a preset's `blocks` array uses the same shape as [section preset block instances](sections.md#section-presets): `name`, optional `id`, `settings`, `container`, and nested `blocks`.

For localized preset copy and locale-keyed preset maps, see [localization.md](localization.md#presets).

## Private Blocks

Set `private: true` to remove a block from the general picker. Private blocks can only be added where explicitly listed in an `allowed_blocks` whitelist:

```json
{ "name": { "en": "Price" }, "category": "content", "private": true }
```

Use `private` for child-only blocks that only make sense inside a specific parent layout block. Unlike a section's `is_creatable: false`, a private block *can* be added — but only where whitelisted.

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

Template **type** is the part before the first dot. `product` and `product.custom` both have type `product`. Template names can also be nested paths (e.g. `"marketing/cart"`).

```json
{ "templates": ["product"] }
```

| Use case                        | `templates`              |
| ------------------------------- | ------------------------ |
| Product part-payment calculator | `["product"]`            |
| Cart-level calculator           | `["cart"]`               |
| Generic marketing block         | omit (available everywhere) |

## Third-Party App Blocks

App blocks use the same schema format and respect `templates`, `private`, `allowed_blocks`, and `presets`. Themes constrain app blocks via section/container whitelists. For fixed integrations, use a static block slot:

```liquid
{% block 'app/part-payment' id: 'product-part-payment' %}
```

## Enforcement

Placement rules are enforced in the designer UI (filtered picker) and on the server (add-block API re-validates `templates`, `private`, and `allowed_blocks`). Validation currently runs on **block add** only — cross-container moves are not yet re-validated server-side.

## Best Practices

- Keep block logic simple and focused on a single purpose
- Use `presets` to offer ready-made block variants in the add-block picker
- Use `private` and `allowed_blocks` to enforce intentional placement
- Use layout blocks for composite components (product cards, etc.)
- Test blocks across sections and template types

## Full Reference

See the [Finqu blocks documentation](https://developers.finqu.com/build-with-finqu/liquid-themes/blocks.md.txt) for complete details.
