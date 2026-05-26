# ZenOS CSS Theming & Design Token System

## Overview

ZenOS uses a **dual-theme CSS Custom Properties** architecture. The default theme is **Dark Mode** (`:root`), and **Light Mode** is activated via `[data-theme="light"]` on the `<html>` element.

> **CRITICAL RULE**: Every CSS variable MUST be declared in BOTH `:root` (dark) AND `[data-theme="light"]` (light). Missing a declaration in either theme will cause visual breakage.

---

## Theme Structure

```css
/* Dark Mode (default) */
:root {
  --color-primary: #ffffff;
  --color-background: #131313;
  /* ... all variables ... */
}

/* Light Mode */
[data-theme="light"] {
  --color-primary: #1a1a2e;
  --color-background: #f5f5f7;
  /* ... must mirror ALL variables from :root ... */
}
```

### Theme Toggle Mechanism

- HTML: `<html lang="en" data-theme="light">` → Light Mode
- HTML: `<html lang="en">` (no attribute) → Dark Mode
- JavaScript toggles via `root.setAttribute('data-theme', 'light')` / `root.removeAttribute('data-theme')`
- Persisted in `localStorage` key: `zenOS-theme`

---

## Variable Naming Conventions

### Color Tokens (`--color-*`)

Follow Material Design 3 naming:

| Variable | Dark Value | Light Value | Usage |
|----------|-----------|-------------|-------|
| `--color-primary` | `#ffffff` | `#1a1a2e` | Primary text, headings |
| `--color-on-primary` | `#2d3133` | `#ffffff` | Text on primary backgrounds |
| `--color-secondary` | `#a4c9ff` | `#0267b8` | Accent color, links |
| `--color-on-secondary` | `#00315d` | `#ffffff` | Text on secondary |
| `--color-error` | `#ffb4ab` | `#ba1a1a` | Error states |
| `--color-on-surface` | `#e5e2e1` | `#1a1a2e` | General body text |
| `--color-on-surface-variant` | `#c4c7c9` | `#555770` | Muted/secondary text |
| `--color-background` | `#131313` | `#f5f5f7` | Page background |
| `--color-surface` | `#131313` | `#f5f5f7` | Surface/card base |
| `--color-surface-container-low` | `#1c1b1b` | `#eeeef2` | Input backgrounds |
| `--color-surface-container` | `#20201f` | `#e8e8ec` | Container backgrounds |
| `--color-surface-container-high` | `#2a2a2a` | `#e0e0e6` | Elevated containers |
| `--color-outline` | `#8e9193` | `#767689` | Borders, dividers |
| `--color-outline-variant` | `#444749` | `#c5c5d0` | Subtle borders |

### Glass/Overlay Tokens (`--glass-*`, `--hover-*`, `--divider-*`)

Used for the glassmorphism design language:

| Variable | Dark | Light | Purpose |
|----------|------|-------|---------|
| `--glass-bg` | `rgba(255,255,255,0.05)` | `rgba(255,255,255,0.7)` | General glass background |
| `--glass-border` | `rgba(255,255,255,0.2)` | `rgba(0,0,0,0.08)` | Glass borders |
| `--glass-shadow` | `rgba(0,0,0,0.4)` | `rgba(0,0,0,0.08)` | Glass shadows |
| `--hover-bg` | `rgba(255,255,255,0.15)` | `rgba(0,0,0,0.15)` | Hover state backgrounds |
| `--divider-color` | `rgba(255,255,255,0.1)` | `rgba(0,0,0,0.08)` | Divider lines |

### Component-Specific Tokens

Each major component has its own token group:

- **Body**: `--body-bg`, `--body-gradient`
- **Glass Card**: `--glass-card-bg`, `--glass-card-border`, `--glass-card-shadow`
- **Ghost Input**: `--input-border-color`, `--input-focus-bg`, `--input-focus-shadow`, `--input-focus-border`
- **Ghost Button**: `--btn-border-color`, `--btn-hover-bg`, `--btn-error-border`, `--btn-error-hover-bg`
- **Sidebar**: `--sidebar-bg`, `--sidebar-border`, `--sidebar-shadow`, `--nav-hover-bg`, `--nav-active-bg`, `--nav-active-arrow`, `--submenu-border`, `--submenu-header-color`
- **Topbar**: `--topbar-bg`, `--topbar-border`, `--avatar-border`
- **Theme Icon**: `--theme-icon-color`, `--theme-icon-animation`, `--theme-icon-filter`
- **Badges**: `--badge-secondary-bg`, `--badge-secondary-border`, `--badge-neutral-bg`, `--badge-neutral-border`
- **Status**: `--status-optimal-bg/border`, `--status-critical-bg/border`, `--status-offline-bg/border`
- **Load Bar**: `--load-bar-bg`, `--load-bar-empty-bg`, `--muted-color`
- **Table**: `--table-border`, `--table-row-border`, `--table-row-hover-bg`, `--table-footer-bg`, `--page-btn-active-bg/border`
- **Dropdown**: `--lang-dropdown-bg`, `--multi-select-panel-bg`, `--lang-active-bg/color/border`, `--dropdown-item-hover-bg`
- **Scrollbar**: `--scrollbar-thumb`, `--scrollbar-thumb-hover`
- **Misc**: `--action-divider-bg`, `--notification-shadow`

---

## Rules for Adding New Variables

### Step-by-Step

1. **Name the variable** following the pattern: `--[component]-[property]`
   - Example: `--modal-overlay-bg`, `--tooltip-border`

2. **Declare in `:root`** (dark mode) with appropriate dark values
   - Use `rgba(255, 255, 255, ...)` for white-based overlays
   - Use low opacity (0.05–0.2) for subtle effects

3. **Declare in `[data-theme="light"]`** with appropriate light values
   - Use `rgba(0, 0, 0, ...)` for dark-based overlays
   - Adjust opacity to match visual weight of dark mode

4. **Apply in component CSS** using `var(--your-variable)`

### Dark ↔ Light Pattern

```css
/* Dark: white-based transparency */
:root {
  --new-component-bg: rgba(255, 255, 255, 0.05);
  --new-component-border: rgba(255, 255, 255, 0.2);
}

/* Light: black-based transparency OR solid light colors */
[data-theme="light"] {
  --new-component-bg: rgba(0, 0, 0, 0.04);
  --new-component-border: rgba(0, 0, 0, 0.08);
}
```

---

## Glassmorphism Recipe

The standard glass effect in ZenOS combines:

```css
.glass-element {
  background-color: var(--glass-card-bg);      /* Semi-transparent */
  backdrop-filter: blur(15px);                  /* Blur behind */
  -webkit-backdrop-filter: blur(15px);          /* Safari support */
  border: 0.5px solid var(--glass-card-border); /* Subtle border */
  box-shadow: var(--glass-card-shadow);         /* Depth shadow */
  border-radius: 20px;                          /* Rounded corners */
}
```

### Body Background Gradient

Both themes use multi-layered `radial-gradient` + `linear-gradient` for depth:

- Dark: Blue-tinted radial gradients with low opacity
- Light: Warm-toned radial gradients with soft white overlays
- Always use `background-attachment: fixed` for parallax effect

---

## Anti-Patterns (DO NOT DO)

1. ❌ **Hardcoding colors**: `color: #ffffff` → ✅ `color: var(--color-primary)`
2. ❌ **Declaring variable in only one theme** → ✅ Always declare in both `:root` AND `[data-theme="light"]`
3. ❌ **Using Bootstrap color utilities**: `text-primary`, `bg-dark` → ✅ Use custom utility classes: `.text-primary-color`, `.text-on-surface-variant`
4. ❌ **High opacity for glass overlays**: `rgba(255,255,255,0.8)` → ✅ Keep glass bg under `0.55` for true glassmorphism
5. ❌ **Using `!important` on colors unless overriding Bootstrap** → ✅ Only use `!important` in utility classes or Bootstrap overrides

---

## Exceptions: Hardcoded Values Allowed

Some values ARE intentionally hardcoded:

- `[data-theme="light"] .ghost-select option` → `background-color: #ffffff` (native `<option>` doesn't support CSS variables well)
- `[data-theme="light"] .page-select option` → Same reason
- Dropdown opaque backgrounds (`--lang-dropdown-bg`) use solid colors to prevent content showing through
