# Đặc tả API — Phía SCEX cung cấp (Tích hợp SCEX $\leftrightarrow$ RAR / C06 / VNeID)

| Trường          | Giá trị                                                                |
| --------------- | ---------------------------------------------------------------------- |
| Phiên bản       | `v1`                                                                   |
| Trạng thái      | Draft                                                                  |
| Cập nhật        | 2026-08-12                                                             |
| Tài liệu cha    | `scex-rar.md` — mục 3.2 Endpoint / Payload / Data Mapping              |
| Nguồn tham chiếu | `tcdn_spec_api.md` (v3.1 — 15/8/2025), `rar_sso.md`                    |
| Phạm vi tài liệu | Chỉ các API/URL **do SCEX xây dựng** (Nhóm B và Nhóm C của tài liệu gốc `scex-rar-api-spec.md`). **Không** bao gồm các API do C06/RAR cung cấp (Nhóm A) — SCEX chỉ là bên gọi vào các API đó, xem `scex-rar-api-spec.md` mục 3.2.1–3.2.9 nếu cần tra cứu contract của C06. |

---

## 0. Cách đọc tài liệu này

Tài liệu này chỉ liệt kê các endpoint/URL **do SCEX xây dựng và làm chủ contract**, chia thành 2 nhóm:

| Nhóm  | Mục         | Bên xây dựng | Chiều gọi                     | Nguồn sự thật của contract                          |
| ----- | ----------- | ------------ | ----------------------------- | --------------------------------------------------- |
| **B** | 3.2.10–3.2.14 | SCEX         | C06 → SCEX Backend            | Tên field bám theo `tcdn_spec_api.md`; host/path do SCEX định nghĩa |
| **C** | 3.2.15–3.2.23 | SCEX         | SCEX App/Web → SCEX Backend   | SCEX tự định nghĩa hoàn toàn                        |

Quy ước ký hiệu:

- `{{PLACEHOLDER}}` — giá trị cấu hình theo môi trường, xem mục 3.2.0.
- `{service-code}` — mã dịch vụ, xem bảng Mã dịch vụ mục 3.2.0.
- Dòng `> ⚠️ Chưa có trong tài liệu C06 — cần confirm.` đánh dấu thông tin còn thiếu, phải làm rõ với C06 trước khi implement.

---

## 3.2.0 Thông tin cấu hình kết nối

### 3.2.0.1 Tham số theo môi trường

Bảng dưới đây là **tham số cần điền trước khi triển khai**. Cột "Nguồn" cho biết ai cung cấp giá trị.

| Placeholder                  | Ý nghĩa                                                   | UAT                                        | Production | Nguồn     |
| ---------------------------- | --------------------------------------------------------- | ------------------------------------------ | ---------- | --------- |
| `{{SCEX_API_ROOT}}`          | Root domain API Backend của SCEX                          | `<điền>`                                   | `<điền>`   | SCEX      |
| `{{SCEX_WEB_ROOT}}`          | Root domain Web App của SCEX                              | `<điền>`                                   | `<điền>`   | SCEX      |
| `{{SCEX_UNIVERSAL_LINK}}`    | Universal Link của SCEX Mobile App                        | `<điền>`                                   | `<điền>`   | SCEX      |
| `{{SCEX_REDIRECT_URI}}`      | Redirect URI nhận `code` sau đăng nhập SSO (mục 3.2.10)   | `<điền>`                                   | `<điền>`   | SCEX      |
| `{{SCEX_LOGOUT_REDIRECT_URI}}` | Redirect URI sau đăng xuất VNeID (mục 3.2.11)           | `<điền>`                                   | `<điền>`   | SCEX      |
| `{{AGENT_GW_DOMAIN}}`        | Domain Agent Gateway (đặt tại hạ tầng SCEX)               | `<điền>`                                   | `<điền>`   | C06 / SCEX |
| `{{AGENT_GW_PORT}}`          | Port Agent Gateway                                        | `<điền>`                                   | `<điền>`   | C06 / SCEX |
| `{{URL_SSO}}`                | URL trang đăng nhập VNeID (Web)                           | `https://sso-uatvneid.dancuquocgia.com`    | `<điền>`   | C06       |
| `{{URL_LOGOUT}}`             | URL đăng xuất VNeID (Web)                                 | `https://sso-uatvneid.dancuquocgia.com/api/idp/sso/login-actions/logout` | `<điền>` | C06 |
| `{{UNIVERSAL_LINK_SSO}}`     | Universal Link đăng nhập VNeID (App)                      | `https://universal.dancuquocgia.com/login-sso` | `<điền>` | C06     |
| `{{UNIVERSAL_LINK_SHARE}}`   | Universal Link consent chia sẻ (App)                      | `https://universal.dancuquocgia.com/share` | `N/A`      | C06       |
| `{{CLIENT_ID}}`              | Mã đăng ký của SCEX trên hệ thống SSO VNeID               | `<điền>`                                   | `<điền>`   | C06 cấp   |
| `{{CLIENT_SECRET}}`          | Secret key SSO của SCEX                                   | `<điền>`                                   | `<điền>`   | C06 cấp   |
| `{{API_KEY}}`                | API key gọi qua Agent Gateway                             | `<điền>`                                   | `<điền>`   | C06 cấp   |
| `{{TARGET_CODE}}`            | Mã mục đích chia sẻ / xử lý thông tin                     | `<điền>`                                   | `<điền>`   | C06 cấp   |
| `{{ORG_CODE}}`               | Mã TCDN của SCEX do Nền tảng cấp                          | `<điền>`                                   | `<điền>`   | C06 cấp   |
| `{{ORG_CONNECTION_CODE}}`    | Mã đơn vị kết nối — query param `code`, chỉ môi trường non-prod | `<điền>`                              | `N/A`      | C06 cấp   |
| `{{RSA_PRIVATE_KEY}}`        | Private key RSA của SCEX để giải mã `encrypt_key` / `key` | `<điền>`                                   | `<điền>`   | SCEX sinh, đăng ký public key với C06 |
| `{{C06_JWS_PUBLIC_KEY}}`     | Public key C06 để verify chữ ký JWS của `userInfo`        | `<điền>`                                   | `<điền>`   | C06 cấp   |

> ⚠️ `{{TARGET_CODE}}` hiện chưa có tài liệu mô tả danh mục giá trị — xem `scex-rar.md` Appendix A, Open Issues. Cần C06 cung cấp danh mục trước khi implement.

### 3.2.0.2 Đường link SCEX phải công bố cho VNeID

Tài liệu C06 yêu cầu TCDN cung cấp 2 đường link để hiển thị trên ứng dụng VNeID (mục III.2.1 và III.2.2):

| STT | Nội dung công bố                                                                                                    | URL      | Nguồn |
| --- | ------------------------------------------------------------------------------------------------------------------- | -------- | ----- |
| 1   | "Điều khoản và cam kết sử dụng dữ liệu được chia sẻ"                                                                 | `<điền>` | SCEX  |
| 2   | "Công bố thông tin rủi ro khi công dân rút lại sự đồng ý chia sẻ dữ liệu, tổ chức doanh nghiệp thực hiện xoá dữ liệu" | `<điền>` | SCEX  |

### 3.2.0.3 Header dùng chung khi gọi qua Agent Gateway (áp dụng nhóm A — *tham khảo, không phải API do SCEX xây dựng*)

> Mục này giữ lại làm tham chiếu cho phần "3.2.0.4 Định dạng phản hồi dùng chung" và các quy tắc thời gian ở mục 3.2.0.6 mà Nhóm B/C có phụ thuộc. Chi tiết đầy đủ API nhóm A xem `scex-rar-api-spec.md`.

| Key            | Type     | Required | Description                                                                                                |
| -------------- | -------- | :------: | ---------------------------------------------------------------------------------------------------------- |
| `Content-Type` | `String` | **Yes**  | `application/json; charset=utf-8` (riêng mục 3.2.2 dùng `application/x-www-form-urlencoded`)                |
| `api_key`      | `String` | **Yes**  | `{{API_KEY}}` — do Tổ chức cung cấp dịch vụ cấp                                                            |
| `target_code`  | `String` | **Yes**  | `{{TARGET_CODE}}` — mã mục đích chia sẻ / xử lý thông tin, dùng để VNeID hiển thị đúng nội dung cho công dân |
| `request_id`   | `String` | **Yes**  | Mã yêu cầu do SCEX tự sinh, **duy nhất trên toàn hệ thống** (khuyến nghị UUID v4)                          |
| `request_time` | `String` | **Yes**  | Thời gian yêu cầu, định dạng `ddMMyyyyHHmmss`                                                              |

