# THIẾT KẾ API - CĂNG TIN TRƯỜNG (Canteen API)

## 1. Quy tắc nghiệp vụ (Business Rules)
* **BR1:** Tên món là duy nhất (không phân biệt hoa thường). Trùng tên trả về mã `409 Conflict`.
* **BR2:** Giá món phải > 0, tối đa 200.000 VNĐ và là bội số của 1.000 VNĐ. Sai quy định trả về mã `422 Unprocessable Entity`.
* **BR3:** Loại món (`category`) chỉ thuộc 1 trong 4 danh mục: `com`, `bun_pho`, `do_uong`, `an_vat`. Sai danh mục trả về mã `422 Unprocessable Entity`.
* **BR4:** Món tạm hết vẫn giữ trong thực đơn, chỉ chuyển trạng thái `available = false` (không xóa khỏi hệ thống).

---

## 2. Bảng 2. Đặc tả tài nguyên MenuItem

| Trường | Kiểu dữ liệu | Ràng buộc | Ví dụ |
| :--- | :--- | :--- | :--- |
| `id` | `int` | Hệ thống tự sinh | `1` |
| `name` | `str` | 2–100 ký tự, duy nhất (BR1) | `Bún cá Hải Phòng` |
| `category` | `enum` | `com` \| `bun_pho` \| `do_uong` \| `an_vat` (BR3) | `bun_pho` |
| `price` | `int` | 0 < price <= 200000, bội số 1000 (BR2) | `30000` |
| `available` | `bool` | Mặc định true (BR4) | `true` |
| `updated_at` | `datetime` | Hệ thống tự cập nhật, chỉ đọc | `2026-10-20T11:30:00` |

---

## 3. Bảng 3. Đặc tả Endpoint

| Phương thức | URI | User Story | Thành công | Mã lỗi |
| :--- | :--- | :--- | :--- | :--- |
| **GET** | `/menu-items` | US1 (có query: category, available, max_price, sort, order) | `200` | `422` |
| **GET** | `/menu-items/{id}` | US2 | `200` | `404, 422` |
| **POST** | `/menu-items` | US3 | `201 + Location` | `409, 422` |
| **PUT** | `/menu-items/{id}` | US4 (toàn bộ) | `200` | `404, 409, 422` |
| **PATCH** | `/menu-items/{id}` | US4 (một phần) | `200` | `404, 409, 422` |
| **DELETE** | `/menu-items/{id}` | US5 | `204` | `404` |