# 1.Thiết kế API quản lý thư viện

## 1.1. Lấy danh sách tất cả sách

* **HTTP Method:** `GET`
* **URL:** `/books`
* **Mô tả:** Trả về danh sách toàn bộ sách trong thư viện.

### Response thành công

* **Status Code:** `200 OK`

```json
[
  {
    "id": 1,
    "title": "Clean Code",
    "author": "Robert C. Martin",
    "year": 2008,
    "available": true
  },
  {
    "id": 2,
    "title": "Java Basics",
    "author": "James Gosling",
    "year": 2015,
    "available": false
  }
]
```

### Các lỗi có thể xảy ra

* `500 Internal Server Error`

    * Lỗi server khi truy xuất dữ liệu.
* `503 Service Unavailable`

    * Hệ thống database tạm thời không hoạt động.

---

## 1.2. Lấy chi tiết sách theo ID

* **HTTP Method:** `GET`
* **URL:** `/books/{id}`
* **Ví dụ:** `/books/1`

### Response thành công

* **Status Code:** `200 OK`

```json
{
  "id": 1,
  "title": "Clean Code",
  "author": "Robert C. Martin",
  "year": 2008,
  "available": true
}
```

### Các lỗi có thể xảy ra

* `404 Not Found`

    * Không tìm thấy sách với id tương ứng.
* `400 Bad Request`

    * ID không đúng định dạng (ví dụ nhập chữ thay vì số).

---

## 1.3. Thêm sách mới

* **HTTP Method:** `POST`
* **URL:** `/books`

### Request Body

```json
{
  "title": "Spring Boot Guide",
  "author": "John Smith",
  "year": 2024,
  "available": true
}
```

### Response thành công

* **Status Code:** `201 Created`

```json
{
  "id": 3,
  "title": "Spring Boot Guide",
  "author": "John Smith",
  "year": 2024,
  "available": true
}
```

### Các lỗi có thể xảy ra

* `400 Bad Request`

    * Thiếu field bắt buộc như `title` hoặc `author`.
* `409 Conflict`

    * Sách đã tồn tại trong hệ thống.

---

## 1.4. Cập nhật toàn bộ thông tin sách theo ID

* **HTTP Method:** `PUT`
* **URL:** `/books/{id}`

### Request Body

```json
{
  "title": "Spring Boot Advanced",
  "author": "John Smith",
  "year": 2025,
  "available": false
}
```

### Response thành công

* **Status Code:** `200 OK`

```json
{
  "id": 3,
  "title": "Spring Boot Advanced",
  "author": "John Smith",
  "year": 2025,
  "available": false
}
```

### Các lỗi có thể xảy ra

* `404 Not Found`

    * Không tìm thấy sách cần cập nhật.
* `400 Bad Request`

    * Dữ liệu gửi lên không hợp lệ.

---

## 1.5. Xóa sách theo ID

* **HTTP Method:** `DELETE`
* **URL:** `/books/{id}`

### Response thành công

* **Status Code:** `200 OK`

```json
{
  "message": "Book deleted successfully"
}
```

### Các lỗi có thể xảy ra

* `404 Not Found`

    * Không tồn tại sách cần xóa.
* `400 Bad Request`

    * ID không hợp lệ.

---

## 1.6. Tìm sách theo tác giả

* **HTTP Method:** `GET`
* **URL:** `/books?author={authorName}`

### Ví dụ

```bash
GET /books?author=Robert C. Martin
```

### Response thành công

* **Status Code:** `200 OK`

```json
[
  {
    "id": 1,
    "title": "Clean Code",
    "author": "Robert C. Martin",
    "year": 2008,
    "available": true
  }
]
```

### Các lỗi có thể xảy ra

* `400 Bad Request`

    * Query parameter `author` bị thiếu hoặc sai định dạng.
* `500 Internal Server Error`

    * Lỗi xử lý dữ liệu phía server.

---

# Giải thích chọn HTTP Method

| Chức năng             | Method | Lý do                                            |
|-----------------------|--------|--------------------------------------------------|
| Lấy danh sách sách    | GET    | Chỉ đọc dữ liệu, không thay đổi dữ liệu          |
| Lấy chi tiết sách     | GET    | Truy xuất dữ liệu theo id                        |
| Thêm sách mới         | POST   | Tạo mới tài nguyên                               |
| Cập nhật toàn bộ sách | PUT    | Thay thế toàn bộ dữ liệu sách                    |
| Xóa sách              | DELETE | Xóa tài nguyên                                   |
| Tìm theo tác giả      | GET    | Chỉ lọc và truy vấn dữ liệu bằng query parameter |

# 2. Ví dụ Request/Response JSON

## 2.1 Thêm một sách mới thành công

### Request

* **Method:** `POST`
* **URL:** `/books`

### Request Body

```json id="k1s9de"
{
  "title": "Spring Boot Guide",
  "author": "John Smith",
  "year": 2024,
  "available": true
}
```

### Response

* **Status Code:** `201 Created`

```json id="r7plxa"
{
  "id": 3,
  "title": "Spring Boot Guide",
  "author": "John Smith",
  "year": 2024,
  "available": true
}
```

---

## 2.2 Tìm sách theo tác giả không có kết quả

### Request

* **Method:** `GET`
* **URL:** `/books?author=Unknown Author`

### Response

* **Status Code:** `200 OK`

```json id="m5yqwv"
[]
```

### Giải thích

* API vẫn xử lý thành công request nên trả về `200 OK`.
* Mảng rỗng `[]` nghĩa là không tìm thấy sách nào của tác giả được yêu cầu.