**Ràng buộc bắt buộc:** nếu `request_time` lệch quá **2 phút** so với thời gian hệ thống Nền tảng, request bị coi là **timeout**. SCEX phải đồng bộ NTP trên toàn bộ node gọi ra Agent Gateway.

### 3.2.0.4 Định dạng phản hồi dùng chung

**Nhóm A — envelope của C06** (áp dụng cho các API `init`, `get-transaction`, `receive-result`):

```json
{
  "responseCode": "0",
  "description": "Success!",
  "responseTime": "12082026091500",
  "data": { }
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

- Nếu `key` **có giá trị** → `data` đã mã hoá AES, SCEX phải giải mã `key` bằng `{{RSA_PRIVATE_KEY}}` rồi giải mã `data`.
- Nếu `key` **không có giá trị / không tồn tại** → `data` trả về ở dạng plaintext.

**Nhóm B / Nhóm C — envelope của SCEX:**

Success:

```json
{
  "code": 200,
  "message": "Success",
  "data": { }
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

> Ngoại lệ: các endpoint nhóm B do C06 gọi vào **không** dùng envelope của SCEX — bắt buộc trả đúng cấu trúc mà tài liệu C06 quy định (`statusCode` / `requestStatus` / `errorDetail`). Xem chi tiết từng mục.

### 3.2.0.5 Mã lỗi HTTP dùng chung (tham chiếu `tcdn_spec_api.md` mục IV.1)

| HTTP Status | Kiểu lỗi                | Nội dung                                                            |
| ----------- | ----------------------- | ------------------------------------------------------------------- |
| `200`       | `SUCCESS`               | Yêu cầu thực hiện thành công                                        |
| `401`       | `ACCESS_TOKEN`          | Token hết hạn (nếu có)                                              |
| `403`       | `FORBIDDEN`             | Không có quyền truy cập dịch vụ                                     |
| `404`       | `NOT_FOUND`             | Địa chỉ yêu cầu không tồn tại                                       |
| `500`       | `INTERNAL SERVER ERROR` | Lỗi trong quá trình xử lý — hệ thống chủ động đưa ra lỗi chi tiết   |
| `501`       | `BIOMETRIC ERROR`       | Lỗi xác thực sinh trắc học — hệ thống chủ động đưa ra lỗi chi tiết  |

Mã lỗi dịch vụ (mục IV.2): `1000` lỗi trong quá trình xử lý · `1001` yêu cầu trùng lặp · `1002` sai định dạng dữ liệu.

### 3.2.0.6 Mã `responseCode` / `resultCode` (tham chiếu mục IV.4)

| Code  | Ý nghĩa                                       | SCEX cần xử lý gì                                                                       |
| ----- | --------------------------------------------- | --------------------------------------------------------------------------------------- |
| `2`   | Công dân **không đồng ý** chia sẻ             | Kết thúc luồng, thông báo cho user; trả về qua API callback (mục 3.2.13)                 |
| `1`   | Công dân **đồng ý** chia sẻ                   | Tiếp tục gọi `get-transaction`                                                          |
| `0`   | Thành công                                    | —                                                                                       |
| `-1`  | Không lấy được thông tin                      | Giao dịch lỗi, khởi tạo lại                                                             |
| `-2`  | Giao dịch đã tồn tại                          | Lỗi nội bộ NT ↔ VNeID — **SCEX không cần xử lý**                                        |
| `-3`  | Giao dịch đã hoàn thành                       | Lỗi nội bộ NT ↔ VNeID — **SCEX không cần xử lý**                                        |
| `-4`  | Giao dịch đang xử lý, thử lại sau             | Đang chờ công dân xác nhận — chờ callback hoặc chờ hết thời gian rồi thực hiện lại        |
| `-5`  | Đầu vào yêu cầu không hợp lệ                  | Kiểm tra lại request body                                                               |
| `-6`  | Không tìm thấy tổ chức                        | Liên hệ C06 kiểm tra cấu hình tổ chức trước khi gọi tiếp                                |
| `-7`  | Không tìm thấy dịch vụ                        | Liên hệ C06 kiểm tra cấu hình dịch vụ                                                   |
| `-8`  | Đường dẫn không hợp lệ                        | Kiểm tra lại URL                                                                        |
| `-9`  | Không tìm thấy mã giao dịch                   | Kiểm tra lại `transactionId` / `consentId`                                              |
| `-10` | Giao dịch đã hoàn tất xác nhận chia sẻ        | Lỗi nội bộ NT ↔ VNeID — **SCEX không cần xử lý**                                        |
| `-11` | Công dân không đồng ý chia sẻ thông tin       | Tương tự mã `2` nhưng trả trong response của `get-transaction` — phải khởi tạo giao dịch mới |
| `-12` | Không tìm thấy cấu hình agent                 | Liên hệ C06 kiểm tra cấu hình Agent Gateway                                             |
| `-14` | Timeout                                       | Hết thời gian chờ xác nhận / trả kết quả — khởi tạo giao dịch mới                        |
| `-15` | Tồn tại trường dữ liệu bắt buộc bị `null`     | **Không** khởi tạo lại cùng dịch vụ + cùng công dân; hướng dẫn công dân kiểm tra thông tin |
| `-16` | Dịch vụ vượt quá hạn mức cho phép             | Liên hệ C06 gia hạn dịch vụ                                                             |
| `-17` | Thông tin công dân không hợp lệ               | Tài khoản mức 1 / không tồn tại / CCCD hết hạn. Giao dịch **hết hiệu lực ngay**. Không khởi tạo lại với cùng thông tin |
| `-18` | Trường đầu vào không tồn tại trong cấu hình   | Đối chiếu lại các field với dịch vụ đã đăng ký                                          |
| `-19` | Tài khoản tổ chức không có quyền dùng dịch vụ | Kiểm tra lại thông tin tài khoản tổ chức                                                |
| `-20` | Thông tin công dân không đúng                 | Dữ liệu gửi lên không khớp CSDL — thử lại chỉ với các field bắt buộc                    |

Trạng thái chia sẻ (dùng ở callback mục 3.2.13): `INIT` — công dân đã nhận màn xác nhận chia sẻ trên VNeID · `SHARED` — đã chia sẻ dữ liệu · `NOT SHARED` — không thành công.

### 3.2.0.7 Ma trận service-code × endpoint

Chỉ liệt kê các service-code **trong phạm vi** của SCEX (xem `scex-rar.md` mục 1.3).

> ⚠️ Các mục 3.2.1–3.2.9 (cột "Khởi tạo" / "Lấy kết quả" dưới đây) thuộc **Nhóm A — API do C06 cung cấp**, đã được lược bỏ khỏi tài liệu này (xem phạm vi ở bảng metadata đầu file). Tra cứu chi tiết các mục đó tại `scex-rar-api-spec.md`.

| Service-code              | Khởi tạo         | Lấy kết quả              | Trả kết quả      | Ghi chú                              |
| ------------------------- | ---------------- | ------------------------ | ---------------- | ------------------------------------ |
| `sso-token`               | —                | 3.2.2 *(Nhóm A)*         | —                | Không dùng `init`/`get-transaction`  |
| `sso-userinfo`            | —                | 3.2.3 *(Nhóm A)*         | —                | Cần `access_token` từ `sso-token`    |
| `share-info`              | 3.2.5 *(Nhóm A)* | **3.2.6** (v1) *(Nhóm A)* | 3.2.8 *(Nhóm A)* | Không có xác thực ảnh mặt            |
| `verify-info`             | 3.2.5 *(Nhóm A)* | **3.2.6** (v1) *hoặc* **3.2.7** (v2) *(Nhóm A)* | 3.2.8 *(Nhóm A)* | Phụ thuộc cấu hình dịch vụ — xem lưu ý dưới |
| `face-matching`           | 3.2.5 *(Nhóm A)* | **3.2.7** (v2) *(Nhóm A)* | 3.2.8 *(Nhóm A)* | Có xác thực ảnh mặt                  |
| `biometric-share-info`    | 3.2.5 *(Nhóm A)* | **3.2.7** (v2) *(Nhóm A)* | 3.2.8 *(Nhóm A)* | Có xác thực ảnh mặt                  |
| `withdraw-consent`        | — (C06 gọi vào)  | —                        | 3.2.14 (response) | Endpoint do SCEX xây dựng — xem mục dưới |
| `withdraw-consent-result` | —                | —                        | 3.2.9 *(Nhóm A)* | Chỉ dùng khi 3.2.14 trả `PROCESSING` |

**Nguyên tắc chọn v1 hay v2 — yếu tố quyết định là dịch vụ có xác thực ảnh mặt hay không, KHÔNG phải bản thân service-code:**

- Dịch vụ **không** bao gồm xác thực ảnh mặt → dùng đường dẫn **không** có `/v2/` (mục 3.2.6 — Nhóm A, `tcdn_spec_api.md` III.2.3).
- Dịch vụ **có** bao gồm xác thực ảnh mặt → dùng đường dẫn có `/v2/` (mục 3.2.7 — Nhóm A, `tcdn_spec_api.md` III.2.4).
- Khởi tạo giao dịch bằng service-code nào thì lấy kết quả theo đúng service-code đó.

`verify-info` xuất hiện ở **cả hai** mục III.2.3 và III.2.4 của tài liệu C06 (mục III.2.4 có biến thể request body "Dịch vụ xác thực thông tin công dân" kèm `faceImage`). Do đó `verify-info` dùng v1 hay v2 phụ thuộc vào việc C06 có cấu hình xác thực ảnh mặt cho dịch vụ này của SCEX hay không.

> ⚠️ Cần confirm với C06: cấu hình dịch vụ cấp cho SCEX — service-code nào có bật xác thực ảnh mặt, cụ thể là `verify-info` đi v1 hay v2. Sai đường dẫn sẽ nhận `responseCode = -8` (đường dẫn không hợp lệ).

---

# NHÓM B — API / URL do SCEX cung cấp cho RAR / C06

SCEX là **bên xây dựng**; C06 (qua Agent Gateway) hoặc ứng dụng VNeID là bên gọi vào / điều hướng vào.

**Base URL:** `https://{{SCEX_API_ROOT}}` · `https://{{SCEX_WEB_ROOT}}` · `{{SCEX_UNIVERSAL_LINK}}`

Contract (tên field, enum, mã trạng thái) **bám theo `tcdn_spec_api.md`** — giữ nguyên `camelCase` của C06, không đổi sang `snake_case`. Host và path do SCEX định nghĩa, và **phải đăng ký trước với C06** để cấu hình vào Agent Gateway.

### Yêu cầu bảo mật tối thiểu cho toàn bộ nhóm B

Đây là các endpoint nhận traffic từ ngoài hệ thống SCEX. Bắt buộc áp dụng:

| Lớp bảo vệ                | Yêu cầu                                                                                          |
| ------------------------- | ------------------------------------------------------------------------------------------------ |
| Mạng                      | Allowlist IP của Agent Gateway; chỉ nhận traffic từ vùng biên SCEX, không expose ra Internet công cộng (trừ redirect URI của Web/App vốn phải public) |
| Kênh truyền               | TLS 1.2+ bắt buộc; khuyến nghị mTLS cho các callback từ Agent Gateway                             |
| Xác thực                  | Verify `api_key` do SCEX cấp cho C06 (chuỗi 32–64 ký tự) trên mọi callback                        |
| Chống replay              | Lưu `request_id` đã xử lý; từ chối `request_id` trùng. Kiểm tra `request_time` trong cửa sổ ±2 phút |
| Idempotency               | Mọi callback phải xử lý idempotent theo `request_id` + `transactionCode` / `txnId`                |
| Rate limit                | Giới hạn theo IP và theo `api_key`                                                                |
| Log & audit               | Ghi log đầy đủ theo `scex-rar.md` mục 6.2; masking PII                                            |

> ⚠️ `tcdn_spec_api.md` **không quy định cơ chế xác thực bắt buộc theo chiều C06 → TCDN** ngoài `api_key` trong header. Không có signature bản tin, không có mTLS bắt buộc. Đây là **rủi ro bảo mật cần confirm với C06**: (1) C06 có hỗ trợ mTLS không; (2) C06 có ký số bản tin callback không; (3) dải IP nguồn của Agent Gateway để allowlist. Nếu chỉ có `api_key`, SCEX phải bù bằng allowlist IP + chống replay + mTLS ở tầng hạ tầng.

---

## 3.2.10 Redirect URI SSO — nhận `code` sau đăng nhập VNeID

`GET` `https://{{SCEX_API_ROOT}}/api/v1/auth/vneid/callback`

**Mô tả:** URL do SCEX xây dựng và đăng ký với C06, để VNeID điều hướng người dùng về kèm mã `code` sau khi xác thực thành công.

**Hệ thống xây dựng:** SCEX (VNeID là bên điều hướng vào)

**Tham chiếu contract:** `tcdn_spec_api.md` mục III.3.2 · `rar_sso.md` mục 4.2

**Authentication:** Public endpoint (điều hướng trình duyệt / app). Bảo vệ bằng `state` + rate limit.

#### Cấu trúc

```
{{SCEX_REDIRECT_URI}}?code=$[code]&encrypt_key=$[encrypt_key]&state=$[state]
```

#### Headers

> Endpoint này không yêu cầu Header đặc thù (điều hướng trình duyệt).

#### Path / Query Parameters

| Field         | Type     | Required | Description                                                                                                     |
| :------------ | :------- | :------: | :-------------------------------------------------------------------------------------------------------------- |
| `code`        | `String` | **Yes**  | Code xác thực thành công, dùng để đổi `access_token` ở mục 3.2.2                                                 |
| `encrypt_key` | `String` |    No    | AES secret key đã được VNeID mã hoá bằng RSA. Nếu **có** giá trị → dữ liệu ở bước sau sẽ được mã hoá; nếu không → plaintext |
| `state`       | `String` |    No*   | Giá trị SCEX đã gửi ở mục 3.2.1, VNeID trả lại nguyên văn                                                        |

\* C06 không bắt buộc; SCEX bắt buộc để chống CSRF.

#### Request Body

> Endpoint này không sử dụng Request Body.

#### Responses

**`302 Found`** — Hợp lệ: SCEX validate `state`, đổi `code` lấy token (mục 3.2.2), lấy userinfo (mục 3.2.3), cấp SCEX token rồi redirect về Web App.

**`302 Found` → trang lỗi** — `state` không hợp lệ / hết hạn / đã dùng, hoặc thiếu `code`.

> Endpoint này **không** trả JSON envelope vì là redirect trên trình duyệt. Với luồng App (mục 3.2.16) SCEX dùng endpoint JSON riêng.

#### Business Rules

- **Không dùng hàm `getQueryParam` có sẵn của ngôn ngữ lập trình** để đọc `encrypt_key`. Đây là cảnh báo tường minh trong `tcdn_spec_api.md` mục III.3.2 và `rar_sso.md` mục 4.2 — giá trị này có thể chứa ký tự làm sai lệch kết quả parse tự động (`+`, `/`, `=`). Phải parse thủ công từ raw query string.
- `state` phải được validate: tồn tại, chưa dùng, chưa hết TTL (300s). Sau khi dùng phải vô hiệu hoá ngay.
- `code` chỉ dùng 1 lần. SCEX phải chống double-submit (user refresh trang callback).
- `{{SCEX_REDIRECT_URI}}` phải nằm trong danh sách SCEX đã đăng ký để C06 cấu hình trước; **không** dùng IP hoặc `localhost`.
- Không log `code` và `encrypt_key`.

#### Example cURL

```bash
curl -i "https://{{SCEX_API_ROOT}}/api/v1/auth/vneid/callback?code=a0c106c9-26ae-450e-89e7-cc9900f95428&encrypt_key=OJkjyfkxJ8mIYCGsyvihdEYVJr%2BUaSBO&state=8f14e45fceea167a5a36dedd4bea2543"
```

---

## 3.2.11 Redirect URI sau đăng xuất VNeID

`GET` `https://{{SCEX_API_ROOT}}/api/v1/auth/vneid/logout/callback`

**Mô tả:** URL do SCEX xây dựng, để VNeID điều hướng về sau khi đăng xuất thành công. Chỉ áp dụng SSO Web.

**Hệ thống xây dựng:** SCEX (VNeID là bên điều hướng vào)

**Tham chiếu contract:** `tcdn_spec_api.md` mục III.3.5 · `rar_sso.md` mục 4.5

**Authentication:** Public endpoint. Bảo vệ bằng `state` + rate limit.

#### Headers

> Endpoint này không yêu cầu Header đặc thù.

#### Path / Query Parameters

| Field   | Type     | Required | Description                                            |
| :------ | :------- | :------: | :----------------------------------------------------- |
| `state` | `String` |    No*   | Giá trị SCEX đã gửi ở mục 3.2.4, dùng để đối chiếu     |

\* SCEX bắt buộc theo yêu cầu bảo mật nội bộ; xem cảnh báo ở mục 3.2.4 về việc VNeID có hỗ trợ `state` hay không.

> ⚠️ `tcdn_spec_api.md` mục III.3.5 và `rar_sso.md` mục 4.5 chỉ nêu "chuyển hướng về `redirect_uri` do TCDN cung cấp", **không đặc tả** VNeID gửi kèm query param nào. SCEX không thể dựa vào bất kỳ tham số nào ở endpoint này. Cần confirm với C06 danh sách tham số thực tế của callback đăng xuất.

#### Request Body

> Endpoint này không sử dụng Request Body.

#### Responses

**`302 Found`** — SCEX validate `state`, xoá phiên đăng nhập nội bộ, redirect về trang logout của Web App.

#### Business Rules

- SCEX **phải** xoá phiên đăng nhập nội bộ, không phụ thuộc kết quả đăng xuất phía VNeID.
- Nếu `state` không hợp lệ, vẫn xoá phiên nội bộ nhưng ghi log cảnh báo — không được để phiên treo.
- App mobile không dùng endpoint này (không có luồng logout qua VNeID).

#### Example cURL

```bash
curl -i "https://{{SCEX_API_ROOT}}/api/v1/auth/vneid/logout/callback?state=8f14e45fceea167a"
```

---

## 3.2.12 Universal Link / URL redirect nhận `transactionCode` từ VNeID

`GET` `{{SCEX_UNIVERSAL_LINK}}/vneid/consent/{transactionCode}` (App-to-App)
`GET` `https://{{SCEX_WEB_ROOT}}/vneid/consent/{transactionCode}` (App-to-Web)

**Mô tả:** Đường dẫn do SCEX xây dựng và cung cấp cho VNeID, để VNeID switch App-to-App (hoặc App-to-Web) trả `transactionCode` về ứng dụng SCEX.

**Hệ thống xây dựng:** SCEX (ứng dụng VNeID là bên mở vào)

**Tham chiếu contract:** `tcdn_spec_api.md` mục III.1.1 · III.2.5

**Authentication:** Không áp dụng (deep link). Xử lý nghiệp vụ phía sau bắt buộc xác thực — xem mục 3.2.20 và 3.2.23.

#### Cấu trúc — 2 dạng theo luồng

| Luồng                                                              | Cấu trúc                                       | Áp dụng                    |
| ------------------------------------------------------------------ | ---------------------------------------------- | -------------------------- |
| **Xuất phát từ ứng dụng SCEX** (SCEX gọi `init` trước, mục 3.2.5)   | `URL_TCDN/[$transactionCode]\|[$result]`        | `scex-rar.md` mục 2.3, 2.4 |
| **Xuất phát từ ứng dụng VNeID** (công dân chọn SCEX trên VNeID)     | `URL_TCDN/[$transactionCode]`                  | `scex-rar.md` mục 2.2      |

#### Path / Query Parameters

| Field               | Type     | Required | Description                                                          |
| :------------------ | :------- | :------: | :------------------------------------------------------------------- |
| `transactionCode`   | `String` | **Yes**  | Mã giao dịch xin consent                                             |
| `result`            | `String` | Chỉ luồng xuất phát từ SCEX | Kết quả — `1` đã đồng ý · `2` không đồng ý · `3` hết hạn |

#### Request Body

> Endpoint này không sử dụng Request Body.

#### Responses

**`200 OK`** — Mở đúng màn hình xử lý tiếp trong ứng dụng SCEX. App/Web sau đó gọi API nội bộ (mục 3.2.20 với luồng xuất phát từ SCEX, mục 3.2.23 với luồng xuất phát từ VNeID).

#### Business Rules

- Dấu phân cách giữa `transactionCode` và `result` trong luồng xuất phát từ SCEX là ký tự `|` — phải URL-encode thành `%7C` khi xử lý.
- `transactionCode` nhận từ deep link là dữ liệu **không tin cậy**. SCEX **bắt buộc** validate ở Backend (mục 3.2.20 / 3.2.23) trước khi xử lý nghiệp vụ; tuyệt đối không tin `result` từ deep link mà phải đối chiếu với kết quả `get-transaction`.
- Universal Link phải cấu hình `apple-app-site-association` (iOS) và `assetlinks.json` (Android) đúng để tránh bị app khác hijack.
- Trường hợp app SCEX chưa được cài: App-to-Web phải có trang fallback hướng dẫn người dùng.

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

**Mô tả:** Callback từ Nền tảng định danh thông báo cho SCEX: (a) tài khoản VNeID của công dân không hợp lệ ngay sau khi `init`, (b) VNeID đã hiển thị màn consent thành công, (c) công dân đã đồng ý / từ chối chia sẻ.

**Hệ thống xây dựng:** SCEX (C06 là client gọi vào) — URL do SCEX định nghĩa và cấu hình sẵn trong Agent Gateway

**Tham chiếu contract:** `tcdn_spec_api.md` mục III.2.2 · mục II.3

**Authentication:** `api_key` do **SCEX cấp cho C06** (chuỗi 32–64 ký tự) + allowlist IP Agent Gateway

#### Headers

| Key            | Type     | Required | Description                                                        |
| :------------- | :------- | :------: | :----------------------------------------------------------------- |
| `Content-Type` | `String` | **Yes**  | `application/json; charset=utf-8`                                  |
| `api_key`      | `String` | **Yes**  | API key **do SCEX cung cấp cho C06** — chuỗi 32–64 ký tự           |
| `request_id`   | `String` | **Yes**  | Mã yêu cầu do **Nền tảng** sinh, duy nhất trên hệ thống của C06    |
| `request_time` | `String` | **Yes**  | `ddMMyyyyHHmmss`                                                   |

#### Path / Query Parameters

> Endpoint này không sử dụng Path/Query parameter.

#### Request Body

Bản tin có **3 biến thể** tuỳ tình huống. SCEX phải xử lý được cả 3.

**Biến thể 1 — VNeID đã hiển thị màn consent thành công (trạng thái `INIT`):**

| Field             | Type     | Required | Description                        | Constraints        |
| :---------------- | :------- | :------: | :--------------------------------- | :----------------- |
| `transactionCode` | `String` | **Yes**  | Mã giao dịch                       | —                  |
| `requestStatus`   | `String` | **Yes**  | Giá trị `INIT`                     | —                  |

```json
{
  "transactionCode": "02d73def-fe15-418c-b97c-37c8214e3ba6",
  "requestStatus": "INIT"
}
```

**Biến thể 2 — công dân đồng ý / từ chối chia sẻ:**

| Field        | Type     | Required | Description                                                            | Constraints                     |
| :----------- | :------- | :------: | :--------------------------------------------------------------------- | :------------------------------ |
| `requestId`  | `String` | **Yes**  | Tham chiếu `request_id` SCEX đã gửi ở `init` (mục 3.2.5)                | —                               |
| `resultCode` | `String` | **Yes**  | `1` đồng ý chia sẻ · `2` không đồng ý chia sẻ — xem mục 3.2.0.6         | —                               |
| `resultDesc` | `String` |    No    | Mô tả chi tiết `resultCode`                                            | —                               |
| `txnId`      | `String` | **Yes**  | Mã giao dịch dùng để truy vấn thông tin công dân (khi công dân đồng ý)  | —                               |
| `data`       | `Object` |    No    | Dữ liệu trả thêm tuỳ cấu hình; không có thì field này không xuất hiện   | —                               |

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

SCEX **bắt buộc** trả `HTTP 200` với bản tin JSON theo đúng cấu trúc C06 quy định (không dùng envelope của SCEX):

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

| Field          | Type          | Required | Description                                             |
| :------------- | :------------ | :------: | :------------------------------------------------------ |
| `responseCode` | `String(2)`   | **Yes**  | `0` thành công; khác `0` là lỗi                          |
| `description`  | `String(100)` | **Yes**  | Mô tả lỗi khi `responseCode` khác `0`                   |
| `responseTime` | `String`      | **Yes**  | `ddMMyyyyHHmmss`                                        |

> ⚠️ Mâu thuẫn trong tài liệu C06: phần API Specs mục III.2.2 ghi chú `"responseCode": 1 //0: thành công, 1: không thành công`, nhưng bảng đặc tả Response body của cùng mục ghi `0: thành công, Khác 0: Lỗi`. Tài liệu này lấy theo **bảng đặc tả** (`0` = thành công). **Cần confirm với C06.**

#### Business Rules

- Thời gian phản hồi của C06 cho biến thể 3 (thông tin công dân không hợp lệ) là **< 1 giây** kể từ khi SCEX gọi `init` (mục III.2.2); mục II.3 của tài liệu C06 lại nêu ngưỡng **tối đa 3 giây**. SCEX nên chọn ngưỡng chờ **3 giây** cho an toàn, sau đó mới switch App-to-App sang VNeID.
- Với biến thể 3, SCEX **phải dừng luồng nghiệp vụ**, không switch sang VNeID, và hiển thị thông báo hướng dẫn công dân kiểm tra lại trên app VNeID. Mã giao dịch đã hết hiệu lực.
- Với `resultCode = 1`, SCEX dùng `txnId` để gọi `get-transaction` (mục 3.2.6 / 3.2.7).
- Với `resultCode = 2`, kết thúc luồng và thông báo cho user; **không** gọi `get-transaction`.
- Endpoint phải **idempotent**: cùng `request_id` gửi lại nhiều lần chỉ được xử lý nghiệp vụ một lần, nhưng vẫn trả `responseCode = 0`.
- SCEX phải quyết định có bật cấu hình nhận trạng thái `INIT` hay không (C06 cấu hình theo yêu cầu của TCDN). Nếu bật, luồng là: `init` → nhận `INIT` → mới switch sang VNeID.
- Phải trả HTTP 200 kể cả khi nghiệp vụ lỗi — lỗi thể hiện qua `responseCode`, không qua HTTP status.

> ⚠️ Cần confirm với C06: SCEX chọn Trường hợp 1 (nhận trạng thái `INIT`) hay Trường hợp 2 (không nhận `INIT`) theo mục II.3. Quyết định này ảnh hưởng trực tiếp tới luồng của SCEX App.

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

**Hệ thống xây dựng:** SCEX (C06 là client gọi vào) — URL do SCEX định nghĩa và cấu hình sẵn trong Agent Gateway

**Tham chiếu contract:** `tcdn_spec_api.md` mục III.4.1 · mục II.4

**Authentication:** `api_key` do **SCEX cấp cho C06** + allowlist IP Agent Gateway

#### Headers

| Key            | Type     | Required | Description                                                     |
| :------------- | :------- | :------: | :-------------------------------------------------------------- |
| `Content-Type` | `String` | **Yes**  | `application/json; charset=utf-8`                               |
| `api_key`      | `String` | **Yes**  | API key **do SCEX cung cấp cho C06**                            |
| `org_code`     | `String` | **Yes**  | `{{ORG_CODE}}` — mã TCDN của SCEX do Nền tảng cấp               |
| `service_code` | `String` | **Yes**  | Giá trị cố định `withdraw-consent`                              |
| `request_id`   | `String` | **Yes**  | Mã yêu cầu do **Nền tảng** sinh. **SCEX phải lưu lại** để dùng ở mục 3.2.9 |
| `request_time` | `String` | **Yes**  | `ddMMyyyyHHmmss`                                                |

#### Path / Query Parameters

> Endpoint này không sử dụng Path/Query parameter.

#### Request Body

| Field             | Type     | Required | Description                                                                                              | Constraints |
| :---------------- | :------- | :------: | :------------------------------------------------------------------------------------------------------- | :---------- |
| `transactionCode` | `String` | **Yes**  | Mã giao dịch cần rút lại đồng ý / xoá dữ liệu. Chính là mã SCEX đã dùng khi `init` (3.2.5) và `get-transaction` (3.2.6 / 3.2.7) | — |

**Example Request:**

```json
{
  "transactionCode": "22fc12e0-be90-4519-8ef6-cdeca95ae22b"
}
```

#### Responses

**`200 OK`** — SCEX **chưa** gọi `get-transaction` (chưa lấy dữ liệu công dân)

```json
{
  "statusCode": 1,
  "requestStatus": "DELETED"
}
```

**`200 OK`** — SCEX **đã** gọi `get-transaction` (đã lấy dữ liệu công dân)

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

| Field           | Type           | Required | Description                                                                    |
| :-------------- | :------------- | :------: | :----------------------------------------------------------------------------- |
| `statusCode`    | `number(1)`    | **Yes**  | `1` thành công, `0` không thành công                                            |
| `requestStatus` | `string(100)`  |    No    | `DELETED` đã rút lại đồng ý thành công · `PROCESSING` đã tiếp nhận và đang xử lý |
| `errorDetail`   | `string(4000)` |    No    | Mô tả chi tiết lỗi, **bắt buộc khi `statusCode = 0`**                            |

#### Business Rules

- **Logic quyết định `requestStatus`:**
  - Nếu SCEX **chưa** gọi `get-transaction` cho `transactionCode` này → trả `DELETED`. Luồng **dừng tại đây**, không gọi mục 3.2.9.
  - Nếu SCEX **đã** gọi `get-transaction` → trả `PROCESSING`, xử lý xoá dữ liệu trong **tối đa 72 giờ**, rồi gọi mục 3.2.9 với `DELETED` hoặc `REJECT`.
- SCEX **bắt buộc lưu `request_id`** nhận được trong header — mục 3.2.9 phải tham chiếu đúng giá trị này.
- Endpoint phải **idempotent** theo `transactionCode`: công dân có thể gửi lại yêu cầu, C06 có thể retry. Lần gọi lại phải trả cùng `requestStatus` mà không tạo thêm tiến trình xoá mới.
- Phải trả HTTP 200 kể cả khi nghiệp vụ lỗi — lỗi thể hiện qua `statusCode = 0` + `errorDetail`.
- SCEX cần theo dõi trạng thái `get-transaction` theo từng `transactionCode` để trả đúng `DELETED` hay `PROCESSING`. Đây là yêu cầu lưu trữ trạng thái bắt buộc, không suy ra được từ dữ liệu nghiệp vụ.
- SCEX phải công bố link "Công bố thông tin rủi ro khi công dân rút lại sự đồng ý chia sẻ dữ liệu" (xem mục 3.2.0.2).
- Cơ sở pháp lý: Nghị định 13/2023/NĐ-CP về quyền rút lại sự đồng ý của chủ thể dữ liệu.

> ⚠️ Phạm vi áp dụng cần confirm với C06: rút lại đồng ý áp dụng cho (1) consent chưa `get-transaction`, (2) đồng ý dài hạn SSO "Không hỏi lại", (3) yêu cầu xoá dữ liệu đã chia sẻ — cần xác định cụ thể trường hợp nào áp dụng cho SCEX. Xem `scex-rar.md` mục 2.5.

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

Các endpoint trong nhóm này **không phơi ra C06**. Client là SCEX Mobile App và SCEX Web. Toàn bộ do SCEX tự định nghĩa.

**Base URL:** `https://{{SCEX_API_ROOT}}`

### Quy ước áp dụng cho toàn bộ nhóm C

| Hạng mục            | Quy ước                                                                                             |
| ------------------- | --------------------------------------------------------------------------------------------------- |
| Naming              | `camelCase` cho field body — đồng nhất với field của C06, tránh phải map 2 lần                       |
| Envelope            | Theo mục 3.2.0.4 (`code` / `message` / `data`, lỗi có `error` / `details` / `request_id`)             |
| Timestamp           | ISO 8601 UTC (ví dụ `2026-08-12T09:15:00Z`)                                                          |
| Idempotency         | Mọi `POST` khởi tạo giao dịch **bắt buộc** hỗ trợ header `Idempotency-Key`                            |
| Correlation         | Mọi response trả `request_id` để đối soát log xuyên hệ thống                                          |
| Authentication      | `Authorization: Bearer <scex_jwt>` — trừ các endpoint đánh dấu **Public**                              |
| Endpoint Public     | 3.2.15, 3.2.16, 3.2.17 — bắt buộc có rate limit và chống bruteforce                                   |

### Ràng buộc bảo mật bắt buộc

- **Không bao giờ trả `access_token` / `refresh_token` / `id_token` của VNeID về client.** Token VNeID chỉ tồn tại ở SCEX Backend. Client chỉ nhận SCEX token.
- **Không trả nguyên văn `userInfo` (JWS) của C06 về client.** Backend verify chữ ký, parse, lọc field theo nhu cầu màn hình rồi mới trả.
- PII (`citizenPid`, `fullName`, `birthDate`, ảnh mặt, `faceImage`) **không được log raw**; áp dụng masking theo chính sách bảo vệ dữ liệu cá nhân của SCEX (xem `scex-rar.md` mục 6.2).
- `state` sinh bằng `SecureRandom` (không random từ tệp số có sẵn hay từ datetime), tối thiểu **16 byte**, TTL **300 giây**, **chỉ dùng 1 lần** (xem `scex-rar.md` mục 2.1).

---

## 3.2.15 Khởi tạo phiên đăng nhập VNeID (Mobile App)

`POST` `https://{{SCEX_API_ROOT}}/api/v1/auth/vneid/sessions`

**Mô tả:** App yêu cầu Backend khởi tạo phiên đăng nhập SSO. Backend sinh `state`, lưu lại, và trả về universal link để App mở sang VNeID.

**Hệ thống xây dựng:** SCEX Backend (client: SCEX Mobile App)

**Luồng liên quan:** `scex-rar.md` mục 2.1 — Luồng đăng nhập APP

**Authentication:** **Public** (chưa đăng nhập)

#### Headers

| Key               | Type     | Required | Description                                            |
| :---------------- | :------- | :------: | :----------------------------------------------------- |
| `Content-Type`    | `String` | **Yes**  | `application/json`                                     |
| `X-Device-Id`     | `String` | **Yes**  | Định danh thiết bị, dùng cho rate limit và chống lạm dụng |
| `X-Platform`      | `String` | **Yes**  | `ios` \| `android`                                     |
| `Idempotency-Key` | `String` |    No    | UUID — trả lại cùng `state` nếu gọi lại trong TTL       |

#### Path / Query Parameters

> Endpoint này không sử dụng Path/Query parameter.

#### Request Body

| Field     | Type     | Required | Description                                   | Constraints                             |
| :-------- | :------- | :------: | :-------------------------------------------- | :-------------------------------------- |
| `purpose` | `String` |    No    | Mục đích phiên đăng nhập                      | `login` (mặc định) \| `link-account`    |

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

| Field          | Type     | Description                                                     |
| :------------- | :------- | :-------------------------------------------------------------- |
| `state`        | `String` | Giá trị chống CSRF, App phải gửi lại ở mục 3.2.16                |
| `expiresAt`    | `String` | Thời điểm `state` hết hạn (ISO 8601 UTC), TTL 300 giây          |
| `authorizeUrl` | `String` | Universal link để App mở sang VNeID                             |

**`429 Too Many Requests`**

```json
{
  "code": 429,
  "error": "Too Many Requests",
  "message": "Rate limit exceeded. Try again in 60 seconds.",
  "request_id": "req_7f3a91c4"
}
```

**`503 Service Unavailable`** — Không dựng được phiên do cấu hình SSO thiếu / VNeID không khả dụng.

#### Business Rules

- `state` sinh bằng `SecureRandom`, ≥ 16 byte, TTL 300 giây, dùng 1 lần duy nhất.
- Backend lưu `state` kèm `X-Device-Id` và `purpose`; ở mục 3.2.16 phải khớp cả 3.
- Rate limit đề xuất: **10 req/phút/thiết bị**, **60 req/phút/IP**.
- Nếu VNeID không khả dụng, trả `503` để App fallback sang phương thức đăng nhập hiện có (OTP / NFC) — không được chặn luồng chính (xem `scex-rar.md` mục 4.3).

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

**Luồng liên quan:** `scex-rar.md` mục 2.1 — Luồng đăng nhập APP

**Authentication:** **Public** (chưa đăng nhập)

#### Headers

| Key            | Type     | Required | Description                              |
| :------------- | :------- | :------: | :--------------------------------------- |
| `Content-Type` | `String` | **Yes**  | `application/json`                       |
| `X-Device-Id`  | `String` | **Yes**  | Phải khớp giá trị đã gửi ở mục 3.2.15    |

#### Path / Query Parameters

> Endpoint này không sử dụng Path/Query parameter.

#### Request Body

| Field               | Type     | Required | Description                                              | Constraints                     |
| :------------------ | :------- | :------: | :------------------------------------------------------- | :------------------------------ |
| `authorizationCode` | `String` | **Yes**  | Mã `code` VNeID trả về qua app-to-app                    | Dùng 1 lần                      |
| `state`             | `String` | **Yes**  | Giá trị nhận từ mục 3.2.15                               | Phải còn hiệu lực, chưa sử dụng |
| `encryptKey`        | `String` |    No    | Giá trị `encrypt_key` VNeID trả kèm (nếu có)             | —                               |

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
  "details": ["VNeID account level must be 2", "accountType must be individual"],
  "request_id": "req_7f3a91c5"
}
```

**`502 Bad Gateway`** — Lỗi khi gọi sang Agent Gateway (`sso-token` / `sso-userinfo` thất bại).

#### Business Rules

- Validate `state` trước mọi thứ: tồn tại, chưa dùng, chưa hết TTL, khớp `X-Device-Id`. Sai bất kỳ điều kiện nào → `400`, ghi log cảnh báo bảo mật.
- Vô hiệu hoá `state` **ngay** sau khi validate thành công (one-time use), trước khi gọi ra C06.
- Bắt buộc kiểm tra `accountLevel = 2` và `accountType = 1` (cá nhân); không đạt → `403` (xem `scex-rar.md` mục 1.3).
- Bắt buộc verify chữ ký JWS trong response của `sso-userinfo` trước khi tin dữ liệu.
- **Không** trả `access_token` / `refresh_token` / `id_token` của VNeID về App. Token VNeID lưu server-side, gắn với phiên nội bộ.
- `citizenPid` trả về client phải **masked**; giá trị đầy đủ chỉ dùng nội bộ.
- Rate limit đề xuất: **10 req/phút/thiết bị**.
- `authorizationCode` chỉ dùng 1 lần — chống double-submit khi App retry.

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

**Luồng liên quan:** `scex-rar.md` mục 2.1 — Luồng đăng nhập WEB

**Authentication:** **Public** (chưa đăng nhập)

#### Headers

| Key      | Type     | Required | Description                                    |
| :------- | :------- | :------: | :--------------------------------------------- |
| `Cookie` | `String` |    No    | Session cookie hiện có (nếu đã có phiên)       |

#### Path / Query Parameters

| Field         | Type     | Required | Description                                                          |
| :------------ | :------- | :------: | :------------------------------------------------------------------- |
| `returnUrl`   | `String` |    No    | Đường dẫn nội bộ để redirect về sau khi đăng nhập thành công          |
| `purpose`     | `String` |    No    | `login` (mặc định) \| `link-account`                                 |

#### Request Body

> Endpoint này không sử dụng Request Body.

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

#### Business Rules

- `state` lưu vào server-side session (hoặc cookie `HttpOnly; Secure; SameSite=Lax` có ký), TTL 300 giây, dùng 1 lần.
- `returnUrl` **bắt buộc** validate là đường dẫn tương đối trong domain của SCEX — chống open redirect. Tuyệt đối không nhận URL tuyệt đối từ bên ngoài.
- Rate limit đề xuất: **30 req/phút/IP**.
- Nếu VNeID không khả dụng, redirect về trang đăng nhập thường của SCEX kèm thông báo, không chặn luồng chính.

#### Example cURL

```bash
curl -i 'https://{{SCEX_API_ROOT}}/api/v1/auth/vneid/login?returnUrl=%2Fdashboard'
```

---

## 3.2.18 Đăng xuất VNeID (Web — HTTP 302)

`POST` `https://{{SCEX_API_ROOT}}/api/v1/auth/vneid/logout`

