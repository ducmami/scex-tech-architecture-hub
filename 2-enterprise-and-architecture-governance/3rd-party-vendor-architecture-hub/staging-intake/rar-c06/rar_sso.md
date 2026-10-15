**HƯỚNG DẪN TÍCH HỢP SSO VỚI VNeID**

## 1. Mô hình tích hợp SSO (Single Sign-On)

Tích hợp theo chuẩn OpenID Connect.

**Hình 1 – Mô hình SSO dựa trên OAuth2**

<!-- source: images/image1-sso-oauth2-model.png -->

```mermaid
sequenceDiagram
    participant RS as Resource Server
    participant C as Client
    participant AS as Authorization Server / IdP

    C->>RS: A. Client requests authorization
    RS->>C: B. Receives authorization grant
    C->>AS: C. Client requests access token w/ grant
    AS->>C: D. Access token is granted
    C->>RS: D. Client requests protected resource w/ token
    RS->>AS: E. Resource server validates access token
    AS-->>RS: AS sends user identity attributes
    RS->>C: F. Client receives resource
```

## 2. Mô hình tích hợp SSO của VNeID

**Hình 2 – SSO VNeID Flow (activity diagram, 3 lane: User / Tổ chức khai thác dịch vụ / VNeID)**

<!-- source: images/image2-vneid-sso-model.png -->

```mermaid
flowchart TB
    subgraph LANE_USER["User"]
        direction TB
        ST(("Start"))
        U1["1. Click button<br/>&quot;Đăng nhập bằng tài khoản VNeID&quot;"]
        EN((("End")))
    end

    subgraph LANE_ORG["Tổ chức khai thác dịch vụ"]
        direction TB
        O2["2. Điều hướng sang<br/>trang Đăng nhập của VNeID"]
        O8["8. Gửi yêu cầu lấy token<br/>sang kèm author code"]
        O10["10. Gửi yêu cầu lấy<br/>thông tin user"]
        O12["12. Nhận thông tin User,<br/>đăng nhập thành công"]
    end

    subgraph LANE_VNEID["VNeID"]
        direction TB
        V3{"3. SSO web hay app?"}
        V41["4.1 Quy trình đăng nhập<br/>trên hệ thống VNeID"]
        V42["4.2 Quy trình đăng nhập<br/>trên hệ thống VNeID"]
        V5["5. Quy trình xin consent<br/>trên ứng dụng VNeID"]
        V6{"6. Đồng ý chia sẻ /<br/>không chia sẻ?"}
        V71["7.1. Trả thông báo<br/>Đăng nhập không thành công"]
        V72["7.2. Điều hướng lại URI<br/>gửi kèm author code"]
        V9["9. Trả thông tin token<br/>đăng nhập"]
        V11["11. Trả thông tin user<br/>đăng nhập"]
    end

    ST --> U1 --> O2 --> V3
    V3 -- WEB --> V41
    V3 -- APP --> V42
    V41 --> V5
    V42 --> V5
    V5 --> V6
    V6 -- "Không chia sẻ" --> V71 --> EN
    V6 -- "Đồng ý" --> V72 --> O8 --> V9 --> O10 --> V11 --> O12 --> EN
```

**Hình 3 – Luồng tích hợp SSO VNeID (sequence diagram)**

<!-- source: images/image3-vneid-sso-flow.png -->

```mermaid
sequenceDiagram
    actor U as User
    participant ORG as Tổ chức khai thác
    participant V as VNEID

    U->>ORG: 1. Click button "Đăng nhập bằng tài khoản VNeID"
    ORG->>V: 2. Điều hướng sang trang Đăng nhập của VNeID
    U->>V: 3. Đăng nhập vào hệ thống VNEID
    V->>V: 4. Kiểm tra thông tin đăng nhập
    V->>V: 5. Hiển thị consent chia sẻ thông tin
    U->>V: 6. Đồng ý chia sẻ / không chia sẻ?
    V->>ORG: 7.2. Điều hướng lại URI gửi kèm author code
    ORG->>V: 8. Gửi yêu cầu lấy token sang kèm author code
    V-->>ORG: 9. Trả thông tin token đăng nhập
    ORG->>V: 10. Gửi yêu cầu lấy thông tin user
    V-->>ORG: 11. Trả thông tin user đăng nhập
    ORG->>ORG: 12. Nhận thông tin User, Đăng nhập thành công
```

