# Hướng dẫn Kiểm thử Đồng bộ Theme & Tính Nhất quán Giao diện (UI Consistency & Accessibility)

Tài liệu này hướng dẫn cách kiểm thử độ tương phản màu sắc của chế độ Sáng/Tối (Light/Dark Mode), tính đồng bộ và nhất quán trong thiết kế của các thành phần giao diện cơ bản (Buttons, Inputs) trên toàn bộ hệ thống **ZenOS Enterprise Dashboard**.

---

## 1. Mục tiêu Kiểm thử
- Đảm bảo tính nhất quán (Consistency) của hệ thống UI/UX trên cả 3 trang (`login.html`, `index.html`, `dashboard.html`).
- Đảm bảo độ tương phản màu sắc (Color Contrast) ở cả 2 chế độ sáng và tối đáp ứng các tiêu chuẩn dễ đọc của WCAG (Web Content Accessibility Guidelines).
- Xác minh hành vi chuyển đổi theme (Dark/Light mode) hoạt động đồng bộ và mượt mà trên tất cả các trang, không bị lỗi hiển thị.

---

## 2. Các Chỉ mục Kiểm thử Chi tiết

### 2.1. Đồng bộ & Tương phản Theme (Dark/Light Mode Sync)
1. **Kiểm tra Kích hoạt Class & LocalStorage**:
   - Khi bấm nút Sun/Moon trên topbar hoặc menu mobile:
     - Thẻ `<html>` phải có thuộc tính `data-theme="dark"` (khi chuyển sang chế độ tối) và xóa thuộc tính này (khi chuyển sang chế độ sáng).
     - Biến `localStorage.getItem("zenOS-theme")` phải được cập nhật tương ứng thành `"dark"` hoặc `"light"`.
     - Trạng thái này phải được giữ nguyên khi chuyển trang hoặc tải lại trang.
