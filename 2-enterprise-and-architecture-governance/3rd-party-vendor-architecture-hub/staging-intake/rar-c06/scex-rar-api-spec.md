# Đặc tả API — Tích hợp SCEX $\leftrightarrow$ RAR / C06 (VNeID)

| Trường           | Giá trị                                              |
| ---------------- | ---------------------------------------------------- |
| Phiên bản        | `v1`                                                 |
| Trạng thái       | Draft                                                |
| Cập nhật         | 2026-08-12                                           |
| Người viết       | <a>duc.mm</a>                                        |
| Người review     | `<điền>`                                             |
| Phạm vi áp dụng  | UAT và Production — tích hợp VNeID qua Agent Gateway |
| Tài liệu cha     | `scex-rar.md`, mục 3.1 Catalog API & Endpoint        |
| Nguồn tham chiếu | `tcdn_spec_api.md` (v3.1, 15/8/2025), `rar_sso.md`   |

### Lịch sử phiên bản

| Phiên bản | Ngày       | Người sửa | Nội dung                                                                                                                                     |
| --------- | ---------- | --------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| `v1`      | 2026-08-12 | `duc.mm`    | Bản draft đầu tiên: chốt contract nhóm A theo `tcdn_spec_api.md` v3.1; định nghĩa nhóm B/C phía SCEX; tổng hợp open issue cần làm rõ với C06 |

---

## 0. Phạm vi, quy ước và giả định

Tài liệu này đặc tả interface tích hợp SCEX với RAR/C06 (VNeID). Contract nhóm A được chốt theo `tcdn_spec_api.md` v3.1 (15/8/2025); mọi sai khác so với nguồn đó phải được C06 xác nhận bằng văn bản trước khi áp dụng. Nhóm B giữ nguyên tên field và enum của C06, nhưng host/path do SCEX định nghĩa và đăng ký trước. Nhóm C là API nội bộ, SCEX tự quyết.

### 0.1 Phân nhóm theo bên xây dựng

| Nhóm  | Mục           | Bên xây dựng  | Chiều gọi                    | Nguồn sự thật của contract                                                                        |
| ----- | ------------- | ------------- | ---------------------------- | ------------------------------------------------------------------------------------------------- |
| **A** | 3.2.1–3.2.9   | C06 / NT ĐDXT | SCEX Backend → Agent Gateway | `tcdn_spec_api.md`, `rar_sso.md`. SCEX không được tự đổi tên field, định dạng, hay thêm/bớt field |
| **B** | 3.2.10–3.2.14 | SCEX          | C06 → SCEX Backend           | Tên field bám `tcdn_spec_api.md`; host/path do SCEX định nghĩa và đăng ký với C06                 |
| **C** | 3.2.15–3.2.23 | SCEX          | SCEX App/Web → SCEX Backend  | SCEX tự định nghĩa hoàn toàn                                                                      |

### 0.2 Quy ước từ khoá

Trong toàn bộ tài liệu:

| Từ khoá                   | Ý nghĩa                                                               |
| ------------------------- | --------------------------------------------------------------------- |
| **PHẢI** / **KHÔNG ĐƯỢC** | Ràng buộc bắt buộc. Sai là hỏng tích hợp hoặc vi phạm bảo mật         |
| **NÊN**                   | Khuyến nghị của SCEX. Được phép làm khác nếu có lý do và ghi nhận lại |
| **CÓ THỂ**                | Tuỳ chọn triển khai                                                   |

Các vấn đề chưa chốt với C06 được đánh mã `OI-xx` tại chỗ và tổng hợp ở Phụ lục cuối tài liệu. Không triển khai phần phụ thuộc vào open issue ở mức Blocker hoặc Cao cho đến khi có xác nhận.

### 0.3 Ký hiệu

- `{{PLACEHOLDER}}`: giá trị cấu hình theo môi trường (mục 3.2.0).
- `{service-code}`: mã dịch vụ (bảng mã dịch vụ mục 3.2.0.7).

### 0.4 Giả định đang treo

Các giả định dưới đây ảnh hưởng trực tiếp tới thiết kế. Nếu C06 xác nhận khác, phải cập nhật lại tài liệu trước khi go-live.

| #   | Giả định                                                                                                           | Open issue liên quan |
| --- | ------------------------------------------------------------------------------------------------------------------ | -------------------- |
| 1   | Môi trường UAT dùng Agent Gateway giống Production                                                                 | OI-13                |
| 2   | `verify-info` dùng `get-transaction` v1 hay v2 phụ thuộc cấu hình C06 cấp cho SCEX; chưa xác định                  | OI-14                |
| 3   | Đặc tả SDK TTXT (chức năng, nền tảng, cách tích hợp) sẽ được C06 cung cấp trước khi implement sinh trắc học        | OI-10                |
| 4   | Callback đăng xuất từ VNeID có thể không mang `state`; SCEX vẫn xoá phiên nội bộ                                   | OI-08, OI-15         |
| 5   | Cơ chế xác thực chiều C06 → SCEX chỉ có `api_key`; SCEX bù bằng allowlist IP, chống replay, và mTLS ở tầng hạ tầng | OI-02                |

---

## 3.2.0 Thông tin cấu hình kết nối

### 3.2.0.1 Tham số theo môi trường

Bảng dưới đây là tham số cần điền trước khi triển khai. Cột "Nguồn" cho biết ai cung cấp giá trị.

| Placeholder                    | Ý nghĩa                                                         | UAT                                                                      | Production | Nguồn                                 |
| ------------------------------ | --------------------------------------------------------------- | ------------------------------------------------------------------------ | ---------- | ------------------------------------- |
| `{{SCEX_API_ROOT}}`            | Root domain API Backend của SCEX                                | `<điền>`                                                                 | `<điền>`   | SCEX                                  |
| `{{SCEX_WEB_ROOT}}`            | Root domain Web App của SCEX                                    | `<điền>`                                                                 | `<điền>`   | SCEX                                  |
| `{{SCEX_UNIVERSAL_LINK}}`      | Universal Link của SCEX Mobile App                              | `<điền>`                                                                 | `<điền>`   | SCEX                                  |
| `{{SCEX_REDIRECT_URI}}`        | Redirect URI nhận `code` sau đăng nhập SSO (mục 3.2.10)         | `<điền>`                                                                 | `<điền>`   | SCEX                                  |
| `{{SCEX_LOGOUT_REDIRECT_URI}}` | Redirect URI sau đăng xuất VNeID (mục 3.2.11)                   | `<điền>`                                                                 | `<điền>`   | SCEX                                  |
| `{{AGENT_GW_DOMAIN}}`          | Domain Agent Gateway (đặt tại hạ tầng SCEX)                     | `<điền>`                                                                 | `<điền>`   | C06 / SCEX                            |
| `{{AGENT_GW_PORT}}`            | Port Agent Gateway                                              | `<điền>`                                                                 | `<điền>`   | C06 / SCEX                            |
| `{{URL_SSO}}`                  | URL trang đăng nhập VNeID (Web)                                 | `https://sso-uatvneid.dancuquocgia.com`                                  | `<điền>`   | C06                                   |
| `{{URL_LOGOUT}}`               | URL đăng xuất VNeID (Web)                                       | `https://sso-uatvneid.dancuquocgia.com/api/idp/sso/login-actions/logout` | `<điền>`   | C06                                   |
| `{{UNIVERSAL_LINK_SSO}}`       | Universal Link đăng nhập VNeID (App)                            | `https://universal.dancuquocgia.com/login-sso`                           | `<điền>`   | C06                                   |
| `{{UNIVERSAL_LINK_SHARE}}`     | Universal Link consent chia sẻ (App)                            | `https://universal.dancuquocgia.com/share`                               | `N/A`      | C06                                   |
| `{{CLIENT_ID}}`                | Mã đăng ký của SCEX trên hệ thống SSO VNeID                     | `<điền>`                                                                 | `<điền>`   | C06 cấp                               |
| `{{CLIENT_SECRET}}`            | Secret key SSO của SCEX                                         | `<điền>`                                                                 | `<điền>`   | C06 cấp                               |
| `{{API_KEY}}`                  | API key gọi qua Agent Gateway                                   | `<điền>`                                                                 | `<điền>`   | C06 cấp                               |
| `{{TARGET_CODE}}`              | Mã mục đích chia sẻ / xử lý thông tin                           | `<điền>`                                                                 | `<điền>`   | C06 cấp                               |
| `{{ORG_CODE}}`                 | Mã TCDN của SCEX do Nền tảng cấp                                | `<điền>`                                                                 | `<điền>`   | C06 cấp                               |
| `{{ORG_CONNECTION_CODE}}`      | Mã đơn vị kết nối — query param `code`, chỉ môi trường non-prod | `<điền>`                                                                 | `N/A`      | C06 cấp                               |
| `{{RSA_PRIVATE_KEY}}`          | Private key RSA của SCEX để giải mã `encrypt_key` / `key`       | `<điền>`                                                                 | `<điền>`   | SCEX sinh, đăng ký public key với C06 |
| `{{C06_JWS_PUBLIC_KEY}}`       | Public key C06 để verify chữ ký JWS của `userInfo`              | `<điền>`                                                                 | `<điền>`   | C06 cấp                               |

**Chưa chốt (OI-01):** danh mục giá trị `{{TARGET_CODE}}` chưa có trong tài liệu C06. Xem `scex-rar.md` Appendix A. C06 phải cung cấp danh mục trước khi implement.

### 3.2.0.2 Đường link SCEX phải công bố cho VNeID

Tài liệu C06 yêu cầu TCDN cung cấp 2 đường link để hiển thị trên ứng dụng VNeID (mục III.2.1 và III.2.2):

| STT | Nội dung công bố                                                                                                      | URL      | Nguồn |
| --- | --------------------------------------------------------------------------------------------------------------------- | -------- | ----- |
| 1   | "Điều khoản và cam kết sử dụng dữ liệu được chia sẻ"                                                                  | `<điền>` | SCEX  |
| 2   | "Công bố thông tin rủi ro khi công dân rút lại sự đồng ý chia sẻ dữ liệu, tổ chức doanh nghiệp thực hiện xoá dữ liệu" | `<điền>` | SCEX  |

### 3.2.0.3 Header dùng chung khi gọi qua Agent Gateway (áp dụng nhóm A)

| Key            | Type     | Required | Description                                                                                                  |
| -------------- | -------- | :------: | ------------------------------------------------------------------------------------------------------------ |
| `Content-Type` | `String` | **Yes**  | `application/json; charset=utf-8` (riêng mục 3.2.2 dùng `application/x-www-form-urlencoded`)                 |
| `api_key`      | `String` | **Yes**  | `{{API_KEY}}` — do Tổ chức cung cấp dịch vụ cấp                                                              |
| `target_code`  | `String` | **Yes**  | `{{TARGET_CODE}}` — mã mục đích chia sẻ / xử lý thông tin, dùng để VNeID hiển thị đúng nội dung cho công dân |
| `request_id`   | `String` | **Yes**  | Mã yêu cầu do SCEX tự sinh, **duy nhất trên toàn hệ thống** (khuyến nghị UUID v4)                            |
| `request_time` | `String` | **Yes**  | Thời gian yêu cầu, định dạng `ddMMyyyyHHmmss`                                                                |

**Ràng buộc:** nếu `request_time` lệch quá 2 phút so với thời gian hệ thống Nền tảng, request bị coi là timeout. SCEX PHẢI đồng bộ NTP trên toàn bộ node gọi ra Agent Gateway.

### 3.2.0.4 Định dạng phản hồi dùng chung

**Nhóm A — envelope của C06** (áp dụng cho các API `init`, `get-transaction`, `receive-result`):

```json
{
  "responseCode": "0",
  "description": "Success!",
  "responseTime": "12082026091500",
  "data": {}
}
```

- `responseCode = "0"` là thành công; khác `0` là lỗi, xem bảng mục 3.2.0.6.
- `responseTime` định dạng `ddMMyyyyHHmmss`.

**Nhóm A — envelope dữ liệu đã mã hoá** (áp dụng `sso-token`, `sso-userinfo`):

```json
{
  "data": "<chuỗi đã mã hoá AES>",
  "key": "<AES secret key đã mã hoá RSA bằng public key của SCEX>"
}
```

- Nếu `key` có giá trị: `data` đã mã hoá AES. SCEX PHẢI giải mã `key` bằng `{{RSA_PRIVATE_KEY}}` rồi giải mã `data`.
- Nếu `key` không có giá trị hoặc không tồn tại: `data` trả về dạng plaintext.

**Nhóm B / Nhóm C — envelope của SCEX:**

Success:

```json
{
  "code": 200,
  "message": "Success",
  "data": {}
}
```

Error:

```json
{
  "code": 400,
  "error": "Validation Failed",
  "details": ["citizenPid must be 12 numeric characters"],
  "request_id": "req_abc123"
}
```

Ngoại lệ: các endpoint nhóm B do C06 gọi vào KHÔNG ĐƯỢC dùng envelope của SCEX. PHẢI trả đúng cấu trúc mà tài liệu C06 quy định (`statusCode` / `requestStatus` / `errorDetail`). Xem chi tiết từng mục.

### 3.2.0.5 Mã lỗi HTTP dùng chung (tham chiếu `tcdn_spec_api.md` mục IV.1)

| HTTP Status | Kiểu lỗi                | Nội dung                                                           |
| ----------- | ----------------------- | ------------------------------------------------------------------ |
| `200`       | `SUCCESS`               | Yêu cầu thực hiện thành công                                       |
| `401`       | `ACCESS_TOKEN`          | Token hết hạn (nếu có)                                             |
| `403`       | `FORBIDDEN`             | Không có quyền truy cập dịch vụ                                    |
| `404`       | `NOT_FOUND`             | Địa chỉ yêu cầu không tồn tại                                      |
| `500`       | `INTERNAL SERVER ERROR` | Lỗi trong quá trình xử lý — hệ thống chủ động đưa ra lỗi chi tiết  |
| `501`       | `BIOMETRIC ERROR`       | Lỗi xác thực sinh trắc học — hệ thống chủ động đưa ra lỗi chi tiết |

Mã lỗi dịch vụ (mục IV.2): `1000` lỗi trong quá trình xử lý · `1001` yêu cầu trùng lặp · `1002` sai định dạng dữ liệu.

### 3.2.0.6 Mã `responseCode` / `resultCode` (tham chiếu mục IV.4)

| Code  | Ý nghĩa                                       | SCEX xử lý                                                                                                         |
| ----- | --------------------------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| `2`   | Công dân không đồng ý chia sẻ                 | Kết thúc luồng, thông báo cho user; trả về qua API callback (mục 3.2.13)                                           |
| `1`   | Công dân đồng ý chia sẻ                       | Tiếp tục gọi `get-transaction`                                                                                     |
| `0`   | Thành công                                    | —                                                                                                                  |
| `-1`  | Không lấy được thông tin                      | Giao dịch lỗi, khởi tạo lại                                                                                        |
| `-2`  | Giao dịch đã tồn tại                          | Lỗi nội bộ NT ↔ VNeID. SCEX không cần xử lý                                                                        |
| `-3`  | Giao dịch đã hoàn thành                       | Lỗi nội bộ NT ↔ VNeID. SCEX không cần xử lý                                                                        |
| `-4`  | Giao dịch đang xử lý, thử lại sau             | Đang chờ công dân xác nhận. Chờ callback hoặc hết thời gian rồi thực hiện lại                                      |
| `-5`  | Đầu vào yêu cầu không hợp lệ                  | Kiểm tra lại request body                                                                                          |
| `-6`  | Không tìm thấy tổ chức                        | Liên hệ C06 kiểm tra cấu hình tổ chức trước khi gọi tiếp                                                           |
| `-7`  | Không tìm thấy dịch vụ                        | Liên hệ C06 kiểm tra cấu hình dịch vụ                                                                              |
| `-8`  | Đường dẫn không hợp lệ                        | Kiểm tra lại URL                                                                                                   |
| `-9`  | Không tìm thấy mã giao dịch                   | Kiểm tra lại `transactionId` / `consentId`                                                                         |
| `-10` | Giao dịch đã hoàn tất xác nhận chia sẻ        | Lỗi nội bộ NT ↔ VNeID. SCEX không cần xử lý                                                                        |
| `-11` | Công dân không đồng ý chia sẻ thông tin       | Tương tự mã `2` nhưng trả trong response của `get-transaction`. PHẢI khởi tạo giao dịch mới                        |
| `-12` | Không tìm thấy cấu hình agent                 | Liên hệ C06 kiểm tra cấu hình Agent Gateway                                                                        |
| `-14` | Timeout                                       | Hết thời gian chờ xác nhận / trả kết quả. Khởi tạo giao dịch mới                                                   |
| `-15` | Tồn tại trường dữ liệu bắt buộc bị `null`     | KHÔNG ĐƯỢC khởi tạo lại cùng dịch vụ + cùng công dân. Hướng dẫn công dân kiểm tra thông tin                        |
| `-16` | Dịch vụ vượt quá hạn mức cho phép             | Liên hệ C06 gia hạn dịch vụ                                                                                        |
| `-17` | Thông tin công dân không hợp lệ               | Tài khoản mức 1 / không tồn tại / CCCD hết hạn. Giao dịch hết hiệu lực ngay. Không khởi tạo lại với cùng thông tin |
| `-18` | Trường đầu vào không tồn tại trong cấu hình   | Đối chiếu lại các field với dịch vụ đã đăng ký                                                                     |
| `-19` | Tài khoản tổ chức không có quyền dùng dịch vụ | Kiểm tra lại thông tin tài khoản tổ chức                                                                           |
| `-20` | Thông tin công dân không đúng                 | Dữ liệu gửi lên không khớp CSDL. Thử lại chỉ với các field bắt buộc                                                |

Trạng thái chia sẻ (callback mục 3.2.13): `INIT` — công dân đã nhận màn xác nhận chia sẻ trên VNeID; `SHARED` — đã chia sẻ dữ liệu; `NOT SHARED` — không thành công.

### 3.2.0.7 Ma trận service-code × endpoint

Chỉ liệt kê các service-code trong phạm vi của SCEX (xem `scex-rar.md` mục 1.3).

| Service-code              | Khởi tạo        | Lấy kết quả                          | Trả kết quả       | Ghi chú                                     |
| ------------------------- | --------------- | ------------------------------------ | ----------------- | ------------------------------------------- |
| `sso-token`               | —               | 3.2.2                                | —                 | Không dùng `init`/`get-transaction`         |
| `sso-userinfo`            | —               | 3.2.3                                | —                 | Cần `access_token` từ `sso-token`           |
| `share-info`              | 3.2.5           | **3.2.6** (v1)                       | 3.2.8             | Không có xác thực ảnh mặt                   |
| `verify-info`             | 3.2.5           | **3.2.6** (v1) _hoặc_ **3.2.7** (v2) | 3.2.8             | Phụ thuộc cấu hình dịch vụ — xem lưu ý dưới |
| `face-matching`           | 3.2.5           | **3.2.7** (v2)                       | 3.2.8             | Có xác thực ảnh mặt                         |
| `biometric-share-info`    | 3.2.5           | **3.2.7** (v2)                       | 3.2.8             | Có xác thực ảnh mặt                         |
| `withdraw-consent`        | — (C06 gọi vào) | —                                    | 3.2.14 (response) | Endpoint do SCEX xây dựng                   |
| `withdraw-consent-result` | —               | —                                    | 3.2.9             | Chỉ dùng khi 3.2.14 trả `PROCESSING`        |

