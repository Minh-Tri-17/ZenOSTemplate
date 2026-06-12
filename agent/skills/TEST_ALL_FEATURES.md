# Hướng dẫn Kiểm thử UI Hệ thống (UI Regression Testing)

Tài liệu này hướng dẫn cách kiểm thử các yếu tố UI/UX trên toàn bộ 3 trang của website **ZenOS Enterprise Dashboard** (`login.html`, `index.html`, `dashboard.html`).

> **Lưu ý phạm vi**: Project này là **template UI mẫu**. Kiểm thử chỉ cần đảm bảo:
> - Các modal mở/đóng tốt
> - Chuyển page hoạt động đúng
> - Chuyển form (form transitions) mượt mà
> - Theme & hệ thống sync ổn định
>
> **Không cần kiểm tra**: Tính năng xóa hàng (Delete rows), logic nghiệp vụ phức tạp, hay các tính năng chưa được implement hoàn chỉnh.

---

## 1. Mục tiêu Kiểm thử
Xác minh rằng toàn bộ UI hoạt động trực quan, hiệu ứng chuyển đổi mượt mà, không có lỗi JavaScript console, và các modal/dropdown/form transitions hoạt động đúng.

---

## 2. Danh mục Kiểm tra theo Trang

### 2.1. Trang Đăng Nhập (`login.html`)

1. **Chuyển hướng sau đăng nhập**:
   - Nhập bất kỳ Username/Password → Bấm Login → Chuyển hướng thành công sang `dashboard.html`.

2. **Chuyển form Quên mật khẩu (2 bước)**:
   - Nhấp **Forgot password?** → Card animate sang Bước 1 (không bị giật/nhấp nháy).
   - Nhập Email → Bấm **Send Verification Code** → Card animate sang Bước 2.
   - Điền thông tin Bước 2 → Bấm Submit → Hiện alert thành công → Quay về form Đăng nhập.
   - Nhấp **Back to Login** ở Bước 1 hoặc Bước 2 → Quay về form Đăng nhập đúng.

3. **Hiệu ứng co giãn chiều cao card**:
   - Card thay đổi chiều cao co giãn mượt mà theo từng form (không bị chồng lấp nội dung, không bị cắt xén).

---

### 2.2. Trang Danh sách (`index.html`)

1. **Bộ lọc (Filter panel open/close)**:
   - Click nút **Filter** → Vùng lọc `#filterCollapse` mở ra mượt mà (Bootstrap Collapse animation).
   - Click lại nút **Filter** → Vùng lọc thu gọn lại.
   - Multi-select dropdown: Click mở → chọn trạng thái → xuất hiện tag với nút `×` → click `×` bỏ tag.
   - Nút **Clear** → Reset toàn bộ bộ lọc về mặc định.

2. **Checkbox & Row Selection UI**:
   - Click checkbox tiêu đề → Tất cả hàng highlight (class `.row-selected`), nút **Delete** hiện số lượng `Delete (N)`.
   - Bỏ chọn một hàng → Checkbox tiêu đề chuyển sang trạng thái Indeterminate (dấu gạch ngang).
   - Bỏ chọn hết → Nút **Delete** bị disabled.

3. **Modal Create Node**:
   - Bấm **Create** → Modal `#createNodeModal` mở ra mượt mà (Bootstrap fade animation).
   - Kéo slider Load % → Số % cập nhật real-time.
   - Điền thông tin → Submit → Modal đóng lại, hàng mới xuất hiện ở đầu bảng với animation trượt nhẹ.
   - Bấm nút `×` hoặc Cancel → Modal đóng lại đúng.

4. **Modal Import**:
   - Bấm **Import** → Modal `#importModal` mở ra.
   - Vùng upload zone hiển thị đúng UI.
   - Bấm **Import** (trong modal) → Progress bar chạy từ 0% → 100% → Modal tự đóng.
   - Bấm **Cancel** → Modal đóng ngay lập tức.

5. **Modal Export**:
   - Bấm **Export** → Modal `#exportModal` mở ra.
   - 3 tùy chọn scope hiển thị đúng (All page / Current page / Select Items).
   - Bấm **Export** (trong modal) → Progress bar chạy từ 0% → 100% → Modal tự đóng.
   - Bấm **Cancel** → Modal đóng ngay lập tức.

---

### 2.3. Trang Dashboard (`dashboard.html`)

1. **Render tổng quan**:
   - Banner hiển thị đúng hình ảnh và nội dung.
   - 4 summary cards (Profit, Revenue, Expenses, Customers) hiển thị số liệu.

