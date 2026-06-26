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
    }
  ]
}
{% endschema %}
```

## Schema Properties

| Property         | Required | Description                                                        |
| ---------------- | -------- | ------------------------------------------------------------------ |
| `name`           | Yes      | Display name (localized object)                                    |
| `tag`            | No       | HTML tag or identifier                                             |
| `class`          | No       | CSS classes applied to the section wrapper                         |
| `category`       | Yes      | Must match an entry in `settings_schema.json` `section_categories` |
| `keywords`       | No       | Search keywords (localized) for the editor                         |
| `settings`       | No       | Array of setting definitions                                       |
| `blocks`         | No       | Array of block type definitions (legacy)                           |
| `templates`      | No       | Template types the section is available on. Empty = all templates  |
| `section_groups` | No       | Restrict section to specific groups (group-only when set)          |
| `allowed_blocks` | No       | Whitelist of block names at the section's top level                |
| `is_creatable`   | No       | If `false`, merchants cannot add this section                    |

Template **type** is the part before the first dot — e.g. `product` and `product.custom` both have type `product`.

## Section Groups

A **section group** is an ordered, merchant-managed collection of sections shared across pages — for example a header or footer.

### Define a group

Create a JSON file under `section-groups/`. The filename (without extension) is the group name.

`section-groups/header-group.json`:

```json
{
  "name": { "en": "Header", "fi": "Ylätunniste" },
  "max_sections": 10,
  "allowed_sections": ["announcement-bar", "header", "navigation"],
  "default_sections": [
    {
      "name": "announcement-bar",
      "title": "Announcement",
      "settings": {},
      "blocks": [],
      "sticky": false
    },
    {
      "name": "header",
      "title": "Header",
      "settings": {},
      "blocks": [],
      "sticky": true
    }
  ]
}
```

| Field              | Description                                              |
| ------------------ | -------------------------------------------------------- |
| `name`             | Display name in the designer                             |
| `max_sections`     | Maximum sections merchants may add (default: 25)           |
| `allowed_sections` | Whitelist of section names. Empty = any opted-in section |
| `default_sections` | Initial layout before merchant customization             |

Render a group in a layout or template:

```liquid
{% sections 'header-group' %}
```

Group section data is stored on the release configuration under `section_groups[groupName].sections`, not on individual templates.

### Opt a section into a group

A section declares which group(s) it belongs to via `section_groups` in its schema. When set, the section is **only** addable inside those groups:

```liquid
{% schema %}
{
  "name": { "en": "Announcement bar" },
  "section_groups": ["header-group"],
  "settings": []
}
{% endschema %}
```

A section is offered for a group when **both** are satisfied:

1. The section's `section_groups` includes the group (or is empty), **and**
2. The group's `allowed_sections` includes the section (or is empty)

## Rendering Blocks in Sections

Use the `container` tag to render blocks assigned to the section:

```liquid
<div class="section-content">
  {% container 'main' %}
</div>
```

The `id` must be unique within the section and is required when the section contains multiple containers. Use `allowed_blocks` in the section schema to restrict which blocks merchants can add at the top level.

## Sticky Sections in Templates

Sections listed in a template's `template_sections` schema are **sticky**: always present, reorderable but not removable by merchants. See `references/templates-layouts.md`.

## Full Reference

See the [Finqu sections documentation](https://developers.finqu.com/build-with-finqu/liquid-themes/sections.md.txt) for complete details.
