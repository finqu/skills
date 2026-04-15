# Integration

## Installation

```bash
npm install @finqu/cool
```

## Import Methods

### ESM (recommended)

```js
import { Common, Toast, Dialog, Dropdown, Select, Tooltip, Popover, Collapse, Sidebar, SectionTabs, ThemeSwitcher } from '@finqu/cool';

// Initialize all components
Common.initialize();
```

### UMD Bundle (includes jQuery + PerfectScrollbar)

```html
<script src="node_modules/@finqu/cool/dist/js/cool.bundle.js"></script>
<script>
    Cool.initialize();
</script>
```

### UMD with external jQuery

```html
<script src="jquery.min.js"></script>
<script src="node_modules/@finqu/cool/dist/js/cool.js"></script>
<script>
    Cool.initialize();
</script>
```

### Individual component plugins

Each component is also available as a standalone UMD file:
```html
<script src="dist/js/plugins/dropdown.js"></script>
```

## CSS

### Compiled CSS

```html
<link rel="stylesheet" href="node_modules/@finqu/cool/dist/css/cool.css">
```

### SCSS source (for customization)

```scss
// Import everything
@use "@finqu/cool/scss/cool";

// Or import individual partials
@use "@finqu/cool/scss/variables" as *;
@use "@finqu/cool/scss/root";
@use "@finqu/cool/scss/buttons";
@use "@finqu/cool/scss/dropdown";
```

All SCSS partials use `@use "variables" as *;` — never `@import`.

## Initialization

```js
// Basic — auto-discover all data-toggle elements
Common.initialize();

// With global component settings
Common.initialize({
    toast: { placement: 'bottom-right', dismiss: 5 },
    dialog: { size: 'md', centered: true },
    dropdown: { animation: false },
    select: { showSearch: true }
});
```

Global settings are accessible at `window.Cool.settings` and override component DEFAULTS.

## Global API (`window.Cool`)

After `Common.initialize()`, these are available:

```js
Cool.initialize(opts)           // Initialize all components
Cool.initComponent(selector)    // Init specific selector(s)
Cool.refresh()                  // Re-scan DOM for new data-toggle elements

Cool.getToast()                 // → Toast singleton
Cool.getSidebar()               // → Sidebar singleton
Cool.getSection()               // → Section singleton

Cool.observeResize(el, cb)      // ResizeObserver wrapper
Cool.addScrollListener(id, cb)  // Scroll event listener
```

## Options Merge Order

Every component merges options in this cascade (later wins):

```
1. ComponentClass.DEFAULTS        — hardcoded in the component
2. window.Cool.settings.{name}   — global runtime config
3. data-* attributes             — on the HTML element
4. constructor opts              — passed to new Component(el, opts)
```

## Peer Dependencies

jQuery (`^3.7.1`) and PerfectScrollbar (`^1.5.6`) are **optional**. The core runs without either.

- **jQuery**: Enables `$(el).coolDropdown('show')` syntax. Only available with UMD builds.
- **PerfectScrollbar**: Provides custom scrollbars in dropdowns and selects. Falls back to native scrolling.

## Package Exports

```json
{
    ".": {
        "import": "./dist/js/cool.esm.js",
        "require": "./dist/js/cool.bundle.js",
        "default": "./dist/js/cool.bundle.js"
    },
    "./scss/*": "./scss/*",
    "./dist/*": "./dist/*"
}
```
