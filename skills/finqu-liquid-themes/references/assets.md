# Assets

Assets are static files (CSS, JavaScript, images, fonts) that make up the theme's visual presentation and interactivity.

## Asset Pipeline

1. **Author** — Put source files in `assets/` (e.g. `main.scss.liquid`, `logo.svg`, `app.js`)
2. **Deploy** — When you run `finqu theme deploy` or publish from the partner portal, Finqu processes `.liquid` assets (renders Liquid, compiles SCSS with Dart Sass) and writes compiled files to `public/` with cache-busted filenames
3. **Reference** — In templates, always use `{{ 'filename.css' | asset_url }}`. Never hardcode `/public/` paths

The `asset_url` filter resolves the correct URL for both source assets and compiled output after deploy.

## File Types

| File Type                             | Processing                                                |
| ------------------------------------- | --------------------------------------------------------- |
| `.css`                                | Served as-is with cachebusting                            |
| `.scss`, `.scss.liquid`               | Compiled with Dart Sass on deploy, then served            |
| `.liquid` (any extension + `.liquid`) | Liquid variables processed, then served with cachebusting |
| `.js`, `.js.liquid`                   | Served with cachebusting                                  |
| Images, fonts                         | Served as-is with cachebusting                            |

## Referencing Assets

Use the `asset_url` filter in templates:

```liquid
<link rel="stylesheet" href="{{ 'main.css' | asset_url }}">
<script src="{{ 'app.js' | asset_url }}" defer></script>
<img src="{{ 'logo.svg' | asset_url }}" alt="Logo">
```

## SCSS with Liquid

The main stylesheet is typically `assets/main.scss.liquid`:

```scss
$primary-color: {{ settings.color_primary }};

.btn-primary {
  background-color: $primary-color;
}
```

On deploy this compiles to a hashed CSS file in `public/`. Reference it as `main.css` via `asset_url`.

## Inline Assets

When the same CSS or JS is needed across multiple sections or blocks, use deduplication tags so content is included only once per page:

```liquid
{% stylesheet %}
  .shared-component { margin: 0; }
{% endstylesheet %}

{% javascript %}
  console.log('loaded once');
{% endjavascript %}
```

Tag bodies cannot contain Liquid. For dynamic values, use a `.scss.liquid` or `.js.liquid` file in `assets/` instead.

## Asset Filters

| Filter                 | Use case                                              |
| ---------------------- | ----------------------------------------------------- |
| `asset_url`            | URL for any file in `assets/` or compiled `public/`   |
| `inline_asset_content` | Inline SVG or small file content into HTML            |
| `svg_tag`              | Render an SVG asset with optional attributes          |

```liquid
<span class="icon">{{ 'icon-account.svg' | inline_asset_content }}</span>
{{ 'icon-chevron.svg' | svg_tag: class: 'chevron', width: 16, height: 16 }}
```

## Checkout Styles

Add `checkout.scss.liquid` to `assets/` to customize checkout appearance. This is a premium feature and may require a specific Finqu plan.

## Best Practices

- Keep source files in `assets/`; let deploy populate `public/`
- Always use `asset_url` — never hardcode paths
- Organize with subfolders (e.g. `assets/images/`, `assets/icons/`)
- Use `{% stylesheet %}` / `{% javascript %}` for shared inline code; use `assets/` for larger bundles
- Test after deploy on a staging store

## Full Reference

See the [Finqu assets documentation](https://developers.finqu.com/build-with-finqu/liquid-themes/assets.md.txt) for complete details.
