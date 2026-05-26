# ZenOS Testing & Verification Skill

## Overview

Sau mỗi lần thay đổi code (HTML, CSS, JS), agent **BẮT BUỘC** phải thực hiện quy trình kiểm thử dưới đây để đảm bảo kết quả đúng mong muốn và không tạo ra bug mới (regression).

> **NGUYÊN TẮC VÀNG**: Không bao giờ báo cáo "hoàn thành" mà chưa chạy kiểm thử. Mọi thay đổi — dù nhỏ đến đâu — đều có thể phá vỡ theme khác, breakpoint khác, hoặc component khác.

---

## Quy Trình Kiểm Thử (Testing Workflow)

### Bước 1: Khởi động Server

```bash
# Từ thư mục project
npx -y http-server . -p 8080 -o
```

Hoặc dùng Live Server extension nếu có.

### Bước 2: Kiểm thử theo Checklist

Thực hiện tuần tự theo 5 nhóm kiểm thử bên dưới.

### Bước 3: Báo cáo kết quả

Sau khi kiểm thử xong, báo cáo dạng:

```
✅ Theme: Dark/Light đều OK
✅ Responsive: 3 breakpoints đều OK  
✅ Components: Tất cả tương tác hoạt động
⚠️ Regression: [mô tả issue nếu có]
```

---

## Nhóm 1: Kiểm Thử Theme (Dark/Light Mode)

### Bắt buộc kiểm tra SAU MỌI thay đổi CSS

| # | Kiểm tra | Cách thực hiện |
|---|----------|---------------|
| 1.1 | **Dark Mode hiển thị đúng** | Mở page ở Dark Mode, kiểm tra phần tử đã sửa |
| 1.2 | **Light Mode hiển thị đúng** | Click nút Moon/Sun chuyển sang Light Mode, kiểm tra lại |
| 1.3 | **Contrast đủ đọc được** | Text trên background phải dễ đọc ở CẢ 2 theme |
| 1.4 | **Biến CSS khai báo đủ cặp** | Nếu thêm biến mới → phải có ở cả `:root` VÀ `[data-theme="light"]` |
| 1.5 | **Không hardcode màu** | Tìm kiếm `#` hoặc `rgb` trong CSS mới thêm, chỉ dùng `var()` |

### Cách kiểm tra nhanh biến CSS thiếu

```bash
# Liệt kê tất cả biến trong :root
grep -oP '(?<=--)[a-z-]+(?=:)' styles.css | sort > /tmp/dark_vars.txt

# Liệt kê tất cả biến trong [data-theme="light"]  
# So sánh 2 file để tìm biến bị thiếu
```

### Checklist Theme cụ thể cho từng component

- [ ] **Sidebar**: background, text color, hover bg, active state, submenu border, section headers
- [ ] **Topbar**: background, search input, icon colors, avatar border
- [ ] **Glass Cards**: background opacity, border, shadow
- [ ] **Buttons**: border, text color, hover state, error variant
- [ ] **Inputs**: border, focus glow, placeholder color
- [ ] **Table**: header, row borders, row hover, footer bg
- [ ] **Dropdowns**: background (opaque), item hover, active item
- [ ] **Status Badges**: optimal (blue), critical (red), offline (gray)
- [ ] **Scrollbar**: thumb color trong sidebar

---

## Nhóm 2: Kiểm Thử Responsive

### 3 Breakpoints bắt buộc kiểm tra

| Breakpoint | Viewport | Kỳ vọng |
|-----------|----------|---------|
| **Mobile** | < 768px | Sidebar ẩn, Topbar ẩn, Stat cards 1 cột, padding 16px |
| **Tablet** | 768px – 991px | Sidebar hiện, Topbar hiện, Stat cards 2 cột |
| **Desktop** | ≥ 992px | Full layout, Stat cards 4 cột |

### Cách kiểm tra

1. **Browser DevTools** → Toggle device toolbar (Ctrl+Shift+M)
2. Kiểm tra tại các width: **375px** (mobile), **768px** (tablet), **1440px** (desktop)
3. Hoặc resize browser window thủ công

### Checklist Responsive

