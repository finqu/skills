# Settings

Settings allow merchants to customize theme appearance and behavior without editing code.

## Setting Levels

| Level            | Defined in                               | Scope                            |
| ---------------- | ---------------------------------------- | -------------------------------- |
| Theme settings   | `config/settings_schema.json`            | Global — applies to entire store |
| Section settings | Section's `{% schema %}`                 | Per section instance             |
| Block settings   | Block's `{% schema %}` or section schema | Per block instance               |

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
  "settings": [
    {
      "id": "primary_color",
      "type": "color",
      "label": { "en": "Primary color" },
      "default": "#000000"
    }
  ]
}
```

## Common Setting Properties

Every setting supports:

```json
{
  "id": "unique_id",
  "type": "setting_type",
  "label": { "en": "Display Label", "fi": "Näyttönimi" },
  "info": "Help text shown to the user",
  "default": "default_value",
  "placeholder": "Placeholder text",
  "conditions": ["show_button eq true"]
}
```

All user-facing text (`label`, `info`, `placeholder`) can be localized with language code objects.

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
| `richtext`      | Rich text editor (`size`, `mode`)                            |
| `image_picker`  | Image selector/uploader (returns object with `src`, etc.)    |
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
| `title`         | Display-only heading                                         |
| `label`         | Display-only label                                           |
| `spacer`        | Visual spacer                                                |
| `separator`     | Visual separator                                             |
| `hidden`        | Hidden value                                                 |
| `setting_page`  | Nested settings page for grouping theme settings             |
| `style_editor`  | Theme-level color style presets (used with `style_picker`)   |
| `style_picker`  | Select a preset from a `style_editor` (sections/blocks)      |

## Style Picker and Style Editor

These form a **define-and-select** pattern for reusable color styles:

1. **Define** presets in theme settings with `style_editor` (creates named color presets)
2. **Select** a preset in section/block settings with `style_picker` via `styles_id`

The stored value of a `style_picker` is the preset id string (e.g. `"general"`, `"dark"`).

## Conditional Rendering

Settings can show/hide based on other settings using string conditions:

```json
{
  "id": "button_text",
  "type": "text",
  "label": { "en": "Button text" },
  "conditions": ["show_button eq true", "items_count gte 10"]
}
```

All conditions must be true for the setting to be shown.

**Operators:** `eq`, `gt`, `gte`, `lt`, `lte`, `contains`, `in`

## Grouping Settings

- **Setting pages** — Use `setting_page` type to nest theme settings hierarchically
- **Tabs** — Horizontal tab groups (max 2 nested levels, max 4 per row)
- **Lists** — Vertical list groups (for many groups or simpler layout)
- **Setting blocks** — Reusable sortable blocks within section settings (must be in their own tab/list)

## settings_data.json

Stores all current values:

```json
{
  "current": {
    "primary_color": "#333333",
    "sections": {
      "header": { "type": "header", "settings": {} }
    }
  },
  "presets": {
    "default": {
      "name": "Default",
      "default": true,
      "settings": {
        "primary_color": "#333333"
      }
    }
  }
}
```

- `current` — Live settings
- `presets` — Predefined configurations for quick setup

## Full Reference

See the [Finqu settings documentation](https://developers.finqu.com/build-with-finqu/liquid-themes/settings.md.txt) for complete details.
