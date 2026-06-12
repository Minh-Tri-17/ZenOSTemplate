# Tiêu chuẩn Viết Code HTML & CSS — ZenOS Enterprise Dashboard

Tài liệu này định nghĩa các tiêu chuẩn bắt buộc khi AI generate hoặc chỉnh sửa code HTML/CSS cho project ZenOS. Mọi đoạn code mới phải tuân thủ đầy đủ.

---

## 1. Thứ tự Class trong HTML

### Quy tắc: **Custom class trước — Bootstrap/thư viện sau**

Khi một phần tử dùng cả class custom lẫn class của Bootstrap, luôn đặt **class custom ở đầu**, tiếp theo mới đến class Bootstrap.

**✅ Đúng:**

```html
<button class="ghost-button btn btn-sm">...</button>
<div class="glass-card h-100 d-flex flex-column">...</div>
<span class="status-badge status-optimal">...</span>
<button class="chart-menu-btn ghost-button btn btn-sm p-0 d-flex align-items-center justify-content-center"></button>
```

**❌ Sai:**

```html
<button class="btn btn-sm ghost-button">...</button>
<div class="d-flex flex-column glass-card h-100">...</div>
```

### Thứ tự ưu tiên trong class string:

1. Custom class chính (định nghĩa layout/component của element)
2. Custom class modifier (biến thể, trạng thái)
3. Bootstrap layout utilities (`d-flex`, `h-100`, `w-100`, `overflow-hidden`...)
4. Bootstrap responsive utilities (`d-none d-sm-block`, `col-lg-8`...)
5. Bootstrap display/position utilities (`position-relative`, `z-1`...)

---

## 2. Không dùng Inline CSS khi đã có Custom Class

### Quy tắc: Nếu element đã có **custom class** (định nghĩa trong `styles.css`), **không được viết** `style="..."` trực tiếp trên element đó.

Lý do: Custom class đã bao gồm đầy đủ styling. Inline CSS tạo ra xung đột và khó bảo trì.

**✅ Đúng** — Element có custom class, KHÔNG dùng inline style:

```html
<h2 class="text-display-lg text-primary-color">Tiêu đề</h2>
<button class="ghost-button btn btn-sm">Action</button>
<div class="glass-card banner-card">...</div>
<p class="text-on-surface-variant">Mô tả</p>
```

**❌ Sai** — Element có custom class nhưng vẫn thêm inline style:

```html
<!-- Sai: text-display-lg đã có font-size, không cần thêm -->
<h2 class="text-display-lg text-primary-color" style="font-weight: 700; font-size: 28px">Tiêu đề</h2>

<!-- Sai: ghost-button đã có padding và height, không cần thêm -->
<button class="ghost-button btn btn-sm" style="padding: 0 24px; width: auto;">Action</button>

<!-- Sai: text-on-surface-variant đã có color/opacity -->
<p class="text-on-surface-variant" style="font-size: 14px; opacity: 0.85">Mô tả</p>
```

**✅ Ngoại lệ được phép** — Inline style dùng cho **dữ liệu động** (giá trị JS không thể đặt vào class):

```html
<!-- OK: width là giá trị động từ data -->
<div class="load-fill load-fill-success" style="width: 42%"></div>
```

---

## 3. Không dùng Bootstrap Spacing Utilities khi đã có Custom Class

### Quy tắc: Nếu element đã có custom class **riêng** (không phải utility class), **không thêm** các class khoảng cách Bootstrap như `mb-*`, `mt-*`, `ms-*`, `me-*`, `p-*`, `px-*`, `py-*`, `gap-*`, `g-*`...

Spacing phải được định nghĩa bên trong custom class ở `styles.css`.

**✅ Đúng:**

```html
<!-- stat-card đã có padding/margin trong CSS -->
<div class="glass-card stat-card">...</div>

<!-- action-bar đã có gap và margin -->
<div class="action-bar">...</div>

<!-- menu-item-pill đã có padding và margin-bottom -->
<div class="menu-item-pill color-green">...</div>

<!-- notification-item đã có padding -->
<a class="notification-item unread" href="#">...</a>
```

**❌ Sai:**

```html
<!-- Sai: stat-card đã có styles riêng, không dùng thêm p-3 hay mb-2 -->
<div class="glass-card stat-card p-3 mb-2">...</div>

<!-- Sai: ghost-button đã có height và padding -->
<button class="ghost-button btn btn-sm px-3 py-2">...</button>

<!-- Sai: chart-card-title đã có margin -->
<h5 class="chart-card-title text-title-md text-primary-color mb-3">...</h5>
```