**Mô tả:** Web App yêu cầu đăng xuất. Backend tạo `state`, trả `HTTP 302` sang URL đăng xuất của VNeID. **Chỉ áp dụng Web** — App mobile không có luồng đăng xuất qua VNeID.

**Hệ thống xây dựng:** SCEX Backend (client: SCEX Web)

**Luồng liên quan:** `scex-rar.md` mục 2.1 — Luồng đăng xuất (chỉ Web)

**Authentication:** Required

#### Headers

| Key             | Type     | Required | Description                    |
| :-------------- | :------- | :------: | :----------------------------- |
| `Authorization` | `String` | **Yes**  | `Bearer <scex_jwt>`            |
| `Cookie`        | `String` | **Yes**  | Session cookie của SCEX Web    |

#### Path / Query Parameters

> Endpoint này không sử dụng Path/Query parameter.

#### Request Body

> Endpoint này không sử dụng Request Body.

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

#### Business Rules
- Phiên nội bộ của SCEX **phải** được xoá dù VNeID có đăng xuất thành công hay không — xử lý ở mục 3.2.11.
- Chỉ áp dụng Web SSO. Client mobile gọi endpoint đăng xuất nội bộ thông thường, không qua VNeID.
- Revoke cả `refreshToken` của SCEX và xoá token VNeID đang giữ server-side.

