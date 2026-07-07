---
name: finqu-liquid-themes
description: 'Liquid theme development for Finqu — templates, sections, blocks, settings, layouts, assets, and checkout'
---

# Finqu Liquid Themes

## When to use

Use this skill when:

- Creating or modifying a Finqu Liquid theme
- Working with templates, sections, blocks, section groups, or layouts
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
    - Locale files in `locales/` (e.g., `en.json`, optionally `en.schema.json`)

2. Identify what you're working with: template, section, block, section group, or asset.

Read: `references/theme-structure.md`

### 1) Templates and layouts

1. Templates live in `templates/` — one per page type (e.g., `product.liquid`, `cart.liquid`, `frontpage.liquid`).
2. Templates use `{% layout 'theme' %}` to inherit from a layout file. Use `{% layout 'none' %}` for standalone pages.
3. Layouts in `layout/` define global structure. Must contain `{{ content_for_header }}` and `{{ content_for_layout }}`.
4. Render sections via:
    - `{% sections 'header-group' %}` — shared section groups (header/footer)
    - `{{ content_for_index }}` — dynamic, editor-manageable template content
    - `{% section 'name' %}` — static, fixed-position sections
5. Template variants (e.g., `product.campaign.liquid`) enable different layouts per product/page.
6. `customers/orders.liquid` is deprecated — order history lives in the checkout theme (`orders.liquid`).

Read: `references/templates-layouts.md`

### 2) Sections

1. Sections live in `sections/` — modular, reorderable components.
2. Each section defines a `{% schema %}` JSON block with `name`, `category`, `settings`, and optional `blocks`, `containers`, `presets`, `section_groups`.
3. Section groups are defined in `section-groups/*.json` and rendered with `{% sections 'group-name' %}`.
4. Sections contain blocks rendered via `{% container 'id' %}`.
5. The `category` must match entries in `settings_schema.json` `section_categories`.

Read: `references/sections.md`

### 3) Blocks

1. Blocks live in `blocks/` — reusable content elements inside sections.
2. Blocks define their own `{% schema %}` with `name`, `category`, `settings`, and optional `containers` (layout blocks).
3. Layout blocks contain child blocks in named containers (single level of nesting).
4. Use `private: true` and `allowed_blocks` to control where blocks can be placed.
5. Block `category` must match entries in `settings_schema.json` `block_categories`.

Read: `references/blocks.md`

### 4) Settings

1. Theme-level settings in `config/settings_schema.json` define global options (colors, fonts, layout).
2. Section and block settings defined in their `{% schema %}` blocks (can use grouped tabs/lists).
3. Values stored in `config/settings_data.json` under `current` and `presets`.
4. Settings support conditional rendering via `conditions` (string format: `"id eq value"`).
5. Repeatable sidebar items use `setting_blocks` — distinct from theme `{% block %}` instances.
6. Theme-level `style-editor` / `style-picker` for reusable style presets.

Read: `references/settings.md`

### 5) Assets

1. Author source files in `assets/` — CSS, JS, images, fonts.
2. Reference assets with the `asset_url` filter: `{{ 'main.css' | asset_url }}`.
3. Deploy compiles `.scss.liquid` to hashed files in `public/`.
4. Use `{% stylesheet %}` and `{% javascript %}` for deduplicated inline CSS/JS across sections.
5. Asset filters: `inline_asset_content`, `svg_tag`.

Read: `references/assets.md`

### 6) Checkout customization

1. Checkout is a **separate theme** — own `templates/`, `layout/`, `assets/`, `config/`, `locales/`.
2. Checkout does **not** support sections, blocks, `{% form %}`, or container tags.
3. Checkout uses **Vue.js** for dynamic features (cart updates, address validation, payment selection).
4. Checkout templates: `checkout.liquid`, `complete.liquid`, `download.liquid`, `order.liquid`, `orders.liquid`, `return.liquid`.
5. Required libraries — Bootstrap CSS in `<head>`, Vue and Bootstrap JS before `</body>`:
    ```html
    <link rel="stylesheet" href="https://static.finqu.com/lib-sdk/bootstrap@5.3.8/bootstrap.min.css">

    <script src="https://static.finqu.com/lib-sdk/checkout-vue@1.3.6/vue.global.prod.js"></script>
    <script src="https://static.finqu.com/lib-sdk/checkout-vue@1.3.6/vue-i18n.global.prod.js"></script>
    <script src="https://static.finqu.com/lib-sdk/checkout-vue@1.3.6/customer-portal.umd.js"></script>
    <script src="https://static.finqu.com/lib-sdk/bootstrap@5.3.8/bootstrap.bundle.min.js"></script>
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

1. Storefront strings in `locales/{lang}.json` — use `{{ 'key' | t }}` in templates.
2. Designer labels in `locales/{lang}.schema.json` or inline `{ "en": "...", "fi": "..." }` objects in schemas.
3. Reference schema translations with `t:` keys (e.g. `"label": "t:sections.hero.settings.heading.label"`).
4. Use locale-keyed preset maps for market-specific default content.

Read: `references/localization.md`

## Verification

- Theme renders without Liquid errors
- `finqu theme dev` starts local preview successfully
- Section groups and template sections appear and are reorderable in theme editor
- Blocks render correctly inside sections and layout block containers
- Settings changes reflect in the storefront
- Assets load without 404s (check browser dev tools)
- Translations display correctly for configured locales (storefront and designer preview language)

## Failure modes / debugging

- **Liquid syntax error**: Check `{% %}` and `{{ }}` tag balance, verify filter names
- **Section not appearing**: Verify `category` matches `section_categories` in `settings_schema.json`; check `templates` and `section_groups` restrictions
- **Block not rendering**: Ensure block `category` is in `block_categories`; check `allowed_blocks`, `private`, and `templates` scope
- **Section group empty**: Verify group JSON in `section-groups/` and section's `section_groups` / group's `allowed_sections` whitelists align
- **Settings not updating**: Check `settings_data.json` for correct JSON structure, verify setting `id` matches template reference
- **Asset 404**: Use `{{ 'filename' | asset_url }}` — do not reference `/public/` directly; redeploy if SCSS changed
- **Checkout Vue component not working**: Ensure Vue library scripts are loaded and `checkout.setUpApp(app)` is called before mount

## Escalation

- Consult [Liquid theme reference](https://developers.finqu.com/build-with-finqu/liquid-themes/theme-structure.md.txt) for structure details
- See [Localization](https://developers.finqu.com/build-with-finqu/liquid-themes/localization.md.txt) for schema translations and preset localization
- See [Liquid objects reference](https://developers.finqu.com/reference/liquid.md.txt) for available objects and filters
- See [Liquid tags reference](https://developers.finqu.com/reference/liquid/tags/store.md.txt) for store-specific tags
- Contact Finqu support for platform-specific issues
