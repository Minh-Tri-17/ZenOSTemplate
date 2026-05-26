# ZenOS Bootstrap 5.3 Integration Rules

## Overview

ZenOS uses **Bootstrap 5.3.3** selectively — primarily for **JavaScript components** (Collapse, Dropdown) and minimal layout utilities. All visual styling is custom CSS. This skill documents the integration boundaries and override patterns.

---

## What Bootstrap Is Used For

### JS Components (via `bootstrap.bundle.min.js`)

| Component | Usage |
|-----------|-------|
| **Collapse** | Sidebar accordion (`#sidebarAccordion`), Filter card (`#filterCollapse`), Settings submenu |
| **Dropdown** | Notification, Language, Avatar dropdowns |

### Layout Utilities (CSS only)

| Utility | Usage |
|---------|-------|
| `d-flex`, `d-none`, `d-md-flex` | Layout + responsive visibility |
| `flex-grow-1`, `flex-shrink-0` | Flex item sizing |
| `align-items-center` | Vertical centering |
| `gap-1` | Small gaps in pagination |
| `mb-3`, `mb-4`, `ms-2` | Minor spacing |
| `text-end` | Right-aligned table columns |
| `overflow-hidden` | Table card container |
| `flex-column`, `flex-wrap` | Direction control |
| `form-control`, `form-select`, `form-check-input` | Base form reset (then overridden) |
| `btn`, `btn-sm` | Base button reset (then overridden) |

---

## What Bootstrap Is NOT Used For

> ❌ **DO NOT use these Bootstrap features:**

- Color utilities: `text-primary`, `bg-dark`, `text-muted`, `bg-light`
- Card component: `.card`, `.card-body` → Use `.glass-card` instead
- Navbar component → Custom `.sidebar` + `.topbar`
- Table styling: `.table`, `.table-striped` → Custom `.data-table`
- Badge component: `.badge` → Custom `.stat-badge`, `.status-badge`
- Alert component → Not used
- Modal component → Not implemented yet
- Pagination component → Custom `.pagination-controls`

---

## Override Patterns

### Ghost Input (overrides `.form-control` / `.form-select`)

Bootstrap provides base styling, then `.ghost-input` overrides:

```css
.ghost-input {
  background-color: transparent;           /* Override Bootstrap white bg */
  border: 0.5px solid var(--input-border-color);
  color: var(--color-primary);
  font-size: 14px;
  border-radius: 8px;
  box-shadow: none;                        /* Kill Bootstrap focus shadow */
}

.ghost-input:focus,
.ghost-input:focus-visible {
  background-color: var(--input-focus-bg);
  outline: none;
  box-shadow: var(--input-focus-shadow);   /* Custom shadow, not Bootstrap's */
  border-color: var(--input-focus-border);
  color: var(--color-primary);
}
```

### Ghost Button (overrides `.btn`)

```css
.ghost-button {
  background-color: transparent;
  border: 1px solid var(--btn-border-color);
  color: var(--color-primary);
  /* Reset Bootstrap active state */
  --bs-btn-active-bg: transparent;
  --bs-btn-active-border-color: var(--btn-border-color);
  --bs-btn-active-color: var(--color-primary);
}
```

> **CRITICAL**: The `--bs-btn-active-*` resets prevent Bootstrap's default blue flash on button click.

### Ghost Select (overrides `.form-select`)

```css
.ghost-select {
  -webkit-appearance: none;
  -moz-appearance: none;
  appearance: none;
  background-image: none;              /* Remove Bootstrap's built-in chevron */
  cursor: pointer;
}
```

The custom chevron is added via an adjacent `<i>` element in a wrapper div.

---

## Dropdown Positioning Override

Bootstrap uses Popper.js for dropdown positioning. ZenOS overrides this completely:

```css
.dropdown-menu {
  inset: auto !important;              /* Kill Popper's positioning */
  transform: none !important;          /* Kill Popper's transform */
  top: calc(100% + 12px) !important;   /* Custom offset from trigger */
  right: 0 !important;                 /* Align to right edge */
  left: auto !important;               /* Don't align to left */
}
```

> **WHY**: Popper.js calculates positions dynamically and can cause jumpy/inconsistent placement. ZenOS uses fixed positioning relative to the wrapper for predictable results.

### Dropdown Item Override

```css
.lang-dropdown.dropdown-menu .dropdown-item {
  background-color: transparent;
  padding: 0;
}

.lang-dropdown.dropdown-menu .dropdown-item:hover,
.lang-dropdown.dropdown-menu .dropdown-item:focus,
.lang-dropdown.dropdown-menu .dropdown-item:active {
  background-color: transparent;       /* Remove Bootstrap's blue highlight */
}
```

Custom hover effects are applied to inner elements (`.lang-item`, `.avatar-dropdown-item`) instead.

---

## Bootstrap Collapse Integration

### Accordion Pattern

```html
<!-- Parent container -->
<ul id="sidebarAccordion">
  <li>
    <a data-bs-toggle="collapse" 
       data-bs-target="#submenuSales" 
       aria-expanded="false">
    </a>
    <div class="collapse" 
         id="submenuSales" 
         data-bs-parent="#sidebarAccordion">
      <!-- Only one panel open at a time -->
    </div>
  </li>
</ul>
```

### Independent Collapse (no accordion)

```html
<a data-bs-toggle="collapse" 
   data-bs-target="#submenuSettings" 
   aria-expanded="false">
</a>
<div class="collapse" id="submenuSettings">
  <!-- No data-bs-parent → toggles independently -->
</div>
```

### Filter Collapse

```html
<button data-bs-toggle="collapse" 
        data-bs-target="#filterCollapse" 
        aria-expanded="false" 
        aria-controls="filterCollapse">
</button>
<div class="collapse" id="filterCollapse">
  <!-- Filter card content -->
</div>
```

---

## Bootstrap Dropdown Integration

### Setup Pattern

```html
<div class="dropdown">
  <button data-bs-toggle="dropdown" aria-expanded="false">
    Trigger
  </button>
  <div class="dropdown-menu dropdown-menu-end">
    Content
  </div>
</div>
```

### JS Instance Access

```javascript
const instance = bootstrap.Dropdown.getOrCreateInstance(triggerElement);
instance.hide(); // Programmatically close
```

---

## CDN References

```html
<!-- CSS (in <head>) -->
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">

<!-- JS Bundle (before </body>) -->
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>
```

Also includes:
- Font Awesome 7.0.1 (icons)
- Google Fonts Inter (typography)

---

## Rules for New Development

1. **Never add Bootstrap visual classes** for colors, cards, tables, badges, or navs
2. **Always override Bootstrap defaults** when using `.form-control`, `.form-select`, `.btn`
3. **Reset `--bs-btn-active-*`** on any custom button to prevent flash
4. **Use `!important` on dropdown positioning** to override Popper.js
5. **Use Bootstrap JS API** (`getOrCreateInstance`) for programmatic control
6. **Keep Bootstrap CSS** only for its reset/normalize and grid basics