#### Example cURL

```bash
curl -i -X POST 'https://{{SCEX_API_ROOT}}/api/v1/auth/vneid/logout' \
  -H 'Authorization: Bearer <scex_jwt>' \
  -H 'Cookie: scex_session=...'
```

---

## 3.2.19 Khởi tạo giao dịch định danh / xác thực với VNeID

`POST` `https://{{SCEX_API_ROOT}}/api/v1/identity/vneid/transactions`

**Mô tả:** App/Web yêu cầu Backend khởi tạo giao dịch chia sẻ thông tin / xác thực sinh trắc học / xác thực thông tin công dân. Backend gọi `init` (mục 3.2.5) ra Agent Gateway và trả về `transactionId` kèm universal link sang VNeID.

**Hệ thống xây dựng:** SCEX Backend (client: SCEX Mobile App / SCEX Web)

**Luồng liên quan:** `scex-rar.md` mục 2.3, 2.4

**Authentication:** Required

#### Headers

| Key               | Type     | Required | Description                                                  |
| :---------------- | :------- | :------: | :----------------------------------------------------------- |
| `Authorization`   | `String` | **Yes**  | `Bearer <scex_jwt>`                                          |
| `Content-Type`    | `String` | **Yes**  | `application/json`                                           |
| `Idempotency-Key` | `String` | **Yes**  | UUID — chống tạo trùng giao dịch khi client retry            |