**Nguyên tắc chọn v1 hay v2.** Yếu tố quyết định là dịch vụ có xác thực ảnh mặt hay không, không phải bản thân service-code:

- Dịch vụ không bao gồm xác thực ảnh mặt: dùng đường dẫn không có `/v2/` (mục 3.2.6, `tcdn_spec_api.md` III.2.3).
- Dịch vụ có bao gồm xác thực ảnh mặt: dùng đường dẫn có `/v2/` (mục 3.2.7, `tcdn_spec_api.md` III.2.4).
- Khởi tạo giao dịch bằng service-code nào thì lấy kết quả theo đúng service-code đó.

`verify-info` xuất hiện ở cả hai mục III.2.3 và III.2.4 của tài liệu C06 (mục III.2.4 có biến thể request body kèm `faceImage`). Do đó `verify-info` dùng v1 hay v2 phụ thuộc vào việc C06 có cấu hình xác thực ảnh mặt cho dịch vụ này của SCEX hay không.

**Chưa chốt (OI-14):** cấu hình dịch vụ cấp cho SCEX chưa xác định service-code nào bật xác thực ảnh mặt, cụ thể `verify-info` đi v1 hay v2. Gọi sai đường dẫn sẽ nhận `responseCode = -8`.

---

# NHÓM A — API do RAR / C06 cung cấp

SCEX Backend là client. Toàn bộ lời gọi đi qua Agent Gateway đặt tại hạ tầng SCEX; SCEX Backend không gọi trực tiếp NT ĐDXT / TTXT.

**Base URL:** `https://{{AGENT_GW_DOMAIN}}:{{AGENT_GW_PORT}}`

Contract của nhóm này do C06 sở hữu. SCEX KHÔNG ĐƯỢC đổi tên field, đổi định dạng, hay thêm/bớt field. Mọi sai khác so với `tcdn_spec_api.md` phải được C06 xác nhận bằng văn bản.

Trừ khi ghi chú khác, các API qua Agent Gateway dùng bộ header chung mục 3.2.0.3. Phần Headers của từng mục chỉ nêu điểm khác biệt.

---

## 3.2.1 URL điều hướng đăng nhập sang VNeID (SSO — Web & App)

`GET` `{{URL_SSO}}/auth` (Web) · `{{UNIVERSAL_LINK_SSO}}/{{CLIENT_ID}}` (App)

**Mô tả:** VNeID cung cấp URL này để SCEX điều hướng người dùng sang trang/ứng dụng đăng nhập. Không phải API REST mà là URL redirect (Web) hoặc universal link (App).

**Hệ thống xây dựng:** C06 / Hệ thống Định danh và xác thực điện tử (SCEX là bên gọi)

**Tham chiếu:** `tcdn_spec_api.md` mục III.3.1 · `rar_sso.md` mục 4.1

**Authentication:** Không áp dụng (public redirect)

#### Cấu trúc

```
WEB: {{URL_SSO}}/auth?response_type=code&client_id={{CLIENT_ID}}&scope=openid&redirect_uri={{SCEX_REDIRECT_URI}}&state=$[state]

APP: {{UNIVERSAL_LINK_SSO}}/{{CLIENT_ID}}?state=$[state]
```

#### Path / Query Parameters

| Field           | Type     | Required | Description                                                                                             |
| :-------------- | :------- | :------: | :------------------------------------------------------------------------------------------------------ |
| `response_type` | `String` | **Yes**  | Giá trị mặc định `code`                                                                                 |
| `client_id`     | `String` | **Yes**  | `{{CLIENT_ID}}` — mã đăng ký của SCEX                                                                   |
| `scope`         | `String` | **Yes**  | Giá trị mặc định `openid`                                                                               |
| `redirect_uri`  | `String` | **Yes**  | `{{SCEX_REDIRECT_URI}}`. PHẢI nằm trong danh sách SCEX đã đăng ký để C06 cấu hình trước                 |
| `state`         | `String` |   No\*   | Tham số SCEX tự định nghĩa; VNeID trả lại nguyên giá trị ở `redirect_uri`. SCEX PHẢI dùng để chống CSRF |

\* C06 không bắt buộc, nhưng SCEX bắt buộc theo yêu cầu bảo mật nội bộ (xem Quyết định của SCEX).

#### Responses

**`302 Found`** — Điều hướng người dùng sang trang/app đăng nhập VNeID. Sau khi công dân xác thực và đồng ý chia sẻ, VNeID điều hướng về `{{SCEX_REDIRECT_URI}}` kèm `code` (xem mục 3.2.10).

Nếu công dân không đồng ý chia sẻ, VNeID hiển thị thông báo đăng nhập không thành công và không điều hướng kèm `code`.

#### Ràng buộc từ C06

- TCDN CÓ THỂ định nghĩa thêm tham số tuỳ ý trong URL; VNeID trả lại giá trị tương ứng tại `redirect_uri`. SCEX dùng cơ chế này để mang `state`.
- `redirect_uri` KHÔNG ĐƯỢC dùng địa chỉ IP hoặc `localhost`.
- Luồng App dùng universal link, không có tham số `response_type` / `scope` / `redirect_uri` trong đường dẫn. Callback được cấu hình sẵn phía C06.

#### Quyết định của SCEX

- `state` PHẢI sinh bằng `SecureRandom` (không random từ tệp số có sẵn hoặc từ datetime), độ dài tối thiểu 16 byte, TTL 300 giây, chỉ dùng 1 lần. Tham chiếu `scex-rar.md` mục 2.1.

**Chưa chốt (OI-16):** hành vi và mã lỗi khi công dân từ chối đăng nhập SSO, `code` hết hạn/không hợp lệ, hoặc `client_id`/`redirect_uri` sai chưa được C06 mô tả đầy đủ.

#### Example cURL

```bash
# Web — chỉ minh hoạ cấu trúc URL, thực tế là redirect trên trình duyệt
curl -i "https://sso-uatvneid.dancuquocgia.com/auth?response_type=code&client_id={{CLIENT_ID}}&scope=openid&redirect_uri=https%3A%2F%2F{{SCEX_API_ROOT}}%2Fapi%2Fv1%2Fauth%2Fvneid%2Fcallback&state=8f14e45fceea167a5a36dedd4bea2543"
```

---

## 3.2.2 API lấy access_token (`sso-token`)

`POST` `https://{{AGENT_GW_DOMAIN}}:{{AGENT_GW_PORT}}/api/tcdn/sso-token/api/vneid/form/sso`

**Mô tả:** Đổi `code` nhận được ở redirect URI (mục 3.2.10) lấy `access_token` của VNeID. Đường dẫn qua Agent Gateway dành cho tổ chức doanh nghiệp.

**Hệ thống xây dựng:** C06 / Nền tảng định danh (SCEX là client)

**Tham chiếu:** `tcdn_spec_api.md` mục III.3.3 · `rar_sso.md` mục 4.3

**Authentication:** `api_key` + `client_id` / `client_secret` trong body

`rar_sso.md` mục 4.3 ghi endpoint trực tiếp của VNeID là `api/idp/sso/auth/token`. Endpoint đó chỉ áp dụng cho Bộ/ban/ngành gọi qua NDXP/VDXP. SCEX là tổ chức doanh nghiệp nên PHẢI dùng đường dẫn qua Agent Gateway ở trên (`tcdn_spec_api.md` mục III.3.3).

#### Headers

Bộ header chung mục 3.2.0.3, với các điểm khác:

| Key            | Type     | Required | Description                                              |
| :------------- | :------- | :------: | :------------------------------------------------------- |
| `accept`       | `String` | **Yes**  | `application/json, text/plain, */*`                      |
| `content-type` | `String` | **Yes**  | `application/x-www-form-urlencoded` (khác mặc định JSON) |
| `api_key`      | `String` | **Yes**  | `{{API_KEY}}`                                            |
| `target_code`  | `String` | **Yes**  | `{{TARGET_CODE}}`                                        |
| `request_id`   | `String` | **Yes**  | Mã yêu cầu do SCEX sinh, duy nhất trên hệ thống          |
| `request_time` | `String` | **Yes**  | `ddMMyyyyHHmmss`                                         |

#### Path / Query Parameters

| Field  | Type     |     Required     | Description                                                           |
| :----- | :------- | :--------------: | :-------------------------------------------------------------------- |
| `code` | `String` | Chỉ **non-prod** | `{{ORG_CONNECTION_CODE}}` — mã đơn vị kết nối. Môi trường prod: `N/A` |

#### Request Body

Định dạng `application/x-www-form-urlencoded`.

| Field           | Type     | Required | Description                                         | Constraints                                   |
| :-------------- | :------- | :------: | :-------------------------------------------------- | :-------------------------------------------- |
| `code`          | `String` | **Yes**  | Code xác thực nhận được ở redirect URI (mục 3.2.10) | Dùng 1 lần                                    |
| `grant_type`    | `String` | **Yes**  | Kiểu xác thực                                       | Giá trị cố định `authorization_code`          |
| `client_id`     | `String` | **Yes**  | `{{CLIENT_ID}}`                                     | —                                             |
| `client_secret` | `String` | **Yes**  | `{{CLIENT_SECRET}}`                                 | Chỉ lưu ở server-side, không đưa xuống client |
| `redirect_uri`  | `String` |    No    | `{{SCEX_REDIRECT_URI}}`                             | KHÔNG ĐƯỢC dùng địa chỉ IP hoặc `localhost`   |

**Example Request:**

```
code=a0c106c9-26ae-450e-89e7-cc9900f95428.574c56d0-f64a-463f-9286-318d2c3200df
&grant_type=authorization_code
&client_id={{CLIENT_ID}}
&client_secret={{CLIENT_SECRET}}
&redirect_uri={{SCEX_REDIRECT_URI}}
```

#### Responses

**`200 OK`** — Trả về dữ liệu đã mã hoá

```json
{
  "data": "9sCIPPKfkuHJyZbq4nF4Z70A6Tm9ul6wd8oBsaTZr3qa18lcw8XzoXYYPcpWGj4y...",
  "key": "OJkjyfkxJ8mIYCGsyvihdEYVJr+UaSBO8ccyL2KiuYESqamq+0VWFlR6zqzvCq0V..."
}
```

**Sau khi giải mã `data`:**

```json
{
  "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldU",
  "expires_in": 3600,
  "refresh_expires_in": 1800,
  "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICIzMz",
  "token_type": "Bearer",
  "id_token": "Dr4wTq9W07d7k3iK5T6J232CNXFngIhEXojH8nJV2jfiI8NiXQ",
  "not-before-policy": 1667019564,
  "session_state": "456b0443-8b89-4b4f-810d-fc5e2a04c162",
  "scope": "openid email profile"
}
```

| Field                | Type     | Required | Description                            |
| :------------------- | :------- | :------: | :------------------------------------- |
| `access_token`       | `String` | **Yes**  | Mã xác thực phiên đăng nhập            |
| `expires_in`         | `Number` | **Yes**  | Thời hạn phiên đăng nhập (giây)        |
| `refresh_expires_in` | `Number` | **Yes**  | Thời hạn làm mới phiên (giây)          |
| `refresh_token`      | `String` | **Yes**  | Mã làm mới phiên đăng nhập             |
| `token_type`         | `String` | **Yes**  | `Bearer`                               |
| `id_token`           | `String` | **Yes**  | Id của mã xác thực                     |
| `not-before-policy`  | `String` |    No    | Thông tin trả thêm. SCEX không sử dụng |

**`401 Unauthorized`** / **`403 Forbidden`** / **`500 Internal Server Error`** — xem mục 3.2.0.5.

#### Ràng buộc từ C06

- `code` chỉ dùng được một lần; gọi lại với cùng `code` sẽ thất bại.
- Nếu response có `key`, PHẢI giải mã `key` bằng `{{RSA_PRIVATE_KEY}}` (RSA) để lấy AES secret, rồi giải mã `data` (AES).

#### Quyết định của SCEX

- `client_secret` là bí mật server-side. KHÔNG ĐƯỢC lộ ra Mobile App hoặc Web client.
- `access_token` của VNeID KHÔNG ĐƯỢC trả về client của SCEX; chỉ giữ ở Backend (xem mục 3.2.16).
- `request_id` PHẢI được lưu lại để đối soát và truy vết log (xem `scex-rar.md` mục 6.2).

#### Example cURL

```bash
curl --location 'https://{{AGENT_GW_DOMAIN}}:{{AGENT_GW_PORT}}/api/tcdn/sso-token/api/vneid/form/sso' \
  --header 'accept: application/json, text/plain, */*' \
  --header 'content-type: application/x-www-form-urlencoded' \
  --header 'api_key: {{API_KEY}}' \
  --header 'target_code: {{TARGET_CODE}}' \
  --header 'request_id: 25a00741-9d9f-4f99-9a75-892ecd6a964d' \
  --header 'request_time: 12082026091500' \
  --data-urlencode 'code=a0c106c9-26ae-450e-89e7-cc9900f95428' \
  --data-urlencode 'grant_type=authorization_code' \
  --data-urlencode 'client_id={{CLIENT_ID}}' \
  --data-urlencode 'client_secret={{CLIENT_SECRET}}' \
  --data-urlencode 'redirect_uri={{SCEX_REDIRECT_URI}}'
```

---

## 3.2.3 API lấy thông tin công dân (`sso-userinfo`)

`GET` `https://{{AGENT_GW_DOMAIN}}/api/tcdn/sso-userinfo/api/vneid/sso`

**Mô tả:** Lấy thông tin định danh của công dân sau khi đã có `access_token` từ mục 3.2.2.

**Hệ thống xây dựng:** C06 / Nền tảng định danh (SCEX là client)

**Tham chiếu:** `tcdn_spec_api.md` mục III.3.4 · `rar_sso.md` mục 4.4

**Authentication:** `Bearer {access_token}` + `api_key`

#### Headers

Bộ header chung mục 3.2.0.3, với các điểm khác:

| Key             | Type     | Required | Description                                                                        |
| :-------------- | :------- | :------: | :--------------------------------------------------------------------------------- |
| `Authorization` | `String` | **Yes**  | `Bearer {access_token}`, lấy từ mục 3.2.2. Header này không thuộc bộ chung 3.2.0.3 |

#### Responses

**`200 OK`** — Trả về dữ liệu đã mã hoá

```json
{
  "data": "9sCIPPKfkuHJyZbq4nF4Z70A6Tm9ul6wd8oBsaTZr3qa18lcw8XzoXYYPcpWGj4y...",
  "key": "OJkjyfkxJ8mIYCGsyvihdEYVJr+UaSBO8ccyL2KiuYESqamq+0VWFlR6zqzvCq0V..."
}
```

**Sau khi giải mã `data`:**

```json
{
  "accountType": 1,
  "transactionCode": "57cf4c7c-ce43-42c2-89f6-5d9e5c88c760",
  "accountLevel": 2,
  "citizenPid": "030095008791",
  "fullName": "Nguyễn Huy Hoàng",
  "birthDate": "30-11-1995",
  "signature": "eyJhbGciOiJFUzI1NiIsInR5cCI6IkpPU0UvSldTIn0=.PEThu68gbGnhu4d1IGPhuqduIGvDvT4=.MEYCIQCZTe5k9SrOzu-p1xUmjYGvLHpgErR56WLGKwYEqftGoAIhAJUrbbI-GQnicA57cRyigE7hDHxJre56_Chvk4bg7H-p"
}
```

| Field             | Type      | Required | Description                              |
| :---------------- | :-------- | :------: | :--------------------------------------- |
| `citizenPid`      | `String`  |    No    | Số định danh cá nhân                     |
| `fullName`        | `String`  |    No    | Họ, chữ đệm và tên                       |
| `birthDate`       | `String`  |    No    | Ngày sinh, định dạng `DD-MM-YYYY`        |
| `accountType`     | `String`  |    No    | Loại tài khoản: `1` cá nhân, `2` tổ chức |
| `accountLevel`    | `Integer` |    No    | Mức định danh của tài khoản VNeID        |
| `transactionCode` | `String`  |    No    | Mã giao dịch                             |
| `signature`       | `String`  |    No    | Chữ ký JWS trên dữ liệu trả về           |

**`401 Unauthorized`** — `access_token` hết hạn hoặc không hợp lệ. Xem thêm mục 3.2.0.5.

#### Ràng buộc từ C06

- Nếu response có `key`, SCEX PHẢI giải mã `key` bằng `{{RSA_PRIVATE_KEY}}` (RSA) để lấy AES secret, rồi giải mã `data` (AES).
- `signature` PHẢI được verify bằng `{{C06_JWS_PUBLIC_KEY}}` trước khi tin dữ liệu. Dùng thư viện JWS tiêu chuẩn, KHÔNG ĐƯỢC tự implement.

#### Quyết định của SCEX

- SCEX PHẢI kiểm tra `accountLevel`. Nghiệp vụ chỉ chấp nhận tài khoản VNeID mức 2.
- SCEX PHẢI kiểm tra `accountType = 1` (cá nhân). Phạm vi tích hợp hiện tại không hỗ trợ tài khoản tổ chức (xem `scex-rar.md` mục 1.3 Out of scope).
- `citizenPid` và `fullName` là PII. KHÔNG ĐƯỢC ghi raw vào log (xem `scex-rar.md` mục 6.2).
- KHÔNG ĐƯỢC trả `access_token` của VNeID hay nguyên văn response này về client. SCEX Backend map sang model nội bộ rồi mới trả (xem mục 3.2.16).

#### Example cURL

```bash
curl --location 'https://{{AGENT_GW_DOMAIN}}/api/tcdn/sso-userinfo/api/vneid/sso' \
  --header 'content-type: application/json; charset=utf-8' \
  --header 'Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldU...' \
  --header 'api_key: {{API_KEY}}' \
  --header 'target_code: {{TARGET_CODE}}' \
  --header 'request_id: 25a00741-9d9f-4f99-9a75-892ecd6a964d' \
  --header 'request_time: 12082026091500'
```

---

## 3.2.4 URL đăng xuất VNeID (chỉ Web)

`GET` `{{URL_LOGOUT}}`

**Mô tả:** URL do VNeID cung cấp để SCEX Web điều hướng người dùng sang trang đăng xuất VNeID. Chỉ áp dụng SSO Web, không áp dụng App mobile.

**Hệ thống xây dựng:** C06 / Hệ thống Định danh và xác thực điện tử (SCEX là bên gọi)

**Tham chiếu:** `tcdn_spec_api.md` mục III.3.5 · `rar_sso.md` mục 4.5

**Authentication:** Không áp dụng (public redirect)

#### Cấu trúc

```
{{URL_LOGOUT}}?client_id={{CLIENT_ID}}&redirect_uri={{SCEX_LOGOUT_REDIRECT_URI}}&state=$[state]
```

#### Path / Query Parameters

| Field           | Type     | Required | Description                                                                 |
| :-------------- | :------- | :------: | :-------------------------------------------------------------------------- |
| `client_id`     | `String` | **Yes**  | `{{CLIENT_ID}}`                                                             |
| `redirect_uri`  | `String` | **Yes**  | `{{SCEX_LOGOUT_REDIRECT_URI}}`. KHÔNG ĐƯỢC dùng địa chỉ IP hoặc `localhost` |
| `state`         | `String` |   No\*   | Tham số SCEX tự định nghĩa để đối chiếu khi VNeID gọi lại `redirect_uri`    |
| `id_token_hint` | `String` |    No    | `id_token` lấy từ mục 3.2.2                                                 |

\* C06 không bắt buộc, nhưng SCEX bắt buộc theo yêu cầu bảo mật nội bộ (xem Quyết định của SCEX).

