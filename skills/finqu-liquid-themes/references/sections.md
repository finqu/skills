# Sections

Sections are modular components that define flexible, customizable layouts in Finqu themes.

## How Sections Work

- Stored in the `sections/` directory — one file per section
- Rendered via `{{ content_for_index }}` (dynamic), `{% section 'name' %}` (static), or `{% sections 'group-name' %}` (section group)
- Contain Liquid markup, logic, and a `{% schema %}` block
- Can include blocks rendered via `{% container 'id' %}`

## Section Schema

Every section defines its configuration in a `{% schema %}` tag:

```liquid
{% schema %}
{
  "name": { "en": "Featured Products", "fi": "Suositellut tuotteet" },
  "description": { "en": "A grid of featured products." },
  "tag": "section",
  "class": "section section-featured-products",
  "category": "theme-featured",
  "keywords": {
    "en": ["featured", "products", "grid"],
    "fi": ["suositellut", "tuotteet"]
  },
  "settings": [ ... ],
  "blocks": [ ... ]
}
{% endschema %}
```

## Schema Properties

| Property         | Required | Description                                                                 |
| ---------------- | -------- | --------------------------------------------------------------------------- |
| `name`           | Yes      | Display name (localized object or `t:` key)                                 |
| `description`    | No       | Description shown in the editor                                             |
| `category`       | Yes      | Must match an entry in `settings_schema.json` `section_categories`          |
| `tag`            | No       | HTML tag or identifier                                                      |
| `class`          | No       | CSS classes applied to the section wrapper                                  |
| `keywords`       | No       | Search keywords for the add-section picker (string array or localized)      |
| `templates`      | No       | Template types the section is available on. Empty/absent = all templates    |
| `section_groups` | No       | Restrict section to specific section groups (group-only when set)           |
| `is_creatable`   | No       | If `false`, merchants cannot add this section. Default `true`               |
| `allowed_blocks` | No     | Whitelist of block names at the section's top level                         |
| `settings`       | No       | Setting definitions (array or grouped object with `groups`)                   |
| `containers`     | No       | Named drop zones within the section, each with optional preset blocks       |
| `presets`        | No       | Predefined variants shown in the add-section picker                         |

See the [section schema definition](https://schemas.finqu.com/liquid/section.schema.json) for the authoritative shape.

## Section Groups

Section groups are shared, merchant-managed collections of sections (e.g. header, footer).

**Define a group** — `section-groups/header-group.json`:

```json
{
  "name": { "en": "Header", "fi": "Ylätunniste" },
  "max_sections": 10,
  "allowed_sections": ["announcement-bar", "header", "navigation"],
  "default_sections": [
    { "name": "announcement-bar", "title": "Announcement", "settings": {}, "blocks": [], "sticky": false },
    { "name": "header", "title": "Header", "settings": {}, "blocks": [], "sticky": true }
  ]
}
```

**Render a group** in layout or template:

```liquid
{% sections 'header-group' %}
```

**Opt a section into a group** via `section_groups` in the section schema:

```json
{ "section_groups": ["header-group"] }
```

A section is offered for a group when both the section's `section_groups` and the group's `allowed_sections` allow it.

## Rendering Blocks in Sections

```liquid
<div class="section-content">
  {% container 'main' %}
</div>
```

- An unnamed `{% container %}` renders blocks with no container assignment
- Container `id` must be unique within the section when multiple containers exist
- Use `allowed_blocks` in the section schema to restrict top-level blocks

Repeatable sidebar item types (e.g. carousel slides) use `setting_blocks` inside grouped settings — not theme `{% block %}` instances. See `references/settings.md`.

## Section Containers

For sections with multiple drop zones, declare `containers` in the schema:

```liquid
<section class="two-column">
  <div class="two-column__left">{% container 'left' %}</div>
  <div class="two-column__right">{% container 'right' %}</div>
</section>

{% schema %}
{
  "name": { "en": "Two columns" },
  "category": "theme-featured",
  "containers": [
    { "id": "left", "title": { "en": "Left column" } },
    { "id": "right", "title": { "en": "Right column" }, "allowed_blocks": ["rich-text"] }
  ]
}
{% endschema %}
```

Container `preset.blocks` seeds read-only fallback blocks when a statically imported section has no stored configuration.

## Section Presets

The `presets` array defines ready-made variants in the add-section picker. Unlike container presets, section presets are applied eagerly when a merchant picks one:

```json
"presets": [
  {
    "name": { "en": "Centered" },
    "default": true,
    "settings": { "heading": "Welcome", "alignment": "center" }
  },
  {
    "name": { "en": "With button" },
    "blocks": [{ "name": "buy-button", "settings": { "label": "Shop now" } }]
  }
]
```

Preset `blocks` entries support `name`, `id`, `settings`, `container`, and nested `blocks`.

## Sticky Sections

Sections in a template's `template_sections` are sticky: always present, reorderable but not removable. All sticky and user-added sections render at `{{ content_for_index }}`.

## Full Reference

See the [Finqu sections documentation](https://developers.finqu.com/build-with-finqu/liquid-themes/sections.md.txt) for complete details.
