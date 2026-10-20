### 1. Tổng quan hệ thống

- Chức năng chính của hệ thống
- Đối tượng sử dụng (end-user, admin, hệ thống khác…)
- Phạm vi hệ thống (in-scope / out-of-scope)

### 2. Sơ đồ kiến trúc logic

- **Sơ đồ** tổng thể các thành phần logic và mối quan hệ giữa chúng
- Mô tả ngắn từng layer / domain / service chính

### 3. Các luồng nghiệp vụ chính

- Flow diagram
- Sequence diagram (các luồng quan trọng)

### 4. Danh sách các thành phần chính

| Thành phần | Chức năng chính | Tech stack | Sizing (ước lượng) | Repository |
| ---------- | --------------- | ---------- | ------------------ | ---------- |
| ...        | ...             | ...        | ...                | ...        |

### 5. Các thành phần phụ thuộc

| Thành phần | Loại (Internal/External) | Chức năng | Cách kết nối | Authen/Author | Fallback khi down |
| ---------- | ------------------------ | --------- | ------------ | ------------- | ----------------- |
| ...        | ...                      | ...       | ...          | ...           | ...               |

### 6. Sơ đồ kiến trúc deployment thực tế

- **Sơ đồ triển khai**
- Các config quan trọng
- Môi trường (Dev / Staging / Production nếu khác nhau)

### 7. Observability & Monitoring

- Các metric khác nếu có (ngoài các metric mặc định theo stack spring/grafana/elk..)
- Troubleshoot, known issues nếu có