**Chưa chốt (OI-08):** Tài liệu C06 mục III.3.5 chỉ liệt kê `client_id` và `redirect_uri` (kèm ký hiệu `&…` cho phép tham số mở rộng). Việc VNeID có xử lý `id_token_hint` và `state` hay không phải được C06 xác nhận trước khi implement.

#### Responses

**`302 Found`** — Sau khi đăng xuất, VNeID điều hướng về `{{SCEX_LOGOUT_REDIRECT_URI}}` (xem mục 3.2.11).

#### Ràng buộc từ C06

- Chỉ Web SSO dùng URL này. App mobile SSO không có luồng đăng xuất qua VNeID. App chỉ xoá phiên nội bộ của SCEX.

#### Quyết định của SCEX

- SCEX PHẢI xoá phiên đăng nhập nội bộ khi nhận được callback, không phụ thuộc vào việc VNeID đăng xuất thành công hay không.
- `state` NÊN được gửi kèm để đối chiếu callback (xem mục 3.2.11), dù C06 chưa xác nhận VNeID trả lại tham số này.

#### Example cURL

```bash
curl -i "https://sso-uatvneid.dancuquocgia.com/api/idp/sso/login-actions/logout?client_id={{CLIENT_ID}}&redirect_uri=https%3A%2F%2F{{SCEX_API_ROOT}}%2Fapi%2Fv1%2Fauth%2Fvneid%2Flogout%2Fcallback&state=8f14e45fceea167a"
```

---

## 3.2.5 API khởi tạo giao dịch (`init transaction`)

`POST` `https://{{AGENT_GW_DOMAIN}}:{{AGENT_GW_PORT}}/api/tcdn/{service-code}/api/agent/init`

**Mô tả:** SCEX gửi yêu cầu chia sẻ thông tin công dân / xác thực sinh trắc học / xác thực thông tin công dân sang Nền tảng định danh qua Agent Gateway. Nền tảng kiểm tra yêu cầu và trả về mã giao dịch (`txnId`) để SCEX điều hướng công dân sang VNeID xác nhận.

**Áp dụng service-code:** `share-info`, `face-matching`, `biometric-share-info`, `verify-info`

**Hệ thống xây dựng:** C06 / Nền tảng định danh (SCEX là client)

**Tham chiếu:** `tcdn_spec_api.md` mục III.2.1

**Authentication:** `api_key` (header)

#### Headers

Bộ header chung mục 3.2.0.3.

#### Path / Query Parameters

| Field          | Type     |     Required     | Description                                                          |
| :------------- | :------- | :--------------: | :------------------------------------------------------------------- |
| `service-code` | `String` |  **Yes** (path)  | Mã dịch vụ, xem mục 3.2.0.7                                          |
| `code`         | `String` | Chỉ **non-prod** | `{{ORG_CONNECTION_CODE}}`, mã đơn vị kết nối. Môi trường prod: `N/A` |

#### Request Body

**Trường hợp 1 — dịch vụ chia sẻ thông tin / xác thực sinh trắc học** (`share-info`, `face-matching`, `biometric-share-info`):

| Field        | Type     | Required | Description               | Constraints                          |
| :----------- | :------- | :------: | :------------------------ | :----------------------------------- |
| `citizenPid` | `String` | **Yes**  | Số định danh của công dân | 12 ký tự số                          |
| `fullName`   | `String` |    No    | Họ và tên đầy đủ          | Max 100 ký tự                        |
| `birthDate`  | `String` |    No    | Ngày sinh                 | Định dạng `dd-MM-yyyy`, max 20 ký tự |

```json
{
  "citizenPid": "036xxxxxxx58",
  "fullName": "Nguyễn Thị Huế",
  "birthDate": "24-04-1988"
}
```

**Trường hợp 2 — dịch vụ xác thực thông tin công dân** (`verify-info`): ngoài `citizenPid` bắt buộc, mọi field khác SCEX gửi lên sẽ được đối khớp và trả về `true`/`false`; field không gửi lên sẽ trả về dữ liệu công dân đồng ý chia sẻ (hoặc `null`).

```json
{
  "citizenPid": "036xxxxxxx58",
  "fullName": "Nguyễn Thị Huế",
  "birthDate": "24-04-1988",
  "nationalityCode": "01",
  "permanentVillageCode": "09877",
  "permanentDistrictCode": null,
  "permanentCityCode": "01"
}
```

Các nhóm thông tin công dân có thể chia sẻ / xác thực: họ và tên · ngày sinh · số CCCD hoặc số định danh · số CMND · ngày cấp CCCD · nơi cấp CCCD · quốc tịch · nơi thường trú · nơi ở hiện tại · ngày hết hạn CCCD · giới tính.

Biến thể định dạng họ tên (tuỳ cấu hình dịch vụ đăng ký với C06): `fullNameUpper` (viết hoa, đầy đủ, có dấu, có cách) · `fullNameSrch` (in hoa, không dấu, có cách).

#### Responses

**`200 OK`** — Khởi tạo thành công

```json
{
  "responseCode": "0",
  "description": "Success!",
  "responseTime": "24032026135411",
  "data": {
    "txnId": "e9ac4603-05dd-4dd0-8090-325887fa2f45",
    "requestId": "25a00741-9d9f-4f99-9a75-892ecd6a964d"
  }
}
```

| Field            | Type          | Required | Description                                      |
| :--------------- | :------------ | :------: | :----------------------------------------------- |
| `responseCode`   | `String(2)`   | **Yes**  | `0` thành công; khác `0` là lỗi, xem mục 3.2.0.6 |
| `description`    | `String(100)` | **Yes**  | Mô tả lỗi khi `responseCode` khác `0`            |
| `responseTime`   | `String`      | **Yes**  | Thời gian phản hồi, định dạng `ddMMyyyyHHmmss`   |
| `data.txnId`     | `String`      |    No    | Mã giao dịch trả về khi khởi tạo thành công      |
| `data.requestId` | `String`      |    No    | Tham chiếu `request_id` SCEX đã gửi sang         |

**`200 OK` với `responseCode` khác `0`** — Khởi tạo thất bại

```json
{
  "responseCode": "-7",
  "description": "Service not found.",
  "responseTime": "30092024190321",
  "data": null
}
```

Các mã lỗi thường gặp ở endpoint này: `-2` giao dịch đã tồn tại · `-5` đầu vào không hợp lệ · `-6` không tìm thấy tổ chức · `-7` không tìm thấy dịch vụ · `-8` đường dẫn không hợp lệ · `-12` không tìm thấy cấu hình agent · `-14` timeout · `-16` vượt hạn mức · `-18` trường đầu vào không có trong cấu hình dịch vụ · `-19` tổ chức không có quyền dùng dịch vụ · `-20` thông tin công dân không đúng. Chi tiết xem mục 3.2.0.6.

#### Ràng buộc từ C06

- Sau khi nhận `txnId`, SCEX App điều hướng công dân sang VNeID qua universal link: `{{UNIVERSAL_LINK_SHARE}}/[$transactionCode]`, trong đó `[$transactionCode]` là `txnId`.
- Dịch vụ này chỉ có trên App VNeID, không triển khai trên Web VNeID. Luồng Web của SCEX không dùng được endpoint này.
- Với `share-info` / `face-matching` / `biometric-share-info`: chỉ `citizenPid` bắt buộc; `fullName` và `birthDate` không bắt buộc.
- Với `verify-info`: field nào gửi lên sẽ được đối khớp trả `true`/`false`; field không gửi lên sẽ trả dữ liệu nếu công dân đồng ý chia sẻ, ngược lại trả `null`.
- Chỉ được gửi các field đã đăng ký trong cấu hình dịch vụ với C06. Field ngoài cấu hình nhận `responseCode = -18`.
- SCEX PHẢI công bố link "Điều khoản và cam kết sử dụng dữ liệu được chia sẻ" để VNeID hiển thị cho công dân (xem mục 3.2.0.2).
- Khởi tạo giao dịch bằng service-code nào thì PHẢI lấy kết quả theo đúng service-code đó (mục 3.2.6 hoặc 3.2.7).

#### Example cURL

```bash
curl --location 'https://{{AGENT_GW_DOMAIN}}:{{AGENT_GW_PORT}}/api/tcdn/share-info/api/agent/init?code={{ORG_CONNECTION_CODE}}' \
  --header 'Content-Type: application/json; charset=utf-8' \
  --header 'api_key: {{API_KEY}}' \
  --header 'target_code: {{TARGET_CODE}}' \
  --header 'request_id: 25a00741-9d9f-4f99-9a75-892ecd6a964d' \
  --header 'request_time: 12082026091500' \
  --data '{
    "citizenPid": "036xxxxxxx58",
    "fullName": "Nguyễn Thị Huế",
    "birthDate": "24-04-1988"
  }'
```

---

## 3.2.6 API lấy kết quả giao dịch — không có xác thực ảnh mặt (`get-transaction` v1)

`POST` `https://{{AGENT_GW_DOMAIN}}:{{AGENT_GW_PORT}}/api/tcdn/{service-code}/api/get-transaction`

**Mô tả:** SCEX yêu cầu trả kết quả dữ liệu công dân đã đồng ý chia sẻ / kết quả xác thực thông tin công dân, cho các dịch vụ không bao gồm xác thực ảnh mặt.

**Áp dụng service-code:** `share-info` · `verify-info` (khi dịch vụ không được cấu hình xác thực ảnh mặt, xem mục 3.2.0.7)

**Hệ thống xây dựng:** C06 / Nền tảng định danh (SCEX là client)

**Tham chiếu:** `tcdn_spec_api.md` mục III.2.3

**Authentication:** `api_key` (header)

#### Headers

Bộ header chung mục 3.2.0.3.

#### Path / Query Parameters

| Field          | Type     |     Required     | Description                                                          |
| :------------- | :------- | :--------------: | :------------------------------------------------------------------- |
| `service-code` | `String` |  **Yes** (path)  | Phải trùng service-code đã dùng khi `init` (mục 3.2.5)               |
| `code`         | `String` | Chỉ **non-prod** | `{{ORG_CONNECTION_CODE}}`, mã đơn vị kết nối. Môi trường prod: `N/A` |

#### Request Body

| Field           | Type     | Required | Description                                                | Constraints         |
| :-------------- | :------- | :------: | :--------------------------------------------------------- | :------------------ |
| `transactionId` | `String` | **Yes**  | Mã giao dịch nhận được từ `init` (`data.txnId`, mục 3.2.5) | Hết hạn sau 30 phút |

**Example Request:**

```json
{
  "transactionId": "087f0479-98e8-4f37-a312-a7e24399e26a"
}
```

#### Responses

**`200 OK`** — Dịch vụ chia sẻ thông tin công dân (`share-info`)

```json
{
  "responseCode": "0",
  "description": "Success!",
  "responseTime": "24032026135443",
  "data": {
    "biometric": 0,
    "pid": null
  },
  "userInfo": "eyJhbGciOiJFUzI1NiIsInR5cCI6IkpPU0UvSldTIn0=.ewogICJjaXRpemVuUGlkIiA6...=.MEUCIQDNyQKCohM9skITRwaHsyRLltiUFfOln6MvDCQDTeLriAIgGHI0SB4vx71S2eFMBsoIBxXPMph7Cr-InfDyxGRAbfM="
}
```

| Field            | Type     | Required | Description                                                                      |
| :--------------- | :------- | :------: | :------------------------------------------------------------------------------- |
| `responseCode`   | `String` | **Yes**  | `0` thành công; khác `0` là lỗi, xem mục 3.2.0.6                                 |
| `description`    | `String` | **Yes**  | Mô tả lỗi khi `responseCode` khác `0`                                            |
| `responseTime`   | `String` | **Yes**  | `ddMMyyyyHHmmss`                                                                 |
| `data.biometric` | `Number` |    No    | Cờ cho biết giao dịch có kèm sinh trắc học hay không (`0` = không)               |
| `data.pid`       | `String` |    No    | Số định danh (có thể `null`)                                                     |
| `userInfo`       | `String` |    No    | Dữ liệu công dân, định dạng JWS `<header>.<payload>.<signature>` (base64 encode) |

**`200 OK`** — Dịch vụ xác thực thông tin công dân (`verify-info`)

Cấu trúc envelope giống trên; khác biệt nằm ở `<payload>` của `userInfo`:

- Field SCEX có gửi trong request `init` → trả về `true` / `false` (trùng khớp / không trùng khớp với CSDL dân cư).
- Field đã cấu hình nhưng SCEX không gửi → trả về giá trị thật của công dân nếu công dân đồng ý chia sẻ, ngược lại `null`.

Ví dụ `<payload>` sau khi parse:

```json
{
  "fullName": true,
  "birthDate": true,
  "nationalityCode": false,
  "citizenPid": true,
  "dateOfIssue": "23-07-2024",
  "issuingAuthority": "Cục Cảnh sát quản lý hành chính về trật tự xã hội",
  "livingPlaceAddress": "Khu Đô Thị Hai Bên Đường Lê Trọng Tấn",
  "livingPlaceVillageCode": "09877",
  "livingPlaceDistrictCode": null,
  "livingPlaceCityCode": "01",
  "permanentAddress": "Khu Đô Thị Hai Bên Đường Lê Trọng Tấn",
  "permanentVillageCode": true,
  "permanentDistrictCode": null,
  "permanentCityCode": true
}
```

**`200 OK` với `responseCode` khác `0`** — các mã thường gặp: `-9` không tìm thấy mã giao dịch · `-11` công dân không đồng ý chia sẻ · `-14` timeout · `-15` tồn tại field bắt buộc bị `null`. Xem mục 3.2.0.6.

#### Cấu trúc `<payload>` của `userInfo` (dịch vụ chia sẻ)

Danh sách field phụ thuộc cấu hình dịch vụ đã đăng ký với C06 và phạm vi công dân đồng ý chia sẻ. Không phải field nào cũng luôn xuất hiện.

| Field                     | Type     | Description                                            |
| :------------------------ | :------- | :----------------------------------------------------- |
| `citizenPid`              | `String` | Số định danh cá nhân                                   |
| `fullName`                | `String` | Họ, chữ đệm và tên                                     |
| `birthDate`               | `String` | Ngày sinh, `dd-MM-yyyy`                                |
| `genderCode`              | `String` | Mã giới tính, tham chiếu `tcdn_spec_api.md` mục IV.5.1 |
| `nationalityCode`         | `String` | Mã quốc tịch (ví dụ `VN`)                              |
| `indentifyNumber`         | `String` | Số CMND (giữ nguyên chính tả của tài liệu C06)         |
| `dateOfIssue`             | `String` | Ngày cấp CCCD, `dd-MM-yyyy`                            |
| `idCardExpireDate`        | `String` | Ngày hết hạn CCCD, có thể là chuỗi "Không thời hạn"    |
| `issuingAuthority`        | `String` | Nơi cấp CCCD                                           |
| `permanentAddress`        | `String` | Địa chỉ thường trú (dạng text)                         |
| `permanentVillageCode`    | `String` | Mã cấp xã, thường trú                                  |
| `permanentDistrictCode`   | `String` | Mã cấp huyện, thường trú. Luôn trả `null` kể từ v3.0   |
| `permanentCityCode`       | `String` | Mã cấp tỉnh, thường trú                                |
| `permanentVillageText`    | `String` | Tên cấp xã, thường trú                                 |
| `permanentDistrictText`   | `String` | Tên cấp huyện, thường trú (`null`)                     |
| `permanentCityText`       | `String` | Tên cấp tỉnh, thường trú                               |
| `livingPlaceAddress`      | `String` | Địa chỉ nơi ở hiện tại                                 |
| `livingPlaceVillageCode`  | `String` | Mã cấp xã, nơi ở hiện tại                              |
| `livingPlaceDistrictCode` | `String` | Mã cấp huyện, nơi ở hiện tại (`null`)                  |
| `livingPlaceCityCode`     | `String` | Mã cấp tỉnh, nơi ở hiện tại                            |
| `livingPlaceVillageText`  | `String` | Tên cấp xã, nơi ở hiện tại                             |
| `livingPlaceDistrictText` | `String` | Tên cấp huyện, nơi ở hiện tại (`null`)                 |
| `livingPlaceCityText`     | `String` | Tên cấp tỉnh, nơi ở hiện tại                           |

> Danh sách field đầy đủ và ràng buộc chi tiết: xem `tcdn_spec_api.md` mục III.2.3, Response body. Tài liệu này không nhân bản lại toàn bộ bảng để tránh lệch phiên bản.

#### Ràng buộc từ C06

- `transactionId` hết hạn sau 30 phút kể từ thời điểm ghi nhận công dân đã xác nhận trên VNeID.
- SCEX chỉ được gọi lấy dữ liệu tối đa 03 lần trong 30 phút đó; quá giới hạn sẽ không nhận được dữ liệu.
- Hạn mức mặc định 10.000 lần/ngày cho mỗi cặp (`target_code`, `service_code`). Nếu SCEX dự kiến vượt, phải đăng ký trước với Tổ chức cung cấp dịch vụ.
- SCEX PHẢI verify chữ ký JWS của `userInfo` bằng `{{C06_JWS_PUBLIC_KEY}}` trước khi tin dữ liệu. Dùng thư viện JWS tiêu chuẩn.
- Với luồng xuất phát từ VNeID (`scex-rar.md` mục 2.2): nếu SCEX đã gọi `get-transaction` thì PHẢI trả kết quả xử lý qua mục 3.2.8. Không trả kết quả sẽ khiến giao dịch của công dân bị treo và công dân không thực hiện được giao dịch mới.

#### Quyết định của SCEX

- Toàn bộ dữ liệu trong `userInfo` là PII. KHÔNG ĐƯỢC log raw; PHẢI masking theo chính sách bảo vệ dữ liệu cá nhân của SCEX.

#### Example cURL

```bash
curl --location 'https://{{AGENT_GW_DOMAIN}}:{{AGENT_GW_PORT}}/api/tcdn/share-info/api/get-transaction?code={{ORG_CONNECTION_CODE}}' \
  --header 'Content-Type: application/json; charset=utf-8' \
  --header 'api_key: {{API_KEY}}' \
  --header 'target_code: {{TARGET_CODE}}' \
  --header 'request_id: 3f2b1c88-7e41-4a90-9c1e-2b7d5a1f9e03' \
  --header 'request_time: 12082026091530' \
  --data '{
    "transactionId": "087f0479-98e8-4f37-a312-a7e24399e26a"
  }'
```

---

## 3.2.7 API lấy kết quả giao dịch — có xác thực ảnh mặt (`get-transaction` v2)

`POST` `https://{{AGENT_GW_DOMAIN}}:{{AGENT_GW_PORT}}/api/tcdn/{service-code}/v2/api/get-transaction`

**Mô tả:** SCEX gửi ảnh mặt và yêu cầu trả kết quả xác thực sinh trắc học kèm dữ liệu công dân đã đồng ý chia sẻ. Phiên bản v2 có ký số kết quả xác thực ảnh mặt (`biometricResult`).

**Áp dụng service-code:** `face-matching` · `biometric-share-info` · `verify-info` (khi dịch vụ được cấu hình xác thực ảnh mặt, xem mục 3.2.0.7)

**Hệ thống xây dựng:** C06 / Nền tảng định danh (SCEX là client)

**Tham chiếu:** `tcdn_spec_api.md` mục III.2.4

So sánh nguồn trong `tcdn_spec_api.md` mục III.2.4: dòng "Tên API" ghi dịch vụ không bao xác thực ảnh mặt, trong khi tiêu đề mục và toàn bộ nội dung (request body có `faceImage`, response có `biometricResult`) mô tả dịch vụ có xác thực ảnh mặt. SCEX chọn theo tiêu đề mục và nội dung kỹ thuật, vì đó khớp với contract thực tế của endpoint v2.