### Trường hợp được phép dùng Bootstrap spacing cùng custom class:

Chỉ dùng khi **custom class là utility class** (dạng text color, display, visibility) chứ không phải component class:

```html
<!-- OK: text-primary-color là utility text color, m-0 hợp lệ -->
<h2 class="text-display-lg text-primary-color m-0">Operations Overview</h2>

<!-- OK: text-on-surface-variant là utility color, mb-1 hợp lệ -->
<p class="text-on-surface-variant text-body-sm mb-1 summary-card-label">...</p>
```

**Phân biệt:**

- **Component class**: `glass-card`, `ghost-button`, `stat-card`, `action-bar`, `nav-link-item`, `notification-item`, `modal-header`... → **Không thêm spacing Bootstrap**
- **Utility class**: `text-primary-color`, `text-on-surface-variant`, `text-secondary-color`... → **Có thể thêm spacing Bootstrap nếu thực sự cần**

---

## 4. Element không có Custom Class → Tự do dùng Bootstrap hoặc Inline Style

Nếu element **chỉ dùng class Bootstrap** hoặc **không có class nào**, có thể tự do dùng Bootstrap utilities hoặc inline CSS.

**✅ Đúng:**

```html
<!-- Wrapper div thuần Bootstrap -->
<div class="d-flex justify-content-between align-items-center mb-3">...</div>
<div class="row g-4 mb-4">...</div>
<div class="col-lg-8">...</div>

<!-- Inline style cho layout wrapper không có custom class -->
<div style="height: 38px; display: flex; align-items: center;">...</div>

<!-- Bootstrap spacing trên element thuần Bootstrap -->
<div class="d-flex gap-2 mb-3">...</div>
```

---

## 5. Tiêu chuẩn CSS trong `styles.css`

### 5.1. Luôn dùng CSS Custom Properties (Variables)

**Không bao giờ** hardcode màu sắc, spacing, hay shadow cố định. Luôn dùng CSS variables đã định nghĩa trong `:root` và `[data-theme="dark"]`.

**✅ Đúng:**

```css
.my-component {
  color: var(--color-primary);
  background-color: var(--glass-card-bg);
  border: 1px solid var(--input-border-color);
  box-shadow: var(--glass-card-shadow);
}
```

**❌ Sai:**

```css
.my-component {
  color: #1a1a2e;
  background-color: rgba(255, 255, 255, 0.45);
  border: 1px solid rgba(0, 0, 0, 0.08);
}
```

### 5.2. Dùng `!important` chỉ để override Bootstrap

Dùng `!important` **chỉ khi** cần override CSS của Bootstrap (`.btn`, `.form-control`, `.form-select`...) hoặc khi cần đảm bảo property không bị ghi đè.

```css
/* OK: Override Bootstrap .btn-sm border-radius */
.ghost-button {
  border-radius: 8px !important;
  height: 38px !important;
  padding: 0 16px !important;
}

/* OK: Đảm bảo ghost-input không bị override bởi .form-control */
.ghost-input {
  background-color: transparent !important;
  box-shadow: none !important;
}
```

### 5.3. Hỗ trợ Dark Mode qua `[data-theme="dark"]`

Mọi component mới phải có dark mode variant. Dùng selector `[data-theme="dark"] .my-component`.

```css
.my-component {
  background-color: var(--glass-card-bg);
  border-color: var(--glass-card-border);
  color: var(--color-primary);
}

/* Nếu cần override màu sắc cụ thể cho dark mode */
[data-theme="dark"] .my-component {
  /* Chỉ viết khi CSS variables chưa đủ */
}
```

### 5.4. Cấu trúc CSS theo sections

Khi thêm CSS mới, thêm vào đúng section trong file `styles.css` (đã có comment phân chia rõ):

```
1. BASE, VARIABLES & THEMES
2. GLOBAL COMPONENTS & INPUTS
3. MOBILE HEADER
4. TOP BAR (DESKTOP)
5. SIDEBAR
... (theo thứ tự layout từ trên xuống dưới)
```

---

## 6. Các Component Custom Tái Sử Dụng

Luôn dùng các component class đã có thay vì tự tạo mới:

| Mục đích             | Class cần dùng                                                                            |
| -------------------- | ----------------------------------------------------------------------------------------- |
| Nút tối giản (ghost) | `ghost-button btn btn-sm`                                                                 |
| Nút nguy hiểm (đỏ)   | `ghost-button btn-error btn btn-sm`                                                       |
| Nút primary đen      | `btn-primary btn` hoặc `button` (animated)                                                |
| Input / Select       | `ghost-input form-control` hoặc `ghost-input ghost-select form-select`                    |
| Card kính mờ         | `glass-card`                                                                              |
| Card kính + modal    | `glass-card glass-modal-content modal-content`                                            |
| Badge trạng thái     | `status-badge status-optimal/warning/critical/offline`                                    |
| Thanh load           | `load-bar` + `load-fill load-fill-success/warning/error`                                  |
| Tiêu đề màu          | `text-primary-color`, `text-secondary-color`, `text-on-surface-variant`                   |
| Font size preset     | `text-display-lg`, `text-headline-lg`, `text-title-md`, `text-body-sm`, `text-label-caps` |

---

## 7. Tiêu chuẩn HTML Accessibility

- Luôn thêm `aria-label` cho button không có text rõ ràng (icon-only buttons).
- Luôn thêm `alt` cho thẻ `<img>`.
- Dùng semantic HTML: `<header>`, `<nav>`, `<main>`, `<section>`, `<button>`, `<table>`, `<form>`.
- Mọi `<input>` phải có `id` và `<label for="...">` tương ứng hoặc `aria-label`.

```html
<!-- ✅ Đúng -->
<button class="ghost-button btn btn-sm" aria-label="Close sidebar">
  <i class="fa-solid fa-xmark"></i>
</button>

<input type="text" class="ghost-input form-control" id="nodeIdInput" placeholder="e.g. Delta-08" />
<label for="nodeIdInput" class="text-on-surface-variant form-label small fw-semibold">Node ID</label>
```

---

## 8. Tự động Phát hiện và Sửa Vi phạm (Auto-fix on Detect)

### Quy tắc: Nếu phát hiện code hiện tại **đang vi phạm** bất kỳ tiêu chuẩn nào ở trên, **hãy fix ngay** — không chỉ viết code mới đúng mà bỏ qua code cũ sai.

AI không được phép chỉ generate code mới theo đúng tiêu chuẩn trong khi để nguyên code vi phạm xung quanh. Phải sửa triệt để.

---

### 8.1. Vi phạm: Spacing Bootstrap trên Component Class → Chuyển vào CSS

Nếu phát hiện element có **component class** nhưng đang thêm Bootstrap spacing utilities, phải:

1. Đọc giá trị CSS tương ứng của các Bootstrap class đó (bảng quy đổi bên dưới)
2. Thêm các property đó vào CSS của custom class trong `styles.css`
3. Xóa Bootstrap spacing utilities khỏi HTML

**Ví dụ phát hiện vi phạm:**

```html
<!-- Vi phạm: ghost-button đã là component class, không được thêm px-3 py-2 -->
<button class="ghost-button btn btn-sm px-3 py-2">...</button>
```

**Hành động fix:**

- Bước 1: Tra cứu `px-3` = `padding-left: 1rem; padding-right: 1rem;` và `py-2` = `padding-top: 0.5rem; padding-bottom: 0.5rem;`
- Bước 2: Thêm vào `.ghost-button` trong `styles.css`:

```css
.ghost-button {
  /* ... các property hiện có ... */
  padding: 0.5rem 1rem !important; /* thêm từ px-3 py-2 */
}
```

- Bước 3: Xóa `px-3 py-2` khỏi HTML:

```html
<button class="ghost-button btn btn-sm">...</button>
```

---

### 8.2. Vi phạm: Inline Style trên Component Class → Chuyển vào CSS

Nếu phát hiện element có component class nhưng có thêm `style="..."`, phải:

1. Đọc các property trong `style="..."`
2. Thêm vào CSS của custom class trong `styles.css` (với `!important` nếu cần)
3. Xóa `style="..."` khỏi HTML

**Ví dụ phát hiện vi phạm:**

```html
<!-- Vi phạm: text-display-lg đã là custom class -->
<h2 class="text-display-lg text-primary-color" style="font-weight: 700; font-size: 28px">...</h2>
```

**Hành động fix:**

