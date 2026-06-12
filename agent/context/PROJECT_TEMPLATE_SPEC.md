# Tài Liệu Đặc Tả Thiết Kế và Cấu Trúc ZenOS Enterprise Template

---

Tài liệu này mô tả chi tiết toàn bộ mã nguồn, cấu trúc thư mục, giao diện (UI/UX) và các logic xử lý JavaScript của dự án **ZenOS Enterprise Dashboard**. Mục tiêu là giúp các AI đọc tài liệu này có thể tái tạo hoặc khởi tạo lại một template hoàn toàn tương đồng từ thiết kế thẩm mỹ (Glassmorphism, Theme mode, Animations) đến các hành vi tương tác động.

---

## 1. Cấu Trúc Thư Mục Dự Án (Directory Structure)

```text
ZenOSTemplate/
├── images/
│   ├── logo.png       # Logo thương hiệu ZenOS
│   ├── topic.png      # Ảnh minh họa (Spa Therapy / Tech Art) dùng ở trang Login & Banner
│   ├── avatar.jpg     # Ảnh đại diện của người dùng (Executive User)
│   ├── vn.png         # Quốc kỳ Việt Nam (Flag)
│   ├── us.png         # Quốc kỳ Mỹ (Flag)
│   ├── cn.png         # Quốc kỳ Trung Quốc (Flag)
│   ├── jp.png         # Quốc kỳ Nhật Bản (Flag)
│   └── kr.png         # Quốc kỳ Hàn Quốc (Flag)
├── login.html         # Trang đăng nhập và khôi phục mật khẩu (chuyển đổi form động)
├── index.html         # Trang quản lý danh sách Node (Bộ lọc, Bảng dữ liệu, CRUD, Import/Export)
├── dashboard.html     # Trang tổng quan số liệu (ApexCharts, ApexTree, Banner, Summary Cards)
└── styles.css         # File CSS chứa toàn bộ Design System, Theme Mode (Light/Dark) và Animations
```

---

## 2. Thư Viện và Liên Kết CDN (Dependencies & CDNs)

Để đảm bảo các hiệu ứng và chức năng hoạt động chính xác, cả 3 trang HTML đều tích hợp các tài nguyên ngoài sau đây trong thẻ `<head>` hoặc trước đóng thẻ `</body>`:

- **Google Fonts**: Font chữ chủ đạo **Plus Jakarta Sans** (Weights: 300, 400, 500, 600, 700, 800)
  ```html
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;500;600;700;800&display=swap" rel="stylesheet" />
  ```
- **Bootstrap 5.3.3 CSS**: Dùng làm khung layout grid và các thành phần CSS cơ bản.
  ```html
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet" />
  ```
- **FontAwesome (v6 hoặc v7)**: Dùng để hiển thị các Icon chất lượng cao.
  ```html
  <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/7.0.1/css/all.min.css" rel="stylesheet" />
  ```
- **jQuery 3.7.1**: Dùng cho toàn bộ các tương tác DOM động và xử lý logic form.
  ```html
  <script src="https://code.jquery.com/jquery-3.7.1.min.js"></script>
  ```
- **Bootstrap 5.3.3 JS Bundle**: Xử lý các dropdown, modal và collapse.
  ```html
  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>
  ```
- **ApexCharts** & **ApexTree** (chỉ có ở `dashboard.html`): Vẽ biểu đồ và sơ đồ tổ chức.
  ```html
  <script src="https://cdn.jsdelivr.net/npm/apexcharts"></script>
  <script src="https://cdn.jsdelivr.net/npm/apextree"></script>
  ```

---

## 3. Đặc Tả File `styles.css` (Hệ Thống Design System)