**Authentication:** `api_key` (header)

#### Headers

Bộ header chung mục 3.2.0.3, với các điểm khác:

| Key            | Type     |   Required   | Description                                                                                                                         |
| :------------- | :------- | :----------: | :---------------------------------------------------------------------------------------------------------------------------------- |
| `request_type` | `String` | Có điều kiện | Bắt buộc khi dùng SDK do TTXT cung cấp. `sdk-liveness-match`: xác thực chống giả mạo + ảnh mặt; `sdk-match`: chỉ xác thực khuôn mặt |

#### Path / Query Parameters

| Field          | Type     |     Required     | Description                                                          |
| :------------- | :------- | :--------------: | :------------------------------------------------------------------- |
| `service-code` | `String` |  **Yes** (path)  | Phải trùng service-code đã dùng khi `init` (mục 3.2.5)               |
| `code`         | `String` | Chỉ **non-prod** | `{{ORG_CONNECTION_CODE}}`, mã đơn vị kết nối. Môi trường prod: `N/A` |

#### Request Body

| Field           | Type     | Required | Description                                  | Constraints                               |
| :-------------- | :------- | :------: | :------------------------------------------- | :---------------------------------------- |
| `transactionId` | `String` | **Yes**  | Mã giao dịch nhận được từ `init` (mục 3.2.5) | Hết hạn sau 30 phút                       |
| `faceImage`     | `String` | **Yes**  | Chuỗi dữ liệu ảnh do SDK của TTXT trả ra     | Không phải ảnh raw do SCEX tự chụp/encode |

**Example Request:**

```json
{
  "transactionId": "9baf1668-a0da-4f9a-b9ac-5eef6f5da5f6",
  "faceImage": "AAABsWU3NmU2MWQ2YTY0NjRlODMyNzRmNWUxZmVlZWJmNzMyBKUUzlviAoxdoHp9aYaUuww..."
}
```

#### Responses

**`200 OK`** — Thành công

```json
{
  "responseCode": 0,
  "description": "Mô tả lỗi nếu có",
  "data": {
    "biometricResult": "ewogICJ0eXAiOiAiand0IiwKICAiYWxnIjogIlJTMjU2Igp9.ewogICJ0cmFuc0lkIiA6...=.gB4mdnRJHRBB17ForY5xlGOcKM3f0CKobEM7hUmwHQAkpItXHeKqA/pN3SNSm2tR..."
  },
  "userInfo": "eyJhbGciOiJFUzI1NiIsInR5cCI6IkpPU0UvSldTIn0=.ewogICJjaXRpemVuUGlkIiA6...=.MEUCIBNqg0LXH7UtJvU_A3vRZLLR20iacQxVK7lT1YZTVTJhAiEAjdPapssFIwhylfs68ufjT7RlByIKtrbjVIjw60CJWnc="
}
```

| Field                  | Type     | Required | Description                                               |
| :--------------------- | :------- | :------: | :-------------------------------------------------------- |
| `responseCode`         | `Number` | **Yes**  | `0` thành công; khác `0` là lỗi, xem mục 3.2.0.6          |
| `description`          | `String` | **Yes**  | Mô tả lỗi nếu có                                          |
| `data.biometricResult` | `String` |    No    | Kết quả sinh trắc học, định dạng JWS (base64 encode)      |
| `userInfo`             | `String` |    No    | Dữ liệu công dân, định dạng JWS. v2 dùng base64URL encode |

**Cấu trúc `<payload>` của `biometricResult` sau khi parse:**

```json
{
  "transId": "22fc12e0-be90-4519-8ef6-cdeca95ae22b",
  "statusCode": 1,
  "data": {
    "result": 1,
    "liveness": {
      "result": 1,
      "score": 0.98
    },
    "faceMatch": {
      "result": 1,
      "score": 0.98,
      "confidence": 3
    },
    "deviceRisk": {
      "isHighRisk": true,
      "flags": [2, 3]
    }
  }
}
```

| Field                        | Type      | Description                                                                                             |
| :--------------------------- | :-------- | :------------------------------------------------------------------------------------------------------ |
| `transId`                    | `String`  | Mã giao dịch                                                                                            |
| `statusCode`                 | `Number`  | Trạng thái gửi yêu cầu: `1` thành công, khác `1` không thành công                                       |
| `data.result`                | `Number`  | Kết quả tổng hợp: `1` xác thực, `0` không xác thực                                                      |
| `data.liveness.result`       | `Number`  | Kết quả chống giả mạo: `1` xác thực, `0` không xác thực. Chỉ có khi `request_type = sdk-liveness-match` |
| `data.liveness.score`        | `Number`  | Trọng số liveness                                                                                       |
| `data.faceMatch.result`      | `Number`  | Kết quả đối khớp khuôn mặt: `1` xác thực, `0` không xác thực                                            |
| `data.faceMatch.score`       | `Number`  | Trọng số face match                                                                                     |
| `data.faceMatch.confidence`  | `Number`  | Độ tin cậy: `1` thấp, `2` trung bình, `3` cao                                                           |
| `data.deviceRisk.isHighRisk` | `Boolean` | `true` thiết bị không an toàn, `false` thiết bị an toàn                                                 |
| `data.deviceRisk.flags`      | `Array`   | Danh sách mã nguyên nhân cảnh báo thiết bị không an toàn                                                |

**Cấu trúc `<payload>` của `userInfo`:** giống mục 3.2.6. Với `face-matching` (dịch vụ xác thực), các field SCEX gửi lên khi `init` sẽ trả `true`/`false`.

**`200 OK` với `responseCode` khác `0`** hoặc **`501 BIOMETRIC ERROR`** — lỗi xác thực sinh trắc học. Xem mục 3.2.0.5 và 3.2.0.6.

#### Ràng buộc từ C06

- Đường dẫn v2 có `/v2/` trước `/api/get-transaction`. Thứ tự này đã được C06 đổi ở v2.9.6 (trước đó là `/api/v2/get-transaction`). Dùng sai thứ tự sẽ nhận `responseCode = -8`.
- `faceImage` PHẢI là chuỗi do SDK của TTXT sinh ra. SCEX KHÔNG ĐƯỢC tự encode ảnh gửi lên.
- Giới hạn giống mục 3.2.6: `transactionId` hết hạn sau 30 phút, tối đa 03 lần gọi, hạn mức 10.000 lần/ngày trên mỗi cặp (`target_code`, `service_code`).
- SCEX PHẢI verify chữ ký JWS của cả `biometricResult` và `userInfo` bằng `{{C06_JWS_PUBLIC_KEY}}`.

#### Quyết định của SCEX

- `deviceRisk.isHighRisk = true` là tín hiệu rủi ro gian lận. SCEX PHẢI định nghĩa chính sách xử lý (chặn / yêu cầu xác thực bổ sung / cảnh báo vận hành), KHÔNG ĐƯỢC bỏ qua.
- SCEX PHẢI lưu `faceMatch.score` và `confidence` để phục vụ tra cứu của admin nghiệp vụ (xem `scex-rar.md` mục 6.2).
- `faceImage` và toàn bộ dữ liệu sinh trắc học là PII mức cao. KHÔNG ĐƯỢC log, KHÔNG ĐƯỢC lưu ngoài phạm vi cần thiết.

**Chưa chốt (OI-09):** Danh mục mã `deviceRisk.flags` chưa có trong tài liệu C06. Trước khi map sang chính sách xử lý nội bộ phải có xác nhận của C06.

**Chưa chốt (OI-10):** Đặc tả SDK của TTXT (chức năng, nền tảng hỗ trợ, cách tích hợp) chưa có tài liệu. Xem `scex-rar.md` Appendix A, Open Issues.

#### Example cURL

```bash
curl --location 'https://{{AGENT_GW_DOMAIN}}:{{AGENT_GW_PORT}}/api/tcdn/biometric-share-info/v2/api/get-transaction?code={{ORG_CONNECTION_CODE}}' \
  --header 'Content-Type: application/json; charset=utf-8' \
  --header 'api_key: {{API_KEY}}' \
  --header 'target_code: {{TARGET_CODE}}' \
  --header 'request_id: 6a1d9f22-3c84-4e17-b0aa-91e5c7d24f08' \
  --header 'request_time: 12082026091600' \
  --header 'request_type: sdk-liveness-match' \
  --data '{
    "transactionId": "9baf1668-a0da-4f9a-b9ac-5eef6f5da5f6",
    "faceImage": "AAABsWU3NmU2MWQ2YTY0NjRlODMyNzRmNWUxZmVlZWJmNzMy..."
  }'
```

---

## 3.2.8 API trả kết quả xử lý dữ liệu công dân (`receive-result`)

`POST` `https://{{AGENT_GW_DOMAIN}}:{{AGENT_GW_PORT}}/api/tcdn/{service-code}/agent/account/receive-result`

**Mô tả:** SCEX trả kết quả xử lý nghiệp vụ nội bộ (sau khi đã lấy dữ liệu công dân) về Nền tảng định danh qua Agent Gateway, để công dân thấy kết quả trên ứng dụng VNeID.

**Hệ thống xây dựng:** C06 / Nền tảng định danh (SCEX là client)

**Tham chiếu:** `tcdn_spec_api.md` mục III.5.1, III.5.2, III.5.3

**Authentication:** `api_key` (header)

#### Headers

Bộ header chung mục 3.2.0.3.

#### Path / Query Parameters

| Field          | Type     |     Required     | Description                                                          |
| :------------- | :------- | :--------------: | :------------------------------------------------------------------- |
| `service-code` | `String` |  **Yes** (path)  | Mã dịch vụ trả kết quả, xem mục 3.2.0.7                              |
| `code`         | `String` | Chỉ **non-prod** | `{{ORG_CONNECTION_CODE}}`, mã đơn vị kết nối. Môi trường prod: `N/A` |

#### Request Body

| Field               | Type     |    Required    | Description                                                             | Constraints                                |
| :------------------ | :------- | :------------: | :---------------------------------------------------------------------- | :----------------------------------------- |
| `requestStatus`     | `String` |    **Yes**     | Kết quả xử lý: `SUCCESS` thành công, `FAILURE` thất bại                 | Chỉ 2 giá trị này                          |
| `transactionId`     | `String` |    **Yes**     | Mã giao dịch SCEX nhận được khi khởi tạo giao dịch                      | —                                          |
| `reasonRequestCode` | `String` | Khi `FAILURE`  | Mã lý do thất bại, thống nhất trước với C06 trong sheet đăng ký dịch vụ | —                                          |
| `reasonRequestText` | `String` | Khi chưa có mã | Nội dung lỗi dạng text, dùng khi chưa có quy định mã lý do              | Phải mô tả rõ lý do                        |
| `fieldName…`        | `varies` | Khi `SUCCESS`  | Các field chi tiết cần hiển thị trên ứng dụng VNeID                     | Phụ thuộc từng dịch vụ, xem lưu ý bên dưới |

**Example Request — kết quả THÀNH CÔNG:**

```json
{
  "requestStatus": "SUCCESS",
  "transactionId": "087f0479-98e8-4f37-a312-a7e24399e26a",
  "citizenPid": "036xxxxxxx58"
}
```

**Example Request — kết quả THẤT BẠI:**

```json
{
  "requestStatus": "FAILURE",
  "transactionId": "087f0479-98e8-4f37-a312-a7e24399e26a",
  "reasonRequestCode": "SCEX_KYC_REJECTED",
  "reasonRequestText": "Thông tin định danh không đáp ứng điều kiện mở tài khoản giao dịch tài sản mã hoá theo quy định nội bộ của SCEX."
}
```

**Chưa chốt (OI-03):** `tcdn_spec_api.md` mục III.5.2 chỉ đặc tả danh sách field chi tiết cho các dịch vụ `create-account`, `create-credit-card`, `create-ewallet-account`, `create-smartca-account`, `register-insurance`…, không có mục riêng cho `share-info` / `biometric-share-info` / `face-matching` / `verify-info`. SCEX cần C06 xác nhận những field chi tiết nào ngoài `requestStatus` + `transactionId` phải trả về.

**Chưa chốt (OI-04):** Danh mục `reasonRequestCode` mà SCEX cần đăng ký trước với C06 chưa được chốt.

#### Responses

**`200 OK`** — Nền tảng đã tiếp nhận kết quả

```json
{
  "statusCode": 1,
  "errorDetail": "Mô tả kết quả trả về (nếu có)",
  "transId": "9e2b5a94-6062-4fe7-b216-48e5b060eba5"
}
```

| Field         | Type          | Required | Description                                              |
| :------------ | :------------ | :------: | :------------------------------------------------------- |
| `statusCode`  | `String(2)`   | **Yes**  | `1` thành công, `0` không thành công                     |
| `errorDetail` | `String(100)` | **Yes**  | Mô tả chi tiết lỗi khi `statusCode = 0`                  |
| `transId`     | `String(36)`  | **Yes**  | Mã giao dịch mà SCEX đang trả kết quả trong request body |

Các mã lỗi HTTP khác: xem mục 3.2.0.5.

#### Ràng buộc từ C06

- SCEX PHẢI gọi endpoint này nếu đã gọi `get-transaction` (mục 3.2.6 hoặc 3.2.7) trong luồng xuất phát từ VNeID. Không trả kết quả sẽ khiến giao dịch của công dân bị treo trên app VNeID và công dân không thực hiện được giao dịch mới, vì VNeID không có thông tin kết quả xử lý nên không tự cập nhật được trạng thái.
- `transactionId` gửi lên PHẢI đúng mã giao dịch đã dùng ở `init` / `get-transaction`.
- `reasonRequestCode` PHẢI nằm trong danh mục đã đăng ký trước với C06. `reasonRequestText` cần mô tả rõ lý do kèm dẫn chứng (nội dung này hiển thị trực tiếp cho công dân trên app VNeID).

#### Quyết định của SCEX

- SCEX NÊN có cơ chế retry có giới hạn và hàng đợi bền (durable queue) cho lời gọi này, vì đây là bước bắt buộc và không được mất.

#### Example cURL

```bash
curl --location 'https://{{AGENT_GW_DOMAIN}}:{{AGENT_GW_PORT}}/api/tcdn/share-info/agent/account/receive-result?code={{ORG_CONNECTION_CODE}}' \
  --header 'Content-Type: application/json; charset=utf-8' \
  --header 'api_key: {{API_KEY}}' \
  --header 'target_code: {{TARGET_CODE}}' \
  --header 'request_id: 9c4f7e10-52ab-4d3f-8e6b-1f0a3c7b95d2' \
  --header 'request_time: 12082026091700' \
  --data '{
    "requestStatus": "SUCCESS",
    "transactionId": "087f0479-98e8-4f37-a312-a7e24399e26a",
    "citizenPid": "036xxxxxxx58"
  }'
```

---

## 3.2.9 API trả kết quả xử lý yêu cầu xoá dữ liệu (`withdraw-consent-result`)

`POST` `https://{{AGENT_GW_DOMAIN}}:{{AGENT_GW_PORT}}/api/tcdn/withdraw-consent-result/agent/account/receive-result`

**Mô tả:** SCEX trả kết quả xử lý yêu cầu xoá dữ liệu công dân đã đồng ý chia sẻ. Chỉ gọi khi SCEX đã phản hồi `PROCESSING` ở mục 3.2.14 (tức SCEX đã lấy dữ liệu công dân trước đó).

**Hệ thống xây dựng:** C06 / Nền tảng định danh (SCEX là client)

**Tham chiếu:** `tcdn_spec_api.md` mục III.4.2

**Authentication:** `api_key` (header)

#### Headers

Bộ header chung mục 3.2.0.3, với các điểm khác:

| Key           | Type     | Required | Description                                                                                  |
| :------------ | :------- | :------: | :------------------------------------------------------------------------------------------- |
| `request_id`  | `String` | **Yes**  | Tham chiếu đúng `request_id` đã nhận được ở mục 3.2.14. KHÔNG ĐƯỢC sinh mới                  |
| `target_code` | `String` | **Yes**  | Giá trị cố định `WITHDRAW-CONSENT-RESULT` (không dùng `{{TARGET_CODE}}` của dịch vụ chia sẻ) |

#### Path / Query Parameters

| Field  | Type     |     Required     | Description                                                          |
| :----- | :------- | :--------------: | :------------------------------------------------------------------- |
| `code` | `String` | Chỉ **non-prod** | `{{ORG_CONNECTION_CODE}}`, mã đơn vị kết nối. Môi trường prod: `N/A` |

#### Request Body

| Field               | Type           |           Required           | Description                                        | Constraints                              |
| :------------------ | :------------- | :--------------------------: | :------------------------------------------------- | :--------------------------------------- |
| `requestStatus`     | `String`       |           **Yes**            | `DELETED` đã xoá thành công · `REJECT` từ chối xoá | Chỉ 2 giá trị này                        |
| `reasonRequestCode` | `String(100)`  |         Khi `REJECT`         | Mã lý do từ chối, thống nhất trước với C06         | —                                        |
| `reasonRequestText` | `String(1000)` | Khi `reasonRequestCode` rỗng | Nội dung chi tiết lý do từ chối                    | Cần nêu rõ lý do kèm dẫn chứng pháp luật |

**Example Request — đã xoá thành công:**

```json
{
  "requestStatus": "DELETED",
  "reasonRequestCode": "SCEX_DATA_PURGED",
  "reasonRequestText": "Đã xoá toàn bộ dữ liệu định danh nhận từ VNeID theo yêu cầu của công dân."
}
```

**Example Request — từ chối xoá:**

```json
{
  "requestStatus": "REJECT",
  "reasonRequestCode": "SCEX_LEGAL_RETENTION",
  "reasonRequestText": "SCEX phải lưu trữ dữ liệu xác thực tối thiểu 10 năm theo Nghị quyết 05/2025/NQ-CP, do đó không thể xoá dữ liệu trong thời hạn lưu trữ bắt buộc."
}
```

#### Responses

**`200 OK`** — Thành công

```json
{
  "statusCode": 1
}
```

**`200 OK`** — Không thành công

```json
{
  "statusCode": 0,
  "errorDetail": "Mô tả lỗi"
}
```

| Field         | Type           | Required | Description                                       |
| :------------ | :------------- | :------: | :------------------------------------------------ |
| `statusCode`  | `number(1)`    | **Yes**  | `1` thành công, `0` không thành công              |
| `errorDetail` | `string(4000)` |    No    | Mô tả chi tiết lỗi, bắt buộc khi `statusCode = 0` |

#### Ràng buộc từ C06

- Chỉ gọi endpoint này khi mục 3.2.14 đã trả `requestStatus = PROCESSING`. Nếu đã trả `DELETED` ngay ở 3.2.14 thì luồng dừng tại đó, không gọi endpoint này.
- Thời hạn xử lý tối đa 72 giờ kể từ khi tiếp nhận yêu cầu ở mục 3.2.14. SCEX PHẢI có job giám sát để không vượt hạn.
- `request_id` PHẢI tham chiếu đúng giá trị nhận được ở mục 3.2.14. Đây là khoá liên kết giữa yêu cầu và kết quả.
- `reasonRequestText` hiển thị trực tiếp cho công dân trên app VNeID, nên phải rõ ràng và có dẫn chứng pháp luật khi từ chối.

#### Quyết định của SCEX

