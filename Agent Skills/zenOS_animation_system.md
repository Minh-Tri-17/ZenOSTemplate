# ZenOS Animation & Micro-Interaction System

## Overview

ZenOS uses CSS keyframe animations and transitions for a premium, alive-feeling UI. Animations are theme-aware — some change behavior between dark and light modes via CSS variables.

---

## Keyframe Animations

### 1. Bell Ring (`bellRing`)

**Target**: `.topbar-btn-bell i`
**Duration**: 3s, infinite loop
**Purpose**: Draws attention to notifications

```css
@keyframes bellRing {
  0%   { transform: rotate(0deg); }
  5%   { transform: rotate(14deg); }
  10%  { transform: rotate(-12deg); }
  15%  { transform: rotate(10deg); }
  20%  { transform: rotate(-8deg); }
  25%  { transform: rotate(6deg); }
  30%  { transform: rotate(-4deg); }
  35%  { transform: rotate(2deg); }
  40%  { transform: rotate(0deg); }
  100% { transform: rotate(0deg); }
}
```

- `transform-origin: top center` — swings from the bell's hook
- Active 0%–40% of cycle, rests 40%–100% for natural feel

---

### 2. Sun Glow (`sunGlow`) — Dark Mode Theme Icon

**Target**: `.topbar-btn-sun i` (when in dark mode, icon shows ☀️)
**Duration**: 4s, infinite loop

```css
@keyframes sunGlow {
  0%   { filter: drop-shadow(0 0 2px rgba(255,200,50,0.3)); transform: rotate(0deg); }
  25%  { filter: drop-shadow(0 0 8px rgba(255,200,50,0.6)); }
  50%  { filter: drop-shadow(0 0 12px rgba(255,200,50,0.8)); transform: rotate(180deg); }
  75%  { filter: drop-shadow(0 0 8px rgba(255,200,50,0.6)); }
  100% { filter: drop-shadow(0 0 2px rgba(255,200,50,0.3)); transform: rotate(360deg); }
}
```

- Combines rotation + pulsing glow
- Color: warm yellow `#f0c040`

---

### 3. Moon Swing (`moonSwing`) — Light Mode Theme Icon

**Target**: `.topbar-btn-sun i` (when in light mode, icon shows 🌙)
**Duration**: 4s, infinite loop

```css
@keyframes moonSwing {
  0%   { transform: rotate(0deg) scale(1); filter: drop-shadow(...0.4); }
  10%  { transform: rotate(12deg) scale(1.05); filter: drop-shadow(...0.7); }
  20%  { transform: rotate(-10deg) scale(1); filter: drop-shadow(...0.8); }
  30%  { transform: rotate(8deg) scale(1.03); filter: drop-shadow(...0.9); }
  40%  { transform: rotate(-5deg) scale(1); filter: drop-shadow(...0.7); }
  50%  { transform: rotate(0deg) scale(1); filter: drop-shadow(...0.5); }
  100% { transform: rotate(0deg) scale(1); filter: drop-shadow(...0.4); }
}
```

- Pendulum swing with subtle scale breathing
- Color: cool blue `#7b8fc7`
- Glow: `rgba(160, 180, 255, ...)`

---

### 4. Theme Icon Switching (via CSS Variables)

The same `.topbar-btn-sun i` element uses CSS variables to swap animations:

```css
:root {
  --theme-icon-color: #f0c040;                          /* Sun yellow */
  --theme-icon-animation: sunGlow 4s linear infinite;
  --theme-icon-filter: none;
}

[data-theme="light"] {
  --theme-icon-color: #7b8fc7;                          /* Moon blue */
  --theme-icon-animation: moonSwing 4s ease-in-out infinite;
  --theme-icon-filter: drop-shadow(0 0 4px rgba(160, 180, 255, 0.5));
}
```

---

### 5. Language Wave (`languageWave`)

**Target**: `.topbar-btn i.fa-language`
**Duration**: 2s, infinite loop

```css
@keyframes languageWave {
  0%, 100% { transform: rotateY(0deg); }
  25%      { transform: rotateY(45deg); }
  50%      { transform: rotateY(0deg); }
  75%      { transform: rotateY(-45deg); }
}
```

- 3D Y-axis rotation (requires `transform-style: preserve-3d`)
- Creates a "card flip" wave effect

---

### 6. Dropdown Fade In (`langDropdownFadeIn`)

**Target**: All dropdown menus (notification, language, avatar)
**Duration**: 0.25s, runs once on open

```css
@keyframes langDropdownFadeIn {
  from { opacity: 0; transform: translateY(-8px) scale(0.96); }
  to   { opacity: 1; transform: translateY(0) scale(1); }
}
```

---

## Transition Patterns

### Standard Durations
- **Fast** (hover states): `0.2s ease-in-out`
- **Medium** (navigation): `0.3s ease-in-out`
- **Smooth** (premium interactions): `0.25s cubic-bezier(0.4, 0, 0.2, 1)`

### Common Transitions

| Element | Property | Duration | Effect |
|---------|----------|----------|--------|
| `.nav-link-item` | `all` | `0.3s ease-in-out` | Bg color + text color |
| `.nav-link-item:active` | `transform` | instant | `scale(0.95)` press effect |
| `.submenu-link span` | `transform` | `0.3s` | `translateX(4px)` on hover |
| `.ghost-button:hover` | `transform` | `0.2s` | `translateY(-1px)` lift |
| `.topbar-btn` | `color, transform` | `0.25s ease` | `scale(1.05)` on hover |
| `.lang-item` | `all` | `0.25s cubic-bezier` | `translateX(4px)` slide |
| `.lang-item img` | `transform, box-shadow` | `0.25s ease` | `scale(1.1)` on hover |
| `.notification-item` | `bg, transform, shadow` | `0.2s ease` | `translateY(-1px)` lift |
| `.avatar-dropdown-item` | `transform, bg, color, shadow` | `0.2s ease` | `translateX(4px)` slide |
| `.data-table tbody tr` | `background-color` | `0.2s` | Row highlight |

### Gradient Overlay Transitions (`.lang-item::before`, `.avatar-dropdown-item::before`)

```css
.element::before {
  content: "";
  position: absolute;
  inset: 0;
  background: linear-gradient(135deg, var(--hover-bg), transparent);
  opacity: 0;
  transition: opacity 0.25s ease;
  pointer-events: none;
}
.element:hover::before {
  opacity: 1;
}
```

---

## Adding New Animations

### Guidelines

1. **Duration**: Match existing patterns (2–4s for continuous, 0.2–0.3s for interactions)
2. **Easing**: Use `ease-in-out` for most, `cubic-bezier(0.4, 0, 0.2, 1)` for premium feel
3. **Rest periods**: For continuous animations, include 40–60% rest time
4. **Theme awareness**: If animation uses colors, consider using CSS variables
5. **Performance**: Prefer `transform` and `opacity` over layout-triggering properties
6. **`will-change`**: Only add for complex/janky animations, not by default