#### Path / Query Parameters

> Endpoint này không sử dụng Path/Query parameter.

#### Request Body

| Field         | Type     | Required | Description                                                                 | Constraints                                                                 |
| :------------ | :------- | :------: | :-------------------------------------------------------------------------- | :-------------------------------------------------------------------------- |
| `serviceCode` | `String` | **Yes**  | Loại dịch vụ cần dùng                                                       | `share-info` \| `face-matching` \| `biometric-share-info` \| `verify-info` |
| `citizenPid`  | `String` | **Yes**  | Số định danh cá nhân                                                        | 12 ký tự số                                                                 |
| `fullName`    | `String` |    No    | Họ và tên đầy đủ                                                            | Max 100 ký tự                                                               |
| `birthDate`   | `String` |    No    | Ngày sinh                                                                   | `dd-MM-yyyy`                                                                |
| `verifyFields`| `Object` |    No    | Chỉ dùng với `verify-info` — các field cần đối khớp true/false               | Chỉ chứa field đã đăng ký trong cấu hình dịch vụ với C06                     |

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
  "details": ["An active transaction already exists for this citizen and service"],
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

#### Business Rules

- `Idempotency-Key` bắt buộc: cùng key trong 30 phút trả lại đúng `transactionId` đã tạo, không gọi `init` lần nữa.
- Backend **phải map** `responseCode` của C06 sang HTTP status + `details` có ý nghĩa, không đẩy nguyên mã lỗi C06 xuống client mà không giải thích (xem mục 3.2.0.6).
- Với `responseCode = -17` (thông tin công dân không hợp lệ), giao dịch hết hiệu lực ngay — Backend không được cho client retry với cùng `citizenPid`.
- Nếu SCEX bật cấu hình nhận trạng thái `INIT` (mục 3.2.13), Backend **chỉ** trả `vneidUrl` sau khi nhận được callback `INIT`; hoặc trả kèm cờ để client chờ. Cần chốt phương án theo kết quả confirm ở mục 3.2.13.
- `citizenPid` gửi lên phải khớp với chủ tài khoản đang đăng nhập (chống lấy định danh của người khác).
- Giao dịch có TTL 30 phút theo ràng buộc của C06 — `expiresAt` phản ánh đúng ràng buộc này.
- Không log `citizenPid` raw.

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