- Xung đột nghiệp vụ cần xử lý: SCEX phải lưu trữ dữ liệu 10 năm (Nghị quyết 05/2025/NQ-CP) trong khi công dân có quyền yêu cầu xoá (Nghị định 13/2023/NĐ-CP). SCEX PHẢI chốt phạm vi dữ liệu được xoá và phạm vi buộc phải giữ, thống nhất mã lý do từ chối với C06 trước khi go-live.

**Chưa chốt (OI-11):** Phạm vi dữ liệu phải xoá khi công dân rút lại đồng ý (dữ liệu nhận từ VNeID, dữ liệu công dân tự nhập trên SCEX, dữ liệu sinh trắc học, log xác thực) chưa được làm rõ. Xem `scex-rar.md` mục 2.5.

#### Example cURL

```bash
curl --location 'https://{{AGENT_GW_DOMAIN}}:{{AGENT_GW_PORT}}/api/tcdn/withdraw-consent-result/agent/account/receive-result?code={{ORG_CONNECTION_CODE}}' \
  --header 'Content-Type: application/json; charset=utf-8' \
  --header 'api_key: {{API_KEY}}' \
  --header 'request_id: 5e8c1b47-9f30-4a2d-b6e1-7c04a9f2d813' \
  --header 'target_code: WITHDRAW-CONSENT-RESULT' \
  --header 'request_time: 12082026091800' \
  --data '{
    "requestStatus": "DELETED",
    "reasonRequestCode": "SCEX_DATA_PURGED",
    "reasonRequestText": "Đã xoá toàn bộ dữ liệu định danh nhận từ VNeID theo yêu cầu của công dân."
  }'
```

---

# NHÓM B — API / URL do SCEX cung cấp cho RAR / C06

SCEX là bên xây dựng. C06 (qua Agent Gateway) hoặc ứng dụng VNeID gọi vào / điều hướng vào các endpoint này.

**Base URL:** `https://{{SCEX_API_ROOT}}` · `https://{{SCEX_WEB_ROOT}}` · `{{SCEX_UNIVERSAL_LINK}}`

Contract (tên field, enum, mã trạng thái) bám `tcdn_spec_api.md`: giữ nguyên `camelCase` của C06, KHÔNG ĐƯỢC đổi sang `snake_case`. Host và path do SCEX định nghĩa và PHẢI đăng ký trước với C06 để cấu hình vào Agent Gateway.

### Đánh giá rủi ro bảo mật nhóm B

Nhóm B nhận traffic từ ngoài biên SCEX. Theo `tcdn_spec_api.md`, chiều C06 → TCDN chỉ yêu cầu `api_key` trong header; không có chữ ký bản tin, không có mTLS bắt buộc. Với bề mặt callback và redirect, chỉ `api_key` là không đủ để kiểm soát giả mạo nguồn, replay, hay lộ kênh.

**Kết luận:** SCEX bù rủi ro bằng lớp điều khiển của riêng mình (không phải yêu cầu C06): allowlist IP Agent Gateway, chống replay theo `request_id` / `request_time`, idempotency, rate limit, và mTLS ở tầng hạ tầng khi C06 hỗ trợ. Redirect URI Web/App vẫn phải public theo bản chất trình duyệt/deep link; các callback API thì KHÔNG ĐƯỢC expose ra Internet công cộng.

| Lớp bảo vệ   | Yêu cầu                                                                                                                                                    |
| ------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Mạng         | Allowlist IP của Agent Gateway; chỉ nhận traffic từ vùng biên SCEX. KHÔNG ĐƯỢC expose ra Internet công cộng (trừ redirect URI của Web/App vốn phải public) |
| Kênh truyền  | TLS 1.2+ PHẢI có. NÊN dùng mTLS cho các callback từ Agent Gateway                                                                                          |
| Xác thực     | Verify `api_key` do SCEX cấp cho C06 (chuỗi 32–64 ký tự) trên mọi callback                                                                                 |
| Chống replay | Lưu `request_id` đã xử lý; từ chối `request_id` trùng. Kiểm tra `request_time` trong cửa sổ ±2 phút                                                        |
| Idempotency  | Mọi callback PHẢI xử lý idempotent theo `request_id` + `transactionCode` / `txnId`                                                                         |
| Rate limit   | Giới hạn theo IP và theo `api_key`                                                                                                                         |
| Log & audit  | Ghi log đầy đủ theo `scex-rar.md` mục 6.2; masking PII                                                                                                     |

**Chưa chốt (OI-02):** Cần C06 xác nhận (1) có hỗ trợ mTLS chiều callback không, (2) có ký số bản tin callback không, (3) dải IP nguồn Agent Gateway để allowlist. Cho đến khi có xác nhận, SCEX triển khai theo bảng bù rủi ro ở trên.

---

## 3.2.10 Redirect URI SSO — nhận `code` sau đăng nhập VNeID

`GET` `https://{{SCEX_API_ROOT}}/api/v1/auth/vneid/callback`

**Mô tả:** URL do SCEX xây dựng và đăng ký với C06. VNeID điều hướng người dùng về kèm mã `code` sau khi xác thực thành công.

**Hệ thống xây dựng:** SCEX (VNeID là bên điều hướng vào)

**Tham chiếu contract:** `tcdn_spec_api.md` mục III.3.2 · `rar_sso.md` mục 4.2

**Authentication:** Public endpoint (điều hướng trình duyệt / app). Bảo vệ bằng `state` + rate limit.

#### Cấu trúc

```
{{SCEX_REDIRECT_URI}}?code=$[code]&encrypt_key=$[encrypt_key]&state=$[state]
```

#### Path / Query Parameters

| Field         | Type     | Required | Description                                                                                                            |
| :------------ | :------- | :------: | :--------------------------------------------------------------------------------------------------------------------- |
| `code`        | `String` | **Yes**  | Code xác thực thành công, dùng để đổi `access_token` ở mục 3.2.2                                                       |
| `encrypt_key` | `String` |    No    | AES secret key đã được VNeID mã hoá bằng RSA. Nếu có giá trị thì dữ liệu bước sau được mã hoá; nếu không thì plaintext |
| `state`       | `String` |   No\*   | Giá trị SCEX đã gửi ở mục 3.2.1, VNeID trả lại nguyên văn                                                              |

\* C06 không bắt buộc; SCEX bắt buộc để chống CSRF.

#### Responses

**`302 Found`** — Hợp lệ: SCEX validate `state`, đổi `code` lấy token (mục 3.2.2), lấy userinfo (mục 3.2.3), cấp SCEX token rồi redirect về Web App.

**`302 Found` → trang lỗi** — `state` không hợp lệ / hết hạn / đã dùng, hoặc thiếu `code`.

Endpoint này KHÔNG ĐƯỢC trả JSON envelope vì là redirect trên trình duyệt. Với luồng App (mục 3.2.16) SCEX dùng endpoint JSON riêng.

#### Ràng buộc từ C06

- KHÔNG ĐƯỢC dùng hàm `getQueryParam` có sẵn của ngôn ngữ lập trình để đọc `encrypt_key`. Cảnh báo tường minh trong `tcdn_spec_api.md` mục III.3.2 và `rar_sso.md` mục 4.2: giá trị này CÓ THỂ chứa ký tự làm sai lệch parse tự động (`+`, `/`, `=`). PHẢI parse thủ công từ raw query string.
- `{{SCEX_REDIRECT_URI}}` PHẢI nằm trong danh sách SCEX đã đăng ký để C06 cấu hình trước; KHÔNG ĐƯỢC dùng IP hoặc `localhost`.

#### Quyết định của SCEX

- `state` PHẢI được validate: tồn tại, chưa dùng, chưa hết TTL (300s). Sau khi dùng PHẢI vô hiệu hoá ngay.
- `code` chỉ dùng 1 lần. SCEX PHẢI chống double-submit (user refresh trang callback).
- KHÔNG ĐƯỢC log `code` và `encrypt_key`.

#### Example cURL

```bash
curl -i "https://{{SCEX_API_ROOT}}/api/v1/auth/vneid/callback?code=a0c106c9-26ae-450e-89e7-cc9900f95428&encrypt_key=OJkjyfkxJ8mIYCGsyvihdEYVJr%2BUaSBO&state=8f14e45fceea167a5a36dedd4bea2543"
```

---

## 3.2.11 Redirect URI sau đăng xuất VNeID

`GET` `https://{{SCEX_API_ROOT}}/api/v1/auth/vneid/logout/callback`

**Mô tả:** URL do SCEX xây dựng. VNeID điều hướng về sau khi đăng xuất thành công. Chỉ áp dụng SSO Web.

**Hệ thống xây dựng:** SCEX (VNeID là bên điều hướng vào)

**Tham chiếu contract:** `tcdn_spec_api.md` mục III.3.5 · `rar_sso.md` mục 4.5

**Authentication:** Public endpoint. Bảo vệ bằng `state` + rate limit.

#### Path / Query Parameters

| Field   | Type     | Required | Description                                        |
| :------ | :------- | :------: | :------------------------------------------------- |
| `state` | `String` |   No\*   | Giá trị SCEX đã gửi ở mục 3.2.4, dùng để đối chiếu |

\* SCEX bắt buộc theo yêu cầu bảo mật nội bộ; xem mục 3.2.4 về việc VNeID có hỗ trợ `state` hay không.

**Chưa chốt (OI-15):** `tcdn_spec_api.md` mục III.3.5 và `rar_sso.md` mục 4.5 chỉ nêu chuyển hướng về `redirect_uri` do TCDN cung cấp, không đặc tả query param nào VNeID gửi kèm. SCEX KHÔNG ĐƯỢC phụ thuộc vào bất kỳ tham số nào ở endpoint này cho đến khi C06 xác nhận danh sách thực tế. Liên quan OI-08 về hỗ trợ `state` trên logout.

#### Responses

**`302 Found`** — SCEX validate `state` (nếu có), xoá phiên đăng nhập nội bộ, redirect về trang logout của Web App.

#### Ràng buộc từ C06

- Endpoint này chỉ áp dụng SSO Web. App mobile không có luồng logout qua VNeID.

#### Quyết định của SCEX

- SCEX PHẢI xoá phiên đăng nhập nội bộ, không phụ thuộc kết quả đăng xuất phía VNeID.
- Nếu `state` không hợp lệ hoặc thiếu, vẫn xoá phiên nội bộ và ghi log cảnh báo. KHÔNG ĐƯỢC để phiên treo.

#### Example cURL

```bash
curl -i "https://{{SCEX_API_ROOT}}/api/v1/auth/vneid/logout/callback?state=8f14e45fceea167a"
```

---

## 3.2.12 Universal Link / URL redirect nhận `transactionCode` từ VNeID

`GET` `{{SCEX_UNIVERSAL_LINK}}/vneid/consent/{transactionCode}` (App-to-App)
`GET` `https://{{SCEX_WEB_ROOT}}/vneid/consent/{transactionCode}` (App-to-Web)

**Mô tả:** Đường dẫn do SCEX xây dựng và cung cấp cho VNeID. VNeID dùng để switch App-to-App (hoặc App-to-Web) trả `transactionCode` về ứng dụng SCEX.

**Hệ thống xây dựng:** SCEX (ứng dụng VNeID là bên mở vào)

**Tham chiếu contract:** `tcdn_spec_api.md` mục III.1.1 · III.2.5

**Authentication:** Không áp dụng (deep link). Xử lý nghiệp vụ phía sau PHẢI xác thực (mục 3.2.20 và 3.2.23).

#### Cấu trúc — 2 dạng theo luồng

| Luồng                                                         | Cấu trúc                                 | Áp dụng                    |
| ------------------------------------------------------------- | ---------------------------------------- | -------------------------- |
| Xuất phát từ ứng dụng SCEX (SCEX gọi `init` trước, mục 3.2.5) | `URL_TCDN/[$transactionCode]\|[$result]` | `scex-rar.md` mục 2.3, 2.4 |
| Xuất phát từ ứng dụng VNeID (công dân chọn SCEX trên VNeID)   | `URL_TCDN/[$transactionCode]`            | `scex-rar.md` mục 2.2      |

#### Path / Query Parameters

| Field             | Type     |          Required           | Description                                             |
| :---------------- | :------- | :-------------------------: | :------------------------------------------------------ |
| `transactionCode` | `String` |           **Yes**           | Mã giao dịch xin consent                                |
| `result`          | `String` | Chỉ luồng xuất phát từ SCEX | Kết quả: `1` đã đồng ý · `2` không đồng ý · `3` hết hạn |

#### Responses

**`200 OK`** — Mở đúng màn hình xử lý tiếp trong ứng dụng SCEX. App/Web sau đó gọi API nội bộ (mục 3.2.20 với luồng xuất phát từ SCEX, mục 3.2.23 với luồng xuất phát từ VNeID).

#### Ràng buộc từ C06

- Dấu phân cách giữa `transactionCode` và `result` trong luồng xuất phát từ SCEX là ký tự `|`. PHẢI URL-encode thành `%7C` khi xử lý.

#### Quyết định của SCEX

- `transactionCode` nhận từ deep link là dữ liệu không tin cậy. SCEX PHẢI validate ở Backend (mục 3.2.20 / 3.2.23) trước khi xử lý nghiệp vụ. KHÔNG ĐƯỢC tin `result` từ deep link; PHẢI đối chiếu với kết quả `get-transaction`.
- Universal Link PHẢI cấu hình `apple-app-site-association` (iOS) và `assetlinks.json` (Android) đúng để tránh bị app khác hijack.
- Nếu app SCEX chưa được cài, App-to-Web PHẢI có trang fallback hướng dẫn người dùng.

#### Example cURL

```bash
# App-to-Web, luồng xuất phát từ SCEX (result = 1: đã đồng ý)
curl -i "https://{{SCEX_WEB_ROOT}}/vneid/consent/9baf1668-a0da-4f9a-b9ac-5eef6f5da5f6%7C1"

# App-to-App, luồng xuất phát từ VNeID
curl -i "{{SCEX_UNIVERSAL_LINK}}/vneid/consent/02d73def-fe15-418c-b97c-37c8214e3ba6"
```

---

## 3.2.13 API nhận thông báo kết quả xác nhận chia sẻ của công dân

`POST` `https://{{SCEX_API_ROOT}}/api/v1/vneid/callbacks/consent-status`

**Mô tả:** Callback từ Nền tảng định danh tới SCEX sau `init`. Payload gồm ba biến thể: tài khoản VNeID không hợp lệ, VNeID đã hiển thị màn consent (`INIT`), hoặc công dân đã đồng ý / từ chối chia sẻ.

**Hệ thống xây dựng:** SCEX (C06 là client gọi vào). URL do SCEX định nghĩa và cấu hình sẵn trong Agent Gateway.

**Tham chiếu contract:** `tcdn_spec_api.md` mục III.2.2 · mục II.3

**Authentication:** `api_key` do SCEX cấp cho C06 (chuỗi 32–64 ký tự) + allowlist IP Agent Gateway

#### Headers

| Key            | Type     | Required | Description                                                 |
| :------------- | :------- | :------: | :---------------------------------------------------------- |
| `Content-Type` | `String` | **Yes**  | `application/json; charset=utf-8`                           |
| `api_key`      | `String` | **Yes**  | API key do SCEX cung cấp cho C06, chuỗi 32–64 ký tự         |
| `request_id`   | `String` | **Yes**  | Mã yêu cầu do Nền tảng sinh, duy nhất trên hệ thống của C06 |
| `request_time` | `String` | **Yes**  | `ddMMyyyyHHmmss`                                            |

#### Request Body

Bản tin có 3 biến thể tuỳ tình huống. SCEX PHẢI xử lý được cả 3.

**Biến thể 1 — VNeID đã hiển thị màn consent thành công (trạng thái `INIT`):**

| Field             | Type     | Required | Description    | Constraints |
| :---------------- | :------- | :------: | :------------- | :---------- |
| `transactionCode` | `String` | **Yes**  | Mã giao dịch   | —           |
| `requestStatus`   | `String` | **Yes**  | Giá trị `INIT` | —           |

```json
{
  "transactionCode": "02d73def-fe15-418c-b97c-37c8214e3ba6",
  "requestStatus": "INIT"
}
```

**Biến thể 2 — công dân đồng ý / từ chối chia sẻ:**

| Field        | Type     | Required | Description                                                            | Constraints |
| :----------- | :------- | :------: | :--------------------------------------------------------------------- | :---------- |
| `requestId`  | `String` | **Yes**  | Tham chiếu `request_id` SCEX đã gửi ở `init` (mục 3.2.5)               | —           |
| `resultCode` | `String` | **Yes**  | `1` đồng ý chia sẻ · `2` không đồng ý chia sẻ — xem mục 3.2.0.6        | —           |
| `resultDesc` | `String` |    No    | Mô tả chi tiết `resultCode`                                            | —           |
| `txnId`      | `String` | **Yes**  | Mã giao dịch dùng để truy vấn thông tin công dân (khi công dân đồng ý) | —           |
| `data`       | `Object` |    No    | Dữ liệu trả thêm tuỳ cấu hình; không có thì field này không xuất hiện  | —           |

```json
{
  "requestId": "7353b035-90e7-436c-93df-faa9e1817d74",
  "resultCode": "1",
  "resultDesc": "Success!",
  "txnId": "63d57f38-99dc-4c53-b05c-57dd24ec386d"
}
```

**Biến thể 3 — thông tin công dân không hợp lệ (tài khoản mức 1, bị khoá, CCCD hết hạn, không tồn tại…):**

```json
{
  "requestId": "d052c2b651964557b242e885efbf82a2",
  "resultCode": "-17",
  "resultDesc": "Thông tin công dân không hợp lệ, vui lòng kiểm tra chi tiết trên ứng dụng VNeID",
  "txnId": "22c3cf62-b6bd-4b04-b8ba-e29a5d03ac42"
}
```

#### Responses

SCEX PHẢI trả `HTTP 200` với bản tin JSON theo đúng cấu trúc C06 quy định (không dùng envelope của SCEX):

**`200 OK`** — Tiếp nhận thành công

```json
{
  "responseCode": "0",
  "description": "",
  "responseTime": "12082026091900"
}
```

**`200 OK`** — Tiếp nhận lỗi

```json
{
  "responseCode": "1",
  "description": "Không tìm thấy giao dịch tương ứng với requestId",
  "responseTime": "12082026091900"
}
```

| Field          | Type          | Required | Description                           |
| :------------- | :------------ | :------: | :------------------------------------ |
| `responseCode` | `String(2)`   | **Yes**  | `0` thành công; khác `0` là lỗi       |
| `description`  | `String(100)` | **Yes**  | Mô tả lỗi khi `responseCode` khác `0` |
| `responseTime` | `String`      | **Yes**  | `ddMMyyyyHHmmss`                      |

Hai chỗ trong `tcdn_spec_api.md` mục III.2.2 không khớp nhau: ghi chú mẫu API Specs ghi `"responseCode": 1 //0: thành công, 1: không thành công`, trong khi bảng đặc tả Response body ghi `0: thành công, Khác 0: Lỗi`. SCEX chọn theo bảng đặc tả: `0` = thành công.

**Chưa chốt (OI-05):** Cần C06 xác nhận chính thức quy ước `responseCode` để tránh lệch triển khai giữa hai phía.

#### Ràng buộc từ C06