- [ ] Sidebar hiện/ẩn đúng tại breakpoint 768px
- [ ] Topbar hiện/ẩn đúng tại breakpoint 768px
- [ ] `margin-left` của `.main-content` reset về 0 trên mobile
- [ ] Stat cards chuyển grid đúng (4 → 2 → 1 cột)
- [ ] Action divider ẩn trên mobile
- [ ] Filter card flex-wrap hoạt động (không bị tràn)
- [ ] Table scroll ngang hoạt động (`.table-responsive`)
- [ ] Pagination không bị cắt hoặc tràn
- [ ] Dropdown menus không bị tràn ra ngoài viewport

---

## Nhóm 3: Kiểm Thử Component Tương Tác

### Sau mỗi thay đổi HTML hoặc JS

| # | Component | Hành động kiểm tra | Kỳ vọng |
|---|-----------|-------------------|---------|
| 3.1 | **Theme Toggle** | Click nút Moon/Sun | Icon đổi, theme chuyển, persist sau reload |
| 3.2 | **Sidebar Accordion** | Click từng nhóm menu | Chỉ 1 nhóm mở, nhóm cũ đóng (trừ Settings) |
| 3.3 | **Settings Submenu** | Click Settings | Mở/đóng độc lập, không ảnh hưởng accordion chính |
| 3.4 | **Arrow Rotation** | Expand/collapse menu | Chevron quay 0° (mở) ↔ -90° (đóng) |
| 3.5 | **Notification Dropdown** | Click bell icon | Dropdown mở, animation fade-in |
| 3.6 | **Mark Notification Read** | Click từng notification | Class `unread` bị remove, badge count giảm |
| 3.7 | **Mark All Read** | Click "Mark all as read" | Tất cả `unread` bị remove, badge ẩn |
| 3.8 | **Language Dropdown** | Click language icon | Dropdown mở, chọn ngôn ngữ |
| 3.9 | **Language Selection** | Click 1 ngôn ngữ | Active state chuyển, dropdown đóng |
| 3.10 | **Avatar Dropdown** | Click avatar | Dropdown mở, items clickable |
| 3.11 | **Filter Toggle** | Click nút Filter | Filter card collapse/expand |
| 3.12 | **Multi-Select** | Click toggle → chọn options | Tags hiển thị, remove button hoạt động |
| 3.13 | **Multi-Select Close** | Click outside | Panel đóng |

### Kiểm tra Animation

- [ ] Bell ringing animation chạy liên tục
- [ ] Sun glow / Moon swing chạy đúng theo theme
- [ ] Language icon wave animation hoạt động
- [ ] Dropdown fade-in animation mượt
- [ ] Button hover lift effect (`translateY(-1px)`)
- [ ] Sidebar item hover background transition
- [ ] Submenu link text slide-right on hover

---

## Nhóm 4: Kiểm Thử Regression (Không Tạo Bug Mới)

### Quy tắc: "Sửa 1 chỗ, kiểm tra 5 chỗ liên quan"

#### Ma trận ảnh hưởng (Impact Matrix)

| Nếu sửa... | Phải kiểm tra thêm... |
|-------------|----------------------|
| CSS Variables (`:root`) | ✅ Tất cả components dùng biến đó ở CẢ 2 themes |
| Sidebar CSS | ✅ Sidebar scrollbar, accordion, footer, hover states |
| Topbar CSS | ✅ Search input, all dropdowns, avatar, responsive ẩn/hiện |
| `.ghost-input` | ✅ Search bar, filter inputs, filter selects, multi-select toggle, page-select |
| `.ghost-button` | ✅ Action bar buttons, filter Clear button, pagination buttons |
| `.glass-card` | ✅ Stat cards, filter card, table card |
| Table CSS | ✅ All table rows, status badges, load bars, pagination |
| Dropdown CSS | ✅ Notification, Language, Avatar dropdowns (cả 3 dùng chung pattern) |
| JavaScript | ✅ Theme persistence (localStorage), dropdown interactions, multi-select |
| Bootstrap version | ✅ ALL Collapse + Dropdown behaviors |

### Regression Checklist Nhanh