| Field           | Type     | Required | Description                              |
| :-------------- | :------- | :------: | :--------------------------------------- |
| `transactionId` | `String` | **Yes**  | Mã giao dịch nhận được từ deep link      |

#### Request Body

| Field    | Type     | Required | Description                                                    | Constraints              |
| :------- | :------- | :------: | :------------------------------------------------------------- | :----------------------- |
| `result` | `String` |    No    | Giá trị `result` nhận từ deep link (nếu có)                    | `1` \| `2` \| `3`        |

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

| `status`           | Ý nghĩa                                                |
| :----------------- | :----------------------------------------------------- |
| `INITIATED`        | Đã khởi tạo, chưa có kết quả consent                   |
| `CONSENT_GRANTED`  | Công dân đã đồng ý chia sẻ                              |
| `CONSENT_DENIED`   | Công dân không đồng ý chia sẻ                           |
| `EXPIRED`          | Giao dịch hết hiệu lực                                 |
| `COMPLETED`        | Đã lấy và xử lý xong dữ liệu                            |
| `FAILED`           | Giao dịch thất bại                                      |

| `nextAction`        | Ý nghĩa                                                |
| :------------------ | :----------------------------------------------------- |
| `SUBMIT_BIOMETRICS` | Cần thực hiện sinh trắc học qua SDK (mục 3.2.21)        |
| `FETCH_RESULT`      | Có thể tra kết quả ngay (mục 3.2.22)                    |
| `NONE`              | Không có bước tiếp theo                                |

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