## 3. Danh sách URL, Endpoint & Param cần tích hợp

### 3.1. Đối với Web

| **Stt** | **EndPoint & Param**  | **Mô tả**                                                                      | **Ghi chú**                                                                                                                                                                                              |
| ------- | --------------------- | ------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1       | Clien id              | Tham số cấu hình kết nối VNeID SSO                                             | _Liên hệ đầu mối VNeID để được cung cấp môi trường tích hợp_                                                                                                                                             |
| 2       | Client secret         | Tham số cấu hình kết nối VNeID SSO                                             |                                                                                                                                                                                                          |
| 3       | PrivateKey            | Thông kin key để giải mã                                                       |                                                                                                                                                                                                          |
| 4       | URL-SSO               | URL điều hướng sang trang đăng nhập VNeID                                      | https://sso-uatvneid.dancuquocgia.com (_Thông tin có thể được thay đổi giữa các môi trường tích hợp_)                                                                                                    |
| 5       | Token Endpoint URL    | Endpoint lấy token                                                             | [https://sso-uatvneid.dancuquocgia.com/api/idp/sso/auth/token](https://sso-uatvneid.dancuquocgia.com/api/idp/sso/auth/token) (_Thông tin có thể được thay đổi giữa các môi trường tích hợp_)             |
| 6       | Callback Url          | Endpoint call back url (_là url nhận lại mã code sau khi xác thực thành công_) | Tổ chức khai thác dịch vụ cung cấp thông tin<br>URL sẽ nằm trong danh sách mà tổ chức khai thác dịch vụ để VNeID config trước.                                                                           |
| 7       | Userinfo Endpoint URL | Endpoint lấy thông tin user                                                    | [https://sso-uatvneid.dancuquocgia.com/api/idp/third-party/userinfo](https://sso-uatvneid.dancuquocgia.com/api/idp/third-party/userinfo) (_Thông tin có thể được thay đổi giữa các môi trường tích hợp_) |
| 8       | URL-LOGOUT            | Endpoint đăng xuất VNeID                                                       | https://sso-uatvneid.dancuquocgia.com/api/idp/sso/login-actions/logout (_Thông tin có thể được thay đổi giữa các môi trường tích hợp_)                                                                   |

### 3.2. Đối với App mobile (sử dụng universal link)

| **Stt** | **EndPoint & Param**   | **Mô tả**                                         | **Ghi chú**                                                                                                                                                                                              |
| ------- | ---------------------- | ------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1       | Client id              | Tham số cấu hình kết nối VNeID SSO                | _Liên hệ đầu mối VNeID để được cung cấp môi trường tích hợp_                                                                                                                                             |
| 2       | Client secret          | Tham số cấu hình kết nối VNeID SSO                |  _Liên hệ đầu mối VNeID để được cung cấp môi trường tích hợp_ |
| 3       | PrivateKey             | Thông kin key để giải mã                          |   _Liên hệ đầu mối VNeID để được cung cấp môi trường tích hợp_|
| 4       | **Universal link SSO** | URL điều hướng sang trang đăng nhập VNeID         | https:// universal.dancuquocgia.com/login-sso/CLIENT_ID (_Thông tin có thể được thay đổi giữa các môi trường tích hợp_)                                                                                  |
| 5       | Token Endpoint URL     | Endpoint lấy token                                | [https://sso-uatvneid.dancuquocgia.com/api/idp/sso/auth/token](https://sso-uatvneid.dancuquocgia.com/api/idp/sso/auth/token) (_Thông tin có thể được thay đổi giữa các môi trường tích hợp_)             |
| 6       | Callback link          | Link nhận lại mã code sau khi xác thực thành công | Tổ chức khai thác dịch vụ cung cấp thông tin<br>URL sẽ nằm trong danh sách mà tổ chức khai thác dịch vụ cung cấp VNeID cấu hình.                                                                         |
| 6       | Userinfo Endpoint URL  | Endpoint lấy thông tin user                       | [https://sso-uatvneid.dancuquocgia.com/api/idp/third-party/userinfo](https://sso-uatvneid.dancuquocgia.com/api/idp/third-party/userinfo) (_Thông tin có thể được thay đổi giữa các môi trường tích hợp_) |

## 4. Chi tiết API, URL tích hợp

### 4.1. URL điều hướng đăng nhập sang VNeID

- **Hệ thống xây dựng URL:** Hệ thống Định danh và xác thực điện tử (VNeID). (Mọi loại tổ chức đều gọi URL này)
- **Mục đích:** URL điều hướng sang trang đăng nhập VNeID gửi kèm các tham số.
- **Cấu trúc:**

**+ WEB:**

```
$[URL-SSO] /auth?response_type=$[response_type]&client_id=$[client_id]&scope=$[openid]&redirect_uri=$[redirect_uri]&…
```

**+ APP:**

```
$[UNIVERSAL-LINK-SSO] /$[client_id]
```

| **Stt** | **Tên trường**                                                                                                                                              | **Kiểu dữ liệu** | **Bắt buộc** | **Mô tả**                                                                                               |
| ------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------- | ------------ | ------------------------------------------------------------------------------------------------------- |
| 1       | URL-SSO/UNIVERSAL-LINK-SSO                                                                                                                                  | String           | x            | URL điều hướng sang trang đăng nhập VNeID<br>Tham khảo mục Danh sách URL, Endpoint & Param cần tích hợp |
| 2       | redirect_uri                                                                                                                                                | String           | x            | Url chuyển tiếp nhận lại mã code sau khi xác thực thành công. (URL Tổ chức khai thác dịch vụ cung cấp)  |
| 3       | response_type                                                                                                                                               | String           | x            | Giá trị mặc định là ‘code’                                                                              |
| 4       | scope                                                                                                                                                       | String           | x            | Giá trị mặc định là ‘openid’                                                                            |
| 5       | Tổ chức khai thác dịch vụ có thể định nghĩa thêm các trường cần thiết rồi truyền vào URL, hệ thống VNeID sẽ trả về giá trị tương ứng như thế ở redirect_uri |                  |              |                                                                                                         |

### 4.2. Redirect_uri Tổ chức khai thác dịch vụ

- **Hệ thống xây dựng URL:** Tổ chức khai thác dịch vụ
- **Mục đích:** Url chuyển tiếp nhận lại mã code sau khi xác thực thành công.
- **Cấu trúc:**

```
$[Redirect_uri]?code=$[code]&encrypt_key=$[encrypt_key]
```


| **Stt** | **Tên trường** | **Kiểu dữ liệu** | **Bắt buộc** | **Mô tả**                                                                                                                                                                                                                                                                                                                                                                                  |
| ------- | -------------- | ---------------- | ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 1       | code           | String           | x            | Code xác thực thành công                                                                                                                                                                                                                                                                                                                                                                   |
| 2       | encrypt_key    | String           |              | Secret key của thuật toán mã hóa đối xứng AES. Key này đã được vneid mã hóa bằng thuật toán bất đối xứng RSA.<br>Nếu trường này có giá trị nghĩa là dữ liệu sẽ mã hoá<br>Nếu không có giá trị hoặc không tồn tại thì đầu ra sẽ trả giống như mục **Đặc tả giá trị nhận được sau khi giải mã data nhận được từ VNeID**<br>Lưu ý: Đừng dùng hàm getQuery param có sẵn của ngôn ngữ lập trình |

### 4.3. API lấy token

- **Hệ thống xây dựng API:** Hệ thống Định danh và xác thực điện tử (VNeID). Nếu tổ chức khai thác dịch vụ là Doanh nghiệp thì sẽ gọi API thông qua Hệ thống Nền tảng Định danh và xác thực điện tử. Nếu tổ chức khai thác dịch vụ là Bộ ban ngành sẽ gọi trực tiếp API này qua các môi trường NDXP/VDXP/CP.NET/…
- **Mục đích:** API dùng để cung cấp thông tin token cho hệ thống Tổ chức khai thác dịch vụ.
- **API Specs:**

| Name         | Lấy thông tin token hệ thống VNeID                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Description  | Cấp accessToken cho hệ thống Tổ chức khai thác dịch vụ                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| URL          | api/idp/sso/auth/token                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| Method       | POST                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| Header       | • Content-Type: application/json                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| Params       |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| Request Body | --data-urlencode 'code=được trả về khi ở Redirect_uri Tổ chức khai thác dịch vụ khai báo' \\<br>--data-urlencode 'grant_type= Mặc dịnh: authorization_code' \\<br>--data-urlencode 'client_id=Client ' \\<br>--data-urlencode 'redirect_uri=http://adminvneid.vn/sync-ext/xac-thuc' \\<br>--data-urlencode 'client_secret=36f70746-fa43-4aa0-aef1-531cc14f9006'                                                                                                                                                                                                                                               |
| Response     | Status Code: 200<br>{<br> "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldU",<br> "expires_in": 3600,<br> "refresh_expires_in": 1800,<br> "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICIzMz ",<br> "token_type": "Bearer",<br> "id_token": "Dr4wTq9W07d7k3iK5T6J232CNXFngIhEXojH8nJV2jfiI8NiXQ",<br> "not-before-policy": 1667019564,<br> "session_state": "456b0443-8b89-4b4f-810d-fc5e2a04c162",<br> "scope": "openid email profile"<br>}                                                                                                                                        |
| Sample       | curl --location 'https://sso-uatvneid.dancuquocgia.com/api/idp/sso/auth/token' \\<br>--header 'accept: application/json, text/plain, \*/\*' \\<br>--header 'content-type: application/x-www-form-urlencoded' \\<br>--data-urlencode 'code=a0c106c9-26ae-450e-89e7-cc9900f95428.574c56d0-f64a-463f-9286-318d2c3200df.d6c8350e-6509-4b73-9b00-d4af9eb564ca' \\<br>--data-urlencode 'grant_type=authorization_code' \\<br>--data-urlencode 'client_id=sso-tcxdd-web' \\<br>--data-urlencode 'redirect_uri=https://tcxdd.vn/xac-thuc' \\<br>--data-urlencode 'client_secret=d3bd2a10-4be8-4810-98eb-008a22c9feae' |

- **Đặc tả đầu vào:**

| **Stt** | **Tên trường** | **Kiểu dữ liệu** | **Bắt buộc** | **Mô tả**                                                                                              |
| ------- | -------------- | ---------------- | ------------ | ------------------------------------------------------------------------------------------------------ |
| 1       | client_id      | String           | x            | Mã đăng ký của tổ chức khai thác dịch vụ                                                               |
| 2       | client_secret  | String           | x            | Secret key của tổ chức khai thác dịch vụ                                                               |
| 3       | grant_type     | String           | x            | Kiểu xác thực ‘authorization_code’                                                                     |
| 4       | code           | String           | x            | Code xác thực nhận được sau khi vneid redirect lại link của tổ chức khai thác dịch vụ                  |
| 5       | redirect_uri   | String           |              | Url chuyển tiếp nhận lại mã code sau khi xác thực thành công. (Url tổ chức khai thác dịch vụ cung cấp) |

- **Đặc tả đầu ra:**

| **Stt** | **Tên trường** | **Kiểu dữ liệu** | **Bắt buộc** | **Mô tả**                                                                                                                                                                                                                                                                                                             |
| ------- | -------------- | ---------------- | ------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1       | data           | String           | x            | Giá trị data đã được vneid mã hóa bằng thuật toán mã hóa đối xứng AES                                                                                                                                                                                                                                                 |
| 2       | key            | String           |              | Secret key của thuật toán mã hóa đối xứng AES. Key này đã được vneid mã hóa bằng thuật toán bất đối xứng RSA.<br>Nếu trường này có giá trị nghĩa là dữ liệu sẽ mã hoá<br>Nếu không có giá trị hoặc không tồn tại thì đầu ra sẽ trả giống như mục **Đặc tả giá trị nhận được sau khi giải mã data nhận được từ VNeID** |

- **Đặc tả giá trị nhận được sau khi giải mã data nhận được từ VNeID:**

| **Stt** | **Tên trường**     | **Kiểu dữ liệu** | **Bắt buộc** | **Mô tả**                                                                            |
| ------- | ------------------ | ---------------- | ------------ | ------------------------------------------------------------------------------------ |
| 1       | access_token       | String           | x            | Mã xác thực phiên đăng nhập                                                          |
| 2       | expires_in         | Number           | x            | Thời hạn xác thực phiên đăng nhập (đơn vị giây)                                      |
| 3       | refresh_expires_in | Number           | x            | Thời hạn làm mới xác thực phiên đăng nhập (đơn vị giây)                              |
| 4       | refresh_token      | String           | x            | Mã xác thực làm mới phiên đăng nhập (đơn vị giây)                                    |
| 5       | token_type         | String           | x            | Kiểu mã xác thực ‘Bearer’                                                            |
| 6       | id_token           | String           | x            | Id của mã xác thực                                                                   |
| 7       | not-before-policy  | String           |              | Thông tin trả thêm. Tổ chức khai thác dịch vụ không sử dụng các trường thông tin này |

### 4.4. API lấy thông tin user

- **Hệ thống xây dựng API:** Hệ thống Định danh và xác thực điện tử (VNeID). Nếu tổ chức khai thác dịch vụ là Doanh nghiệp thì sẽ gọi API thông qua Hệ thống Nền tảng Định danh và xác thực điện tử. Nếu tổ chức khai thác dịch vụ là Bộ ban ngành sẽ gọi trực tiếp API này qua các môi trường NDXP/VDXP/CP.NET/…
- **Mục đích:** API dùng để cung cấp thông tin user cho hệ thống Tổ chức khai thác dịch vụ.
- **API Specs:**

| Name         | Lấy thông tin user hệ thống VNeID                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Description  | Cấp thông tin user cho hệ thống Tổ chức khai thác dịch vụ                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| URL          | api/idp/third-party/userinfo                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| Method       | GET                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| Header       | • Content-Type: application/json<br>• Authorization: Bearer \[Mã xác thực phiên đăng nhập]                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| Params       |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| Request Body |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| Response     | Status Code: 200<br>{<br> Thông tin User (Tuỳ vào dừng tổ chức khai thác dịch vụ các trường thông tin sẽ khác nhau)<br>}                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| Sample       | curl --location 'https://sso-uatvneid.dancuquocgia.com/api/idp/third-party/userinfo' \\<br>--header 'Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJwMHdLc3RpenFSMmEzblU1ajlaUGdKdU14WlVzRkpNNmRKdVlPZlVOc3kwIn0.eyJleHAiOjE3NTk5NTIyNDMsImlhdCI6MTc1OTkyMzQ0MywianRpIjoib25ydHJvOjk5M2QwNWY3LTU5NzQtNDI4Yi05OWU0LTRkNWJjNDM2ZDI2MCIsImlzcyI6Imh0dHBzOi8vYWRtaW4tZGQuZGV2bGVhZC50b3AvYXV0aC9yZWFsbXMvY2l0aXplbiIsImF1ZCI6ImFjY291bnQiLCJzdWIiOiIxYjJiMzgxOC1hODdiLTRkNDMtYWEwYy1kYWQ3YzA2MjMxZWEiLCJ0eXAiOiJCZWFyZXIiLCJhenAiOiJzc28tdGN4ZGQtd2ViIiwic2lkIjoiNWQxYjkzZDEtN2VlOS00ODg1LThmNDAtNzRiODFkMWU3MmFlIiwiYWNyIjoiMSIsImFsbG93ZWQtb3JpZ2lucyI6WyJodHRwOi8vbG9jYWxob3N0OjgwODAiXSwicmVhbG1fYWNjZXNzIjp7InJvbGVzIjpbImRlZmF1bHQtcm9sZXMtY2l0aXplbiIsIm9mZmxpbmVfYWNjZXNzIiwidW1hX2F1dGhvcml6YXRpb24iXX0sInJlc291cmNlX2FjY2VzcyI6eyJhY2NvdW50Ijp7InJvbGVzIjpbIm1hbmFnZS1hY2NvdW50IiwibWFuYWdlLWFjY291bnQtbGlua3MiLCJ2aWV3LXByb2ZpbGUiXX19LCJzY29wZSI6Im9wZW5pZCBlbWFpbCBwcm9maWxlIiwiZW1haWxfdmVyaWZpZWQiOmZhbHNlLCJwcmVmZXJyZWRfdXNlcm5hbWUiOiIwMzQxOTYwMDU2OTQifQ.Y4mJu7d8HbJoSe6tfVY2FRiR33UpbtwCaebd9900tPI2v4hNg6kEdRLmN4ENmb-yygNBGerPae5JWnyy2HvNhKMZBMBgEMdvT43am5-p2as_TS3d5EQMwdpsRvYr-6EP7ASGPYLACJo2mob6SbcsBL1ccbvOh7aVPUdg3xFxCTKKsDgc-Dm2g1Dr9X14sSRQWsQ3afG_vUdkfpt38C8-lHfwxmvZL4c0bYaT9IEj_TF_5W4gbTEytHn677vd837NR8m4a-l21WTTrpCnmMkt_V8JU0Ob8L8OujzTyal_g25cLse9GsYinr3PIXsAZLCj_0F4Ia5QdYvp8UhClMB4fA' \\<br>--header 'Cookie: JSESSIONID=F27012A716F0C6C5E6CE762AB77C4CFF' |

- **Đặc tả đầu vào: N/A**
- **Đặc tả đầu ra:**

| **Stt** | **Tên trường** | **Kiểu dữ liệu** | **Bắt buộc** | **Mô tả**                                                                                                                                                                                                                                                                                                             |
| ------- | -------------- | ---------------- | ------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1       | data           | String           | x            | Giá trị data đã được vneid mã hóa bằng thuật toán mã hóa đối xứng AES                                                                                                                                                                                                                                                 |
| 2       | key            | String           |              | Secret key của thuật toán mã hóa đối xứng AES. Key này đã được vneid mã hóa bằng thuật toán bất đối xứng RSA.<br>Nếu trường này có giá trị nghĩa là dữ liệu sẽ mã hoá<br>Nếu không có giá trị hoặc không tồn tại thì đầu ra sẽ trả giống như mục **Đặc tả giá trị nhận được sau khi giải mã data nhận được từ VNeID** |

- **Đặc tả giá trị nhận được sau khi giải mã data nhận được từ VNeID:**

| **Stt** | **Tên trường** | **Định dạng** | **Mô tả**                                                         |
| ------- | -------------- | ------------- | ----------------------------------------------------------------- |
| 1       | citizenPid     | String        | Số Định danh cá nhân                                              |
| 2       | fullName       | String        | Thông tin họ, chữ đệm và tên                                      |
| 3       | birthDate      | String        | Thông tin ngày, tháng, năm sinh. Định dạng: DD-MM-YYYY            |
| 4       | accountType    | String        | Tài khoản dành cho cá nhân hoặc tổ chức<br>1 Cá nhân<br>2 Tổ chức |
| 5       | accountLevel   | Integer       | Mức độ tài khoản                                                  |

### 4.5. URL đăng xuất VneID

- **Hệ thống xây dựng URL:** Hệ thống Định danh và xác thực điện tử (VNeID). (Mọi loại tổ chức đều gọi URL này)
- **Mục đích:** URL đăng xuất VNeID (chỉ sso web gọi).
- **Cấu trúc:**

```
$[URL-LOGOUT]? client_id=$[client_id]& redirect_uri=$[redirect_uri]&…
```

| **Stt** | **Tên trường** | **Kiểu dữ liệu** | **Bắt buộc** | **Mô tả**                                                                                           |
| ------- | -------------- | ---------------- | ------------ | --------------------------------------------------------------------------------------------------- |
| 1       | URL-LOGOUT     | String           | x            | URL điều hướng sang link logout VNeID<br>Tham khảo mục Danh sách URL, Endpoint & Param cần tích hợp |
| 2       | client_id      | String           | x            | Mã đăng ký của tổ chức khai thác dịch vụ                                                            |
| 3       | redirect_uri   | String           | x            | Url chuyển tiếp sau khi đăng xuất thành công. (URL Tổ chức khai thác dịch vụ cung cấp)              |

- **Đặc tả đầu ra:** Chuyển hướng về redirect_uri của Tổ chức khai thác dịch vụ cung cấp
