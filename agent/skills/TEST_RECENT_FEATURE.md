# Hướng dẫn Kiểm thử Chức năng Đặt lại Mật khẩu (Bước 2)

Tài liệu này hướng dẫn cách kiểm thử tự động hoặc thủ công đối với tính năng **Bước 2: Đặt lại mật khẩu** vừa được tích hợp vào luồng Đăng nhập trong dự án **ZenOS Enterprise Dashboard**.

---

## 1. Mục tiêu Kiểm thử
Xác minh luồng biểu mẫu khôi phục mật khẩu hoạt động trơn tru từ Bước 1 sang Bước 2, kiểm tra hoạt ảnh chuyển tiếp, các trường nhập liệu ở Bước 2, và hành động quay về trang đăng nhập chính.

## 2. Các Thành phần Cần Kiểm tra
- **Form Section**: `#resetPasswordStep2Section` trong file [login.html](file:///d:/Backup/Source code/ZenOSProject/ZenOSFrontend/ZenOSTemplate/login.html).
- **Các trường Input ở Bước 2**:
  - Tên đăng nhập: `#resetUsername` (required)
  - Mật khẩu mới: `#resetNewPassword` (required, type="password")
  - Số điện thoại đăng ký: `#resetPhone` (required)
  - Mã OTP: `#resetOtp` (required)
- **Nút Submit**: Button `.btn-step-submit` chứa hiệu ứng góc gập (`.fold`) và hạt lơ lửng (`.points-wrapper`).
- **Nút Quay lại**: `#btnStep2BackToLogin` ("Back to Login").

---

## 3. Kịch bản Kiểm thử Chi tiết (Test Scenarios)

### Kịch bản 1: Luồng chuẩn (Happy Path)
1. Truy cập trang `login.html`.
2. Nhấp vào liên kết **"Forgot password?"**. Xác nhận form trượt sang Bước 1 (`#resetFormSection`).
3. Điền một email bất kỳ vào ô Email và nhấn nút **"Send Verification Code"**.
4. Xác nhận form trượt mượt mà sang **Bước 2** (`#resetPasswordStep2Section`).
5. Điền đầy đủ thông tin vào các trường ở Bước 2:
   - Tên đăng nhập: `admin`
   - Mật khẩu mới: `NewPassword123`
   - Số điện thoại đăng ký: `0987654321`
   - Mã OTP: `123456`
6. Nhấp vào nút **"Register"** (nút submit).
7. Hệ thống phải hiển thị hộp thoại thông báo: `"Password has been reset successfully!"`.
8. Sau khi nhấn OK trên hộp thoại thông báo, biểu mẫu phải tự động chuyển tiếp mượt mà quay lại trang **Đăng nhập** chính (`#loginFormSection`).

### Kịch bản 2: Quay lại trang Đăng nhập từ Bước 2 (Back to Login)
1. Thực hiện các bước từ 1 đến 4 của Kịch bản 1 để đến màn hình **Bước 2**.
2. Nhấp vào liên kết **"Back to Login"** (`#btnStep2BackToLogin`).
3. Xác nhận biểu mẫu chuyển đổi mượt mà và hiển thị lại trang **Đăng nhập** chính (`#loginFormSection`).

### Kịch bản 3: Kiểm tra Validation HTML5 ở Bước 2
1. Đến màn hình **Bước 2**.
2. Để trống một hoặc nhiều trường (ví dụ: không nhập OTP hoặc không nhập Số điện thoại).
3. Nhấn nút **"Register"**.
4. Xác nhận trình duyệt ngăn chặn submit biểu mẫu và hiển thị thông báo yêu cầu điền đầy đủ thông tin (HTML5 validation).

### Kịch bản 4: Hiệu ứng chuyển động (Form Transition & Button Effects)
1. Khi chuyển tiếp giữa các bước (Login -> Step 1 -> Step 2 -> Login), kiểm tra chiều cao của thẻ `#formCard` có thay đổi mượt mà (`transition: height 0.35s`) không bị giật lag hay chồng chéo chữ.
2. Kiểm tra hiệu ứng hover và active trên nút **"Register"**:
   - Khi hover: góc gập `.fold` thu nhỏ/di chuyển, các chấm nhỏ bay lượn từ dưới lên.
   - Khi click active: nút co giãn nhẹ (`transform: scale(0.95)`).

---

## 4. Yêu cầu Báo cáo Kết quả (Test Report Requirements)
Sau khi thực hiện kiểm thử, AI/người kiểm thử cần đưa ra báo cáo chi tiết theo cấu trúc sau:

```markdown
# BÁO CÁO KẾT QUẢ KIỂM THỬ: CHỨC NĂNG ĐẶT LẠI MẬT KHẨU (BƯỚC 2)

- **Người/AI thực hiện**: [Tên]
- **Thời gian thực hiện**: [Thời gian]
- **Trình duyệt/Môi trường**: [Chrome/Firefox/Edge...]

## 1. Kết quả Kịch bản Kiểm thử
| Mã Kịch bản | Tên Kịch bản | Kết quả (PASS/FAIL) | Ghi chú / Chi tiết lỗi (nếu có) |
| :--- | :--- | :---: | :--- |
| TC-01 | Luồng chuẩn (Happy Path) | | |
| TC-02 | Quay lại từ Bước 2 (Back to Login) | | |
| TC-03 | HTML5 Form Validation | | |
| TC-04 | Hiệu ứng chuyển tiếp & Button | | |

## 2. Đánh giá Trải nghiệm Người dùng (UI/UX)
- Tốc độ chuyển đổi giữa các bước: [Mượt mà/Hơi chậm/Bị giật...]
- Trực quan hóa chiều cao của card `#formCard`: [Co giãn tốt/Bị vỡ layout...]
- Các hiệu ứng vi hạt (particles) và góc gập trên nút hoạt động đúng: [Có/Không]

## 3. Kết luận & Đề xuất
- Tổng số kịch bản đạt: [X/4]
- Đề xuất sửa đổi (nếu có): ...
```
