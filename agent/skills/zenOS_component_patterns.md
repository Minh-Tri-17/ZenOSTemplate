# ZenOS Component Architecture & Patterns

## Overview

ZenOS uses vanilla HTML/CSS with Bootstrap 5.3.3 for JS behaviors only (Collapse, Dropdown). All visual styling is custom.

---

## 1. Glass Card (`.glass-card`)

```html
<div class="glass-card"><!-- Content --></div>
```

Uses: `var(--glass-card-bg)`, `backdrop-filter: blur(15px)`, `border-radius: 20px`.
Variants: `.stat-card`, `.filter-card`, table container.

---

## 2. Ghost Input (`.ghost-input`)

```html
<input class="form-control form-control-sm ghost-input" placeholder="..." type="text">
```

- Transparent bg, focus: subtle fill + glow
- `.ghost-select`: custom appearance reset, paired with `<i>` chevron
- Inside `.filter-card`: fixed `height: 38px`

---

## 3. Ghost Button (`.ghost-button`)

```html
<button class="btn btn-sm ghost-button"><i class="fa-solid fa-plus"></i>Create</button>
<button class="btn btn-sm ghost-button btn-error"><i class="fa-solid fa-trash"></i>Delete</button>
```

- Transparent bg + border, hover: lift `translateY(-1px)`
- Error variant: `--color-error` text/border
- Resets `--bs-btn-active-*` to prevent Bootstrap flash

---

## 4. Stat Card

```html
<div class="stat-cards-grid">
  <div class="glass-card stat-card">
    <div class="stat-card-header">
      <i class="fa-solid fa-diagram-project icon-secondary"></i>
      <span class="stat-badge stat-badge-secondary">+2.4%</span>
    </div>
    <div>
      <p class="stat-label">Total Nodes</p>
      <h3 class="stat-value">124</h3>
    </div>
  </div>
</div>
```

- Grid: 4 cols desktop, 2 tablet, 1 mobile
- Badge: `.stat-badge-secondary` (blue) or `.stat-badge-neutral` (muted)
- Icon: `.icon-secondary` or `.icon-primary`

---

## 5. Data Table

```html
<div class="glass-card d-flex flex-column overflow-hidden mb-4">
  <div class="table-responsive">
    <table class="data-table">
      <thead><tr><th>Column</th></tr></thead>
      <tbody><tr><td>Data</td></tr></tbody>
    </table>
  </div>
  <div class="table-footer"><!-- Pagination --></div>
</div>
```

### Status Badges
- `.status-optimal` (blue), `.status-critical` (red), `.status-offline` (gray)
- Each has `.status-dot` (6px circle indicator)

### Load Bar
```html
<div class="load-bar-wrapper">
  <span class="load-percent">42%</span>
  <div class="load-bar">
    <div class="load-fill load-fill-secondary" style="width: 42%"></div>
  </div>
</div>
```

### Column Classes
- `.td-node-id` — bold primary, `.td-region` / `.td-sync` — muted variant

---

## 6. Pagination

```html
<div class="table-footer">
  <div class="page-info">
    <span>Items per page:</span>
    <div class="page-select-wrapper">
      <select class="form-select form-select-sm ghost-input page-select">
        <option>10</option><option>25</option><option>50</option>
      </select>
      <i class="fa-solid fa-chevron-down"></i>
    </div>
    <span class="ms-2">1-5 of 124</span>
  </div>
  <div class="pagination-controls">
    <button class="btn btn-sm ghost-button page-btn" disabled>
      <i class="fa-solid fa-chevron-left"></i>
    </button>
    <div class="d-flex align-items-center gap-1">
      <button class="btn btn-sm page-btn page-btn-active">1</button>
      <button class="btn btn-sm ghost-button page-btn text-on-surface-variant">2</button>
      <span class="page-ellipsis">...</span>
    </div>
    <button class="btn btn-sm ghost-button page-btn page-btn-nav">
      <i class="fa-solid fa-chevron-right"></i>
    </button>
  </div>
</div>
```

---

## 7. Dropdown Pattern (Notification, Language, Avatar)

All share the same architecture:

```html
<div class="[component]-wrapper dropdown">
  <button class="topbar-btn" id="[id]" data-bs-toggle="dropdown" aria-expanded="false">
    <i class="fa-solid fa-[icon]"></i>
  </button>
  <div class="[component]-dropdown dropdown-menu dropdown-menu-end" id="[id]">
    <!-- Content -->
  </div>
</div>
```

### Critical CSS (prevents Popper.js interference)
```css
.dropdown-menu {
  inset: auto !important;
  transform: none !important;
  top: calc(100% + 12px) !important;
  right: 0 !important;
  left: auto !important;
}
```

- Background: `--lang-dropdown-bg` (opaque, NOT transparent glass)
- Animation: `langDropdownFadeIn 0.25s ease`
- `z-index: 1050`

---

## 8. Multi-Select Component

Custom-built (not Bootstrap):

```html
<div class="multi-select" data-multi-select>
  <button class="btn btn-sm ghost-input multi-select-toggle" type="button">
    <span class="multi-select-tags" data-multi-select-tags>
      <span class="multi-select-placeholder">Select...</span>
    </span>
    <i class="fa-solid fa-chevron-down"></i>
  </button>
  <div class="multi-select-panel" role="listbox">
    <label class="multi-select-option">
      <input class="form-check-input" type="checkbox" value="val" data-label="Label">
      <span>Label</span>
    </label>
  </div>
</div>
```

- Toggle: `.multi-select.open` controls panel visibility
- Selected → `.multi-select-tag` pills with `×` remove
- Panel: `--multi-select-panel-bg` (opaque)
- JS: IIFE, queries `[data-multi-select]`

---

## 9. Action Bar + Filter Card

```html
<div class="action-bar">
  <div class="action-group"><!-- Primary: Create, Update, Delete --></div>
  <div class="action-group">
    <!-- Secondary: Import, Export -->
    <div class="action-divider d-none d-sm-block"></div>
    <!-- Filter toggle (Bootstrap Collapse) -->
  </div>
</div>

<div class="collapse" id="filterCollapse">
  <div class="glass-card filter-card">
    <div class="filter-group">
      <label>Label</label>
      <!-- Input/Select/Multi-select -->
    </div>
  </div>
</div>
```

- Filter card: flex-wrap, `gap: 24px`, `align-items: flex-end`, `z-index: 40`
- Filter group: `flex: 1; min-width: 200px`