- Mục III.2.2 nêu phản hồi biến thể 3 (công dân không hợp lệ) trong khoảng dưới 1 giây kể từ `init`; mục II.3 lại nêu ngưỡng tối đa 3 giây. Xem quyết định SCEX và OI-07.
- Với biến thể 3, SCEX PHẢI dừng luồng nghiệp vụ, KHÔNG ĐƯỢC switch sang VNeID, và hiển thị thông báo hướng dẫn công dân kiểm tra lại trên app VNeID. Mã giao dịch đã hết hiệu lực.
- Với `resultCode = 1`, SCEX dùng `txnId` để gọi `get-transaction` (mục 3.2.6 / 3.2.7).
- Với `resultCode = 2`, kết thúc luồng và thông báo cho user; KHÔNG ĐƯỢC gọi `get-transaction`.
- Endpoint PHẢI idempotent: cùng `request_id` gửi lại nhiều lần chỉ được xử lý nghiệp vụ một lần, nhưng vẫn trả `responseCode = 0`.
- PHẢI trả HTTP 200 kể cả khi nghiệp vụ lỗi. Lỗi thể hiện qua `responseCode`, không qua HTTP status.

#### Quyết định của SCEX

- SCEX chọn ngưỡng chờ 3 giây sau `init` trước khi switch App-to-App sang VNeID (an toàn hơn ngưỡng 1 giây ở mục III.2.2).

**Chưa chốt (OI-07):** Cần C06 thống nhất ngưỡng thời gian phản hồi biến thể 3 giữa mục III.2.2 (< 1s) và mục II.3 (tối đa 3s).

- SCEX phải quyết định có bật nhận trạng thái `INIT` hay không (C06 cấu hình theo yêu cầu TCDN). Nếu bật, luồng là: `init` → nhận `INIT` → mới switch sang VNeID.

**Chưa chốt (OI-06):** SCEX chọn Trường hợp 1 (nhận `INIT`) hay Trường hợp 2 (không nhận `INIT`) theo mục II.3. Quyết định này ảnh hưởng trực tiếp tới luồng SCEX App.

#### Example cURL

```bash
curl --location 'https://{{SCEX_API_ROOT}}/api/v1/vneid/callbacks/consent-status' \
  --header 'Content-Type: application/json; charset=utf-8' \
  --header 'api_key: <api_key SCEX cấp cho C06>' \
  --header 'request_id: 7353b035-90e7-436c-93df-faa9e1817d74' \
  --header 'request_time: 12082026091900' \
  --data '{
    "requestId": "7353b035-90e7-436c-93df-faa9e1817d74",
    "resultCode": "1",
    "resultDesc": "Success!",
    "txnId": "63d57f38-99dc-4c53-b05c-57dd24ec386d"
  }'
```

---

## 3.2.14 API nhận yêu cầu rút lại đồng ý chia sẻ / xoá dữ liệu (`withdraw-consent`)

`POST` `https://{{SCEX_API_ROOT}}/api/v1/vneid/callbacks/withdraw-consent`

**Mô tả:** Callback từ Nền tảng định danh khi công dân yêu cầu rút lại sự đồng ý chia sẻ dữ liệu hoặc yêu cầu SCEX xoá dữ liệu đã chia sẻ.

**Hệ thống xây dựng:** SCEX (C06 là client gọi vào). URL do SCEX định nghĩa và cấu hình sẵn trong Agent Gateway.

**Tham chiếu contract:** `tcdn_spec_api.md` mục III.4.1 · mục II.4

**Authentication:** `api_key` do SCEX cấp cho C06 + allowlist IP Agent Gateway

#### Headers

| Key            | Type     | Required | Description                                                        |
| :------------- | :------- | :------: | :----------------------------------------------------------------- |
| `Content-Type` | `String` | **Yes**  | `application/json; charset=utf-8`                                  |
| `api_key`      | `String` | **Yes**  | API key do SCEX cung cấp cho C06                                   |
| `org_code`     | `String` | **Yes**  | `{{ORG_CODE}}`. Mã TCDN của SCEX do Nền tảng cấp                   |
| `service_code` | `String` | **Yes**  | Giá trị cố định `withdraw-consent`                                 |
| `request_id`   | `String` | **Yes**  | Mã yêu cầu do Nền tảng sinh. SCEX PHẢI lưu lại để dùng ở mục 3.2.9 |
| `request_time` | `String` | **Yes**  | `ddMMyyyyHHmmss`                                                   |

#### Request Body

| Field             | Type     | Required | Description                                                                                                                     | Constraints |
| :---------------- | :------- | :------: | :------------------------------------------------------------------------------------------------------------------------------ | :---------- |
| `transactionCode` | `String` | **Yes**  | Mã giao dịch cần rút lại đồng ý / xoá dữ liệu. Chính là mã SCEX đã dùng khi `init` (3.2.5) và `get-transaction` (3.2.6 / 3.2.7) | —           |

**Example Request:**

```json
{
  "transactionCode": "22fc12e0-be90-4519-8ef6-cdeca95ae22b"
}
```

#### Responses

**`200 OK`** — SCEX chưa gọi `get-transaction` (chưa lấy dữ liệu công dân)

```json
{
  "statusCode": 1,
  "requestStatus": "DELETED"
}
```

**`200 OK`** — SCEX đã gọi `get-transaction` (đã lấy dữ liệu công dân)

```json
{
  "statusCode": 1,
  "requestStatus": "PROCESSING"
}
```

**`200 OK`** — Không thành công

```json
{
  "statusCode": 0,
  "errorDetail": "Không tìm thấy giao dịch tương ứng với transactionCode"
}
```

| Field           | Type           | Required | Description                                                                      |
| :-------------- | :------------- | :------: | :------------------------------------------------------------------------------- |
| `statusCode`    | `number(1)`    | **Yes**  | `1` thành công, `0` không thành công                                             |
| `requestStatus` | `string(100)`  |    No    | `DELETED` đã rút lại đồng ý thành công · `PROCESSING` đã tiếp nhận và đang xử lý |
| `errorDetail`   | `string(4000)` |    No    | Mô tả chi tiết lỗi, bắt buộc khi `statusCode = 0`                                |

#### Ràng buộc từ C06

- Logic quyết định `requestStatus`:
  - Nếu SCEX chưa gọi `get-transaction` cho `transactionCode` này → trả `DELETED`. Luồng dừng tại đây, KHÔNG ĐƯỢC gọi mục 3.2.9.
  - Nếu SCEX đã gọi `get-transaction` → trả `PROCESSING`, xử lý xoá dữ liệu trong tối đa 72 giờ, rồi gọi mục 3.2.9 với `DELETED` hoặc `REJECT`.
- SCEX PHẢI lưu `request_id` nhận được trong header. Mục 3.2.9 PHẢI tham chiếu đúng giá trị này.
- Endpoint PHẢI idempotent theo `transactionCode`: công dân CÓ THỂ gửi lại yêu cầu, C06 CÓ THỂ retry. Lần gọi lại PHẢI trả cùng `requestStatus` mà không tạo thêm tiến trình xoá mới.
- PHẢI trả HTTP 200 kể cả khi nghiệp vụ lỗi. Lỗi thể hiện qua `statusCode = 0` + `errorDetail`.
- Cơ sở pháp lý: Nghị định 13/2023/NĐ-CP về quyền rút lại sự đồng ý của chủ thể dữ liệu.

#### Quyết định của SCEX

- SCEX PHẢI theo dõi trạng thái `get-transaction` theo từng `transactionCode` để trả đúng `DELETED` hay `PROCESSING`. Đây là yêu cầu lưu trữ trạng thái bắt buộc, không suy ra được từ dữ liệu nghiệp vụ.
- SCEX PHẢI công bố link "Công bố thông tin rủi ro khi công dân rút lại sự đồng ý chia sẻ dữ liệu" (xem mục 3.2.0.2).

**Chưa chốt (OI-11):** Phạm vi áp dụng rút lại đồng ý cần C06 xác nhận: consent chưa `get-transaction`, đồng ý dài hạn SSO "Không hỏi lại", và/hoặc yêu cầu xoá dữ liệu đã chia sẻ. Xem `scex-rar.md` mục 2.5.

#### Example cURL

```bash
curl --location 'https://{{SCEX_API_ROOT}}/api/v1/vneid/callbacks/withdraw-consent' \
  --header 'Content-Type: application/json; charset=utf-8' \
  --header 'api_key: <api_key SCEX cấp cho C06>' \
  --header 'org_code: {{ORG_CODE}}' \
  --header 'service_code: withdraw-consent' \
  --header 'request_id: 5e8c1b47-9f30-4a2d-b6e1-7c04a9f2d813' \
  --header 'request_time: 12082026092000' \
  --data '{
    "transactionCode": "22fc12e0-be90-4519-8ef6-cdeca95ae22b"
  }'
```

---

# NHÓM C — API nội bộ SCEX (App / Web ↔ SCEX Backend)

Nhóm C là contract nội bộ giữa SCEX App/Web và SCEX Backend. Endpoint trong nhóm này KHÔNG ĐƯỢC phơi ra C06. SCEX tự định nghĩa path, field và mã trạng thái.

**Base URL:** `https://{{SCEX_API_ROOT}}`

### Quy ước áp dụng cho toàn bộ nhóm C

Các quy ước dưới đây là lựa chọn thiết kế của SCEX, không phải yêu cầu từ C06.

Naming dùng `camelCase` cùng kiểu field của C06 để Backend không phải map tên field hai lần khi gọi nhóm A rồi trả về client. Envelope, timestamp, idempotency và correlation thống nhất với mục 3.2.0.4 để App/Web xử lý lỗi và đối soát log theo một pattern duy nhất.

Endpoint 3.2.23 trả `202 Accepted` và xử lý bất đồng bộ vì luồng gồm nhiều bước gọi ra Agent Gateway (`get-transaction`, nghiệp vụ nội bộ, `receive-result`). Giữ request đồng bộ sẽ vượt ngưỡng timeout phía App và dễ sót bước trả kết quả về C06.

| Hạng mục        | Quy ước                                                                                   |
| --------------- | ----------------------------------------------------------------------------------------- |
| Naming          | `camelCase` cho field body, đồng nhất với field của C06, tránh phải map 2 lần             |
| Envelope        | Theo mục 3.2.0.4 (`code` / `message` / `data`, lỗi có `error` / `details` / `request_id`) |
| Timestamp       | ISO 8601 UTC (ví dụ `2026-08-12T09:15:00Z`)                                               |
| Idempotency     | Mọi `POST` khởi tạo giao dịch PHẢI hỗ trợ header `Idempotency-Key`                        |
| Correlation     | Mọi response PHẢI trả `request_id` để đối soát log xuyên hệ thống                         |
| Authentication  | `Authorization: Bearer <scex_jwt>`, trừ các endpoint đánh dấu Public                      |
| Endpoint Public | 3.2.15, 3.2.16, 3.2.17 — PHẢI có rate limit và chống bruteforce                           |

### Ràng buộc bảo mật bắt buộc

Token VNeID (`access_token` / `refresh_token` / `id_token`) KHÔNG ĐƯỢC rời khỏi SCEX Backend. Client chỉ nhận SCEX token. Lý do: token VNeID gắn quyền gọi Agent Gateway; nếu lộ về App/Web thì mất kiểm soát biên bảo mật và không thu hồi được theo vòng đời phiên SCEX.

`userInfo` dạng JWS của C06 cũng KHÔNG ĐƯỢC trả nguyên văn về client. Backend PHẢI verify chữ ký, parse, rồi lọc field theo nhu cầu màn hình trước khi trả.

- PII (`citizenPid`, `fullName`, `birthDate`, ảnh mặt, `faceImage`) KHÔNG ĐƯỢC log raw. PHẢI masking theo chính sách bảo vệ dữ liệu cá nhân của SCEX (xem `scex-rar.md` mục 6.2).
- `state` PHẢI sinh bằng `SecureRandom` (không random từ tệp số có sẵn hay từ datetime), tối thiểu 16 byte, TTL 300 giây, chỉ dùng 1 lần (xem `scex-rar.md` mục 2.1).

---

## 3.2.15 Khởi tạo phiên đăng nhập VNeID (Mobile App)

`POST` `https://{{SCEX_API_ROOT}}/api/v1/auth/vneid/sessions`

**Mô tả:** App yêu cầu Backend khởi tạo phiên đăng nhập SSO. Backend sinh `state`, lưu lại, và trả universal link để App mở sang VNeID.

**Hệ thống xây dựng:** SCEX Backend (client: SCEX Mobile App)

**Luồng liên quan:** `scex-rar.md` mục 2.1 (Luồng đăng nhập APP)

**Authentication:** Public (chưa đăng nhập)

#### Headers

| Key               | Type     | Required | Description                                               |
| :---------------- | :------- | :------: | :-------------------------------------------------------- |
| `Content-Type`    | `String` | **Yes**  | `application/json`                                        |
| `X-Device-Id`     | `String` | **Yes**  | Định danh thiết bị, dùng cho rate limit và chống lạm dụng |
| `X-Platform`      | `String` | **Yes**  | `ios` \| `android`                                        |
| `Idempotency-Key` | `String` |    No    | UUID; trả lại cùng `state` nếu gọi lại trong TTL          |

#### Request Body

| Field     | Type     | Required | Description              | Constraints                          |
| :-------- | :------- | :------: | :----------------------- | :----------------------------------- |
| `purpose` | `String` |    No    | Mục đích phiên đăng nhập | `login` (mặc định) \| `link-account` |

**Example Request:**

```json
{
  "purpose": "login"
}
```

#### Responses

**`201 Created`** — Khởi tạo thành công

```json
{
  "code": 201,
  "message": "Success",
  "data": {
    "state": "8f14e45fceea167a5a36dedd4bea2543",
    "expiresAt": "2026-08-12T09:20:00Z",
    "authorizeUrl": "https://universal.dancuquocgia.com/login-sso/{{CLIENT_ID}}?state=8f14e45fceea167a5a36dedd4bea2543"
  },
  "request_id": "req_7f3a91c4"
}
```

| Field          | Type     | Description                                            |
| :------------- | :------- | :----------------------------------------------------- |
| `state`        | `String` | Giá trị chống CSRF; App PHẢI gửi lại ở mục 3.2.16      |
| `expiresAt`    | `String` | Thời điểm `state` hết hạn (ISO 8601 UTC), TTL 300 giây |
| `authorizeUrl` | `String` | Universal link để App mở sang VNeID                    |

**`429 Too Many Requests`**

```json
{
  "code": 429,
  "error": "Too Many Requests",
  "message": "Rate limit exceeded. Try again in 60 seconds.",
  "request_id": "req_7f3a91c4"
}
```

**`503 Service Unavailable`** — Không dựng được phiên do cấu hình SSO thiếu hoặc VNeID không khả dụng.

#### Quyết định của SCEX

- `state` PHẢI sinh bằng `SecureRandom`, ≥ 16 byte, TTL 300 giây, dùng 1 lần duy nhất.
- Backend PHẢI lưu `state` kèm `X-Device-Id` và `purpose`; ở mục 3.2.16 PHẢI khớp cả 3.
- Rate limit NÊN áp dụng: 10 req/phút/thiết bị, 60 req/phút/IP.
- Nếu VNeID không khả dụng, PHẢI trả `503` để App fallback sang phương thức đăng nhập hiện có (OTP / NFC). KHÔNG ĐƯỢC chặn luồng chính (xem `scex-rar.md` mục 4.3).

#### Example cURL

```bash
curl -X POST 'https://{{SCEX_API_ROOT}}/api/v1/auth/vneid/sessions' \
  -H 'Content-Type: application/json' \
  -H 'X-Device-Id: 6f1a7c2e-9b34-4d05-8a11-c7e2f4b0d913' \
  -H 'X-Platform: ios' \
  -d '{"purpose": "login"}'
```

---

## 3.2.16 Hoàn tất đăng nhập VNeID — đổi `authorizationCode` lấy SCEX token

`POST` `https://{{SCEX_API_ROOT}}/api/v1/auth/vneid/sessions/token`

**Mô tả:** App gửi `authorizationCode` và `state` nhận được từ VNeID. Backend validate `state`, gọi `sso-token` (3.2.2) và `sso-userinfo` (3.2.3), kiểm tra điều kiện nghiệp vụ, rồi cấp SCEX access token.

**Hệ thống xây dựng:** SCEX Backend (client: SCEX Mobile App)

**Luồng liên quan:** `scex-rar.md` mục 2.1 (Luồng đăng nhập APP)

**Authentication:** Public (chưa đăng nhập)

#### Headers

| Key            | Type     | Required | Description                           |
| :------------- | :------- | :------: | :------------------------------------ |
| `Content-Type` | `String` | **Yes**  | `application/json`                    |
| `X-Device-Id`  | `String` | **Yes**  | PHẢI khớp giá trị đã gửi ở mục 3.2.15 |

#### Request Body

| Field               | Type     | Required | Description                                  | Constraints                     |
| :------------------ | :------- | :------: | :------------------------------------------- | :------------------------------ |
| `authorizationCode` | `String` | **Yes**  | Mã `code` VNeID trả về qua app-to-app        | Dùng 1 lần                      |
| `state`             | `String` | **Yes**  | Giá trị nhận từ mục 3.2.15                   | Phải còn hiệu lực, chưa sử dụng |
| `encryptKey`        | `String` |    No    | Giá trị `encrypt_key` VNeID trả kèm (nếu có) | —                               |

**Example Request:**

```json
{
  "authorizationCode": "a0c106c9-26ae-450e-89e7-cc9900f95428",
  "state": "8f14e45fceea167a5a36dedd4bea2543",
  "encryptKey": "OJkjyfkxJ8mIYCGsyvihdEYVJr+UaSBO8ccyL2KiuYESqamq"
}
```

#### Responses

**`200 OK`** — Đăng nhập thành công

```json
{
  "code": 200,
  "message": "Success",
  "data": {
    "accessToken": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9...",
    "refreshToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "tokenType": "Bearer",
    "expiresIn": 3600,
    "user": {
      "userId": "usr_9921a",
      "maskedCitizenPid": "0361******58",
      "fullName": "Nguyễn Thị Huế",
      "accountLevel": 2,
      "kycStatus": "VERIFIED",
      "vneidLinked": true
    }
  },
  "request_id": "req_7f3a91c5"
}
```

**`400 Bad Request`** — `state` không hợp lệ / hết hạn / đã dùng

```json
{
  "code": 400,
  "error": "Invalid State",
  "details": ["state has expired or was already consumed"],
  "request_id": "req_7f3a91c5"
}
```

**`403 Forbidden`** — Không đáp ứng điều kiện nghiệp vụ

```json
{
  "code": 403,
  "error": "Account Not Eligible",
  "details": [
    "VNeID account level must be 2",
    "accountType must be individual"
  ],
  "request_id": "req_7f3a91c5"
}
```

**`502 Bad Gateway`** — Lỗi khi gọi sang Agent Gateway (`sso-token` / `sso-userinfo` thất bại).

#### Quyết định của SCEX

- PHẢI validate `state` trước mọi thứ: tồn tại, chưa dùng, chưa hết TTL, khớp `X-Device-Id`. Sai bất kỳ điều kiện nào thì trả `400` và ghi log cảnh báo bảo mật.
- PHẢI vô hiệu hoá `state` ngay sau khi validate thành công (one-time use), trước khi gọi ra C06.
- PHẢI kiểm tra `accountLevel = 2` và `accountType = 1` (cá nhân); không đạt thì trả `403` (xem `scex-rar.md` mục 1.3).
- PHẢI verify chữ ký JWS trong response của `sso-userinfo` trước khi tin dữ liệu.
- KHÔNG ĐƯỢC trả `access_token` / `refresh_token` / `id_token` của VNeID về App. Token VNeID lưu server-side, gắn với phiên nội bộ.
- `citizenPid` trả về client PHẢI masked; giá trị đầy đủ chỉ dùng nội bộ.
- Rate limit NÊN áp dụng: 10 req/phút/thiết bị.
- `authorizationCode` chỉ dùng 1 lần; PHẢI chống double-submit khi App retry.

