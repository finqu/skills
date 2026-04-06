# Blocks

Blocks are modular content elements that live inside sections. They enable flexible, customizable content within section containers.

## How Blocks Work

- Stored in the `blocks/` directory — one file per block type
- Rendered inside sections using the `{% container %}` tag
- Each block has its own `{% schema %}` with settings
- Blocks **cannot** contain other blocks or sections
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

| Property   | Required | Description                                                      |
| ---------- | -------- | ---------------------------------------------------------------- |
| `name`     | Yes      | Display name (localized object)                                  |
| `tag`      | No       | HTML tag (default: `div`)                                        |
| `class`    | No       | CSS classes applied to the block wrapper                         |
| `category` | Yes      | Must match an entry in `settings_schema.json` `block_categories` |
| `settings` | No       | Array of setting definitions                                     |

## Block Example

```liquid
<div class="faq-item">
  <h3 class="faq-question">{{ block.settings.question }}</h3>
  <div class="faq-answer">{{ block.settings.answer }}</div>
</div>

{% schema %}
{
  "name": { "en": "FAQ Item" },
  "tag": "div",
  "class": "block block-faq-item",
  "category": "content",
  "settings": [
    { "id": "question", "type": "text", "label": { "en": "Question" } },
    { "id": "answer", "type": "richtext", "label": { "en": "Answer" } }
  ]
}
{% endschema %}
```

## Rendering Blocks

Blocks are rendered inside sections using the `container` tag:

```liquid
{% container 'main' %}
```

Or rendered statically with the `block` tag:

```liquid
{% block 'faq-item' %}
```

## Best Practices

- Keep block logic simple and focused on a single purpose
- Use clear, descriptive names and categories
- Test blocks in different sections for compatibility
- Organize settings logically for the editor experience

## Full Reference

See the [Finqu blocks documentation](https://developers.finqu.com/build-with-finqu/liquid-themes/blocks.md.txt) for complete details.
