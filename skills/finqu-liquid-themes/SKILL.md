---
name: finqu-liquid-themes
description: 'Liquid theme development for Finqu — templates, sections, blocks, settings, layouts, assets, and checkout'
---

# Finqu Liquid Themes

## When to use

Use this skill when:

- Creating or modifying a Finqu Liquid theme
- Working with templates, sections, blocks, or layouts
- Editing `settings_schema.json` or `settings_data.json`
- Customizing the checkout experience
- Managing theme assets (CSS, JS, images)
- Debugging theme rendering issues

## Inputs required

- **Theme root**: path to the theme directory
- **Target file/component**: which template, section, or block to modify
- **Finqu CLI authenticated**: `finqu sign-in` completed (for preview/deploy)

## Procedure

### 0) Understand theme structure

1. Verify the theme has required files:
    - `layout/theme.liquid` — main layout
    - `config/settings_schema.json` — settings schema
    - `config/settings_data.json` — settings values
    - At least one template in `templates/`
    - Locale files in `locales/` (e.g., `en.json`)

2. Identify what you're working with: template, section, block, or asset.

Read: `references/theme-structure.md`

### 1) Templates and layouts

1. Templates live in `templates/` — one per page type (e.g., `product.liquid`, `cart.liquid`, `frontpage.liquid`).
2. Templates use `{% layout 'theme' %}` to inherit from a layout file.
3. Layouts in `layout/` define global structure. Must contain `{{ content_for_header }}` and `{{ content_for_layout }}`.
4. Templates render sections via `{{ content_for_index }}` (dynamic, editor-manageable) or `{% section 'name' %}` (static).
5. Template variants (e.g., `product.campaign.liquid`) enable different layouts per product/page.

Read: `references/templates-layouts.md`

### 2) Sections

1. Sections live in `sections/` — modular, reorderable components.
2. Each section defines a `{% schema %}` JSON block with `name`, `tag`, `class`, `category`, `settings`, and optional `blocks`.
3. Sections are rendered at `{{ content_for_index }}` or with `{% section 'name' %}`.
4. Sections can contain blocks rendered via `{% container 'id' %}`.
5. The `category` must match entries in `settings_schema.json` `section_categories`.

Read: `references/sections.md`

### 3) Blocks

1. Blocks live in `blocks/` — reusable content elements inside sections.
2. Blocks define their own `{% schema %}` with `name`, `tag`, `class`, `category`, `settings`.
3. Blocks **cannot** contain other blocks or sections.
4. Block `category` must match entries in `settings_schema.json` `block_categories`.

Read: `references/blocks.md`

### 4) Settings

1. Theme-level settings in `config/settings_schema.json` define global options (colors, fonts, layout).
2. Section and block settings defined in their `{% schema %}` blocks.
3. Values stored in `config/settings_data.json` under `current` (live) and `presets`.
4. Settings support conditional rendering via `conditions` property.
5. Available types: text, textarea, color, select, checkbox, radio, range, number, richtext, image_picker, font_picker, margin, padding, border_radius, border, menu, category, product, article, page, url, and more.

Read: `references/settings.md`

### 5) Assets

1. Assets live in `assets/` — CSS, JS, images, fonts.
2. Reference assets with the `asset_url` filter: `{{ 'style.css' | asset_url }}`.
3. `.liquid` files in assets are processed (Liquid variables substituted) and cachebusted.
4. Sass files (`.scss`, `.scss.liquid`) are compiled with Dart Sass.
5. Public files in `public/` are directly accessible via browser.

Read: `references/assets.md`

### 6) Checkout customization

1. Checkout is a **separate theme** — own `templates/`, `layout/`, `assets/`, `config/`, `locales/`.
2. Checkout does **not** support sections or blocks — all layout defined directly in Liquid templates.
3. Checkout uses **Vue.js** for dynamic features (cart updates, address validation, payment selection).
4. Checkout templates: `checkout.liquid`, `complete.liquid`, `download.liquid`, `order.liquid`.
5. Vue library must be included before `</body>`:
    ```html
    <script src="https://cdn.finqu.com/lib-sdk/checkout-vue@1.3.5/vue.global.prod.js"></script>
    <script src="https://cdn.finqu.com/lib-sdk/checkout-vue@1.3.5/vue-i18n.global.prod.js"></script>
    <script src="https://cdn.finqu.com/lib-sdk/checkout-vue@1.3.5/components.umd.js"></script>
    ```

Read: `references/checkout.md`

### 7) Local development

1. Install CLI: `npm install -g @finqu/cli`
2. Sign in: `finqu sign-in`
3. Configure theme: `finqu theme configure`
4. Install Theme Development Kit (see CLI docs)
5. Start dev server: `finqu theme dev`
6. Deploy: `finqu theme deploy`

### 8) Localization

1. Translation files in `locales/` as JSON (e.g., `en.json`, `fi.json`).
2. All user-facing text in schema (`label`, `info`, `placeholder`) can be localized with language code objects:
    ```json
    { "en": "Add to cart", "fi": "Lisää ostoskoriin" }
    ```

## Verification

- Theme renders without Liquid errors
- `finqu theme dev` starts local preview successfully
- Sections appear and are reorderable in theme editor
- Blocks render correctly inside sections
- Settings changes reflect in the storefront
- Assets load without 404s (check browser dev tools)
- Translations display correctly for configured locales

## Failure modes / debugging

- **Liquid syntax error**: Check `{% %}` and `{{ }}` tag balance, verify filter names
- **Section not appearing**: Verify `category` matches `section_categories` in `settings_schema.json`
- **Block not rendering**: Ensure block `category` is in `block_categories` and section schema includes the block type
- **Settings not updating**: Check `settings_data.json` for correct JSON structure, verify setting `id` matches template reference
- **Asset 404**: Verify file is in `assets/` and referenced with `{{ 'filename' | asset_url }}`
- **Checkout Vue component not working**: Ensure Vue library scripts are loaded before component initialization

## Escalation

- Consult [Liquid theme reference](https://developers.finqu.com/build-with-finqu/liquid-themes/theme-structure.md.txt) for structure details
- See [Liquid objects reference](https://developers.finqu.com/reference/liquid.md.txt) for available objects and filters
- See [Liquid tags reference](https://developers.finqu.com/reference/liquid/tags/store.md.txt) for store-specific tags
- Contact Finqu support for platform-specific issues