#### Example cURL

```bash
curl -X POST 'https://{{SCEX_API_ROOT}}/api/v1/auth/vneid/sessions/token' \
  -H 'Content-Type: application/json' \
  -H 'X-Device-Id: 6f1a7c2e-9b34-4d05-8a11-c7e2f4b0d913' \
  -d '{
    "authorizationCode": "a0c106c9-26ae-450e-89e7-cc9900f95428",
    "state": "8f14e45fceea167a5a36dedd4bea2543"
  }'
```

---

## 3.2.17 Khởi tạo đăng nhập VNeID (Web — HTTP 302)

`GET` `https://{{SCEX_API_ROOT}}/api/v1/auth/vneid/login`

**Mô tả:** Web App điều hướng người dùng tới endpoint này. Backend tạo và lưu `state` vào session, rồi trả `HTTP 302` sang trang đăng nhập VNeID.

**Hệ thống xây dựng:** SCEX Backend (client: SCEX Web)

**Luồng liên quan:** `scex-rar.md` mục 2.1 (Luồng đăng nhập WEB)

**Authentication:** Public (chưa đăng nhập)

#### Headers

| Key      | Type     | Required | Description                              |
| :------- | :------- | :------: | :--------------------------------------- |
| `Cookie` | `String` |    No    | Session cookie hiện có (nếu đã có phiên) |

#### Path / Query Parameters

| Field       | Type     | Required | Description                                                  |
| :---------- | :------- | :------: | :----------------------------------------------------------- |
| `returnUrl` | `String` |    No    | Đường dẫn nội bộ để redirect về sau khi đăng nhập thành công |
| `purpose`   | `String` |    No    | `login` (mặc định) \| `link-account`                         |

#### Responses

**`302 Found`** — Điều hướng sang VNeID

```
HTTP/1.1 302 Found
Location: {{URL_SSO}}/auth?response_type=code&client_id={{CLIENT_ID}}&scope=openid&redirect_uri={{SCEX_REDIRECT_URI}}&state=8f14e45fceea167a
Set-Cookie: scex_sso_state=...; HttpOnly; Secure; SameSite=Lax; Max-Age=300
```

**`400 Bad Request`** — `returnUrl` không hợp lệ

```json
{
  "code": 400,
  "error": "Validation Failed",
  "details": ["returnUrl must be a relative path within the SCEX domain"],
  "request_id": "req_7f3a91c6"
}
```

**`503 Service Unavailable`** — Cấu hình SSO thiếu hoặc VNeID không khả dụng.

#### Quyết định của SCEX

- `state` PHẢI lưu vào server-side session (hoặc cookie `HttpOnly; Secure; SameSite=Lax` có ký), TTL 300 giây, dùng 1 lần.
- `returnUrl` PHẢI validate là đường dẫn tương đối trong domain của SCEX (chống open redirect). KHÔNG ĐƯỢC nhận URL tuyệt đối từ bên ngoài.
- Rate limit NÊN áp dụng: 30 req/phút/IP.
- Nếu VNeID không khả dụng, NÊN redirect về trang đăng nhập thường của SCEX kèm thông báo; KHÔNG ĐƯỢC chặn luồng chính.

#### Example cURL

```bash
curl -i 'https://{{SCEX_API_ROOT}}/api/v1/auth/vneid/login?returnUrl=%2Fdashboard'
```

---

## 3.2.18 Đăng xuất VNeID (Web — HTTP 302)

`POST` `https://{{SCEX_API_ROOT}}/api/v1/auth/vneid/logout`

**Mô tả:** Web App yêu cầu đăng xuất. Backend tạo `state`, trả `HTTP 302` sang URL đăng xuất của VNeID. Chỉ áp dụng Web; App mobile không có luồng đăng xuất qua VNeID.

**Hệ thống xây dựng:** SCEX Backend (client: SCEX Web)

**Luồng liên quan:** `scex-rar.md` mục 2.1 (Luồng đăng xuất, chỉ Web)

**Authentication:** Required

#### Headers

| Key             | Type     | Required | Description                 |
| :-------------- | :------- | :------: | :-------------------------- |
| `Authorization` | `String` | **Yes**  | `Bearer <scex_jwt>`         |
| `Cookie`        | `String` | **Yes**  | Session cookie của SCEX Web |

#### Responses

**`302 Found`** — Điều hướng sang trang đăng xuất VNeID

```
HTTP/1.1 302 Found
Location: {{URL_LOGOUT}}?client_id={{CLIENT_ID}}&redirect_uri={{SCEX_LOGOUT_REDIRECT_URI}}&state=8f14e45fceea167a
```

**`401 Unauthorized`** — Token không hợp lệ hoặc đã hết hạn

```json
{
  "code": 401,
  "error": "Unauthorized",
  "message": "Invalid or missing token",
  "request_id": "req_7f3a91c7"
}
```

#### Quyết định của SCEX

- Phiên nội bộ của SCEX PHẢI được xoá dù VNeID có đăng xuất thành công hay không (xử lý ở mục 3.2.11).
- Chỉ áp dụng Web SSO. Client mobile gọi endpoint đăng xuất nội bộ thông thường, không qua VNeID.
- PHẢI revoke cả `refreshToken` của SCEX và xoá token VNeID đang giữ server-side.

#### Example cURL

```bash
curl -i -X POST 'https://{{SCEX_API_ROOT}}/api/v1/auth/vneid/logout' \
  -H 'Authorization: Bearer <scex_jwt>' \
  -H 'Cookie: scex_session=...'
```

---

## 3.2.19 Khởi tạo giao dịch định danh / xác thực với VNeID

`POST` `https://{{SCEX_API_ROOT}}/api/v1/identity/vneid/transactions`

**Mô tả:** App/Web yêu cầu Backend khởi tạo giao dịch chia sẻ thông tin / xác thực sinh trắc học / xác thực thông tin công dân. Backend gọi `init` (mục 3.2.5) ra Agent Gateway và trả `transactionId` kèm universal link sang VNeID.

**Hệ thống xây dựng:** SCEX Backend (client: SCEX Mobile App / SCEX Web)

**Luồng liên quan:** `scex-rar.md` mục 2.3, 2.4

**Authentication:** Required

#### Headers

| Key               | Type     | Required | Description                                      |
| :---------------- | :------- | :------: | :----------------------------------------------- |
| `Authorization`   | `String` | **Yes**  | `Bearer <scex_jwt>`                              |
| `Content-Type`    | `String` | **Yes**  | `application/json`                               |
| `Idempotency-Key` | `String` | **Yes**  | UUID; chống tạo trùng giao dịch khi client retry |

#### Request Body

| Field          | Type     | Required | Description                                                   | Constraints                                                                |
| :------------- | :------- | :------: | :------------------------------------------------------------ | :------------------------------------------------------------------------- |
| `serviceCode`  | `String` | **Yes**  | Loại dịch vụ cần dùng                                         | `share-info` \| `face-matching` \| `biometric-share-info` \| `verify-info` |
| `citizenPid`   | `String` | **Yes**  | Số định danh cá nhân                                          | 12 ký tự số                                                                |
| `fullName`     | `String` |    No    | Họ và tên đầy đủ                                              | Max 100 ký tự                                                              |
| `birthDate`    | `String` |    No    | Ngày sinh                                                     | `dd-MM-yyyy`                                                               |
| `verifyFields` | `Object` |    No    | Chỉ dùng với `verify-info`; các field cần đối khớp true/false | Chỉ chứa field đã đăng ký trong cấu hình dịch vụ với C06                   |

**Example Request:**

```json
{
  "serviceCode": "biometric-share-info",
  "citizenPid": "036188001958",
  "fullName": "Nguyễn Thị Huế",
  "birthDate": "24-04-1988"
}
```

**Example Request — `verify-info`:**

```json
{
  "serviceCode": "verify-info",
  "citizenPid": "036188001958",
  "verifyFields": {
    "fullName": "Nguyễn Thị Huế",
    "birthDate": "24-04-1988",
    "permanentCityCode": "01"
  }
}
```

#### Responses

**`201 Created`** — Khởi tạo thành công

```json
{
  "code": 201,
  "message": "Success",
  "data": {
    "transactionId": "9baf1668-a0da-4f9a-b9ac-5eef6f5da5f6",
    "serviceCode": "biometric-share-info",
    "status": "INITIATED",
    "vneidUrl": "https://universal.dancuquocgia.com/share/9baf1668-a0da-4f9a-b9ac-5eef6f5da5f6",
    "expiresAt": "2026-08-12T09:45:00Z"
  },
  "request_id": "req_7f3a91c8"
}
```

**`400 Bad Request`** — Dữ liệu không hợp lệ

```json
{
  "code": 400,
  "error": "Validation Failed",
  "details": [
    "citizenPid must be 12 numeric characters",
    "serviceCode is not supported"
  ],
  "request_id": "req_7f3a91c8"
}
```

**`409 Conflict`** — Đã có giao dịch đang mở cho cùng `citizenPid` + `serviceCode`

```json
{
  "code": 409,
  "error": "Transaction In Progress",
  "details": [
    "An active transaction already exists for this citizen and service"
  ],
  "data": { "transactionId": "9baf1668-a0da-4f9a-b9ac-5eef6f5da5f6" },
  "request_id": "req_7f3a91c8"
}
```

**`422 Unprocessable Entity`** — C06 từ chối khởi tạo (`responseCode` khác `0`)

```json
{
  "code": 422,
  "error": "Provider Rejected",
  "details": ["-20: Thông tin công dân không đúng"],
  "request_id": "req_7f3a91c8"
}
```

**`502 Bad Gateway`** — Không gọi được Agent Gateway.

#### Ràng buộc từ C06

- Giao dịch có TTL 30 phút. `expiresAt` PHẢI phản ánh đúng ràng buộc này.
- Với `responseCode = -17` (thông tin công dân không hợp lệ), giao dịch hết hiệu lực ngay.

#### Quyết định của SCEX

- `Idempotency-Key` bắt buộc: cùng key trong 30 phút PHẢI trả lại đúng `transactionId` đã tạo, không gọi `init` lần nữa.
- Backend PHẢI map `responseCode` của C06 sang HTTP status + `details` có ý nghĩa; KHÔNG ĐƯỢC đẩy nguyên mã lỗi C06 xuống client mà không giải thích (xem mục 3.2.0.6).
- Khi nhận `responseCode = -17`, Backend KHÔNG ĐƯỢC cho client retry với cùng `citizenPid`.
- Nếu SCEX bật cấu hình nhận trạng thái `INIT` (mục 3.2.13), Backend CHỈ trả `vneidUrl` sau khi nhận được callback `INIT`, hoặc trả kèm cờ để client chờ. Cần chốt phương án theo kết quả confirm ở mục 3.2.13.
- `citizenPid` gửi lên PHẢI khớp với chủ tài khoản đang đăng nhập (chống lấy định danh của người khác).
- KHÔNG ĐƯỢC log `citizenPid` raw.

#### Example cURL

```bash
curl -X POST 'https://{{SCEX_API_ROOT}}/api/v1/identity/vneid/transactions' \
  -H 'Authorization: Bearer <scex_jwt>' \
  -H 'Content-Type: application/json' \
  -H 'Idempotency-Key: 4d9f2a10-6c31-4b8e-9f02-1a7c3e5b8d64' \
  -d '{
    "serviceCode": "biometric-share-info",
    "citizenPid": "036188001958",
    "fullName": "Nguyễn Thị Huế",
    "birthDate": "24-04-1988"
  }'
```

---

## 3.2.20 Validate giao dịch khi App được VNeID mở lại

`POST` `https://{{SCEX_API_ROOT}}/api/v1/identity/vneid/transactions/{transactionId}/validate`

**Mô tả:** Sau khi VNeID switch App-to-App về SCEX (mục 3.2.12), App gọi endpoint này để Backend xác thực `transactionId` thuộc về phiên hiện tại và còn hiệu lực, trước khi cho phép bước tiếp theo.

**Hệ thống xây dựng:** SCEX Backend (client: SCEX Mobile App / SCEX Web)

**Luồng liên quan:** `scex-rar.md` mục 2.3, 2.4

**Authentication:** Required

#### Headers

| Key             | Type     | Required | Description         |
| :-------------- | :------- | :------: | :------------------ |
| `Authorization` | `String` | **Yes**  | `Bearer <scex_jwt>` |
| `Content-Type`  | `String` | **Yes**  | `application/json`  |

#### Path / Query Parameters

| Field           | Type     | Required | Description                         |
| :-------------- | :------- | :------: | :---------------------------------- |
| `transactionId` | `String` | **Yes**  | Mã giao dịch nhận được từ deep link |

#### Request Body

| Field    | Type     | Required | Description                                 | Constraints       |
| :------- | :------- | :------: | :------------------------------------------ | :---------------- |
| `result` | `String` |    No    | Giá trị `result` nhận từ deep link (nếu có) | `1` \| `2` \| `3` |

**Example Request:**

```json
{
  "result": "1"
}
```

#### Responses

**`200 OK`** — Giao dịch hợp lệ, cho phép bước tiếp theo

```json
{
  "code": 200,
  "message": "Success",
  "data": {
    "transactionId": "9baf1668-a0da-4f9a-b9ac-5eef6f5da5f6",
    "serviceCode": "biometric-share-info",
    "status": "CONSENT_GRANTED",
    "nextAction": "SUBMIT_BIOMETRICS",
    "expiresAt": "2026-08-12T09:45:00Z"
  },
  "request_id": "req_7f3a91c9"
}
```

| `status`          | Ý nghĩa                              |
| :---------------- | :----------------------------------- |
| `INITIATED`       | Đã khởi tạo, chưa có kết quả consent |
| `CONSENT_GRANTED` | Công dân đã đồng ý chia sẻ           |
| `CONSENT_DENIED`  | Công dân không đồng ý chia sẻ        |
| `EXPIRED`         | Giao dịch hết hiệu lực               |
| `COMPLETED`       | Đã lấy và xử lý xong dữ liệu         |
| `FAILED`          | Giao dịch thất bại                   |

| `nextAction`        | Ý nghĩa                                          |
| :------------------ | :----------------------------------------------- |
| `SUBMIT_BIOMETRICS` | Cần thực hiện sinh trắc học qua SDK (mục 3.2.21) |
| `FETCH_RESULT`      | Có thể tra kết quả ngay (mục 3.2.22)             |
| `NONE`              | Không có bước tiếp theo                          |

**`403 Forbidden`** — Giao dịch không thuộc người dùng đang đăng nhập

```json
{
  "code": 403,
  "error": "Forbidden",
  "message": "Transaction does not belong to the current session",
  "request_id": "req_7f3a91c9"
}
```

**`404 Not Found`** — Không tìm thấy giao dịch

**`410 Gone`** — Giao dịch đã hết hiệu lực (quá 30 phút)

```json
{
  "code": 410,
  "error": "Transaction Expired",
  "message": "Transaction expired. Please start a new one.",
  "request_id": "req_7f3a91c9"
}
```

#### Ràng buộc từ C06

- Giao dịch hết hiệu lực sau 30 phút; khi quá hạn PHẢI trả `410 Gone`.

#### Quyết định của SCEX

- KHÔNG ĐƯỢC tin `result` từ deep link. Trạng thái thật lấy từ callback (mục 3.2.13) hoặc từ `get-transaction`. `result` chỉ dùng để tối ưu UX (hiển thị sớm), không dùng làm căn cứ nghiệp vụ.
- PHẢI kiểm tra `transactionId` thuộc về `userId` trong JWT (chống IDOR).
- KHÔNG ĐƯỢC tiết lộ thông tin nội bộ trong `404` vượt mức cần thiết; không phân biệt "không tồn tại" và "không thuộc bạn" ra ngoài mức cần thiết.

#### Example cURL

```bash
curl -X POST 'https://{{SCEX_API_ROOT}}/api/v1/identity/vneid/transactions/9baf1668-a0da-4f9a-b9ac-5eef6f5da5f6/validate' \
  -H 'Authorization: Bearer <scex_jwt>' \
  -H 'Content-Type: application/json' \
  -d '{"result": "1"}'
```

---

## 3.2.21 Submit dữ liệu sinh trắc học / hoàn tất giao dịch

`POST` `https://{{SCEX_API_ROOT}}/api/v1/identity/vneid/transactions/{transactionId}/biometrics`

**Mô tả:** App gửi chuỗi dữ liệu ảnh mặt do SDK của TTXT sinh ra. Backend gọi `get-transaction` v2 (mục 3.2.7) để lấy kết quả sinh trắc học và dữ liệu công dân.

**Hệ thống xây dựng:** SCEX Backend (client: SCEX Mobile App)

**Luồng liên quan:** `scex-rar.md` mục 2.3, 2.4

**Authentication:** Required

#### Headers

| Key               | Type     | Required | Description                                            |
| :---------------- | :------- | :------: | :----------------------------------------------------- |
| `Authorization`   | `String` | **Yes**  | `Bearer <scex_jwt>`                                    |
| `Content-Type`    | `String` | **Yes**  | `application/json`                                     |
| `Idempotency-Key` | `String` | **Yes**  | UUID; chống gọi trùng làm tiêu hao quota 3 lần của C06 |

#### Path / Query Parameters

| Field           | Type     | Required | Description  |
| :-------------- | :------- | :------: | :----------- |
| `transactionId` | `String` | **Yes**  | Mã giao dịch |

#### Request Body

| Field         | Type     | Required | Description                              | Constraints                            |
| :------------ | :------- | :------: | :--------------------------------------- | :------------------------------------- |
| `faceImage`   | `String` | **Yes**  | Chuỗi dữ liệu ảnh do SDK của TTXT trả ra | Không phải ảnh raw tự chụp / tự encode |
| `requestType` | `String` | **Yes**  | Loại xác thực đã dùng ở SDK              | `sdk-liveness-match` \| `sdk-match`    |

**Example Request:**

```json
{
  "faceImage": "AAABsWU3NmU2MWQ2YTY0NjRlODMyNzRmNWUxZmVlZWJmNzMy...",
  "requestType": "sdk-liveness-match"
}
```

#### Responses

**`200 OK`** — Xác thực thành công

```json
{
  "code": 200,
  "message": "Success",
  "data": {
    "transactionId": "9baf1668-a0da-4f9a-b9ac-5eef6f5da5f6",
    "status": "COMPLETED",
    "biometric": {
      "result": "MATCHED",
      "faceMatchConfidence": "HIGH",
      "livenessPassed": true,
      "deviceRisk": { "isHighRisk": false }
    },
    "identity": {
      "maskedCitizenPid": "0361******58",
      "fullName": "Nguyễn Thị Huế",
      "birthDate": "24-04-1988",
      "verified": true
    }
  },
  "request_id": "req_7f3a91ca"
}
```

**`200 OK`** — Xác thực không khớp

```json
{
  "code": 200,
  "message": "Success",
  "data": {
    "transactionId": "9baf1668-a0da-4f9a-b9ac-5eef6f5da5f6",
    "status": "FAILED",
    "biometric": {
      "result": "NOT_MATCHED",
      "faceMatchConfidence": "LOW",
      "livenessPassed": true,
      "deviceRisk": { "isHighRisk": true }
    }
  },
  "request_id": "req_7f3a91ca"
}
```

**`400 Bad Request`** — Thiếu / sai định dạng `faceImage` hoặc `requestType`.

**`409 Conflict`** — Giao dịch không ở trạng thái cho phép submit sinh trắc học.

