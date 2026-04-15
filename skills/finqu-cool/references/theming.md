# Theming

## CSS Custom Properties

All visual tokens are exposed as CSS custom properties on `:root`. Override them to customize any component.

### Property Naming Convention

| Pattern | Usage | Example |
|---------|-------|---------|
| `--cool-{component}-{property}` | Component token | `--cool-btn-bg`, `--cool-dropdown-border-color` |
| `--cool-theme-color-{name}` | Theme palette | `--cool-theme-color-primary`, `--cool-theme-color-danger` |
| `--cool-spacing-{n}` | Spacing scale | `--cool-spacing-0` (0) through `--cool-spacing-6` (30px) |
| `--cool-zindex-{name}` | Z-index stack | `--cool-zindex-dropdown`, `--cool-zindex-toast` |

### Override Examples

```css
:root {
    /* Buttons */
    --cool-btn-bg: #3b82f6;
    --cool-btn-border-radius: 4px;
    --cool-btn-padding-y: 10px;
    --cool-btn-padding-x: 16px;
    --cool-btn-font-size: 14px;

    /* Dropdowns */
    --cool-dropdown-bg: #ffffff;
    --cool-dropdown-border-color: #e2e8f0;
    --cool-dropdown-border-radius: 8px;
    --cool-dropdown-box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);

    /* Toasts */
    --cool-toast-bg: #1e293b;
    --cool-toast-border-radius: 12px;
    --cool-toast-offset-x: 20px;
    --cool-toast-offset-y: 20px;

    /* Dialog */
    --cool-dialog-border-radius: 12px;

    /* Tooltips */
    --cool-tooltip-bg: #0c0e1d;
    --cool-tooltip-border-radius: 4px;
}
```

### Component Variants

Variants override custom properties on the selector — they don't use `!important`:

```css
.btn-primary {
    --cool-btn-bg: var(--cool-theme-color-primary);
    --cool-btn-color: var(--cool-theme-foreground-color-primary);
}

.btn-danger {
    --cool-btn-bg: var(--cool-theme-color-danger);
    --cool-btn-color: var(--cool-theme-foreground-color-danger);
}
```

## Theme Colors

Available via `--cool-theme-color-{name}`:

| Name | Default | Usage |
|------|---------|-------|
| `primary` | `#0c0e1d` | Primary actions, active states |
| `secondary` | `#d0d9e4` | Secondary actions |
| `success` | `#62b47f` | Success feedback |
| `info` | `#448bbf` | Informational |
| `warning` | `#f0a030` | Warning states |
| `danger` | `#d86783` | Destructive actions, errors |
| `light` | `#eff3fa` | Light backgrounds |
| `medium` | `#62748e` | Muted text |
| `dark` | `#020618` | Dark backgrounds |
| `brand` | `#e70d68` | Brand accent |

Each theme color has corresponding `--cool-theme-foreground-color-{name}` for readable text on that background.

## Dark Mode

### Using Theme Switcher component

```html
<button data-toggle="theme-switcher" data-theme="dark">🌙</button>
<button data-toggle="theme-switcher" data-theme="light">☀️</button>
<button data-toggle="theme-switcher" data-theme="auto">🖥</button>
```

### Programmatic

```js
const ts = ThemeSwitcher.getInstance(el);
ts.setTheme('dark');    // 'light' | 'dark' | 'auto'
```

### Manual

Set `data-theme` attribute on a parent element:
```html
<html data-theme="dark">
```

Dark mode overrides are defined in `scss/_dark.scss`. Custom dark mode overrides:
```css
[data-theme="dark"] {
    --cool-btn-bg: #334155;
    --cool-dropdown-bg: #1e293b;
    --cool-dropdown-border-color: #334155;
}
```

## Utility Classes

28 utility modules are included. Key utilities:

| Category | Examples |
|----------|---------|
| Display | `.d-block`, `.d-flex`, `.d-grid`, `.d-none`, `.d-inline-flex` |
| Flex | `.flex-row`, `.flex-column`, `.gap-1` to `.gap-6`, `.justify-content-between`, `.align-items-center` |
| Spacing | `.m-0` to `.m-6`, `.p-0` to `.p-6`, `.mt-2`, `.px-3`, `.mb-auto` |
| Text | `.font-weight-bold`, `.text-center`, `.text-truncate`, `.font-size-sm` |
| Sizing | `.w-100`, `.h-100`, `.min-w-0`, `.max-w-100` |
| Borders | `.border`, `.border-radius-sm`, `.border-0` |
| Background | `.bg-primary`, `.bg-light`, `.bg-transparent` |
| Position | `.position-relative`, `.position-absolute`, `.top-0`, `.bottom-0` |
| Visibility | `.visible`, `.invisible`, `.overflow-hidden` |
| Cursor | `.cursor-pointer`, `.cursor-default` |
| Shadows | `.shadow-sm`, `.shadow`, `.shadow-lg` |
| Accessibility | `.sr-only`, `.visually-hidden` |

Spacing uses a 5px base: `0` = 0, `1` = 5px, `2` = 10px, `3` = 15px, `4` = 20px, `5` = 25px, `6` = 30px.

Responsive variants are available at breakpoints: `xs`, `xs-lg` (400px), `sm` (568px), `md` (768px), `lg` (1047px), `xl` (1280px).

```html
<div class="d-none d-md-flex gap-2 p-3">
    Visible only on md+ screens
</div>
```

## Grid

12-column grid system:

```html
<div class="row">
    <div class="col-6">Half</div>
    <div class="col-6">Half</div>
</div>

<div class="row">
    <div class="col-12 col-md-4">Third on md+</div>
    <div class="col-12 col-md-8">Two-thirds on md+</div>
</div>
```

## SCSS Customization

To customize at the Sass level, override variables before importing:

```scss
@use "@finqu/cool/scss/variables" with (
    $font-size-base: 14px,
    $btn-border-radius: 4px,
    $theme-colors: (
        "primary": #3b82f6,
        "danger": #ef4444
    )
);
@use "@finqu/cool/scss/cool";
```