#### Business Rules

- **Không tin `result` từ deep link.** Trạng thái thật lấy từ callback (mục 3.2.13) hoặc từ `get-transaction`. `result` chỉ dùng để tối ưu UX (hiển thị sớm), không dùng làm căn cứ nghiệp vụ.
- Bắt buộc kiểm tra `transactionId` thuộc về `userId` trong JWT — chống truy vấn giao dịch của người khác (IDOR).
- Không tiết lộ thông tin nội bộ trong `404` — không phân biệt "không tồn tại" và "không thuộc bạn" ra ngoài mức cần thiết.

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
| `Idempotency-Key` | `String` | **Yes**  | UUID — chống gọi trùng làm tiêu hao quota 3 lần của C06 |

#### Path / Query Parameters

| Field           | Type     | Required | Description       |
| :-------------- | :------- | :------: | :---------------- |
| `transactionId` | `String` | **Yes**  | Mã giao dịch      |

#### Request Body

| Field         | Type     | Required | Description                                                | Constraints                                                    |
| :------------ | :------- | :------: | :--------------------------------------------------------- | :------------------------------------------------------------- |
| `faceImage`   | `String` | **Yes**  | Chuỗi dữ liệu ảnh **do SDK của TTXT trả ra**               | Không phải ảnh raw tự chụp / tự encode                          |
| `requestType` | `String` | **Yes**  | Loại xác thực đã dùng ở SDK                                | `sdk-liveness-match` \| `sdk-match`                             |

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

#### Business Rules

- **Quota cứng của C06: tối đa 3 lần gọi `get-transaction` trong 30 phút.** Backend phải đếm và chặn ở phía SCEX trước khi gọi ra, để không đốt quota bằng những lần retry vô nghĩa.
- `Idempotency-Key` bắt buộc — client retry mạng không được tạo lời gọi mới ra C06.
- Backend phải verify chữ ký JWS của `biometricResult` và `userInfo` trước khi tin dữ liệu.
- `faceMatch.score` và `liveness.score` là **số liệu nội bộ** — không trả về client; chỉ trả nhãn định tính (`HIGH` / `MEDIUM` / `LOW`). Điểm số lưu lại để admin nghiệp vụ tra cứu (xem `scex-rar.md` mục 6.2).
- `deviceRisk.isHighRisk = true` phải kích hoạt chính sách rủi ro của SCEX (chặn / xác thực bổ sung / cảnh báo vận hành) — không được bỏ qua.
- `faceImage` **không được lưu** sau khi giao dịch hoàn tất, **không log**.
- `faceImage` là payload lớn (chuỗi dữ liệu ảnh từ SDK). Cần nâng giới hạn body size của API gateway / reverse proxy cho riêng endpoint này, và cân nhắc nén ở tầng transport. Nếu kích thước vượt ngưỡng chấp nhận được, phương án thay thế là App upload ảnh qua endpoint riêng rồi truyền `faceImageRef` — nhưng phải đảm bảo dữ liệu vẫn nguyên vẹn đúng định dạng SDK sinh ra khi Backend gửi sang C06.
- Với luồng xuất phát từ VNeID, sau khi xử lý nghiệp vụ xong Backend **bắt buộc** gọi `receive-result` (mục 3.2.8).

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

| Field           | Type      | Required | Description                                                              |
| :-------------- | :-------- | :------: | :----------------------------------------------------------------------- |
| `transactionId` | `String`  | **Yes**  | Mã giao dịch (path)                                                      |
| `includeResult` | `Boolean` |    No    | `true` để trả kèm dữ liệu định danh đã đối khớp. Mặc định `false`         |

#### Request Body

> Endpoint này không sử dụng Request Body.

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

**`200 OK`** — Dịch vụ xác thực thông tin (`verify-info`) — kết quả đối khớp

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

#### Business Rules