**`410 Gone`** — Giao dịch hết hiệu lực hoặc đã dùng hết 3 lần gọi.

```json
{
  "code": 410,
  "error": "Retry Limit Exceeded",
  "message": "Transaction has reached the maximum of 3 data retrieval attempts.",
  "request_id": "req_7f3a91ca"
}
```

**`502 Bad Gateway`** — Lỗi khi gọi Agent Gateway.

#### Ràng buộc từ C06

- Tối đa 3 lần gọi `get-transaction` trong 30 phút cho mỗi giao dịch. Hết quota thì C06 từ chối; SCEX PHẢI phản ánh bằng `410 Gone`.

#### Quyết định của SCEX

- Backend PHẢI đếm và chặn ở phía SCEX trước khi gọi ra C06, để không đốt quota bằng những lần retry vô nghĩa.
- `Idempotency-Key` bắt buộc: client retry mạng KHÔNG ĐƯỢC tạo lời gọi mới ra C06.
- Backend PHẢI verify chữ ký JWS của `biometricResult` và `userInfo` trước khi tin dữ liệu.
- `faceMatch.score` và `liveness.score` là số liệu nội bộ; KHÔNG ĐƯỢC trả về client, chỉ trả nhãn định tính (`HIGH` / `MEDIUM` / `LOW`). Điểm số lưu lại để admin nghiệp vụ tra cứu (xem `scex-rar.md` mục 6.2).
- Khi `deviceRisk.isHighRisk = true`, PHẢI kích hoạt chính sách rủi ro của SCEX (chặn / xác thực bổ sung / cảnh báo vận hành); KHÔNG ĐƯỢC bỏ qua.
- `faceImage` KHÔNG ĐƯỢC lưu sau khi giao dịch hoàn tất, KHÔNG ĐƯỢC log.
- `faceImage` là payload lớn (chuỗi dữ liệu ảnh từ SDK). NÊN nâng giới hạn body size của API gateway / reverse proxy cho riêng endpoint này, và cân nhắc nén ở tầng transport. Nếu kích thước vượt ngưỡng chấp nhận được, CÓ THỂ để App upload ảnh qua endpoint riêng rồi truyền `faceImageRef`, nhưng PHẢI đảm bảo dữ liệu vẫn nguyên vẹn đúng định dạng SDK sinh ra khi Backend gửi sang C06.
- Với luồng xuất phát từ VNeID, sau khi xử lý nghiệp vụ xong Backend PHẢI gọi `receive-result` (mục 3.2.8).

#### Example cURL

```bash
curl -X POST 'https://{{SCEX_API_ROOT}}/api/v1/identity/vneid/transactions/9baf1668-a0da-4f9a-b9ac-5eef6f5da5f6/biometrics' \
  -H 'Authorization: Bearer <scex_jwt>' \
  -H 'Content-Type: application/json' \
  -H 'Idempotency-Key: b71c4e93-2f85-4a06-9d13-8e5a0c7f2b41' \
  -d '{
    "faceImage": "AAABsWU3NmU2MWQ2YTY0NjRlODMyNzRmNWUxZmVlZWJmNzMy...",
    "requestType": "sdk-liveness-match"
  }'
```

---

## 3.2.22 Tra cứu trạng thái và kết quả giao dịch

`GET` `https://{{SCEX_API_ROOT}}/api/v1/identity/vneid/transactions/{transactionId}`

**Mô tả:** Trả trạng thái hiện tại và kết quả của giao dịch. Dùng cho các dịch vụ không có sinh trắc học (`share-info`, `verify-info`) và để App polling / khôi phục trạng thái sau khi bị ngắt giữa luồng.

**Hệ thống xây dựng:** SCEX Backend (client: SCEX Mobile App / SCEX Web)

**Luồng liên quan:** `scex-rar.md` mục 2.2, 2.3, 2.4, 4.3

**Authentication:** Required

#### Headers

| Key             | Type     | Required | Description         |
| :-------------- | :------- | :------: | :------------------ |
| `Authorization` | `String` | **Yes**  | `Bearer <scex_jwt>` |

#### Path / Query Parameters

| Field           | Type      | Required | Description                                                       |
| :-------------- | :-------- | :------: | :---------------------------------------------------------------- |
| `transactionId` | `String`  | **Yes**  | Mã giao dịch (path)                                               |
| `includeResult` | `Boolean` |    No    | `true` để trả kèm dữ liệu định danh đã đối khớp. Mặc định `false` |

#### Responses

**`200 OK`** — Dịch vụ chia sẻ thông tin (`share-info`)

```json
{
  "code": 200,
  "message": "Success",
  "data": {
    "transactionId": "087f0479-98e8-4f37-a312-a7e24399e26a",
    "serviceCode": "share-info",
    "status": "COMPLETED",
    "createdAt": "2026-08-12T09:15:00Z",
    "completedAt": "2026-08-12T09:18:22Z",
    "identity": {
      "maskedCitizenPid": "0361******58",
      "fullName": "Nguyễn Thị Huế",
      "birthDate": "24-04-1988",
      "genderCode": "3",
      "nationalityCode": "VN",
      "permanentAddress": "Khu Đô Thị Hai Bên Đường Lê Trọng Tấn",
      "livingPlaceAddress": "Khu Đô Thị Hai Bên Đường Lê Trọng Tấn",
      "idCardExpireDate": "30-12-2030"
    }
  },
  "request_id": "req_7f3a91cb"
}
```

**`200 OK`** — Dịch vụ xác thực thông tin (`verify-info`); kết quả đối khớp

```json
{
  "code": 200,
  "message": "Success",
  "data": {
    "transactionId": "087f0479-98e8-4f37-a312-a7e24399e26a",
    "serviceCode": "verify-info",
    "status": "COMPLETED",
    "verifyResult": {
      "fullName": true,
      "birthDate": true,
      "nationalityCode": false,
      "citizenPid": true,
      "permanentCityCode": true
    },
    "sharedFields": {
      "dateOfIssue": "23-07-2024",
      "issuingAuthority": "Cục Cảnh sát quản lý hành chính về trật tự xã hội"
    }
  },
  "request_id": "req_7f3a91cb"
}
```

**`200 OK`** — Công dân từ chối chia sẻ

```json
{
  "code": 200,
  "message": "Success",
  "data": {
    "transactionId": "087f0479-98e8-4f37-a312-a7e24399e26a",
    "status": "CONSENT_DENIED",
    "reason": "Công dân không đồng ý chia sẻ thông tin"
  },
  "request_id": "req_7f3a91cb"
}
```

**`403 Forbidden`** — Giao dịch không thuộc người dùng đang đăng nhập.

**`404 Not Found`** — Không tìm thấy giao dịch.

**`429 Too Many Requests`** — Vượt giới hạn polling.

#### Ràng buộc từ C06

- Khi Backend cần gọi `get-transaction`, PHẢI tôn trọng quota tối đa 3 lần / 30 phút.

#### Quyết định của SCEX

- Endpoint này KHÔNG ĐƯỢC gọi ra C06 mỗi lần được gọi. Backend đọc từ trạng thái đã lưu; chỉ gọi `get-transaction` khi trạng thái là `CONSENT_GRANTED` và chưa lấy dữ liệu.
- Rate limit polling NÊN áp dụng: 12 req/phút/giao dịch (5 giây/lần).
- PHẢI kiểm tra quyền sở hữu giao dịch theo `userId` trong JWT.
- `citizenPid` trả về PHẢI luôn masked. Chỉ trả các field tối thiểu mà màn hình cần; KHÔNG ĐƯỢC dump toàn bộ `userInfo`.
- Endpoint này là cơ chế khôi phục trạng thái khi luồng bị ngắt giữa chừng: người dùng không mất dữ liệu đã nhập và CÓ THỂ tiếp tục từ bước dở dang (xem `scex-rar.md` mục 4.3).
- `verifyResult` chỉ trả `true`/`false` cho các field SCEX đã gửi lên; `sharedFields` chứa các field công dân đồng ý chia sẻ thêm.

#### Example cURL

```bash
curl -X GET 'https://{{SCEX_API_ROOT}}/api/v1/identity/vneid/transactions/087f0479-98e8-4f37-a312-a7e24399e26a?includeResult=true' \
  -H 'Authorization: Bearer <scex_jwt>'
```

---

## 3.2.23 Tiếp nhận `consentId` từ luồng xuất phát VNeID

`POST` `https://{{SCEX_API_ROOT}}/api/v1/identity/vneid/consents`

**Mô tả:** Khi công dân chọn dịch vụ SCEX trên ứng dụng VNeID, VNeID switch App-to-App về SCEX kèm `transactionCode` (mục 3.2.12). App gửi mã này cho Backend để Backend gọi `get-transaction`, xử lý nghiệp vụ, rồi trả kết quả về C06 (mục 3.2.8).

**Hệ thống xây dựng:** SCEX Backend (client: SCEX Mobile App)

**Luồng liên quan:** `scex-rar.md` mục 2.2

**Authentication:** Required

#### Headers

| Key               | Type     | Required | Description                                       |
| :---------------- | :------- | :------: | :------------------------------------------------ |
| `Authorization`   | `String` | **Yes**  | `Bearer <scex_jwt>`                               |
| `Content-Type`    | `String` | **Yes**  | `application/json`                                |
| `Idempotency-Key` | `String` | **Yes**  | UUID; chống xử lý trùng và tiêu hao quota của C06 |

#### Request Body

| Field             | Type     | Required | Description                                          | Constraints |
| :---------------- | :------- | :------: | :--------------------------------------------------- | :---------- |
| `transactionCode` | `String` | **Yes**  | Mã giao dịch (consentId) nhận từ deep link của VNeID | —           |

**Example Request:**

```json
{
  "transactionCode": "02d73def-fe15-418c-b97c-37c8214e3ba6"
}
```

#### Responses

**`202 Accepted`** — Đã tiếp nhận, đang xử lý bất đồng bộ

```json
{
  "code": 202,
  "message": "Accepted",
  "data": {
    "transactionCode": "02d73def-fe15-418c-b97c-37c8214e3ba6",
    "status": "PROCESSING",
    "pollUrl": "/api/v1/identity/vneid/transactions/02d73def-fe15-418c-b97c-37c8214e3ba6"
  },
  "request_id": "req_7f3a91cc"
}
```

**`400 Bad Request`** — `transactionCode` sai định dạng.

**`404 Not Found`** — C06 không tìm thấy giao dịch (`responseCode = -9`).

**`409 Conflict`** — `transactionCode` đã được xử lý trước đó

```json
{
  "code": 409,
  "error": "Already Processed",
  "details": ["This consent has already been claimed and processed"],
  "request_id": "req_7f3a91cc"
}
```

**`410 Gone`** — Giao dịch đã hết hiệu lực (quá 30 phút) hoặc hết quota 3 lần.

**`502 Bad Gateway`** — Lỗi khi gọi Agent Gateway.

#### Ràng buộc từ C06

- Quota tối đa 3 lần gọi `get-transaction` / 30 phút áp dụng cho endpoint này.
- Nếu Backend đã gọi `get-transaction` cho `transactionCode` này thì PHẢI gọi `receive-result` (mục 3.2.8) để trả kết quả xử lý về C06. Không trả kết quả sẽ khiến giao dịch của công dân bị treo trên app VNeID và công dân không thực hiện được giao dịch mới.

#### Quyết định của SCEX

- Trả `202 Accepted` và xử lý bất đồng bộ vì luồng gồm nhiều bước gọi ra ngoài (`get-transaction`, nghiệp vụ nội bộ, `receive-result`). App theo dõi qua `pollUrl` (mục 3.2.22).
- Bước `receive-result` PHẢI nằm trong hàng đợi bền có retry, không phụ thuộc vòng đời request HTTP.
- PHẢI idempotent theo `transactionCode`; VNeID CÓ THỂ mở lại app nhiều lần với cùng mã.
- Backend PHẢI đối chiếu `citizenPid` trong dữ liệu nhận được với chủ tài khoản đang đăng nhập. Không khớp thì KHÔNG ĐƯỢC ghép dữ liệu vào tài khoản, và PHẢI trả kết quả `FAILURE` về C06 kèm lý do.
- PHẢI lưu trạng thái "đã gọi `get-transaction`" theo `transactionCode` để phục vụ mục 3.2.14 (quyết định trả `DELETED` hay `PROCESSING`).

#### Example cURL

```bash
curl -X POST 'https://{{SCEX_API_ROOT}}/api/v1/identity/vneid/consents' \
  -H 'Authorization: Bearer <scex_jwt>' \
  -H 'Content-Type: application/json' \
  -H 'Idempotency-Key: e2f9c481-7a35-4b60-9c18-3d05f7a2e916' \
  -d '{
    "transactionCode": "02d73def-fe15-418c-b97c-37c8214e3ba6"
  }'
```

---

## Tổng hợp ràng buộc triển khai

- Timestamp nhóm C (API nội bộ SCEX): ISO 8601 UTC. Timestamp nhóm A/B (contract C06): `ddMMyyyyHHmmss` và ngày tháng `dd-MM-yyyy`. SCEX KHÔNG ĐƯỢC tự chuẩn hoá định dạng khi gọi ra C06.
- Nhóm A và B là contract với bên thứ ba. Mọi thay đổi PHẢI đối chiếu lại `tcdn_spec_api.md` phiên bản tương ứng. Tài liệu này chốt theo v3.1 (15/8/2025).
- Tài liệu này chỉ đặc tả interface. Chi tiết mã hoá / ký số, mô hình bảo mật, và giám sát nằm ở `scex-rar.md` mục 5 và mục 6.

### Ràng buộc thời gian

| Ràng buộc                                    | Giá trị                                                      | Nguồn      |
| -------------------------------------------- | ------------------------------------------------------------ | ---------- |
| `request_time` lệch so với hệ thống Nền tảng | > 2 phút → timeout                                           | C06        |
| `state` TTL                                  | 300 giây, dùng 1 lần                                         | SCEX       |
| `transactionId` hết hạn                      | 30 phút kể từ lúc công dân xác nhận                          | C06        |
| Số lần gọi `get-transaction`                 | Tối đa 3 lần / 30 phút                                       | C06        |
| Hạn mức gọi theo ngày                        | 10.000 lần/ngày trên mỗi cặp (`target_code`, `service_code`) | C06        |
| Callback thông tin công dân không hợp lệ     | < 1–3 giây sau `init` (SCEX chọn ngưỡng chờ 3 giây, OI-07)   | C06 / SCEX |
| Xử lý rút lại đồng ý                         | Tối đa 72 giờ kể từ tiếp nhận                                | C06        |

---

## Phụ lục — Danh sách vấn đề cần chốt với C06

Các mục dưới đây PHẢI được làm rõ trước khi go-live (Blocker / Cao) hoặc trước khi implement phần phụ thuộc (Trung bình). Mã `OI-xx` được dẫn chiếu tại chỗ trong tài liệu.

| Mã    | Mục                 | Vấn đề                                                                                                                          | Mức độ     | Đầu mối            | Ảnh hưởng nếu không chốt                                                              |
| ----- | ------------------- | ------------------------------------------------------------------------------------------------------------------------------- | ---------- | ------------------ | ------------------------------------------------------------------------------------- |
| OI-01 | 3.2.0.1             | Danh mục giá trị `target_code` chưa có tài liệu                                                                                 | Blocker    | C06                | Không gọi được API qua Agent Gateway đúng mục đích hiển thị trên VNeID                |
| OI-02 | Nhóm B              | Cơ chế xác thực chiều C06 → SCEX chỉ có `api_key`; chưa rõ mTLS / ký số bản tin / dải IP nguồn                                  | Cao        | C06 + SCEX Infra   | Rủi ro callback giả mạo; SCEX đang bù bằng allowlist IP + chống replay + mTLS hạ tầng |
| OI-03 | 3.2.8               | Không có đặc tả field chi tiết của `receive-result` cho `share-info` / `biometric-share-info` / `face-matching` / `verify-info` | Blocker    | C06                | Không implement được trả kết quả; giao dịch công dân có thể bị treo                   |
| OI-04 | 3.2.8, 3.2.9        | Danh mục `reasonRequestCode` mà SCEX cần đăng ký trước với C06                                                                  | Blocker    | C06 + SCEX Legal   | Không trả được lý do thất bại / từ chối xoá đúng contract                             |
| OI-05 | 3.2.13              | Mâu thuẫn `responseCode`: API Specs ghi `1 = không thành công`, bảng đặc tả ghi `0 = thành công`. SCEX tạm lấy theo bảng đặc tả | Cao        | C06                | Sai mã phản hồi → C06 coi SCEX xử lý lỗi                                              |
| OI-06 | 3.2.13              | SCEX chọn Trường hợp 1 (nhận `INIT`) hay Trường hợp 2 (không nhận) theo mục II.3                                                | Cao        | SCEX Product + C06 | Ảnh hưởng trực tiếp luồng App (có chờ `INIT` trước khi switch hay không)              |
| OI-07 | 3.2.13              | Ngưỡng callback thông tin công dân không hợp lệ: III.2.2 ghi < 1s, II.3 ghi tối đa 3s. SCEX tạm chọn 3s                         | Trung bình | C06                | Chờ quá ngắn có thể switch sang VNeID dù tài khoản không hợp lệ                       |
| OI-08 | 3.2.4, 3.2.11       | VNeID có xử lý `id_token_hint` và `state` ở URL logout hay không                                                                | Trung bình | C06                | Ảnh hưởng chống CSRF và tối ưu logout Web                                             |
| OI-09 | 3.2.7               | Danh mục mã `deviceRisk.flags`                                                                                                  | Trung bình | C06                | Không map được nguyên nhân thiết bị rủi ro sang chính sách SCEX                       |
| OI-10 | 3.2.7               | Đặc tả SDK của TTXT: chức năng, nền tảng hỗ trợ, cách tích hợp                                                                  | Blocker    | C06                | Không implement được luồng sinh trắc học                                              |
| OI-11 | 3.2.14              | Phạm vi dữ liệu phải xoá khi công dân rút lại đồng ý; phạm vi trường hợp áp dụng cho SCEX                                       | Cao        | C06 + SCEX Legal   | Xung đột với nghĩa vụ lưu trữ 10 năm (Nghị quyết 05/2025/NQ-CP)                       |
| OI-12 | Chung               | Vòng đời `consentId` / `transactionId`; phân biệt `consentId` SSO và luồng chia sẻ thông tin                                    | Trung bình | C06                | Dễ nhầm khoá đối soát giữa các luồng                                                  |
| OI-13 | Chung               | Môi trường UAT có cần Agent Gateway hay không                                                                                   | Trung bình | C06 + SCEX Infra   | Ảnh hưởng môi trường kiểm thử và checklist go-live                                    |
| OI-14 | 3.2.0.7             | Cấu hình dịch vụ: service-code nào bật xác thực ảnh mặt; `verify-info` dùng v1 hay v2                                           | Cao        | C06                | Gọi sai đường dẫn → `responseCode = -8`                                               |
| OI-15 | 3.2.11              | VNeID gửi kèm query param nào ở callback đăng xuất — tài liệu không đặc tả                                                      | Trung bình | C06                | SCEX không thể phụ thuộc tham số callback để xác nhận logout                          |
| OI-16 | 3.2.1, 3.2.2, 3.2.4 | Hành vi và mã lỗi khi công dân từ chối SSO, `code` hết hạn/không hợp lệ, `client_id`/`redirect_uri` sai                         | Trung bình | C06                | Thiếu mapping lỗi → UX và monitoring không đầy đủ                                     |