2. **Biểu đồ ApexCharts render**:
   - Tất cả các chart container có nội dung (không bị trống trắng): Column Chart, Line Chart, Donut Chart, Radar Chart, và các chart còn lại.
   - Hover vào biểu đồ → Tooltip hiện ra.

3. **Sơ đồ Tổ chức ApexTree**:
   - `#treeContainer` hiển thị sơ đồ cây với các node.
   - Thử Zoom In/Out nếu có nút điều khiển.

---

### 2.4. Đồng bộ hóa Toàn hệ thống

1. **Dark/Light Mode**:
   - Bấm nút Sun/Moon trên Topbar → Theme toàn trang chuyển đổi ngay lập tức.
   - Reload trang → Theme vẫn được giữ nguyên (persistence qua `localStorage`).
   - Chuyển sang trang khác → Theme vẫn áp dụng đúng.

2. **Language Switcher**:
   - Bấm icon Ngôn ngữ trên Topbar → Dropdown hiện ra.
   - Chọn một ngôn ngữ → Item được đánh dấu active.
   - Mở menu Mobile → Dropdown ngôn ngữ mobile hiển thị ngôn ngữ đang active đúng (sync).

3. **Notification Dropdown**:
   - Bấm icon chuông → Dropdown thông báo mở ra.
   - Click một thông báo chưa đọc → Thông báo mất style unread, badge số giảm.
   - Bấm **Mark all as read** → Tất cả thông báo mark đã đọc, badge biến mất.

---

## 3. Kịch bản Kiểm thử End-to-End (Tích hợp)

1. Mở `login.html`. Thực hiện luồng **Forgot password?** đầy đủ (Step 1 → Step 2 → Quay về Login).
2. Đăng nhập → Chuyển sang `dashboard.html`. Xác nhận theme được giữ nguyên.
3. Trên `dashboard.html`, bấm icon chuông → Mở/đóng notification dropdown. Bấm "Mark all as read".
4. Chuyển sang `index.html` qua Sidebar. Xác nhận theme vẫn đúng.
5. Mở bộ lọc → Chọn 2–3 status → Kiểm tra tags → Clear bộ lọc.
6. Bấm **Create** → Mở modal → Di chuyển slider → Submit → Kiểm tra hàng mới xuất hiện.
7. Bấm **Import** → Mở modal → Bấm Import → Xem progress bar → Modal tự đóng.
8. Bấm **Export** → Mở modal → Chọn scope → Bấm Export → Xem progress bar → Modal tự đóng.

---

## 4. Yêu cầu Báo cáo Kết quả

```markdown
# BÁO CÁO KIỂM THỬ UI — ZenOS Enterprise Dashboard

- **Người thực hiện**: [Tên AI/QA]
- **Môi trường / Browser**: [Ví dụ: Windows 11 / Chrome 124 / localhost:8080]
- **Thời gian**: [Thời gian]

## 1. Tóm tắt
- **Tổng số mục kiểm tra**: [Số lượng]
- **Đạt (PASS)**: [Số lượng]
- **Lỗi (FAIL)**: [Số lượng]

## 2. Kết quả chi tiết

### A. login.html
- [ ] Chuyển hướng sau đăng nhập: [PASS/FAIL]
- [ ] Chuyển form Quên mật khẩu (Step 1 → 2 → Login): [PASS/FAIL]
- [ ] Hiệu ứng co giãn chiều cao card: [PASS/FAIL]

### B. index.html
- [ ] Filter panel mở/đóng & Clear: [PASS/FAIL]
- [ ] Multi-select dropdown & tags: [PASS/FAIL]
- [ ] Checkbox row selection & Indeterminate: [PASS/FAIL]
- [ ] Modal Create Node (mở/đóng/submit): [PASS/FAIL]
- [ ] Modal Import (progress bar → tự đóng): [PASS/FAIL]
- [ ] Modal Export (progress bar → tự đóng): [PASS/FAIL]

### C. dashboard.html
- [ ] Banner & Summary Cards render: [PASS/FAIL]
- [ ] ApexCharts render & tooltip: [PASS/FAIL]
- [ ] ApexTree Org Chart render: [PASS/FAIL]

### D. System-wide Sync
- [ ] Dark/Light Mode toggle & localStorage persistence: [PASS/FAIL]
- [ ] Language Switcher sync (Topbar ↔ Mobile): [PASS/FAIL]
- [ ] Notification dropdown & unread badge: [PASS/FAIL]

## 3. Lỗi phát hiện
| Mã | Mô tả | Bước tái hiện | Mức độ |
|---|---|---|---|
| BUG-01 | ... | ... | High/Medium/Low |

## 4. Nhận xét UX/UI
...
```
