BỘ CÔNG AN

**CỤC CẢNH SÁT QLHC VỀ TRẬT TỰ XÃ HỘI**

<img src="images/image1.jpg" style="width:1.6425in;height:1.278in" alt="P9#yIS1" />

**TÀI LIỆU ĐẶC TẢ KỸ THUẬT API CHO AGENT GATEWAY**

**(DÀNH CHO TỔ CHỨC DOANH NGHIỆP KHAI THÁC DỊCH VỤ)**

\*\*Hà Nội – 2024\*\*

**MỤC LỤC**

[DANH MỤC TỪ VIẾT TẮT](#danh-mục-từ-viết-tắt)

[LỊCH SỬ CẬP NHẬT](#lịch-sử-cập-nhật)

[I. MÔ HÌNH KẾT NỐI HỆ THỐNG](#i-mo-hinh-ket-noi-he-thong)

[II. LUỒNG DỮ LIỆU CHIA SẺ THÔNG TIN CÔNG DÂN](#ii-luong-du-lieu-chia-se-thong-tin-cong-dan)

[II.1. Luồng chia sẻ thông tin công dân kèm xác thực sinh trắc học xuất phát từ ứng dụng VNeID](#ii-1-luong-chia-se-thong-tin-cong-dan-kem-xac-thuc-sinh-trac-hoc)

[II.2. Đăng nhập ứng dụng của TCDN bằng tài khoản VNeID (SSO)](#ii-2-dang-nhap-ung-dung-tcdn-vneid-sso)

[II.3. Luồng chia sẻ thông tin, xác thực sinh trắc học xuất phát từ TCDN khai thác](#ii-3-luong-chia-se-thong-tin-tcdn-khai-thac)

[II.4. Rút lại sự đồng ý chia sẻ dữ liệu, xoá dữ liệu công dân đã đồng ý chia sẻ từ ứng dụng VNeID](#ii-4-rut-lai-su-dong-y-chia-se-du-lieu)

[III. ĐẶC TẢ API](#iii-dac-ta-api)

[III.1. Dịch vụ chia sẻ thông tin công dân kèm xác thực sinh trắc học từ ứng dụng VNeID](#iii-1-dich-vu-chia-se-thong-tin-cong-dan-vneid)

[III.1.1. Doanh nghiệp cung cấp đường dẫn URL redirect cho VNeID để trả kết quả cho Web/App doanh nghiệp](#iii-1-1-doanh-nghiep-cung-cap-duong-dan)

[III.1.2. API yêu cầu trả kết quả xác thực sinh trắc học và dữ liệu công dân đồng ý chia sẻ](#iii-1-2-api-yeu-cau-tra-ket-qua-xac-thuc-sinh-trac-hoc)

[III.2. Dịch vụ xác nhận chia sẻ thông tin công dân, xác thực sinh trắc học, xác thực thông tin công dân](#iii-2-dich-vu-xac-nhan-chia-se-thong-tin-cong-dan)

[III.2.1. API gửi yêu cầu chia sẻ thông tin công dân, xác thực sinh trắc học, xác thực thông tin công dân](#iii-2-1-api-gui-yeu-cau-chia-se-thong-tin-cong-dan)

[III.2.2. API thông báo kết quả xác nhận chia sẻ/ không chia sẻ của công dân và các mã lỗi khác cho ứng dụng của TCDN.](#iii-2-2-api-thong-bao-ket-qua-xac-nhan-cong-dan)

[III.2.3. API yêu cầu trả kết quả dữ liệu công dân đồng ý chia sẻ, kết quả xác thực thông tin công dân (áp dụng với các dịch vụ không bao gồm xác thực ảnh mặt)](#iii-2-3-api-yeu-cau-tra-ket-qua-khong-bao-gom-anh-mat)

[III.2.4. API yêu cầu trả kết quả xác thực sinh trắc học và dữ liệu công dân đồng ý chia sẻ, kết quả xác thực thông tin công dân (áp dụng với các dịch vụ bao gồm xác thực ảnh mặt)](#iii-2-4-api-yeu-cau-tra-ket-qua-bao-gom-anh-mat)

[III.2.5. Universal Link](#iii-2-5-universal-link)

> ⚠️ **Lưu ý:** Mục III.2.6 “API gửi yêu cầu chia sẻ thông tin định danh tổ chức, xác thực thông tin định danh tổ chức” có trong mục lục gốc nhưng không tồn tại nội dung tương ứng trong file docx nguồn.

[III.3. Đăng nhập ứng dụng của TCDN bằng tài khoản VNeID (SSO)](#iii-3-dang-nhap-tcdn-vneid-sso)

[III.3.1. URL điều hướng đăng nhập sang VNeID](#iii-3-1-url-dieu-huong-dang-nhap)

[III.3.2. Redirect_URI của tổ chức khai thác dịch vụ](#iii-3-2-redirect-uri)

[III.3.3. TCDN lấy token khi người dùng đăng nhập thành công trên ứng dụng VNeID](#iii-3-3-tcdn-lay-token)

[III.3.4. TCDN lấy thông tin công dân sau khi đã có token đăng nhập](#iii-3-4-tcdn-lay-thong-tin-cong-dan)

[III.3.5. URL đăng xuất VneID](#iii-3-5-url-dang-xuat-vneid)

[III.4. Yêu cầu rút lại sự đồng ý chia sẻ dữ liệu, xoá dữ liệu công dân đã đồng ý chia sẻ từ ứng dụng VNeID](#iii-4-yeu-cau-rut-lai-su-dong-y)

[III.4.1. API nhận yêu cầu rút lại sự đồng ý chia sẻ dữ liệu, xóa dữ liệu công dân đã đồng ý chia sẻ từ ứng dụng VNeID](#iii-4-1-api-nhan-yeu-cau-rut-lai)

[III.4.2. API trả kết quả xử lý yêu cầu xóa dữ liệu công dân đã đồng ý chia sẻ](#iii-4-2-api-tra-ket-qua-xu-ly-xoa-du-lieu)

[III.5. Ứng dụng doanh nghiệp trả kết quả xử lý dữ liệu công dân](#iii-5-ung-dung-doanh-nghiep-tra-ket-qua)

[III.5.1. API Specs](#iii-5-1-api-specs)

[III.5.2. Request body](#iii-5-2-request-body)

[III.5.3. Response body](#iii-5-3-response-body)

[III.6. Dịch vụ xác nhận chia sẻ thông tin định danh tổ chức, xác thực thông tin định danh tổ chức](#iii-6-dich-vu-xac-nhan-dinh-danh-to-chuc)

[III.6.1. API gửi yêu cầu chia sẻ thông tin định danh tổ chức, xác thực thông tin định danh tổ chức](#iii-6-1-api-gui-yeu-cau-dinh-danh-to-chuc)

[III.6.2. API thông báo kết quả xác nhận chia sẻ/ không chia sẻ của tổ chức và các mã lỗi khác cho ứng dụng của TCDN.](#iii-6-2-api-thong-bao-ket-qua-to-chuc)

[III.6.3. API yêu cầu trả kết quả dữ liệu định danh tổ chức sau khi người đại diện pháp luật đồng ý chia sẻ, kết quả xác thực thông tin định danh tổ chức](#iii-6-3-api-yeu-cau-tra-ket-qua-dinh-danh-to-chuc)

[III.6.4. Universal Link](#iii-6-4-universal-link)

[III.7. Dịch vụ xác nhận chia sẻ thông tin định danh tổ chức, cá nhân kèm dịch vụ chứng thư số](#iii-7-dich-vu-xac-nhan-dinh-danh-to-chuc-ca-nhan)

[III.7.1. API gửi yêu cầu chia sẻ thông tin định danh tổ chức, cá nhân](#iii-7-1-api-gui-yeu-cau-dinh-danh-to-chuc-ca-nhan)

[III.7.2. API thông báo kết quả xác nhận chia sẻ/ không chia sẻ của tổ chức, cá nhân và các mã lỗi khác cho ứng dụng của TCDN.](#iii-7-2-api-thong-bao-ket-qua-to-chuc-ca-nhan)

[III.7.3. API yêu cầu trả kết quả dữ liệu định danh tổ chức, cá nhân](#iii-7-3-api-yeu-cau-tra-ket-qua-dinh-danh-to-chuc-ca-nhan)

[III.7.4. Universal Link](#iii-7-4-universal-link)

[III.7.5. API thông báo kết quả đăng ký chứng thư số cho CA.](#iii-7-5-api-thong-bao-dang-ky-chung-thu-so)

[IV. BẢNG MÃ DÙNG CHUNG TRONG BẢN TIN API](#iv-bang-ma-dung-chung)

[IV.1. Mã lỗi đáp ứng HTTP (HTTP Status Codes)](#iv-1-ma-loi-http-status-codes)

[IV.2. Mã lỗi dịch vụ](#iv-2-ma-loi-dich-vu)

[IV.3. Mã dịch vụ](#iv-3-ma-dich-vu)

[IV.4. Mã responseCode, resultCode](#iv-4-ma-responsecode-resultcode)

[IV.5. Mã danh mục dùng chung](#iv-5-ma-danh-muc-dung-chung)

[IV.5.1. Mã danh mục giới tính (genderCode)](#iv-5-1-ma-danh-muc-gioi-tinh)

[IV.5.2. Danh mục loại hóa đơn (áp dụng cho billing-history)](#iv-5-2-danh-muc-loai-hoa-don)

[IV.5.3. Danh mục Trạng thái (áp dụng cho billing-history)](#iv-5-3-danh-muc-trang-thai)

> ⚠️ **Lưu ý:** Mục lục gốc còn có mục “V. HỖ TRỢ TÍCH HỢP”, nhưng nội dung mục này không tồn tại trong file docx nguồn (đã xác nhận bằng cách export văn bản thuần từ file gốc — nội dung tài liệu kết thúc ngay sau mục IV.5.3).

# DANH MỤC TỪ VIẾT TẮT

| **STT** | **Từ viết tắt** | **Ý nghĩa**                                  |
| :-----: | :-------------: | -------------------------------------------- |
|    1    |       NT        | Nền tảng định danh và xác thực điện tử       |
|    2    |     AgentGW     | Agent Gateway                                |
|    3    |      TCDN       | Tổ chức doanh nghiệp khai thác               |
|    4    |      TTXT       | Trung tâm xác thực điện tử                   |
|    5    |      VNeID      | Hệ thống định danh và xác thực điện tử VNeID |

#

# LỊCH SỬ CẬP NHẬT

<table>
<colgroup>
<col style="width: 19%" />
<col style="width: 13%" />
<col style="width: 66%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>Ngày cập nhật</strong></th>
<th style="text-align: center;"><strong>Phiên bản</strong></th>
<th style="text-align: center;"><strong>Nội dung</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: center;">17/07/2024</td>
<td style="text-align: center;">2.6.9</td>
<td>- Cập nhật tài liệu VNeID-SSO-V2.1, bổ sung mã hoá dữ liệu</td>
</tr>
<tr>
<td style="text-align: center;">17/07/2024</td>
<td style="text-align: center;">2.8.0</td>
<td>- Điều chỉnh request body, response body API 4.3</td>
</tr>
<tr>
<td style="text-align: center;">18/07/2024</td>
<td style="text-align: center;">2.8.1</td>
<td><p>- Bổ sung trường transactionId trong Request body API III.2.</p>
<p>- Làm rõ thông tin object userInfo trong Response body API III.4.3</p></td>
</tr>
<tr>
<td style="text-align: center;">19/07/2024</td>
<td style="text-align: center;">2.8.2</td>
<td>- Cập nhật bổ sung mã kết quả trả về (công dân chưa xác nhận, hết thời gian xác nhận) trong Request body API III.2</td>
</tr>
<tr>
<td style="text-align: center;">22/07/2024</td>
<td style="text-align: center;">2.8.3</td>
<td><p>- Cập nhật URL API tại III.2: từ /api/tcdn/{service-code}/result thành /api/tcdn/{service-code}/agent/account/receive-result</p>
<p>- Bổ sung mã dịch vụ tại mục IV.3: create-account, create-credit-card, create-ewallet-account, create-account-result</p></td>
</tr>
<tr>
<td style="text-align: center;">23/07/2024</td>
<td style="text-align: center;">2.8.4</td>
<td><p>- Cập nhật URL API tại III.4.3: từ /thirdparty/get-transaction thành /api/get-transaction</p>
<p>- Cập nhật tài liệu tích hợp SSO (v2.2) tại mục III.3</p></td>
</tr>
<tr>
<td style="text-align: center;">25/07/2024</td>
<td style="text-align: center;">2.8.5</td>
<td><p>- Cập nhật thời gian hết hiệu lực của transaction tại API III.4.3</p>
<p>- Cập nhật mã trạng thái tiếp nhận responseCode trong Respond body của API III.4.3.</p>
<p>- Bổ sung mô tả URL APP tại mục III.4.1.3 và UniversalLink tại mục III.4.4</p>
<p>- Bổ sung mã dịch vụ tại mục IV.3: billing-history</p></td>
</tr>
<tr>
<td style="text-align: center;">31/07/2024</td>
<td style="text-align: center;">2.8.6</td>
<td><p>- Cập nhật tài liệu tích hợp SSO (v2.3) tại mục III.3</p>
<p>- Cập nhật bỏ bắt buộc với trường fullName, birthdate trong API tại mục III.4.1 (chỉ bắt buộc citizenPid).</p>
<p>- Cập nhật nêu rõ trường hợp không sử dụng liveness, ký số thì các object tương ứng không trả về trong response tại mục III.4.3</p>
<p>- Cập nhật mô tả chi tiết lỗi đối với API tại mục III.4.3</p>
<p>- Bổ sung mã dịch vụ tại mục IV.3: create-smartca-account</p></td>
</tr>
<tr>
<td style="text-align: center;">01/08/2024</td>
<td style="text-align: center;">2.8.7</td>
<td>- Cập nhật API mục III.4.2: chuyển trường transactionId thành txnId</td>
</tr>
<tr>
<td style="text-align: center;">04/08/2024</td>
<td style="text-align: center;">2.8.8</td>
<td>- Bổ sung mã lỗi -15, -16 trong mục IV.4</td>
</tr>
<tr>
<td style="text-align: center;">19/08/2024</td>
<td style="text-align: center;">2.8.9</td>
<td>- Bổ sung mã lỗi -17, -18, -19, -20 trong mục IV.4</td>
</tr>
<tr>
<td style="text-align: center;">21/08/2024</td>
<td style="text-align: center;">2.9.0</td>
<td><p>- Cập nhật bản tin trả về đối với object thông tin kết quả xác thực sinh trắc học trong response body tại mục III.4.3</p>
<p>- Bỏ trường permanentCountryCode, permanentCountryText thuộc object userinfo trong response body tại mục III.4.3</p>
<p>- Hiệu đính mã dịch vụ sso-token và sso-userinfo tại mục IV.3</p></td>
</tr>
<tr>
<td style="text-align: center;">27/08/2024</td>
<td style="text-align: center;">2.9.1</td>
<td><p>- Tách mục III.4.3.4 (cũ) chuyển thành mục IV.4. Mã lỗi respondCode (dùng chung).</p>
<p>- Cập nhật mô tả đối với mã lỗi -17 trong mục IV.4</p>
<p>- Bổ sung trường idCardExpireDate trong thuộc object userinfo trong response body tại mục III.4.3.</p></td>
</tr>
<tr>
<td style="text-align: center;">10/09/2024</td>
<td style="text-align: center;">2.9.2</td>
<td><p>- Bổ sung request body trong mục III.2 đối với các trường hợp trả kết quả mở ví điện tử &amp; đăng ký chứng thư số.</p>
<p>- Bổ sung dịch vụ verify-info tại mục III.4</p></td>
</tr>
<tr>
<td style="text-align: center;">01/10/2024</td>
<td style="text-align: center;">2.9.3</td>
<td>- Chuyển định dạng ngày tháng từ dạng dd/MM/yyyy sang dd-MM-yyyy</td>
</tr>
<tr>
<td style="text-align: center;">10/10/2024</td>
<td style="text-align: center;">2.9.4</td>
<td><p>- Gộp dịch vụ xác thực thông tin công dân (III.6 cũ) vào mục III.4 thành Dịch vụ xác nhận chia sẻ thông tin công dân, xác thực sinh trắc học, xác thực thông tin công dân.</p>
<p>- Bổ sung target_code trong header phục vụ hiển thị đúng nội dung mục đích chia sẻ, xử lý thông tin trên ứng dụng VNeID.</p>
<p>- Bổ sung trả các trường thông tin tên địa giới hành chính (song song với mã địa giới hành chính) trong khối userInfo, gồm:</p>
<p>permanentVillageText, permanentCityText, livingPlaceVillageText, livingPlaceCityText</p></td>
</tr>
<tr>
<td style="text-align: center;">15/11/2024</td>
<td style="text-align: center;">2.9.5</td>
<td><p>- Bỏ các mã RECEPTION_FAILED, RECEIVED của trường requestStatus trong Request body API tại mục III.2 (Ứng dụng TCDN trả kết quả xử lý). Đồng thời tách mô tả các response body riêng tương ứng với từng dịch vụ.</p>
<p>- Hiệu chỉnh mô tả trường resultCode trong Request body API tại mục III.4.2.</p>
<p>- Bổ sung luồng lại sự đồng ý chia sẻ dữ liệu, xoá dữ liệu công dân đã đồng ý chia sẻ từ ứng dụng VNeID tại mục II.4 và API tương ứng tại mục III.5; bổ sung mã dịch vụ withdraw-consent và withdraw-consent-result tại mục V.3.</p>
<p>- Bổ sung mã dịch vụ mới tại mục V.3 gồm: payment-result và vnpay-querydr .</p>
<p>- Bổ sung III.4.4. API yêu cầu trả kết quả xác thực sinh trắc học và dữ liệu công dân đồng ý chia sẻ, kết quả xác thực thông tin công dân (version 2 – Có ký số kết quả xác thực ảnh mặt).</p>
<p>- Bỏ object signature trong response body API III.4.3, III.4.4</p>
<p>- Chuyển mục Universal Link từ III.4.4 thành mục III.4.5 (nội dung không thay đổi).</p>
<p>- Bổ sung mã danh mục loại hóa đơn, trạng thái thanh toán tương ứng với dịch vụ billing-history.</p>
<p>- Bổ sung yêu cầu TCDN cung cấp đường link công bố “Điều khoản và cam kết sử dụng dữ liệu được chia sẻ” tại mục III.4; “Công bố thông tin rủi ro khi công dân rút lại sự đồng ý chia sẻ dữ liệu, tổ chức doanh nghiệp thực hiện xoá dữ liệu công dân đã đồng ý chia sẻ” tại mục III.5.</p></td>
</tr>
<tr>
<td style="text-align: center;">11/12/2024</td>
<td style="text-align: center;">2.9.6</td>
<td><p>- Thay đổi URL tại mục III.4.4</p>
<p>Trước: /{service-code}/<mark>api/v2/</mark>get-transaction</p>
<p>Sau: /{service-code<mark>}/v2/api/</mark>get-transaction</p></td>
</tr>
<tr>
<td style="text-align: center;">26/12/2024</td>
<td style="text-align: center;">2.9.7</td>
<td><p>- TrongThay đổi userInfo từ định dạng base64 encode sang base64Url Encode (vẫn tuân theo chuẩn JWS), trong đó &lt;signature&gt; gồm 2 phần như sau:</p>
<p>base64UrlEncode(header).base64UrlEncode(payload)</p>
<p>- Cập nhật các ví dụ mẫu trong các mục III.2, III.4.3, III.4.4.</p>
<p>- Bổ sung danh mục mã giới tính (genderCode) tại mục IV.5.</p></td>
</tr>
<tr>
<td style="text-align: center;">17/01/2025</td>
<td style="text-align: center;">2.9.8</td>
<td><p>- Bổ sung mô tả header, request body, respond body trong mục III.3.3 và III.3.4</p>
<p>- Cập nhật bản tin đối với API mục III.5 (Rút lại sự đồng ý, yêu cầu xóa dữ liệu đã chia sẻ)</p></td>
</tr>
<tr>
<td style="text-align: center;">20/03/2025</td>
<td style="text-align: center;">2.9.9</td>
<td><p>- Điều chỉnh giá trị requestStatus trong response body của API III.5.1 từ WITH-DRAW sang DELETED.</p>
<p>- Bổ sung HTTP Status Codes: 501 tại mục IV.1</p>
<p>- Bổ sung mục III.6: Dịch vụ xác nhận chia sẻ thông tin định danh tổ chức, xác thực thông tin định danh tổ chức</p></td>
</tr>
<tr>
<td style="text-align: center;">22/05/2025</td>
<td style="text-align: center;">2.9.9f</td>
<td>- Bổ sung mục III.6: Dịch vụ xác nhận chia sẻ thông tin định danh tổ chức, xác thực thông tin định danh tổ chức</td>
</tr>
<tr>
<td style="text-align: center;">24/6/2025</td>
<td style="text-align: center;">3.0</td>
<td><p>- Điều chỉnh trả null các giá trị cấp huyện tại các dịch vụ</p>
<p>- Sắp xếp lại thứ tự các api để khớp với luồng dữ liệu</p></td>
</tr>
<tr>
<td style="text-align: center;">24/7/2025</td>
<td style="text-align: center;">3.0d</td>
<td><p id="bổ-sung-trạng-thái-chia-sẻ-dữ-liệu-tại-mục-iv.4-mã-responsecode-resultcode">- Bổ sung trạng thái chia sẻ dữ liệu tại mục IV.4 Mã responseCode, resultCode;</p>
<p id="update-mô-tả-các-luồng-chia-sẻ-dữ-liệu">- Update mô tả các luồng chia sẻ dữ liệu</p>
<p>- Bổ sung mục III.5.2.8. Trả kết quả thông tin bảo hiểm (dịch vụ register-insurance)</p></td>
</tr>
<tr>
<td style="text-align: center;">15/8/2025</td>
<td style="text-align: center;">3.1</td>
<td><p id="bổ-sung-mục-iii.7.-dịch-vụ-xác-nhận-chia-sẻ-thông-tin-định-danh-tổ-chức-cá-nhân" class="A_Heading-2">- Bổ sung mục III.7. Dịch vụ xác nhận chia sẻ thông tin định danh tổ chức, cá nhân</p></td>
</tr>
</tbody>
</table>

**TÀI LIỆU ĐẶC TẢ KỸ THUẬT API CHO AGENT GATEWAY**

**(CỔNG TIẾP NHẬN CHO TỔ CHỨC DOANH NGHIỆP)**



Tài liệu này đặc tả API kết nối cho tổ chức doanh nghiệp với Nền tảng định danh và xác thực điện tử. Tài liệu này nêu giải pháp kỹ thuật phục vụ triển khai, kết nối tích hợp dữ liệu giữa Nền tảng định danh và xác thực điện tử với các tổ chức doanh nghiệp dân sự khai thác thông tin định danh công dân.

# I. MÔ HÌNH KẾT NỐI HỆ THỐNG {#i-mo-hinh-ket-noi-he-thong}

<img src="images/image2.png" style="width:6.49653in;height:2.93593in" alt="P74#yIS1" />

4.  _Mô hình kết nối giữa TCDN đến TTXT_

# II. LUỒNG DỮ LIỆU CHIA SẺ THÔNG TIN CÔNG DÂN {#ii-luong-du-lieu-chia-se-thong-tin-cong-dan}

## II.1. Luồng chia sẻ thông tin công dân kèm xác thực sinh trắc học xuất phát từ ứng dụng VNeID {#ii-1-luong-chia-se-thong-tin-cong-dan-kem-xac-thuc-sinh-trac-hoc}

**Mục đích:**

Khi công dân đăng nhập thanh công, chọn dịch vụ của doanh nghiệp (mở tài khoản ngân hàng, thẻ tín dụng, chứng thư số…) trên ứng dụng VNeID để chia sẻ thông tin trong cơ sở dữ liệu quốc gia về dân cư, hệ thống định danh và xác thực điện tử, cho phép xác thực sinh trắc học với dữ liệu sinh trắc đã được lưu trong hệ thống cơ sở dữ liệu căn cước.

<img src="images/image3.png" style="width:6.49653in;height:2.76458in" />

_Sơ đồ luồng chia sẻ thông tin công dân kèm xác thực sinh trắc học từ ứng dụng VNeID_

**Mô tả luồng:**

- **Bước 1:** Trên ứng dụng VNeID, công dân thực hiện gửi yêu cầu dịch vụ và thực hiện xác nhận chia sẻ thông tin. Yêu cầu sau đó được Hệ thống định danh (VNeID-BE) gửi sang hệ thống Nền tảng định danh (NEID). Nền tảng định danh tiếp nhận yêu cầu và trả lại kết quả tiếp nhận.

  Nền tảng định danh thực hiện xử lý dữ liệu công dân đồng ý chia sẻ và ký số bản tin dữ liệu. (_(Dựa theo yêu cầu của TCDN, Nền tảng định danh có thể thực hiện trả kết quả công dân xác nhận chia sẻ consent về hệ thống TCDN)._

- **Bước 2:** Từ ứng dụng VNeID, công dân thao tác để chuyển hướng sang ứng dụng của TCDN _(luồng App to App thông qua Universal link TCDN, gửi kèm consentId)_ để thực hiện tiếp các bước nghiệp vụ trên ứng dụng của TCDN (nếu có). Dựa vào consentId nhận được, Ứng dụng của TCDN gửi yêu cầu tra cứu thông tin công dân chia sẻ theo mã giao dịch _(API III.2.3. đối với DN không tích hợp dịch vụ không có sinh trắc hoặc và API III.2.4. đối với DN có tích hợp dịch vụ có xác thực ảnh mặt)_ và nhận về thông tin để xử lý nghiệp vụ trên ứng dụng của TCDN.

- **Bước 3:** Sau khi xử lý nghiệp vụ, hệ thống của TCDN thực hiện trả kết quả xử lý yêu cầu của công dân thông _(API III.5)_ qua Agent GW (encrypted data) và hệ thống TTDL (decrypted data). Công dân sẽ nhận được kết quả xử lý yêu cầu dịch vụ trực tiếp trên ứng dụng VNeID.

  **\* Chú ý:** Với luồng dịch vụ xuất phát từ APP VNeID, nếu TCDN đã thực hiện đã gửi yêu cầu tra cứu thông tin công dân*(API III.2.3. đối với DN không tích hợp dịch vụ không có sinh trắc hoặc và API III.2.4. đối với DN có tích hợp dịch vụ có xác thực ảnh mặt)* thì bắt buộc phải trả kết quả xử lý _(API III.5)_ để cập nhật kết quả trên APP VNeID. Nếu không thực hiện trả kết quả thì công dân sẽ không thể thực hiện giao dịch mới và treo giao dịch cũ đến khi TCDN trả kết quả xử lí (Vì VNeID không có thông tin kết quả xử lí nên không thể tự cập nhật trạng thái của giao dịch đó).

## II.2. Đăng nhập ứng dụng của TCDN bằng tài khoản VNeID (SSO) {#ii-2-dang-nhap-ung-dung-tcdn-vneid-sso}

**Mục đích:**

Tích hợp đăng nhập một lần (SSO - Single Sign On) giữa ứng dụng TCDN và VNeID, cho phép công dân đăng nhập ứng dụng của TCDN bằng tài khoản VNeID.

5.  ![](images/image4.png)

Sơ đồ luồng đăng nhập ứng dụng của TCDN bằng tài khoản VNeID (SSO)

**Mô tả luồng đăng nhập:**

- Người dùng yêu cầu đăng nhập trên ứng dụng (Web/App mobile) thuộc đơn vị kết nối cần tích hợp SSO với hệ thống VNeID

- Web/App của đơn vị kết nối sẽ thực hiện chuyển hướng (redirect) tới Web/App VNeID để tiến hành đăng nhập

- Sau khi người dùng đăng nhập và xác thực thành công trên Web/App VNeID, hệ thống Định danh và xác thực điện tử chuyển hướng sang Web/App của đơn vị kết nối đồng thời trả mã code cho đơn vị kết nối.

- Web/App của đơn vị kết nối sử dụng mã Code này để lấy Access-Token từ Web/App VNeID thông qua Hệ thống TCCCDV và Nền tảng định danh và xác thực điện tử

- Web/App của đơn vị kết nối khởi tạo phiên đăng nhập, sử dụng Access-Token để truy vấn thông tin chia sẻ từ hệ thống Định danh và xác thực điện tử bằng hình thức qua Hệ thống TCCCDV và Nền tảng định danh và xác thực điện tử

**Mô tả luồng đăng xuất:**

- Người dùng yêu cầu đăng xuất trên Web của đơn vị kết nối

- Hệ thống của đơn vị kết nối sẽ thực hiện gọi yêu cầu đăng xuất sang Web VNeID

- Web VNeID đăng xuất và trả lại thông báo đăng xuất cho Web của đơn vị kết nối

- Web của đơn vị kết nối thực hiện xóa phiên đăng nhập của người dùng.

**Lưu ý:** Luồng đăng xuất chỉ áp dụng đối với WebSSO, không áp dụng cho App mobileSSO.

## II.3. Luồng chia sẻ thông tin, xác thực sinh trắc học xuất phát từ TCDN khai thác {#ii-3-luong-chia-se-thong-tin-tcdn-khai-thac}

**Mục đích:**

Khi công dân thực hiện xác nghiệp vụ tại doanh nghiệp (web, app hoặc trực tiếp) để thực hiện mở tài khoản, đăng ký chứng thư số… hệ thống của TCDN thực hiện gọi sang hệ thống định danh và xác thực điện tử để yêu cầu công dân chia sẻ thông tin, xác thực thông tin, xác thực sinh trắc học và lấy dữ liệu về phục vụ các hoạt động nghiệp vụ của TCDN sau khi công dân đã đồng ý chia sẻ trên ứng dụng VNeID.

<img src="images/image5.png" style="width:6.49653in;height:2.75in" />

_Sơ đồ luồng chia sẻ thông tin kèm xác thực sinh trắc từ doanh nghiệp khai thác_

**Mô tả luồng:**

- **Bước 1:** Ứng dụng của TCDN khai thác thực hiện gửi yêu cầu khởi tạo giao dịch (API II.2.1) kèm theo dịch vụ cần công dân xác nhận chia sẻ (chia sẻ thông tin, xác thực sinh trắc học hoặc cả hai) thông qua Agent GW đến Tổ chức cung cấp dịch vụ và chuyển tiếp đến Nền tảng định danh. Nền tảng định danh sẽ kiểm tra yêu cầu và trả về mã giao dịch (transactionId) cho ứng dụng của TCDN khai thác, đồng thời đồng bộ mã giao dịch này với Hệ thống định danh (VNeID-BE)

- **Bước 2:** Dựa trên mã giao dịch (transactionId) nhận được, ứng dụng của TCDN khai thác điều hướng công dân sang ứng dụng VNeID _(luồng App to App thông qua Universal link VNeID, gửi kèm transactionId)_ để thực hiện xác nhận chia sẻ thông tin, xác thực chia sẻ sinh trắc học trên ứng dụng VNeID. _(Dựa theo yêu cầu của TCDN, Nền tảng định danh có thể thực hiện thông báo về hệ thống TCDN trạng thái nhận thông tin xác nhận chia sẻ trên ứng dụng VNeID. Sau khi nhận được thông tin xác nhận chia sẻ, TCDN thực hiện điều hướng công dân sang ứng dụng VNeID)._

  **\* Giải thích ý nghĩa API III.2.2 Thông báo kết quả xác nhận:** Sau khi TCDN thực hiện Bước 1, Nền tảng định danh sẽ thực hiện check thông tin công dân với hệ thống định danh điện tử.\
  - Nếu thông tin công dân không hợp lệ, tài khoản VNeID của công dân đang mức 1, không tồn tại, bị khoá, hết hạn,…. Hệ thống sẽ thực hiện trả kết quả kèm mã lỗi ngay lập tức cho BE TCDN để TCDN nắm thông tin về tài khoản VNeID của công dân và thực hiện dừng tiếp tục các nghiệp vụ.

  **+** Nếu thông tin công dân đã hợp lệ:\
  **Trường hợp 1:** Nếu TCDN có nhu cầu thông báo trạng thái đã hiển thị màn xác nhận chia sẻ trên ứng dụng VNeID _(Trạng thái INIT)_, Nền tảng định danh sẽ thực hiện trả qua _API III.2.2_, sau đó TCDN sẽ thực hiện điều hướng dân sang ứng dụng VNeID để xác nhận chia sẻ. Sau khi công dân đã xác nhận chia sẻ/không chia sẻ, Nền tảng định danh sẽ thực hiện trả trạng thái qua _API III.2.2_ để TCDN thực hiện tiếp Bước 3.

  **Trường hợp 2:** Nếu TCDN không có nhu cầu nhận trạng thái đã hiển thị màn xác nhận chia sẻ trên ứng dụng VNeID _(Trạng thái INIT)_, TCDN thực hiện điều hướng App to App sang ứng dụng VNeID sau khi không có thông tin callback _(API III.2.2)_ Nền tảng định danh trả về thông báo lỗi liên quan đến tài khoản VNeID của công dân trong tối đa 3s kể từ lúc khởi tạo giao dịch ở _Bước 1_. Sau khi công dân đã xác nhận chia sẻ/không chia sẻ, Nền tảng định danh sẽ thực hiện trả trạng thái qua _API III.2.2_ để TCDN thực hiện tiếp _Bước 3_.

- **Bước 3:** Trên ứng dụng VNeID, công dân thực hiện xác nhận chia sẻ thông tin, xác thực chia sẻ sinh trắc học (tùy theo dịch vụ được yêu cầu). Sau khi hoàn tất xác nhận chia sẻ, công dân được điều hướng về ứng dụng của TCDN khai thác _(luồng App to App thông qua Universal link TCDN, gửi kèm consentId + result)_

- **Bước 4:** Dựa vào consentId nhận được, Ứng dụng của TCDN khai thác gửi yêu cầu trả kết quả xác thực sinh trắc học và dữ liệu công dân đồng ý chia sẻ _(API II.2.3)._ Tổ chức cung cấp dịch vụ nhận được yêu cầu trả kết quả sẽ dựa trên dịch vụ TCDN đang yêu cầu và thực hiện: (1) lấy dữ liệu công dân đồng ý chia sẻ từ Nền tảng định danh (thông tin userInfo được mã hóa); (2) xác thực thông tin sinh trắc học; sau đó tổng hợp kết quả trả về để xử lý tiếp nghiệp vụ trên ứng dụng của TCDN khai thác. _(Hoặc dựa theo yêu cầu của TCDN, Nền tảng định danh có thể thực hiện thông báo về hệ thống TCDN trạng thái kết quả đã chia sẻ dữ liệu trên VNeID)_

  \*\*\* TCDN khai thác có thể sử dụng các thư viện tiêu chuẩn để verify chữ ký (với public key được C06 cung cấp) và parse dữ liệu payload để có được thông tin công dân.

## II.4. Rút lại sự đồng ý chia sẻ dữ liệu, xoá dữ liệu công dân đã đồng ý chia sẻ từ ứng dụng VNeID {#ii-4-rut-lai-su-dong-y-chia-se-du-lieu}

**Mục đích:**

Để đảm bảo quyền lợi của công dân theo quy định của Nghị định 13/2023/NĐ-CP ngày 17 tháng 4 năm 2023 của Chính phủ, công dân có thể rút lại sự đồng ý đối với các đồng ý chia sẻ dữ liệu nhưng TCDN chưa thực hiện lấy dữ liệu hoặc các đồng ý dài hạn (“Không hỏi lại”) của dịch vụ SSO. Đồng thời, công dân có thể yêu cầu TCDN xoá dữ liệu đã chia sẻ.

![](images/image6.png)

Sơ đồ luồng rút lại sự đồng ý chia sẻ dữ liệu, xoá dữ liệu công dân đã đồng ý chia sẻ từ ứng dụng VNeID

6.  **Mô tả luồng:**

- **Bước 1:** Trên ứng dụng VNeID, công dân thực hiện gửi “Yêu cầu rút lại sự đồng ý chia sẻ dữ liệu, xoá dữ liệu công dân đã đồng ý chia sẻ” với một tổ chức doanh nghiệp và xác nhận hành động này. Ứng dụng VNeID gửi yêu cầu xuống back-end của Hệ thống định danh (VNeID-BE) để xử lý.

- **Bước 2:** Hệ thống định danh (VNeID-BE) tiếp nhận yêu cầu và thực hiện kiểm tra trạng thái của consentId gửi lên đã được TCDN lấy dữ liệu công dân chia sẻ hay chưa?

7.  Trường hợp TCDN chưa lấy dữ liệu công dân về hệ thống của TCDN thì chuyển Bước 3 (luồng số 1 trong sơ đồ).

    Ngược lại, trường hợp TCDN đã lấy dữ liệu công dân về hệ thống TCDN thì chuyển Bước 5 (luồng số 2 trong sơ đồ).

- **Bước 3:** (TCDN chưa lấy dữ liệu) Hệ thống định danh (VNeID-BE) thực hiện cập nhật trạng thái chia sẻ của consentId trên VNeID và gửi yêu cầu cập nhật trạng thái chia sẻ kèm consentId sang Nền tảng định danh (NEID). Quy trình chuyển tiếp Bước 4

- **Bước 4:** Nền tảng định danh (NEID) Hệ thống định danh thực hiện chuyển tiếp yêu cầu kèm consentId trên NEID cho TCDN. Hệ thống TCDN tiếp nhận và phản hồi kết quả tiếp nhận bản tin.

- **Bước 5:** (TCDN đã lấy dữ liệu) Hệ thống định danh (VNeID-BE) gửi yêu cầu cập nhật trạng thái chia sẻ kèm consentId sang Nền tảng định danh (NEID). Quy trình chuyển tiếp Bước 6.

- **Bước 6:** Nền tảng định danh (NEID) chuyển tiếp yêu cầu xóa dữ liệu công dân đã đồng ý chia sẻ _(kèm consentId)_ cho Hệ thống của TCDN (thông qua Tổ chức cung cấp dịch vụ 🡪 AgentGW). Hệ thống của TCDN phản hồi đã nhận yêu cầu lại cho Nền tảng và tiến hành xử lý nghiệp vụ ở phía TCDN (trong tối đa 72 giờ phải trả kết quả).

- **Bước 7:** Sau khi xử lý xong, Hệ thống TCDN thực hiện trả kết quả yêu cầu xoá dữ liệu công dân đã chia sẻ về cho Nền tảng định danh (thông qua AgentGW 🡪 Tổ chức cung cấp dịch vụ). Nền tảng định danh chuyển tiếp kết quả về cho Hệ thống định danh (VNeID) đồng thời cập nhật trạng thái chia sẻ của consentId.

- **Bước 8:** Hệ thống định danh (VNeID) tiếp nhận kết quả, cập nhật thông tin và thông báo cho công dân trên ứng dụng VNeID. Sau đó phản hồi đã nhận kết quả thành công cho Nền tảng định danh, Hệ thống của TCDN và kết thúc luồng rút lại sự đồng ý chia sẻ, xoá dữ liệu đã chia sẻ trong trường hợp TCDN đã lấy dữ liệu tại đây.

  **\* Tóm tắt luồng nghiệp vụ rút lại chia sẻ:**\
  **Trường hợp 1:** Nếu công dân đồng ý trên app -\> TCDN chưa get trans lấy dữ liệu -\> Công dân thực hiện y/c rút lại -\> VNeID gọi sang TCDN qua _API III.4.1_ thì TCDN trả response ở _API III.4.1_ với _"requestStatus": " DELETED"_ và dừng luồng rút lại _(Không phải gọi api III.4.2 trả kết quả)_

  **Trường hợp 2:** Nếu công dân đồng ý trên app -\> TCDN đã get trans lấy dữ liệu -\> Công dân thực hiện y/c rút lại -\> VNeID gọi sang TCDN qua _API III.4.1_ thì TCDN trả response ở api _API III.4.1_ với _"requestStatus": " PROCESSING"_ -\> Sau tối đa 72h làm việc vs công dân, TCDN gọi trả kết quả ở _API III.4.2_ với:

  _"requestStatus": "DELETED"_ -\> Đã xoá dữ liệu công dân

  _"requestStatus": "REJECT"_ kèm "reasonRequestCode" _(Mã responseCode này TCDN defined trước trong sheet đăng kí dịch vụ)_ với "reasonRequestText" _(Chuỗi Text này cần được mô tả rõ lí do từ chối kèm dẫn chứng Pháp luật)_ để hiển thị trên app VNeID.

# III. ĐẶC TẢ API {#iii-dac-ta-api}

## III.1. Dịch vụ chia sẻ thông tin công dân(có thể kèm dịch vụ sinh trắc học) xuất phát từ ứng dụng VNeID {#iii-1-dich-vu-chia-se-thong-tin-cong-dan-vneid}

### III.1.1. Doanh nghiệp cung cấp đường dẫn Universal link(Đối với APP) và URL redirect(Đối với Web) cho VNeID để Switch App to App hoặc App to Web cho Web/App doanh nghiệp {#iii-1-1-doanh-nghiep-cung-cap-duong-dan}

| **Thông tin** | **Mô tả**                                                                                                           |
| :------------ | :------------------------------------------------------------------------------------------------------------------ |
| HT xây dựng   | Hệ thống của tổ chức doanh nghiệp khai thác xây dựng và cung cấp                                                    |
| Mục đích      | Dùng để ứng dụng VNeID switch App to App hoặc App to Web trả TransactionId về cho hệ thống tổ chức sử dụng dịch vụ. |
| URL           | URL_TCDN_Khai_Thac/\[\$transactionCode\]                                                                            |

### III.1.2. API yêu cầu trả kết quả xác thực sinh trắc học và dữ liệu công dân đồng ý chia sẻ {#iii-1-2-api-yeu-cau-tra-ket-qua-xac-thuc-sinh-trac-hoc}

**Lưu ý:**

- transactionId sẽ hết hạn sau 30 phút kể từ thời điểm ghi nhận thành công việc công dân đã thực hiện xác nhận trên VNeID.

- TCDN chỉ có thể gọi lấy dữ liệu tối đa 03 lần trong khoảng thời gian 30 phút, quá thời gian trên sẽ không nhận được dữ liệu công dân đã chia sẻ.

<img src="images/image7.png" style="width:4.77778in;height:2.5in" />

<table>
<colgroup>
<col style="width: 15%" />
<col style="width: 84%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>Thông tin</strong></th>
<th style="text-align: center;"><strong>Mô tả</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: left;">Mục đích</td>
<td style="text-align: left;">API cung cấp cho TCDN khai thác truy vấn thông tin công dân đã đồng ý chia sẻ (kèm kết quả xác thực sinh trắc học - nếu được cấu hình).</td>
</tr>
<tr>
<td style="text-align: left;">URL</td>
<td style="text-align: left;"><p>Đối với các dịch vụ không có xác thực ảnh mặt</p>
<p>https://&lt;domain&gt;:&lt;port&gt;/api/tcdn<strong>/{service-code}/</strong>api/get-transaction</p>
<p><strong>{service-code}</strong> tham chiếu theo bảng Mã dịch vụ IV.3.</p>
<p>Đối với các dịch vụ có xác thực ảnh mặt</p>
<p>https://&lt;domain&gt;:&lt;port&gt;/api/tcdn<strong>/{service-code}/</strong>v2/api/get-transaction</p>
<p><strong>{service-code}</strong> tham chiếu theo bảng Mã dịch vụ IV.3.</p></td>
</tr>
<tr>
<td style="text-align: left;">Method</td>
<td style="text-align: left;">POST</td>
</tr>
<tr>
<td style="text-align: left;">Header</td>
<td style="text-align: left;"><ul>
<li><p><strong>Content-Type:</strong> application/json; charset=utf-8</p></li>
<li><p><strong>api_key:</strong> API key</p></li>
<li><p><mark><strong>target_code:</strong> Mã mục đích chia sẻ, xử lý thông tin do Hệ thống định danh cung cấp, phục vụ hiển thị đúng nội dung mục đích chia sẻ, xử lý thông tin trên ứng dụng VNeID.</mark></p></li>
<li><p><strong>request_id:</strong> Mã yêu cầu do hệ thống của TCDN tự sinh, là duy nhất trên hệ thống.</p></li>
<li><p><strong>request_time:</strong> Thời gian yêu cầu, định dạng ddMMyyyyHHmmss.</p></li>
<li><p><strong>request_type:</strong> Áp dụng đối với dịch vụ có xác thực sinh trắc học. Đối với trường hợp sử dụng SDK do TTXT cung cấp thì giá trị trường này là:</p></li>
</ul>
<ul>
<li><p>“sdk-liveness-match”: nếu sử dụng dịch vụ xác thực chống giả mạo khuôn mặt và dịch vụ xác thực ảnh mặt.</p></li>
<li><p>“sdk-match”: nếu sử dụng dịch vụ xác thực khuôn mặt</p></li>
</ul></td>
</tr>
<tr>
<td style="text-align: left;">Params</td>
<td style="text-align: left;"><p>Trên môi trường non-production:</p>
<p>code=mã đơn vị kết nối (do Đơn vị cung cấp dịch vụ cấp)</p>
<p>Trên môi trường prod: N/A</p></td>
</tr>
<tr>
<td style="text-align: left;">Request body</td>
<td style="text-align: left;"><p>Thông tin AgentGW gửi cho ứng dụng của TCDN qua HTTP với phương thức là POST, dữ liệu theo định dạng JSON.</p>
<p>Đối với dịch vụ chỉ chia sẻ thông tin công dân</p>
<p>{</p>
<p>    "transactionId": "a569782f-8768-42bb-b231-8b0c9cb4b2e4"</p>
<p>    // là mã ứng dụng doanh nghiệp nhận được khi khởi tạo giao dịch.</p>
<p>}</p>
<p>Đối với dịch vụ chia sẻ thông tin công dân kèm xác thực sinh trắc học</p>
<p>{</p>
<p>    "transactionId": "9baf1668-a0da-4f9a-b9ac-5eef6f5da5f6",</p>
<p>    "faceImage": "AAABsWU3NmU2MWQ2YTY0NjRlODMyNzRmNWUxZmVlZWJmNzMyBKUUzlviAoxdoHp9aYaUuwQ5oYZkt0mdPnQnZqgCSvtiY8oqOLrwLDijz3ytzZ/mNx/IgKYZl11EEX0qCMklHy/7d/DY/t1KGwF9epGfS9n14BGjzO/nWJ22m0rWzIuGMARgzxM7+Jd5RFf96DN0yOkUdpQqCGI1OYBTLQMJgfwql9pLISZKlod3Y8RuKBlFg7mFuW7pkLaAjthaUOkaY5H9MNwJBkY+tY8aISWNnOeY258HddtOeOiW/gv47Yd00lOfaoUmA3Je+EaP5EXimjHqqFYEt+y5vpBuwazCs1YVdFAc3W2lxSMJZtJSxUGe4+FE5ovt3hIp3/bjyrdLV4ca1CribOuotojN+A5h0DPV8voR7ng97dD7FU2fGzywdJMsoRoDDo1CKTDgbIlLv8C9B/wX/ykG+d6pr+ZF/MYvNuJA9lZC5tbbrc4lJtPVwaVB1Ymdyn0C7HoGK5Ip7tIAWtFwR+68w5753reNP2X1KsnTH/K6ZZn2phgw/……"</p>
<p>    //chuỗi dữ liệu ảnh do SDK trả ra</p>
<p>}</p></td>
</tr>
<tr>
<td style="text-align: left;">Response body</td>
<td style="text-align: left;"><p>Đối với các dịch vụ không có xác thực ảnh mặt</p>
<p>{</p>
<p>"description": "Success!",</p>
<p>"responseTime": "24032026135443",</p>
<p>"responseCode": "0",</p>
<p>"data": {</p>
<p>"biometric": 0,</p>
<p>"pid": null</p>
<p>},</p>
<p>"userInfo": "eyJhbGciOiJFUzI1NiIsInR5cCI6IkpPU0UvSldTIn0=.ewogICJjaXRpemVuUGlkIiA6ICIwMDEwOTkwMDgwNjAiLAogICJmdWxsTmFtZSIgOiAiTmd1eeG7hW4gTWluaCDEkOG7qWMiLAogICJiaXJ0aERhdGUiIDogIjExLTEyLTE5OTkiLAogICJuYXRpb25hbGl0eUNvZGUiIDogIlZOIiwKICAicGVybWFuZW50Q2l0eUNvZGUiIDogIjAxIiwKICAicGVybWFuZW50RGlzdHJpY3RDb2RlIiA6IG51bGwsCiAgInBlcm1hbmVudFZpbGxhZ2VDb2RlIiA6ICIwMDI0MSIsCiAgInBlcm1hbmVudEFkZHJlc3MiIDogIjEyIE5ndXnhu4VuIER1IiwKICAibGl2aW5nUGxhY2VDaXR5Q29kZSIgOiAiMDEiLAogICJsaXZpbmdQbGFjZURpc3RyaWN0Q29kZSIgOiBudWxsLAogICJsaXZpbmdQbGFjZVZpbGxhZ2VDb2RlIiA6ICIwMDI0MSIsCiAgImxpdmluZ1BsYWNlQWRkcmVzcyIgOiAiMTIgTmd1eeG7hW4gRHUiLAogICJkYXRlT2ZJc3N1ZSIgOiAiMDEtMDEtMjAyMCIsCiAgImlkQ2FyZEV4cGlyZURhdGUiIDogIktow7RuZyB0aOG7nWkgaOG6oW4iLAogICJpc3N1aW5nQXV0aG9yaXR5IiA6ICJD4bulYyBD4bqjbmggc8OhdCBxdeG6o24gbMO9IGjDoG5oIGNow61uaCB24buBIHRy4bqtdCB04buxIHjDoyBo4buZaSIsCiAgImxpdmluZ1BsYWNlQ2l0eVRleHQiIDogIlRow6BuaCBwaOG7kSBIw6AgTuG7mWkiLAogICJsaXZpbmdQbGFjZURpc3RyaWN0VGV4dCIgOiBudWxsLAogICJnZW5kZXJDb2RlIiA6IG51bGwsCiAgInBlcm1hbmVudENpdHlUZXh0IiA6ICJUaMOgbmggcGjhu5EgSMOgIE7hu5lpIiwKICAicGVybWFuZW50RGlzdHJpY3RUZXh0IiA6IG51bGwsCiAgInBlcm1hbmVudFZpbGxhZ2VUZXh0IiA6ICJQaMaw4budbmcgTmd1eeG7hW4gRHUiLAogICJsaXZpbmdQbGFjZVZpbGxhZ2VUZXh0IiA6ICJQaMaw4budbmcgTmd1eeG7hW4gRHUiCn0=.MEUCIQDNyQKCohM9skITRwaHsyRLltiUFfOln6MvDCQDTeLriAIgGHI0SB4vx71S2eFMBsoIBxXPMph7Cr-InfDyxGRAbfM="</p>
<p>}        //userInfo: định dạng <mark>base64 encode</mark> tuân theo chuẩn JWS có format: &lt;header&gt;.&lt;payload&gt;.&lt;signature&gt;</p>
<p>        // Tổ chức khai thác có thể sử dụng các thư viện tiêu chuẩn để verify chữ ký (với public key được C06 cung cấp) và parse dữ liệu payload để có được thông tin công dân.</p>
<p>    }</p>
<p>}</p>
<p><strong>// Ví dụ minh họa đối với userInfo sau khi parse theo chuẩn JWS</strong></p>
<p><strong>// Cấu trúc &lt;payload&gt; :</strong> Lưu ý các trường thông tin có thể thay đổi tuỳ vào nhu cầu của TCDN cũng như sự đồng ý chia sẻ của công dân trên ứng dụng VNeID.</p>
<p>{</p>
<p>    "fullName": "Nguyễn Thị Huế",</p>
<p>    "birthDate": "24-04-1988",</p>
<p>    "nationalityCode": "VN",</p>
<p>    "citizenPid": "03xxxxx1958",</p>
<p>    "indentifyNumber": "6xxxxx832",</p>
<p>    "dateOfIssue": "23-07-2024",</p>
<p>    "issuingAuthority": "Cục Cảnh sát quản lý hành chính về trật tự xã hội",</p>
<p>    "genderCode": "3",</p>
<p>    "livingPlaceAddress": "Khu Đô Thị Hai Bên Đường Lê Trọng Tấn",</p>
<p>    "livingPlaceVillageCode": "09877",</p>
<p>    "livingPlaceDistrictCode": <strong>null</strong>,</p>
<p>    "livingPlaceCityCode": "01",</p>
<p>    "permanentAddress": "Khu Đô Thị Hai Bên Đường Lê Trọng Tấn",</p>
<p>    "permanentVillageCode": "09877",</p>
<p>    "permanentDistrictCode": <strong>null</strong>,</p>
<p>    "permanentCityCode": "01",</p>
<p>    "idCardExpireDate": "30-12-2030"</p>
<p>}</p>
<p>Đối với các dịch vụ có xác thực ảnh mặt</p>
<p>{</p>
<p>    "responseCode": 0, //0: thành công, khác 0: lỗi</p>
<p>    "description": "Mô tả lỗi nếu có",</p>
<p>    "data": {</p>
<p>        "biometricResult": "ewogICJ0eXAiOiAiand0IiwKICAiYWxnIjogIlJTMjU2Igp9.ewogICJ0cmFuc0lkIiA6ICIzZGJjYjc0ZS0yMWVmLTRjNjItOTkzMy04YzlhOGEwMGE5ZTYiLAogICJzdGF0dXNDb2RlIiA6IDEsCiAgImRhdGEiIDogewogICAgInJlc3VsdCIgOiAxLAogICAgImxpdmVuZXNzIiA6IHsKICAgICAgInJlc3VsdCIgOiAxLAogICAgICAic2NvcmUiIDogMC45OTk5ODc5CiAgICB9LAogICAgImZhY2VNYXRjaCIgOiB7CiAgICAgICJyZXN1bHQiIDogMSwKICAgICAgInNjb3JlIiA6IDAuOTk5OTk5OTQsCiAgICAgICJjb25maWRlbmNlIiA6IDMKICAgIH0sCiAgICAiZGV2aWNlUmlzayIgOiB7CiAgICAgICJpc0hpZ2hSaXNrIiA6IGZhbHNlLAogICAgICAiZmxhZ3MiIDogWyBdCiAgICB9CiAgfQp9.gB4mdnRJHRBB17ForY5xlGOcKM3f0CKobEM7hUmwHQAkpItXHeKqA/pN3SNSm2tR0iCQ4rJ/zdLB1Ht2fH/0lgCM3sWD8+rNTGRzGhMQFE58RQeLcNF8+5svrRFw1WXRgnCcDKm3W5t/s+n/V57pp7NnO4B68CNjRD9rnSt59lr605h0I6Vc2VKDVNuNcSy2i1rOMs2L5JpqgemZJqw6/B+vpD2aETgNiXYSkYja8UELmwxBfBJXjzwcI/6JL65FqKf+dAyAgl1nkwcFsP1auwWtdPj3+eNSqMz8KyNDrTtRGwMDj8ZJT4RymW/FIAWGAkcPG79y6tWxLWijUsoy2A=="</p>
<p>        // biometricResult : Chuỗi <mark>base64 encode</mark> tuân theo chuẩn JWS có format: &lt;header&gt;.&lt;payload&gt;.&lt;signature&gt;</p>
<p>    },</p>
<p>    "userInfo": "eyJhbGciOiJFUzI1NiIsInR5cCI6IkpPU0UvSldTIn0=.ewogICJjaXRpemVuUGlkIiA6ICIwMzYxODgwMDE5NTgiLAogICJmdWxsTmFtZSIgOiAiTmd1eeG7hW4gVGjhu4sgSHXhur8iLAogICJsaXZpbmdQbGFjZUNpdHlUZXh0IiA6ICJUaMOgbmggcGjhu5EgSMOgIE7hu5lpIiwKICAibGl2aW5nUGxhY2VEaXN0cmljdFRleHQiIDogIkh1eeG7h24gSG_DoGkgxJDhu6ljIiwKICAicGVybWFuZW50Q2l0eVRleHQiIDogIlRow6BuaCBwaOG7kSBIw6AgTuG7mWkiLAogICJwZXJtYW5lbnREaXN0cmljdFRleHQiIDogIkh1eeG7h24gSG_DoGkgxJDhu6ljIiwKICAicGVybWFuZW50VmlsbGFnZVRleHQiIDogIljDoyBBbiBLaMOhbmgiLAogICJsaXZpbmdQbGFjZVZpbGxhZ2VUZXh0IiA6ICJYw6MgQW4gS2jDoW5oIiwKICAicGVybWFuZW50Q2l0eUNvZGUiIDogIjAxIiwKICAicGVybWFuZW50RGlzdHJpY3RDb2RlIiA6ICIyNzQiLAogICJwZXJtYW5lbnRWaWxsYWdlQ29kZSIgOiAiMDk4NzciLAogICJwZXJtYW5lbnRBZGRyZXNzIiA6ICJLaHUgxJDDtCBUaOG7iyBIYWkgQsOqbiDEkMaw4budbmcgTMOqIFRy4buNbmcgVOG6pW4sIEFuIEtow6FuaCwgSG_DoGkgxJDhu6ljLCBIw6AgTuG7mWkiLAogICJsaXZpbmdQbGFjZUNpdHlDb2RlIiA6ICIwMSIsCiAgImxpdmluZ1BsYWNlRGlzdHJpY3RDb2RlIiA6ICIyNzQiLAogICJsaXZpbmdQbGFjZVZpbGxhZ2VDb2RlIiA6ICIwOTg3NyIsCiAgImxpdmluZ1BsYWNlQWRkcmVzcyIgOiAiS2h1IMSQw7QgVGjhu4sgSGFpIELDqm4gxJDGsOG7nW5nIEzDqiBUcuG7jW5nIFThuqVuLCBBbiBLaMOhbmgsIEhvw6BpIMSQ4bupYywgSMOgIE7hu5lpIiwKICAiZGF0ZU9mSXNzdWUiIDogIjIzLTA3LTIwMjQiLAogICJpZENhcmRFeHBpcmVEYXRlIiA6ICIzMC0xMi0yMDMwIiwKICAiaXNzdWluZ0F1dGhvcml0eSIgOiAiQ-G7pWMgQ-G6o25oIHPDoXQgcXXhuqNuIGzDvSBow6BuaCBjaMOtbmggduG7gSB0cuG6rXQgdOG7sSB4w6MgaOG7mWkiLAogICJiaXJ0aERhdGUiIDogIjI0LTA0LTE5ODgiLAogICJuYXRpb25hbGl0eUNvZGUiIDogIlZOIgp9.MEUCIBNqg0LXH7UtJvU_A3vRZLLR20iacQxVK7lT1YZTVTJhAiEAjdPapssFIwhylfs68ufjT7RlByIKtrbjVIjw60CJWnc="</p>
<p>    //userInfo: định dạng <mark>base64URL encode</mark> tuân theo chuẩn JWS có format: &lt;header&gt;.&lt;payload&gt;.&lt;signature&gt;</p>
<p>    //Tổ chức khai thác có thể sử dụng các thư viện tiêu chuẩn để verify chữ ký (với public key được C06 cung cấp) và parse dữ liệu payload để có được thông tin công dân.</p>
<p>}</p>
<p>// Cấu trúc &lt;payload&gt; biometricResult sau khi parse theo chuẩn JWS</p>
<p>    {</p>
<p>        "transId": "22fc12e0-be90-4519-8ef6-cdeca95ae22b",</p>
<p>        "statusCode": 1, // Trạng thái gửi yêu cầu: 1-Thành công, Khác 1-Không thành công</p>
<p>        "data": {</p>
<p>            "result": 1, //0 không xác thực, 1 xác thực</p>
<p>            "liveness": {</p>
<p>                "result": 1, //1 xác thực, 0 không xác thực</p>
<p>                "score": 0.98 // Trọng số</p>
<p>            },</p>
<p>            //Object "liveness" chỉ có trong trường hợp request_type = sdk-liveness-match</p>
<p>            "faceMatch": {</p>
<p>                "result": 1, //1 xác thực, 0 không xác thực</p>
<p>                "score": 0.98, // Trọng số</p>
<p>                "confidence": 3 //độ tin cậy dựa vào score của face match 1: Thấp 2: Trung Bình 3: Cao</p>
<p>            },</p>
<p>            "deviceRisk": {</p>
<p>                "isHighRisk": <strong>true</strong>, // Cảnh báo thiết bị không an toàn: true: thiết bị không an toàn, false: thiết bị an toàn</p>
<p>                "flags": [</p>
<p>                    2,</p>
<p>                    3</p>
<p>                ] // Danh sách mã nguyên nhân cảnh báo thiết bị không an toàn</p>
<p>            }</p>
<p>        }</p>
<p>    }</p>
<p><strong>// Ví dụ minh họa đối với userInfo sau khi parse theo chuẩn JWS</strong></p>
<p>// Cấu trúc &lt;header&gt;</p>
<p>    {</p>
<p>        "alg": "ES256",</p>
<p>        "typ": "JOSE/JWS"</p>
<p>    }</p>
<p>// Cấu trúc &lt;payload&gt; : Lưu ý các trường thông tin có thể thay đổi tuỳ vào nhu cầu của TCDN cũng như sự đồng ý chia sẻ của công dân trên ứng dụng VNeID.</p>
<p>{</p>
<p>    "fullName": "Nguyễn Thị Huế",</p>
<p>    "birthDate": "24-04-1988",</p>
<p>    "nationalityCode": "VN",</p>
<p>    "citizenPid": "03xxxxxxx958",</p>
<p>    "indentifyNumber": "6xxxxxx32",</p>
<p>    "dateOfIssue": "23-07-2024",</p>
<p>    "issuingAuthority": "Cục Cảnh sát quản lý hành chính về trật tự xã hội",</p>
<p>    "genderCode": "3",</p>
<p>    "livingPlaceAddress": "Khu Đô Thị Hai Bên Đường Lê Trọng Tấn",</p>
<p>    "livingPlaceVillageCode": "09877",</p>
<p>    "livingPlaceDistrictCode": <strong>null</strong>,</p>
<p>    "livingPlaceCityCode": "01",</p>
<p>    "permanentAddress": "Khu Đô Thị Hai Bên Đường Lê Trọng Tấn",</p>
<p>    "permanentVillageCode": "09877",</p>
<p>    "permanentDistrictCode": <strong>null</strong>,</p>
<p>    "permanentCityCode": "01",</p>
<p>    "idCardExpireDate": "30-12-2030"</p>
<p>}</p>
<p>// Cấu trúc &lt;signature&gt;</p>
<p>        HMACSHA256(</p>
<p>        base64UrlEncode(header) + "." +</p>
<p>        base64UrlEncode(payload),</p>
<p>        secret)</p></td>
</tr>
</tbody>
</table>

## III.2. Dịch vụ xác nhận chia sẻ thông tin công dân(Có thể kèm dịch vụ xác thực sinh trắc học), xác thực sinh trắc học, xác thực thông tin công dân {#iii-2-dich-vu-xac-nhan-chia-se-thong-tin-cong-dan}

### III.2.1. API gửi yêu cầu chia sẻ thông tin công dân(Có thể kèm dịch vụ xác thực sinh trắc học), xác thực sinh trắc học, xác thực thông tin công dân {#iii-2-1-api-gui-yeu-cau-chia-se-thong-tin-cong-dan}

\* **Tổ chức doanh nghiệp cần cung cấp** đường link dẫn tới trang công bố **“Điều khoản và cam kết sử dụng dữ liệu được chia sẻ”** của Tổ chức doanh nghiệp, nhằm hiển thị trên ứng dụng VNeID cho công dân.

#### API Specs

<table>
<colgroup>
<col style="width: 16%" />
<col style="width: 83%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>Thông tin</strong></th>
<th style="text-align: center;"><strong>Mô tả</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td>Mục đích</td>
<td>API sử dụng để AgentGW chuyển tiếp yêu cầu xác nhận chia sẻ thông tin công dân và xác thực sinh trắc học từ TCDN đến NT.</td>
</tr>
<tr>
<td>URL</td>
<td><p>https://&lt;domain&gt;:&lt;port&gt;/api/tcdn/<strong>{service-code}/</strong>api/agent/init</p>
<p><strong>{service-code}</strong> tham chiếu theo bảng Mã dịch vụ IV.3.</p>
<p><em>Ví dụ: <strong>share-info</strong> là mã dịch vụ của dịch vụ chia sẻ thông tin công dân.</em></p></td>
</tr>
<tr>
<td>Method</td>
<td>POST</td>
</tr>
<tr>
<td>Header</td>
<td><ul>
<li><p><strong>Content-Type:</strong> application/json; charset=utf-8</p></li>
<li><p><strong>api_key:</strong> <mark>API key do Tổ chức cung cấp dịch vụ cung cấp</mark></p></li>
<li><p><mark><strong>target_code:</strong> Mã mục đích chia sẻ, xử lý thông tin do Hệ thống định danh cung cấp, phục vụ hiển thị đúng nội dung mục đích chia sẻ, xử lý thông tin trên ứng dụng VNeID.</mark></p></li>
<li><p><strong>request_id:</strong> Mã yêu cầu do hệ thống của TCDN tự sinh, là duy nhất trên hệ thống.</p></li>
<li><p><strong>request_time:</strong> Thời gian yêu cầu, định dạng ddMMyyyyHHmmss. Là thời gian hiện tại của hệ thống, trong trường hợp request_time gửi sang Hệ thống nền tảng lệch quá 2 phút so với thời gian hiện tại của Hệ thống nền tảng thì request đó được coi là timeout.</p></li>
</ul></td>
</tr>
<tr>
<td style="text-align: left;">Params</td>
<td style="text-align: left;"><p>Trên môi trường non-production:</p>
<p>code=mã đơn vị kết nối (do Đơn vị cung cấp dịch vụ cấp)</p>
<p>Trên môi trường prod: N/A</p></td>
</tr>
<tr>
<td>Request body</td>
<td style="text-align: left;"><p>Dịch vụ chia sẻ thông tin công dân, xác thực sinh trắc học</p>
<p>{</p>
<p>    "fullName": "Nguyễn Thị Huế",</p>
<p>    "birthDate": "24-04-1988",</p>
<p>    "citizenPid": "03xxxxxxx58"</p>
<p>}</p>
<p>Dịch vụ xác thực thông tin công dân</p>
<p>{</p>
<p>    "fullName": "Nguyễn Thị Huế",</p>
<p>    "birthDate": "24-04-1988",</p>
<p>    "nationalityCode": "01",</p>
<p>    "citizenPid": "036xxxxxxx58",</p>
<p>    "permanentVillageCode": "09877",</p>
<p>    "permanentDistrictCode": null,</p>
<p>    "permanentCityCode": "01"</p>
<p>//Danh sách các trường cần xác thực.</p>
<p>}</p></td>
</tr>
<tr>
<td>Respond body</td>
<td style="text-align: left;"><p>Thành công</p>
<p>{</p>
<p>"description": "Success!",</p>
<p>"data": {</p>
<p>"txnId": "e9ac4603-05dd-4dd0-8090-325887fa2f45",</p>
<p>"requestId": "25a00741-9d9f-4f99-9a75-892ecd6a964d"</p>
<p>},</p>
<p>"responseTime": "24032026135411", //ddMMyyyyHHmmss</p>
<p>"responseCode": "0" //0: thành công, Khác 0: không thành công</p>
<p>}</p>
<p>Không thành công</p>
<p>{</p>
<p>    "responseCode": "-7”, //Mã responseCode tham khảo bảng mã lỗi IV.4</p>
<p>    "description": "Service not found.",</p>
<p>    "responseTime": "30092024190321", //ddMMyyyyHHmmss</p>
<p>    "data": null</p>
<p>}</p></td>
</tr>
</tbody>
</table>

#### Request Body

**\* Các thông tin công dân** có thể chia sẻ thông qua các dịch vụ chia sẻ thông tin, xác thực thông tin công dân bao gồm:

- Họ và tên

- Ngày sinh

- Số CCCD/ Số định danh

- Số CMND

- Ngày cấp CCCD/ Số định danh

- Nơi cấp CCCD/ Số định danh

- Quốc tịch

- Nơi thường trú

- Nơi ở hiện tại

- Ngày hết hạn CCCD

- Giới tính

  **\* Đối với dịch vụ chia sẻ thông tin, xác thực sinh trắc học:** Bắt buộc truyền trường Số CCCD/ Số định danh (citizenPid), trường Họ và tên và Ngày sinh không bắt buộc.

  **\* Đối với dịch vụ xác thực thông tin công dân:** Trừ trường số CCCD/ Số định danh (citizenPid) là trường bắt buộc trong request body, còn lại các trường khác nếu TCDN truyền lên sẽ nhận về kết quả Đúng/Sai (True/False) so với dữ liệu trong CSDL dân cư, trường nào không truyền lên sẽ nhận về dữ liệu công dân đồng ý chia sẻ.

<table>
<colgroup>
<col style="width: 6%" />
<col style="width: 17%" />
<col style="width: 26%" />
<col style="width: 14%" />
<col style="width: 35%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>TT</strong></th>
<th style="text-align: center;"><strong>Tên trường</strong></th>
<th style="text-align: center;"><strong>Định dạng</strong></th>
<th style="text-align: center;"><strong>Bắt buộc</strong></th>
<th style="text-align: center;"><strong>Mô tả</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: left;">1</td>
<td style="text-align: left;">citizenPid</td>
<td style="text-align: left;">String (12 ký tự số)</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Số định danh của công dân</td>
</tr>
<tr>
<td style="text-align: left;">2</td>
<td style="text-align: left;">fullName</td>
<td style="text-align: left;">string (100 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;"><p>Họ và tên đầy đủ của công dân</p>
<p><em>(Tùy vào yêu cầu của TCDN, trường thông tin họ và tên có thể chuyển thành các định dạng:</em></p>
<p><em>- fullNameUpper: Viết hoa, đầy đủ, có dấu, có cách.</em></p>
<p><em>- fullNameSrch: Viết in hoa, không dấu, có cách)</em></p></td>
</tr>
<tr>
<td style="text-align: left;">3</td>
<td style="text-align: left;">birthDate</td>
<td style="text-align: left;">string (20 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Ngày tháng năm sinh của công dân, định dạng dd-MM-yyyy</td>
</tr>
<tr>
<td style="text-align: left;">4</td>
<td style="text-align: left;">Các trường thông tin cần xác thực khác</td>
<td style="text-align: left;"></td>
<td style="text-align: center;"></td>
<td style="text-align: left;"></td>
</tr>
</tbody>
</table>

#### Response Body

<table>
<colgroup>
<col style="width: 6%" />
<col style="width: 18%" />
<col style="width: 19%" />
<col style="width: 11%" />
<col style="width: 44%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>TT</strong></th>
<th style="text-align: center;"><strong>Tên trường</strong></th>
<th style="text-align: center;"><strong>Định dạng</strong></th>
<th style="text-align: center;"><strong>Bắt buộc</strong></th>
<th style="text-align: center;"><strong>Mô tả</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: center;">1</td>
<td>responseCode</td>
<td><p>String</p>
<p>(2 ký tự)</p></td>
<td style="text-align: center;">x</td>
<td style="text-align: left;"><p>Mã trạng thái tiếp nhận:</p>
<p>0: thành công</p>
<p>Khác 0: Lỗi (mô tả chi tiết tại trường description)</p>
<p><em>Tham chiếu mục IV.4, cụ thể:</em></p>
<p>-2: Giao dịch đã tồn tại</p>
<p>-5: Đầu vào yêu cầu không hợp lệ!</p>
<p>-6: Không tìm thấy tổ chức</p>
<p>-7: Không tìm thấy dịch vụ</p>
<p>-8: Đường dẫn không hợp lệ</p>
<p>-12: Không tìm thấy cấu hình agent</p>
<p>-14: Timeout</p>
<p>-16: Dịch vụ đang sử dụng vượt quá hạn mức cho phép</p>
<p>-18: Trường đầu vào không tồn tại trong cấu hình dịch vụ</p>
<p>-19: Tài khoản tổ chức không có quyền sử dụng dịch vụ</p>
<p>-20: Thông tin công dân không đúng</p></td>
</tr>
<tr>
<td style="text-align: center;">2</td>
<td>description</td>
<td><p>String</p>
<p>(100 ký tự)</p></td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Mô tả lỗi trường hợp trạng thái tiếp nhận khác 0</td>
</tr>
<tr>
<td style="text-align: center;">3</td>
<td>responseTime</td>
<td><p>String</p>
<p>(100 ký tự)</p></td>
<td style="text-align: center;">x</td>
<td style="text-align: left;"><p>Thời gian phản hồi kết quả</p>
<p>Định dạng: ddMMyyyyHHmmss</p></td>
</tr>
<tr>
<td style="text-align: center;">4</td>
<td>data</td>
<td>JsonObject</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Kết quả trả về dưới dạng Json Object</td>
</tr>
<tr>
<td style="text-align: center;"></td>
<td colspan="4">Mô tả cấu trúc JsonObject <strong>data</strong></td>
</tr>
<tr>
<td style="text-align: center;"><em>4.1</em></td>
<td><em>txnId</em></td>
<td><em>String</em></td>
<td style="text-align: center;"></td>
<td style="text-align: left;"><em>Mã giao dịch được trả về khi khởi tạo yêu cầu ban đầu thành công</em></td>
</tr>
<tr>
<td style="text-align: center;"><em>4.2</em></td>
<td><em>requestId</em></td>
<td></td>
<td style="text-align: center;"></td>
<td style="text-align: left;"><em>Tham chiếu mã yêu cầu do hệ thống của TCDN gửi sang Hệ thống nền tảng</em></td>
</tr>
<tr>
<td colspan="5" style="text-align: center;">Ứng dụng của TCDN sau khi nhận được mã giao dịch (txnId), tiến hành gọi Web/App VNeID theo URL được cung cấp dưới đây để chuyển hướng người dùng sang VNeID thực hiện Consent</td>
</tr>
<tr>
<td style="text-align: center;"></td>
<td>URL WEB</td>
<td colspan="3" style="text-align: left;">Dịch vụ này chỉ có trên APP VNeID không triển khai trên WEB VNeID</td>
</tr>
<tr>
<td style="text-align: center;"></td>
<td>URL APP</td>
<td colspan="3" style="text-align: left;"><p><em>Chi tiết tham chiếu mục III.6.4 Tài liệu này.</em></p>
<p>- Môi trường tích hợp: https://universal.dancuquocgia.com/share/[$transactionCode]</p>
<p>- Môi trường PRO: N/A</p></td>
</tr>
</tbody>
</table>

### III.2.2. API thông báo kết quả xác nhận chia sẻ/ không chia sẻ của công dân và các mã lỗi khác cho ứng dụng của TCDN. {#iii-2-2-api-thong-bao-ket-qua-xac-nhan-cong-dan}

#### API Specs

<table>
<colgroup>
<col style="width: 15%" />
<col style="width: 84%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>Thông tin</strong></th>
<th style="text-align: center;"><strong>Mô tả</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: left;">Mục đích</td>
<td style="text-align: left;"><p>Thông báo kết quả xác nhận chia sẻ/ không chia sẻ của công dân và các mã lỗi khác cho ứng dụng của TCDN</p>
<p><mark>API này sẽ thực hiện kiểm tra tính xác thực thông tin tài khoản VNeID của công dân ngay khi TCDN khởi giao dịch từ API III.2.1</mark></p>
<p><mark><strong>Nếu thông tin công dân không hợp lệ, hết hạn, bị khoá, mức 1, đã hết hạn CCCD, không tồn tại, …..</strong> VNeID sẽ thực hiện gọi API này thông báo cho TCDN dừng luồng nghiệp vụ không switch App to App hoặc Web to App sang VNeID <em>(Thời gian trả lại thông tin API này chỉ trong vòng &lt; 1s khi TCDN khởi tạo giao dịch API III.2.1)</em></mark></p>
<p><mark><strong>Nếu thông tin công dân đã hợp lệ:</strong><br />
- Nếu TCDN có nhu cầu tiếp nhận thông tin VNeID đã hiển thị thành công màn đồng ý/không đồng ý trên ứng dụng VNeID (data request: <em>"requestStatus": "INIT").</em> VNeID sẽ thực hiện cấu hình trả thêm phần data này để TCDN nhận được và sau đó mới thực hiện Switch App to App hoặc Web to App sang ứng dụng VNeID.</mark></p>
<p><mark>- Nếu TCDN không cần tiếp nhận data trên và thực hiện Switch App to App hoặc Web to App sang ứng dụng VNeID, Công dân sẽ thực hiện đồng ý/không đồng ý trên ứng dụng VNeID. VNeID sẽ trả thông báo kết quả đồng ý/không đồng ý qua API này đến Backend TCDN.</mark></p></td>
</tr>
<tr>
<td style="text-align: left;">URL</td>
<td style="text-align: left;">TCDN định nghĩa và cấu hình sẵn trong AgentGW</td>
</tr>
<tr>
<td style="text-align: left;">Method</td>
<td style="text-align: left;">POST</td>
</tr>
<tr>
<td style="text-align: left;">Header</td>
<td style="text-align: left;"><ul>
<li><p><strong>Content-Type:</strong> application/json; charset=utf-8</p></li>
<li><p><strong>api_key:</strong> API key //Do TCDN cung cấp – Chuỗi mã trung bình 32-64 ký tự.</p></li>
<li><p><strong>request_id:</strong> Mã yêu cầu do hệ thống Nền tảng tự sinh, là duy nhất trên hệ thống.</p></li>
<li><p><strong>request_time:</strong> Thời gian yêu cầu, định dạng ddMMyyyyHHmmss.</p></li>
</ul></td>
</tr>
<tr>
<td style="text-align: left;">Request body</td>
<td style="text-align: left;"><p><mark>Callback Khi Ứng dụng VneID hiển thị màn Consent thành công</mark></p>
<p>{</p>
<p>"transactionCode": "02d73def-fe15-418c-b97c-37c8214e3ba6",</p>
<p>"requestStatus": "INIT"</p>
<p>}'</p>
<p><mark>Callback khi Công dân đồng ý chia sẻ dữ liệu</mark></p>
<p>{</p>
<p>    "requestId": "7353b035-90e7-436c-93df-faa9e1817d74",</p>
<p>    "resultCode": "1",</p>
<p>    "resultDesc": "Success!",</p>
<p>    "txnId": "63d57f38-99dc-4c53-b05c-57dd24ec386d"</p>
<p>}</p>
<p><mark>Callback khi Công dân từ chối chia sẻ dữ liệu</mark><br />
{</p>
<p>    "requestId": "27cc4003-587f-4798-a975-42d7297dc6f0",</p>
<p>    "resultCode": "2",</p>
<p>    "resultDesc": "Success!",</p>
<p>    "txnId": "beaa6974-2bd8-41b4-9843-88b251b42991"</p>
<p>}</p>
<p><mark>Callback khi Thông tin công dân không hợp lệ, Mức 1, …</mark></p>
<p>{</p>
<p>    "requestId": "d052c2b651964557b242e885efbf82a2",</p>
<p>    "resultCode": "-17",</p>
<p>    "resultDesc": "Thông tin công dân không hợp lệ, vui lòng kiểm tra chi tiết trên ứng dụng VNeID",</p>
<p>    "txnId": "22c3cf62-b6bd-4b04-b8ba-e29a5d03ac42"</p>
<p>}</p></td>
</tr>
<tr>
<td style="text-align: left;">Response body</td>
<td style="text-align: left;"><p>{</p>
<p>    "responseCode": 1, //0: thành công, 1: không thành công</p>
<p>    "description": "Mô tả lỗi",</p>
<p>    "responseTime": "30092024190321" //ddMMyyyyHHmmss</p>
<p>}</p></td>
</tr>
</tbody>
</table>

#### Request Body

<table>
<colgroup>
<col style="width: 7%" />
<col style="width: 15%" />
<col style="width: 12%" />
<col style="width: 9%" />
<col style="width: 54%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>STT</strong></th>
<th style="text-align: center;"><strong>Tên trường</strong></th>
<th style="text-align: center;"><strong>Kiểu dữ liệu</strong></th>
<th style="text-align: center;"><strong>Bắt buộc</strong></th>
<th style="text-align: center;"><strong>Mô tả</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: center;">1</td>
<td>requestId</td>
<td>String</td>
<td style="text-align: center;">x</td>
<td>Tham chiếu đến request_id trong API gửi yêu cầu khởi tạo ban đầu của TCDN</td>
</tr>
<tr>
<td style="text-align: center;">2</td>
<td>resultCode</td>
<td>String</td>
<td style="text-align: center;">x</td>
<td><p>Mã kết quả xử lý yêu cầu chia sẻ thông tin:</p>
<p><em>Tham chiếu mục IV.4 – Mã lỗi responseCode, resultCode</em></p>
<p>1: Đồng ý chia sẻ</p>
<p>2: Không đồng ý chia sẻ</p>
<p>INIT: Hiển thị màn Consent thành công</p></td>
</tr>
<tr>
<td style="text-align: center;">3</td>
<td>resultDesc</td>
<td>String</td>
<td style="text-align: center;"></td>
<td><p>Mô tả chi tiết mã kết quả resultCode:</p>
<p>Tham chiếu mục IV.4 – Mã lỗi responseCode, resultCode</p></td>
</tr>
<tr>
<td style="text-align: center;">4</td>
<td>txnId</td>
<td>String</td>
<td style="text-align: center;">x</td>
<td>Mã giao dịch để sử dụng truy vấn thông tin công dân (trường hợp công dân xác nhận đồng ý)</td>
</tr>
<tr>
<td style="text-align: center;">5</td>
<td>data</td>
<td>Object</td>
<td style="text-align: center;"></td>
<td><p>Trường dữ liệu trả thêm tùy vào nhu cầu của TCDN và ứng dụng VNeID để trả dữ liệu (nếu cần)</p>
<p><em>(Trường hợp không có dữ liệu trả thêm thì sẽ không có trường này)</em></p></td>
</tr>
</tbody>
</table>

#### Response body

9.  Ứng dụng của tổ chức doanh nghiệp trả về trạng thái với **httpstatus = 200 (OK)** cùng bản tin theo định dạng JSON.

<table>
<colgroup>
<col style="width: 7%" />
<col style="width: 17%" />
<col style="width: 16%" />
<col style="width: 10%" />
<col style="width: 46%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>STT</strong></th>
<th style="text-align: center;"><strong>Tên trường</strong></th>
<th style="text-align: center;"><strong>Kiểu dữ liệu</strong></th>
<th style="text-align: center;"><strong>Bắt buộc</strong></th>
<th style="text-align: center;"><strong>Ghi chú</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: center;">1</td>
<td>responseCode</td>
<td><p>String</p>
<p>(2 ký tự)</p></td>
<td style="text-align: center;">x</td>
<td style="text-align: left;"><p>Mã trạng thái tiếp nhận:</p>
<p>0: thành công</p>
<p>Khác 0: Lỗi (mô tả chi tiết tại trường description)</p></td>
</tr>
<tr>
<td style="text-align: center;">2</td>
<td>description</td>
<td><p>String</p>
<p>(100 ký tự)</p></td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Mô tả lỗi trong trường hợp trạng thái tiếp nhận lỗi (responseCode khác 0)</td>
</tr>
<tr>
<td style="text-align: center;">3</td>
<td>responseTime</td>
<td><p>String</p>
<p>(100 ký tự)</p></td>
<td style="text-align: center;">x</td>
<td style="text-align: left;"><p>Thời gian phản hồi kết quả</p>
<p>Định dạng: ddMMyyyyHHmmss</p></td>
</tr>
</tbody>
</table>

### III.2.3. API yêu cầu trả kết quả dữ liệu công dân đồng ý chia sẻ, kết quả xác thực thông tin công dân <span class="mark">(áp dụng với các dịch vụ không bao gồm xác thực ảnh mặt)</span> {#iii-2-3-api-yeu-cau-tra-ket-qua-khong-bao-gom-anh-mat}

**Lưu ý:**

\- 1 Ngày tối đa TCDN sẽ được gọi API yêu cầu trả kết quả xác thực sinh trắc học và dữ liệu công dân đồng ý chia sẻ, kết quả xác thực thông tin công dân mặc định 10.000 lần với mỗi 1 target code – 1 service code(Nếu TCDN có nhu cầu hoặc estimate số lượng user sử dụng dịch vụ lớn hơn dự kiến sẽ cần đăng ký trước với Tổ chức cung cấp dịch vụ).

\- TransactionId sẽ hết hạn sau 30 phút kể từ thời điểm ghi nhận thành công việc công dân đã thực hiện xác nhận trên VNeID.

\- TCDN chỉ có thể gọi lấy dữ liệu tối đa 03 lần trong khoảng thời gian 30 phút, quá thời gian trên sẽ không nhận được dữ liệu công dân đã chia sẻ.

#### API Specs

<table>
<colgroup>
<col style="width: 18%" />
<col style="width: 81%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>Thông tin</strong></th>
<th style="text-align: center;"><strong>Mô tả</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: left;">Tên API</td>
<td style="text-align: left;">API cung cấp cho TCDN khai thác yêu cầu trả kết quả dữ liệu công dân đồng ý chia sẻ (áp dụng với các dịch vụ không bao gồm xác thực ảnh mặt)</td>
</tr>
<tr>
<td style="text-align: left;">Mục đích</td>
<td style="text-align: left;">Cho phép TCDN gửi yêu cầu trả kết quả dữ liệu công dân đồng ý chia sẻ</td>
</tr>
<tr>
<td style="text-align: left;">URL</td>
<td style="text-align: left;"><p>https://&lt;domain&gt;:&lt;port&gt;/api/tcdn<strong>/{service-code}</strong>/api/get-transaction</p>
<p><strong>{service-code}</strong> tham chiếu theo bảng Mã dịch vụ IV.3.</p></td>
</tr>
<tr>
<td style="text-align: left;">Method</td>
<td style="text-align: left;">POST</td>
</tr>
<tr>
<td style="text-align: left;">Header</td>
<td style="text-align: left;"><ul>
<li><p><strong>Content-Type:</strong> application/json; charset=utf-8</p></li>
<li><p><strong>api_key:</strong> API key Tổ chức cung cấp dịch vụ cung cấp</p></li>
<li><p><mark><strong>target_code:</strong> Mã mục đích chia sẻ, xử lý thông tin do Tổ chức cung cấp dịch vụ cung cấp, phục vụ hiển thị đúng nội dung mục đích chia sẻ, xử lý thông tin trên ứng dụng VNeID.</mark></p></li>
<li><p><strong>request_id:</strong> Mã yêu cầu do hệ thống của TCDN tự sinh, là duy nhất trên hệ thống.</p></li>
<li><p><strong>request_time:</strong> Thời gian yêu cầu, định dạng ddMMyyyyHHmmss. Là thời gian hiện tại của hệ thống, trong trường hợp request_time gửi sang Hệ thống VNeID lệch quá 2 phút so với thời gian hiện tại của Hệ thống VNeID thì request đó được coi là timeout.</p></li>
</ul></td>
</tr>
<tr>
<td style="text-align: left;">Params</td>
<td style="text-align: left;"><p>Trên môi trường non-production:</p>
<p>code=mã đơn vị kết nối (do Đơn vị cung cấp dịch vụ cấp)</p>
<p>Trên môi trường prod: N/A</p></td>
</tr>
<tr>
<td style="text-align: left;">Request body</td>
<td style="text-align: left;"><p><strong>Dịch vụ xác thực thông tin công dân</strong></p>
<p>{</p>
<p>    "transactionId":"087f0479-98e8-4f37-a312-a7e24399e26a"</p>
<p>}</p></td>
</tr>
<tr>
<td style="text-align: left;">Response body</td>
<td style="text-align: left;"><p><strong>Dịch vụ chia sẻ thông tin công dân</strong></p>
<p>{</p>
<p>"description": "Success!",</p>
<p>"responseTime": "24032026135443",</p>
<p>"responseCode": "0",</p>
<p>"data": {</p>
<p>"biometric": 0,</p>
<p>"pid": null</p>
<p>},</p>
<p>"userInfo": "eyJhbGciOiJFUzI1NiIsInR5cCI6IkpPU0UvSldTIn0=.ewogICJjaXRpemVuUGlkIiA6ICIwMDEwOTkwMDgwNjAiLAogICJmdWxsTmFtZSIgOiAiTmd1eeG7hW4gTWluaCDEkOG7qWMiLAogICJiaXJ0aERhdGUiIDogIjExLTEyLTE5OTkiLAogICJuYXRpb25hbGl0eUNvZGUiIDogIlZOIiwKICAicGVybWFuZW50Q2l0eUNvZGUiIDogIjAxIiwKICAicGVybWFuZW50RGlzdHJpY3RDb2RlIiA6IG51bGwsCiAgInBlcm1hbmVudFZpbGxhZ2VDb2RlIiA6ICIwMDI0MSIsCiAgInBlcm1hbmVudEFkZHJlc3MiIDogIjEyIE5ndXnhu4VuIER1IiwKICAibGl2aW5nUGxhY2VDaXR5Q29kZSIgOiAiMDEiLAogICJsaXZpbmdQbGFjZURpc3RyaWN0Q29kZSIgOiBudWxsLAogICJsaXZpbmdQbGFjZVZpbGxhZ2VDb2RlIiA6ICIwMDI0MSIsCiAgImxpdmluZ1BsYWNlQWRkcmVzcyIgOiAiMTIgTmd1eeG7hW4gRHUiLAogICJkYXRlT2ZJc3N1ZSIgOiAiMDEtMDEtMjAyMCIsCiAgImlkQ2FyZEV4cGlyZURhdGUiIDogIktow7RuZyB0aOG7nWkgaOG6oW4iLAogICJpc3N1aW5nQXV0aG9yaXR5IiA6ICJD4bulYyBD4bqjbmggc8OhdCBxdeG6o24gbMO9IGjDoG5oIGNow61uaCB24buBIHRy4bqtdCB04buxIHjDoyBo4buZaSIsCiAgImxpdmluZ1BsYWNlQ2l0eVRleHQiIDogIlRow6BuaCBwaOG7kSBIw6AgTuG7mWkiLAogICJsaXZpbmdQbGFjZURpc3RyaWN0VGV4dCIgOiBudWxsLAogICJnZW5kZXJDb2RlIiA6IG51bGwsCiAgInBlcm1hbmVudENpdHlUZXh0IiA6ICJUaMOgbmggcGjhu5EgSMOgIE7hu5lpIiwKICAicGVybWFuZW50RGlzdHJpY3RUZXh0IiA6IG51bGwsCiAgInBlcm1hbmVudFZpbGxhZ2VUZXh0IiA6ICJQaMaw4budbmcgTmd1eeG7hW4gRHUiLAogICJsaXZpbmdQbGFjZVZpbGxhZ2VUZXh0IiA6ICJQaMaw4budbmcgTmd1eeG7hW4gRHUiCn0=.MEUCIQDNyQKCohM9skITRwaHsyRLltiUFfOln6MvDCQDTeLriAIgGHI0SB4vx71S2eFMBsoIBxXPMph7Cr-InfDyxGRAbfM="</p>
<p>}        //userInfo: định dạng <mark>base64 encode</mark> tuân theo chuẩn JWS có format: &lt;header&gt;.&lt;payload&gt;.&lt;signature&gt;</p>
<p>        // Tổ chức khai thác có thể sử dụng các thư viện tiêu chuẩn để verify chữ ký (với public key được C06 cung cấp) và parse dữ liệu payload để có được thông tin công dân.</p>
<p>    }</p>
<p>}</p>
<p><strong>// Ví dụ minh họa đối với userInfo sau khi parse theo chuẩn JWS</strong></p>
<p><strong>// Cấu trúc &lt;payload&gt; :</strong> Lưu ý các trường thông tin có thể thay đổi tuỳ vào nhu cầu của TCDN cũng như sự đồng ý chia sẻ của công dân trên ứng dụng VNeID.</p>
<p>{</p>
<p>    "fullName": "Nguyễn Thị Huế",</p>
<p>    "birthDate": "24-04-1988",</p>
<p>    "nationalityCode": "VN",</p>
<p>    "citizenPid": "03xxxxx1958",</p>
<p>    "indentifyNumber": "6xxxxx832",</p>
<p>    "dateOfIssue": "23-07-2024",</p>
<p>    "issuingAuthority": "Cục Cảnh sát quản lý hành chính về trật tự xã hội",</p>
<p>    "genderCode": "3",</p>
<p>    "livingPlaceAddress": "Khu Đô Thị Hai Bên Đường Lê Trọng Tấn",</p>
<p>    "livingPlaceVillageCode": "09877",</p>
<p>    "livingPlaceDistrictCode": <strong>null</strong>,</p>
<p>    "livingPlaceCityCode": "01",</p>
<p>    "permanentAddress": "Khu Đô Thị Hai Bên Đường Lê Trọng Tấn",</p>
<p>    "permanentVillageCode": "09877",</p>
<p>    "permanentDistrictCode": <strong>null</strong>,</p>
<p>    "permanentCityCode": "01",</p>
<p>    "idCardExpireDate": "30-12-2030"</p>
<p>}</p>
<p><strong>Dịch vụ xác thực thông tin công dân</strong></p>
<p>{</p>
<p>    "description": "Success!",</p>
<p>    "responseTime": "01102024100947",</p>
<p>    "responseCode": "0",</p>
<p>    "data": {</p>
<p>        "biometric": 0,</p>
<p>        "pid": <strong>null</strong></p>
<p>    },</p>
<p>    "userInfo": "eyJraWQiOiItMzI0NDIiLCJhbGciOiJTSEEyNTYiLCJjdHkiOiJFSUQiLCJ0eXAiOiJKT1NFL0pXUyJ9.ewogICJmdWxsTmFtZSIgOiB0cnVlLAogICJiaXJ0aERhdGUiIDogdHJ1ZSwKICAibmF0aW9uYWxpdHlDb2RlIiA6IGZhbHNlLAogICJjaXRpemVuUGlkIiA6IHRydWUsCiAgImRhdGVPZklzc3VlIiA6ICIyMy0wNy0yMDI0IiwKICAiaXNzdWluZ0F1dGhvcml0eSIgOiAiQ+G7pWMgQ+G6o25oIHPDoXQgcXXhuqNuIGzDvSBow6BuaCBjaMOtbmggduG7gSB0cuG6rXQgdOG7sSB4w6MgaOG7mWkiLAogICJsaXZpbmdQbGFjZUFkZHJlc3MiIDogIktodSDEkMO0IFRo4buLIEhhaSBCw6puIMSQxrDhu51uZyBMw6ogVHLhu41uZyBU4bqlbiwgQW4gS2jDoW5oLCBIb8OgaSDEkOG7qWMsIEjDoCBO4buZaSIsCiAgImxpdmluZ1BsYWNlVmlsbGFnZUNvZGUiIDogIjA5ODc3IiwKICAibGl2aW5nUGxhY2VDaXR5Q29kZSIgOiAiMDEiLAogICJsaXZpbmdQbGFjZURpc3RyaWN0Q29kZSIgOiAiMjc0IiwKICAicGVybWFuZW50QWRkcmVzcyIgOiAiS2h1IMSQw7QgVGjhu4sgSGFpIELDqm4gxJDGsOG7nW5nIEzDqiBUcuG7jW5nIFThuqVuLCBBbiBLaMOhbmgsIEhvw6BpIMSQ4bupYywgSMOgIE7hu5lpIiwKICAicGVybWFuZW50VmlsbGFnZUNvZGUiIDogdHJ1ZSwKICAicGVybWFuZW50RGlzdHJpY3RDb2RlIiA6IHRydWUsCiAgInBlcm1hbmVudENpdHlDb2RlIiA6IHRydWUKfQ==.MEUCIEvoA6SolfQteGSYfk7WwsrjfYfF3HT5fyNKVi6X+fK0AiEArzvwrywy0CCYolESLFnHec6dYM4P1L4tg6nBa6K29ks="</p>
<p>}</p>
<p>// Tương tự như trên, userInfo: định dạng <mark>base64 encode</mark> tuân theo chuẩn JWS có format: &lt;header&gt;.&lt;payload&gt;.&lt;signature&gt;. Cấu trúc &lt;payload&gt; userInfo sau khi parse theo chuẩn JWS như sau:</p>
<p>// Lưu ý: các trường thông tin có thể thay đổi tuỳ vào các trường trong yêu cầu xác thực của TCDN cũng như sự đồng ý chia sẻ của công dân trên ứng dụng VNeID.</p>
<p>- Đối với các trường đầu vào có dữ liệu trong request body API tại mục III.4.1 kết quả sẽ trả về <strong>true</strong>/ false tương ứng với dữ liệu gửi lên <strong>trùng khớp</strong>/ không trùng khớp với thông tin của công dân trong CSDL.</p>
<p>- Đối với các trường đầu vào cấu hình nhưng không có dữ liệu trong request body API tại mục III.4.1 kết quả sẽ trả về thông tin tương ứng của công dân nếu công dân đồng ý chia sẻ.</p>
<p>{</p>
<p>    "fullName": <strong>true</strong>,</p>
<p>    "birthDate": <strong>true</strong>,</p>
<p>    "nationalityCode": <strong>false</strong>,</p>
<p>    "citizenPid": <strong>true</strong>,</p>
<p>    "dateOfIssue": "23-07-2024",</p>
<p>    "issuingAuthority": "Cục Cảnh sát quản lý hành chính về trật tự xã hội",</p>
<p>    "livingPlaceAddress": "Khu Đô Thị Hai Bên Đường Lê Trọng Tấn",</p>
<p>    "livingPlaceVillageCode": "09877",</p>
<p>    "livingPlaceDistrictCode": null,</p>
<p>    "livingPlaceCityCode": "01",</p>
<p>    "permanentAddress": "Khu Đô Thị Hai Bên Đường Lê Trọng Tấn",</p>
<p>    "permanentVillageCode": <strong>true</strong>,</p>
<p>    "permanentDistrictCode": null,</p>
<p>    "permanentCityCode": <strong>true</strong></p>
<p>}</p></td>
</tr>
</tbody>
</table>

#### Request body

| **STT** | **Tên trường** | **Kiểu dữ liệu** | **Bắt buộc** | **Mô tả**                                                                           |
| :-----: | :------------: | :--------------: | :----------: | ----------------------------------------------------------------------------------- |
|    1    | transactionId  |      String      |      x       | Mã giao dịch đã được trả về khi khởi tạo yêu cầu ban đầu thành công từ API III.2.1. |

#### Response body

10. Hệ thống nền tảng định danh trả về trạng thái với **httpstatus = 200 (OK)** cùng bản tin theo định dạng JSON.

<table>
<colgroup>
<col style="width: 11%" />
<col style="width: 27%" />
<col style="width: 14%" />
<col style="width: 8%" />
<col style="width: 38%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>STT</strong></th>
<th style="text-align: center;"><strong>Tên trường</strong></th>
<th style="text-align: center;"><strong>Kiểu dữ liệu</strong></th>
<th style="text-align: center;"><strong>Bắt buộc</strong></th>
<th style="text-align: center;"><strong>Ghi chú</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: center;">1</td>
<td>responseCode</td>
<td><p>String</p>
<p>(2 ký tự)</p></td>
<td style="text-align: center;">x</td>
<td style="text-align: left;"><p>Mã trạng thái tiếp nhận:</p>
<p>0: thành công</p>
<p>Khác 0: Lỗi (mô tả chi tiết tại trường description)</p>
<p><em>Tham chiếu mục IV.4, cụ thể:</em></p>
<p><em>-1: Không lấy được thông tin</em></p>
<p><em>-4: Giao dịch đang xử lý, vui lòng thử lại sau</em></p>
<p><em>-5: Đầu vào yêu cầu không hợp lệ!</em></p>
<p><em>-6: Không tìm thấy tổ chức</em></p>
<p><em>-7: Không tìm thấy dịch vụ</em></p>
<p><em>-8: Đường dẫn không hợp lệ</em></p>
<p><em>-9: Không tìm thấy mã giao dịch</em></p>
<p><em>-11: Công dân không đồng ý chia sẻ thông tin</em></p>
<p><em>-12: Không tìm thấy cấu hình agent</em></p>
<p><em>-14: Timeout</em></p>
<p><em>-15: Tồn tại trường dữ liệu bắt buộc bị “null”</em></p>
<p><em>-16: Dịch vụ đang sử dụng vượt quá hạn mức cho phép</em></p>
<p><em>-17: Thông tin công dân không hợp lệ, vui lòng kiểm tra chi tiết trên ứng dụng VNeID</em></p>
<p><em>-19: Tài khoản tổ chức không có quyền sử dụng dịch vụ</em></p>
<p><em>-20: Thông tin công dân không đúng</em></p></td>
</tr>
<tr>
<td style="text-align: center;">2</td>
<td>description</td>
<td><p>String</p>
<p>(100 ký tự)</p></td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Mô tả lỗi trường hợp trạng thái tiếp nhận khác 0</td>
</tr>
<tr>
<td style="text-align: center;">3</td>
<td>responseTime</td>
<td><p>String</p>
<p>(100 ký tự)</p></td>
<td style="text-align: center;">x</td>
<td style="text-align: left;"><p>Thời gian phản hồi kết quả</p>
<p>Định dạng: ddMMyyyyHHmmss</p></td>
</tr>
<tr>
<td style="text-align: center;">4</td>
<td>data</td>
<td>JsonObject</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;"><p>Kết quả xác thực sinh trắc học.</p>
<p>Tuân theo chuẩn JWS. Là chuỗi Base64 có format:</p>
<p>&lt;header&gt;.&lt;payload&gt;.&lt;signature&gt;</p></td>
</tr>
<tr>
<td style="text-align: center;"></td>
<td colspan="4" style="text-align: left;">Mô tả cấu trúc JsonObject <strong>data</strong></td>
</tr>
<tr>
<td style="text-align: center;">4.1</td>
<td>biometric</td>
<td>number</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;"><p>Trạng thái công dân đồng ý xác thực sinh trắc học</p>
<p>0 - Không đồng ý</p>
<p>1 - Đồng ý</p></td>
</tr>
<tr>
<td style="text-align: center;">4.2</td>
<td>Pid</td>
<td>String</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">null</td>
</tr>
<tr>
<td style="text-align: center;">5</td>
<td>userInfo</td>
<td>string</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;"><p>Tuân theo chuẩn JWS. Là chuỗi Base64 có format:</p>
<p>&lt;header&gt;.&lt;payload&gt;.&lt;signature&gt;</p></td>
</tr>
<tr>
<td style="text-align: center;"></td>
<td colspan="4"><p><strong>Mô tả cấu trúc trường thông tin userInfo sau khi parse theo chuẩn JWS</strong></p>
<p><em><strong>Lưu ý:</strong> các trường thông tin có thể thay đổi tuỳ vào nhu cầu của TCDN cũng như sự đồng ý chia sẻ của công dân trên ứng dụng VNeID.</em></p></td>
</tr>
<tr>
<td style="text-align: right;">1</td>
<td>fullName</td>
<td>string (100 ký tự)</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Họ và tên đầy đủ của công dân</td>
</tr>
<tr>
<td style="text-align: right;">2</td>
<td>birthDate</td>
<td>string (20 ký tự)</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Ngày tháng năm sinh của công dân, định dạng dd-MM-yyyy</td>
</tr>
<tr>
<td style="text-align: right;">3</td>
<td>citizenPid</td>
<td>string (12 ký tự số)</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Số định danh/ CCCD của công dân</td>
</tr>
<tr>
<td style="text-align: right;">4</td>
<td>dateOfIssue</td>
<td>string (20 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Ngày cấp số định danh/ CCCD của công dân</td>
</tr>
<tr>
<td style="text-align: right;">5</td>
<td>issuingAuthority</td>
<td>string (100 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Nơi cấp định danh/ CCCD của công dân</td>
</tr>
<tr>
<td style="text-align: right;">6</td>
<td>nationalityCode</td>
<td>string (2 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Mã quốc tịch của công dân</td>
</tr>
<tr>
<td style="text-align: right;">7.1</td>
<td>permanentAddress</td>
<td>string (200 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Địa chỉ thường trú (chi tiết) của công dân</td>
</tr>
<tr>
<td style="text-align: right;">7.2</td>
<td>permanentVillageCode</td>
<td>string (20 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Mã xã thường trú (Ref danh mục xã)</td>
</tr>
<tr>
<td style="text-align: right;">7.3</td>
<td>permanentVillageText</td>
<td>string (100 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Tên xã thường trú</td>
</tr>
<tr>
<td style="text-align: right;">7.4</td>
<td>permanentDistrictCode</td>
<td>string (20 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Mã huyện thường trú (Ref danh mục huyện)</td>
</tr>
<tr>
<td style="text-align: right;">7.5</td>
<td>permanentDistrictText</td>
<td>string (100 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Tên huyện thường trú</td>
</tr>
<tr>
<td style="text-align: right;">7.6</td>
<td>permanentCityCode</td>
<td>string (20 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Mã tỉnh thường trú (Ref danh mục tỉnh)</td>
</tr>
<tr>
<td style="text-align: right;">7.7</td>
<td>permanentCityText</td>
<td>string (100 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Tên tỉnh thường trú</td>
</tr>
<tr>
<td style="text-align: right;">8</td>
<td style="text-align: left;">livingPlaceAddress</td>
<td>string (200 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Địa chỉ nơi ở hiện tại (chi tiết) của công dân</td>
</tr>
<tr>
<td style="text-align: right;">8.1</td>
<td>livingPlaceVillageCode</td>
<td>string (20 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Mã xã nơi ở hiện tại (Ref danh mục xã)</td>
</tr>
<tr>
<td style="text-align: right;">8.2</td>
<td>livingPlaceVillageText</td>
<td>string (100 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Tên xã nơi ở hiện tại</td>
</tr>
<tr>
<td style="text-align: right;">8.3</td>
<td>livingPlaceDistrictCode</td>
<td>string (20 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Mã huyện nơi ở hiện tại (Ref danh mục huyện)</td>
</tr>
<tr>
<td style="text-align: right;">8.4</td>
<td>livingPlaceDistrictText</td>
<td>string (100 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Tên huyện nơi ở hiện tại</td>
</tr>
<tr>
<td style="text-align: right;">8.5</td>
<td>livingPlaceCityCode</td>
<td>string (20 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Mã tỉnh nơi ở hiện tại (Ref danh mục tỉnh)</td>
</tr>
<tr>
<td style="text-align: right;">8.6</td>
<td>livingPlaceCityText</td>
<td>string (100 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Tên tỉnh nơi ở hiện tại</td>
</tr>
<tr>
<td style="text-align: right;">9</td>
<td>idCardExpireDate</td>
<td>String</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Ngày hết hạn của CCCD (mới nhất đến thời điểm chia sẻ)</td>
</tr>
<tr>
<td style="text-align: right;">10</td>
<td>genderCode</td>
<td>String (10 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;"><p>Mã giới tính của công dân</p>
<p><em>Tham chiếu mục IV.5.1 Mã danh mục giới tính</em></p></td>
</tr>
<tr>
<td style="text-align: right;">11</td>
<td>identifyNumber</td>
<td>string (20 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Số CMND của công dân</td>
</tr>
</tbody>
</table>

### III.2.4. API yêu cầu trả kết quả xác thực sinh trắc học và dữ liệu công dân đồng ý chia sẻ, kết quả xác thực thông tin công dân <span class="mark">(áp dụng với các dịch vụ bao gồm xác thực ảnh mặt)</span> {#iii-2-4-api-yeu-cau-tra-ket-qua-bao-gom-anh-mat}

**Lưu ý:**

\- 1 Ngày tối đa TCDN sẽ được gọi API yêu cầu trả kết quả xác thực sinh trắc học và dữ liệu công dân đồng ý chia sẻ, kết quả xác thực thông tin công dân mặc định 10.000 lần với mỗi 1 target code – 1 service code(Nếu TCDN có nhu cầu hoặc estimate số lượng user sử dụng dịch vụ lớn hơn dự kiến sẽ cần đăng ký trước với Tổ chức cung cấp dịch vụ).

\- TransactionId sẽ hết hạn sau 30 phút kể từ thời điểm ghi nhận thành công việc công dân đã thực hiện xác nhận trên VNeID.

\- TCDN chỉ có thể gọi lấy dữ liệu tối đa 03 lần trong khoảng thời gian 30 phút, quá thời gian trên sẽ không nhận được dữ liệu công dân đã chia sẻ.

#### API Specs

<table>
<colgroup>
<col style="width: 18%" />
<col style="width: 81%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>Thông tin</strong></th>
<th style="text-align: center;"><strong>Mô tả</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: left;">Tên API</td>
<td style="text-align: left;">API cung cấp cho TCDN khai thác yêu cầu trả kết quả xác thực sinh trắc học và dữ liệu công dân đồng ý chia sẻ (áp dụng với các dịch vụ không bao xác thực ảnh mặt)</td>
</tr>
<tr>
<td style="text-align: left;">Mục đích</td>
<td style="text-align: left;">Cho phép TCDN gửi yêu cầu trả kết quả xác thực sinh trắc học và dữ liệu công dân đồng ý chia sẻ</td>
</tr>
<tr>
<td style="text-align: left;">URL</td>
<td style="text-align: left;"><p>https://&lt;domain&gt;:&lt;port&gt;/api/tcdn<strong>/{service-code}</strong><mark>/v2/api/get-transaction</mark></p>
<p><strong>{service-code}</strong> tham chiếu theo bảng Mã dịch vụ IV.3.</p></td>
</tr>
<tr>
<td style="text-align: left;">Method</td>
<td style="text-align: left;">POST</td>
</tr>
<tr>
<td style="text-align: left;">Header</td>
<td style="text-align: left;"><ul>
<li><p><strong>Content-Type:</strong> application/json; charset=utf-8</p></li>
<li><p><strong>api_key:</strong> API key Tổ chức cung cấp dịch vụ cung cấp</p></li>
<li><p><mark><strong>target_code:</strong> Mã mục đích chia sẻ, xử lý thông tin do Tổ chức cung cấp dịch vụ cung cấp, phục vụ hiển thị đúng nội dung mục đích chia sẻ, xử lý thông tin trên ứng dụng VNeID.</mark></p></li>
<li><p><strong>request_id:</strong> Mã yêu cầu do hệ thống của TCDN tự sinh, là duy nhất trên hệ thống.</p></li>
<li><p><strong>request_time:</strong> Thời gian yêu cầu, định dạng ddMMyyyyHHmmss. Là thời gian hiện tại của hệ thống, trong trường hợp request_time gửi sang Hệ thống VNeID lệch quá 2 phút so với thời gian hiện tại của Hệ thống VNeID thì request đó được coi là timeout.</p></li>
<li><p><strong>request_type:</strong> Đối với trường hợp sử dụng SDK do TTXT cung cấp thì giá trị trường này là:</p></li>
</ul>
<ul>
<li><p>“sdk-liveness-match”: nếu sử dụng dịch vụ xác thực chống giả mạo khuôn mặt và dịch vụ xác thực ảnh mặt.</p></li>
<li><p>“sdk-match”: nếu sử dụng dịch vụ xác thực khuôn mặt</p></li>
</ul></td>
</tr>
<tr>
<td style="text-align: left;">Params</td>
<td style="text-align: left;"><p>Trên môi trường non-production:</p>
<p>code=mã đơn vị kết nối (do Đơn vị cung cấp dịch vụ cấp)</p>
<p>Trên môi trường prod: N/A</p></td>
</tr>
<tr>
<td style="text-align: left;">Request body</td>
<td style="text-align: left;"><p><strong>Dịch vụ chia sẻ thông tin công dân, xác thực sinh trắc học</strong></p>
<p>{</p>
<p>    "transactionId":"9baf1668-a0da-4f9a-b9ac-5eef6f5da5f6",</p>
<p>    "faceImage": "AAABsWU3NmU2MWQ2YTY0NjRlODMyNzRmNWUxZmVlZWJmNzMyBKUUzlviAoxdoHp9aYaUuwQ5oYZkt0mdPnQnZqgCSvtiY8oqOLrwLDijz3ytzZ/mNx/IgKYZl11EEX0qCMklHy/7d/DY/t1KGwF9epGfS9n14BGjzO/nWJ22m0rWzIuGMARgzxM7+Jd5RFf96DN0yOkUdpQqCGI1OYBTLQMJgfwql9pLISZKlod3Y8RuKBlFg7mFuW7pkLaAjthaUOkaY5H9MNwJBkY+tY8aISWNnOeY258HddtOeOiW/gv47Yd00lOfaoUmA3Je+EaP5EXimjHqqFYEt+y5vpBuwazCs1YVdFAc3W2lxSMJZtJSxUGe4+FE5ovt3hIp3/bjyrdLV4ca1CribOuotojN+A5h0DPV8voR7ng97dD7FU2fGzywdJMsoRoDDo1CKTDgbIlLv8C9B/wX/ykG+d6pr+ZF/MYvNuJA9lZC5tbbrc4lJtPVwaVB1Ymdyn0C7HoGK5Ip7tIAWtFwR+68w5753reNP2X1KsnTH/K6ZZn2phgw/hse1Qogh……" //chuỗi dữ liệu ảnh do SDK trả ra</p>
<p>}</p>
<p><strong>Dịch vụ xác thực thông tin công dân</strong></p>
<p>{</p>
<p>    "transactionId":"087f0479-98e8-4f37-a312-a7e24399e26a",</p>
<p>"faceImage": "AAABsWU3NmU2MWQ2YTY0NjRlODMyNzRmNWUxZmVlZWJmNzMyBKUUzlviAoxdoHp9aYaUuwQ5oYZkt0mdPnQnZqgCSvtiY8oqOLrwLDijz3ytzZ/mNx/IgKYZl11EEX0qCMklHy/7d/DY/t1KGwF9epGfS9n14BGjzO/nWJ22m0rWzIuGMARgzxM7+Jd5RFf96DN0yOkUdpQqCGI1OYBTLQMJgfwql9pLISZKlod3Y8RuKBlFg7mFuW7pkLaAjthaUOkaY5H9MNwJBkY+tY8aISWNnOeY258HddtOeOiW/gv47Yd00lOfaoUmA3Je+EaP5EXimjHqqFYEt+y5vpBuwazCs1YVdFAc3W2lxSMJZtJSxUGe4+FE5ovt3hIp3/bjyrdLV4ca1CribOuotojN+A5h0DPV8voR7ng97dD7FU2fGzywdJMsoRoDDo1CKTDgbIlLv8C9B/wX/ykG+d6pr+ZF/MYvNuJA9lZC5tbbrc4lJtPVwaVB1Ymdyn0C7HoGK5Ip7tIAWtFwR+68w5753reNP2X1KsnTH/K6ZZn2phgw/hse1Qogh……" //chuỗi dữ liệu ảnh do SDK trả ra</p>
<p>}</p></td>
</tr>
<tr>
<td style="text-align: left;">Response body</td>
<td style="text-align: left;"><p><strong>Dịch vụ chia sẻ thông tin công dân, xác thực sinh trắc học</strong></p>
<p>{</p>
<p>    "responseCode": 0, //0: thành công, khác 0: lỗi</p>
<p>    "description": "Mô tả lỗi nếu có",</p>
<p>    "data": {</p>
<p>        "biometricResult": "ewogICJ0eXAiOiAiand0IiwKICAiYWxnIjogIlJTMjU2Igp9.ewogICJ0cmFuc0lkIiA6ICIzZGJjYjc0ZS0yMWVmLTRjNjItOTkzMy04YzlhOGEwMGE5ZTYiLAogICJzdGF0dXNDb2RlIiA6IDEsCiAgImRhdGEiIDogewogICAgInJlc3VsdCIgOiAxLAogICAgImxpdmVuZXNzIiA6IHsKICAgICAgInJlc3VsdCIgOiAxLAogICAgICAic2NvcmUiIDogMC45OTk5ODc5CiAgICB9LAogICAgImZhY2VNYXRjaCIgOiB7CiAgICAgICJyZXN1bHQiIDogMSwKICAgICAgInNjb3JlIiA6IDAuOTk5OTk5OTQsCiAgICAgICJjb25maWRlbmNlIiA6IDMKICAgIH0sCiAgICAiZGV2aWNlUmlzayIgOiB7CiAgICAgICJpc0hpZ2hSaXNrIiA6IGZhbHNlLAogICAgICAiZmxhZ3MiIDogWyBdCiAgICB9CiAgfQp9.gB4mdnRJHRBB17ForY5xlGOcKM3f0CKobEM7hUmwHQAkpItXHeKqA/pN3SNSm2tR0iCQ4rJ/zdLB1Ht2fH/0lgCM3sWD8+rNTGRzGhMQFE58RQeLcNF8+5svrRFw1WXRgnCcDKm3W5t/s+n/V57pp7NnO4B68CNjRD9rnSt59lr605h0I6Vc2VKDVNuNcSy2i1rOMs2L5JpqgemZJqw6/B+vpD2aETgNiXYSkYja8UELmwxBfBJXjzwcI/6JL65FqKf+dAyAgl1nkwcFsP1auwWtdPj3+eNSqMz8KyNDrTtRGwMDj8ZJT4RymW/FIAWGAkcPG79y6tWxLWijUsoy2A=="</p>
<p>        // biometricResult : Chuỗi <mark>base64 encode</mark> tuân theo chuẩn JWS có format: &lt;header&gt;.&lt;payload&gt;.&lt;signature&gt;</p>
<p>    },</p>
<p>    "userInfo": "eyJhbGciOiJFUzI1NiIsInR5cCI6IkpPU0UvSldTIn0=.ewogICJjaXRpemVuUGlkIiA6ICIwMzYxODgwMDE5NTgiLAogICJmdWxsTmFtZSIgOiAiTmd1eeG7hW4gVGjhu4sgSHXhur8iLAogICJsaXZpbmdQbGFjZUNpdHlUZXh0IiA6ICJUaMOgbmggcGjhu5EgSMOgIE7hu5lpIiwKICAibGl2aW5nUGxhY2VEaXN0cmljdFRleHQiIDogIkh1eeG7h24gSG_DoGkgxJDhu6ljIiwKICAicGVybWFuZW50Q2l0eVRleHQiIDogIlRow6BuaCBwaOG7kSBIw6AgTuG7mWkiLAogICJwZXJtYW5lbnREaXN0cmljdFRleHQiIDogIkh1eeG7h24gSG_DoGkgxJDhu6ljIiwKICAicGVybWFuZW50VmlsbGFnZVRleHQiIDogIljDoyBBbiBLaMOhbmgiLAogICJsaXZpbmdQbGFjZVZpbGxhZ2VUZXh0IiA6ICJYw6MgQW4gS2jDoW5oIiwKICAicGVybWFuZW50Q2l0eUNvZGUiIDogIjAxIiwKICAicGVybWFuZW50RGlzdHJpY3RDb2RlIiA6ICIyNzQiLAogICJwZXJtYW5lbnRWaWxsYWdlQ29kZSIgOiAiMDk4NzciLAogICJwZXJtYW5lbnRBZGRyZXNzIiA6ICJLaHUgxJDDtCBUaOG7iyBIYWkgQsOqbiDEkMaw4budbmcgTMOqIFRy4buNbmcgVOG6pW4sIEFuIEtow6FuaCwgSG_DoGkgxJDhu6ljLCBIw6AgTuG7mWkiLAogICJsaXZpbmdQbGFjZUNpdHlDb2RlIiA6ICIwMSIsCiAgImxpdmluZ1BsYWNlRGlzdHJpY3RDb2RlIiA6ICIyNzQiLAogICJsaXZpbmdQbGFjZVZpbGxhZ2VDb2RlIiA6ICIwOTg3NyIsCiAgImxpdmluZ1BsYWNlQWRkcmVzcyIgOiAiS2h1IMSQw7QgVGjhu4sgSGFpIELDqm4gxJDGsOG7nW5nIEzDqiBUcuG7jW5nIFThuqVuLCBBbiBLaMOhbmgsIEhvw6BpIMSQ4bupYywgSMOgIE7hu5lpIiwKICAiZGF0ZU9mSXNzdWUiIDogIjIzLTA3LTIwMjQiLAogICJpZENhcmRFeHBpcmVEYXRlIiA6ICIzMC0xMi0yMDMwIiwKICAiaXNzdWluZ0F1dGhvcml0eSIgOiAiQ-G7pWMgQ-G6o25oIHPDoXQgcXXhuqNuIGzDvSBow6BuaCBjaMOtbmggduG7gSB0cuG6rXQgdOG7sSB4w6MgaOG7mWkiLAogICJiaXJ0aERhdGUiIDogIjI0LTA0LTE5ODgiLAogICJuYXRpb25hbGl0eUNvZGUiIDogIlZOIgp9.MEUCIBNqg0LXH7UtJvU_A3vRZLLR20iacQxVK7lT1YZTVTJhAiEAjdPapssFIwhylfs68ufjT7RlByIKtrbjVIjw60CJWnc="</p>
<p>    //userInfo: định dạng <mark>base64URL encode</mark> tuân theo chuẩn JWS có format: &lt;header&gt;.&lt;payload&gt;.&lt;signature&gt;</p>
<p>    //Tổ chức khai thác có thể sử dụng các thư viện tiêu chuẩn để verify chữ ký (với public key được C06 cung cấp) và parse dữ liệu payload để có được thông tin công dân.</p>
<p>}</p>
<p>// Cấu trúc &lt;payload&gt; biometricResult sau khi parse theo chuẩn JWS</p>
<p>    {</p>
<p>        "transId": "22fc12e0-be90-4519-8ef6-cdeca95ae22b",</p>
<p>        "statusCode": 1, // Trạng thái gửi yêu cầu: 1-Thành công, Khác 1-Không thành công</p>
<p>        "data": {</p>
<p>            "result": 1, //0 không xác thực, 1 xác thực</p>
<p>            "liveness": {</p>
<p>                "result": 1, //1 xác thực, 0 không xác thực</p>
<p>                "score": 0.98 // Trọng số</p>
<p>            },</p>
<p>            //Object "liveness" chỉ có trong trường hợp request_type = sdk-liveness-match</p>
<p>            "faceMatch": {</p>
<p>                "result": 1, //1 xác thực, 0 không xác thực</p>
<p>                "score": 0.98, // Trọng số</p>
<p>                "confidence": 3 //độ tin cậy dựa vào score của face match 1: Thấp 2: Trung Bình 3: Cao</p>
<p>            },</p>
<p>            "deviceRisk": {</p>
<p>                "isHighRisk": <strong>true</strong>, // Cảnh báo thiết bị không an toàn: true: thiết bị không an toàn, false: thiết bị an toàn</p>
<p>                "flags": [</p>
<p>                    2,</p>
<p>                    3</p>
<p>                ] // Danh sách mã nguyên nhân cảnh báo thiết bị không an toàn</p>
<p>            }</p>
<p>        }</p>
<p>    }</p>
<p><strong>// Ví dụ minh họa đối với userInfo sau khi parse theo chuẩn JWS</strong></p>
<p>// Cấu trúc &lt;header&gt;</p>
<p>    {</p>
<p>        "alg": "ES256",</p>
<p>        "typ": "JOSE/JWS"</p>
<p>    }</p>
<p>// Cấu trúc &lt;payload&gt; : Lưu ý các trường thông tin có thể thay đổi tuỳ vào nhu cầu của TCDN cũng như sự đồng ý chia sẻ của công dân trên ứng dụng VNeID.</p>
<p>{</p>
<p>    "fullName": "Nguyễn Thị Huế",</p>
<p>    "birthDate": "24-04-1988",</p>
<p>    "nationalityCode": "VN",</p>
<p>    "citizenPid": "036188001958",</p>
<p>    "indentifyNumber": "654721832",</p>
<p>    "dateOfIssue": "23-07-2024",</p>
<p>    "issuingAuthority": "Cục Cảnh sát quản lý hành chính về trật tự xã hội",</p>
<p>    "genderCode": "3",</p>
<p>    "livingPlaceAddress": "Khu Đô Thị Hai Bên Đường Lê Trọng Tấn",</p>
<p>    "livingPlaceVillageCode": "09877",</p>
<p>    "livingPlaceDistrictCode": <strong>null</strong>,</p>
<p>    "livingPlaceCityCode": "01",</p>
<p>    "permanentAddress": "Khu Đô Thị Hai Bên Đường Lê Trọng Tấn",</p>
<p>    "permanentVillageCode": "09877",</p>
<p>    "permanentDistrictCode": <strong>null</strong>,</p>
<p>    "permanentCityCode": "01",</p>
<p>    "idCardExpireDate": "30-12-2030"</p>
<p>}</p>
<p>// Cấu trúc &lt;signature&gt;</p>
<p>        HMACSHA256(</p>
<p>        base64UrlEncode(header) + "." +</p>
<p>        base64UrlEncode(payload),</p>
<p>        secret)</p>
<p><strong>Dịch vụ xác thực thông tin công dân</strong></p>
<p>{</p>
<p>    "description": "Success!",</p>
<p>    "responseTime": "01102024100947",</p>
<p>    "responseCode": "0",</p>
<p>    "data": {</p>
<p>        "biometric": 0,</p>
<p>        "pid": <strong>null</strong></p>
<p>    },</p>
<p>    "userInfo": "eyJraWQiOiItMzI0NDIiLCJhbGciOiJTSEEyNTYiLCJjdHkiOiJFSUQiLCJ0eXAiOiJKT1NFL0pXUyJ9.ewogICJmdWxsTmFtZSIgOiB0cnVlLAogICJiaXJ0aERhdGUiIDogdHJ1ZSwKICAibmF0aW9uYWxpdHlDb2RlIiA6IGZhbHNlLAogICJjaXRpemVuUGlkIiA6IHRydWUsCiAgImRhdGVPZklzc3VlIiA6ICIyMy0wNy0yMDI0IiwKICAiaXNzdWluZ0F1dGhvcml0eSIgOiAiQ+G7pWMgQ+G6o25oIHPDoXQgcXXhuqNuIGzDvSBow6BuaCBjaMOtbmggduG7gSB0cuG6rXQgdOG7sSB4w6MgaOG7mWkiLAogICJsaXZpbmdQbGFjZUFkZHJlc3MiIDogIktodSDEkMO0IFRo4buLIEhhaSBCw6puIMSQxrDhu51uZyBMw6ogVHLhu41uZyBU4bqlbiwgQW4gS2jDoW5oLCBIb8OgaSDEkOG7qWMsIEjDoCBO4buZaSIsCiAgImxpdmluZ1BsYWNlVmlsbGFnZUNvZGUiIDogIjA5ODc3IiwKICAibGl2aW5nUGxhY2VDaXR5Q29kZSIgOiAiMDEiLAogICJsaXZpbmdQbGFjZURpc3RyaWN0Q29kZSIgOiAiMjc0IiwKICAicGVybWFuZW50QWRkcmVzcyIgOiAiS2h1IMSQw7QgVGjhu4sgSGFpIELDqm4gxJDGsOG7nW5nIEzDqiBUcuG7jW5nIFThuqVuLCBBbiBLaMOhbmgsIEhvw6BpIMSQ4bupYywgSMOgIE7hu5lpIiwKICAicGVybWFuZW50VmlsbGFnZUNvZGUiIDogdHJ1ZSwKICAicGVybWFuZW50RGlzdHJpY3RDb2RlIiA6IHRydWUsCiAgInBlcm1hbmVudENpdHlDb2RlIiA6IHRydWUKfQ==.MEUCIEvoA6SolfQteGSYfk7WwsrjfYfF3HT5fyNKVi6X+fK0AiEArzvwrywy0CCYolESLFnHec6dYM4P1L4tg6nBa6K29ks="</p>
<p>}</p>
<p>// Tương tự như trên, userInfo: định dạng base64URL encode tuân theo chuẩn JWS có format: &lt;header&gt;.&lt;payload&gt;.&lt;signature&gt;. Cấu trúc &lt;payload&gt; userInfo sau khi parse theo chuẩn JWS như sau:</p>
<p>// Lưu ý: các trường thông tin có thể thay đổi tuỳ vào các trường trong yêu cầu xác thực của TCDN cũng như sự đồng ý chia sẻ của công dân trên ứng dụng VNeID.</p>
<p>- Đối với các trường đầu vào có dữ liệu trong request body API tại mục III.4.1 kết quả sẽ trả về <strong>true</strong>/ false tương ứng với dữ liệu gửi lên <strong>trùng khớp</strong>/ không trùng khớp với thông tin của công dân trong CSDL.</p>
<p>- Đối với các trường đầu vào cấu hình nhưng không có dữ liệu trong request body API tại mục III.4.1 kết quả sẽ trả về thông tin tương ứng của công dân nếu công dân đồng ý chia sẻ.</p>
<p>{</p>
<p>    "fullName": <strong>true</strong>,</p>
<p>    "birthDate": <strong>true</strong>,</p>
<p>    "nationalityCode": <strong>false</strong>,</p>
<p>    "citizenPid": <strong>true</strong>,</p>
<p>    "dateOfIssue": "23-07-2024",</p>
<p>    "issuingAuthority": "Cục Cảnh sát quản lý hành chính về trật tự xã hội",</p>
<p>    "livingPlaceAddress": "Khu Đô Thị Hai Bên Đường Lê Trọng Tấn",</p>
<p>    "livingPlaceVillageCode": "09877",</p>
<p>    "livingPlaceDistrictCode": null,</p>
<p>    "livingPlaceCityCode": "01",</p>
<p>    "permanentAddress": "Khu Đô Thị Hai Bên Đường Lê Trọng Tấn",</p>
<p>    "permanentVillageCode": <strong>true</strong>,</p>
<p>    "permanentDistrictCode": null,</p>
<p>    "permanentCityCode": <strong>true</strong></p>
<p>}</p></td>
</tr>
</tbody>
</table>

#### Request body

<table>
<colgroup>
<col style="width: 7%" />
<col style="width: 16%" />
<col style="width: 14%" />
<col style="width: 10%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>STT</strong></th>
<th style="text-align: center;"><strong>Tên trường</strong></th>
<th style="text-align: center;"><strong>Kiểu dữ liệu</strong></th>
<th style="text-align: center;"><strong>Bắt buộc</strong></th>
<th style="text-align: center;"><strong>Mô tả</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: center;">1</td>
<td style="text-align: center;">transactionId</td>
<td style="text-align: center;">String</td>
<td style="text-align: center;">x</td>
<td>Mã giao dịch đã được trả về khi khởi tạo yêu cầu ban đầu thành công.</td>
</tr>
<tr>
<td style="text-align: center;">2</td>
<td style="text-align: center;">faceImage</td>
<td style="text-align: center;">Base64</td>
<td style="text-align: center;"></td>
<td><p>Dữ liệu khuôn mặt (Bắt buộc nếu sử dụng các dịch vụ:</p>
<p>-biometric-share-info</p>
<p>-face-matching</p></td>
</tr>
</tbody>
</table>

#### Response body

11. Hệ thống nền tảng định danh trả về trạng thái với **httpstatus = 200 (OK)** cùng bản tin theo định dạng JSON.

<table>
<colgroup>
<col style="width: 11%" />
<col style="width: 27%" />
<col style="width: 14%" />
<col style="width: 8%" />
<col style="width: 38%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>STT</strong></th>
<th style="text-align: center;"><strong>Tên trường</strong></th>
<th style="text-align: center;"><strong>Kiểu dữ liệu</strong></th>
<th style="text-align: center;"><strong>Bắt buộc</strong></th>
<th style="text-align: center;"><strong>Ghi chú</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: center;">1</td>
<td>responseCode</td>
<td><p>String</p>
<p>(2 ký tự)</p></td>
<td style="text-align: center;">x</td>
<td style="text-align: left;"><p>Mã trạng thái tiếp nhận:</p>
<p>0: thành công</p>
<p>Khác 0: Lỗi (mô tả chi tiết tại trường description)</p>
<p><em>Tham chiếu mục IV.4, cụ thể:</em></p>
<p><em>-1: Không lấy được thông tin</em></p>
<p><em>-4: Giao dịch đang xử lý, vui lòng thử lại sau</em></p>
<p><em>-5: Đầu vào yêu cầu không hợp lệ!</em></p>
<p><em>-6: Không tìm thấy tổ chức</em></p>
<p><em>-7: Không tìm thấy dịch vụ</em></p>
<p><em>-8: Đường dẫn không hợp lệ</em></p>
<p><em>-9: Không tìm thấy mã giao dịch</em></p>
<p><em>-11: Công dân không đồng ý chia sẻ thông tin</em></p>
<p><em>-12: Không tìm thấy cấu hình agent</em></p>
<p><em>-14: Timeout</em></p>
<p><em>-15: Tồn tại trường dữ liệu bắt buộc bị “null”</em></p>
<p><em>-16: Dịch vụ đang sử dụng vượt quá hạn mức cho phép</em></p>
<p><em>-17: Thông tin công dân không hợp lệ, vui lòng kiểm tra chi tiết trên ứng dụng VNeID</em></p>
<p><em>-19: Tài khoản tổ chức không có quyền sử dụng dịch vụ</em></p>
<p><em>-20: Thông tin công dân không đúng</em></p></td>
</tr>
<tr>
<td style="text-align: center;">2</td>
<td>description</td>
<td><p>String</p>
<p>(100 ký tự)</p></td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Mô tả lỗi trường hợp trạng thái tiếp nhận khác 0</td>
</tr>
<tr>
<td style="text-align: center;">3</td>
<td>responseTime</td>
<td><p>String</p>
<p>(100 ký tự)</p></td>
<td style="text-align: center;">x</td>
<td style="text-align: left;"><p>Thời gian phản hồi kết quả</p>
<p>Định dạng: ddMMyyyyHHmmss</p></td>
</tr>
<tr>
<td style="text-align: center;">4</td>
<td>data</td>
<td>JsonObject</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;"><p>Kết quả xác thực sinh trắc học.</p>
<p>Tuân theo chuẩn JWS. Là chuỗi Base64 có format:</p>
<p>&lt;header&gt;.&lt;payload&gt;.&lt;signature&gt;</p></td>
</tr>
<tr>
<td style="text-align: center;"></td>
<td colspan="4" style="text-align: left;">Mô tả cấu trúc JsonObject <strong>data</strong></td>
</tr>
<tr>
<td style="text-align: center;">4.1</td>
<td>biometric</td>
<td>number</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;"><p>Trạng thái công dân đồng ý xác thực sinh trắc học</p>
<p>0 - Không đồng ý</p>
<p>1 - Đồng ý</p></td>
</tr>
<tr>
<td style="text-align: center;">4.2</td>
<td>biometricResult</td>
<td>JsonObject</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Thông tin kết quả xác thực sinh trắc</td>
</tr>
<tr>
<td style="text-align: center;">4.2.1</td>
<td>statusCode</td>
<td>number</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;"><p>Trạng thái kết quả trả về:</p>
<p>0 - Thất bại<br />
1 - Thành công</p>
<p>2 - Số CCCD không đúng định dạng</p></td>
</tr>
<tr>
<td style="text-align: center;">4.2.2</td>
<td>transId</td>
<td>String</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Là trường transId ở request body</td>
</tr>
<tr>
<td colspan="5" style="text-align: left;">Nếu statusCode = 1</td>
</tr>
<tr>
<td style="text-align: center;">4.2.3</td>
<td>data</td>
<td>JsonObject</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Bắt buộc nếu statusCode = 1</td>
</tr>
<tr>
<td style="text-align: center;">4.2.3.1</td>
<td>result</td>
<td>Number</td>
<td style="text-align: center;"></td>
<td style="text-align: left;"><p>Kết quả tổng:</p>
<p>0 - Không xác thực,</p>
<p>1 - Xác thực</p></td>
</tr>
<tr>
<td style="text-align: center;">4.2.3.2</td>
<td>liveness</td>
<td>Object</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Chỉ bắt buộc khi TCDN sử dụng dịch vụ liveness. Trường hợp không sử dụng liveness thì không có Object này trả về.</td>
</tr>
<tr>
<td style="text-align: center;"><em>4.2.3.2.1</em></td>
<td><em>result</em></td>
<td><em>Number</em></td>
<td style="text-align: center;"></td>
<td style="text-align: left;"><p><em>Kết quả của liveness:</em></p>
<p><em>1 - Xác thực</em></p>
<p><em>0 - Không xác thực</em></p></td>
</tr>
<tr>
<td style="text-align: center;"><em>4.2.3.2.2</em></td>
<td><em>score</em></td>
<td><em>Float</em></td>
<td style="text-align: center;"></td>
<td style="text-align: left;"><em>Trọng số của kết quả</em></td>
</tr>
<tr>
<td style="text-align: center;">4.2.3.3</td>
<td>faceMatch</td>
<td>Object</td>
<td style="text-align: center;"></td>
<td style="text-align: left;"></td>
</tr>
<tr>
<td style="text-align: center;">4.2.3.3.1</td>
<td>result</td>
<td>Number</td>
<td style="text-align: center;"></td>
<td style="text-align: left;"><p>Kết quả của faceMatch:</p>
<p>1 - Xác thực</p>
<p>0 - Không xác thực</p></td>
</tr>
<tr>
<td style="text-align: center;">4.2.3.3.2</td>
<td>score</td>
<td>Float</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Trọng số của kết quả</td>
</tr>
<tr>
<td style="text-align: center;">4.2.3.3.3</td>
<td>confidence</td>
<td>Number</td>
<td style="text-align: center;"></td>
<td style="text-align: left;"><p>Độ tin cậy dựa vào score của face match</p>
<p>1 - Thấp</p>
<p>2 - Trung Bình</p>
<p>3 - Cao</p></td>
</tr>
<tr>
<td style="text-align: center;">4.2.3.4</td>
<td>deviceRisk</td>
<td>Object</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Thông tin cảnh báo thiết bị không an toàn.</td>
</tr>
<tr>
<td style="text-align: center;">4.2.3.4.1</td>
<td>isHighRisk</td>
<td>Boolean</td>
<td style="text-align: center;"></td>
<td style="text-align: left;"><p>Cảnh báo thiết bị không an toàn:</p>
<p>- true: Thiết bị không an toàn<br />
- false: Thiết bị an toàn</p></td>
</tr>
<tr>
<td style="text-align: center;">4.2.3.4.2</td>
<td>flags</td>
<td>Array</td>
<td style="text-align: center;"></td>
<td style="text-align: left;"><p>Danh sách mã nguyên nhân cảnh báo thiết bị không an toàn:</p>
<blockquote>
<p>1: REPACK</p>
<p>2: EMULATOR</p>
<p>3: HOOK</p>
<p>4: DEBUG</p>
<p>5: ROOT</p>
<p>6: UNLOCK_BOOTLOADER</p>
<p>8: VIRTUAL_ENVIRONMENT</p>
</blockquote>
<p>10: ADB</p>
<blockquote>
<p>11: DEVELOPMENT_SETTING</p>
</blockquote>
<p>Ví dụ. flags: [2,3,4]</p></td>
</tr>
<tr>
<td colspan="5" style="text-align: left;">Nếu statusCode != 1</td>
</tr>
<tr>
<td style="text-align: center;">4.2.4</td>
<td>errorDetail</td>
<td>String</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Nội dung lỗi xác thực sinh trắc học</td>
</tr>
<tr>
<td style="text-align: center;">5</td>
<td>userInfo</td>
<td>string</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;"><p>Tuân theo chuẩn JWS. Là chuỗi Base64 có format:</p>
<p>&lt;header&gt;.&lt;payload&gt;.&lt;signature&gt;</p></td>
</tr>
<tr>
<td style="text-align: center;"></td>
<td colspan="4"><p><strong>Mô tả cấu trúc trường thông tin userInfo sau khi parse theo chuẩn JWS</strong></p>
<p><em><strong>Lưu ý:</strong> các trường thông tin có thể thay đổi tuỳ vào nhu cầu của TCDN cũng như sự đồng ý chia sẻ của công dân trên ứng dụng VNeID.</em></p></td>
</tr>
<tr>
<td style="text-align: right;">1</td>
<td>fullName</td>
<td>string (100 ký tự)</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Họ và tên đầy đủ của công dân</td>
</tr>
<tr>
<td style="text-align: right;">2</td>
<td>birthDate</td>
<td>string (20 ký tự)</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Ngày tháng năm sinh của công dân, định dạng dd-MM-yyyy</td>
</tr>
<tr>
<td style="text-align: right;">3</td>
<td>citizenPid</td>
<td>string (12 ký tự số)</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Số định danh/ CCCD của công dân</td>
</tr>
<tr>
<td style="text-align: right;">4</td>
<td>dateOfIssue</td>
<td>string (20 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Ngày cấp số định danh/ CCCD của công dân</td>
</tr>
<tr>
<td style="text-align: right;">5</td>
<td>issuingAuthority</td>
<td>string (100 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Nơi cấp định danh/ CCCD của công dân</td>
</tr>
<tr>
<td style="text-align: right;">6</td>
<td>nationalityCode</td>
<td>string (2 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Mã quốc tịch của công dân</td>
</tr>
<tr>
<td style="text-align: right;">7.1</td>
<td>permanentAddress</td>
<td>string (200 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Địa chỉ thường trú (chi tiết) của công dân</td>
</tr>
<tr>
<td style="text-align: right;">7.2</td>
<td>permanentVillageCode</td>
<td>string (20 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Mã xã thường trú (Ref danh mục xã)</td>
</tr>
<tr>
<td style="text-align: right;">7.3</td>
<td>permanentVillageText</td>
<td>string (100 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Tên xã thường trú</td>
</tr>
<tr>
<td style="text-align: right;">7.4</td>
<td>permanentDistrictCode</td>
<td>string (20 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Mã huyện thường trú (Ref danh mục huyện)</td>
</tr>
<tr>
<td style="text-align: right;">7.5</td>
<td>permanentDistrictText</td>
<td>string (100 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Tên huyện thường trú</td>
</tr>
<tr>
<td style="text-align: right;">7.6</td>
<td>permanentCityCode</td>
<td>string (20 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Mã tỉnh thường trú (Ref danh mục tỉnh)</td>
</tr>
<tr>
<td style="text-align: right;">7.7</td>
<td>permanentCityText</td>
<td>string (100 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Tên tỉnh thường trú</td>
</tr>
<tr>
<td style="text-align: right;">8.1</td>
<td style="text-align: left;">livingPlaceAddress</td>
<td>string (200 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Địa chỉ nơi ở hiện tại (chi tiết) của công dân</td>
</tr>
<tr>
<td style="text-align: right;">8.2</td>
<td>livingPlaceVillageCode</td>
<td>string (20 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Mã xã nơi ở hiện tại (Ref danh mục xã)</td>
</tr>
<tr>
<td style="text-align: right;">8.3</td>
<td>livingPlaceVillageText</td>
<td>string (100 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Tên xã nơi ở hiện tại</td>
</tr>
<tr>
<td style="text-align: right;">8.4</td>
<td>livingPlaceDistrictCode</td>
<td>string (20 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Mã huyện nơi ở hiện tại (Ref danh mục huyện)</td>
</tr>
<tr>
<td style="text-align: right;">8.5</td>
<td>livingPlaceDistrictText</td>
<td>string (100 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Tên huyện nơi ở hiện tại</td>
</tr>
<tr>
<td style="text-align: right;">8.6</td>
<td>livingPlaceCityCode</td>
<td>string (20 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Mã tỉnh nơi ở hiện tại (Ref danh mục tỉnh)</td>
</tr>
<tr>
<td style="text-align: right;">8.7</td>
<td>livingPlaceCityText</td>
<td>string (100 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Tên tỉnh nơi ở hiện tại</td>
</tr>
<tr>
<td style="text-align: right;">9</td>
<td>idCardExpireDate</td>
<td>String</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Ngày hết hạn của CCCD (mới nhất đến thời điểm chia sẻ)</td>
</tr>
<tr>
<td style="text-align: right;">10</td>
<td>genderCode</td>
<td>String (10 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;"><p>Mã giới tính của công dân</p>
<p><em>Tham chiếu mục IV.5.1 Mã danh mục giới tính</em></p></td>
</tr>
<tr>
<td style="text-align: right;">11</td>
<td>indentifyNumber</td>
<td>string (20 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Số CMND của công dân</td>
</tr>
</tbody>
</table>

### III.2.5. Universal Link {#iii-2-5-universal-link}

#### Universal Link ứng dụng VNeID

- **Cấu trúc:** UniversalLink/share/\[\$transactionCode\]

- UniversalLink:
  - Môi trường tích hợp: https://universal.dancuquocgia.com/share/\[\$transactionCode\]

  - Môi trường Prod: N/A

- \[\$transactionCode\] : mã giao dịch xin consent khi khởi tạo giao dịch ở API III.2.1

#### RedirectUrl tổ chức sử dụng dịch vụ khai báo cho ứng dụng VneID

12. <span class="mark">\* Universal link khi Switch App to App từ VNeID sang App TCDN có 2 dạng:</span>

    1\. Luồng xuất phát từ Ứng dụng TCDN

- **Cấu trúc:** URL_TCDN/\[\$transactionCode\]\|\[\$result\]

- **Mô tả:**

<table>
<colgroup>
<col style="width: 10%" />
<col style="width: 35%" />
<col style="width: 54%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>STT</strong></th>
<th style="text-align: center;"><strong>Code</strong></th>
<th style="text-align: center;"><strong>Mô tả</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: center;">1</td>
<td style="text-align: left;">URL_TCDN</td>
<td>Tổ chức sử dụng dịch vụ cung cấp</td>
</tr>
<tr>
<td style="text-align: center;">2</td>
<td style="text-align: left;">[$transactionCode]</td>
<td>Mã giao dịch xin consent</td>
</tr>
<tr>
<td style="text-align: center;">3</td>
<td style="text-align: left;">[$result]</td>
<td><p>Kết quả:</p>
<p>1: Đã đồng ý; 2: Không đồng ý; 3: Hết hạn</p></td>
</tr>
</tbody>
</table>

14. <span id="_Toc201751266" class="anchor"></span>2. Luồng xuất phát từ Ứng dụng VNeID

    **- Cấu trúc:** URL_TCDN/\[\$transactionCode\]

    \- Mô tả:

| **STT** | **Code**              | **Mô tả**                        |
| :-----: | :-------------------- | -------------------------------- |
|    1    | URL_TCDN              | Tổ chức sử dụng dịch vụ cung cấp |
|    2    | \[\$transactionCode\] | Mã giao dịch xin consent         |

16.

## III.3. Đăng nhập ứng dụng của TCDN bằng tài khoản VNeID (SSO) {#iii-3-dang-nhap-tcdn-vneid-sso}

17. **Tài liệu tích hợp SSO:** Tổ chức doanh nghiệp tham chiếu tài liệu dưới đây thực hiện tích hợp đăng nhập ứng dụng của TCDN bằng tài khoản VNeID (SSO).

![](images/image8.png)

18. <span class="mark">\* Tài liệu trên chỉ hướng dẫn cách thức, luồng nghiệp vụ dịch vụ SSO vận hành. Không sử dụng các API nêu trong tài liệu VNeID-SSO-V2.0-MT47.docx. TCDN sử dụng _API III.3.3_ và _API III.3.4_ để thực hiện đăng nhập SSO bằng tài khoản VNeID và sử dụng _API III.3.5_ để thực hiện logout tài khoản VNeID*(API III.3.5 chỉ sử dụng đối với SSO qua Web VNeID).*</span>

    Căn cứ vào nhu cầu, TCDN thực hiện khai báo và tích hợp các API dưới đây:

### III.3.1. URL điều hướng đăng nhập sang VNeID {#iii-3-1-url-dieu-huong-dang-nhap}

- **Hệ thống xây dựng URL:** Hệ thống Định danh và xác thực điện tử (VNeID). (Mọi loại tổ chức đều gọi URL này)

- **Mục đích:** URL điều hướng sang trang đăng nhập VNeID gửi kèm các tham số.

- **Cấu trúc:**

20. **WEB:** \$\[URL-SSO\] /auth?response_type=\$\[response_type\]&client_id=\$ \[client_id\]&scope=\$\[openid\]&redirect_uri=\$\[redirect_uri\]&…

    **APP:** \$\[UNIVERSAL-LINK-SSO\] /\$\[client_id\]

<table>
<colgroup>
<col style="width: 7%" />
<col style="width: 24%" />
<col style="width: 11%" />
<col style="width: 9%" />
<col style="width: 46%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>STT</strong></th>
<th style="text-align: center;"><strong>Tên trường</strong></th>
<th style="text-align: center;"><strong>Kiểu dữ liệu</strong></th>
<th style="text-align: center;"><strong>Bắt buộc</strong></th>
<th style="text-align: center;"><strong>Mô tả</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: center;">1</td>
<td style="text-align: center;">URL-SSO/ UNIVERSAL-LINK-SSO</td>
<td style="text-align: center;">String</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;"><p>URL điều hướng sang trang đăng nhập VNeID</p>
<p>Tham khảo mục Danh sách URL, Endpoint &amp; Param cần tích hợp</p></td>
</tr>
<tr>
<td style="text-align: center;">2</td>
<td style="text-align: center;">redirect_uri</td>
<td style="text-align: center;">String</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Url chuyển tiếp nhận lại mã code sau khi xác thực thành công. (URL Tổ chức khai thác dịch vụ cung cấp)</td>
</tr>
<tr>
<td style="text-align: center;">3</td>
<td style="text-align: center;">response_type</td>
<td style="text-align: center;">String</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Giá trị mặc định là ‘code’</td>
</tr>
<tr>
<td style="text-align: center;">4</td>
<td style="text-align: center;">scope</td>
<td style="text-align: center;">String</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Giá trị mặc định là ‘openid’</td>
</tr>
<tr>
<td style="text-align: center;">5</td>
<td colspan="4" style="text-align: center;">Tổ chức khai thác dịch vụ có thể định nghĩa thêm các trường cần thiết rồi truyền vào URL, hệ thống VNeID sẽ trả về giá trị tương ứng như thế ở redirect_uri</td>
</tr>
</tbody>
</table>

### III.3.2. Redirect_URI của tổ chức khai thác dịch vụ {#iii-3-2-redirect-uri}

22. **Hệ thống xây dựng URL:** Tổ chức khai thác dịch vụ

    **Mục đích:** URL chuyển tiếp của TCDN để nhận lại mã code sau khi xác thực thành công.

    **Cấu trúc:** \$\[Redirect_uri\]?code=\$\[code\]&encrypt_key=\$\[encrypt_key\]

<table>
<colgroup>
<col style="width: 7%" />
<col style="width: 24%" />
<col style="width: 11%" />
<col style="width: 9%" />
<col style="width: 46%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>STT</strong></th>
<th style="text-align: center;"><strong>Tên trường</strong></th>
<th style="text-align: center;"><strong>Kiểu dữ liệu</strong></th>
<th style="text-align: center;"><strong>Bắt buộc</strong></th>
<th style="text-align: center;"><strong>Mô tả</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: center;">1</td>
<td style="text-align: center;">code</td>
<td style="text-align: center;">String</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Code xác thực thành công</td>
</tr>
<tr>
<td style="text-align: center;">2</td>
<td style="text-align: center;">encrypt_key</td>
<td style="text-align: center;">String</td>
<td style="text-align: center;"></td>
<td style="text-align: left;"><p>Secret key của thuật toán mã hóa đối xứng AES. Key này đã được VNeID mã hóa bằng thuật toán bất đối xứng RSA.</p>
<p>Nếu trường này có giá trị nghĩa là dữ liệu sẽ mã hoá</p>
<p>Nếu không có giá trị hoặc không tồn tại thì đầu ra sẽ trả giống như mục <strong>Đặc tả giá trị nhận được sau khi giải mã data nhận được từ VNeID</strong></p>
<p>Lưu ý: Không sử dụng hàm getQuery param có sẵn của ngôn ngữ lập trình</p></td>
</tr>
</tbody>
</table>

### III.3.3. TCDN lấy token khi người dùng đăng nhập thành công trên ứng dụng VNeID {#iii-3-3-tcdn-lay-token}

#### API Specs

<table>
<colgroup>
<col style="width: 16%" />
<col style="width: 83%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>Thông tin</strong></th>
<th style="text-align: center;"><strong>Mô tả</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td>Mục đích</td>
<td>Cung cấp accessToken cho TCDK khai thác sau khi người dùng login VNeID và chuyển hướng về ứng dụng của TCDN</td>
</tr>
<tr>
<td>URL</td>
<td><p>https://&lt;domain&gt;:&lt;port&gt;/api/tcdn/<strong>{service-code}/</strong>api/vneid/form/sso</p>
<p><strong>{service-code}</strong> tham chiếu theo bảng Mã dịch vụ IV.3</p>
<p>Với API Get-token thì service code thường sẽ là <em><strong>“sso-token”</strong></em></p></td>
</tr>
<tr>
<td>Method</td>
<td>POST</td>
</tr>
<tr>
<td>Header</td>
<td><ul>
<li><p><strong>accept:</strong> application/json, text/plain, */*</p></li>
<li><p><strong>content-type:</strong> application/x-www-form-urlencoded</p></li>
<li><p><strong>api_key:</strong> API key do Tổ chức cung cấp dịch vụ cung cấp</p></li>
<li><p><strong>request_id:</strong> Mã yêu cầu do hệ thống của TCDN tự sinh, là duy nhất trên hệ thống.</p></li>
<li><p><mark><strong>target_code:</strong> Mã mục đích chia sẻ, xử lý thông tin do Hệ thống định danh cung cấp, phục vụ hiển thị đúng nội dung mục đích chia sẻ, xử lý thông tin trên ứng dụng VNeID.</mark></p></li>
<li><p><strong>request_time:</strong> Thời gian yêu cầu, định dạng ddMMyyyyHHmmss. Là thời gian hiện tại của hệ thống, trong trường hợp request_time gửi sang Hệ thống nền tảng lệch quá 2 phút so với thời gian hiện tại của Hệ thống nền tảng thì request đó được coi là timeout.</p></li>
</ul></td>
</tr>
<tr>
<td>Request body</td>
<td><p>--data-urlencode 'code= code trả về ở redirect_uri TCDN khai báo tại mục III.3.2' \</p>
<p>--data-urlencode 'grant_type= mặc định là authorization_code' \</p>
<p>--data-urlencode 'client_id= Client' \ //</p>
<p>--data-urlencode 'redirect_uri= redirect_uri của TCDN khai thác' \</p>
<p>--data-urlencode 'client_secret= 36f70746-fa43-4aa0-aef1-531cc14f9006'</p></td>
</tr>
<tr>
<td>Respond body</td>
<td style="text-align: left;"><p>Status Code: 200</p>
<p>{</p>
<p>    "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldU",</p>
<p>    "expires_in": 3600,</p>
<p>    "refresh_expires_in": 1800,</p>
<p>    "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICIzMz ",</p>
<p>    "token_type": "Bearer",</p>
<p>    "id_token": "Dr4wTq9W07d7k3iK5T6J232CNXFngIhEXojH8nJV2jfiI8NiXQ",</p>
<p>    "not-before-policy": 1667019564,</p>
<p>    "session_state": "456b0443-8b89-4b4f-810d-fc5e2a04c162",</p>
<p>    "scope": "openid email profile"</p>
<p>}</p></td>
</tr>
</tbody>
</table>

#### Request body

<table>
<colgroup>
<col style="width: 7%" />
<col style="width: 16%" />
<col style="width: 14%" />
<col style="width: 10%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>STT</strong></th>
<th style="text-align: center;"><strong>Tên trường</strong></th>
<th style="text-align: center;"><strong>Kiểu dữ liệu</strong></th>
<th style="text-align: center;"><strong>Bắt buộc</strong></th>
<th style="text-align: center;"><strong>Mô tả</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: center;">1</td>
<td style="text-align: left;">client_id</td>
<td style="text-align: center;">String</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Mã đăng ký của tổ chức khai thác dịch vụ - Do tổ chức cung cấp dịch vụ cung cấp</td>
</tr>
<tr>
<td style="text-align: center;">2</td>
<td style="text-align: left;">client_secret</td>
<td style="text-align: center;">String</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Secret key của tổ chức khai thác dịch vụ - Do tổ chức cung cấp dịch vụ cung cấp</td>
</tr>
<tr>
<td style="text-align: center;">3</td>
<td style="text-align: left;">grant_type</td>
<td style="text-align: center;">String</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Kiểu xác thực ‘authorization_code’</td>
</tr>
<tr>
<td style="text-align: center;">4</td>
<td style="text-align: left;">code</td>
<td style="text-align: center;">String</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Code xác thực nhận được sau khi VNeID redirect lại link của tổ chức khai thác dịch vụ</td>
</tr>
<tr>
<td style="text-align: center;">5</td>
<td style="text-align: left;">redirect_uri</td>
<td style="text-align: center;">String</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">URL chuyển tiếp nhận lại mã code sau khi xác thực thành công. (URL tổ chức khai thác dịch vụ cung cấp)<br />
* Lưu ý: Không để Địa chỉ ip, Localhost</td>
</tr>
</tbody>
</table>

#### Respond body

<table>
<colgroup>
<col style="width: 9%" />
<col style="width: 29%" />
<col style="width: 14%" />
<col style="width: 8%" />
<col style="width: 38%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>STT</strong></th>
<th style="text-align: center;"><strong>Tên trường</strong></th>
<th style="text-align: center;"><strong>Kiểu dữ liệu</strong></th>
<th style="text-align: center;"><strong>Bắt buộc</strong></th>
<th style="text-align: center;"><strong>Ghi chú</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: center;">1</td>
<td style="text-align: left;">data</td>
<td style="text-align: center;">String</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Giá trị data đã được VNeID mã hóa bằng thuật toán mã hóa đối xứng AES</td>
</tr>
<tr>
<td style="text-align: center;">2</td>
<td style="text-align: left;">key</td>
<td style="text-align: center;">String</td>
<td style="text-align: center;"></td>
<td style="text-align: left;"><p>Secret key của thuật toán mã hóa đối xứng AES. Key này đã được VNeID mã hóa bằng thuật toán bất đối xứng RSA.</p>
<p>Nếu trường này có giá trị nghĩa là dữ liệu sẽ mã hoá</p>
<p>Nếu không có giá trị hoặc không tồn tại thì đầu ra sẽ trả giống như mục <strong>Đặc tả giá trị nhận được sau khi giải mã data nhận được</strong>.</p></td>
</tr>
</tbody>
</table>

**Đặc tả giá trị nhận được sau khi giải mã data nhận được**

| **STT** | **Tên trường**     | **Kiểu dữ liệu** | **Bắt buộc** | **Ghi chú**                                                                          |
| :-----: | :----------------- | :--------------: | :----------: | ------------------------------------------------------------------------------------ |
|    1    | access_token       |      String      |      x       | Mã xác thực phiên đăng nhập                                                          |
|    2    | expires_in         |      Number      |      x       | Thời hạn xác thực phiên đăng nhập (đơn vị giây)                                      |
|    3    | refresh_expires_in |      Number      |      x       | Thời hạn làm mới xác thực phiên đăng nhập (đơn vị giây)                              |
|    4    | refresh_token      |      String      |      x       | Mã xác thực làm mới phiên đăng nhập (đơn vị giây)                                    |
|    5    | token_type         |      String      |      x       | Kiểu mã xác thực ‘Bearer’                                                            |
|    6    | id_token           |      String      |      x       | Id của mã xác thực                                                                   |
|    7    | not-before-policy  |      String      |              | Thông tin trả thêm. Tổ chức khai thác dịch vụ không sử dụng các trường thông tin này |

### III.3.4. TCDN lấy thông tin công dân sau khi đã có token đăng nhập {#iii-3-4-tcdn-lay-thong-tin-cong-dan}

#### API Specs

<table>
<colgroup>
<col style="width: 16%" />
<col style="width: 83%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>Thông tin</strong></th>
<th style="text-align: center;"><strong>Mô tả</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td>Mục đích</td>
<td>API get userInfo khi người dùng login từ VNEID sau khi đã có token</td>
</tr>
<tr>
<td>URL</td>
<td><p>https://&lt;domain&gt;/api/tcdn/<strong>{service-code}/</strong>api/vneid/sso</p>
<p><strong>{service-code}</strong> tham chiếu theo bảng Mã dịch vụ IV.3.<br />
Với API Get-userinfo thì service code thường sẽ là <em><strong>“sso-userinfo”</strong></em></p></td>
</tr>
<tr>
<td>Method</td>
<td>GET</td>
</tr>
<tr>
<td>Header</td>
<td><ul>
<li><p><strong>content-type:</strong> application/json; charset=utf-8</p></li>
<li><p><strong>Authorization</strong>: Bearer [Mã xác thực phiên đăng nhập]</p></li>
<li><p><strong>api_key:</strong> API key do Tổ chức cung cấp dịch vụ cung cấp</p></li>
<li><p><strong>request_id:</strong> Mã yêu cầu do hệ thống của TCDN tự sinh, là duy nhất trên hệ thống.</p></li>
<li><p><mark><strong>target_code:</strong> Mã mục đích chia sẻ, xử lý thông tin do Hệ thống định danh cung cấp, phục vụ hiển thị đúng nội dung mục đích chia sẻ, xử lý thông tin trên ứng dụng VNeID.</mark></p></li>
<li><p><strong>request_time:</strong> Thời gian yêu cầu, định dạng ddMMyyyyHHmmss. Là thời gian hiện tại của hệ thống, trong trường hợp request_time gửi sang Hệ thống nền tảng lệch quá 2 phút so với thời gian hiện tại của Hệ thống nền tảng thì request đó được coi là timeout.</p></li>
</ul></td>
</tr>
<tr>
<td>Request body</td>
<td style="text-align: left;">N/A</td>
</tr>
<tr>
<td>Response body</td>
<td style="text-align: left;"><p>Status Code: 200</p>
<p>Response body khi nhận về dưới dạng mã hoá</p>
<p>{</p>
<p>"data":"9sCIPPKfkuHJyZbq4nF4Z70A6Tm9ul6wd8oBsaTZr3qa18lcw8XzoXYYPcpWGj4y2I606OZ6NoPN97r+9l7CpOxjj7WRTRdnw8a2xg3xW1qnw180XrMCUot6wR5WTN1vHmC2nvy/14xfPYahsBDw5u9RPBQVCEO4soelJbB1XueUA05osICj2ycYrjHW+ZvMsCi9lHykkK2nAnFZfV9U1vCJuUu0T3y+pBMnu2mAHOnTPueFgGgLEMX0Xy+LmpxrLoO45ulMis9/He2pc9Tf6dQ5RWYgrPmSw4w/drFO/Up46e8HtALHX/55O3C4ucARjzWpXf0o7PHrE3oWEnjyTgVFJ6Zi5F+GYRPr82JddRp2YAH98mBdyzD7OioDyETuafza05/Un6lFRSDagmeSsTMReLbvlbQbcf38oU1xQFv4UJ7zbsglGxMfX4EqVmzjd+XaO227uWxo5TJmv6mSQRKSVfH2jelLCsOWr8DrNV0=",</p>
<p>"key":"OJkjyfkxJ8mIYCGsyvihdEYVJr+UaSBO8ccyL2KiuYESqamq+0VWFlR6zqzvCq0VRLMzm2NlDLwoc4NMPx+BI2cONT2Pz3AhzGQee7//WReeZ3RDB1AW7tjgrDxuu35kycx7pN0vWhRjvLyzaVdMITaqnVan03X/ccM6LD2GlbvLbQUmfRC3+yoHVgq3EMSUPtPB/V2OKCnDYaWecVBWdGGRuY9LiBAOreFDN97KvEzprIoFTssBhOQNNmU+tOlsAeSIqvqunC2n4itBg7agGnnXmHvkV7CQJHhhwqbHVm8cYWx0DVhOizXixkQ53uEokKqUcnCrFBdf7911c3jNBQ=="</p>
<p>}</p>
<p>Response body sau khi giải mã</p>
<p>{</p>
<p>"accountType": 1,</p>
<p>"transactionCode": "57cf4c7c-ce43-42c2-89f6-5d9e5c88c760",</p>
<p>"accountLevel": 2,</p>
<p>"citizenPid": "030095008791",</p>
<p>"fullName": "Nguyễn Huy Hoàng",</p>
<p>"birthDate": "30-11-1995",</p>
<p>"signature": "eyJhbGciOiJFUzI1NiIsInR5cCI6IkpPU0UvSldTIn0=.PEThu68gbGnhu4d1IGPhuqduIGvDvT4=.MEYCIQCZTe5k9SrOzu-p1xUmjYGvLHpgErR56WLGKwYEqftGoAIhAJUrbbI-GQnicA57cRyigE7hDHxJre56_Chvk4bg7H-p"</p>
<p>}</p></td>
</tr>
</tbody>
</table>

#### Request body

- N/A

#### Respond body

<table>
<colgroup>
<col style="width: 9%" />
<col style="width: 29%" />
<col style="width: 14%" />
<col style="width: 8%" />
<col style="width: 38%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>STT</strong></th>
<th style="text-align: center;"><strong>Tên trường</strong></th>
<th style="text-align: center;"><strong>Kiểu dữ liệu</strong></th>
<th style="text-align: center;"><strong>Bắt buộc</strong></th>
<th style="text-align: center;"><strong>Ghi chú</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: center;">1</td>
<td style="text-align: left;">data</td>
<td style="text-align: center;">String</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Giá trị data đã được VNeID mã hóa bằng thuật toán mã hóa đối xứng AES</td>
</tr>
<tr>
<td style="text-align: center;">2</td>
<td style="text-align: left;">key</td>
<td style="text-align: center;">String</td>
<td style="text-align: center;"></td>
<td style="text-align: left;"><p>Secret key của thuật toán mã hóa đối xứng AES. Key này đã được VNeID mã hóa bằng thuật toán bất đối xứng RSA.</p>
<p>Nếu trường này có giá trị nghĩa là dữ liệu sẽ mã hoá</p>
<p>Nếu không có giá trị hoặc không tồn tại thì đầu ra sẽ trả giống như mục <strong>Đặc tả giá trị nhận được sau khi giải mã data nhận được</strong>.</p></td>
</tr>
</tbody>
</table>

**Đặc tả giá trị nhận được sau khi giải mã data nhận được**

| **STT** | **Tên trường**  | **Kiểu dữ liệu** | **Bắt buộc** | **Ghi chú**                                            |
| :-----: | :-------------- | :--------------: | :----------: | ------------------------------------------------------ |
|    1    | citizenPid      |      String      |              | Số định danh cá nhân                                   |
|    2    | fullName        |      String      |              | Thông tin họ, chữ đệm và tên                           |
|    3    | birthDate       |      String      |              | Thông tin ngày, tháng, năm sinh. Định dạng: DD-MM-YYYY |
|    4    | accountType     |      String      |              | Tài khoản dành cho cá nhân hoặc tổ chức                |
|    5    | accountLevel    |     Integer      |              | Mức level tài khoản VNeID                              |
|    6    | transactionCode |      String      |              | Mã giao dịch                                           |

### III.3.5. URL đăng xuất VneID {#iii-3-5-url-dang-xuat-vneid}

- **Hệ thống xây dựng URL:** Hệ thống Định danh và xác thực điện tử (VNeID).

- **Mục đích:** URL đăng xuất VNeID (chỉ sso web gọi).

- Cấu trúc: \$\[URL-LOGOUT\]?client_id=\$\[client_id\]&redirect_uri=\$\[redirect_uri\]&…

<table style="width:97%;">
<colgroup>
<col style="width: 7%" />
<col style="width: 16%" />
<col style="width: 10%" />
<col style="width: 10%" />
<col style="width: 51%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>STT</strong></th>
<th style="text-align: center;"><strong>Tên trường</strong></th>
<th style="text-align: center;"><strong>Kiểu dữ liệu</strong></th>
<th style="text-align: center;"><strong>Bắt buộc</strong></th>
<th style="text-align: center;"><strong>Mô tả</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: center;">1</td>
<td style="text-align: center;">URL-LOGOUT</td>
<td style="text-align: center;">String</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;"><p>URL điều hướng sang link logout VNeID</p>
<p>Tham khảo mục Danh sách URL, Endpoint &amp; Param cần tích hợp</p></td>
</tr>
<tr>
<td style="text-align: center;">2</td>
<td style="text-align: center;">client_id</td>
<td style="text-align: center;">String</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Mã đăng ký của tổ chức khai thác dịch vụ</td>
</tr>
<tr>
<td style="text-align: center;">3</td>
<td style="text-align: center;">redirect_uri</td>
<td style="text-align: center;">String</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;"><p>Url chuyển tiếp sau khi đăng xuất thành công. (URL Tổ chức khai thác dịch vụ cung cấp)</p>
<p>* Lưu ý: Không để Địa chỉ ip, Localhost</p></td>
</tr>
</tbody>
</table>

- **Đặc tả đầu ra:** Chuyển hướng về redirect_uri do TCDN khai thác cung cấp

## III.4. Yêu cầu rút lại sự đồng ý chia sẻ dữ liệu, xoá dữ liệu công dân đã đồng ý chia sẻ từ ứng dụng VNeID {#iii-4-yeu-cau-rut-lai-su-dong-y}

### III.4.1. API nhận yêu cầu rút lại sự đồng ý chia sẻ dữ liệu, xóa dữ liệu công dân đã đồng ý chia sẻ từ ứng dụng VNeID {#iii-4-1-api-nhan-yeu-cau-rut-lai}

\* **Tổ chức doanh nghiệp cần cung cấp** đường link dẫn tới trang **“Công bố thông tin rủi ro khi công dân rút lại sự đồng ý chia sẻ dữ liệu, tổ chức doanh nghiệp thực hiện xoá dữ liệu công dân đã đồng ý chia sẻ”** để hiển thị trên ứng dụng VNeID cho công dân.

#### API Specs

<table>
<colgroup>
<col style="width: 15%" />
<col style="width: 84%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>Thông tin</strong></th>
<th style="text-align: center;"><strong>Mô tả</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: left;">Tên API</td>
<td style="text-align: left;">API nhận yêu cầu rút lại sự đồng ý chia sẻ dữ liệu, xóa dữ liệu công dân đã đồng ý chia sẻ xuất phát từ ứng dụng VNeID.</td>
</tr>
<tr>
<td style="text-align: left;">Mục đích</td>
<td style="text-align: left;">Cho phép công dân gửi yêu cầu rút lại sự đồng ý chia sẻ dữ liệu, xóa dữ liệu công dân đã đồng ý chia sẻ từ ứng dụng VNeID đối với một giao dịch đã đồng ý chia sẻ trước đó.</td>
</tr>
<tr>
<td style="text-align: left;">URL</td>
<td style="text-align: left;">TCDN định nghĩa và cấu hình sẵn trong AgentGW</td>
</tr>
<tr>
<td style="text-align: left;">Method</td>
<td style="text-align: left;">POST</td>
</tr>
<tr>
<td style="text-align: left;">Header</td>
<td style="text-align: left;"><ul>
<li><p><strong>Content-Type:</strong> application/json; charset=utf-8</p></li>
<li><p><strong>api_key:</strong> API Key do TCDN cung cấp</p></li>
<li><p><strong>org_code:</strong> Mã TCDN do Nền tảng cung cấp.</p></li>
<li><p><strong>service_code:</strong> <mark>withdraw-consent</mark> (Mã yêu cầu rút lại sự đồng ý chia sẻ dữ liệu, xóa dữ liệu công dân đã đồng ý chia sẻ ở mục V.3)</p></li>
<li><p><strong>request_id:</strong> Mã yêu cầu do hệ thống Nền tảng tự sinh, là duy nhất trên hệ thống.</p></li>
<li><p><strong>request_time:</strong> Thời gian yêu cầu, định dạng ddMMyyyyHHmmss.</p></li>
</ul></td>
</tr>
<tr>
<td style="text-align: left;">Request body</td>
<td style="text-align: left;"><p>{</p>
<p>    "transactionCode":"22fc12e0-be90-4519-8ef6-cdeca95ae22b"</p>
<blockquote>
<p>//Mã giao dịch cần rút lại sự đồng ý chia sẻ dữ liệu, xóa dữ liệu công dân đã đồng ý chia sẻ</p>
</blockquote>
<p>}</p></td>
</tr>
<tr>
<td style="text-align: left;">Response body</td>
<td style="text-align: left;"><p>Status Code: 200</p>
<p>Thành công</p>
<p>{</p>
<p>    "statusCode": 1, //1 thành công, 0 không thành công</p>
<p>    "requestStatus": " DELETED"</p>
<p>//DELETED: Đã xóa thành công, PROCESSING: Đã tiếp nhận và đang xử lý</p>
<p>}</p>
<p>Không thành công</p>
<p>{</p>
<p>    "statusCode": 0, //1 thành công, 0 không thành công</p>
<p>    "errorDetail": "Mô tả lỗi"</p>
<p>}</p></td>
</tr>
</tbody>
</table>

#### Request body

<table>
<colgroup>
<col style="width: 7%" />
<col style="width: 20%" />
<col style="width: 12%" />
<col style="width: 10%" />
<col style="width: 49%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>STT</strong></th>
<th style="text-align: center;"><strong>Tên trường</strong></th>
<th style="text-align: center;"><strong>Kiểu dữ liệu</strong></th>
<th style="text-align: center;"><strong>Bắt buộc</strong></th>
<th style="text-align: center;"><strong>Mô tả</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: center;">1</td>
<td style="text-align: left;">transactionCode</td>
<td style="text-align: center;">String</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;"><p>Mã giao dịch cần rút lại sự đồng ý chia sẻ dữ liệu, xóa dữ liệu công dân đã đồng ý chia sẻ</p>
<p>* Mã giao dịch này chính là mã TCDN đã thực hiện khởi tạo giao dịch ở <em>API III.2.1</em> và thực hiện yêu cầu trả kết quả dữ liệu công dân ở <em>API III.2.3</em> và <em>API III.2.4</em></p></td>
</tr>
</tbody>
</table>

#### Respond body

<table>
<colgroup>
<col style="width: 9%" />
<col style="width: 29%" />
<col style="width: 14%" />
<col style="width: 8%" />
<col style="width: 38%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>STT</strong></th>
<th style="text-align: center;"><strong>Tên trường</strong></th>
<th style="text-align: center;"><strong>Kiểu dữ liệu</strong></th>
<th style="text-align: center;"><strong>Bắt buộc</strong></th>
<th style="text-align: center;"><strong>Ghi chú</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: center;">1</td>
<td style="text-align: left;">statusCode</td>
<td style="text-align: center;">number (1)</td>
<td style="text-align: center;">x</td>
<td><p>Trạng thái kết quả trả về:</p>
<p>- 1: Thành công</p>
<p>- 0: Không thành công</p></td>
</tr>
<tr>
<td style="text-align: center;">2</td>
<td style="text-align: left;">requestStatus</td>
<td style="text-align: center;">string (100)</td>
<td style="text-align: center;"></td>
<td><p>Mã định nghĩa kết quả của yêu cầu</p>
<p>- DELETED: Đã rút lại sự đồng ý thành công</p>
<p>- PROCESSING: Đã tiếp nhận và đang xử lý</p></td>
</tr>
<tr>
<td style="text-align: center;">3</td>
<td style="text-align: left;">errorDetail</td>
<td style="text-align: center;">string (4000)</td>
<td style="text-align: center;"></td>
<td><p>Mô tả chi tiết lỗi.</p>
<p>Bắt buộc khi statusCode = 0</p></td>
</tr>
</tbody>
</table>

### III.4.2. API trả kết quả xử lý yêu cầu xóa dữ liệu công dân đã đồng ý chia sẻ {#iii-4-2-api-tra-ket-qua-xu-ly-xoa-du-lieu}

\* **Tổ chức doanh nghiệp cần cung cấp** đường link dẫn tới trang công bố thông tin rủi ro khi công dân rút lại sự đồng ý chia sẻ dữ liệu, tổ chức doanh nghiệp thực hiện xoá dữ liệu công dân đã đồng ý chia sẻ để hiển thị trên ứng dụng VNeID cho công dân.

#### API Specs

<table>
<colgroup>
<col style="width: 15%" />
<col style="width: 84%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>Thông tin</strong></th>
<th style="text-align: center;"><strong>Mô tả</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td>Mục đích</td>
<td>API dùng để hệ thống TCDN trả kết quả xử lý yêu cầu xóa dữ liệu công dân đã đồng ý chia sẻ thông qua AgentGW</td>
</tr>
<tr>
<td>URL</td>
<td><p>https://&lt;domain&gt;:&lt;port&gt;/api/tcdn<strong>/{service-code}</strong>/agent/account/receive-result</p>
<p><strong>{service-code}</strong> tham chiếu theo bảng Mã dịch vụ V.3</p>
<p>Ví dụ: <mark>withdraw-consent-result</mark></p></td>
</tr>
<tr>
<td>Method</td>
<td>POST</td>
</tr>
<tr>
<td>Header</td>
<td><ul>
<li><p><strong>Content-Type:</strong> application/json; charset=utf-8</p></li>
<li><p><strong>api_key:</strong> API key //doanh nghiệp cấu hình do AgentGW cung cấp</p></li>
<li><p><strong>request_id:</strong> Mã yêu cầu tham chiếu đến <strong>request_id</strong> nhận được ở API III.4.1.</p></li>
<li><p><strong>target_code:</strong> <mark>WITHDRAW-CONSENT-RESULT</mark> //Mã này mặc định cho API III.4.2.1</p></li>
<li><p><strong>request_time:</strong> Thời gian yêu cầu, định dạng ddMMyyyyHHmmss. Là thời gian hiện tại của hệ thống, trong trường hợp request_time gửi sang Hệ thống nền tảng lệch quá 2 phút so với thời gian hiện tại của Hệ thống nền tảng thì request đó được coi là timeout.</p></li>
</ul></td>
</tr>
<tr>
<td>Params</td>
<td><p>Trên môi trường non-production:</p>
<p>code=mã đơn vị kết nối (do Đơn vị cung cấp dịch vụ cấp)</p>
<p>Trên môi trường prod: N/A</p></td>
</tr>
<tr>
<td>Request body</td>
<td><p>Kết quả xử lý dữ liệu công dân yêu cầu hủy gửi từ TCDN theo cấu trúc được mô tả như ví dụ dưới đây:</p>
<p>TCDN trả kết quả “TỪ CHỐI XÓA”</p>
<p>{</p>
<p>"requestStatus": "REJECT",</p>
<p>"reasonRequestCode": "Mã lý do từ chối thống nhất giữa VNeID và Tổ chức doanh nghiệp (bắt buộc với requestStatus = REJECT)",</p>
<p>"reasonRequestText": "Nội dung lỗi do Tổ chức doanh nghiệp trả về có quy định về mã lý do (bắt buộc với reasonRequestCode = null)"</p>
<p>}</p>
<p>TCDN trả kết quả “ĐÃ XÓA THÀNH CÔNG”</p>
<p>{</p>
<p>"requestStatus": "DELETED",</p>
<p>"reasonRequestCode": "Mã lý do thống nhất giữa VNeID và TCDN",</p>
<p>"reasonRequestText": "Nội dung TCDN trả về có quy định về mã lý do"</p>
<p>}</p></td>
</tr>
<tr>
<td>Response body</td>
<td style="text-align: left;"><p>Status Code: 200</p>
<p>Thành công</p>
<p>{</p>
<p>    "statusCode": 1, //1 thành công, 0 không thành công</p>
<p>}</p>
<p>Không thành công</p>
<p>{</p>
<p>    "statusCode": 0, //1 thành công, 0 không thành công</p>
<p>    "errorDetail": "Mô tả lỗi"</p>
<p>}</p></td>
</tr>
</tbody>
</table>

#### Request body

<table>
<colgroup>
<col style="width: 7%" />
<col style="width: 24%" />
<col style="width: 11%" />
<col style="width: 12%" />
<col style="width: 43%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>STT</strong></th>
<th style="text-align: center;"><strong>Tên trường</strong></th>
<th style="text-align: center;"><strong>Kiểu dữ liệu</strong></th>
<th style="text-align: center;"><strong>Bắt buộc</strong></th>
<th style="text-align: center;"><strong>Mô tả</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: center;">1</td>
<td style="text-align: left;">requestStatus</td>
<td style="text-align: center;">number (1)</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;"><p>Mã định nghĩa kết quả của yêu cầu</p>
<p>- DELETED: Đã xoá thành công</p>
<p>- REJECT: Từ chối xoá</p></td>
</tr>
<tr>
<td style="text-align: center;">2</td>
<td style="text-align: left;">reasonRequestCode</td>
<td style="text-align: center;">string (100)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;"><p>Mã lý do từ chối.</p>
<p>Bắt buộc khi requestStatus = REJECT</p></td>
</tr>
<tr>
<td style="text-align: center;">3</td>
<td style="text-align: left;">reasonRequestText</td>
<td style="text-align: center;">string (1000)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;"><p>Chi tiết nội dung từ chối</p>
<p>Bắt buộc khi reasonRequestCode = null</p></td>
</tr>
</tbody>
</table>

#### Respond body

<table>
<colgroup>
<col style="width: 9%" />
<col style="width: 29%" />
<col style="width: 14%" />
<col style="width: 8%" />
<col style="width: 38%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>STT</strong></th>
<th style="text-align: center;"><strong>Tên trường</strong></th>
<th style="text-align: center;"><strong>Kiểu dữ liệu</strong></th>
<th style="text-align: center;"><strong>Bắt buộc</strong></th>
<th style="text-align: center;"><strong>Ghi chú</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: center;">1</td>
<td style="text-align: left;">statusCode</td>
<td style="text-align: center;">number (1)</td>
<td style="text-align: center;">x</td>
<td><p>Trạng thái kết quả trả về:</p>
<p>- 1: Thành công</p>
<p>- 0: Không thành công</p></td>
</tr>
<tr>
<td style="text-align: center;">2</td>
<td style="text-align: left;">errorDetail</td>
<td style="text-align: center;">string (4000)</td>
<td style="text-align: center;"></td>
<td><p>Mô tả chi tiết lỗi.</p>
<p>Bắt buộc khi statusCode = 0</p></td>
</tr>
</tbody>
</table>

## III.5. Ứng dụng doanh nghiệp trả kết quả xử lý dữ liệu công dân {#iii-5-ung-dung-doanh-nghiep-tra-ket-qua}

### III.5.1. API Specs {#iii-5-1-api-specs}

<table>
<colgroup>
<col style="width: 15%" />
<col style="width: 84%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>Thông tin</strong></th>
<th style="text-align: center;"><strong>Mô tả</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: left;">Mục đích</td>
<td style="text-align: left;">API dùng để hệ thống TCDN trả về kết quả xử lý dữ liệu thông tin định danh công dân cho NT thông qua AgentGW</td>
</tr>
<tr>
<td style="text-align: left;">URL</td>
<td style="text-align: left;"><p>https://&lt;domain&gt;:&lt;port&gt;/api/tcdn<strong>/{service-code}</strong>/agent/account/receive-result</p>
<p><strong>{service-code}</strong> tham chiếu theo bảng Mã dịch vụ IV.3.</p>
<p>Ví dụ: {service-code} trả kết quả mở tài khoản là create-account-result</p></td>
</tr>
<tr>
<td style="text-align: left;">Method</td>
<td style="text-align: left;">POST</td>
</tr>
<tr>
<td style="text-align: left;">Header</td>
<td style="text-align: left;"><ul>
<li><p><strong>Content-Type:</strong> application/json; charset=utf-8</p></li>
<li><p><strong>api_key:</strong> API key //doanh nghiệp cấu hình do AgentGW cung cấp</p></li>
<li><p><mark><strong>target_code:</strong> Mã mục đích chia sẻ, xử lý thông tin do Hệ thống định danh cung cấp, phục vụ hiển thị đúng nội dung mục đích chia sẻ, xử lý thông tin trên ứng dụng VNeID.</mark></p></li>
<li><p><strong>request_id:</strong> Mã yêu cầu do hệ thống của TCDN tự sinh, là duy nhất trên hệ thống.</p></li>
<li><p><strong>request_time:</strong> Thời gian yêu cầu, định dạng ddMMyyyyHHmmss. Là thời gian hiện tại của hệ thống, trong trường hợp request_time gửi sang Hệ thống nền tảng lệch quá 2 phút so với thời gian hiện tại của Hệ thống nền tảng thì request đó được coi là timeout.</p></li>
</ul></td>
</tr>
<tr>
<td style="text-align: left;">Params</td>
<td style="text-align: left;"><p>Trên môi trường non-production:</p>
<p>code=mã đơn vị kết nối (do Đơn vị cung cấp dịch vụ cấp)</p>
<p>Trên môi trường prod: N/A</p></td>
</tr>
<tr>
<td style="text-align: left;">Request body</td>
<td style="text-align: left;"><p>Kết quả xử lý dữ liệu thông tin định danh công dân từ TCDN theo cấu trúc được mô tả như ví dụ dưới đây:</p>
<p>Request body trả kết quả về VNeID khi kết quả xử lý THẤT BẠI</p>
<p>{</p>
<p>"requestStatus": "FAILURE",</p>
<p>"transactionId": "Mã giao dịch mà ứng dụng doanh nghiệp nhận được khi khởi tạo giao dịch",</p>
<p>"reasonRequestCode": "Mã lý do từ chối thống nhất giữa VNeID và Tổ chức doanh nghiệp",</p>
<p>"reasonRequestText": "Nội dung lỗi do Tổ chức doanh nghiệp trả về nếu chưa có quy định về mã lý do"</p>
<p>}</p>
<p>Request body trả kết quả về VNeID khi kết quả xử lý THÀNH CÔNG</p>
<p>{</p>
<p>    "requestStatus": "SUCCESS",</p>
<p>    "transactionId": "Mã giao dịch mà ứng dụng doanh nghiệp nhận được khi khởi tạo giao dịch",</p>
<p>    "fieldName1": "value"  //Tiếp theo là các trường thông tin chi tiết cần trả để hiển thị trên ứng dụng VNeID.</p>
<p>    //Chi tiết xem tại các mục mô tả chi tiết Request body đối với từng dịch vụ dưới đây.</p>
<p>}</p></td>
</tr>
<tr>
<td style="text-align: left;">Response body</td>
<td style="text-align: left;"><p>{</p>
<p>    "statusCode": 0, //1: Thành công, 0: Không thành công</p>
<p>    "errorDetail": "Mô tả kết quả trả về (nếu có)",</p>
<p>    "transId": "9e2b5a94-6062-4fe7-b216-48e5b060eba5"</p>
<p>}</p></td>
</tr>
</tbody>
</table>

### III.5.2. Request body {#iii-5-2-request-body}

#### Trả kết quả mở tài khoản thanh toán (dịch vụ create-account)

<table style="width:100%;">
<colgroup>
<col style="width: 7%" />
<col style="width: 20%" />
<col style="width: 14%" />
<col style="width: 10%" />
<col style="width: 47%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>STT</strong></th>
<th style="text-align: center;"><strong>Tên trường</strong></th>
<th style="text-align: center;"><strong>Kiểu dữ liệu</strong></th>
<th style="text-align: center;"><strong>Bắt buộc</strong></th>
<th style="text-align: center;"><strong>Mô tả</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: center;"></td>
<td colspan="4"><p>Sample:</p>
<p>{</p>
<p>    "requestStatus": "SUCCESS",</p>
<p>    "transactionId": "Mã giao dịch mà TCDN nhận được khi khởi tạo giao dịch",</p>
<p>    "citizenPid": "001100234576",</p>
<p>    "accountNumber": "Nguyễn Văn A",</p>
<p>    "bankId": "01358001",</p>
<p>    "alias": "0123568A",</p>
<p>    "cardNumber": "01235684565"</p>
<p>}</p></td>
</tr>
<tr>
<td style="text-align: center;">1</td>
<td style="text-align: left;">requestStatus</td>
<td style="text-align: center;">string</td>
<td style="text-align: center;">x</td>
<td><p>Mã kết quả xử lý yêu cầu của công dân do TCDN trả về Nền tảng</p>
<p>SUCCESS: Thành công</p>
<p>FAILURE: Thất bại</p></td>
</tr>
<tr>
<td style="text-align: center;">2</td>
<td style="text-align: left;">transactionId</td>
<td style="text-align: center;">string</td>
<td style="text-align: center;">x</td>
<td>Mã giao dịch mà TCDN nhận được khi khởi tạo giao dịch.</td>
</tr>
<tr>
<td style="text-align: center;">3</td>
<td style="text-align: left;">citizenPid</td>
<td style="text-align: center;">number</td>
<td style="text-align: center;">x</td>
<td>Mã định danh của công dân</td>
</tr>
<tr>
<td style="text-align: center;">4</td>
<td style="text-align: left;">accountNumber</td>
<td style="text-align: center;">string</td>
<td style="text-align: center;"></td>
<td>Số tài khoản đã mở thành công</td>
</tr>
<tr>
<td style="text-align: center;">5</td>
<td style="text-align: left;">bankId</td>
<td style="text-align: center;">string</td>
<td style="text-align: center;"></td>
<td>Mã CITAD của Ngân hàng</td>
</tr>
<tr>
<td style="text-align: center;">6</td>
<td style="text-align: left;">alias</td>
<td style="text-align: center;">string</td>
<td style="text-align: center;"></td>
<td>Bí danh của tài khoản</td>
</tr>
<tr>
<td style="text-align: center;">7</td>
<td style="text-align: left;">cardNumber</td>
<td style="text-align: center;">string</td>
<td style="text-align: center;"></td>
<td>Số thẻ ngân hàng đã mở thành công</td>
</tr>
</tbody>
</table>

#### Trả kết quả mở thẻ tín dụng (dịch vụ create-credit-card)

<table>
<colgroup>
<col style="width: 7%" />
<col style="width: 17%" />
<col style="width: 14%" />
<col style="width: 10%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>STT</strong></th>
<th style="text-align: center;"><strong>Tên trường</strong></th>
<th style="text-align: center;"><strong>Kiểu dữ liệu</strong></th>
<th style="text-align: center;"><strong>Bắt buộc</strong></th>
<th style="text-align: center;"><strong>Mô tả</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: center;"></td>
<td colspan="4"><p>Sample:</p>
<p>{</p>
<p>    "requestStatus": "SUCCESS",</p>
<p>    "transactionId": "Mã giao dịch mà TCDN nhận được khi khởi tạo giao dịch",</p>
<p>    "citizenPid": "001100234576",</p>
<p>    "cardType": "Loại thẻ",</p>
<p>    "cardLimit": "Hạn mức",</p>
<p>}</p></td>
</tr>
<tr>
<td style="text-align: center;">1</td>
<td style="text-align: left;">requestStatus</td>
<td style="text-align: center;">string</td>
<td style="text-align: center;">x</td>
<td><p>Mã kết quả xử lý yêu cầu của công dân do TCDN trả về Nền tảng</p>
<p>SUCCESS: Thành công</p>
<p>FAILURE: Thất bại</p></td>
</tr>
<tr>
<td style="text-align: center;">2</td>
<td style="text-align: left;">transactionId</td>
<td style="text-align: center;">string</td>
<td style="text-align: center;">x</td>
<td>Mã giao dịch mà TCDN nhận được khi khởi tạo giao dịch.</td>
</tr>
<tr>
<td style="text-align: center;">3</td>
<td style="text-align: left;">citizenPid</td>
<td style="text-align: center;">number</td>
<td style="text-align: center;">x</td>
<td>Mã định danh của công dân</td>
</tr>
<tr>
<td style="text-align: center;">4</td>
<td style="text-align: left;">cardType</td>
<td style="text-align: center;">string</td>
<td style="text-align: center;"></td>
<td>Loại thẻ tín dụng được mở</td>
</tr>
<tr>
<td style="text-align: center;">5</td>
<td style="text-align: left;">cardLimit</td>
<td style="text-align: center;">string</td>
<td style="text-align: center;"></td>
<td>Hạn mức của thẻ (ĐVT: đồng)</td>
</tr>
</tbody>
</table>

#### Trả kết quả mở ví điện tử (dịch vụ create-ewallet-account)

<table>
<colgroup>
<col style="width: 7%" />
<col style="width: 17%" />
<col style="width: 13%" />
<col style="width: 10%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>STT</strong></th>
<th style="text-align: center;"><strong>Tên trường</strong></th>
<th style="text-align: center;"><strong>Kiểu dữ liệu</strong></th>
<th style="text-align: center;"><strong>Bắt buộc</strong></th>
<th style="text-align: center;"><strong>Mô tả</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: center;"></td>
<td colspan="4"><p>Sample:</p>
<p>{</p>
<p>    "requestStatus": "SUCCESS",</p>
<p>    "transactionId": "Mã giao dịch mà TCDN nhận được khi khởi tạo giao dịch"</p>
<p>}</p></td>
</tr>
<tr>
<td style="text-align: center;">1</td>
<td style="text-align: left;">requestStatus</td>
<td style="text-align: center;">string</td>
<td style="text-align: center;">x</td>
<td><p>Mã kết quả xử lý yêu cầu của công dân do TCDN trả về Nền tảng</p>
<p>SUCCESS: Thành công</p>
<p>FAILURE: Thất bại</p></td>
</tr>
<tr>
<td style="text-align: center;">2</td>
<td style="text-align: left;">transactionId</td>
<td style="text-align: center;">string</td>
<td style="text-align: center;">x</td>
<td>Mã giao dịch mà TCDN nhận được khi khởi tạo giao dịch.</td>
</tr>
</tbody>
</table>

#### Trả kết quả đăng ký chứng thư số (dịch vụ create-smartca-account)

<table>
<colgroup>
<col style="width: 7%" />
<col style="width: 24%" />
<col style="width: 13%" />
<col style="width: 9%" />
<col style="width: 44%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>STT</strong></th>
<th style="text-align: center;"><strong>Tên trường</strong></th>
<th style="text-align: center;"><strong>Kiểu dữ liệu</strong></th>
<th style="text-align: center;"><strong>Bắt buộc</strong></th>
<th style="text-align: center;"><strong>Mô tả</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: center;"></td>
<td colspan="4"><p>Sample:</p>
<p>{</p>
<p>    "requestStatus": "SUCCESS",</p>
<p>    "transactionId": "Mã giao dịch mà TCDN nhận được khi khởi tạo giao dịch",</p>
<p>    "citizenPid": "001100234576",</p>
<p>    "feeCode": "17116",</p>
<p>    "serialNumber": "C28A39C5-3115-430F-B294-6758160E630C",</p>
<p>    "expirationDateFrom": "20-06-2022",</p>
<p>    "expirationDateTo": "20-12-2022",</p>
<p>    "status": "GOOD",</p>
<p>    "source": "VNeID"</p>
<p>}</p></td>
</tr>
<tr>
<td style="text-align: center;">1</td>
<td style="text-align: left;">requestStatus</td>
<td style="text-align: center;">string</td>
<td style="text-align: center;">x</td>
<td><p>Mã kết quả xử lý yêu cầu của công dân do TCDN trả về Nền tảng</p>
<p>SUCCESS: Thành công</p>
<p>FAILURE: Thất bại</p></td>
</tr>
<tr>
<td style="text-align: center;">2</td>
<td style="text-align: left;">transactionId</td>
<td style="text-align: center;">string</td>
<td style="text-align: center;">x</td>
<td>Mã giao dịch mà TCDN nhận được khi khởi tạo giao dịch.</td>
</tr>
<tr>
<td style="text-align: center;">3</td>
<td style="text-align: left;">citizenPid</td>
<td style="text-align: center;">number</td>
<td style="text-align: center;">x</td>
<td>Mã định danh của công dân</td>
</tr>
<tr>
<td style="text-align: center;">4</td>
<td style="text-align: left;">feeCode</td>
<td style="text-align: center;">string</td>
<td style="text-align: center;"></td>
<td>Mã gói cước SMARTCA</td>
</tr>
<tr>
<td style="text-align: center;">5</td>
<td style="text-align: left;">serialNumber</td>
<td style="text-align: center;">string</td>
<td style="text-align: center;"></td>
<td>Mã serial của chứng thư số</td>
</tr>
<tr>
<td style="text-align: center;">6</td>
<td style="text-align: left;">expirationDateFrom</td>
<td style="text-align: center;">string</td>
<td style="text-align: center;"></td>
<td>Ngày hiệu lực</td>
</tr>
<tr>
<td style="text-align: center;">7</td>
<td style="text-align: left;">expirationDateTo</td>
<td style="text-align: center;">string</td>
<td style="text-align: center;"></td>
<td>Ngày hết hạn</td>
</tr>
<tr>
<td style="text-align: center;">8</td>
<td style="text-align: left;">status</td>
<td style="text-align: center;">string</td>
<td style="text-align: center;"></td>
<td><p>Trạng thái của chứng thư số:</p>
<p>Trạng thái thông tin chứng thư số:</p>
<p>GOOD: Đang hoạt động</p>
<p>REVOKED: Đã thu hồi</p>
<p>EXPIRE: Hết hiệu lực</p>
<p>PAUSING: Đang tạm dừng</p></td>
</tr>
<tr>
<td style="text-align: center;">9</td>
<td style="text-align: left;">source</td>
<td style="text-align: center;">string</td>
<td style="text-align: center;"></td>
<td><p>Nguồn đăng ký chứng thư số do VNeID quy định. Ví dụ:</p>
<p>VNeID: đăng ký từ VNeID</p>
<p>VNPT_CA: Hệ thống nội bộ VNPT</p>
<p>KHAC_VNPT_CA: Các hệ thống khác liên kết với VNPT</p></td>
</tr>
</tbody>
</table>

#### Trả kết quả đăng ký SIM di động (dịch vụ create-sim)

<table>
<colgroup>
<col style="width: 7%" />
<col style="width: 18%" />
<col style="width: 12%" />
<col style="width: 10%" />
<col style="width: 51%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>STT</strong></th>
<th style="text-align: center;"><strong>Tên trường</strong></th>
<th style="text-align: center;"><strong>Kiểu dữ liệu</strong></th>
<th style="text-align: center;"><strong>Bắt buộc</strong></th>
<th style="text-align: center;"><strong>Mô tả</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: center;"></td>
<td colspan="4"><p>Sample:</p>
<p>{</p>
<p>    "requestStatus": "SUCCESS",</p>
<p>    "transactionId": "Mã giao dịch mà TCDN nhận được khi khởi tạo giao dịch",</p>
<p>    "orderCode": "123456789",</p>
<p>    "simType": "CONNECT_PREPAID",</p>
<p>    "phoneNumber": "0383777790"</p>
<p>}</p></td>
</tr>
<tr>
<td style="text-align: center;">1</td>
<td style="text-align: left;">requestStatus</td>
<td style="text-align: center;">string</td>
<td style="text-align: center;">x</td>
<td><p>Mã kết quả xử lý yêu cầu của công dân do TCDN trả về Nền tảng</p>
<p>SUCCESS: Thành công</p>
<p>FAILURE: Thất bại</p></td>
</tr>
<tr>
<td style="text-align: center;">2</td>
<td style="text-align: left;">transactionId</td>
<td style="text-align: center;">string</td>
<td style="text-align: center;">x</td>
<td>Mã giao dịch mà TCDN nhận được khi khởi tạo giao dịch.</td>
</tr>
<tr>
<td style="text-align: center;">3</td>
<td style="text-align: left;">orderCode</td>
<td style="text-align: center;">string</td>
<td style="text-align: center;"></td>
<td>Mã đơn hàng đăng ký sim</td>
</tr>
<tr>
<td style="text-align: center;">4</td>
<td style="text-align: left;">simType</td>
<td style="text-align: center;">string</td>
<td style="text-align: center;"></td>
<td><p>Loại thuê bao:</p>
<p>- CONNECT_PREPAID (sim trả trước)</p>
<p>- CONNECT_POSTPAID (sim trả sau)</p></td>
</tr>
<tr>
<td style="text-align: center;">5</td>
<td style="text-align: left;">phoneNumber</td>
<td style="text-align: center;">string</td>
<td style="text-align: center;"></td>
<td>Số thuê bao đăng ký thành công</td>
</tr>
</tbody>
</table>

#### Trả thông tin lịch sử hóa đơn về ứng dụng VNeID (dịch vụ billing-history)

<table>
<colgroup>
<col style="width: 7%" />
<col style="width: 18%" />
<col style="width: 11%" />
<col style="width: 10%" />
<col style="width: 51%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>STT</strong></th>
<th style="text-align: center;"><strong>Tên trường</strong></th>
<th style="text-align: center;"><strong>Kiểu dữ liệu</strong></th>
<th style="text-align: center;"><strong>Bắt buộc</strong></th>
<th style="text-align: center;"><strong>Mô tả</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: center;"></td>
<td colspan="4"><p>Sample:</p>
<p>{</p>
<p>    "citizenPid": "001096033931",</p>
<p>    "billingType": 1,</p>
<p>    "billingStatus": "SUCCESS",</p>
<p>    "amount": "10000",</p>
<p>    "description": "Mô tả",</p>
<p>    "paymentTime": "20:15:00 23/07/2024",</p>
<p>    "detail": [</p>
<p>        {</p>
<p>            "label": "Label trường muốn hiển thị",</p>
<p>            "value": "Dữ liệu tương ứng vói label muốn hiển thị",</p>
<p>            "isCopy": <strong>true</strong></p>
<p>        }</p>
<p>    ]</p>
<p>}</p></td>
</tr>
<tr>
<td style="text-align: center;">1</td>
<td style="text-align: left;">citizenPid</td>
<td style="text-align: center;">string</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Mã định danh của công dân</td>
</tr>
<tr>
<td style="text-align: center;">2</td>
<td style="text-align: left;">billingType</td>
<td style="text-align: center;">String (5)</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Loại hóa đơn</td>
</tr>
<tr>
<td style="text-align: center;">3</td>
<td style="text-align: left;">billingStatus</td>
<td style="text-align: center;">String (50)</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Trạng thái hóa đơn</td>
</tr>
<tr>
<td style="text-align: center;">4</td>
<td style="text-align: left;">amount</td>
<td style="text-align: center;">String (50)</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;"><p>Tổng số tiền (ĐVT: đồng)</p>
<p>Chỉ chấp nhận giá trị số.</p></td>
</tr>
<tr>
<td style="text-align: center;">5</td>
<td style="text-align: left;">description</td>
<td style="text-align: center;">String (200)</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Mô tả nội dung thanh toán.</td>
</tr>
<tr>
<td style="text-align: center;">6</td>
<td style="text-align: left;">paymentTime</td>
<td style="text-align: center;">string</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;"><p>Thời gian thanh toán</p>
<p>Định dạng hh:mm:ss dd-MM-yyyy</p></td>
</tr>
<tr>
<td style="text-align: center;">7</td>
<td style="text-align: left;">detail</td>
<td style="text-align: center;">list object</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Danh sách thông tin chi tiết muốn hiển thị (tối đa 10 phần tử)</td>
</tr>
<tr>
<td style="text-align: center;">8</td>
<td style="text-align: left;">label</td>
<td style="text-align: center;">String (100)</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Tiêu đề thông tin</td>
</tr>
<tr>
<td style="text-align: center;">9</td>
<td style="text-align: left;">value</td>
<td style="text-align: center;">String (50)</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Giá trị gắn với tiêu đề</td>
</tr>
<tr>
<td style="text-align: center;">10</td>
<td style="text-align: left;">isCopy</td>
<td style="text-align: center;">boolean</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;"><p>Thiết lập cho phép sao chép/ không giao chép giá trị trên ứng dụng VNeID:</p>
<p>true: cho phép sao chép</p>
<p>false: không cho sao chép</p></td>
</tr>
</tbody>
</table>

#### Trả thông tin lịch sử mua thuốc (dịch vụ buy-medicine-history)

<table>
<colgroup>
<col style="width: 7%" />
<col style="width: 24%" />
<col style="width: 12%" />
<col style="width: 9%" />
<col style="width: 46%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>STT</strong></th>
<th style="text-align: center;"><strong>Tên trường</strong></th>
<th style="text-align: center;"><strong>Kiểu dữ liệu</strong></th>
<th style="text-align: center;"><strong>Bắt buộc</strong></th>
<th style="text-align: center;"><strong>Mô tả</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: center;"></td>
<td colspan="4"><p>Sample:</p>
<p>{</p>
<p>    "citizenPid": "Số định danh cá nhân",</p>
<p>    "orderCode": "80046016941729677443644",</p>
<p>    "orderDate": "15:58:00 23/08/2024",</p>
<p>    "shop": {</p>
<p>        "shopName": "LC HCM 224A Lê Văn Lương",</p>
<p>        "shopNameDisplay": "Nhà thuốc Long Châu 224A Lê Văn Lương, P. Tân Hưng, Q. 7, TP. Hồ Chí Minh",</p>
<p>        "shopAddress": "224A Lê Văn Lương, P. Tân Hưng, Q. 7, TP. Hồ Chí Minh"</p>
<p>    },</p>
<p>    "items": [</p>
<p>        {</p>
<p>            "itemCode": "00000694",</p>
<p>            "itemName": "Nước tắm trẻ em Amibebe Herbe hỗ trợ điều trị rôm sẩy, mụn nhọt, lở ngứa (250ml)",</p>
<p>            "quantity": 1,</p>
<p>            "unitName": "Chai",</p>
<p>            "price": "95700",</p>
<p>            "priceAfterDiscount": "61829",</p>
<p>            "finalPrice": "21312"</p>
<p>        },</p>
<p>        {</p>
<p>            "itemCode": "00502343",</p>
<p>            "itemName": "Nước hồng sâm mật ong Kwangdong hỗ trợ tăng cường sức đề kháng (10 chai x 170ml) HT",</p>
<p>            "quantity": 2,</p>
<p>            "unitName": "Hộp",</p>
<p>            "price": "95700",</p>
<p>            "priceAfterDiscount": "61829",</p>
<p>            "finalPrice": "21312"</p>
<p>        }</p>
<p>    ],</p>
<p>    "pricing": {</p>
<p>        "totalPrice": "95700",</p>
<p>        "discount": "95700",</p>
<p>        "voucherDiscount": "95700",</p>
<p>        "deliveryFee": "0",</p>
<p>        "finalPrice": "95700"</p>
<p>    }</p>
<p>}</p></td>
</tr>
<tr>
<td style="text-align: center;">1</td>
<td style="text-align: left;">citizenPid</td>
<td style="text-align: center;">number (12)</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Mã định danh công dân</td>
</tr>
<tr>
<td style="text-align: center;">2</td>
<td style="text-align: left;">orderCode</td>
<td style="text-align: center;">string (50)</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Mã đơn hàng</td>
</tr>
<tr>
<td style="text-align: center;">3</td>
<td style="text-align: left;">orderDate</td>
<td style="text-align: center;">string</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;"><p>Thời gian thanh toán</p>
<p>Định dạng hh:mm:ss dd-MM-yyyy</p></td>
</tr>
<tr>
<td style="text-align: center;">4</td>
<td style="text-align: left;">shop</td>
<td style="text-align: center;">object</td>
<td style="text-align: center;"></td>
<td style="text-align: left;"></td>
</tr>
<tr>
<td style="text-align: center;">5</td>
<td style="text-align: left;">shopName</td>
<td style="text-align: center;">string</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Tên cửa hàng</td>
</tr>
<tr>
<td style="text-align: center;">6</td>
<td style="text-align: left;">shopNameDisplay</td>
<td style="text-align: center;">string</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Tên hiển thị của cửa hàng</td>
</tr>
<tr>
<td style="text-align: center;">7</td>
<td style="text-align: left;">shopAddress</td>
<td style="text-align: center;">string</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Địa chỉ cửa hàng</td>
</tr>
<tr>
<td style="text-align: center;">8</td>
<td style="text-align: left;">items</td>
<td style="text-align: center;">object</td>
<td style="text-align: center;"></td>
<td style="text-align: left;"></td>
</tr>
<tr>
<td style="text-align: center;">8.1</td>
<td style="text-align: left;">itemCode</td>
<td style="text-align: center;">string</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Mã thuốc</td>
</tr>
<tr>
<td style="text-align: center;">8.2</td>
<td style="text-align: left;">itemName</td>
<td style="text-align: center;">string</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Tên thuốc</td>
</tr>
<tr>
<td style="text-align: center;">8.3</td>
<td style="text-align: left;">Quantity</td>
<td style="text-align: center;">string</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Số lượng</td>
</tr>
<tr>
<td style="text-align: center;">8.4</td>
<td style="text-align: left;">unitName</td>
<td style="text-align: center;">string</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Đơn vị tính</td>
</tr>
<tr>
<td style="text-align: center;">8.5</td>
<td style="text-align: left;">Price</td>
<td style="text-align: center;">string</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Giá tiền</td>
</tr>
<tr>
<td style="text-align: center;">8.6</td>
<td style="text-align: left;">priceAfterDiscount</td>
<td style="text-align: center;">string</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Giá tiền sau giảm giá</td>
</tr>
<tr>
<td style="text-align: center;">8.7</td>
<td style="text-align: left;">finalPrice</td>
<td style="text-align: center;">string</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Thành tiền</td>
</tr>
<tr>
<td style="text-align: center;">9</td>
<td style="text-align: left;">pricing</td>
<td style="text-align: center;">object</td>
<td style="text-align: center;"></td>
<td style="text-align: left;"></td>
</tr>
<tr>
<td style="text-align: center;">9.1</td>
<td style="text-align: left;">totalPrice</td>
<td style="text-align: center;">string</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Tổng tiền</td>
</tr>
<tr>
<td style="text-align: center;">9.2</td>
<td style="text-align: left;">discount</td>
<td style="text-align: center;">string</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Giảm giá</td>
</tr>
<tr>
<td style="text-align: center;">9.3</td>
<td style="text-align: left;">voucherDiscount</td>
<td style="text-align: center;">string</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Mã giảm giá</td>
</tr>
<tr>
<td style="text-align: center;">9.4</td>
<td style="text-align: left;">deliveryFee</td>
<td style="text-align: center;">string</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Phí vận chuyển</td>
</tr>
<tr>
<td style="text-align: center;">9.5</td>
<td style="text-align: left;">finalPrice</td>
<td style="text-align: center;">string</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Tổng tiền cuối cùng</td>
</tr>
</tbody>
</table>

#### Trả kết quả thông tin bảo hiểm (dịch vụ register-insurance)

<table>
<colgroup>
<col style="width: 7%" />
<col style="width: 24%" />
<col style="width: 12%" />
<col style="width: 9%" />
<col style="width: 45%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>STT</strong></th>
<th style="text-align: center;"><strong>Tên trường</strong></th>
<th style="text-align: center;"><strong>Kiểu dữ liệu</strong></th>
<th style="text-align: center;"><strong>Bắt buộc</strong></th>
<th style="text-align: center;"><strong>Mô tả</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: center;"></td>
<td colspan="4"><p>Sample:</p>
<p>{</p>
<p>    "requestStatus": "SUCCESS",</p>
<p>    "transactionCode": "NAPAS-4b266127-075c-4f42-98d6-fe409b8b8ea4",</p>
<p>    "code": "Mã code định danh hợp đồng bảo hiểm (BH)",</p>
<p>    "productCode": "Mã sản phẩm bảo hiểm",</p>
<p>    "productName": "Tên sản phẩm bảo hiểm",</p>
<p>    "contractFee": "Phí bảo hiểm",</p>
<p>    "effectiveDate": "Ngày bắt đầu hiệu lực BH. Định dạng dd/MM/yyyy",</p>
<p>    "expirationDate": "Ngày hết hạn BH. Định dạng dd/MM/yyyy",</p>
<p>    "certificationPath": "Link file GCN BH"</p>
<p>}</p></td>
</tr>
<tr>
<td style="text-align: center;">1</td>
<td style="text-align: left;">transactionCode</td>
<td style="text-align: center;">string</td>
<td style="text-align: center;">x</td>
<td>Mã giao dịch gắn với API gửi yêu cầu</td>
</tr>
<tr>
<td style="text-align: center;">2</td>
<td style="text-align: left;">requestStatus</td>
<td style="text-align: center;">string</td>
<td style="text-align: center;">x</td>
<td><p>Trạng thái yêu cầu trả về</p>
<p>- SHARED: Đã lấy dữ liệu</p>
<p>- SUCCESS: Thành công</p>
<p>- FAILURE: Thất bại</p></td>
</tr>
<tr>
<td style="text-align: center;">3</td>
<td style="text-align: left;">reasonRequestCode</td>
<td style="text-align: center;">number</td>
<td style="text-align: center;">x</td>
<td>Mã lý do từ chối thống nhất giữa VNeID và Tổ chức doanh nghiệp</td>
</tr>
<tr>
<td style="text-align: center;">4</td>
<td style="text-align: left;">reasonRequestText</td>
<td style="text-align: center;">string</td>
<td style="text-align: center;"></td>
<td>Nội dung lỗi do Tổ chức doanh nghiệp trả về nếu chưa có quy định về mã lý do</td>
</tr>
<tr>
<td style="text-align: center;">5</td>
<td style="text-align: left;">code</td>
<td style="text-align: center;">string</td>
<td style="text-align: center;">x</td>
<td>Mã code định danh hợp đồng bảo hiểm (BH)</td>
</tr>
<tr>
<td style="text-align: center;">6</td>
<td style="text-align: left;">productCode</td>
<td style="text-align: center;">string</td>
<td style="text-align: center;">x</td>
<td>Mã sản phẩm bảo hiểm</td>
</tr>
<tr>
<td style="text-align: center;">7</td>
<td style="text-align: left;">productName</td>
<td style="text-align: center;">string</td>
<td style="text-align: center;">x</td>
<td>Tên sản phẩm bảo hiểm</td>
</tr>
<tr>
<td style="text-align: center;">8</td>
<td style="text-align: left;">contractFee</td>
<td style="text-align: center;">string</td>
<td style="text-align: center;"></td>
<td>Phí bảo hiểm</td>
</tr>
<tr>
<td style="text-align: center;">9</td>
<td style="text-align: left;">effectiveDate</td>
<td style="text-align: center;">string</td>
<td style="text-align: center;">x</td>
<td>Ngày bắt đầu hiệu lực BH. Định dạng dd/MM/yyyy. Nếu không có data thì bắt buộc phải truyền null</td>
</tr>
<tr>
<td style="text-align: center;">10</td>
<td style="text-align: left;">expirationDate</td>
<td style="text-align: center;">String</td>
<td style="text-align: center;">x</td>
<td>Ngày hết hạn BH. Định dạng dd/MM/yyyy. Nếu không có data thì bắt buộc phải truyền null</td>
</tr>
<tr>
<td style="text-align: center;">11</td>
<td style="text-align: left;">certificationPath</td>
<td style="text-align: center;">String</td>
<td style="text-align: center;">x</td>
<td>Link file GCN BH</td>
</tr>
</tbody>
</table>

#### Trả kết quả đồng bộ vé metro (dịch vụ sync-ticket)

<table>
<colgroup>
<col style="width: 16%" />
<col style="width: 24%" />
<col style="width: 24%" />
<col style="width: 19%" />
<col style="width: 14%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>STT</strong></th>
<th style="text-align: center;"><strong>Tên trường</strong></th>
<th style="text-align: center;"><strong>Kiểu dữ liệu</strong></th>
<th style="text-align: center;"><strong>Bắt buộc</strong></th>
<th style="text-align: center;"><strong>Mô tả</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: center;">Request body</td>
<td colspan="4"><p>{</p>
<p>    "data": [ // Danh sách vé metro đồng bộ</p>
<p>        {</p>
<p>            "citizenNumber": "001200004202", // Số định danh công dân gắn với vé</p>
<p>            "fullName": "NGUYỄN VĂN A", // Họ và tên công dân gắn với vé</p>
<p>            "ticketCode": "Abc@123", // Mã vé</p>
<p>            "routeCode": "BCD#123", // Mã tuyến</p>
<p>            "routeName": "Tuyến Nhổn - Hà Nội", // Tên tuyến</p>
<p>            "effectivePeriod": "1 tháng", // Thời gian vé có hiệu lực</p>
<p>            "effectiveFrom": "01/01/2025", // Ngày bắt đầu hiệu lực. Bắt buộc chuỗi định dạng dd/MM/yyyy</p>
<p>            "effectiveTo": "31/01/2025" // Ngày kết thúc hiệu lực. Bắt buộc chuỗi định dạng dd/MM/yyyy</p>
<p>        }</p>
<p>    ]</p>
<p>}</p></td>
</tr>
<tr>
<td style="text-align: center;">Response body</td>
<td colspan="4"><p>Status Code: 200</p>
<p>Thành công:</p>
<p>{</p>
<p>    "statusCode": 1, // Trạng thái xử lý yêu cầu. 1 (Thành công) - 0 (Thất bại)</p>
<p>    "data": { // Dữ liệu kết quả xử lý yêu cầu</p>
<p>        "success": [ // Danh sách các vé đồng bộ thành công</p>
<p>            {</p>
<p>                "ticketCode": "Abc@123" // Mã vé</p>
<p>            }</p>
<p>        ],</p>
<p>        "failure": [ // Danh sách các vé đồng bộ thất bại</p>
<p>            {</p>
<p>                "ticketCode": "Abc@123", // Mã vé</p>
<p>                "errorDetail": "Công dân chưa có tài khoản VNeID" // Thông tin lỗi nếu có</p>
<p>            }</p>
<p>        ]</p>
<p>    }</p>
<p>}</p>
<p>Không thành công:</p>
<p>{</p>
<p>    "statusCode": 0, // Trạng thái xử lý yêu cầu. 1 (Thành công) - 0 (Thất bại)</p>
<p>    "errorDetail": "Lỗi yêu cầu" // Thông báo lỗi nếu có</p>
<p>}</p></td>
</tr>
</tbody>
</table>

- **RequestBody:**

<table>
<colgroup>
<col style="width: 8%" />
<col style="width: 21%" />
<col style="width: 16%" />
<col style="width: 9%" />
<col style="width: 43%" />
</colgroup>
<tbody>
<tr>
<td style="text-align: center;"><strong>STT</strong></td>
<td style="text-align: center;"><strong>Tên trường</strong></td>
<td style="text-align: center;"><p><strong>Kiểu</strong></p>
<p><strong>dữ liệu</strong></p></td>
<td style="text-align: center;"><strong>Bắt buộc</strong></td>
<td style="text-align: center;"><strong>Mô tả</strong></td>
</tr>
<tr>
<td style="text-align: center;">1</td>
<td style="text-align: center;">data</td>
<td style="text-align: center;"><p>List</p>
<p>&lt;Object&gt;</p></td>
<td style="text-align: center;">X</td>
<td style="text-align: center;">Danh sách vé metro đồng bộ</td>
</tr>
<tr>
<td style="text-align: center;">1.1</td>
<td style="text-align: center;">citizenNumber</td>
<td style="text-align: center;">String</td>
<td style="text-align: center;">X</td>
<td style="text-align: center;"><p>Số định danh công dân gắn với vé</p>
<p>Bắt buộc chuỗi số 12 ký tự</p></td>
</tr>
<tr>
<td style="text-align: center;">1.2</td>
<td style="text-align: center;">fullName</td>
<td style="text-align: center;">String</td>
<td style="text-align: center;">X</td>
<td style="text-align: center;"><p>Họ và tên công dân gắn với vé</p>
<p>Bắt buộc chuỗi viết hoa không chứa số, ký tự đặc biệt</p></td>
</tr>
<tr>
<td style="text-align: center;">1.3</td>
<td style="text-align: center;">ticketCode</td>
<td style="text-align: center;">String</td>
<td style="text-align: center;">X</td>
<td style="text-align: center;">Mã vé</td>
</tr>
<tr>
<td style="text-align: center;">1.4</td>
<td style="text-align: center;">routeCode</td>
<td style="text-align: center;">String</td>
<td style="text-align: center;">X</td>
<td style="text-align: center;">Mã tuyến</td>
</tr>
<tr>
<td style="text-align: center;">1.5</td>
<td style="text-align: center;">routeName</td>
<td style="text-align: center;">String</td>
<td style="text-align: center;">X</td>
<td style="text-align: center;">Tên tuyến</td>
</tr>
<tr>
<td style="text-align: center;">1.6</td>
<td style="text-align: center;">effectivePeriod</td>
<td style="text-align: center;">String</td>
<td style="text-align: center;">X</td>
<td style="text-align: center;">Thời gian vé có hiệu lực</td>
</tr>
<tr>
<td style="text-align: center;">1.7</td>
<td style="text-align: center;">effectiveFrom</td>
<td style="text-align: center;">String</td>
<td style="text-align: center;">X</td>
<td style="text-align: center;"><p>Ngày bắt đầu hiệu lực.</p>
<p>Bắt buộc chuỗi định dạng dd/MM/yyyy</p></td>
</tr>
<tr>
<td style="text-align: center;">1.8</td>
<td style="text-align: center;">effectiveTo</td>
<td style="text-align: center;">String</td>
<td style="text-align: center;">X</td>
<td style="text-align: center;"><p>Ngày kết thúc hiệu lực.</p>
<p>Bắt buộc chuỗi định dạng dd/MM/yyyy</p></td>
</tr>
</tbody>
</table>

- **Response Body:**

<table>
<colgroup>
<col style="width: 10%" />
<col style="width: 18%" />
<col style="width: 10%" />
<col style="width: 15%" />
<col style="width: 45%" />
</colgroup>
<tbody>
<tr>
<td style="text-align: center;"><strong>STT</strong></td>
<td style="text-align: center;"><strong>Tên trường</strong></td>
<td style="text-align: center;"><strong>Kiểu dữ liệu</strong></td>
<td style="text-align: center;"><strong>Bắt buộc</strong></td>
<td style="text-align: center;"><strong>Mô tả</strong></td>
</tr>
<tr>
<td style="text-align: center;">1</td>
<td style="text-align: center;">statusCode</td>
<td style="text-align: center;">String</td>
<td style="text-align: center;">x</td>
<td style="text-align: center;"><p>Trạng thái xử lý yêu cầu:</p>
<p>- Thành công (1)</p>
<p>- Thất bại (0)</p></td>
</tr>
<tr>
<td style="text-align: center;">2</td>
<td style="text-align: center;">errorDetail</td>
<td style="text-align: center;">String</td>
<td style="text-align: center;"></td>
<td style="text-align: center;">Thông báo lỗi nếu có</td>
</tr>
<tr>
<td style="text-align: center;">3</td>
<td style="text-align: center;">data</td>
<td style="text-align: center;">List</td>
<td style="text-align: center;">X</td>
<td style="text-align: center;">Dữ liệu kết quả xử lý yêu cầu</td>
</tr>
<tr>
<td style="text-align: center;">3.1</td>
<td style="text-align: center;">success</td>
<td style="text-align: center;">String</td>
<td style="text-align: center;">X</td>
<td style="text-align: center;">Danh sách các vé đồng bộ thành công</td>
</tr>
<tr>
<td style="text-align: center;">3.1.1</td>
<td style="text-align: center;">ticketCode</td>
<td style="text-align: center;">String</td>
<td style="text-align: center;">X</td>
<td style="text-align: center;">Mã vé</td>
</tr>
<tr>
<td style="text-align: center;">3.2</td>
<td style="text-align: center;">failure</td>
<td style="text-align: center;">String</td>
<td style="text-align: center;">X</td>
<td style="text-align: center;">Danh sách các vé đồng bộ thất bại</td>
</tr>
<tr>
<td style="text-align: center;">3.2.1</td>
<td style="text-align: center;">ticketCode</td>
<td style="text-align: center;">String</td>
<td style="text-align: center;">X</td>
<td style="text-align: center;">Mã vé</td>
</tr>
<tr>
<td style="text-align: center;">3.2.2</td>
<td style="text-align: center;">errorDetail</td>
<td style="text-align: center;">String</td>
<td style="text-align: center;"></td>
<td style="text-align: center;">Thông tin lỗi nếu có</td>
</tr>
</tbody>
</table>

#### Trả kết quả đồng bộ vé máy bay (dịch vụ checkin-onl-face-result)

<table>
<colgroup>
<col style="width: 2%" />
<col style="width: 97%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"></th>
<th><p>Sample:</p>
<p>{</p>
<p>"transactionCode": "HVN-efb255ed-7d54-46fc-826d-7b4a8d2d262f", // Mã giao dịch gắn với API gửi yêu cầu</p>
<p>"requestStatus": "SUCCESS", // Trạng thái Checkin</p>
<p>"checkInCitizenNumber": "001200004611", // Số định danh công dân mua vé</p>
<p>"checkIncitizenName": "Hoàng Quốc A", // Họ và tên công dân mua vé</p>
<p>"boardingPasses": [ // Danh sách vé</p>
<p>{</p>
<p>"status": "CHECKIN-FACE", //Trạng thái: CHECKIN-FACE : check in có xác thực sinh trắc học, CHECKIN: check in không xác thực sinh trắc học</p>
<p>"citizenNumber": "001200004611", // Số định danh công dân gắn với lần check in</p>
<p>"citizenName": "Hoàng Quốc A", // Họ và tên công dân gắn với lần check in</p>
<p>"flightNumber": "VJ198", // Mã chuyến bay</p>
<p>"departureDate": "29/04/2025 22:15:00", // Ngày giờ khởi hành chuyến bay (ISO) - Trường này và flightNumber đảm bảo unique cho 1 chuyến bay dd/MM/yyyy HH:mm:ss</p>
<p>"seat": "13A", // Số ghế ngồi (nếu có thì truyền, không có truyền null)</p>
<p>"gate": "Của bay", //Cửa bay (nếu có thì truyền, không có truyền null)</p>
<p>"barcode": "M1D/D E3987S3 SGNHANVJ 0196 95 022D0001 100", // Mã Boarding Pass của KH</p>
<p>"reservationLocator": "92GWRY", // Mã booking của hành khách</p>
<p>"cityFrom": { // Thông tin thành phố nơi đi</p>
<p>"code": "SGN", // Mã thành phố trong danh mục của TCDN</p>
<p>"name": "Sài Gòn", // Tên thành phố</p>
<p>"nationalCode": "VN", // Mã quốc gia trong danh mục của TCDN</p>
<p>"nationalName": "Việt Nam" // Tên quốc gia</p>
<p>},</p>
<p>"cityTo": { // Thông tin thành phố nơi đến</p>
<p>"code": "HAN", // Mã thành phố trong danh mục của TCDN</p>
<p>"name": "Hà Nội", // Tên thành phố</p>
<p>"nationalCode": "VN", // Mã quốc gia trong danh mục của TCDN</p>
<p>"nationalName": "Việt Nam" // Tên quốc gia</p>
<p>},</p>
<p>"totalAmount": "100000", // Tổng số tiền vé (đơn vị việt nam đồng)</p>
<p>"note": "Ghi chú" //(nếu có thì truyền, không có truyền null)</p>
<p>}</p>
<p>]</p>
<p>}</p></th>
</tr>
</thead>
<tbody>
</tbody>
</table>

### III.5.3. Response body {#iii-5-3-response-body}

<table style="width:98%;">
<colgroup>
<col style="width: 7%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 10%" />
<col style="width: 47%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>STT</strong></th>
<th style="text-align: center;"><strong>Tên trường</strong></th>
<th style="text-align: center;"><strong>Kiểu dữ liệu</strong></th>
<th style="text-align: center;"><strong>Bắt buộc</strong></th>
<th style="text-align: center;"><strong>Ghi chú</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: center;">1</td>
<td>statusCode</td>
<td><p>String</p>
<p>(2 ký tự)</p></td>
<td style="text-align: center;">x</td>
<td style="text-align: left;"><p>Mã trạng thái tiếp nhận:</p>
<p>1: Thành công</p>
<p>0: Không thành công</p></td>
</tr>
<tr>
<td style="text-align: center;">2</td>
<td>errorDetail</td>
<td><p>String</p>
<p>(100 ký tự)</p></td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Mô tả chi tiết lỗi trả về trong trường hợp statusCode = 0</td>
</tr>
<tr>
<td style="text-align: center;">3</td>
<td>transId</td>
<td><p>String</p>
<p>(36 ký tự)</p></td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Mã giao dịch mà ứng dụng doanh nghiệp đang trả kết quả ở request body</td>
</tr>
</tbody>
</table>

## III.6. Dịch vụ xác nhận chia sẻ thông tin định danh tổ chức, xác thực thông tin định danh tổ chức {#iii-6-dich-vu-xac-nhan-dinh-danh-to-chuc}

### III.6.1. API gửi yêu cầu chia sẻ thông tin định danh tổ chức, xác thực thông tin định danh tổ chức {#iii-6-1-api-gui-yeu-cau-dinh-danh-to-chuc}

\* **Tổ chức doanh nghiệp cần cung cấp** đường link dẫn tới trang công bố **“Điều khoản và cam kết sử dụng dữ liệu được chia sẻ”** của Tổ chức doanh nghiệp, nhằm hiển thị trên ứng dụng VNeID.

#### API Specs

<table>
<colgroup>
<col style="width: 16%" />
<col style="width: 83%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>Thông tin</strong></th>
<th style="text-align: center;"><strong>Mô tả</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td>Mục đích</td>
<td>API sử dụng để AgentGW chuyển tiếp yêu cầu xác nhận chia sẻ thông tin định danh tổ chức từ TCDN đến NT.</td>
</tr>
<tr>
<td>URL</td>
<td><p>https://&lt;domain&gt;:&lt;port&gt;/api/tcdn/<strong>{service-code}/</strong>api/agent/init</p>
<p><strong>{service-code}</strong> tham chiếu theo bảng Mã dịch vụ IV.3.</p></td>
</tr>
<tr>
<td>Method</td>
<td>POST</td>
</tr>
<tr>
<td>Header</td>
<td><ul>
<li><p><strong>Content-Type:</strong> application/json; charset=utf-8</p></li>
<li><p><strong>api_key:</strong> API key do tổ chức cung cấp dịch vụ cung cấp</p></li>
<li><p><mark><strong>target_code:</strong> Mã mục đích chia sẻ, xử lý thông tin do Tổ chức cung cấp dịch vụ cung cấp, phục vụ hiển thị đúng nội dung mục đích chia sẻ, xử lý thông tin trên ứng dụng VNeID.</mark></p></li>
<li><p><strong>request_id:</strong> Mã yêu cầu do hệ thống của TCDN tự sinh, là duy nhất trên hệ thống.</p></li>
<li><p><strong>request_time:</strong> Thời gian yêu cầu, định dạng ddMMyyyyHHmmss. Là thời gian hiện tại của hệ thống, trong trường hợp request_time gửi sang Hệ thống VNeID lệch quá 2 phút so với thời gian hiện tại của Hệ thống VNeID thì request đó được coi là timeout.</p></li>
</ul></td>
</tr>
<tr>
<td style="text-align: left;">Params</td>
<td style="text-align: left;"><p>Trên môi trường non-production:</p>
<p>code=mã đơn vị kết nối (do Đơn vị cung cấp dịch vụ cấp)</p>
<p>Trên môi trường prod: N/A</p></td>
</tr>
<tr>
<td>Request body</td>
<td style="text-align: left;"><p>Dịch vụ chia sẻ thông tin định danh tổ chức</p>
<p>{</p>
<p>    "fullName": <strong>null</strong>,</p>
<p>    "birthDate": <strong>null</strong>,</p>
<p>    "citizenPid": "",</p>
<p>    "type": "ORG", //Loại consent, type = Per hoặc null thì là consent cá nhâm , type = ORG sẽ là consent chia sẻ thông tin tổ chức</p>
<p>    "shareOrgNumber": "Số định danh tổ chức cần lấy dữ liệu", //Số định danh tổ chức cần lấy dữ liệu, bắt buộc khi type = ORG</p>
<p>}</p>
<p>Dịch vụ xác thực thông tin định danh tổ chức</p>
<p>{</p>
<p>    "fullName": "Nguyễn Thị Huế",</p>
<p>    "birthDate": "24-04-1988",</p>
<p>    "citizenPid": "036188001958",</p>
<p>//Danh sách các trường cần xác thực.</p>
<p>    "type": "ORG", //Loại consent, type = Per hoặc null thì là consent cá nhâm , type = ORG sẽ là consent chia sẻ thông tin tổ chức</p>
<p>    "shareOrgNumber": "Số định danh tổ chức cần lấy dữ liệu", //Số định danh tổ chức cần lấy dữ liệu, bắt buộc khi type = ORG</p>
<p>}</p></td>
</tr>
<tr>
<td>Respond body</td>
<td style="text-align: left;"><p>Thành công</p>
<p>{</p>
<p>    "responseCode": 0, //0: thành công, Khác 0: không thành công</p>
<p>    "description": "Thành công",</p>
<p>    "responseTime": "30092024190321", //ddMMyyyyHHmmss</p>
<p>    "data": {</p>
<p>        "txnId": "22fc12e0-be90-4519-8ef6-cdeca95ae22b",</p>
<p>        "requestId": "3d60591a-315c-457e-b926-1b3ec699e0a9"</p>
<p>    }</p>
<p>}</p>
<p>Không thành công</p>
<p>{</p>
<p>    "responseCode": 1, //0: thành công, Khác 0: không thành công</p>
<p>    "description": "Mô tả lỗi",</p>
<p>    "responseTime": "30092024190321", //ddMMyyyyHHmmss</p>
<p>    "data": ""</p>
<p>}</p></td>
</tr>
</tbody>
</table>

#### Request Body

**\* Các thông tin định danh tổ chức** có thể chia sẻ thông qua các dịch vụ chia sẻ thông tin bao gồm: Tên doanh nghiệp (tên tiếng việt, tên tiếng anh, tên viết tắt), ngành nghề đăng ký kinh doanh, thông tin người đại diện pháp luật,…

**\* Đối với dịch vụ xác thực thông tin định danh tổ chức:** Các trường dữ liệu được TCDN truyền lên sẽ nhận về kết quả Đúng/Sai (True/False) so với dữ liệu trong CSDL định danh tổ chức.

#### Response Body

<table>
<colgroup>
<col style="width: 6%" />
<col style="width: 18%" />
<col style="width: 19%" />
<col style="width: 11%" />
<col style="width: 44%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>TT</strong></th>
<th style="text-align: center;"><strong>Tên trường</strong></th>
<th style="text-align: center;"><strong>Định dạng</strong></th>
<th style="text-align: center;"><strong>Bắt buộc</strong></th>
<th style="text-align: center;"><strong>Mô tả</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: center;">1</td>
<td>responseCode</td>
<td><p>String</p>
<p>(2 ký tự)</p></td>
<td style="text-align: center;">x</td>
<td style="text-align: left;"><p>Mã trạng thái tiếp nhận:</p>
<p>0: thành công</p>
<p>Khác 0: Lỗi (mô tả chi tiết tại trường description)</p>
<p><em>Tham chiếu mục IV.4, cụ thể:</em></p>
<p>-2: Giao dịch đã tồn tại</p>
<p>-5: Đầu vào yêu cầu không hợp lệ!</p>
<p>-6: Không tìm thấy tổ chức</p>
<p>-7: Không tìm thấy dịch vụ</p>
<p>-8: Đường dẫn không hợp lệ</p>
<p>-12: Không tìm thấy cấu hình agent</p>
<p>-14: Timeout</p>
<p>-16: Dịch vụ đang sử dụng vượt quá hạn mức cho phép</p>
<p>-18: Trường đầu vào không tồn tại trong cấu hình dịch vụ</p>
<p>-19: Tài khoản tổ chức không có quyền sử dụng dịch vụ</p>
<p>-20: Thông tin định danh tổ chức không đúng</p></td>
</tr>
<tr>
<td style="text-align: center;">2</td>
<td>description</td>
<td><p>String</p>
<p>(100 ký tự)</p></td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Mô tả lỗi trường hợp trạng thái tiếp nhận khác 0</td>
</tr>
<tr>
<td style="text-align: center;">3</td>
<td>responseTime</td>
<td><p>String</p>
<p>(100 ký tự)</p></td>
<td style="text-align: center;">x</td>
<td style="text-align: left;"><p>Thời gian phản hồi kết quả</p>
<p>Định dạng: ddMMyyyyHHmmss</p></td>
</tr>
<tr>
<td style="text-align: center;">4</td>
<td>data</td>
<td>JsonObject</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Kết quả trả về dưới dạng Json Object</td>
</tr>
<tr>
<td style="text-align: center;"></td>
<td colspan="4">Mô tả cấu trúc JsonObject <strong>data</strong></td>
</tr>
<tr>
<td style="text-align: center;"><em>4.1</em></td>
<td><em>txnId</em></td>
<td><em>String</em></td>
<td style="text-align: center;"></td>
<td style="text-align: left;"><em>Mã giao dịch được trả về khi khởi tạo yêu cầu ban đầu thành công</em></td>
</tr>
<tr>
<td style="text-align: center;"><em>4.2</em></td>
<td><em>requestId</em></td>
<td></td>
<td style="text-align: center;"></td>
<td style="text-align: left;"><em>Tham chiếu mã yêu cầu do hệ thống của TCDN gửi sang Hệ thống nền tảng</em></td>
</tr>
<tr>
<td colspan="5" style="text-align: center;">Ứng dụng của TCDN sau khi nhận được mã giao dịch (txnId), tiến hành gọi Web/App VNeID theo URL được cung cấp dưới đây để chuyển hướng người dùng sang VNeID thực hiện Consent</td>
</tr>
<tr>
<td style="text-align: center;"></td>
<td>URL WEB</td>
<td colspan="3" style="text-align: left;">Dịch vụ này chỉ có trên APP VNeID không triển khai trên WEB VNeID</td>
</tr>
<tr>
<td style="text-align: center;"></td>
<td>URL APP</td>
<td colspan="3" style="text-align: left;"><p><em>Chi tiết tham chiếu mục III.4.4 Tài liệu này.</em></p>
<p>- Môi trường tích hợp: https://universal.dancuquocgia.com/share/[$transactionCode]</p>
<p>- Môi trường PRO: N/A</p></td>
</tr>
</tbody>
</table>

### III.6.2. API thông báo kết quả xác nhận chia sẻ/ không chia sẻ của tổ chức và các mã lỗi khác cho ứng dụng của TCDN. {#iii-6-2-api-thong-bao-ket-qua-to-chuc}

25. \* Cơ chế hoạt động của API III.6.2 này tương tự API III.2.2. Bên dưới mô tả Sample, Tham khảo Request Body và Cơ chế ở API III.2.2.

#### API Specs

<table>
<colgroup>
<col style="width: 15%" />
<col style="width: 84%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>Thông tin</strong></th>
<th style="text-align: center;"><strong>Mô tả</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: left;">Mục đích</td>
<td style="text-align: left;">Thông báo kết quả xác nhận chia sẻ/ không chia sẻ của tổ chức và các mã lỗi khác cho ứng dụng của TCDN</td>
</tr>
<tr>
<td style="text-align: left;">URL</td>
<td style="text-align: left;">TCDN định nghĩa và cấu hình sẵn trong AgentGW</td>
</tr>
<tr>
<td style="text-align: left;">Method</td>
<td style="text-align: left;">POST</td>
</tr>
<tr>
<td style="text-align: left;">Header</td>
<td style="text-align: left;"><ul>
<li><p><strong>Content-Type:</strong> application/json; charset=utf-8</p></li>
<li><p><strong>api_key:</strong> API key //Do TCDN cung cấp.</p></li>
<li><p><strong>request_id:</strong> Mã yêu cầu do hệ thống Nền tảng tự sinh, là duy nhất trên hệ thống.</p></li>
<li><p><strong>request_time:</strong> Thời gian yêu cầu, định dạng ddMMyyyyHHmmss.</p></li>
</ul></td>
</tr>
<tr>
<td style="text-align: left;">Request body</td>
<td style="text-align: left;"><p>{</p>
<p>    "requestId": "3d60591a-315c-457e-b926-1b3ec699e0a9",</p>
<p>    "resultCode": "-4", //Tham chiếu bảng mã tại mục IV.4</p>
<p>    "resultDesc": "Giao dịch đang xử lý, vui lòng thử lại sau",</p>
<p>    "txnId": "22fc12e0-be90-4519-8ef6-cdeca95ae22b"</p>
<p>}</p></td>
</tr>
<tr>
<td style="text-align: left;">Response body</td>
<td style="text-align: left;"><p>{</p>
<p>    "responseCode": 1, //0: thành công, 1: không thành công</p>
<p>    "description": "Mô tả lỗi",</p>
<p>    "responseTime": "30092024190321" //ddMMyyyyHHmmss</p>
<p>}</p></td>
</tr>
</tbody>
</table>

#### Request Body

<table style="width:100%;">
<colgroup>
<col style="width: 7%" />
<col style="width: 14%" />
<col style="width: 11%" />
<col style="width: 9%" />
<col style="width: 56%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>STT</strong></th>
<th style="text-align: center;"><strong>Tên trường</strong></th>
<th style="text-align: center;"><strong>Kiểu dữ liệu</strong></th>
<th style="text-align: center;"><strong>Bắt buộc</strong></th>
<th style="text-align: center;"><strong>Mô tả</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: center;">1</td>
<td>requestId</td>
<td>String</td>
<td style="text-align: center;">x</td>
<td>Tham chiếu đến request_id trong API gửi yêu cầu khởi tạo ban đầu của TCDN</td>
</tr>
<tr>
<td style="text-align: center;">2</td>
<td>resultCode</td>
<td>String</td>
<td style="text-align: center;">x</td>
<td><p>Mã kết quả xử lý yêu cầu chia sẻ thông tin:</p>
<p><em>Tham chiếu mục IV.4 – Mã lỗi responseCode, resultCode</em></p>
<p>1: Đồng ý chia sẻ</p>
<p>2: Không đồng ý chia sẻ</p></td>
</tr>
<tr>
<td style="text-align: center;">3</td>
<td>resultDesc</td>
<td>String</td>
<td style="text-align: center;"></td>
<td><p>Mô tả chi tiết mã kết quả resultCode:</p>
<p>Tham chiếu mục IV.4 – Mã lỗi responseCode, resultCode</p></td>
</tr>
<tr>
<td style="text-align: center;">4</td>
<td>txnId</td>
<td>String</td>
<td style="text-align: center;">x</td>
<td>Mã giao dịch để sử dụng truy vấn thông tin địn danh tổ chức (trường hợp người đại diện pháp luật xác nhận đồng ý)</td>
</tr>
</tbody>
</table>

#### Response body

26. Ứng dụng của tổ chức doanh nghiệp trả về trạng thái với **httpstatus = 200 (OK)** cùng bản tin theo định dạng JSON.

<table>
<colgroup>
<col style="width: 7%" />
<col style="width: 17%" />
<col style="width: 16%" />
<col style="width: 10%" />
<col style="width: 46%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>STT</strong></th>
<th style="text-align: center;"><strong>Tên trường</strong></th>
<th style="text-align: center;"><strong>Kiểu dữ liệu</strong></th>
<th style="text-align: center;"><strong>Bắt buộc</strong></th>
<th style="text-align: center;"><strong>Ghi chú</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: center;">1</td>
<td>responseCode</td>
<td><p>String</p>
<p>(2 ký tự)</p></td>
<td style="text-align: center;">x</td>
<td style="text-align: left;"><p>Mã trạng thái tiếp nhận:</p>
<p>0: thành công</p>
<p>Khác 0: Lỗi (mô tả chi tiết tại trường description)</p>
<p><em>Tham chiếu mục IV.4, cụ thể:</em></p>
<p>2: Không đồng ý chia sẻ</p>
<p>1: Đồng ý chia sẻ</p>
<p>-1: Không lấy được thông tin</p>
<p>-15: Tồn tại trường dữ liệu bắt buộc bị “null”</p>
<p>-17: Thông tin định danh tổ chức không hợp lệ, vui lòng kiểm tra chi tiết trên ứng dụng VNeID</p>
<p>-20: Thông tin định danh tổ chức không đúng</p></td>
</tr>
<tr>
<td style="text-align: center;">2</td>
<td>description</td>
<td><p>String</p>
<p>(100 ký tự)</p></td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Mô tả lỗi trong trường hợp trạng thái tiếp nhận lỗi (responseCode khác 0)</td>
</tr>
<tr>
<td style="text-align: center;">3</td>
<td>responseTime</td>
<td><p>String</p>
<p>(100 ký tự)</p></td>
<td style="text-align: center;">x</td>
<td style="text-align: left;"><p>Thời gian phản hồi kết quả</p>
<p>Định dạng: ddMMyyyyHHmmss</p></td>
</tr>
</tbody>
</table>

### III.6.3. API yêu cầu trả kết quả dữ liệu định danh tổ chức sau khi người đại diện pháp luật đồng ý chia sẻ, kết quả xác thực thông tin định danh tổ chức {#iii-6-3-api-yeu-cau-tra-ket-qua-dinh-danh-to-chuc}

**Lưu ý:**

- transactionId sẽ hết hạn sau 30 phút kể từ thời điểm ghi nhận thành công việc người đại diện pháp luật đã thực hiện xác nhận trên VNeID.

- TCDN chỉ có thể gọi lấy dữ liệu tối đa 03 lần trong khoảng thời gian 30 phút, quá thời gian trên sẽ không nhận được dữ liệu định danh tổ chức đã chia sẻ.

#### API Specs

<table>
<colgroup>
<col style="width: 18%" />
<col style="width: 81%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>Thông tin</strong></th>
<th style="text-align: center;"><strong>Mô tả</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: left;">Tên API</td>
<td style="text-align: left;">API cung cấp cho TCDN khai thác yêu cầu trả kết quả dữ liệu định danh tổ chức đồng ý chia sẻ</td>
</tr>
<tr>
<td style="text-align: left;">Mục đích</td>
<td style="text-align: left;">Cho phép TCDN gửi yêu cầu trả kết quả dữ liệu định danh tổ chức đồng ý chia sẻ</td>
</tr>
<tr>
<td style="text-align: left;">URL</td>
<td style="text-align: left;"><p>https://&lt;domain&gt;:&lt;port&gt;/api/tcdn<strong>/{service-code}</strong>/api/get-transaction</p>
<p><strong>{service-code}</strong> tham chiếu theo bảng Mã dịch vụ IV.3.</p></td>
</tr>
<tr>
<td style="text-align: left;">Method</td>
<td style="text-align: left;">POST</td>
</tr>
<tr>
<td style="text-align: left;">Header</td>
<td style="text-align: left;"><ul>
<li><p><strong>Content-Type:</strong> application/json; charset=utf-8</p></li>
<li><p><strong>api_key:</strong> API key</p></li>
<li><p><mark><strong>target_code:</strong> Mã mục đích chia sẻ, xử lý thông tin do Hệ thống định danh cung cấp, phục vụ hiển thị đúng nội dung mục đích chia sẻ, xử lý thông tin trên ứng dụng VNeID.</mark></p></li>
<li><p><strong>request_id:</strong> Mã yêu cầu do hệ thống của TCDN tự sinh, là duy nhất trên hệ thống.</p></li>
<li><p><strong>request_time:</strong> Thời gian yêu cầu, định dạng ddMMyyyyHHmmss. Là thời gian hiện tại của hệ thống, trong trường hợp request_time gửi sang Hệ thống NT lệch quá 2 phút so với thời gian hiện tại của Hệ thống nền tảng thì request đó được coi là timeout.</p></li>
</ul></td>
</tr>
<tr>
<td style="text-align: left;">Params</td>
<td style="text-align: left;"><p>Trên môi trường non-production:</p>
<p>code=mã đơn vị kết nối (do Đơn vị cung cấp dịch vụ cấp)</p>
<p>Trên môi trường prod: N/A</p></td>
</tr>
<tr>
<td style="text-align: left;">Request body</td>
<td style="text-align: left;"><p><strong>Dịch vụ xác thực/chia sẻ thông tin định danh tổ chức</strong></p>
<p>{</p>
<p>    "transactionId":"9baf1668-a0da-4f9a-b9ac-5eef6f5da5f6",</p>
<p>}</p></td>
</tr>
<tr>
<td style="text-align: left;">Response body</td>
<td style="text-align: left;"><p><strong>Dịch vụ chia sẻ thông tin định danh tổ chức</strong></p>
<p>{</p>
<p>    "responseCode": 0, //0: thành công, khác 0: lỗi</p>
<p>    "description": "Mô tả lỗi nếu có",</p>
<p>    "responseTime": "ddMMyyyyHHmmss",</p>
<p>    "data": {</p>
<p>        "userInfo": "eyJhbGciOiJFUzI1NiIsInR5cCI6IkpPU0UvSldTIn0=.ewogICJjaXRpemVuUGlkIiA6ICIwMzYxODgwMDE5NTgiLAogICJmdWxsTmFtZSIgOiAiTmd1eeG7hW4gVGjhu4sgSHXhur8iLAogICJsaXZpbmdQbGFjZUNpdHlUZXh0IiA6ICJUaMOgbmggcGjhu5EgSMOgIE7hu5lpIiwKICAibGl2aW5nUGxhY2VEaXN0cmljdFRleHQiIDogIkh1eeG7h24gSG_DoGkgxJDhu6ljIiwKICAicGVybWFuZW50Q2l0eVRleHQiIDogIlRow6BuaCBwaOG7kSBIw6AgTuG7mWkiLAogICJwZXJtYW5lbnREaXN0cmljdFRleHQiIDogIkh1eeG7h24gSG_DoGkgxJDhu6ljIiwKICAicGVybWFuZW50VmlsbGFnZVRleHQiIDogIljDoyBBbiBLaMOhbmgiLAogICJsaXZpbmdQbGFjZVZpbGxhZ2VUZXh0IiA6ICJYw6MgQW4gS2jDoW5oIiwKICAicGVybWFuZW50Q2l0eUNvZGUiIDogIjAxIiwKICAicGVybWFuZW50RGlzdHJpY3RDb2RlIiA6ICIyNzQiLAogICJwZXJtYW5lbnRWaWxsYWdlQ29kZSIgOiAiMDk4NzciLAogICJwZXJtYW5lbnRBZGRyZXNzIiA6ICJLaHUgxJDDtCBUaOG7iyBIYWkgQsOqbiDEkMaw4budbmcgTMOqIFRy4buNbmcgVOG6pW4sIEFuIEtow6FuaCwgSG_DoGkgxJDhu6ljLCBIw6AgTuG7mWkiLAogICJsaXZpbmdQbGFjZUNpdHlDb2RlIiA6ICIwMSIsCiAgImxpdmluZ1BsYWNlRGlzdHJpY3RDb2RlIiA6ICIyNzQiLAogICJsaXZpbmdQbGFjZVZpbGxhZ2VDb2RlIiA6ICIwOTg3NyIsCiAgImxpdmluZ1BsYWNlQWRkcmVzcyIgOiAiS2h1IMSQw7QgVGjhu4sgSGFpIELDqm4gxJDGsOG7nW5nIEzDqiBUcuG7jW5nIFThuqVuLCBBbiBLaMOhbmgsIEhvw6BpIMSQ4bupYywgSMOgIE7hu5lpIiwKICAiZGF0ZU9mSXNzdWUiIDogIjIzLTA3LTIwMjQiLAogICJpZENhcmRFeHBpcmVEYXRlIiA6ICIzMC0xMi0yMDMwIiwKICAiaXNzdWluZ0F1dGhvcml0eSIgOiAiQ-G7pWMgQ-G6o25oIHPDoXQgcXXhuqNuIGzDvSBow6BuaCBjaMOtbmggduG7gSB0cuG6rXQgdOG7sSB4w6MgaOG7mWkiLAogICJiaXJ0aERhdGUiIDogIjI0LTA0LTE5ODgiLAogICJuYXRpb25hbGl0eUNvZGUiIDogIlZOIgp9.MEUCIBNqg0LXH7UtJvU_A3vRZLLR20iacQxVK7lT1YZTVTJhAiEAjdPapssFIwhylfs68ufjT7RlByIKtrbjVIjw60CJWnc="</p>
<p>        //userInfo: định dạng <mark>base64 encode</mark> tuân theo chuẩn JWS có format: &lt;header&gt;.&lt;payload&gt;.&lt;signature&gt;</p>
<p>        // Tổ chức khai thác có thể sử dụng các thư viện tiêu chuẩn để verify chữ ký (với public key được C06 cung cấp) và parse dữ liệu payload để có được thông tin công dân.</p>
<p>    }</p>
<p>}</p>
<p><strong>// Ví dụ minh họa đối với userInfo sau khi parse theo chuẩn JWS</strong></p>
<p><strong>// Cấu trúc &lt;payload&gt; :</strong> Lưu ý các trường thông tin có thể thay đổi tuỳ vào nhu cầu của TCDN cũng như sự đồng ý chia sẻ của công dân trên ứng dụng VNeID.</p>
<p>{</p>
<p>    "orgPid": "Số định danh tổ chức",</p>
<p>    "orgName": "Tên doanh nghiệp",</p>
<p>    "orgShortName": "Tên viết tắt",</p>
<p>    "taxCode": "Mã số thuế, mã số doanh nghiệp, mã số hợp tác xã",</p>
<p>    "foundingDate": "Ngày thành lập định dạng dd/MM/yyyy",</p>
<p>    "addressProvinceCode": "Địa chỉ trụ sở chính mã tỉnh/tp của tổ chức",</p>
<p>    "addressProvinceText": "Địa chỉ trụ sở chính tên tỉnh/tp của tổ chức",</p>
<p>    "addressWardCode": "Địa chỉ trụ sở chính mã phường/xã của tổ chức",</p>
<p>    "addressWardText": "Địa chỉ trụ sở chính tên phường/xã của tổ chức",</p>
<p>    "addressDetail": "Địa chỉ chi tiết của tổ chức",</p>
<p>    "representCitizenPid": "Số định danh người đại điện tài khoản định danh tổ chức",</p>
<p>    "representFullName": "Tên định danh người đại điện tài khoản định danh tổ chức",</p>
<p>    "representPositionText": "Tên chức danh",</p>
<p>    "business": [ //danh sách ngành nghề kinh doanh</p>
<p>        {</p>
<p>            "code": "Mã ngành nghề kinh doanh",</p>
<p>            "name": "Tên ngành nghề kinh doanh"</p>
<p>        }</p>
<p>    ],</p>
<p>    "represents": [</p>
<p>        {</p>
<p>            "citizenPid": "Số định danh",</p>
<p>            "fullName": "Họ và tên",</p>
<p>            "positionText": "Tên chức danh"</p>
<p>        }</p>
<p>    ]</p>
<p>}</p>
<p><strong>Dịch vụ xác thực thông tin định danh tổ chức</strong></p>
<p>{</p>
<p>    "description": "Success!",</p>
<p>    "responseTime": "01102024100947",</p>
<p>    "responseCode": "0",</p>
<p>    "data": {</p>
<p>        "biometric": 0,</p>
<p>        "pid": <strong>null</strong></p>
<p>    },</p>
<p>    "userInfo": "eyJraWQiOiItMzI0NDIiLCJhbGciOiJTSEEyNTYiLCJjdHkiOiJFSUQiLCJ0eXAiOiJKT1NFL0pXUyJ9.ewogICJmdWxsTmFtZSIgOiB0cnVlLAogICJiaXJ0aERhdGUiIDogdHJ1ZSwKICAibmF0aW9uYWxpdHlDb2RlIiA6IGZhbHNlLAogICJjaXRpemVuUGlkIiA6IHRydWUsCiAgImRhdGVPZklzc3VlIiA6ICIyMy0wNy0yMDI0IiwKICAiaXNzdWluZ0F1dGhvcml0eSIgOiAiQ+G7pWMgQ+G6o25oIHPDoXQgcXXhuqNuIGzDvSBow6BuaCBjaMOtbmggduG7gSB0cuG6rXQgdOG7sSB4w6MgaOG7mWkiLAogICJsaXZpbmdQbGFjZUFkZHJlc3MiIDogIktodSDEkMO0IFRo4buLIEhhaSBCw6puIMSQxrDhu51uZyBMw6ogVHLhu41uZyBU4bqlbiwgQW4gS2jDoW5oLCBIb8OgaSDEkOG7qWMsIEjDoCBO4buZaSIsCiAgImxpdmluZ1BsYWNlVmlsbGFnZUNvZGUiIDogIjA5ODc3IiwKICAibGl2aW5nUGxhY2VDaXR5Q29kZSIgOiAiMDEiLAogICJsaXZpbmdQbGFjZURpc3RyaWN0Q29kZSIgOiAiMjc0IiwKICAicGVybWFuZW50QWRkcmVzcyIgOiAiS2h1IMSQw7QgVGjhu4sgSGFpIELDqm4gxJDGsOG7nW5nIEzDqiBUcuG7jW5nIFThuqVuLCBBbiBLaMOhbmgsIEhvw6BpIMSQ4bupYywgSMOgIE7hu5lpIiwKICAicGVybWFuZW50VmlsbGFnZUNvZGUiIDogdHJ1ZSwKICAicGVybWFuZW50RGlzdHJpY3RDb2RlIiA6IHRydWUsCiAgInBlcm1hbmVudENpdHlDb2RlIiA6IHRydWUKfQ==.MEUCIEvoA6SolfQteGSYfk7WwsrjfYfF3HT5fyNKVi6X+fK0AiEArzvwrywy0CCYolESLFnHec6dYM4P1L4tg6nBa6K29ks="</p>
<p>}</p>
<p>// Tương tự như trên, userInfo: định dạng <mark>base64 encode</mark> tuân theo chuẩn JWS có format: &lt;header&gt;.&lt;payload&gt;.&lt;signature&gt;. Cấu trúc &lt;payload&gt; userInfo sau khi parse theo chuẩn JWS như sau:</p>
<p>// Lưu ý: các trường thông tin có thể thay đổi tuỳ vào các trường trong yêu cầu xác thực của TCDN cũng như sự đồng ý chia sẻ của công dân trên ứng dụng VNeID.</p>
<p>- Đối với các trường đầu vào có dữ liệu trong request body API tại mục III.4.1 kết quả sẽ trả về <strong>true</strong>/ false tương ứng với dữ liệu gửi lên <strong>trùng khớp</strong>/ không trùng khớp với thông tin của công dân trong CSDL.</p>
<p>- Đối với các trường đầu vào cấu hình nhưng không có dữ liệu trong request body API tại mục III.4.1 kết quả sẽ trả về thông tin tương ứng của công dân nếu công dân đồng ý chia sẻ.</p>
<p>{</p>
<p>    "orgPid": "Số định danh tổ chức",</p>
<p>    "orgName": "Tên doanh nghiệp",</p>
<p>    "orgShortName": "Tên viết tắt",</p>
<p>    "taxCode": "Mã số thuế, mã số doanh nghiệp, mã số hợp tác xã",</p>
<p>    "foundingDate": "Ngày thành lập định dạng dd/MM/yyyy"</p>
<p>}</p></td>
</tr>
</tbody>
</table>

#### Request body

| **STT** | **Tên trường** | **Kiểu dữ liệu** | **Bắt buộc** | **Mô tả**                                                            |
| :-----: | :------------: | :--------------: | :----------: | -------------------------------------------------------------------- |
|    1    | transactionId  |      String      |      x       | Mã giao dịch đã được trả về khi khởi tạo yêu cầu ban đầu thành công. |

#### Response body

27. Hệ thống nền tảng định danh trả về trạng thái với **httpstatus = 200 (OK)** cùng bản tin theo định dạng JSON.

<table>
<colgroup>
<col style="width: 11%" />
<col style="width: 27%" />
<col style="width: 14%" />
<col style="width: 8%" />
<col style="width: 38%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>STT</strong></th>
<th style="text-align: center;"><strong>Tên trường</strong></th>
<th style="text-align: center;"><strong>Kiểu dữ liệu</strong></th>
<th style="text-align: center;"><strong>Bắt buộc</strong></th>
<th style="text-align: center;"><strong>Ghi chú</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: center;">1</td>
<td>responseCode</td>
<td><p>String</p>
<p>(2 ký tự)</p></td>
<td style="text-align: center;">x</td>
<td style="text-align: left;"><p>Mã trạng thái tiếp nhận:</p>
<p>0: thành công</p>
<p>Khác 0: Lỗi (mô tả chi tiết tại trường description)</p>
<p><em>Tham chiếu mục IV.4, cụ thể:</em></p>
<p><em>-1: Không lấy được thông tin</em></p>
<p><em>-4: Giao dịch đang xử lý, vui lòng thử lại sau</em></p>
<p><em>-5: Đầu vào yêu cầu không hợp lệ!</em></p>
<p><em>-6: Không tìm thấy tổ chức</em></p>
<p><em>-7: Không tìm thấy dịch vụ</em></p>
<p><em>-8: Đường dẫn không hợp lệ</em></p>
<p><em>-9: Không tìm thấy mã giao dịch</em></p>
<p><em>-11: Không đồng ý chia sẻ thông tin</em></p>
<p><em>-12: Không tìm thấy cấu hình agent</em></p>
<p><em>-14: Timeout</em></p>
<p><em>-15: Tồn tại trường dữ liệu bắt buộc bị “null”</em></p>
<p><em>-16: Dịch vụ đang sử dụng vượt quá hạn mức cho phép</em></p>
<p><em>-17: Thông tin công dân không hợp lệ, vui lòng kiểm tra chi tiết trên ứng dụng VNeID</em></p>
<p><em>-19: Tài khoản tổ chức không có quyền sử dụng dịch vụ</em></p>
<p><em>-20: Thông tin định danh tổ chức không đúng</em></p></td>
</tr>
<tr>
<td style="text-align: center;">2</td>
<td>description</td>
<td><p>String</p>
<p>(100 ký tự)</p></td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Mô tả lỗi trường hợp trạng thái tiếp nhận khác 0</td>
</tr>
<tr>
<td style="text-align: center;">3</td>
<td>responseTime</td>
<td><p>String</p>
<p>(100 ký tự)</p></td>
<td style="text-align: center;">x</td>
<td style="text-align: left;"><p>Thời gian phản hồi kết quả</p>
<p>Định dạng: ddMMyyyyHHmmss</p></td>
</tr>
<tr>
<td style="text-align: center;">4</td>
<td>data</td>
<td>JsonObject</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;"><p>Kết quả xác thực sinh trắc học.</p>
<p>Tuân theo chuẩn JWS. Là chuỗi Base64 có format:</p>
<p>&lt;header&gt;.&lt;payload&gt;.&lt;signature&gt;</p></td>
</tr>
<tr>
<td style="text-align: center;"></td>
<td colspan="4" style="text-align: left;">Mô tả cấu trúc JsonObject <strong>data</strong></td>
</tr>
<tr>
<td style="text-align: center;">4.2.2</td>
<td>transId</td>
<td>String</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Là trường transId ở request body</td>
</tr>
<tr>
<td style="text-align: center;"></td>
<td colspan="4"><p><strong>Mô tả cấu trúc trường thông tin userInfo sau khi parse theo chuẩn JWS</strong></p>
<p><em><strong>Lưu ý:</strong> các trường thông tin có thể thay đổi tuỳ vào nhu cầu của TCDN cũng như sự đồng ý chia sẻ của tổ chức trên ứng dụng VNeID.</em></p></td>
</tr>
<tr>
<td style="text-align: center;">4.1</td>
<td>orgPid</td>
<td>string</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Số định danh tổ chức</td>
</tr>
<tr>
<td style="text-align: center;">4.2</td>
<td>orgName</td>
<td>string</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Tên doanh nghiệp</td>
</tr>
<tr>
<td style="text-align: center;">4.3</td>
<td>orgShortName</td>
<td>string</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Tên viết tắt</td>
</tr>
<tr>
<td style="text-align: center;">4.4</td>
<td>taxCode</td>
<td>string</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Mã số thuế, mã số doanh nghiệp, mã số hợp tác xã</td>
</tr>
<tr>
<td style="text-align: center;">4.5</td>
<td>foundingDate</td>
<td>string</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Ngày thành lập định dạng dd/MM/yyyy</td>
</tr>
<tr>
<td style="text-align: center;">4.6</td>
<td>addressProvinceCode</td>
<td>string</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Địa chỉ trụ sở chính mã tỉnh/tpcủa tổ chức</td>
</tr>
<tr>
<td style="text-align: center;">4.7</td>
<td>addressProvinceText</td>
<td>string</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Địa chỉ trụ sở chính tên tỉnh/tp của tổ chức</td>
</tr>
<tr>
<td style="text-align: center;">4.8</td>
<td>addressWardCode</td>
<td>string</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Địa chỉ trụ sở chính mã phường/xã của tổ chức</td>
</tr>
<tr>
<td style="text-align: center;">4.9</td>
<td>addressWardText</td>
<td>string</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Địa chỉ trụ sở chính tên phường/xã của tổ chức</td>
</tr>
<tr>
<td style="text-align: center;">4.10</td>
<td>addressDetail</td>
<td>string</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Địa chỉ chi tiết của tổ chức</td>
</tr>
<tr>
<td style="text-align: center;">4.11</td>
<td>representCitizenPid</td>
<td>string</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Số định danh người đại điện tàikhoản định danh tổ chức</td>
</tr>
<tr>
<td style="text-align: center;">4.12</td>
<td>representFullName</td>
<td>string</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Tên định danh người đại điện tàii khoản định danh tổ chức</td>
</tr>
<tr>
<td style="text-align: center;">4.13</td>
<td>representPositionText</td>
<td>string</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Tên chức danh</td>
</tr>
<tr>
<td style="text-align: center;">4.14</td>
<td>business</td>
<td>array</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">danh sách ngành nghề kinh doanh</td>
</tr>
<tr>
<td style="text-align: center;">4.14.1</td>
<td>code</td>
<td>string</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Mã ngành nghề kinh doanh</td>
</tr>
<tr>
<td style="text-align: center;">4.14.2</td>
<td>name</td>
<td>string</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Tên ngành nghề kinh doanh</td>
</tr>
<tr>
<td style="text-align: center;">4.15</td>
<td>represents</td>
<td>array</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Danh sách đại diện pháp luật của tổ chức</td>
</tr>
<tr>
<td style="text-align: center;">4.15.1</td>
<td>citizenPid</td>
<td>string</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Số định danh</td>
</tr>
<tr>
<td style="text-align: center;">4.15.2</td>
<td>fullName</td>
<td>string</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Họ và tên</td>
</tr>
<tr>
<td style="text-align: center;">4.15.3</td>
<td>positionText</td>
<td>string</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Tên chức danh</td>
</tr>
</tbody>
</table>

### III.6.4. Universal Link {#iii-6-4-universal-link}

#### Universal Link ứng dụng VNeID

- <span id="_Toc201751284" class="anchor"></span>**Cấu trúc:** UniversalLink/share/\[\$transactionCode\]

- UniversalLink:
  - Môi trường tích hợp: https://universal.dancuquocgia.com/share/\[\$transactionCode\]

  - Môi trường Prod: N/A

- \[\$transactionCode\] : mã giao dịch xin consent khi khởi tạo giao dịch ở API III.2.1

#### RedirectUrl tổ chức sử dụng dịch vụ khai báo cho ứng dụng VneID

28. <span class="mark">\* Universal link khi Switch App to App từ VNeID sang App TCDN có 2 dạng:</span>

    1\. Luồng xuất phát từ Ứng dụng TCDN

- **Cấu trúc:** URL_TCDN/\[\$transactionCode\]\|\[\$result\]

- **Mô tả:**

<table>
<colgroup>
<col style="width: 10%" />
<col style="width: 35%" />
<col style="width: 54%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>STT</strong></th>
<th style="text-align: center;"><strong>Code</strong></th>
<th style="text-align: center;"><strong>Mô tả</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: center;">1</td>
<td style="text-align: left;">URL_TCDN</td>
<td>Tổ chức sử dụng dịch vụ cung cấp</td>
</tr>
<tr>
<td style="text-align: center;">2</td>
<td style="text-align: left;">[$transactionCode]</td>
<td>Mã giao dịch xin consent</td>
</tr>
<tr>
<td style="text-align: center;">3</td>
<td style="text-align: left;">[$result]</td>
<td><p>Kết quả:</p>
<p>1: Đã đồng ý; 2: Không đồng ý; 3: Hết hạn</p></td>
</tr>
</tbody>
</table>

30. 2\. Luồng xuất phát từ Ứng dụng VNeID

    **- Cấu trúc:** URL_TCDN/\[\$transactionCode\]

    \- Mô tả:

| **STT** | **Code**              | **Mô tả**                        |
| :-----: | :-------------------- | -------------------------------- |
|    1    | URL_TCDN              | Tổ chức sử dụng dịch vụ cung cấp |
|    2    | \[\$transactionCode\] | Mã giao dịch xin consent         |

## III.7. Dịch vụ xác nhận chia sẻ thông tin định danh tổ chức, cá nhân kèm dịch vụ chứng thư số {#iii-7-dich-vu-xac-nhan-dinh-danh-to-chuc-ca-nhan}

### III.7.1. API gửi yêu cầu chia sẻ thông tin định danh tổ chức, cá nhân {#iii-7-1-api-gui-yeu-cau-dinh-danh-to-chuc-ca-nhan}

\* **Tổ chức doanh nghiệp cần cung cấp** đường link dẫn tới trang công bố **“Điều khoản và cam kết sử dụng dữ liệu được chia sẻ”** của Tổ chức doanh nghiệp, nhằm hiển thị trên ứng dụng VNeID.

#### API Specs

<table>
<colgroup>
<col style="width: 16%" />
<col style="width: 83%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>Thông tin</strong></th>
<th style="text-align: center;"><strong>Mô tả</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td>Mục đích</td>
<td>API sử dụng để AgentGW chuyển tiếp yêu cầu xác nhận chia sẻ thông tin định danh tổ chức, cá nhân từ TCDN đến NT.</td>
</tr>
<tr>
<td>URL</td>
<td><p>https://&lt;domain&gt;:&lt;port&gt;/api/tcdn/<strong>{service-code}/</strong>api/agent/init</p>
<p><strong>{service-code}</strong> tham chiếu theo bảng Mã dịch vụ IV.3.</p></td>
</tr>
<tr>
<td>Method</td>
<td>POST</td>
</tr>
<tr>
<td>Header</td>
<td><ul>
<li><p><strong>Content-Type:</strong> application/json; charset=utf-8</p></li>
<li><p><strong>api_key:</strong> API key</p></li>
<li><p><mark><strong>target_code:</strong> Mã mục đích chia sẻ, xử lý thông tin do Hệ thống định danh cung cấp, phục vụ hiển thị đúng nội dung mục đích chia sẻ, xử lý thông tin trên ứng dụng VNeID.</mark></p></li>
<li><p><strong>request_id:</strong> Mã yêu cầu do hệ thống của TCDN tự sinh, là duy nhất trên hệ thống.</p></li>
<li><p><strong>request_time:</strong> Thời gian yêu cầu, định dạng ddMMyyyyHHmmss. Là thời gian hiện tại của hệ thống, trong trường hợp request_time gửi sang Hệ thống nền tảng lệch quá 2 phút so với thời gian hiện tại của Hệ thống nền tảng thì request đó được coi là timeout.</p></li>
</ul></td>
</tr>
<tr>
<td style="text-align: left;">Params</td>
<td style="text-align: left;"><p>Trên môi trường non-production:</p>
<p>code=mã đơn vị kết nối (do Đơn vị cung cấp dịch vụ cấp)</p>
<p>Trên môi trường prod: N/A</p></td>
</tr>
<tr>
<td>Request body</td>
<td style="text-align: left;"><p>Dịch vụ chia sẻ thông tin định danh tổ chức, cá nhân</p>
<p>{</p>
<p>    "fullName": <strong>null</strong>,</p>
<p>    "birthDate": <strong>null</strong>,</p>
<p>    "citizenPid": "",</p>
<p>    "type": "ORG", //Loại consent, type = Per hoặc null thì là consent cá nhâm , type = ORG sẽ là consent chia sẻ thông tin tổ chức</p>
<p>    "shareOrgNumber": "Số định danh tổ chức cần lấy dữ liệu", //Số định danh tổ chức cần lấy dữ liệu, bắt buộc khi type = ORG</p>
<p>    "listOrgCode": ["VCB", "BIDV"],</p>
<p>    <mark>"feeCode": "acbd",//Gói dịch vụ đã đăng ký với CA</mark></p>
<p><mark>    "keyData": "efgh"// thông tin truyền lên cho SDK sinh khóa (do CA cấp cho tổ chức)</mark></p>
<p>}</p></td>
</tr>
<tr>
<td>Respond body</td>
<td style="text-align: left;"><p>Thành công</p>
<p>{</p>
<p>    "responseCode": 0, //0: thành công, Khác 0: không thành công</p>
<p>    "description": "Thành công",</p>
<p>    "responseTime": "30092024190321", //ddMMyyyyHHmmss</p>
<p>    "data": {</p>
<p>        "txnId": "22fc12e0-be90-4519-8ef6-cdeca95ae22b",</p>
<p>        "requestId": "3d60591a-315c-457e-b926-1b3ec699e0a9"</p>
<p>    }</p>
<p>}</p>
<p>Không thành công</p>
<p>{</p>
<p>    "responseCode": 1, //0: thành công, Khác 0: không thành công</p>
<p>    "description": "Mô tả lỗi",</p>
<p>    "responseTime": "30092024190321", //ddMMyyyyHHmmss</p>
<p>    "data": ""</p>
<p>}</p></td>
</tr>
</tbody>
</table>

#### Request Body

**\* Các thông tin định danh tổ chức** có thể chia sẻ thông qua các dịch vụ chia sẻ thông tin bao gồm: Tên doanh nghiệp (tên tiếng việt, tên tiếng anh, tên viết tắt), ngành nghề đăng ký kinh doanh, thông tin người đại diện pháp luật,…

**\* Đối với dịch vụ xác thực thông tin định danh tổ chức:** Các trường dữ liệu được TCDN truyền lên sẽ nhận về kết quả Đúng/Sai (True/False) so với dữ liệu trong CSDL định danh tổ chức.

#### Response Body

<table>
<colgroup>
<col style="width: 6%" />
<col style="width: 18%" />
<col style="width: 19%" />
<col style="width: 11%" />
<col style="width: 44%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>TT</strong></th>
<th style="text-align: center;"><strong>Tên trường</strong></th>
<th style="text-align: center;"><strong>Định dạng</strong></th>
<th style="text-align: center;"><strong>Bắt buộc</strong></th>
<th style="text-align: center;"><strong>Mô tả</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: center;">1</td>
<td>responseCode</td>
<td><p>String</p>
<p>(2 ký tự)</p></td>
<td style="text-align: center;">x</td>
<td style="text-align: left;"><p>Mã trạng thái tiếp nhận:</p>
<p>0: thành công</p>
<p>Khác 0: Lỗi (mô tả chi tiết tại trường description)</p>
<p><em>Tham chiếu mục IV.4, cụ thể:</em></p>
<p>-2: Giao dịch đã tồn tại</p>
<p>-5: Đầu vào yêu cầu không hợp lệ!</p>
<p>-6: Không tìm thấy tổ chức</p>
<p>-7: Không tìm thấy dịch vụ</p>
<p>-8: Đường dẫn không hợp lệ</p>
<p>-12: Không tìm thấy cấu hình agent</p>
<p>-14: Timeout</p>
<p>-16: Dịch vụ đang sử dụng vượt quá hạn mức cho phép</p>
<p>-18: Trường đầu vào không tồn tại trong cấu hình dịch vụ</p>
<p>-19: Tài khoản tổ chức không có quyền sử dụng dịch vụ</p>
<p>-20: Thông tin định danh tổ chức không đúng</p></td>
</tr>
<tr>
<td style="text-align: center;">2</td>
<td>description</td>
<td><p>String</p>
<p>(100 ký tự)</p></td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Mô tả lỗi trường hợp trạng thái tiếp nhận khác 0</td>
</tr>
<tr>
<td style="text-align: center;">3</td>
<td>responseTime</td>
<td><p>String</p>
<p>(100 ký tự)</p></td>
<td style="text-align: center;">x</td>
<td style="text-align: left;"><p>Thời gian phản hồi kết quả</p>
<p>Định dạng: ddMMyyyyHHmmss</p></td>
</tr>
<tr>
<td style="text-align: center;">4</td>
<td>data</td>
<td>JsonObject</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Kết quả trả về dưới dạng Json Object</td>
</tr>
<tr>
<td style="text-align: center;"></td>
<td colspan="4">Mô tả cấu trúc JsonObject <strong>data</strong></td>
</tr>
<tr>
<td style="text-align: center;"><em>4.1</em></td>
<td><em>txnId</em></td>
<td><em>String</em></td>
<td style="text-align: center;"></td>
<td style="text-align: left;"><em>Mã giao dịch được trả về khi khởi tạo yêu cầu ban đầu thành công</em></td>
</tr>
<tr>
<td style="text-align: center;"><em>4.2</em></td>
<td><em>requestId</em></td>
<td></td>
<td style="text-align: center;"></td>
<td style="text-align: left;"><em>Tham chiếu mã yêu cầu do hệ thống của TCDN gửi sang Hệ thống nền tảng</em></td>
</tr>
<tr>
<td colspan="5" style="text-align: center;">Ứng dụng của TCDN sau khi nhận được mã giao dịch (txnId), tiến hành gọi Web/App VNeID theo URL được cung cấp dưới đây để chuyển hướng người dùng sang VNeID thực hiện Consent</td>
</tr>
<tr>
<td style="text-align: center;"></td>
<td>URL WEB</td>
<td colspan="3" style="text-align: left;">Dịch vụ này chỉ có trên APP VNeID không triển khai trên WEB VNeID</td>
</tr>
<tr>
<td style="text-align: center;"></td>
<td>URL APP</td>
<td colspan="3" style="text-align: left;"><p><em>Chi tiết tham chiếu mục III.4.4 Tài liệu này.</em></p>
<p>- Môi trường tích hợp: https://universal.dancuquocgia.com/share/[$transactionCode]</p>
<p>- Môi trường PRO: N/A</p></td>
</tr>
</tbody>
</table>

### III.7.2. API thông báo kết quả xác nhận chia sẻ/ không chia sẻ của tổ chức, cá nhân và các mã lỗi khác cho ứng dụng của TCDN. {#iii-7-2-api-thong-bao-ket-qua-to-chuc-ca-nhan}

#### API Specs

<table>
<colgroup>
<col style="width: 15%" />
<col style="width: 84%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>Thông tin</strong></th>
<th style="text-align: center;"><strong>Mô tả</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: left;">Mục đích</td>
<td style="text-align: left;">Thông báo kết quả xác nhận chia sẻ/ không chia sẻ của tổ chức, cá nhân và các mã lỗi khác cho ứng dụng của TCDN</td>
</tr>
<tr>
<td style="text-align: left;">URL</td>
<td style="text-align: left;">TCDN định nghĩa và cấu hình sẵn trong AgentGW</td>
</tr>
<tr>
<td style="text-align: left;">Method</td>
<td style="text-align: left;">POST</td>
</tr>
<tr>
<td style="text-align: left;">Header</td>
<td style="text-align: left;"><ul>
<li><p><strong>Content-Type:</strong> application/json; charset=utf-8</p></li>
<li><p><strong>api_key:</strong> API key //Do TCDN cung cấp.</p></li>
<li><p><strong>request_id:</strong> Mã yêu cầu do hệ thống Nền tảng tự sinh, là duy nhất trên hệ thống.</p></li>
<li><p><strong>request_time:</strong> Thời gian yêu cầu, định dạng ddMMyyyyHHmmss.</p></li>
<li><p><mark><strong>target_code:</strong> mã mục đích.</mark></p></li>
<li><p><mark><strong>service_code:</strong> mã dịch vụ.</mark></p></li>
<li><p><mark><strong>org_code:</strong> mã tổ chức.</mark></p></li>
</ul></td>
</tr>
<tr>
<td style="text-align: left;">Request body</td>
<td style="text-align: left;"><p>{</p>
<p>    "requestId": "3d60591a-315c-457e-b926-1b3ec699e0a9",</p>
<p>    "resultCode": "-4", //Tham chiếu bảng mã tại mục IV.4</p>
<p>    "resultDesc": "Giao dịch đang xử lý, vui lòng thử lại sau",</p>
<p>    "txnId": "22fc12e0-be90-4519-8ef6-cdeca95ae22b"",</p>
<p><mark>"data": {</mark></p>
<p><mark>        "feeCode": "",//Gói dịch vụ người dân chọn khi đăng ký, đối với luồng đăng ký từ VNeID</mark></p>
<p><mark>        "csr": "MIICsTCCAZkCAQAwb[…] "/// csr đọc từ sdk sinh khóa thiết bị</mark></p>
<p>    }</p>
<p>}</p></td>
</tr>
<tr>
<td style="text-align: left;">Response body</td>
<td style="text-align: left;"><p>{</p>
<p>    "responseCode": 1, //0: thành công, 1: không thành công</p>
<p>    "description": "Mô tả lỗi",</p>
<p>    "responseTime": "30092024190321" //ddMMyyyyHHmmss</p>
<p>}</p></td>
</tr>
</tbody>
</table>

#### Request Body

<table style="width:100%;">
<colgroup>
<col style="width: 7%" />
<col style="width: 14%" />
<col style="width: 11%" />
<col style="width: 9%" />
<col style="width: 56%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>STT</strong></th>
<th style="text-align: center;"><strong>Tên trường</strong></th>
<th style="text-align: center;"><strong>Kiểu dữ liệu</strong></th>
<th style="text-align: center;"><strong>Bắt buộc</strong></th>
<th style="text-align: center;"><strong>Mô tả</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: center;">1</td>
<td>requestId</td>
<td>String</td>
<td style="text-align: center;">x</td>
<td>Tham chiếu đến request_id trong API gửi yêu cầu khởi tạo ban đầu của TCDN</td>
</tr>
<tr>
<td style="text-align: center;">2</td>
<td>resultCode</td>
<td>String</td>
<td style="text-align: center;">x</td>
<td><p>Mã kết quả xử lý yêu cầu chia sẻ thông tin:</p>
<p><em>Tham chiếu mục IV.4 – Mã lỗi responseCode, resultCode</em></p>
<p>1: Đồng ý chia sẻ</p>
<p>2: Không đồng ý chia sẻ</p></td>
</tr>
<tr>
<td style="text-align: center;">3</td>
<td>resultDesc</td>
<td>String</td>
<td style="text-align: center;"></td>
<td><p>Mô tả chi tiết mã kết quả resultCode:</p>
<p>Tham chiếu mục IV.4 – Mã lỗi responseCode, resultCode</p></td>
</tr>
<tr>
<td style="text-align: center;">4</td>
<td>txnId</td>
<td>String</td>
<td style="text-align: center;">x</td>
<td>Mã giao dịch để sử dụng truy vấn thông tin địn danh tổ chức (trường hợp người đại diện pháp luật xác nhận đồng ý)</td>
</tr>
</tbody>
</table>

#### Response body

32. Ứng dụng của tổ chức doanh nghiệp trả về trạng thái với **httpstatus = 200 (OK)** cùng bản tin theo định dạng JSON.

<table>
<colgroup>
<col style="width: 7%" />
<col style="width: 17%" />
<col style="width: 16%" />
<col style="width: 10%" />
<col style="width: 46%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>STT</strong></th>
<th style="text-align: center;"><strong>Tên trường</strong></th>
<th style="text-align: center;"><strong>Kiểu dữ liệu</strong></th>
<th style="text-align: center;"><strong>Bắt buộc</strong></th>
<th style="text-align: center;"><strong>Ghi chú</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: center;">1</td>
<td>responseCode</td>
<td><p>String</p>
<p>(2 ký tự)</p></td>
<td style="text-align: center;">x</td>
<td style="text-align: left;"><p>Mã trạng thái tiếp nhận:</p>
<p>0: thành công</p>
<p>Khác 0: Lỗi (mô tả chi tiết tại trường description)</p>
<p><em>Tham chiếu mục IV.4, cụ thể:</em></p>
<p>2: Không đồng ý chia sẻ</p>
<p>1: Đồng ý chia sẻ</p>
<p>-1: Không lấy được thông tin</p>
<p>-15: Tồn tại trường dữ liệu bắt buộc bị “null”</p>
<p>-17: Thông tin định danh tổ chức không hợp lệ, vui lòng kiểm tra chi tiết trên ứng dụng VNeID</p>
<p>-20: Thông tin định danh tổ chức không đúng</p></td>
</tr>
<tr>
<td style="text-align: center;">2</td>
<td>description</td>
<td><p>String</p>
<p>(100 ký tự)</p></td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Mô tả lỗi trong trường hợp trạng thái tiếp nhận lỗi (responseCode khác 0)</td>
</tr>
<tr>
<td style="text-align: center;">3</td>
<td>responseTime</td>
<td><p>String</p>
<p>(100 ký tự)</p></td>
<td style="text-align: center;">x</td>
<td style="text-align: left;"><p>Thời gian phản hồi kết quả</p>
<p>Định dạng: ddMMyyyyHHmmss</p></td>
</tr>
</tbody>
</table>

### III.7.3. API yêu cầu trả kết quả dữ liệu định danh tổ chức, cá nhân {#iii-7-3-api-yeu-cau-tra-ket-qua-dinh-danh-to-chuc-ca-nhan}

**Lưu ý:**

- transactionId sẽ hết hạn sau 30 phút kể từ thời điểm ghi nhận thành công việc người đại diện pháp luật đã thực hiện xác nhận trên VNeID.

- TCDN chỉ có thể gọi lấy dữ liệu tối đa 03 lần trong khoảng thời gian 30 phút, quá thời gian trên sẽ không nhận được dữ liệu định danh tổ chức đã chia sẻ.

#### API Specs

<table>
<colgroup>
<col style="width: 18%" />
<col style="width: 81%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>Thông tin</strong></th>
<th style="text-align: center;"><strong>Mô tả</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: left;">Tên API</td>
<td style="text-align: left;">API cung cấp cho TCDN khai thác yêu cầu trả kết quả dữ liệu định danh tổ chức, cá nhân đồng ý chia sẻ</td>
</tr>
<tr>
<td style="text-align: left;">Mục đích</td>
<td style="text-align: left;">Cho phép TCDN gửi yêu cầu trả kết quả dữ liệu định danh tổ chức, cá nhân đồng ý chia sẻ</td>
</tr>
<tr>
<td style="text-align: left;">URL</td>
<td style="text-align: left;"><p>https://&lt;domain&gt;:&lt;port&gt;/api/tcdn<strong>/{service-code}</strong>/api/get-transaction</p>
<p><strong>{service-code}</strong> tham chiếu theo bảng Mã dịch vụ IV.3.</p></td>
</tr>
<tr>
<td style="text-align: left;">Method</td>
<td style="text-align: left;">POST</td>
</tr>
<tr>
<td style="text-align: left;">Header</td>
<td style="text-align: left;"><ul>
<li><p><strong>Content-Type:</strong> application/json; charset=utf-8</p></li>
<li><p><strong>api_key:</strong> API key</p></li>
<li><p><mark><strong>target_code:</strong> Mã mục đích chia sẻ, xử lý thông tin do Hệ thống định danh cung cấp, phục vụ hiển thị đúng nội dung mục đích chia sẻ, xử lý thông tin trên ứng dụng VNeID.</mark></p></li>
<li><p><strong>request_id:</strong> Mã yêu cầu do hệ thống của TCDN tự sinh, là duy nhất trên hệ thống.</p></li>
<li><p><strong>request_time:</strong> Thời gian yêu cầu, định dạng ddMMyyyyHHmmss. Là thời gian hiện tại của hệ thống, trong trường hợp request_time gửi sang Hệ thống NT lệch quá 2 phút so với thời gian hiện tại của Hệ thống nền tảng thì request đó được coi là timeout.</p></li>
</ul></td>
</tr>
<tr>
<td style="text-align: left;">Params</td>
<td style="text-align: left;"><p>Trên môi trường non-production:</p>
<p>code=mã đơn vị kết nối (do Đơn vị cung cấp dịch vụ cấp)</p>
<p>Trên môi trường prod: N/A</p></td>
</tr>
<tr>
<td style="text-align: left;">Request body</td>
<td style="text-align: left;"><p><strong>Dịch vụ chia sẻ thông tin định danh tổ chức, cá nhân</strong></p>
<p>{</p>
<p>    "transactionId":"9baf1668-a0da-4f9a-b9ac-5eef6f5da5f6",</p>
<p>}</p></td>
</tr>
<tr>
<td style="text-align: left;">Response body</td>
<td style="text-align: left;"><p><strong>Dịch vụ chia sẻ thông tin định danh tổ chức, cá nhân</strong></p>
<p>{</p>
<p>    "responseCode": 0, //0: thành công, khác 0: lỗi</p>
<p>    "description": "Mô tả lỗi nếu có",</p>
<p>    "responseTime": "ddMMyyyyHHmmss",</p>
<p>    "data": {</p>
<p>        "userInfo": "eyJhbGciOiJFUzI1NiIsInR5cCI6IkpPU0UvSldTIn0=.ewogICJjaXRpemVuUGlkIiA6ICIwMzYxODgwMDE5NTgiLAogICJmdWxsTmFtZSIgOiAiTmd1eeG7hW4gVGjhu4sgSHXhur8iLAogICJsaXZpbmdQbGFjZUNpdHlUZXh0IiA6ICJUaMOgbmggcGjhu5EgSMOgIE7hu5lpIiwKICAibGl2aW5nUGxhY2VEaXN0cmljdFRleHQiIDogIkh1eeG7h24gSG_DoGkgxJDhu6ljIiwKICAicGVybWFuZW50Q2l0eVRleHQiIDogIlRow6BuaCBwaOG7kSBIw6AgTuG7mWkiLAogICJwZXJtYW5lbnREaXN0cmljdFRleHQiIDogIkh1eeG7h24gSG_DoGkgxJDhu6ljIiwKICAicGVybWFuZW50VmlsbGFnZVRleHQiIDogIljDoyBBbiBLaMOhbmgiLAogICJsaXZpbmdQbGFjZVZpbGxhZ2VUZXh0IiA6ICJYw6MgQW4gS2jDoW5oIiwKICAicGVybWFuZW50Q2l0eUNvZGUiIDogIjAxIiwKICAicGVybWFuZW50RGlzdHJpY3RDb2RlIiA6ICIyNzQiLAogICJwZXJtYW5lbnRWaWxsYWdlQ29kZSIgOiAiMDk4NzciLAogICJwZXJtYW5lbnRBZGRyZXNzIiA6ICJLaHUgxJDDtCBUaOG7iyBIYWkgQsOqbiDEkMaw4budbmcgTMOqIFRy4buNbmcgVOG6pW4sIEFuIEtow6FuaCwgSG_DoGkgxJDhu6ljLCBIw6AgTuG7mWkiLAogICJsaXZpbmdQbGFjZUNpdHlDb2RlIiA6ICIwMSIsCiAgImxpdmluZ1BsYWNlRGlzdHJpY3RDb2RlIiA6ICIyNzQiLAogICJsaXZpbmdQbGFjZVZpbGxhZ2VDb2RlIiA6ICIwOTg3NyIsCiAgImxpdmluZ1BsYWNlQWRkcmVzcyIgOiAiS2h1IMSQw7QgVGjhu4sgSGFpIELDqm4gxJDGsOG7nW5nIEzDqiBUcuG7jW5nIFThuqVuLCBBbiBLaMOhbmgsIEhvw6BpIMSQ4bupYywgSMOgIE7hu5lpIiwKICAiZGF0ZU9mSXNzdWUiIDogIjIzLTA3LTIwMjQiLAogICJpZENhcmRFeHBpcmVEYXRlIiA6ICIzMC0xMi0yMDMwIiwKICAiaXNzdWluZ0F1dGhvcml0eSIgOiAiQ-G7pWMgQ-G6o25oIHPDoXQgcXXhuqNuIGzDvSBow6BuaCBjaMOtbmggduG7gSB0cuG6rXQgdOG7sSB4w6MgaOG7mWkiLAogICJiaXJ0aERhdGUiIDogIjI0LTA0LTE5ODgiLAogICJuYXRpb25hbGl0eUNvZGUiIDogIlZOIgp9.MEUCIBNqg0LXH7UtJvU_A3vRZLLR20iacQxVK7lT1YZTVTJhAiEAjdPapssFIwhylfs68ufjT7RlByIKtrbjVIjw60CJWnc="</p>
<p>        //userInfo: định dạng <mark>base64 encode</mark> tuân theo chuẩn JWS có format: &lt;header&gt;.&lt;payload&gt;.&lt;signature&gt;</p>
<p>        // Tổ chức khai thác có thể sử dụng các thư viện tiêu chuẩn để verify chữ ký (với public key được C06 cung cấp) và parse dữ liệu payload để có được thông tin công dân.</p>
<p>    }</p>
<p>}</p>
<p><strong>// Ví dụ minh họa đối với userInfo sau khi parse theo chuẩn JWS</strong></p>
<p><strong>// Cấu trúc &lt;payload&gt; :</strong> Lưu ý các trường thông tin có thể thay đổi tuỳ vào nhu cầu của TCDN cũng như sự đồng ý chia sẻ của công dân trên ứng dụng VNeID.</p>
<p>{</p>
<p>    "person":</p>
<p>    {</p>
<p>        "fullName": "Nguyễn Thị Huế",</p>
<p>        "birthDate": "24-04-1988",</p>
<p>        "nationalityCode": "VN",</p>
<p>        "citizenPid": "036188001958",</p>
<p>        "indentifyNumber": "654721832",</p>
<p>        "dateOfIssue": "23-07-2024",</p>
<p>        "issuingAuthority": "Cục Cảnh sát quản lý hành chính về trật tự xã hội",</p>
<p>        "genderCode": "3",</p>
<p>        "livingPlaceAddress": "Khu Đô Thị Hai Bên Đường Lê Trọng Tấn",</p>
<p>        "livingPlaceVillageCode": "09877",</p>
<p>        "livingPlaceDistrictCode": <strong>null</strong>,</p>
<p>        "livingPlaceCityCode": "01",</p>
<p>        "permanentAddress": "Khu Đô Thị Hai Bên Đường Lê Trọng Tấn",</p>
<p>        "permanentVillageCode": "09877",</p>
<p>        "permanentDistrictCode": <strong>null</strong>,</p>
<p>        "permanentCityCode": "01",</p>
<p>        "idCardExpireDate": "30-12-2030"</p>
<p>    },</p>
<p>    "org":</p>
<p>    {</p>
<p>        "orgPid": "Số định danh tổ chức",</p>
<p>        "orgName": "Tên doanh nghiệp",</p>
<p>        "orgShortName": "Tên viết tắt",</p>
<p>        "taxCode": "Mã số thuế, mã số doanh nghiệp, mã số hợp tác xã",</p>
<p>        "foundingDate": "Ngày thành lập định dạng dd/MM/yyyy",</p>
<p>        "addressProvinceCode": "Địa chỉ trụ sở chính mã tỉnh/tp của tổ chức",</p>
<p>        "addressProvinceText": "Địa chỉ trụ sở chính tên tỉnh/tp của tổ chức",</p>
<p>        "addressWardCode": "Địa chỉ trụ sở chính mã phường/xã của tổ chức",</p>
<p>        "addressWardText": "Địa chỉ trụ sở chính tên phường/xã của tổ chức",</p>
<p>        "addressDetail": "Địa chỉ chi tiết của tổ chức",</p>
<p>        "representCitizenPid": "Số định danh người đại điện tài khoản định danh tổ chức",</p>
<p>        "representFullName": "Tên định danh người đại điện tài khoản định danh tổ chức",</p>
<p>        "representPositionText": "Tên chức danh",</p>
<p>        "business": [ //danh sách ngành nghề kinh doanh</p>
<p>            {</p>
<p>                "code": "Mã ngành nghề kinh doanh",</p>
<p>                "name": "Tên ngành nghề kinh doanh"</p>
<p>            }</p>
<p>        ],</p>
<p>        "represents": [</p>
<p>            {</p>
<p>                "citizenPid": "Số định danh",</p>
<p>                "fullName": "Họ và tên",</p>
<p>                "positionText": "Tên chức danh"</p>
<p>            }</p>
<p>        ]</p>
<p>    }</p>
<p>}</p></td>
</tr>
</tbody>
</table>

#### Request body

| **STT** | **Tên trường** | **Kiểu dữ liệu** | **Bắt buộc** | **Mô tả**                                                            |
| :-----: | :------------: | :--------------: | :----------: | -------------------------------------------------------------------- |
|    1    | transactionId  |      String      |      x       | Mã giao dịch đã được trả về khi khởi tạo yêu cầu ban đầu thành công. |

#### Response body

33. Hệ thống nền tảng định danh trả về trạng thái với **httpstatus = 200 (OK)** cùng bản tin theo định dạng JSON.

<table>
<colgroup>
<col style="width: 9%" />
<col style="width: 20%" />
<col style="width: 15%" />
<col style="width: 9%" />
<col style="width: 45%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>STT</strong></th>
<th style="text-align: center;"><strong>Tên trường</strong></th>
<th style="text-align: center;"><strong>Kiểu dữ liệu</strong></th>
<th style="text-align: center;"><strong>Bắt buộc</strong></th>
<th style="text-align: center;"><strong>Ghi chú</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: center;">1</td>
<td>responseCode</td>
<td><p>String</p>
<p>(2 ký tự)</p></td>
<td style="text-align: center;">x</td>
<td style="text-align: left;"><p>Mã trạng thái tiếp nhận:</p>
<p>0: thành công</p>
<p>Khác 0: Lỗi (mô tả chi tiết tại trường description)</p>
<p><em>Tham chiếu mục IV.4, cụ thể:</em></p>
<p><em>-1: Không lấy được thông tin</em></p>
<p><em>-4: Giao dịch đang xử lý, vui lòng thử lại sau</em></p>
<p><em>-5: Đầu vào yêu cầu không hợp lệ!</em></p>
<p><em>-6: Không tìm thấy tổ chức</em></p>
<p><em>-7: Không tìm thấy dịch vụ</em></p>
<p><em>-8: Đường dẫn không hợp lệ</em></p>
<p><em>-9: Không tìm thấy mã giao dịch</em></p>
<p><em>-11: Không đồng ý chia sẻ thông tin</em></p>
<p><em>-12: Không tìm thấy cấu hình agent</em></p>
<p><em>-14: Timeout</em></p>
<p><em>-15: Tồn tại trường dữ liệu bắt buộc bị “null”</em></p>
<p><em>-16: Dịch vụ đang sử dụng vượt quá hạn mức cho phép</em></p>
<p><em>-17: Thông tin công dân không hợp lệ, vui lòng kiểm tra chi tiết trên ứng dụng VNeID</em></p>
<p><em>-19: Tài khoản tổ chức không có quyền sử dụng dịch vụ</em></p>
<p><em>-20: Thông tin định danh tổ chức không đúng</em></p></td>
</tr>
<tr>
<td style="text-align: center;">2</td>
<td>description</td>
<td><p>String</p>
<p>(100 ký tự)</p></td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Mô tả lỗi trường hợp trạng thái tiếp nhận khác 0</td>
</tr>
<tr>
<td style="text-align: center;">3</td>
<td>responseTime</td>
<td><p>String</p>
<p>(100 ký tự)</p></td>
<td style="text-align: center;">x</td>
<td style="text-align: left;"><p>Thời gian phản hồi kết quả</p>
<p>Định dạng: ddMMyyyyHHmmss</p></td>
</tr>
<tr>
<td style="text-align: center;">4</td>
<td>data</td>
<td>JsonObject</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;"><p>Kết quả xác thực sinh trắc học.</p>
<p>Tuân theo chuẩn JWS. Là chuỗi Base64 có format:</p>
<p>&lt;header&gt;.&lt;payload&gt;.&lt;signature&gt;</p></td>
</tr>
<tr>
<td style="text-align: center;"></td>
<td colspan="4" style="text-align: left;">Mô tả cấu trúc JsonObject <strong>data</strong></td>
</tr>
<tr>
<td style="text-align: center;">4.2.2</td>
<td>transId</td>
<td>String</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Là trường transId ở request body</td>
</tr>
<tr>
<td style="text-align: center;"></td>
<td colspan="4"><p><strong>Mô tả cấu trúc trường thông tin userInfo sau khi parse theo chuẩn JWS</strong></p>
<p><em><strong>Lưu ý:</strong> các trường thông tin có thể thay đổi tuỳ vào nhu cầu của TCDN cũng như sự đồng ý chia sẻ của tổ chức trên ứng dụng VNeID.</em></p></td>
</tr>
<tr>
<td colspan="5" style="text-align: center;"><strong>Thông tin công dân “person”</strong></td>
</tr>
<tr>
<td style="text-align: right;"></td>
<td>fullName</td>
<td>string (100 ký tự)</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Họ và tên đầy đủ của công dân</td>
</tr>
<tr>
<td style="text-align: right;"></td>
<td>birthDate</td>
<td>string (20 ký tự)</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Ngày tháng năm sinh của công dân, định dạng dd-MM-yyyy</td>
</tr>
<tr>
<td style="text-align: right;"></td>
<td>citizenPid</td>
<td>string (12 ký tự số)</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Số định danh/ CCCD của công dân</td>
</tr>
<tr>
<td style="text-align: right;"></td>
<td>dateOfIssue</td>
<td>string (20 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Ngày cấp số định danh/ CCCD của công dân</td>
</tr>
<tr>
<td style="text-align: right;"></td>
<td>issuingAuthority</td>
<td>string (100 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Nơi cấp định danh/ CCCD của công dân</td>
</tr>
<tr>
<td style="text-align: right;"></td>
<td>nationalityCode</td>
<td>string (2 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Mã quốc tịch của công dân</td>
</tr>
<tr>
<td style="text-align: right;"></td>
<td>permanentAddress</td>
<td>string (200 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Địa chỉ thường trú (chi tiết) của công dân</td>
</tr>
<tr>
<td style="text-align: right;"></td>
<td>permanentVillageCode</td>
<td>string (20 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Mã xã thường trú (Ref danh mục xã)</td>
</tr>
<tr>
<td style="text-align: right;"></td>
<td>permanentVillageText</td>
<td>string (100 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Tên xã thường trú</td>
</tr>
<tr>
<td style="text-align: right;"></td>
<td>permanentDistrictCode</td>
<td>string (20 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Mã huyện thường trú (Ref danh mục huyện)</td>
</tr>
<tr>
<td style="text-align: right;"></td>
<td>permanentDistrictText</td>
<td>string (100 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Tên huyện thường trú</td>
</tr>
<tr>
<td style="text-align: right;"></td>
<td>permanentCityCode</td>
<td>string (20 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Mã tỉnh thường trú (Ref danh mục tỉnh)</td>
</tr>
<tr>
<td style="text-align: right;"></td>
<td>permanentCityText</td>
<td>string (100 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Tên tỉnh thường trú</td>
</tr>
<tr>
<td style="text-align: right;"></td>
<td>livingPlaceAddress</td>
<td>string (200 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Địa chỉ nơi ở hiện tại (chi tiết) của công dân</td>
</tr>
<tr>
<td style="text-align: right;"></td>
<td>livingPlaceVillageCode</td>
<td>string (20 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Mã xã nơi ở hiện tại (Ref danh mục xã)</td>
</tr>
<tr>
<td style="text-align: right;"></td>
<td>livingPlaceVillageText</td>
<td>string (100 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Tên xã nơi ở hiện tại</td>
</tr>
<tr>
<td style="text-align: right;"></td>
<td>livingPlaceDistrictCode</td>
<td>string (20 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Mã huyện nơi ở hiện tại (Ref danh mục huyện)</td>
</tr>
<tr>
<td style="text-align: right;"></td>
<td>livingPlaceDistrictText</td>
<td>string (100 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Tên huyện nơi ở hiện tại</td>
</tr>
<tr>
<td style="text-align: right;"></td>
<td>livingPlaceCityCode</td>
<td>string (20 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Mã tỉnh nơi ở hiện tại (Ref danh mục tỉnh)</td>
</tr>
<tr>
<td style="text-align: right;"></td>
<td>livingPlaceCityText</td>
<td>string (100 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Tên tỉnh nơi ở hiện tại</td>
</tr>
<tr>
<td style="text-align: right;"></td>
<td>idCardExpireDate</td>
<td>String</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Ngày hết hạn của CCCD (mới nhất đến thời điểm chia sẻ)</td>
</tr>
<tr>
<td style="text-align: right;"></td>
<td>genderCode</td>
<td>String (10 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;"><p>Mã giới tính của công dân</p>
<p>Tham chiếu mục IV.5.1 Mã danh mục giới tính</p></td>
</tr>
<tr>
<td style="text-align: right;"></td>
<td>indentifyNumber</td>
<td>string (20 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Số CMND của công dân</td>
</tr>
<tr>
<td colspan="5" style="text-align: center;"><strong>Thông tin định danh tổ chức “org”</strong></td>
</tr>
<tr>
<td style="text-align: right;"></td>
<td>orgPid</td>
<td>String (12 ký tự số)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Số định danh tổ chức</td>
</tr>
<tr>
<td style="text-align: right;"></td>
<td>orgName</td>
<td>String (100 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Tên doanh nghiệp</td>
</tr>
<tr>
<td style="text-align: right;"></td>
<td>orgShortName</td>
<td>String (50 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Tên viết tắt</td>
</tr>
<tr>
<td style="text-align: right;"></td>
<td>taxCode</td>
<td>String (15 ký tự số)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Mã số thuế, mã số doanh nghiệp, mã số hợp tác xã</td>
</tr>
<tr>
<td style="text-align: right;"></td>
<td>foundingDate</td>
<td>String (10 ký tự số)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Ngày thành lập định dạng dd/MM/yyyy</td>
</tr>
<tr>
<td style="text-align: right;"></td>
<td>addressProvinceCode</td>
<td>String (20 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Địa chỉ trụ sở chính mã tỉnh/tp của tổ chức</td>
</tr>
<tr>
<td style="text-align: right;"></td>
<td>addressProvinceText</td>
<td>String (100 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Địa chỉ trụ sở chính tên tỉnh/tpcủa tổ chức</td>
</tr>
<tr>
<td style="text-align: right;"></td>
<td>addressWardCode</td>
<td>String (20 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Địa chỉ trụ sở chính mã phường/xã của tổ chức</td>
</tr>
<tr>
<td style="text-align: right;"></td>
<td>addressWardText</td>
<td>String (100 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Địa chỉ trụ sở chính tên phường/xã của tổ chức</td>
</tr>
<tr>
<td style="text-align: right;"></td>
<td>addressDetail</td>
<td>String (100 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Địa chỉ chi tiết của tổ chức</td>
</tr>
<tr>
<td style="text-align: right;"></td>
<td>representCitizenPid</td>
<td>String (12 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Số định danh người đại điện tài khoản định danh tổ chức</td>
</tr>
<tr>
<td style="text-align: right;"></td>
<td>representFullName</td>
<td>String (12 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;"><p>Tên định danh người đại điện tài khoản </p>
<p>định danh tổ chức</p></td>
</tr>
<tr>
<td style="text-align: right;"></td>
<td>representPositionText</td>
<td>String (20 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Tên chức danh</td>
</tr>
<tr>
<td style="text-align: right;"></td>
<td>business</td>
<td>array</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">danh sách ngành nghề kinh doanh</td>
</tr>
<tr>
<td style="text-align: right;"></td>
<td>code</td>
<td>String (12 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Mã ngành nghề kinh doanh</td>
</tr>
<tr>
<td style="text-align: right;"></td>
<td>name</td>
<td>String (100 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Tên ngành nghề kinh doanh</td>
</tr>
<tr>
<td style="text-align: right;"></td>
<td>represents</td>
<td>array</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Danh sách đại diện pháp luật của tổ chức</td>
</tr>
<tr>
<td style="text-align: right;"></td>
<td>citizenPid</td>
<td>String (12 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Số định danh</td>
</tr>
<tr>
<td style="text-align: right;"></td>
<td>fullName</td>
<td>String (100 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Họ và tên</td>
</tr>
<tr>
<td style="text-align: right;"></td>
<td>positionText</td>
<td>String (100 ký tự)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;">Tên chức danh</td>
</tr>
</tbody>
</table>

### III.7.4. Universal Link {#iii-7-4-universal-link}

#### Universal Link ứng dụng VNeID

- **Cấu trúc:** UniversalLink/share/\[\$transactionCode\]

- UniversalLink:
  - Môi trường tích hợp: https://universal.dancuquocgia.com/share/\[\$transactionCode\]

  - Môi trường Prod: N/A

- \[\$transactionCode\] : mã giao dịch xin consent

#### RedirectUrl tổ chức sử dụng dịch vụ khai báo cho ứng dụng VNeID

- **Cấu trúc:** URL_TCDN/\[\$transactionCode\]\|\[\$result\]

- **Mô tả:**

<table>
<colgroup>
<col style="width: 10%" />
<col style="width: 35%" />
<col style="width: 54%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>STT</strong></th>
<th style="text-align: center;"><strong>Code</strong></th>
<th style="text-align: center;"><strong>Mô tả</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: center;">1</td>
<td style="text-align: left;">URL_TCDN</td>
<td>Tổ chức sử dụng dịch vụ cung cấp</td>
</tr>
<tr>
<td style="text-align: center;">2</td>
<td style="text-align: left;">[$transactionCode]</td>
<td>Mã giao dịch xin consent</td>
</tr>
<tr>
<td style="text-align: center;">3</td>
<td style="text-align: left;">[$result]</td>
<td><p>Kết quả:</p>
<p>1: Đã đồng ý; 2: Không đồng ý; 3: Hết hạn</p></td>
</tr>
</tbody>
</table>

### III.7.5. <span class="mark">API thông báo kết quả đăng ký chứng thư số cho CA.</span> {#iii-7-5-api-thong-bao-dang-ky-chung-thu-so}

#### <span class="mark">API Specs</span>

<table>
<colgroup>
<col style="width: 15%" />
<col style="width: 84%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong><mark>Thông tin</mark></strong></th>
<th style="text-align: center;"><strong><mark>Mô tả</mark></strong></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: left;"><mark>Mục đích</mark></td>
<td style="text-align: left;"><mark>API thông báo kết quả đăng ký chứng thư chữ ký số</mark></td>
</tr>
<tr>
<td style="text-align: left;"><mark>URL</mark></td>
<td style="text-align: left;">/api/tcdn/{service-code}/agent/account/receive-result</td>
</tr>
<tr>
<td style="text-align: left;"><mark>Method</mark></td>
<td style="text-align: left;"><mark>POST</mark></td>
</tr>
<tr>
<td style="text-align: left;"><mark>Header</mark></td>
<td style="text-align: left;"><ul>
<li><p><strong>Content-Type:</strong> application/json; charset=utf-8</p></li>
<li><p><strong>api_key:</strong> API key</p></li>
<li><p><mark><strong>target_code:</strong> Mã mục đích chia sẻ, xử lý thông tin do Hệ thống định danh cung cấp, phục vụ hiển thị đúng nội dung mục đích chia sẻ, xử lý thông tin trên ứng dụng VNeID.</mark></p></li>
<li><p><strong>request_id:</strong> Mã yêu cầu do hệ thống của TCDN tự sinh, là duy nhất trên hệ thống.</p></li>
</ul>
<p><strong>request_time:</strong> Thời gian yêu cầu, định dạng ddMMyyyyHHmmss. Là thời gian hiện tại của hệ thống, trong trường hợp request_time gửi sang Hệ thống nền tảng lệch quá 2 phút so với thời gian hiện tại của Hệ thống nền tảng thì request đó được coi là timeout.</p></td>
</tr>
<tr>
<td style="text-align: left;"><mark>Request body</mark></td>
<td style="text-align: left;"><p><mark>Trả kết quả thành công</mark></p>
<p>{</p>
<p>    "transactionCode": "Mã giao dịch",</p>
<p>    "requestStatus": "SUCCESS", // SHARED: Đã lấy dữ liệu, SUCCESS: Đăng ký thành công, Chờ kích hoạt; FAILURE: Thất bại - NỀN TẢNG nhận kết quả từ  bên thứ 3 rồi trả về các trạng thái này cho VNeID</p>
<p>    "feeCode": "17116",</p>
<p>    "citizenNumber": "Số định danh thuê bao",</p>
<p>    "fullName": "Họ và tên thuê bao",</p>
<p>    "orgPid": "Số định danh tổ chức",</p>
<p>    "orgName": "Tên doanh nghiệp",</p>
<p>    "taxCode": "Mã số thuế, mã số doanh nghiệp, mã số hợp tác xã",</p>
<p>    "orgCode": "Tên tổ chức quản lý thuê bao",</p>
<p>    "province": "Tên tỉnh/TP thuê bao",</p>
<p>    "nation": "Tên nước",</p>
<p>    "serialNumber": "C28A39C5-3115-430F-B294-6758160E630C",</p>
<p>    "expirationDateFrom": "20/06/2022",</p>
<p>    "expirationDateTo": "20/12/2022",</p>
<p>    "expirationTimeActivate": "ddMMyyyyHHmmss", //Thời gian hết hạn kích hoạt</p>
<p>    "redirectUrl": "Link để redirect sau khi kích hoạt thành công",</p>
<p>    "type": "PER", //Loại ký số: PER: cá nhân, ORG: tổ chức, PER-ORG: cá nhân trong tổ chức</p>
<p>    "status": "SUCCESS" //Trường hợp mà không cần kích hoạt truyền thêm SUCCESS</p>
<p>}</p>
<p><mark>Trả kết quả thất bại</mark></p>
<p>{</p>
<p>"transactionCode": "Mã giao dịch",</p>
<p>"requestStatus": "FAILURE", // SHARED: Đã lấy dữ liệu, SUCCESS: Thành công, FAILURE: Thất bại</p>
<p>"reasonRequestCode": "Mã lý do từ chối thống nhất giữa VNeID và Tổ chức doanh nghiệp",</p>
<p>"reasonRequestText": "Nội dung lỗi do Tổ chức doanh nghiệp trả về nếu chưa có quy định về mã lý do"</p>
<p>}</p></td>
</tr>
<tr>
<td style="text-align: left;"><mark>Response body</mark></td>
<td style="text-align: left;"><p>Thành công</p>
<p>{</p>
<p>    "responseCode": 0, //0: thành công, Khác 0: không thành công</p>
<p>    "description": "Thành công",</p>
<p>    "responseTime": "30092024190321", //ddMMyyyyHHmmss</p>
<p>    "data": null</p>
<p>}</p>
<p>Không thành công</p>
<p>{</p>
<p>    "responseCode": 1, //0: thành công, Khác 0: không thành công</p>
<p>    "description": "Mô tả lỗi",</p>
<p>    "responseTime": "30092024190321", //ddMMyyyyHHmmss</p>
<p>    "data": ""</p>
<p>}</p></td>
</tr>
</tbody>
</table>

#### <span class="mark">Request Body</span>

<table>
<colgroup>
<col style="width: 7%" />
<col style="width: 28%" />
<col style="width: 16%" />
<col style="width: 12%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong><mark>STT</mark></strong></th>
<th style="text-align: center;"><strong><mark>Tên trường</mark></strong></th>
<th style="text-align: center;"><strong><mark>Kiểu dữ liệu</mark></strong></th>
<th style="text-align: center;"><strong><mark>Bắt buộc</mark></strong></th>
<th style="text-align: center;"><strong><mark>Mô tả</mark></strong></th>
</tr>
</thead>
<tbody>
<tr>
<td><ol type="1">
<li></li>
</ol></td>
<td>transactionCode</td>
<td>String</td>
<td style="text-align: center;"><mark>x</mark></td>
<td>Mã giao dịch đã được trả về khi khởi tạo yêu cầu ban đầu thành công.</td>
</tr>
<tr>
<td><ol start="2" type="1">
<li></li>
</ol></td>
<td>requestStatus</td>
<td>String</td>
<td style="text-align: center;"><mark>x</mark></td>
<td><p>Mã kết quả xử lý yêu cầu</p>
<p>SHARED: Đã lấy dữ liệu,</p>
<p>SUCCESS: Đăng ký thành công, Chờ kích hoạt;</p>
<p>FAILURE: Thất bại - NỀN TẢNG nhận kết quả từ bên thứ 3 rồi trả về các trạng thái này cho VNeID</p></td>
</tr>
<tr>
<td><ol start="3" type="1">
<li></li>
</ol></td>
<td>feeCode</td>
<td>String</td>
<td style="text-align: center;"></td>
<td><mark>Mã gói dịch vụ</mark></td>
</tr>
<tr>
<td><ol start="4" type="1">
<li></li>
</ol></td>
<td>citizenNumber</td>
<td>String</td>
<td style="text-align: center;"></td>
<td>Số định danh thuê bao</td>
</tr>
<tr>
<td><ol start="5" type="1">
<li></li>
</ol></td>
<td>fullName</td>
<td>String</td>
<td style="text-align: center;"></td>
<td>Họ và tên thuê bao</td>
</tr>
<tr>
<td><ol start="6" type="1">
<li></li>
</ol></td>
<td>orgPid</td>
<td>String</td>
<td style="text-align: center;"></td>
<td>Số định danh tổ chức</td>
</tr>
<tr>
<td><ol start="7" type="1">
<li></li>
</ol></td>
<td>orgName</td>
<td>String</td>
<td style="text-align: center;"></td>
<td>Tên doanh nghiệp</td>
</tr>
<tr>
<td><ol start="8" type="1">
<li></li>
</ol></td>
<td>taxCode</td>
<td>String</td>
<td style="text-align: center;"></td>
<td>Mã số thuế, mã số doanh nghiệp, mã số hợp tác xã</td>
</tr>
<tr>
<td><ol start="9" type="1">
<li></li>
</ol></td>
<td>orgCode</td>
<td>String</td>
<td style="text-align: center;"></td>
<td>Tên tổ chức quản lý thuê bao</td>
</tr>
<tr>
<td><ol start="10" type="1">
<li></li>
</ol></td>
<td>province</td>
<td>String</td>
<td style="text-align: center;"></td>
<td>Tên tỉnh/TP thuê bao</td>
</tr>
<tr>
<td><ol start="11" type="1">
<li></li>
</ol></td>
<td>nation</td>
<td>String</td>
<td style="text-align: center;"></td>
<td>Tên nước</td>
</tr>
<tr>
<td><ol start="12" type="1">
<li></li>
</ol></td>
<td>serialNumber</td>
<td>String</td>
<td style="text-align: center;"></td>
<td><mark>Serialnumber của chứng thư chữ ký số</mark></td>
</tr>
<tr>
<td><ol start="13" type="1">
<li></li>
</ol></td>
<td>expirationDateFrom</td>
<td>String</td>
<td style="text-align: center;"></td>
<td><mark>Thời gian bắt đầu hiệu lực của chứng thư</mark></td>
</tr>
<tr>
<td><ol start="14" type="1">
<li></li>
</ol></td>
<td>expirationDateTo</td>
<td>String</td>
<td style="text-align: center;"></td>
<td><mark>Thời gian kết thúc hiệu lực của chứng thư</mark></td>
</tr>
<tr>
<td><ol start="15" type="1">
<li></li>
</ol></td>
<td>expirationTimeActivate</td>
<td>String</td>
<td style="text-align: center;"></td>
<td><mark>Thời gian hết hạn kích hoạt chứng thư</mark></td>
</tr>
<tr>
<td><ol start="16" type="1">
<li></li>
</ol></td>
<td>redirectUrl</td>
<td>String</td>
<td style="text-align: center;"></td>
<td>Link để redirect sau khi kích hoạt thành công</td>
</tr>
<tr>
<td><ol start="17" type="1">
<li></li>
</ol></td>
<td>type</td>
<td>String</td>
<td style="text-align: center;"></td>
<td><p>Loại chứng thư chữ ký số: PER: cá nhân</p>
<p>ORG: tổ chức</p>
<p>PER-ORG: cá nhân trong tổ chức</p></td>
</tr>
<tr>
<td><ol start="18" type="1">
<li></li>
</ol></td>
<td>status</td>
<td>String</td>
<td style="text-align: center;"></td>
<td>Trường hợp mà không cần kích hoạt chứng thư truyền thêm SUCCESS</td>
</tr>
<tr>
<td><ol start="19" type="1">
<li></li>
</ol></td>
<td>requestStatus</td>
<td>number (1)</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;"><p>Mã định nghĩa kết quả của yêu cầu</p>
<p>SHARED: Đã lấy dữ liệu,</p>
<p>SUCCESS: Thành công,</p>
<p>FAILURE: Thất bại</p></td>
</tr>
<tr>
<td><ol start="20" type="1">
<li></li>
</ol></td>
<td>reasonRequestCode</td>
<td>string (100)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;"><p>Mã lý do từ chối.</p>
<p>Bắt buộc khi requestStatus = REJECT</p></td>
</tr>
<tr>
<td><ol start="21" type="1">
<li></li>
</ol></td>
<td>reasonRequestText</td>
<td>string (1000)</td>
<td style="text-align: center;"></td>
<td style="text-align: left;"><p>Chi tiết nội dung từ chối</p>
<p>Bắt buộc khi reasonRequestCode = null</p></td>
</tr>
</tbody>
</table>

#### <span class="mark">Response body</span>

34. <span class="mark">Ứng dụng của tổ chức doanh nghiệp trả về trạng thái với **httpstatus = 200 (OK)** cùng bản tin theo định dạng JSON.</span>

<table>
<colgroup>
<col style="width: 5%" />
<col style="width: 17%" />
<col style="width: 14%" />
<col style="width: 11%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>TT</strong></th>
<th style="text-align: center;"><strong>Tên trường</strong></th>
<th style="text-align: center;"><strong>Định dạng</strong></th>
<th style="text-align: center;"><strong>Bắt buộc</strong></th>
<th style="text-align: center;"><strong>Mô tả</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: center;">1</td>
<td>responseCode</td>
<td><p>String</p>
<p>(2 ký tự)</p></td>
<td style="text-align: center;">x</td>
<td style="text-align: left;"><p>Mã trạng thái tiếp nhận:</p>
<p>0: thành công</p>
<p>Khác 0: Lỗi (mô tả chi tiết tại trường description)</p></td>
</tr>
<tr>
<td style="text-align: center;">2</td>
<td>description</td>
<td><p>String</p>
<p>(100 ký tự)</p></td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Mô tả lỗi trường hợp trạng thái tiếp nhận khác 0</td>
</tr>
<tr>
<td style="text-align: center;">3</td>
<td>responseTime</td>
<td><p>String</p>
<p>(100 ký tự)</p></td>
<td style="text-align: center;">x</td>
<td style="text-align: left;"><p>Thời gian phản hồi kết quả</p>
<p>Định dạng: ddMMyyyyHHmmss</p></td>
</tr>
<tr>
<td style="text-align: center;">4</td>
<td>data</td>
<td>JsonObject</td>
<td style="text-align: center;">x</td>
<td style="text-align: left;">Kết quả trả về dưới dạng Json Object</td>
</tr>
</tbody>
</table>

# IV. BẢNG MÃ DÙNG CHUNG TRONG BẢN TIN API {#iv-bang-ma-dung-chung}

## IV.1. Mã lỗi đáp ứng HTTP (HTTP Status Codes) {#iv-1-ma-loi-http-status-codes}

| **STT** | **Mã lỗi** | **Kiểu lỗi**          | **Nội dung**                                                                       |
| :-----: | :--------: | :-------------------- | :--------------------------------------------------------------------------------- |
|    1    |    200     | SUCCESS               | Yêu cầu thực hiện thành công                                                       |
|    2    |    401     | ACCESS_TOKEN          | Token hết hạn (nếu có)                                                             |
|    3    |    403     | FORBIDDEN             | Không có quyền truy cập dịch vụ                                                    |
|    4    |    404     | NOT_FOUND             | Địa chỉ yêu cầu không tồn tại                                                      |
|    5    |    500     | INTERNAL SERVER ERROR | Xảy ra lỗi trong quá trình xử lý. Hệ thống thông tin chủ động đưa ra lỗi chi tiết. |
|    6    |    501     | BIOMETRIC ERROR       | Lỗi xác thực sinh trắc học. Hệ thống thông tin chủ động đưa ra lỗi chi tiết.       |

## IV.2. Mã lỗi dịch vụ {#iv-2-ma-loi-dich-vu}

| **ErrCode** | **Ghi chú**               |
| :---------: | :------------------------ |
|    1000     | Lỗi trong quá trình xử lý |
|    1001     | Yêu cầu trùng lặp         |
|    1002     | Sai định dạng dữ liệu     |

## IV.3. Mã dịch vụ {#iv-3-ma-dich-vu}

| **STT** | **Service-code**        | **Mô tả**                                                                                    |
| :-----: | :---------------------- | :------------------------------------------------------------------------------------------- |
|    1    | sso-token               | Lấy token VNeID                                                                              |
|    2    | sso-userinfo            | Lấy thông tin người dùng khi đã có token VNeID                                               |
|    3    | share-info              | Dịch vụ chia sẻ thông tin định danh điện tử                                                  |
|    4    | face-matching           | Dịch vụ xác thực ảnh mặt                                                                     |
|    5    | biometric-share-info    | Dịch vụ chia sẻ thông tin định danh điện tử kèm xác thực ảnh mặt                             |
|    6    | verify-info             | Dịch vụ xác thực thông tin công dân                                                          |
|    7    | create-account          | Dịch vụ mở tài khoản ngân hàng từ VNeID                                                      |
|    8    | create-credit-card      | Dịch vụ đăng ký thẻ tín dụng từ VNeID                                                        |
|    9    | create-ewallet-account  | Dịch vụ mở tài khoản ví điện tử từ VNeID                                                     |
|   10    | create-account-result   | Dịch vụ trả kết quả mở tài khoản ngân hàng/ mở tài khoản ví điện tử/ đăng ký thẻ tín dụng    |
|   11    | billing-history         | Dịch vụ trả thông tin hóa đơn về VNeID                                                       |
|   12    | create-smartca-account  | Dịch vụ mở chứng thư số SMARTCA                                                              |
|   13    | create-sim              | Dịch vụ đăng ký SIM di động                                                                  |
|   14    | buy-medicine-history    | Dịch vụ trả thông tin hóa đơn thuốc về VNeID                                                 |
|   15    | withdraw-consent        | Yêu cầu rút lại sự đồng ý chia sẻ, yêu cầu xoá dữ liệu công dân đã đồng ý chia sẻ (từ VNeID) |
|   16    | withdraw-consent-result | TCDN trả kết quả xử lý yêu cầu xóa dữ liệu công dân đã đồng ý chia sẻ                        |
|   17    | payment-result          | Dịch vụ trả kết quả thanh toán từ VNPAY về VNeID                                             |
|   18    | vnpay-querydr           | Dịch vụ yêu cầu tra cứu trạng thái thanh toán sang VNPAY                                     |

## IV.4. Mã responseCode, resultCode {#iv-4-ma-responsecode-resultcode}

<table>
<colgroup>
<col style="width: 13%" />
<col style="width: 36%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>Code</strong></th>
<th style="text-align: center;"><strong>Lỗi</strong></th>
<th style="text-align: center;"><strong>Mô tả</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: center;">2</td>
<td style="text-align: left;">Không đồng ý chia sẻ</td>
<td style="text-align: left;"><p>Công dân lựa chọn “Không đồng ý chia sẻ” trên ứng dụng VNeID.</p>
<p>Mã này trả về qua API callback do TCDN cung cấp ở mục III.4.2</p></td>
</tr>
<tr>
<td style="text-align: center;">1</td>
<td style="text-align: left;">Đồng ý chia sẻ</td>
<td style="text-align: left;">Công dân lựa chọn “Đồng ý chia sẻ” trên ứng dụng VNeID.</td>
</tr>
<tr>
<td style="text-align: center;">0</td>
<td style="text-align: left;">Thành công</td>
<td style="text-align: left;">Giao dịch thực hiện thành công.</td>
</tr>
<tr>
<td style="text-align: center;">-1</td>
<td style="text-align: left;">Không lấy được thông tin</td>
<td style="text-align: left;">Giao dịch bị lỗi, không thực hiện được.</td>
</tr>
<tr>
<td style="text-align: center;">-2</td>
<td style="text-align: left;">Giao dịch đã tồn tại</td>
<td style="text-align: left;"><p>Mã lỗi trao đổi nội bộ giữa Nền tảng và VNeID trong trường hợp cùng một mã giao dịch được gửi nhiều lần sang VNeID.</p>
<p><em>TCDN không cần xử lý mã lỗi này.</em></p></td>
</tr>
<tr>
<td style="text-align: center;">-3</td>
<td style="text-align: left;">Giao dịch đã hoàn thành</td>
<td style="text-align: left;"><p>Mã lỗi trao đổi giữa Nền tảng và VNeID trong trường hợp mã giao dịch được gửi sang VNeID thành công.</p>
<p><em>TCDN không cần xử lý mã lỗi này.</em></p></td>
</tr>
<tr>
<td style="text-align: center;">-4</td>
<td style="text-align: left;">Giao dịch đang xử lý, vui lòng thử lại sau</td>
<td style="text-align: left;"><p>Giao dịch đang được xử lý.</p>
<p><em>Giao dịch đang chờ công dân xác nhận chia sẻ. TCDN chờ kết quả được trả qua API callback hoặc chờ hết thời gian công dân xác nhận chia sẻ để thực hiện lại.</em></p></td>
</tr>
<tr>
<td style="text-align: center;">-5</td>
<td style="text-align: left;">Đầu vào yêu cầu không hợp lệ!</td>
<td style="text-align: left;"><p>Thông tin đầu vào của API không hợp lệ.</p>
<p><em>TCDN cần kiểm tra lại đầu vào của API.</em></p></td>
</tr>
<tr>
<td style="text-align: center;">-6</td>
<td style="text-align: left;">Không tìm thấy tổ chức</td>
<td style="text-align: left;"><p>Không tìm thấy tổ chức gửi yêu cầu.</p>
<p><em>TCDN cần liên hệ để kiểm tra lại thông tin tổ chức, thông tin dịch vụ, cấu hình trước khi thực hiện giao dịch khác.</em></p></td>
</tr>
<tr>
<td style="text-align: center;">-7</td>
<td style="text-align: left;">Không tìm thấy dịch vụ</td>
<td style="text-align: left;"><p>Không tìm thấy dịch vụ hoặc tổ chức gửi yêu cầu không được cấu hình dịch vụ.</p>
<p><em>TCDN cần liên hệ để kiểm tra lại thông tin tổ chức, thông tin dịch vụ, cấu hình trước khi thực hiện giao dịch khác.</em></p></td>
</tr>
<tr>
<td style="text-align: center;">-8</td>
<td style="text-align: left;">Đường dẫn không hợp lệ</td>
<td style="text-align: left;"><p>Đường dẫn API không hợp lệ.</p>
<p><em>TCDN cần kiểm tra lại đường dẫn API trước khi thực hiện giao dịch khác.</em></p></td>
</tr>
<tr>
<td style="text-align: center;">-9</td>
<td style="text-align: left;">Không tìm thấy mã giao dịch</td>
<td style="text-align: left;"><p>Không tìm thấy mã giao dịch trong yêu cầu.</p>
<p><em>TCDN cần kiểm tra lại mã giao dịch (transactionId/ consentId) trước khi thực hiện lại giao dịch.</em></p></td>
</tr>
<tr>
<td style="text-align: center;">-10</td>
<td style="text-align: left;">Giao dịch đã hoàn tất xác nhận chia sẻ</td>
<td style="text-align: left;"><p>Mã lỗi trao đổi giữa Nền tảng và VNeID trong trường hợp mã giao dịch đã được công dân xác nhận chia sẻ thành công trên VNeID.</p>
<p><em>TCDN không cần xử lý mã lỗi này.</em></p></td>
</tr>
<tr>
<td style="text-align: center;">-11</td>
<td style="text-align: left;">Công dân không đồng ý chia sẻ thông tin</td>
<td style="text-align: left;"><p>Công dân không đồng ý chia sẻ thông tin trên VNeID.</p>
<p>Mã này tương tự mã 2, nhưng trả về trong response body khi TCDN gửi yêu cầu lấy thông tin công dân (get-transaction) ở mục III.4.3 và III.4.4.</p>
<p><em>TCDN cần khởi tạo giao dịch khác để yêu cầu chia sẻ thông tin công dân và xác thực sinh trắc học (nếu cần).</em></p></td>
</tr>
<tr>
<td style="text-align: center;">-12</td>
<td style="text-align: left;">Không tìm thấy cấu hình agent</td>
<td style="text-align: left;"><p>Không tìm thấy cấu hình agent.</p>
<p><em>TCDN cần liên hệ để kiểm tra cấu hình Agent trước khi thực hiện giao dịch khác.</em></p></td>
</tr>
<tr>
<td style="text-align: center;">-14</td>
<td style="text-align: left;">Timeout</td>
<td style="text-align: left;"><p>Mã dịch vụ hết thời gian chờ xác nhận chia sẻ hoặc hết thời gian yêu cầu trả kết quả.</p>
<p><em>TCDN cần khởi tạo giao dịch khác để yêu cầu chia sẻ thông tin công dân và xác thực sinh trắc học (nếu cần).</em></p></td>
</tr>
<tr>
<td style="text-align: center;">-15</td>
<td style="text-align: left;">Tồn tại trường dữ liệu bắt buộc bị “null”</td>
<td style="text-align: left;"><p>Đối với dịch vụ được cấu hình các trường bắt buộc mà khi công dân chia sẻ lại tồn tại một trường bắt buộc không có giá trị (null).</p>
<p><em>TCDN không nên khởi tạo giao dịch với cùng dịch vụ và thông tin công dân trên, có thể hướng dẫn công dân kiểm tra lại thông tin hoặc liên hệ hỗ trợ kiểm tra cấu hình các trường bắt buộc (nếu cần).</em></p></td>
</tr>
<tr>
<td style="text-align: center;">-16</td>
<td style="text-align: left;">Dịch vụ đang sử dụng vượt quá hạn mức cho phép</td>
<td style="text-align: left;"><p>Trường hợp doanh nghiệp sử dụng dịch vụ vượt quá số lượng giao dịch đã đăng ký với Tổ chức cung cấp dịch vụ hoặc thời gian sử dụng dịch vụ nằm ngoài khoảng thời gian dịch vụ có hiệu lực.</p>
<p><em>TCDN cần liên hệ để gia hạn dịch vụ (thời gian sử dụng + giới hạn truy vấn) trước khi thực hiện giao dịch khác.</em></p></td>
</tr>
<tr>
<td style="text-align: center;">-17</td>
<td style="text-align: left;">Thông tin công dân không hợp lệ, vui lòng kiểm tra chi tiết trên ứng dụng VNeID</td>
<td style="text-align: left;"><p>Lỗi trả về trong trường hợp thông tin công dân không tồn tại, tài khoản định danh công dân ở mức 1, CCCD hết hạn... Mã giao dịch trong trường hợp này sẽ hết hiệu lực (kết thúc) ngay sau khi trả về lỗi cho TCDN.</p>
<p><em>TCDN không nên khởi tạo giao dịch với cùng thông tin công dân trên, có thể hướng dẫn công dân kiểm tra lại thông tin cá nhân hoặc thông báo lỗi cụ thể trên ứng dụng VNeID.</em></p></td>
</tr>
<tr>
<td style="text-align: center;">-18</td>
<td style="text-align: left;">Trường đầu vào không tồn tại trong cấu hình dịch vụ</td>
<td style="text-align: left;"><p>Lỗi trả về trong trường hợp bản tin gửi lên có trường thông tin nằm ngoài các trường đã được cấu hình trong dịch vụ.</p>
<p><em>TCDN cần kiểm tra lại các trường trong bản tin trong giao dịch đã khớp với dịch vụ đăng ký sử dụng chưa?</em></p></td>
</tr>
<tr>
<td style="text-align: center;">-19</td>
<td style="text-align: left;">Tài khoản tổ chức không có quyền sử dụng dịch vụ</td>
<td style="text-align: left;"><p>Lỗi trả về trong trường hợp yêu cầu dịch vụ sử dụng thông tin tài khoản của tổ chức không có quyền sử dụng dịch vụ.</p>
<p><em>TCDN cần kiểm tra lại các thông tin</em></p></td>
</tr>
<tr>
<td style="text-align: center;">-20</td>
<td style="text-align: left;">Thông tin công dân không đúng</td>
<td style="text-align: left;"><p>Lỗi trả về trong trường hợp thông tin công dân do TCDN gửi lên không trùng khớp với dữ liệu trong CSDL.</p>
<p><em>TCDN cần kiểm tra lại các thông tin công dân gửi lên đã đúng chưa? Có thể thử lại bằng cách chỉ gửi các trường bắt buộc theo API specs hoặc hướng dẫn công dân kiểm tra lại thông tin trên ứng dụng VNeID.</em></p></td>
</tr>
<tr>
<td style="text-align: center;">SHARED</td>
<td style="text-align: left;"></td>
<td style="text-align: left;"><em>nền tảng trả trạng thái đã chia sẻ dữ liệu của VNeID</em></td>
</tr>
<tr>
<td style="text-align: center;">NOT SHARED</td>
<td style="text-align: left;"></td>
<td style="text-align: left;"><em>nền tảng trả trạng thái không thành công của VNeID</em></td>
</tr>
<tr>
<td style="text-align: center;">INIT</td>
<td style="text-align: left;"></td>
<td style="text-align: left;"><em>Trạng thái công dân đã nhận thông tin xác nhận chia sẻ trên ứng dụng VNeID</em></td>
</tr>
</tbody>
</table>

## IV.5. Mã danh mục dùng chung {#iv-5-ma-danh-muc-dung-chung}

### IV.5.1. Mã danh mục giới tính (genderCode) {#iv-5-1-ma-danh-muc-gioi-tinh}

| **STT** | **Mã danh mục** | **Tên danh mục**  |
| :-----: | :-------------: | :---------------- |
|    1    |        1        | Chưa rõ thông tin |
|    2    |        2        | Nam               |
|    3    |        3        | Nữ                |
|    4    |        4        | Khác              |

### IV.5.2. Danh mục loại hóa đơn (áp dụng cho billing-history) {#iv-5-2-danh-muc-loai-hoa-don}

| **STT** | **Mã danh mục** | **Tên danh mục**    |
| :-----: | :-------------: | :------------------ |
|    1    |        1        | Điện                |
|    2    |        2        | Nước                |
|    3    |        3        | Truyền hình         |
|    4    |        4        | Internet            |
|    5    |        5        | Di động trả sau     |
|    6    |        6        | Điện thoại cố định  |
|    7    |        7        | Khoản vay           |
|    8    |        8        | Vé máy bay          |
|    9    |        9        | Thanh toán học phí  |
|   10    |       10        | Phí bảo hiểm        |
|   11    |       11        | Vé tàu hoả          |
|   12    |       12        | Phí chung cư        |
|   13    |       13        | Nạp tiền điện thoại |
|   14    |       14        | 3G/4G               |
|   15    |      KHAC       | Khác                |

### IV.5.3. Danh mục Trạng thái (áp dụng cho billing-history) {#iv-5-3-danh-muc-trang-thai}

| **STT** | **Mã danh mục** | **Tên danh mục** |
| :-----: | :-------------: | :--------------- |
|    1    |     SUCCESS     | Thành công       |
|    2    |     FAILURE     | Thất bại         |
|    3    |     WAITING     | Chờ kết quả      |
