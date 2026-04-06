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
    "placeholder": "Placeholder text"
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
| `title`         | Display-only heading                                         |
| `label`         | Display-only label                                           |
| `spacer`        | Visual spacer                                                |
| `separator`     | Visual separator                                             |
| `hidden`        | Hidden value                                                 |

## Conditional Rendering

Settings can show/hide based on other settings:

```json
{
    "id": "button_text",
    "type": "text",
    "label": { "en": "Button text" },
    "conditions": [{ "id": "show_button", "value": true, "operator": "eq" }]
}
```

**Operators:** `eq`, `gt`, `gte`, `lt`, `lte`, `contains`, `in`

## Grouping Settings

- **Tabs** — Horizontal tab groups (max 2 nested levels, max 4 per row)
- **Lists** — Vertical list groups (for many groups or simpler layout)

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
    "default": { ... }
  }
}
```

- `current` — Live settings
- `presets` — Predefined configurations for quick setup

## Full Reference

See the [Finqu settings documentation](https://developers.finqu.com/build-with-finqu/liquid-themes/settings.md.txt) for complete details.
