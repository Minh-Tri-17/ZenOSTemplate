# HTML Class Ordering Rules

## Overview

In ZenOS, we follow a strict class ordering convention in HTML templates. **All custom CSS classes must always be declared BEFORE Bootstrap classes** in the `class` attribute of any HTML element.

This ensures readability, consistency, and alignment with the ZenOS design system (which prioritizes custom styling over default Bootstrap aesthetics).

---

## The Ordering Hierarchy

When writing or editing HTML class attributes, classes must be ordered from left to right as follows:

1. **Custom Classes**: Unique classes defined in `styles.css` (e.g., `.mobile-header`, `.glass-card`, `.ghost-button`, `.topbar-btn`).
2. **Bootstrap Components**: Standard Bootstrap components (e.g., `.btn`, `.dropdown`, `.dropdown-menu`, `.form-control`).
3. **Bootstrap Utilities**: Layout, spacing, alignment, and responsiveness classes (e.g., `.d-flex`, `.align-items-center`, `.gap-3`, `.py-2`, `.px-3`, `.d-none`, `.d-md-flex`).

```html
<!-- Format -->
<element class="[Custom Classes] [Bootstrap Components] [Bootstrap Utilities]">
```

---

## Examples

### 1. Dropdown Wrapper
*   **Incorrect (Bootstrap first)**:
    ```html
    <div class="dropdown notification-dropdown-wrapper">
    ```
*   **Correct (Custom first)**:
    ```html
    <div class="notification-dropdown-wrapper dropdown">
    ```

### 2. Action Button with Utilities
*   **Incorrect (Bootstrap first / mixed)**:
    ```html
    <button class="btn btn-sm d-flex align-items-center ghost-button gap-2">
    ```
*   **Correct (Custom -> Component -> Utilities)**:
    ```html
    <button class="ghost-button btn btn-sm d-flex align-items-center gap-2">
    ```

### 3. Theme Toggle Button
*   **Incorrect**:
    ```html
    <button class="dropdown-item mobile-btn-sun d-flex align-items-center gap-3 py-2 px-3">
    ```
*   **Correct**:
    ```html
    <button class="mobile-btn-sun dropdown-item d-flex align-items-center gap-3 py-2 px-3">
    ```

---

## Anti-Patterns to Avoid

*   ❌ **Mixing custom classes inside utility class lists**: Keeping custom classes scattered makes them extremely difficult to locate.
*   ❌ **Putting Bootstrap utilities first**: Never start a class list with `d-flex`, `p-3`, etc., if there is a custom class.
*   ❌ **Hardcoding layout classes after utilities**: Always keep custom classes grouped at the very front.

---

## Quy tắc bằng tiếng Việt (Vietnamese Summary)

*   **Quy tắc bắt buộc**: Các class CSS tự định nghĩa (custom classes) của dự án **phải luôn đứng trước** các class của Bootstrap.
*   **Thứ tự ưu tiên**: `[Class Custom] -> [Class Component Bootstrap] -> [Class Utility Bootstrap (d-flex, py-2, gap-3,...)]`.
