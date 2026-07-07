# Localization

Finqu themes support multiple languages through JSON files in the `locales/` directory.

## Storefront Translations

Each language needs a locale file:

```
locales/
  en.json
  fi.json
  sv.json
```

Use translation keys in Liquid templates:

```liquid
{{ 'general.add_to_cart' | t }}
```

Keys are nested in the JSON file:

```json
{
    "general": {
        "add_to_cart": "Add to cart"
    }
}
```

The active language comes from the customer's locale (`request.locale`).

## Schema Translations

Section, block, and setting labels shown in the **theme designer** can be translated separately from storefront copy:

```
locales/
  en.json
  en.schema.json
  fi.json
  fi.schema.json
```

| File                 | Purpose                                                           |
| -------------------- | ----------------------------------------------------------------- |
| `{lang}.json`        | Strings customers see on the storefront                           |
| `{lang}.schema.json` | Labels merchants see in the designer (sections, blocks, settings) |

Schema translations are merged into the corresponding locale at runtime. You do not need to duplicate schema labels inside `fi.json` unless the same string is also used on the storefront.

## Translation Keys (`t:`)

In schema JSON, reference schema locale files with a `t:` prefix:

```json
{
    "type": "checkbox",
    "id": "showPromotion",
    "label": "t:blocks.product-title-price.settings.showPromotion.label"
}
```

The path after `t:` maps to nested keys in `{lang}.schema.json`:

```json
{
    "blocks": {
        "product-title-price": {
            "settings": {
                "showPromotion": {
                    "label": "Näytä tarjous"
                }
            }
        }
    }
}
```

**Rules:**

- Use nested JSON objects, not flat keys
- Keys are case-sensitive
- Missing keys fall back to `en.schema.json`
- On the storefront, `t:` values resolve when Liquid reads setting values
- In the designer, `t:` values resolve when schemas and preset data load for the active preview language

## Inline Localization Objects

You can also localize schema labels inline:

```json
{
    "name": { "en": "Hero banner", "fi": "Hero-banneri" },
    "label": { "en": "Heading", "fi": "Otsikko" }
}
```

| Format                         | Best for                           |
| ------------------------------ | ---------------------------------- |
| `t:sections.hero.name`         | Large themes, many sections/blocks |
| `{ "en": "...", "fi": "..." }` | Small schemas, few labels          |

Both formats work in the same theme.

## Presets

Presets ship default configuration for themes, sections, and blocks. For market-specific defaults, define **full preset content per locale** — not just translated labels.

Resolution order: active locale → `default` → first available entry.

**Locale-keyed preset map:**

```json
"presets": {
  "fi": [{ "name": "Oletus", "settings": { "heading": "Tervetuloa" } }],
  "default": [{ "name": "Default", "settings": { "heading": "Welcome" } }]
}
```

Theme-level presets in `settings_data.json` can include locale-specific snapshots under each preset id (`default`, `fi`, etc.).

See the [Finqu localization documentation](https://developers.finqu.com/build-with-finqu/liquid-themes/localization.md.txt) for theme-level, section-level, and setting-level preset patterns.

## Best Practices

- Keep storefront copy in `{lang}.json` and designer labels in `{lang}.schema.json` or inline schema objects
- Use locale-keyed preset maps to ship market-specific rich text, images, and layout together
- Test the designer with the preview language picker, not only the merchant UI language
- Ship a locale file for every language your theme claims to support

## Full Reference

See the [Finqu localization documentation](https://developers.finqu.com/build-with-finqu/liquid-themes/localization.md.txt) for complete details.
