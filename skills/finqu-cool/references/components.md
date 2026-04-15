# Components

## Auto-Initialization

All components auto-initialize when `Common.initialize()` is called. It scans the DOM for `data-toggle` attributes and creates instances.

```js
import { Common } from '@finqu/cool';
Common.initialize();
```

For dynamically added elements, re-scan:
```js
Common.initComponent('#new-content');   // Single selector
Common.initComponent(['#a', '#b']);     // Array of selectors
Common.refresh();                       // Re-scan everything
```

## Instance Access

```js
// Get an existing instance
const dd = Dropdown.getInstance(element);

// Create a new instance programmatically
const dd = new Dropdown(triggerEl, { placement: 'top' });

// All components share these lifecycle callbacks:
new Dropdown(el, {
    onInit: () => {},
    onShow: () => {},
    onClose: () => {},
    onDestroy: () => {},
    onUpdate: () => {}
});
```

---

## Dropdown

Positioned menus that open on click.

**HTML:**
```html
<div class="dropdown">
    <button data-toggle="dropdown" class="btn btn-secondary">Menu</button>
    <div class="dropdown-menu">
        <a class="dropdown-item" href="#">Edit</a>
        <a class="dropdown-item" href="#">Archive</a>
        <div class="dropdown-divider"></div>
        <span class="dropdown-title">Danger</span>
        <a class="dropdown-item" href="#">Delete</a>
    </div>
</div>
```

**Direction variants** (class on wrapper):
```html
<div class="dropdown dropup">...</div>
<div class="dropdown dropright">...</div>
<div class="dropdown dropleft">...</div>
```

**Data attributes:**
- `data-toggle="dropdown"` — required
- `data-placement="bottom"` — `top` | `bottom` | `left` | `right`
- `data-position="fixed"` — `fixed` | `absolute`
- `data-close-on-item-click="false"` — keep open after clicking item
- `data-scroll="true"` — scrollable content
- `data-scroll-content-height="200"` — scroll area height in px

**JS API:**
```js
const dd = Dropdown.getInstance(triggerEl);
dd.show();
dd.close();
dd.toggle();
dd.update();    // Reposition
dd.destroy();
```

**DEFAULTS:**
```js
{
    animation: true,
    animationIn: 'zoomIn',
    animationOut: 'zoomOut',
    animationSpeed: 'fastest',
    placement: 'bottom',
    align: 'start',
    position: 'absolute',
    offset: null,
    minWidth: null,
    closeOnItemClick: true,
    positionObserver: true,
    scroll: true,
    scrollContentHeight: 100,
    content: false,
    onInit: null,
    onUpdate: null,
    onDestroy: null,
    onShow: null,
    onClose: null,
    onItemClick: null
}
```

---

## Toast

Position-based notification system with queuing, auto-dismiss, and stacking.

**Show a toast** (singleton pattern):
```js
const toast = window.Cool.getToast();
// or: Toast.getInstance(document.body)

toast.init({
    content: 'Changes saved',
    placement: 'bottom-right',
    dismiss: 3
});

// With title, icon, and action button
toast.init({
    title: 'Success',
    content: 'Item created',
    icon: '<i class="fas fa-check"></i>',
    action: {
        content: 'Undo',
        callback: () => console.log('undone')
    },
    dismiss: 5
});

// Custom template
toast.init({
    template: (t) => `<div class="custom">${t.content}</div>`,
    content: 'Hello'
});
```

**Placements:** `top-left`, `top-center`, `top-right`, `bottom-left`, `bottom-center`, `bottom-right`

**DEFAULTS:**
```js
{
    title: '',
    content: '',
    icon: '',
    action: null,
    template: '',
    placement: 'top-right',
    animation: true,
    animationIn: null,
    animationOut: null,
    animationSpeed: 'faster',
    offsetX: null,
    offsetY: null,
    dismiss: 3,           // seconds; 0 = manual dismiss only
    header: false,
    maxVisible: 3,
    onInitialize: null,
    onDestroy: null,
    onInit: null,
    onShow: null,
    onClose: null,
    onClick: null
}
```

---

## Dialog

Modal windows with header, body, footer, and callback-driven actions.

**Show a dialog** (singleton pattern):
```js
const dialog = window.Cool.Dialog.getInstance(document.body);

// Confirmation dialog
dialog.init({
    title: 'Delete item?',
    body: '<p>This action cannot be undone.</p>',
    size: 'sm',
    centered: true,
    callbacks: {
        confirm: function (done) {
            deleteItem();
            done();  // closes the dialog
        },
        close: function (done) { done(); }
    },
    actions: {
        confirm: { content: 'Delete' },
        close: { content: 'Cancel' }
    }
});

// Custom template
dialog.init({
    template: () => `<div class="dialog-content">
        <div class="dialog-header"><h5>Form</h5></div>
        <div class="dialog-body"><form>...</form></div>
        <div class="dialog-footer">
            <button class="btn" data-action="close">Cancel</button>
            <button class="btn btn-primary" data-action="submit">Save</button>
        </div>
    </div>`
});
```

**Sizes:** `''` (default) | `'sm'` (300px) | `'md'` (512px) | `'lg'` (960px) | `'xl'` (1200px) | `'full'`

**DEFAULTS:**
```js
{
    title: '',
    template: '',
    body: '',
    footer: true,
    header: true,
    size: '',
    classes: '',
    centered: true,
    backdrop: true,
    preventScroll: true,
    closeBtn: true,
    animation: true,
    animationIn: 'zoomIn',
    animationOut: 'fadeOut',
    animationSpeed: 'fastest',
    maxBodyHeight: null,
    overflowVisible: false,
    callbacks: {},
    actions: {},
    onInitialize: null,
    onDestroy: null,
    onInit: null,
    onShow: null,
    onClose: null
}
```

