---
name: finqu-liquid-themes
description: 'Liquid theme development for Finqu — templates, sections, blocks, settings, layouts, assets, and checkout'
---

# Finqu Liquid Themes

## When to use

Use this skill when:

- Creating or modifying a Finqu Liquid theme
- Working with templates, sections, section groups, blocks, or layouts
- Editing `settings_schema.json` or `settings_data.json`
- Customizing the checkout experience
- Managing theme assets (CSS, JS, images)
- Debugging theme rendering issues

## Inputs required

- **Theme root**: path to the theme directory
- **Target file/component**: which template, section, block, or section group to modify
- **Finqu CLI authenticated**: `finqu sign-in` completed (for preview/deploy)

## Procedure

### 0) Understand theme structure

1. Verify the theme has required files:
    - `layout/theme.liquid` — main layout
    - `config/settings_schema.json` — settings schema
    - `config/settings_data.json` — settings values
    - At least one template in `templates/`
    - Locale files in `locales/` (e.g., `en.json`)
2. Identify what you're working with: template, section, section group, block, or asset.
3. Remember: source assets live in `assets/`; compiled output goes to `public/` on deploy.

Read: `references/theme-structure.md`

### 1) Templates and layouts

1. Templates live in `templates/` — one per page type (e.g., `product.liquid`, `cart.liquid`, `frontpage.liquid`).
2. Templates use `{% layout 'theme' %}` to inherit from a layout file. Use `{% layout 'none' %}` for standalone pages.
3. Layouts in `layout/` define global structure. Must contain `{{ content_for_header }}` and `{{ content_for_layout }}`.
4. Per-template sections render via `{{ content_for_index }}` (dynamic, editor-manageable).
5. Shared header/footer regions use section groups: `{% sections 'header-group' %}` in the layout.
6. Fixed sections use `{% section 'name' %}` — not editable by merchants.
7. Template variants (e.g., `product.campaign.liquid`) enable different layouts per product/page.
8. `customers/orders.liquid` is deprecated — order history lives in the checkout theme.

Read: `references/templates-layouts.md`

### 2) Sections

1. Sections live in `sections/` — modular, reorderable components.
2. Each section defines a `{% schema %}` JSON block with `name`, `tag`, `class`, `category`, `settings`, and optional placement flags.
3. Section groups are defined in `section-groups/*.json` and rendered with `{% sections 'group-name' %}`.
4. Sections can contain blocks rendered via `{% container 'id' %}`.
5. Use `section_groups`, `templates`, `allowed_blocks`, and `is_creatable` to control where sections appear.
6. The `category` must match entries in `settings_schema.json` `section_categories`.

Read: `references/sections.md`

### 3) Blocks

1. Blocks live in `blocks/` — reusable content elements inside sections.
2. Blocks define their own `{% schema %}` with `name`, `tag`, `class`, `category`, `settings`.
3. **Layout blocks** can contain child blocks in named containers (single level of nesting).
4. Use `private: true` and `allowed_blocks` to control block placement.
5. Use `templates` to scope blocks to specific page types.
6. Block `category` must match entries in `settings_schema.json` `block_categories`.

Read: `references/blocks.md`

### 4) Settings

1. Theme-level settings in `config/settings_schema.json` define global options (colors, fonts, layout).
2. Section and block settings defined in their `{% schema %}` blocks.
3. Values stored in `config/settings_data.json` under `current` (live) and `presets`.
4. Settings support conditional rendering via `conditions` (string array format).
5. Use `style_editor` + `style_picker` for reusable color style presets.
6. Available types include: text, textarea, color, select, checkbox, radio, radio_pill, range, number, datetime, richtext, image_picker, font_picker, margin, padding, border_radius, border, menu, category, product, article, page, url, style_editor, style_picker, and more.

Read: `references/settings.md`

### 5) Assets

1. Source assets live in `assets/` — CSS, JS, images, fonts.
2. Reference assets with the `asset_url` filter: `{{ 'main.css' | asset_url }}`. Never hardcode `/public/` paths.
3. `.liquid` files in assets are processed (Liquid variables substituted) and compiled on deploy.
4. Sass files (`.scss`, `.scss.liquid`) are compiled with Dart Sass into `public/`.
5. Use `{% stylesheet %}` and `{% javascript %}` for deduplicated inline CSS/JS across sections/blocks.
6. Use `inline_asset_content` and `svg_tag` filters for inline SVG icons.

Read: `references/assets.md`

### 6) Checkout customization

1. Checkout is a **separate theme** — own `templates/`, `layout/`, `assets/`, `config/`, `locales/`.
2. Checkout does **not** support sections, blocks, `{% form %}`, or container tags — all layout defined directly in Liquid templates.
3. Checkout uses **Vue.js** for dynamic features (cart updates, address validation, payment selection).
4. Checkout templates: `checkout.liquid`, `complete.liquid`, `download.liquid`, `order.liquid`, `orders.liquid`, `return.liquid`.
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

1. Storefront translations in `locales/{lang}.json` — use `{{ 'key' | t }}` in templates.
2. Designer labels in `locales/{lang}.schema.json` — separate from storefront copy.
3. All user-facing text in schema (`label`, `info`, `placeholder`) can also be localized inline:
    ```json
    { "en": "Add to cart", "fi": "Lisää ostoskoriin" }
    ```

Read: `references/localization.md`

## Verification

- Theme renders without Liquid errors
- `finqu theme dev` starts local preview successfully
- Section groups appear and are editable in theme designer
- Sections appear and are reorderable in theme editor
- Blocks render correctly inside sections and layout block containers
- Settings changes reflect in the storefront
- Assets load without 404s (check browser dev tools)
- Translations display correctly for configured locales
- After deploy, compiled CSS appears in `public/` and loads via `asset_url`

## Failure modes / debugging

- **Liquid syntax error**: Check `{% %}` and `{{ }}` tag balance, verify filter names
- **Section not appearing**: Verify `category` matches `section_categories` in `settings_schema.json`; check `templates` and `section_groups` restrictions
- **Block not rendering**: Ensure block `category` is in `block_categories`; check `allowed_blocks`, `private`, and `templates` placement rules
- **Section group empty**: Verify `section-groups/*.json` exists and section opts into the group via `section_groups`
- **Settings not updating**: Check `settings_data.json` for correct JSON structure, verify setting `id` matches template reference
- **Asset 404**: Verify file is in `assets/` and referenced with `{{ 'filename' | asset_url }}`; test after deploy since compile output goes to `public/`
- **Checkout Vue component not working**: Ensure Vue library scripts are loaded before component initialization; use `checkout.setUpApp(app)` and `checkout.components()`

## Escalation

- Consult [Liquid theme reference](https://developers.finqu.com/build-with-finqu/liquid-themes/theme-structure.md.txt) for structure details
- See [Liquid objects reference](https://developers.finqu.com/reference/liquid.md.txt) for available objects and filters
- See [Liquid tags reference](https://developers.finqu.com/reference/liquid/tags/store.md.txt) for store-specific tags
- Contact Finqu support for platform-specific issues
