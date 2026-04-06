# Assets

Assets are static files (CSS, JavaScript, images, fonts) that make up the theme's visual presentation and interactivity.

## Asset Directory

All assets are stored in the `assets/` directory. Special processing applies to certain file types:

| File Type                             | Processing                                                |
| ------------------------------------- | --------------------------------------------------------- |
| `.css`                                | Served as-is with cachebusting                            |
| `.scss`, `.scss.liquid`               | Compiled with Dart Sass, then served                      |
| `.liquid` (any extension + `.liquid`) | Liquid variables processed, then served with cachebusting |
| `.js`                                 | Served as-is with cachebusting                            |
| Images, fonts                         | Served as-is with cachebusting                            |

## Referencing Assets

Use the `asset_url` filter in templates:

```liquid
<!-- CSS -->
<link rel="stylesheet" href="{{ 'style.css' | asset_url }}">

<!-- JavaScript -->
<script src="{{ 'app.js' | asset_url }}"></script>

<!-- Images -->
<img src="{{ 'logo.svg' | asset_url }}" alt="Logo">

<!-- Liquid-processed CSS -->
<link rel="stylesheet" href="{{ 'theme.css.liquid' | asset_url }}">
```

## Public Directory

Files in `public/` are served directly via browser without any processing. Use for files that need a stable, predictable URL.

## Sass Support

Sass files are compiled with Dart Sass. You can use:

- Variables, mixins, nesting
- `@import` for partials
- `.scss.liquid` extension for Liquid variables inside Sass

Example `theme.scss.liquid`:

```scss
$primary: {{ settings.primary_color }};

.button {
  background-color: $primary;
  &:hover {
    background-color: darken($primary, 10%);
  }
}
```

## Best Practices

- Use `asset_url` for all asset references — never hardcode paths
- Organize assets logically (separate CSS, JS, images)
- Minimize inline styles — use stylesheets
- Leverage Sass for maintainable CSS
- Keep file sizes reasonable for performance

## Full Reference

See the [Finqu assets documentation](https://developers.finqu.com/build-with-finqu/liquid-themes/assets.md.txt) for complete details.
