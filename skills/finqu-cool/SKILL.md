---
name: finqu-cool
description: 'Cool UI toolkit usage — JavaScript components, CSS utility classes, theming with CSS custom properties, and integration into projects'
---

# Finqu Cool UI

## When to use

Use this skill when:

- Using Cool UI components (dropdowns, toasts, dialogs, selects, etc.) in a project
- Writing HTML markup with Cool UI classes and data attributes
- Initializing or configuring Cool UI components via JavaScript
- Theming or customizing appearance via CSS custom properties
- Integrating Cool UI into an application (ESM import, UMD script, jQuery)
- Debugging component behavior, styling, or initialization issues

## Inputs required

- **Target component(s)**: which Cool UI component to use (dropdown, toast, dialog, select, etc.)
- **Integration method**: ESM import, UMD `<script>`, or jQuery
- **Context**: whether the component is auto-initialized via HTML or created programmatically

## Procedure

### 0) Set up Cool UI

1. Install: `npm install @finqu/cool`
2. Import CSS — either the compiled file or SCSS source:
   ```html
   <link rel="stylesheet" href="@finqu/cool/dist/css/cool.css">
   ```
   ```scss
   @use "@finqu/cool/scss/cool";
   ```
3. Import JS — choose your format:
   ```js
   // ESM (tree-shakeable)
   import { Common, Toast, Dialog, Dropdown } from '@finqu/cool';

   // UMD bundle (includes jQuery + PerfectScrollbar)
   <script src="@finqu/cool/dist/js/cool.bundle.js"></script>

   // UMD standalone (bring your own jQuery)
   <script src="jquery.js"></script>
   <script src="@finqu/cool/dist/js/cool.js"></script>
   ```
4. Initialize all components on the page:
   ```js
   Common.initialize();
   // or with global settings:
   Common.initialize({
       toast: { placement: 'bottom-right', dismiss: 5 },
       select: { showSearch: true }
   });
   ```

Read: `references/integration.md`

### 1) Use components via HTML

1. Add `data-toggle="{component}"` to trigger elements. `Common.initialize()` auto-discovers and instantiates them.
2. Pass options via `data-*` attributes on the element.
3. Use the correct HTML structure and CSS classes for each component.

Read: `references/components.md`

### 2) Use components via JavaScript

1. Import or access the component class.
2. Create instances programmatically: `new Dropdown(triggerEl, { placement: 'top' })`.
3. Access existing instances: `Dropdown.getInstance(el)`.
4. Call methods: `instance.show()`, `instance.close()`, `instance.destroy()`.
5. For dynamically added DOM, re-scan with `Common.initComponent(selector)` or `Common.refresh()`.

Read: `references/components.md`

### 3) Customize appearance

1. Override CSS custom properties — all visual tokens use the `--cool-{component}-{property}` pattern:
   ```css
   :root {
       --cool-btn-bg: #3b82f6;
       --cool-btn-border-radius: 4px;
       --cool-dropdown-bg: #1e293b;
       --cool-toast-border-radius: 12px;
   }
   ```
2. Use utility classes for layout: `.d-flex`, `.gap-2`, `.p-3`, `.text-center`, etc.
3. Theme colors are available as `--cool-theme-color-{name}` (primary, secondary, success, info, warning, danger, light, medium, dark, brand).
4. Dark mode: set `data-theme="dark"` on a parent element, or use the ThemeSwitcher component.

Read: `references/theming.md`

### 4) Verify

1. Components initialize — check browser console for errors.
2. `data-toggle` elements become interactive.
3. Programmatic instances respond to method calls (`show`, `close`, etc.).
4. CSS custom property overrides reflect visually (browser dev tools → Computed).
5. Components work without jQuery (native-only mode).

## Verification

- Components render and respond to user interaction
- No console errors on initialization
- Toast/Dialog singletons show content when called programmatically
- Dropdowns position correctly relative to their triggers
- Select components report correct data via `getData()`
- CSS custom property overrides apply (inspect with browser dev tools)
- Theme switching works (light/dark/auto)

## Failure modes / debugging

- **Component not initializing**: Verify `Common.initialize()` is called after DOM ready and `data-toggle` value matches a component name
- **Options not applying**: Check merge order — constructor opts override data attributes override `window.Cool.settings` override DEFAULTS
- **Dropdown positioning wrong**: Check `data-placement` and `data-position` attributes; ensure parent is not `overflow: hidden`
- **Toast not showing**: Get the singleton via `Toast.getInstance(document.body)` or `window.Cool.getToast()`, then call `.init({ content: '...' })`
- **Select data not updating**: Call `instance.getData()` to inspect; use `instance.setData()` to set programmatically
- **Styles not applying**: Verify CSS is loaded; check that custom property names match `--cool-{component}-{property}` convention
- **jQuery interface missing**: Ensure using the UMD build (`cool.js` or `cool.bundle.js`), not the ESM build

## Escalation

- Review the [Cool UI source](https://github.com/finqu/cool) for component DEFAULTS and accepted options
- Check existing component files (`dropdown.js`, `toast.js`, `select.js`, `dialog.js`) for the full API
- Contact Finqu engineering for toolkit questions
