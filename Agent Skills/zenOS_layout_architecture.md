# ZenOS Layout & Responsive Architecture

## Overview

ZenOS uses a fixed sidebar + scrollable main content layout. The topbar is fixed within the main content area. Responsive design uses 3 breakpoints with sidebar/topbar hidden on mobile.

---

## Layout Structure

```
<body>
  <div class="d-flex">
    ├── <nav class="sidebar d-none d-md-flex">  (fixed, 256px)
    └── <div class="main-content">              (flex: 1, margin-left: 256px)
         ├── <header class="topbar d-none d-md-flex">  (fixed, 80px height)
         └── <main class="content-area">               (scrollable, padded)
              ├── Header Section (stat cards)
              ├── Action Bar
              ├── Filter Card (collapsible)
              └── Table Card (with pagination)
  </div>
</body>
```

---

## Key Dimensions

| Element | Property | Value |
|---------|----------|-------|
| Sidebar | Width | `256px` |
| Sidebar | Position | `fixed`, full height |
| Sidebar | z-index | `20` |
| Topbar | Height | `80px` |
| Topbar | Position | `fixed`, `left: 256px` |
| Topbar | Width | `calc(100% - 256px)` |
| Topbar | z-index | `100` |
| Topbar | Left margin | `margin-left: 25px` (gap from sidebar) |
| Content Area | Top padding | `112px` (clears fixed topbar) |
| Content Area | Side padding | `40px` (desktop) / `16px` (mobile) |
| Content Area | Max width | `1440px` |

---

## CSS Layout Tokens

```css
:root {
  --gutter: 24px;           /* Grid gap between elements */
  --margin-desktop: 40px;   /* Content area side padding */
  --margin-mobile: 16px;    /* Mobile content padding */
  --container-max: 1440px;  /* Max content width */
}
```

---

## Sidebar Layout

```css
.sidebar {
  width: 256px;
  position: fixed;
  left: 0; top: 0;
  height: 100%;
  border-radius: 0 20px 20px 0;  /* Rounded right side */
  padding: 30px 22px;
  z-index: 20;
  display: flex;
  flex-direction: column;
}
```

Internal layout:
- `.sidebar-brand` — flex-shrink: 0 (never shrinks)
- `.sidebar-nav` — flex: 1, overflow-y: auto (scrollable)
- `.sidebar-footer` — margin-top: auto (pinned to bottom)

---

## Topbar Layout

```css
.topbar {
  position: fixed;
  top: 0;
  left: 256px;
  width: calc(100% - 256px);
  height: 80px;
  z-index: 100;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 24px;
  margin-left: 25px;
  border-bottom-left-radius: 20px;
}
```

Internal layout:
- Left section: Search bar (`width: 33%`)
- Right section: `.topbar-actions` (flex, gap: 24px)

---

## Main Content Area

```css
.main-content {
  margin-left: 256px;
  flex: 1;
  display: flex;
  flex-direction: column;
  min-height: calc(100vh + 100px);
}

.content-area {
  flex: 1;
  padding: var(--margin-desktop);
  padding-top: 112px;         /* Clears fixed topbar */
  display: flex;
  flex-direction: column;
  gap: var(--gutter);
  max-width: var(--container-max);
  margin: 0 auto;
  width: 100%;
}
```

---

## Z-Index Hierarchy

| Layer | z-index | Element |
|-------|---------|---------|
| Base content | auto | Content area, cards |
| Filter card | 40 | `.filter-card` |
| Sidebar | 20 | `.sidebar` |
| Topbar | 100 | `.topbar` |
| Dropdown menus | 1050 | All dropdown panels |
| Multi-select panel | 100 | `.multi-select-panel` |

---

## Responsive Breakpoints

### Mobile (<768px)

```css
@media (max-width: 767.98px) {
  .sidebar { display: none; }
  .topbar { display: none; }
  .main-content { margin-left: 0; }
  .content-area {
    padding: var(--margin-mobile);
    padding-top: 16px;
  }
  .stat-cards-grid { grid-template-columns: 1fr; }
  .action-divider { display: none; }
}
```

### Small Tablet (576px – 767px)

```css
@media (min-width: 576px) and (max-width: 767.98px) {
  .stat-cards-grid { grid-template-columns: repeat(2, 1fr); }
}
```

### Tablet (768px – 991px)

```css
@media (min-width: 768px) and (max-width: 991.98px) {
  .stat-cards-grid { grid-template-columns: repeat(2, 1fr); }
}
```

### Desktop (≥992px)
- Full sidebar + topbar visible
- Stat cards: 4 columns
- All features active

---

## Body Background

```css
body {
  background-color: var(--body-bg);
  background-image: var(--body-gradient);
  background-attachment: fixed;  /* Parallax effect */
  min-height: calc(100vh + 100px);
}
```

The gradient is a multi-layered radial gradient that differs by theme:
- **Dark**: Blue-tinted orbs on dark background
- **Light**: Warm-toned orbs with white overlays

---

## Adding New Pages

When adding a new page/section to the content area:

1. Place content inside `<main class="content-area">`
2. Use `.glass-card` for container cards
3. Maintain `gap: var(--gutter)` between major sections
4. Content auto-centers at `max-width: 1440px`
5. Do NOT add fixed positioning elements without considering the z-index hierarchy
6. Test at all 3 breakpoints (mobile, tablet, desktop)
