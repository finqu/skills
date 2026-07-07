# Theme Structure

A Finqu Liquid theme is organized into directories that separate concerns — layout, content, styling, and configuration.

## Directory Overview

| Directory        | Purpose                                                                                    |
| ---------------- | ------------------------------------------------------------------------------------------ |
| `assets`         | Source CSS, JS, images, fonts. `.liquid` and Sass files are processed on deploy.           |
| `blocks`         | Reusable components added to sections. Each block has its own settings via `{% schema %}`. |
| `config`         | `settings_schema.json` (schema definition) and `settings_data.json` (stored values).       |
| `layout`         | Layout files like `theme.liquid`. Define the overall page structure.                       |
| `locales`        | Storefront strings (`en.json`) and designer labels (`en.schema.json`) per language.        |
| `public`         | Compiled output written on deploy. Do not author here — reference via `asset_url`.         |
| `section-groups` | JSON definitions for shared section regions (header, footer). See `sections.md`.           |
| `sections`       | Page-level building blocks. Sections contain blocks and are the main structural elements.  |
| `snippets`       | Small reusable code fragments. Rendered in their own context, included via `{% render %}`. |
| `templates`      | Page templates for different store pages (e.g., `frontpage.liquid`, `product.liquid`).     |

## Required Files

Every theme must have:

- `layout/theme.liquid` — The main layout file
- `config/settings_schema.json` — Defines available theme settings
- `config/settings_data.json` — Stores current setting values
- At least one template in `templates/` (e.g., `frontpage.liquid`)
- A locale file in `locales/` for each supported language (e.g., `en.json`)

## Optional Components

- **Blocks** in `blocks/` — Modular components for sections (including layout blocks with child containers)
- **Section groups** in `section-groups/` — Shared, merchant-managed header/footer regions rendered with `{% sections %}`
- **Snippets** in `snippets/` — Reusable code pieces (icons, widgets, helpers)
- **Additional layouts** — For special page types (e.g., `campaign.liquid`)
- `config/.data/` — Used by the Finqu designer for merchant draft edits (do not edit manually)

## Locale Files

- `{lang}.json` — Storefront strings used in Liquid templates (`{{ 'key' | t }}`)
- `{lang}.schema.json` — Designer labels for sections, blocks, and settings (merged at runtime)

See `references/localization.md` for `t:` keys and preset localization.

## Deploy Lifecycle

1. **Develop locally** — Edit theme files; preview with Theme Development Kit (`finqu theme dev`)
2. **Deploy** — Run `finqu theme deploy` or publish from the partner portal. Finqu compiles `.scss.liquid` assets into `public/` on the merchant store
3. **Customize** — Merchants edit sections and settings in the admin designer; changes are draft until published

Your theme repo is the source of truth. After deploy, merchants customize via `settings_data.json` on the store — not by editing your repo directly.

## Asset Handling

- Author source files in `assets/`; deploy writes compiled output to `public/`
- Reference assets using the `asset_url` filter: `{{ 'main.css' | asset_url }}` — never hardcode `/public/` paths
- Sass files are compiled with Dart Sass
- `.liquid` files in `assets/` are processed and cachebusted automatically

## Full Reference

See the [Finqu theme structure documentation](https://developers.finqu.com/build-with-finqu/liquid-themes/theme-structure.md.txt) for complete details.