2. **Kiểm tra Độ tương phản Màu sắc Chữ và Nền**:
   - Ở cả 2 chế độ Sáng và Tối, hãy đọc các biến CSS trong [styles.css](file:///d:/Backup/Source code/ZenOSProject/ZenOSFrontend/ZenOSTemplate/styles.css) và kiểm tra trực quan độ dễ đọc của các cặp màu sau:
     - **Tiêu đề chính**: Cặp màu `--color-primary` trên nền `--color-background`.
     - **Văn bản mô tả phụ**: Cặp màu `--color-on-surface-variant` trên nền bề mặt card `--glass-card-bg`.
     - **Các văn bản lỗi/cảnh báo**: Cặp màu `--color-error` trên nền cảnh báo tương ứng.
     - **Các liên kết**: Các thẻ `<a>` (Forgot password?, Back to Login, links ở sidebar) phải rõ ràng, không bị chìm màu.
3. **Đồng bộ Màu sắc Biểu đồ (ApexCharts)**:
   - Khi chuyển sang chế độ Tối (Dark):
     - Các nhãn (labels) trên trục X/Y của biểu đồ phải đổi sang màu sáng (ví dụ: `#c4c7c9`).
     - Đường lưới (grid lines) phải đổi sang màu tối dịu (ví dụ: `rgba(255, 255, 255, 0.1)`).
   - Khi chuyển sang chế độ Sáng (Light):
     - Các nhãn phải đổi sang màu tối (ví dụ: `#555770`).
     - Đường lưới phải đổi sang màu xám mờ (ví dụ: `rgba(0, 0, 0, 0.08)`).
   - Màu sắc của text trong sơ đồ ApexTree phải tương phản rõ rệt với màu nền card kính.

### 2.2. Tính Nhất quán của Button (Nút Bấm)
1. **Nút bấm Phong cách macOS/iOS Premium (`.button`)**:
   - Các nút submit chính (Nút Login, Nút Send Verification Code ở Step 1, Nút Register ở Step 2, Nút submit trong Create Node Modal) phải sử dụng chung class `.button` để đảm bảo:
     - Có cùng chiều cao (`height: 40px`), cùng góc bo (`border-radius: 8px`).
     - Có đầy đủ hiệu ứng góc gập `.fold` và các điểm vi hạt lơ lửng `.points-wrapper`.
     - Các thuộc tính gradient màu nền và hiệu ứng active (`transform: scale(0.95)`) hoạt động đồng bộ.
2. **Nút bấm Ghost (`.ghost-button`)**:
   - Các nút phụ như nút Cancel trong modal, nút Filter ở Action Bar phải có chung kiểu viền mờ, màu chữ phản hồi theo theme mode, cùng kích thước font (`font-size: 14px`) và cùng độ bo góc.
3. **Trạng thái Hover & Active**:
   - Tất cả các nút bấm khi hover phải có hiệu ứng chuyển đổi mượt mà (`transition: all 0.2s` hoặc `transition: transform 0.2s`).
   - Không được có nút bấm nào bị giật hình hoặc thay đổi đột ngột kích thước khi hover/click.

### 2.3. Tính Nhất quán của Input (Trường Nhập Liệu)
1. **Input Phong cách Viền Mờ (`.ghost-input`)**:
   - Mọi trường nhập liệu dạng text/email/password trên trang Login, vùng Filter, và Modal Tạo mới Node đều phải sử dụng class `.ghost-input`.
   - Kiểm tra các thuộc tính để đảm bảo đồng bộ:
     - Độ bo góc (`border-radius: 8px`).
     - Padding trong input phải thống nhất (đảm bảo văn bản nhập vào không bị dính sát viền).
     - Màu sắc của placeholder phải dễ nhìn nhưng mờ hơn chữ nhập vào, ở cả 2 theme.
2. **Trạng thái Focus (Active Input)**:
   - Khi click vào bất kỳ ô input nào, trạng thái `:focus` phải đồng nhất:
     - Đổi màu viền và đổ bóng nhẹ (ví dụ: sử dụng shadow màu lam nhạt hoặc shadow mờ mềm mại).
     - Màu nền của input có thể sáng nhẹ hoặc tối nhẹ tùy theme.
3. **Checkbox tùy chỉnh (`.form-check-input`)**:
   - Checkbox "Remember password" ở trang Login, Checkbox ở hàng tiêu đề bảng `#selectAllCheckbox`, và các checkbox dòng trong bảng phải có cùng kích thước (`18px x 18px`), cùng độ bo góc (`4px`).
   - Hiệu ứng hiển thị dấu tick trắng trên nền tối đậm khi checked phải đồng bộ.

---

## 3. Quy trình Kiểm tra và Đo lường (Test Steps)
1. **Kiểm tra độ lệch style (Style Drift Audit)**:
   - Sử dụng Developer Tools của trình duyệt để kiểm tra `border-radius`, `font-family`, và `padding` của các nút bấm và ô input ở các trang khác nhau. Ghi nhận các giá trị không khớp.
2. **Kiểm tra tương phản màu sắc (Contrast Ratio Test)**:
   - Dùng tính năng Audit/Lighthouse của Chrome DevTools hoặc bộ kiểm tra màu sắc (Color Contrast Eyedropper) để đo tỷ lệ tương phản chữ/nền tại các vị trí chính ở cả 2 chế độ Sáng và Tối.
3. **Kiểm tra hiệu ứng nháy sáng (FOUC Test)**:
   - FOUC xảy ra khi trang web tải chế độ Sáng trước rồi mới áp dụng chế độ Tối (khiến người dùng bị chói mắt trong tích tắc). Thử tải lại trang ở chế độ Tối nhiều lần và quan sát xem có hiện tượng nháy trắng không.

---

## 4. Yêu cầu Báo cáo Kết quả (Test Report Requirements)
Sau khi thực hiện kiểm thử sự đồng bộ và nhất quán giao diện, AI/người kiểm thử phải cung cấp báo cáo theo mẫu sau:

```markdown
# BÁO CÁO KIỂM THỬ ĐỒNG BỘ THEME & NHẤT QUÁN GIAO DIỆN (UI/UX AUDIT)

- **Người thực hiện**: [Tên AI/QA]
- **Thời gian đánh giá**: [Thời gian]

## 1. Đánh giá Đồng bộ Theme (Dark/Light Mode)
- **Tình trạng lưu trữ và chuyển đổi**: [Tốt / Có độ trễ / Lỗi không lưu theme...]
- **Hiện tượng nháy sáng (FOUC)**: [Có bị nháy sáng khi reload trang không?]
- **Bảng đánh giá độ tương phản màu sắc (WCAG AAA/AA)**:
  | Cặp Màu sắc (Chữ / Nền) | Trạng thái ở Light Mode | Trạng thái ở Dark Mode | Đánh giá độ dễ đọc |
  | :--- | :---: | :---: | :--- |
  | Văn bản chính / Nền Body | [Đạt / Không đạt] | [Đạt / Không đạt] | |
  | Văn bản phụ / Nền Card | [Đạt / Không đạt] | [Đạt / Không đạt] | |
  | Chữ liên kết / Nền Card | [Đạt / Không đạt] | [Đạt / Không đạt] | |
  | Nhãn biểu đồ / Nền lưới | [Đạt / Không đạt] | [Đạt / Không đạt] | |

## 2. Đánh giá Tính nhất quán của Nút bấm (Buttons)
- **Độ bo góc và kích thước**: [Thống nhất 8px hay bị lệch?]
- **Các hiệu ứng chuyển động (Hover, Active)**: [Hoạt động mượt mà / Có nút bị giật hình...]
- **Các vị trí phát hiện sự bất đồng nhất (nếu có)**:
  - *Ví dụ: Nút "Create" ở trang Index có style khác biệt so với Nút "Login" ở trang Login...*

## 3. Đánh giá Tính nhất quán của Trường nhập liệu (Inputs)
- **Kiểu dáng `.ghost-input`**: [Đồng bộ ở mọi trang / Có ô nhập liệu bị lệch font/border...]
- **Hiệu ứng `:focus`**: [Đồng nhất đổ bóng viền / Có ô bị mất hiệu ứng focus...]
- **Đồng bộ Checkbox và Slider**: [Kích thước và hoạt ảnh checked có đồng đều không?]

## 4. Kết luận & Khuyến nghị cải thiện
...
```