- Endpoint này **không** gọi ra C06 mỗi lần được gọi. Backend đọc từ trạng thái đã lưu; chỉ gọi `get-transaction` khi trạng thái là `CONSENT_GRANTED` và chưa lấy dữ liệu, và phải tôn trọng quota 3 lần / 30 phút.
- Rate limit polling đề xuất: **12 req/phút/giao dịch** (5 giây/lần).
- Bắt buộc kiểm tra quyền sở hữu giao dịch theo `userId` trong JWT.
- `citizenPid` trả về **luôn masked**. Chỉ trả các field tối thiểu mà màn hình cần — không dump toàn bộ `userInfo`.
- Endpoint này là cơ chế **khôi phục trạng thái** khi luồng bị ngắt giữa chừng: người dùng không mất dữ liệu đã nhập và có thể tiếp tục từ bước dở dang (xem `scex-rar.md` mục 4.3).
- `verifyResult` chỉ trả `true`/`false` cho các field SCEX đã gửi lên; `sharedFields` chứa các field công dân đồng ý chia sẻ thêm.

#### Example cURL

```bash
curl -X GET 'https://{{SCEX_API_ROOT}}/api/v1/identity/vneid/transactions/087f0479-98e8-4f37-a312-a7e24399e26a?includeResult=true' \
  -H 'Authorization: Bearer <scex_jwt>'
```

---

## 3.2.23 Tiếp nhận `consentId` từ luồng xuất phát VNeID

`POST` `https://{{SCEX_API_ROOT}}/api/v1/identity/vneid/consents`

**Mô tả:** Khi công dân chọn dịch vụ SCEX **trên ứng dụng VNeID**, VNeID switch App-to-App về SCEX kèm `transactionCode` (mục 3.2.12). App gửi mã này cho Backend để Backend gọi `get-transaction`, xử lý nghiệp vụ, rồi trả kết quả về C06 (mục 3.2.8).

**Hệ thống xây dựng:** SCEX Backend (client: SCEX Mobile App)

**Luồng liên quan:** `scex-rar.md` mục 2.2

**Authentication:** Required

#### Headers

| Key               | Type     | Required | Description                                            |
| :---------------- | :------- | :------: | :----------------------------------------------------- |
| `Authorization`   | `String` | **Yes**  | `Bearer <scex_jwt>`                                    |
| `Content-Type`    | `String` | **Yes**  | `application/json`                                     |
| `Idempotency-Key` | `String` | **Yes**  | UUID — chống xử lý trùng và tiêu hao quota của C06     |

#### Path / Query Parameters

> Endpoint này không sử dụng Path/Query parameter.

#### Request Body

| Field             | Type     | Required | Description                                                       | Constraints |
| :---------------- | :------- | :------: | :---------------------------------------------------------------- | :---------- |
| `transactionCode` | `String` | **Yes**  | Mã giao dịch (consentId) nhận từ deep link của VNeID              | —           |

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

#### Business Rules

- **Ràng buộc quan trọng nhất:** nếu Backend đã gọi `get-transaction` cho `transactionCode` này thì **bắt buộc** phải gọi `receive-result` (mục 3.2.8) để trả kết quả xử lý về C06. Không trả kết quả sẽ khiến giao dịch của công dân bị treo trên app VNeID và công dân không thực hiện được giao dịch mới. Backend phải đưa bước này vào **hàng đợi bền có retry**, không phụ thuộc vòng đời request.
- Xử lý bất đồng bộ (`202`) vì luồng gồm nhiều bước gọi ra ngoài; App theo dõi qua `pollUrl` (mục 3.2.22).
- Bắt buộc idempotent theo `transactionCode` — VNeID có thể mở lại app nhiều lần với cùng mã.
- Backend phải đối chiếu `citizenPid` trong dữ liệu nhận được với chủ tài khoản đang đăng nhập. Không khớp thì **không** ghép dữ liệu vào tài khoản, và trả kết quả `FAILURE` về C06 kèm lý do.
- Phải lưu trạng thái "đã gọi `get-transaction`" theo `transactionCode` để phục vụ mục 3.2.14 (quyết định trả `DELETED` hay `PROCESSING`).
- Quota 3 lần / 30 phút của C06 áp dụng cho endpoint này.

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

## Notes

- Tất cả timestamp của API nội bộ SCEX (nhóm C) dùng ISO 8601 UTC. API nhóm B (do SCEX xây dựng nhưng C06 gọi vào) dùng `ddMMyyyyHHmmss` và ngày tháng `dd-MM-yyyy` theo quy ước của C06 — **không được** tự chuẩn hoá.
- Ràng buộc thời gian tổng hợp: `request_time` lệch > 2 phút → timeout · `state` TTL 300 giây · `transactionId` hết hạn 30 phút · tối đa 3 lần gọi `get-transaction` / 30 phút (API của C06, xem `scex-rar-api-spec.md`) · hạn mức 10.000 lần/ngày trên mỗi cặp (`target_code`, `service_code`) · callback thông tin công dân không hợp lệ trong < 1–3 giây · rút lại đồng ý xử lý trong tối đa 72 giờ.
- Nhóm B là contract với bên thứ ba: mọi thay đổi phải đối chiếu lại `tcdn_spec_api.md` phiên bản tương ứng. Tài liệu này chốt theo **v3.1 (15/8/2025)**.
- Tài liệu này chỉ đặc tả interface. Chi tiết mã hoá / ký số, mô hình bảo mật, và giám sát nằm ở `scex-rar.md` mục 5 và mục 6.
- Tài liệu này **không** bao gồm các API do C06 cung cấp (Nhóm A, mục 3.2.1–3.2.9) — xem `scex-rar-api-spec.md` để tra cứu đầy đủ cả 3 nhóm.

---

## Appendix — Tổng hợp điểm cần confirm với C06

> Chỉ liệt kê các điểm liên quan trực tiếp đến API/URL phía SCEX (Nhóm B, C). Các điểm liên quan riêng đến Nhóm A (API do C06 cung cấp) xem `scex-rar-api-spec.md`.

| # | Mục       | Vấn đề                                                                                                                       | Mức độ    |
| - | --------- | ---------------------------------------------------------------------------------------------------------------------------- | --------- |
| 1 | 3.2.0.1   | Danh mục giá trị `target_code` chưa có tài liệu                                                                              | Blocker   |
| 2 | Nhóm B    | Cơ chế xác thực chiều C06 → SCEX chỉ có `api_key`; chưa rõ có mTLS / ký số bản tin / dải IP nguồn để allowlist                 | **Cao**   |
| 3 | 3.2.8 *(scex-rar-api-spec.md)* | Không có đặc tả field chi tiết của `receive-result` cho `share-info` / `biometric-share-info` / `face-matching` / `verify-info` — ảnh hưởng trực tiếp đến việc SCEX implement mục 3.2.13 | Blocker   |
| 4 | 3.2.8, 3.2.9 *(scex-rar-api-spec.md)* | Danh mục `reasonRequestCode` mà SCEX cần đăng ký trước với C06 — dùng trong request body mục 3.2.13, 3.2.14                | Blocker   |
| 5 | 3.2.13    | Mâu thuẫn `responseCode`: API Specs ghi `1 = không thành công`, bảng đặc tả ghi `0 = thành công`                               | **Cao**   |
| 6 | 3.2.13    | SCEX chọn Trường hợp 1 (nhận trạng thái `INIT`) hay Trường hợp 2 (không nhận) theo mục II.3                                    | **Cao**   |
| 7 | 3.2.13    | Ngưỡng thời gian callback thông tin công dân không hợp lệ: mục III.2.2 ghi < 1s, mục II.3 ghi tối đa 3s                        | Trung bình |
| 8 | 3.2.11    | VNeID có xử lý `id_token_hint` và `state` ở URL logout hay không                                                              | Trung bình |
| 9 | 3.2.14   | Phạm vi dữ liệu phải xoá khi công dân rút lại đồng ý; phạm vi trường hợp áp dụng cho SCEX                                      | **Cao**   |
| 10 | Chung    | Vòng đời `consentId` / `transactionId`; phân biệt `consentId` của SSO và của luồng chia sẻ thông tin                            | Trung bình |
| 11 | Chung    | Môi trường UAT có cần Agent Gateway hay không                                                                                  | Trung bình |
| 12 | 3.2.0.7  | Cấu hình dịch vụ cấp cho SCEX: service-code nào bật xác thực ảnh mặt; cụ thể `verify-info` dùng `get-transaction` v1 hay v2 *(ảnh hưởng cách SCEX Backend gọi ra ở mục 3.2.19/3.2.21)*      | **Cao**   |
| 13 | 3.2.11   | VNeID gửi kèm query param nào ở callback đăng xuất — tài liệu không đặc tả                                                       | Trung bình |