Sau MỌI thay đổi, chạy qua danh sách này:

```
[ ] Page load không có lỗi console (F12 → Console)
[ ] Dark mode: tổng thể nhìn OK
[ ] Light mode: tổng thể nhìn OK
[ ] Sidebar mở/đóng accordion bình thường
[ ] Ít nhất 1 dropdown (notification/language/avatar) mở được
[ ] Filter card expand/collapse hoạt động
[ ] Multi-select chọn/bỏ chọn hoạt động
[ ] Table hiển thị đúng, hover row hoạt động
[ ] Theme toggle hoạt động + persist sau reload
```

---

## Nhóm 5: Kiểm Thử Chất Lượng Code

### CSS Quality Check

| # | Kiểm tra | Tool/Cách |
|---|----------|-----------|
| 5.1 | **Không có CSS dư thừa** | Tìm selector không dùng đến |
| 5.2 | **Không trùng lặp property** | Kiểm tra cùng property khai báo 2 lần trong 1 rule |
| 5.3 | **`!important` hợp lý** | Chỉ dùng khi override Bootstrap hoặc trong utility class |
| 5.4 | **Biến CSS naming đúng convention** | `--[component]-[property]` |
| 5.5 | **Comment mô tả section** | Mỗi section lớn có `/* ===== Section Name ===== */` |

### HTML Quality Check

| # | Kiểm tra | Cách |
|---|----------|------|
| 5.6 | **Semantic HTML** | Đúng thẻ: `nav`, `header`, `main`, `section` |
| 5.7 | **Accessibility** | `aria-expanded`, `aria-label`, `role` cho interactive elements |
| 5.8 | **ID unique** | Không trùng `id` trong cùng page |
| 5.9 | **Alt text cho images** | Tất cả `<img>` phải có `alt` |

### JavaScript Quality Check

| # | Kiểm tra | Cách |
|---|----------|------|
| 5.10 | **Không có console errors** | F12 → Console tab |
| 5.11 | **IIFE pattern** | Mỗi script block wrap trong `(function() { ... })()` |
| 5.12 | **Null checks** | Kiểm tra `if (!element) return` trước khi dùng |
| 5.13 | **Event listener không leak** | Không add listener trong loop mà không cleanup |

---

## Template Báo Cáo Kiểm Thử

Sau khi kiểm thử xong, agent điền vào template này:

```markdown
## 🧪 Kết Quả Kiểm Thử

**Thay đổi**: [mô tả ngắn gọn thay đổi đã thực hiện]

### Theme
- Dark Mode: ✅ OK / ⚠️ Issue: [mô tả]
- Light Mode: ✅ OK / ⚠️ Issue: [mô tả]

### Responsive  
- Mobile (375px): ✅ OK / ⚠️ Issue: [mô tả]
- Tablet (768px): ✅ OK / ⚠️ Issue: [mô tả]
- Desktop (1440px): ✅ OK / ⚠️ Issue: [mô tả]

### Components
- Sidebar Accordion: ✅ / ⚠️
- Dropdowns: ✅ / ⚠️
- Filter Card: ✅ / ⚠️
- Multi-Select: ✅ / ⚠️
- Theme Toggle: ✅ / ⚠️

### Regression
- Console Errors: None / [list errors]
- Visual Regression: None / [describe]

### Kết luận: ✅ PASS / ❌ FAIL (cần sửa: [danh sách])
```

---

## Quy Trình Xử Lý Khi Phát Hiện Bug

1. **Ghi nhận bug** — mô tả rõ: component nào, theme nào, breakpoint nào
2. **Xác định nguyên nhân** — bug do thay đổi mới hay đã tồn tại từ trước
3. **Fix bug** — thực hiện sửa
4. **Kiểm thử lại** — chạy lại TOÀN BỘ regression checklist (không chỉ phần vừa fix)
5. **Báo cáo** — cập nhật template báo cáo

> **QUAN TRỌNG**: Khi fix 1 bug mà tạo ra bug mới → PHẢI tiếp tục fix cho đến khi regression checklist sạch hoàn toàn. Không bao giờ để lại bug "sẽ fix sau".
