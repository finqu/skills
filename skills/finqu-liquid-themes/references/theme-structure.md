# Theme Structure

A Finqu Liquid theme is organized into directories that separate concerns — layout, content, styling, and configuration.

## Directory Overview

| Directory   | Purpose                                                                                    |
| ----------- | ------------------------------------------------------------------------------------------ |
| `assets`    | Images, stylesheets, JavaScript, fonts. `.liquid` and Sass files get special processing.   |
| `blocks`    | Reusable components added to sections. Each block has its own settings via `{% schema %}`. |
| `config`    | `settings_schema.json` (schema definition) and `settings_data.json` (stored values).       |
| `layout`    | Layout files like `theme.liquid`. Define the overall page structure.                       |
| `locales`   | Translation JSON files per language (e.g., `en.json`, `fi.json`).                          |
| `public`    | Publicly accessible files — served directly via browser without processing.                |
| `sections`  | Page-level building blocks. Sections contain blocks and are the main structural elements.  |
| `snippets`  | Small reusable code fragments. Rendered in their own context, included via `{% render %}`. |
| `templates` | Page templates for different store pages (e.g., `frontpage.liquid`, `product.liquid`).     |

## Required Files

Every theme must have:

- `layout/theme.liquid` — The main layout file
- `config/settings_schema.json` — Defines available theme settings
- `config/settings_data.json` — Stores current setting values
- At least one template in `templates/` (e.g., `frontpage.liquid`)
- At least one locale file in `locales/` (e.g., `en.json`)

## Optional Components

- **Blocks** in `blocks/` — Modular components for sections
- **Snippets** in `snippets/` — Reusable code pieces (icons, widgets, helpers)
- **Additional layouts** — For special page types (e.g., `campaign.liquid`)
- `.data` directory — Used by the Finqu editor for drafts (do not edit manually)

## Asset Handling

- Reference assets using the `asset_url` filter: `{{ 'my_file.svg' | asset_url }}`
- Sass files are compiled with Dart Sass
- `.liquid` files in `assets/` are processed and cachebusted automatically
- Files in `public/` are served as-is without processing

## Full Reference

See the [Finqu theme structure documentation](https://developers.finqu.com/build-with-finqu/liquid-themes/theme-structure.md.txt) for complete details.