File [styles.css](file:///d:/Backup/Source code/ZenOSProject/ZenOSFrontend/ZenOSTemplate/styles.css) là linh hồn của giao diện, định nghĩa triết lý thiết kế **Glassmorphism hiện đại** kết hợp **Dark/Light Mode** đồng bộ.

### 3.1. Các Biến CSS (CSS Custom Variables)

Đặc tả bảng màu HSL / Hex và các thuộc tính hiệu ứng kính (glass):

| Tên biến CSS                 | Mô tả / Công dụng             | Giá trị Light Mode (`:root`)     | Giá trị Dark Mode (`[data-theme="dark"]`) |
| :--------------------------- | :---------------------------- | :------------------------------- | :---------------------------------------- |
| `--color-primary`            | Màu chính / Chữ tiêu đề chính | `#1a1a2e`                        | `#ffffff`                                 |
| `--color-secondary`          | Màu phụ / Nút nhấn phụ        | `#41548c`                        | `#b4c4f9`                                 |
| `--color-background`         | Màu nền hệ thống              | `#f5f5f7`                        | `#131313`                                 |
| `--color-surface`            | Màu nền của bề mặt card       | `#f5f5f7`                        | `#131313`                                 |
| `--color-on-surface-variant` | Màu chữ phụ, chú thích        | `#555770`                        | `#c4c7c9`                                 |
| `--color-error`              | Màu trạng thái lỗi            | `#ba1a1a`                        | `#ffb4ab`                                 |
| `--glass-bg`                 | Màu nền đè dạng kính mờ       | `rgba(255, 255, 255, 0.7)`       | `rgba(255, 255, 255, 0.05)`               |
| `--glass-border`             | Màu viền kính mờ              | `rgba(0, 0, 0, 0.08)`            | `rgba(255, 255, 255, 0.2)`                |
| `--glass-card-bg`            | Màu nền card kính mờ          | `rgba(255, 255, 255, 0.45)`      | `rgba(255, 255, 255, 0.05)`               |
| `--glass-card-border`        | Viền của card kính mờ         | `rgba(255, 255, 255, 0.35)`      | `rgba(255, 255, 255, 0.2)`                |
| `--glass-card-shadow`        | Bóng đổ mịn của card          | `0 8px 32px rgba(0, 0, 0, 0.06)` | `0 20px 40px rgba(0, 0, 0, 0.4)`          |
| `--body-gradient`            | Nền gradient động phía sau    | Linear + Radial Soft Gradients   | Darker Radial Gradients                   |
| `--sidebar-bg`               | Nền thanh bên                 | `rgba(255, 255, 255, 0.55)`      | `rgba(255, 255, 255, 0.1)`                |
| `--topbar-bg`                | Nền thanh công cụ trên        | `rgba(255, 255, 255, 0.5)`       | `rgba(28, 27, 27, 0.5)`                   |

### 3.2. Thành Phần Nền Tảng (Base Components)

- **`.glass-card`**:
  - Thuộc tính cốt lõi: `backdrop-filter: blur(15px); -webkit-backdrop-filter: blur(15px); border: 0.5px solid var(--glass-card-border); background-color: var(--glass-card-bg); box-shadow: var(--glass-card-shadow); border-radius: 20px; transition: transform 0.3s, box-shadow 0.3s, border-color 0.3s;`
  - Hiệu ứng Hover: `transform: translateY(-2px);` và tăng độ đậm của bóng đổ/viền.
- **`.ghost-input`** (Input phong cách viền mờ):
  - `background-color: transparent; border: 0.5px solid var(--input-border-color); border-radius: 8px; color: var(--color-primary);`
  - Hiệu ứng Focus: `background-color: var(--input-focus-bg); border-color: var(--input-focus-border); box-shadow: var(--input-focus-shadow);`
- **`.ghost-button`** (Nút phong cách tối giản):
  - `border: 1px solid var(--btn-border-color); background-color: transparent; color: var(--color-primary); border-radius: 8px; font-size: 14px; gap: 8px; transition: all 0.2s;`
  - Hiệu ứng Hover: `background-color: var(--btn-hover-bg); transform: translateY(-1px);`
- **`.btn-primary`** (Nút chính mang phong cách macOS/iOS Premium):
  - `background-color: #242426; color: #ffffff; border: none; font-weight: 600; box-shadow: 0 4px 12px rgba(36,36,38,0.15), inset 0 1px 0 rgba(255,255,255,0.15);` (Khi chuyển sang Dark Mode sẽ đổi thành màu sáng `#e2e2e9` chữ tối `#242426`).
- **`.form-check-input`** (Checkbox tùy chỉnh):
  - `width: 18px; height: 18px; border-radius: 4px; border: 1px solid var(--input-border-color); background-color: transparent;`
  - Khi Checked: Nền tối đậm, có hình ảnh SVG dấu tick màu trắng (hoặc đen ở Dark Mode).
- **`.btn-close`** (Nút đóng chéo):
  - `background-color: rgba(0, 0, 0, 0.05); border-radius: 50%; transition: all 0.25s;`
  - Hiệu ứng Hover: `transform: rotate(90deg); background-color: rgba(0,0,0,0.12);`

### 3.3. Hiệu Ứng Micro-Animations (Keyframes)

- **`bellRing`** (Chuông rung tự động cho icon thông báo):
  - Rung xoay nhẹ từ góc trên giữa (`transform-origin: top center;`), tự động kích hoạt lặp đi lặp lại sau mỗi 3 giây (`animation: bellRing 3s ease-in-out infinite;`).
- **`sunGlow`** (Mặt trời tỏa sáng & xoay đều):
  - Xoay tròn từ `0deg` đến `360deg` kết hợp bộ lọc bóng phát sáng vàng cam (`filter: drop-shadow(...)`).
- **`moonSwing`** (Mặt trăng đu đưa nhẹ nhàng):
  - Xoay góc nghiêng từ `-10deg` đến `12deg` kèm theo bóng mờ phát sáng nhẹ màu xanh lam/tím nhạt.
- **`languageWave`** (Icon ngôn ngữ lượn sóng 3D):
  - Xoay trục Y nghiêng góc 3D (`transform: rotateY(...)`) tạo cảm giác vẫy chào.
- **`langDropdownFadeIn`** (Dropdown hiện lên mượt):
  - Hiệu ứng fade-in kết hợp phóng to nhẹ (`opacity: 0; transform: translateY(-8px) scale(0.96);` đến `opacity: 1; transform: translateY(0) scale(1);`).

### 3.4. Bố Cục Layout

Gồm cấu trúc khung linh hoạt (Flexbox/Grid) hỗ trợ Responsive:

- **Desktop Layout**:
  - `.sidebar`: Cố định bên trái (`width: 256px; position: fixed;`).
  - `.topbar`: Cố định góc trên bên phải (`left: 256px; width: calc(100% - 281px); height: 80px; position: fixed; margin-left: 20px; border-bottom-left-radius: 20px;`).
  - `.main-content`: Nằm bên phải sidebar (`margin-left: 256px; padding: 104px 24px 24px;`).
- **Mobile Layout (Dưới 768px)**:
  - `.mobile-header`: Hiển thị ở góc trên (`display: flex; height: 64px; position: fixed; width: 100%;`).
  - `.sidebar`: Được ẩn bằng `transform: translateX(-100%);` và kích hoạt dạng trượt khi nhấn nút Menu Burger thông qua việc thêm class `.active`.
  - `.topbar`: Bị ẩn đi (`display: none;`).
  - `.main-content`: Đẩy sát lề (`margin-left: 0; padding-top: 88px;`).

---

## 4. Trang Đăng Nhập (`login.html`)

Trang đăng nhập tối giản và sang trọng. Sử dụng bố cục 2 cột chính.

### 4.1. Cấu Trúc Giao Diện

- **Cột Trái (`.login-left`)**: Chiếm 50% chiều rộng trên màn hình lớn. Chứa `.art-frame` và thẻ ảnh `<img src="images/topic.png" class="art-image">`.
- **Cột Phải (`.login-right`)**: Chứa `.login-card` (id: `#formCard`) có nền kính mờ. Bên trong chứa 3 phân đoạn form (chỉ 1 phân đoạn hiển thị tại một thời điểm nhờ class `.active-form`):
  1.  **Form Đăng Nhập (`#loginFormSection`)**: Nhập Username, Password, nút ghi nhớ mật khẩu, nút liên kết "Forgot password?" và nút submit "Login" có hiệu ứng động.
  2.  **Form Reset Mật Khẩu Bước 1 (`#resetFormSection`)**: Nhập Email, nút submit "Send Verification Code" và liên kết "Back to Login".
  3.  **Form Đặt Lại Mật Khẩu Bước 2 (`#resetPasswordStep2Section`)**: Nhập Tên đăng nhập (Username), Mật khẩu mới (New Password), Số điện thoại đăng ký (Phone), OTP. Nút submit "Register" và liên kết "Back to Login".

### 4.2. Logic JavaScript Chuyển Đổi Form Mượt Mà (Dynamic Height Transition)

Để tạo trải nghiệm mượt mà khi di chuyển giữa các bước, hàm dùng chung `transitionForms(fromSection, toSection, animationClass)` tự động tính toán chiều cao đích và thực hiện chuyển tiếp hiệu ứng trượt/phai mờ:

```javascript
function transitionForms(fromSection, toSection, animationClass = "form-fade-in-slide-up") {
  if (isTransitioning) return;
  isTransitioning = true;

  const initialHeight = formCard.offsetHeight;
  formCard.style.height = `${initialHeight}px`;

  fromSection.classList.add("form-fade-out");

  setTimeout(() => {
    fromSection.classList.remove("active-form", "form-fade-out");
    toSection.classList.add("active-form", animationClass);

    formCard.style.height = "auto";
    const targetHeight = formCard.offsetHeight;
    formCard.style.height = `${initialHeight}px`;

    formCard.offsetHeight; // force reflow
    formCard.style.height = `${targetHeight}px`;

    setTimeout(() => {
      toSection.classList.remove(animationClass);
      formCard.style.height = "auto"; 
      isTransitioning = false;
    }, 350); 
  }, 250);
}

// Logic chuyển đổi:
// 1. Click "Forgot password?" -> Transition từ Login sang Step 1
// 2. Submit Step 1 -> Transition từ Step 1 sang Step 2
// 3. Submit Step 2 -> Hiện thông báo thành công và Transition về Login
// 4. Click "Back to Login" tại các bước -> Transition về Login
```

---

## 5. Trang Quản Lý Bảng Dữ Liệu (`index.html`)

Trang quản lý các "Node" hạ tầng của hệ thống ZenOS, cung cấp tính năng CRUD mô phỏng và lọc nâng cao.

### 5.1. Các Khối Chức Năng Chính

1.  **Khối Thống Kê Nhanh (`stat-cards-grid`)**: Grid gồm 4 card thủy tinh thể hiện các chỉ số: Total Nodes, Active Regions, System Uptime, Average Load.
2.  **Thanh Thao Tác (`action-bar`)**:
    - Bên trái: Nút "Create" (Mở modal thêm), "Update" (Bị disabled mặc định), "Delete" (Nút đỏ lỗi, bị disabled mặc định).
    - Bên phải: Nút "Import", "Export" và Nút "Filter" để thu gọn/mở rộng bộ lọc.
3.  **Bộ Lọc Thu Gọn (`#filterCollapse`)**:
    - Trạng thái (Status): Dùng hộp chọn **Multi-select dropdown** độc đáo.
    - Khu vực (Region): Hộp chọn Select chuẩn mờ.
    - Mã định danh (Identifier): Ghost input text.
    - Bộ lọc xóa (Is Delete): Checkbox đánh dấu xem bản ghi đã xóa chưa.
    - Nút "Clear" xóa sạch bộ lọc.
4.  **Bảng Dữ Liệu (`data-table`)**:
    - Hàng tiêu đề: Có checkbox tổng `#selectAllCheckbox`.
    - Các cột: Node ID, Status, Region, Load %, Last Sync, Create At, Create By, Update At, Update By.
    - Cột Status: Chứa các `.status-badge` có dấu chấm tròn màu tương ứng (`status-optimal` - xanh lá, `status-warning` - vàng, `status-critical` - đỏ, `status-offline` - xám).
    - Cột Load %: Chứa thanh tiến trình dạng nhỏ `.load-bar` có màu sắc biến đổi theo phần trăm tải (Success khi tải thấp, Warning khi tải vừa, Error khi quá tải).
5.  **Phần Chân Bảng (Table Footer)**: Lựa chọn số lượng bản ghi hiển thị (10/25/50) và bộ nút phân trang (Pagination).

### 5.2. Các Modal Tương Tác

- **Modal Tạo Mới Node (`#createNodeModal`)**: Form nhập Node ID, chọn Status, chọn Region, và một thanh kéo chọn phần trăm Load (`#nodeLoadSlider`) hiển thị nhãn số phần trăm động.
- **Modal Export Dữ Liệu (`#exportModal`)**: Cho phép chọn phạm vi xuất dữ liệu (Tất cả, Trang hiện tại, hoặc các dòng đang chọn). Khi bấm Export sẽ chạy tiến trình mô phỏng chạy từ 0% đến 100% kèm nhãn trạng thái thay đổi liên tục.
- **Modal Import Dữ Liệu (`#importModal`)**:
  - Vùng kéo thả tệp tin (`.import-upload-zone`): Bắt các sự kiện `dragover`, `dragleave`, `drop` để hiển thị hiệu ứng viền đứt nét sáng màu.
  - Hộp thoại chọn file ẩn, hiển thị tên file kèm theo nút xóa tệp đã chọn.
  - Thanh tiến trình chạy mô phỏng sau khi bấm Import.
- **Modal About và Notifications**: Hiển thị thông tin hệ thống và danh sách thông báo đầy đủ.

### 5.3. Logic JavaScript Động

- **Xử lý Lựa chọn Checkbox trong Bảng (Table Row Checks)**:

  ```javascript
  // Đồng bộ checkbox tổng và checkbox dòng
  $selectAllCheckbox.on("change", function () {
    const isChecked = $(this).prop("checked");
    $(".data-table tbody .row-checkbox").prop("checked", isChecked);
    updateUIState();
  });

  // Cập nhật trạng thái UI khi checkbox thay đổi
  function updateUIState() {
    const $checkedBoxes = $(".data-table tbody .row-checkbox:checked");
    const checkedCount = $checkedBoxes.length;
    const totalCount = $(".data-table tbody .row-checkbox").length;

    // Cập nhật thuộc tính indeterminate của checkbox tổng
    if (checkedCount === 0) {
      $selectAllCheckbox.prop({ checked: false, indeterminate: false });
    } else if (checkedCount === totalCount) {
      $selectAllCheckbox.prop({ checked: true, indeterminate: false });
    } else {
      $selectAllCheckbox.prop({ checked: false, indeterminate: true });
    }

    // Thêm class highlight dòng được chọn
    $(".data-table tbody .row-checkbox").each(function () {
      const $row = $(this).closest("tr");
      if ($(this).prop("checked")) {
        $row.addClass("row-selected");
      } else {
        $row.removeClass("row-selected");
      }
    });

    // Kích hoạt/Vô hiệu hóa nút xóa dựa trên số lượng chọn
    if (checkedCount > 0) {
      $deleteBtn.removeAttr("disabled").html(`<i class="fa-solid fa-trash"></i>Delete (${checkedCount})`);
    } else {
      $deleteBtn.attr("disabled", "disabled").html(`<i class="fa-solid fa-trash"></i>Delete`);
    }
  }
  ```

- **Hành động Thêm Hàng Mới (Create Node Submission)**:
  Khi submit `#createNodeForm`, dữ liệu được chuyển đổi thành mã HTML `<tr>` mới, thêm class hiệu ứng rồi chèn vào trên cùng của `<tbody>` bằng phương thức `.prepend()`. Dòng mới sẽ trượt xuống nhẹ nhàng nhờ CSS transition:
  ```javascript
  $newRow.css({ opacity: "0", transform: "translateY(-10px)", transition: "all 0.4s ease-out" });
  $tbody.prepend($newRow);
  requestAnimationFrame(() => {
    $newRow.css({ opacity: "1", transform: "translateY(0)" });
  });
  ```
- **Bộ Lọc Trạng Thái Multi-select**:
  Nhấp vào toggle sẽ hiển thị menu các checkbox. Khi tích chọn/bỏ tích, một thẻ tag đại diện màu kính mờ sẽ được tạo động chèn vào khung hiển thị (có dấu nhân `&times;` bên cạnh để nhấp nhanh xóa thẻ).

---

## 6. Trang Số Liệu & Biểu Đồ (`dashboard.html`)

Trang phân tích trực quan nâng cao, sử dụng các thư viện ApexCharts và ApexTree để hiển thị dữ liệu trực quan sinh động.

### 6.1. Khối Cấu Trúc Đặc Trưng

- **Banner Chào Mừng Kỹ Thuật Số (`.banner-card`)**: Thiết kế chiếm 2/3 hàng đầu tiên, có tiêu đề lớn "Optimized Solutions for Enterprise 4.0 🎯" và nút khám phá. Nền bên phải chứa ảnh minh họa `images/topic.png` lồng ghép khéo léo.
- **Bốn Card Tóm Tắt Chỉ Số (`.summary-card`)**: Gồm Profit (Optimal - Xanh lá), Revenue (Warning - Vàng), Expenses (Critical - Đỏ), Customers (Secondary - Xanh lam), hiển thị biểu đồ nhỏ hoặc số liệu lớn.
- **Khu Vực Vẽ Sơ Đồ Tổ Chức (`#treeContainer`)**: Một vùng chứa kính mờ, hiển thị sơ đồ cây cấu trúc nhân sự cấp cao thông qua thư viện ApexTree.

### 6.2. Cấu Hình và Đồng Bộ Hóa ApexCharts Theo Theme Mode

Có 6 loại biểu đồ được cấu hình động trên trang:

1.  **Column Chart** (`#columnChart`): Thể hiện dữ liệu doanh số/tải lượng hàng tháng.
2.  **Line Chart** (`#lineChart`): Thể hiện xu hướng theo thời gian thực.
3.  **Donut Chart** (`#donutChart`): Phân tích tỉ trọng các danh mục món ăn/nước uống bán chạy.
4.  **Radar Chart** (`#radarChart`): Đánh giá đa chiều năng lực hệ thống.
5.  **Funnel Chart** (`#funnelChart`): Biểu đồ phễu nằm ngang thể hiện chuỗi chuyển đổi nguyên liệu.
6.  **Slope Chart** (`#slopeChart`): Đường dốc thể hiện biến động chéo giữa các danh mục.

Để biểu đồ đổi màu chữ nhãn và màu đường kẻ lưới khi người dùng đổi Theme Mode (Sáng/Tối), hàm `syncChartsTheme(theme)` được gọi để chạy cập nhật cấu hình động:

```javascript
function syncChartsTheme(theme) {
  const isDark = theme === "dark";
  const fontColor = isDark ? "#c4c7c9" : "#555770";
  const gridColor = isDark ? "rgba(255, 255, 255, 0.1)" : "rgba(0, 0, 0, 0.08)";

  window.dashboardCharts.forEach((chart) => {
    chart.updateOptions({
      theme: { mode: theme },
      grid: { borderColor: gridColor },
      xaxis: { labels: { style: { colors: fontColor } } },
      yaxis: { labels: { style: { colors: fontColor } } },
    });
  });
}
```

### 6.3. Tích Hợp Sơ Đồ Tổ Chức Cấp Cao (ApexTree)

Sơ đồ cơ cấu tổ chức được vẽ động từ một đối tượng dữ liệu phân tầng (`orgData`). Các node hiển thị được định dạng tùy chỉnh bằng mã HTML lồng ảnh đại diện có hiệu ứng dịch màu xoay sắc (`hue-rotate`):

```javascript
const options = {
  width: "100%",
  height: 500,
  nodeWidth: 180,
  nodeHeight: 65,
  direction: "top",
  contentKey: "data",
  edgeStyle: "curved",
  edgeColor: "var(--color-outline-variant)",
  edgeWidth: 1.5,
  enableZoomPan: true,
  nodeTemplate: (content) => {
    return `
      <div class="org-node" style="border-bottom: 3.5px solid ${content.color};">
        <img src="${content.image}" style="filter: hue-rotate(${content.hueRotate});" alt="${content.name}">
        <div class="org-node-info">
          <div class="org-node-name">${content.name}</div>
          <div class="org-node-role">${content.role}</div>
        </div>
      </div>
    `;
  },
};
const treeInstance = new ApexTree(document.getElementById("treeContainer"), options);
treeInstance.render(orgData);
```

---

## 7. Các Chức Năng Hệ Thống Đồng Bộ Giữa Các Trang

### 7.1. Chuyển Đổi Chế Độ Theme (Dark/Light Mode Sync)

- **Trạng thái kích hoạt**: Nhấp vào nút Moon/Sun trên topbar (hoặc thanh mobile) sẽ kiểm tra sự tồn tại của thuộc tính `data-theme="dark"` trên thẻ `<html>`.
- **Lưu trữ**: Lưu lựa chọn vào `localStorage.setItem("zenOS-theme", "dark" | "light")`.
- **Đồng bộ icon**: Đổi icon từ `fa-moon` sang `fa-sun` và ngược lại trên tất cả các nút tương ứng.
- **Đồng bộ biểu đồ**: Gọi hàm `syncChartsTheme` (trên trang `dashboard.html`).

### 7.2. Đồng Bộ Hóa Bộ Chọn Ngôn Ngữ (Language Switcher Sync)

Khi nhấp chọn một ngôn ngữ ở Topbar chính, bộ chọn ngôn ngữ ở thanh menu phụ mobile cũng tự động nhận trạng thái kích hoạt tương đồng (thêm class `.lang-item-active` vào quốc kỳ/tên hiển thị tương ứng) nhằm đảo bảo tính đồng nhất thông tin.

### 7.3. Trạng Thái Hộp Thông Báo (Notifications Badge & Unread Sync)

- Nhấp vào một thông báo cụ thể sẽ xóa bỏ class `.unread` khỏi thông báo đó.
- Nút "Mark all as read" sẽ xóa class `.unread` của toàn bộ các thông báo.
- Hệ thống tự động tính toán lại số lượng chưa đọc còn lại để cập nhật số hiển thị trên nhãn tròn đỏ (`.notification-badge`) của icon chuông và nhãn phụ tiêu đề của dropdown thông báo. Nếu số lượng bằng 0, nhãn đỏ sẽ được ẩn đi (`display: none;`).

---

## 8. Nguyên Tắc Tạo Hình Ảnh Tài Nguyên (Images Assets Guidelines)

Nếu cần tạo lại thư mục `images/` từ đầu, hãy tuân theo các yêu cầu sau:

- `logo.png`: Logo vector dạng tròn hoặc chữ nghệ thuật có độ tương phản tốt trên cả nền tối và sáng.
- `topic.png`: Ảnh nghệ thuật trừu tượng có chiều ngang lớn (Landscape), sử dụng bảng màu Pastel nhã nhặn phù hợp với chủ đề Spa / Y khoa số hoặc Công nghệ thông tin xanh.
- `avatar.jpg`: Ảnh chân dung đại diện góc nhìn chính diện chất lượng tốt.
- `cn.png`, `jp.png`, `kr.png`, `us.png`, `vn.png`: Các tệp cờ định dạng chữ nhật tỉ lệ 3:2, được bo tròn viền nhẹ bằng CSS khi hiển thị.
