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

Section, block, and setting labels shown in the **theme designer** can be translated separately from storefront copy using schema locale files:

```
locales/
  en.json
  en.schema.json
  fi.json
  fi.schema.json
```

- **`en.json`** — strings customers see on the storefront
- **`en.schema.json`** — labels merchants see in the designer (section names, setting labels, block titles)

Example `locales/fi.schema.json`:

```json
{
  "sections": {
    "hero": {
      "name": "Hero-banneri",
      "settings": {
        "heading": {
          "label": "Otsikko"
        }
      }
    }
  }
}
```

At runtime, schema translations are merged into the corresponding locale. You do not need to duplicate schema labels inside `fi.json` unless the same string is also used on the storefront.

## Inline Schema Localization

You can also localize schema inline when defining `{% schema %}`:

```liquid
{% schema %}
{
  "name": {
    "en": "Hero banner",
    "fi": "Hero-banneri"
  },
  "settings": [
    {
      "type": "text",
      "id": "heading",
      "label": {
        "en": "Heading",
        "fi": "Otsikko"
      }
    }
  ]
}
{% endschema %}
```

Use inline localization for small schemas; use `{lang}.schema.json` when you have many sections and blocks to translate.

## Best Practices

- Keep storefront copy in `{lang}.json` and designer labels in `{lang}.schema.json` or inline schema objects
- Ship a locale file for every language your theme claims to support
- Use the same language codes as your store's enabled locales (`en`, `fi`, `sv`, etc.)

## Full Reference

See the [Finqu localization documentation](https://developers.finqu.com/build-with-finqu/liquid-themes/localization.md.txt) for complete details.