---

## Select

Custom select component with checkbox/radio modes, search, and chips.

**HTML:**
```html
<div class="select-container">
    <div class="select"
         data-toggle="select"
         data-name="status"
         data-type="checkbox"
         data-items='[{"id":"a","label":"Active"},{"id":"d","label":"Draft"}]'
         data-show-search
         data-dynamic-title="true">
        <div class="select-header">
            <span class="select-title">Select items…</span>
            <span class="select-icon">▾</span>
        </div>
    </div>
</div>
```

**Data attributes:**
- `data-toggle="select"` — required
- `data-name="fieldname"` — form field name
- `data-type="checkbox|radio"` — multi or single select
- `data-items='[...]'` — JSON array of items `{id, label}`
- `data-show-search` — enable search input
- `data-show-footer` — show footer with action buttons
- `data-dynamic-title="true"` — update title with selection count
- `data-use-chips="true"` — display selected items as chips
- `data-set-data='{...}'` — pre-selected values
- `data-items-to-exclude="id1,id2"` — hide specific items
- `data-prevent-uncheck="true"` — prevent deselecting

**JS API:**
```js
const s = Select.getInstance(selectEl);
s.show();
s.close();
s.reset();
s.update();
s.getData();                    // → { fieldname: ['id1', 'id2'] }
s.setData({ fieldname: ['id1'] });
s.destroy();
```

**DEFAULTS:**
```js
{
    name: '',
    type: 'checkbox',
    items: [],
    setData: null,
    showSearch: false,
    showFooter: false,
    searchApi: false,
    dynamicTitle: false,
    dynamicTitleDefault: '',
    dynamicTitleEmptyDefault: '',
    useChips: false,
    scrollContentHeight: 300,
    useNativeScroll: false,
    contentWidth: null,
    allowNoneOnRadioSelect: true,
    preventUncheck: false,
    showValidStateIcon: true,
    itemsToExclude: [],
    primaryKeyword: 'id',
    position: 'absolute',
    scrollEl: '.content',
    onInit: null,
    onUpdate: null,
    onDestroy: null,
    onShow: null,
    onClose: null,
    onReset: null,
    onSearch: null,
    onSelect: null
}
```

---

## Collapse

Accordion-style show/hide for content regions.

**HTML:**
```html
<button data-toggle="collapse" data-target="#details" class="btn">Toggle</button>
<div id="details" class="collapse">
    Content here
</div>
```

**Data attributes:**
- `data-toggle="collapse"` — required
- `data-target="#id"` — target element to toggle
- `data-breakpoint="1024"` — only collapse below this viewport width

**JS API:**
```js
const c = Collapse.getInstance(triggerEl);
c.show();
c.close();
c.toggle();
c.update();
c.destroy();
```

---

## Tooltip

Small positioned text hints.

**HTML:**
```html
<button data-toggle="tooltip" data-content="Help text" class="btn">Info</button>
```

**Data attributes:**
- `data-toggle="tooltip"` — required
- `data-content="text"` — tooltip text (or function)
- `data-placement="bottom"` — `top` | `bottom` | `left` | `right`

**JS API:**
```js
const t = Tooltip.getInstance(el);
t.show();
t.close();
t.update();
t.destroy();
```

**DEFAULTS:**
```js
{
    container: 'body',
    animation: false,
    animationIn: 'fadeIn',
    animationOut: 'fadeOut',
    animationSpeed: 'fastest',
    placement: 'bottom',
    content: '',
    trigger: 'auto',
    onInit: null,
    onUpdate: null,
    onDestroy: null,
    onShow: null,
    onClose: null
}
```

---

## Popover

Positioned popups with title and content.

**HTML:**
```html
<button data-toggle="popover"
        data-title="Popover Title"
        data-content="Popover body content"
        data-trigger="focus"
        class="btn">Trigger</button>
```

**Data attributes:**
- `data-toggle="popover"` — required
- `data-title="..."` — heading
- `data-content="..."` — body (string or function)
- `data-trigger="focus|hover|click"`
- `data-placement="bottom"` — `top` | `bottom` | `left` | `right`
- `data-align="center"` — `start` | `center` | `end`

**JS API:**
```js
const p = Popover.getInstance(el);
p.show();
p.close();
p.update();
p.destroy();
```

**DEFAULTS:**
```js
{
    container: 'body',
    containerAutoDetect: false,
    trigger: 'focus',
    placement: 'bottom',
    align: 'center',
    offset: null,
    collisionDetection: true,
    title: '',
    content: '',
    onInit: null,
    onUpdate: null,
    onDestroy: null,
    onShow: null,
    onClose: null
}
```

---

## Theme Switcher

Toggle between light, dark, and auto (system) themes.

**HTML:**
```html
<button data-toggle="theme-switcher" data-theme="dark" class="btn">🌙</button>
```

**JS API:**
```js
const ts = ThemeSwitcher.getInstance(el);
ts.setTheme('light');       // 'light' | 'dark' | 'auto'
ts.getTheme();              // → 'light' | 'dark' | 'auto'
ts.getResolvedTheme();      // → 'light' | 'dark' (resolved from 'auto')
```

Persists to `localStorage`. Sets `data-theme` attribute on the root element.

---

## Section Tabs

Tab navigation within page sections.

**HTML:**
```html
<div data-toggle="sectionTabs">
    <div class="section-tabs">
        <a class="section-tab active" data-target="#tab1">Tab 1</a>
        <a class="section-tab" data-target="#tab2">Tab 2</a>
    </div>
    <div id="tab1" class="section-tab-content active">Content 1</div>
    <div id="tab2" class="section-tab-content">Content 2</div>
</div>
```
