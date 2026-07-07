# Settings

Settings allow merchants to customize theme appearance and behavior without editing code.

## Setting Levels

| Level            | Defined in                               | Scope                            |
| ---------------- | ---------------------------------------- | -------------------------------- |
| Theme settings   | `config/settings_schema.json`            | Global — applies to entire store |
| Section settings | Section's `{% schema %}`                 | Per section instance             |
| Block settings   | Block's `{% schema %}`                   | Per block instance               |

All values are stored in `config/settings_data.json`.

## settings_schema.json Structure

```json
{
  "name": "theme-identifier",
  "theme_name": "My Theme",
  "theme_author": "Author Name",
  "theme_image": "assets/theme-image.jpg",
  "theme_documentation_url": "https://docs.example.com",
  "theme_demo_url": "https://demo.example.com",
  "section_categories": [{ "id": "theme-featured", "name": { "en": "Featured" } }],
  "block_categories": [{ "id": "content", "name": { "en": "Content" } }],
  "settings": [ ... ]
}
```

## Common Setting Properties

```json
{
  "id": "unique_id",
  "type": "setting_type",
  "label": { "en": "Display Label", "fi": "Näyttönimi" },
  "info": "Help text shown to the user",
  "default": "default_value",
  "placeholder": "Placeholder text"
}
```

All user-facing text (`label`, `info`, `placeholder`) can be localized with language code objects or `t:` keys. See `references/localization.md`.

## Grouping Settings

**Theme settings** can use `setting_page` for nested pages:

```json
{
  "type": "setting_page",
  "label": "Colors",
  "settings": [ ... ]
}
```

**Section/block settings** can use grouped settings:

- **Tabs** — Horizontal tab groups (max 2 nested levels, max 4 per row)
- **Lists** — Vertical list groups (for many groups or simpler layout)

```json
{
  "settings": {
    "list_type": "tabs",
    "groups": [
      { "title": "General", "settings": [ ... ] },
      { "title": "Slides", "id": "slides", "is_sortable": true, "setting_blocks": [ ... ] }
    ]
  }
}
```

## Setting Blocks

Setting blocks define **repeatable settings item types** in the sidebar (e.g. carousel slides). Stored as `setting_blocks` data — not theme `{% block %}` instances.

Access in Liquid:

```liquid
{% for block in section.setting_blocks.slides %}
  {% if block.type == 'slide' %}
    ...
  {% endif %}
{% endfor %}
```

Setting blocks must be defined in their own tab or list — do not mix with other settings in the same group.

## Style Editor and Style Picker

**Theme-level only.** Define reusable color/style presets with `style-editor`, then reference them in sections/blocks with `style-picker`:

```json
{ "type": "style-editor", "id": "styles", "presets": { ... } }
{ "type": "style-picker", "id": "button_style", "styles_id": "styles" }
```

The stored value of a `style-picker` is the preset id string (e.g. `"primary"`).

## Conditional Rendering

Settings can show/hide based on other settings using string conditions:

```json
{
  "id": "button_text",
  "type": "text",
  "label": { "en": "Button text" },
  "conditions": ["show_button eq true"]
}
```

Multiple conditions — all must be true:

```json
"conditions": ["show_text eq true", "value gt 5"]
```

**Operators:** `eq`, `gt`, `gte`, `lt`, `lte`, `contains`, `in`

## Available Setting Types

| Type            | Description                                                  |
| --------------- | ------------------------------------------------------------ |
| `text`          | Single-line text input                                       |
| `textarea`      | Multi-line text input                                        |
| `color`         | Color picker (supports `nullable`)                           |
| `select`        | Dropdown with options                                        |
| `checkbox`      | Boolean toggle                                               |
| `radio`         | Radio button group                                           |
| `radio_pill`    | Radio buttons as pills (max 5 options)                       |
| `range`         | Numeric slider (min/max/step)                                |
| `number`        | Numeric input                                                |
| `datetime`      | Date and time picker                                         |
| `richtext`      | Rich text editor                                             |
| `image_picker`  | Image selector/uploader                                      |
| `font_picker`   | Font family selector                                         |
| `margin`        | CSS margin (top/right/bottom/left)                           |
| `padding`       | CSS padding                                                  |
| `border_radius` | CSS border-radius                                            |
| `border`        | Border with width/style/color                                |
| `menu`          | Menu selector                                                |
| `category`      | Category autocomplete                                        |
| `manufacturer`  | Manufacturer autocomplete                                    |
| `product`       | Product autocomplete                                         |
| `article`       | Article autocomplete                                         |
| `page`          | Page autocomplete                                            |
| `url`           | URL autocomplete (supports `include_text`, `include_target`) |
| `heading`       | Display-only heading                                         |
| `label`         | Display-only label                                           |
| `spacer`        | Visual spacer                                                |
| `separator`     | Visual separator                                             |
| `hidden`        | Hidden value                                                 |
| `setting_page`  | Nested settings page (theme settings only)                   |
| `style-editor`  | Theme-level style preset definitions                         |
| `style-picker`  | Select a style preset from a `style-editor`                  |

## settings_data.json

Stores all current values:

```json
{
  "current": {
    "primary_color": "#333333",
    "sections": {
      "header": { "type": "header", "settings": { ... } }
    }
  },
  "presets": {
    "default": { "name": "Default", "settings": { ... } }
  }
}
```

- `current` — Live settings
- `presets` — Predefined configurations for quick setup (supports locale-keyed snapshots)

## Full Reference

See the [Finqu settings documentation](https://developers.finqu.com/build-with-finqu/liquid-themes/settings.md.txt) for complete details.