- Bước 1: Kiểm tra `.text-display-lg` trong `styles.css` đã có `font-size` và `font-weight` chưa
- Bước 2: Nếu chưa có → thêm vào `.text-display-lg`; nếu giá trị khác → tạo modifier class hoặc cập nhật giá trị
- Bước 3: Xóa `style="..."` khỏi HTML

---

### 8.3. Bảng quy đổi Bootstrap Spacing → CSS Value

> Bootstrap dùng `$spacer = 1rem (16px)` làm đơn vị cơ sở.

| Bootstrap Class | CSS Property                     | Giá trị           |
| --------------- | -------------------------------- | ----------------- |
| `m-0`           | `margin`                         | `0`               |
| `m-1`           | `margin`                         | `0.25rem (4px)`   |
| `m-2`           | `margin`                         | `0.5rem (8px)`    |
| `m-3`           | `margin`                         | `1rem (16px)`     |
| `m-4`           | `margin`                         | `1.5rem (24px)`   |
| `m-5`           | `margin`                         | `3rem (48px)`     |
| `mt-*`          | `margin-top`                     | (cùng scale trên) |
| `mb-*`          | `margin-bottom`                  | (cùng scale trên) |
| `ms-*`          | `margin-left`                    | (cùng scale trên) |
| `me-*`          | `margin-right`                   | (cùng scale trên) |
| `mx-*`          | `margin-left` + `margin-right`   | (cùng scale)      |
| `my-*`          | `margin-top` + `margin-bottom`   | (cùng scale)      |
| `p-0`           | `padding`                        | `0`               |
| `p-1`           | `padding`                        | `0.25rem (4px)`   |
| `p-2`           | `padding`                        | `0.5rem (8px)`    |
| `p-3`           | `padding`                        | `1rem (16px)`     |
| `p-4`           | `padding`                        | `1.5rem (24px)`   |
| `p-5`           | `padding`                        | `3rem (48px)`     |
| `pt-*`          | `padding-top`                    | (cùng scale trên) |
| `pb-*`          | `padding-bottom`                 | (cùng scale trên) |
| `ps-*`          | `padding-left`                   | (cùng scale trên) |
| `pe-*`          | `padding-right`                  | (cùng scale trên) |
| `px-*`          | `padding-left` + `padding-right` | (cùng scale)      |
| `py-*`          | `padding-top` + `padding-bottom` | (cùng scale)      |
| `gap-1`         | `gap`                            | `0.25rem (4px)`   |
| `gap-2`         | `gap`                            | `0.5rem (8px)`    |
| `gap-3`         | `gap`                            | `1rem (16px)`     |
| `gap-4`         | `gap`                            | `1.5rem (24px)`   |
| `g-1` (grid)    | `gap`                            | `0.25rem`         |
| `g-2` (grid)    | `gap`                            | `0.5rem`          |
| `g-3` (grid)    | `gap`                            | `1rem`            |
| `g-4` (grid)    | `gap`                            | `1.5rem`          |

---

### 8.4. Ưu tiên sửa vi phạm

Khi làm việc trong 1 file HTML/CSS, thứ tự ưu tiên:

1. **Sửa trước** bất kỳ vi phạm nào trong các element đang được chỉnh sửa hoặc xung quanh
2. **Báo cáo** các vi phạm tìm thấy ở nơi khác trong file (nếu không được yêu cầu sửa hết)
3. **Không tạo vi phạm mới** khi generate code

---

## 9. Tóm tắt nhanh (Cheat Sheet)

```
Element có custom class riêng (component class)?
  ├─ CÓ → Custom class TRƯỚC, Bootstrap SAU
  │        → KHÔNG dùng inline style=""
  │        → KHÔNG dùng spacing Bootstrap (mb-*, p-*, gap-*, ...)
  │        → NẾU đang vi phạm → fix ngay: chuyển property vào styles.css
  └─ KHÔNG → Tự do dùng Bootstrap utilities hoặc inline style

Cần màu/shadow/spacing trong CSS?
  → Luôn dùng var(--tên-variable)
  → Không hardcode giá trị màu

Cần override Bootstrap?
  → Dùng !important có kiểm soát

Thêm component mới?
  → Thêm dark mode variant [data-theme="dark"] .class
  → Đặt vào đúng section trong styles.css

Phát hiện vi phạm trong code cũ?
  → Bootstrap spacing trên component class → quy đổi theo bảng Section 9.3 → thêm vào CSS → xóa khỏi HTML
  → Inline style trên component class → chuyển vào CSS class → xóa style attribute
```
