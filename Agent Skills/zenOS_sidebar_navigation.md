# ZenOS Sidebar & Navigation System

## Overview

The sidebar is a fixed 256px navigation panel using Bootstrap 5 Collapse for accordion behavior. It contains 10 navigation groups with an independent footer section.

---

## Structure

```
sidebar (fixed, 256px)
├── sidebar-brand (logo + title)
├── sidebar-nav #sidebarAccordion (scrollable, accordion parent)
│   ├── Dashboard (single link, no submenu)
│   ├── Sales (accordion group)
│   ├── Products (accordion group)
│   ├── Customers (accordion group)
│   ├── HRM (accordion group)
│   ├── Operations (accordion group)
│   ├── Communication (accordion group)
│   ├── Access Control (accordion group)
│   └── Categories (accordion group with section headers)
└── sidebar-footer (separate from accordion)
    └── Settings (independent collapse)
```

---

## HTML Patterns

### Single Link (Dashboard)

```html
<li>
  <a class="nav-link-item nav-link-active" href="#">
    <i class="fa-solid fa-gauge"></i>
    <span>Dashboard</span>
  </a>
</li>
```

### Accordion Group

```html
<li>
  <a class="nav-link-item" href="#" 
     data-bs-toggle="collapse" 
     data-bs-target="#submenuSales" 
     aria-expanded="false">
    <i class="fa-solid fa-cart-shopping"></i>
    <span class="flex-grow-1">Sales</span>
    <i class="fa-solid fa-chevron-down nav-arrow"></i>
  </a>
  <div class="collapse" id="submenuSales" data-bs-parent="#sidebarAccordion">
    <ul class="sidebar-submenu">
      <li><a class="submenu-link" href="#"><span>Order</span></a></li>
      <li><a class="submenu-link" href="#"><span>Invoice</span></a></li>
    </ul>
  </div>
</li>
```

### Section Headers (Categories group only)

```html
<ul class="sidebar-submenu">
  <li class="submenu-section-header">Geography</li>
  <li><a class="submenu-link" href="#"><span>Country</span></a></li>
  <li><a class="submenu-link" href="#"><span>Province</span></a></li>
  
  <li class="submenu-section-header">HRM</li>
  <li><a class="submenu-link" href="#"><span>Department</span></a></li>
</ul>
```

### Footer (Settings — outside accordion)

```html
<div class="sidebar-footer">
  <a class="nav-link-item" href="#" 
     data-bs-toggle="collapse" 
     data-bs-target="#submenuSettings" 
     aria-expanded="false">
    <i class="fa-solid fa-gear"></i>
    <span class="flex-grow-1">Settings</span>
    <i class="fa-solid fa-chevron-down nav-arrow"></i>
  </a>
  <div class="collapse" id="submenuSettings">
    <!-- NO data-bs-parent → independent from accordion -->
    <ul class="sidebar-submenu">
      <li><a class="submenu-link" href="#"><span>System Setting</span></a></li>
    </ul>
  </div>
</div>
```

> **IMPORTANT**: Settings does NOT have `data-bs-parent="#sidebarAccordion"` — it collapses independently so it can stay open while any main menu group is also open.

---

## Key Classes

| Class | Element | Purpose |
|-------|---------|---------|
| `.sidebar` | `<nav>` | Fixed container, 256px wide |
| `.sidebar-brand` | `<div>` | Logo + app name header |
| `.sidebar-nav` | `<ul>` | Scrollable nav list, accordion parent |
| `.nav-link-item` | `<a>` | Top-level menu item |
| `.nav-link-active` | `<a>` | Currently active page indicator |
| `.nav-arrow` | `<i>` | Chevron icon, rotates on expand |
| `.sidebar-submenu` | `<ul>` | Nested submenu list |
| `.submenu-link` | `<a>` | Submenu item link |
| `.submenu-section-header` | `<li>` | Category group header (10px uppercase) |
| `.sidebar-footer` | `<div>` | Pinned footer section |

---

## CSS Behaviors

### Arrow Rotation
```css
.nav-link-item[aria-expanded="true"] .nav-arrow {
  transform: rotate(0deg);     /* Pointing down = open */
}
.nav-link-item[aria-expanded="false"] .nav-arrow {
  transform: rotate(-90deg);   /* Pointing right = closed */
}
```

### Submenu Link Hover
```css
.submenu-link:hover span {
  transform: translateX(4px);  /* Subtle slide-right effect */
}
```

### Active State
```css
.nav-link-active {
  color: var(--color-primary) !important;
  font-weight: 600;
  border-left: 2px solid var(--color-primary);
  background-color: var(--nav-active-bg);
}
```

### Scrollbar
- Sidebar nav uses `overflow-y: auto` with custom thin scrollbar
- `scrollbar-width: thin` (Firefox)
- Custom webkit scrollbar: 4px wide, transparent track

---

## Adding New Menu Items

### Add a new accordion group:

1. Add `<li>` inside `<ul class="sidebar-nav" id="sidebarAccordion">`
2. Use unique `id` for collapse target (e.g., `#submenuNewModule`)
3. Include `data-bs-parent="#sidebarAccordion"` for accordion behavior
4. Choose appropriate Font Awesome icon

### Add a submenu item:

1. Add `<li><a class="submenu-link" href="#"><span>Item Name</span></a></li>` inside the group's `<ul class="sidebar-submenu">`

### Add section headers (for large groups):

1. Add `<li class="submenu-section-header">Section Name</li>` before the group of related items

---

## Current Menu Groups & Icons

| Group | Icon | Submenu Count |
|-------|------|--------------|
| Dashboard | `fa-gauge` | 0 (single link) |
| Sales | `fa-cart-shopping` | 6 items |
| Products | `fa-utensils` | 8 items |
| Customers | `fa-users` | 3 items |
| HRM | `fa-id-badge` | 8 items |
| Operations | `fa-store` | 3 items |
| Communication | `fa-envelope` | 4 items |
| Access Control | `fa-shield-halved` | 4 items |
| Categories | `fa-layer-group` | 10 items (3 sections) |
| Settings *(footer)* | `fa-gear` | 4 items |

---

## Responsive Behavior

- **Desktop (≥768px)**: Sidebar visible, fixed position
- **Mobile (<768px)**: `display: none` — sidebar is completely hidden
- No hamburger menu or mobile drawer is currently implemented
