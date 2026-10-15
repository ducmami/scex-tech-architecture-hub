# **BRD: Cơ chế tạo tài khoản và đăng nhập qua tích hợp VNeID trên SCEX**

## **1. Bối cảnh và mục tiêu**

- VNeID là ứng dụng định danh và xác thực điện tử quốc gia, xây dựng trên Cơ sở dữ liệu quốc gia về dân cư. Tài khoản định danh điện tử có hai mức: mức 1 đăng ký online, mức 2 xác thực trực tiếp kèm đối chiếu sinh trắc học (vân tay, khuôn mặt) với dữ liệu Căn cước công dân. Chỉ tài khoản mức 2 mới đủ điều kiện dùng cho xác thực tài chính, ngân hàng.

- Hiện tại, cơ chế tạo tài khoản và đăng nhập trên app SCEX dùng email hoặc số điện thoại kèm OTP, kết hợp quét NFC Căn cước công dân gắn chip cho bước eKYC. Phương thức NFC gặp rào cản với người dùng có thiết bị không hỗ trợ NFC hoặc thao tác quét gặp lỗi.

- Nhiều tổ chức tài chính tại Việt Nam đã triển khai kết nối app-to-app với VNeID để mở tài khoản và xác thực eKYC mà không cần NFC, thông qua hợp tác với Trung tâm Nghiên cứu, Ứng dụng dữ liệu dân cư và Căn cước công dân (RAR, thuộc Bộ Công an). Một số ví dụ đã triển khai: Techcombank (từ 11/2024), BIDV, SHB (2026), và Công ty Chứng khoán VNDIRECT, đơn vị chứng khoán đầu tiên ký hợp tác với RAR (8/2025).

- Trải nghiệm người dùng ở các ứng dụng trên: chọn mở tài khoản hoặc cập nhật sinh trắc học qua VNeID ngay trong app tài chính, hệ thống điều hướng sang app VNeID trên cùng thiết bị để xác thực, dữ liệu định danh đã đối chiếu được gửi ngược lại app tài chính để hoàn tất, toàn bộ thao tác trong vài phút và không cần đến quầy giao dịch.

- Ngoài cơ chế app-to-app dùng cho ứng dụng di động, VNeID còn hỗ trợ xác thực trên nền web bằng cách quét mã QR từ app VNeID trên điện thoại, mô hình đã áp dụng cho Cổng Dịch vụ công Quốc gia: trang web hiển thị mã QR, người dùng quét bằng app VNeID, xác nhận ngay trên điện thoại, phiên làm việc trên web tự động được xác thực.

- **Mục tiêu của tài liệu:**
  - bổ sung VNeID như một phương thức song song, bên cạnh phương thức hiện tại, để nhà đầu tư xác thực danh tính khi mở tài khoản giao dịch tài sản mã hoá trên SCEX và xác thực đăng nhập, áp dụng cho cả ứng dụng di động và nền web. Việc này giúp mở rộng khả năng tiếp cận cho người dùng không có thiết bị NFC, đồng thời tận dụng dữ liệu đã được Bộ Công an xác thực để rút ngắn thời gian eKYC.

## **2. Phạm vi**

**Trong phạm vi:**

- Luồng tạo tài khoản SCEX mới qua VNeID, áp dụng cho người dùng chưa có tài khoản SCEX. Luồng này gồm bước liên kết VNeID, vì tài khoản vừa tạo đã gắn sẵn định danh VNeID ngay từ đầu.

- Luồng liên kết VNeID cho tài khoản SCEX đã tồn tại, tạo trước đó bằng email, số điện thoại, hoặc NFC Căn cước công dân. Thực hiện trong phần cài đặt tài khoản khi người dùng đã đăng nhập vào SCEX.
  - Mục đích: gắn định danh VNeID mức 2 vào tài khoản đang có, để nâng cấp mức xác thực eKYC không cần NFC, đồng thời mở khả năng dùng VNeID cho các lần đăng nhập sau.

- Luồng xác thực đăng nhập lại bằng VNeID, áp dụng cho tài khoản đã hoàn tất liên kết VNeID, dù liên kết qua luồng tạo tài khoản mới hay luồng liên kết nói trên. Thực hiện tại mỗi phiên đăng nhập để xác minh danh tính khớp với dữ liệu đã liên kết.

- Luồng tạo tài khoản và đăng nhập trên nền web SCEX qua VNeID, dùng cơ chế quét mã QR thay cho kết nối app-to-app, vì trình duyệt web không điều hướng trực tiếp sang app VNeID được.

- Bước xác thực danh tính qua VNeID trong luồng chuyển đổi tài khoản Simulator hiện hữu sang Real Trading, với vai trò một phương thức xác thực danh tính ngang hàng với NFC tại đúng bước bắt buộc xác minh danh tính của luồng chuyển đổi đó. Các bước còn lại của luồng chuyển đổi Simulator sang Real Trading do bộ tài liệu riêng của dự án đó quy định, không đề cập mô tả lại ở đây.

**Ngoài phạm vi:**

- Luồng huỷ liên kết VNeID, chưa đưa vào phạm vi tài liệu này
- Các bước của luồng chuyển đổi Simulator sang Real Trading ngoài bước xác thực danh tính qua VNeID, ví dụ hồ sơ mở tài khoản Giao dịch thật hay điều kiện đặt lệnh, thuộc phạm vi bộ tài liệu riêng của dự án Simulator lên Real Trading
- Định danh tổ chức, doanh nghiệp qua VNeID, vì tài liệu này chỉ phục vụ nhóm khách hàng cá nhân

## **3. Constraints lưu ý (Căn cứ pháp lý và điểm trọng yếu)**

- **Nghị định 69/2024/NĐ-CP** về định danh và xác thực điện tử, hiệu lực từ 1/7/2024. Điều 18 quy định điều kiện, trình tự kết nối với hệ thống định danh và xác thực điện tử: hệ thống thông tin của bên kết nối phải đạt an toàn thông tin tối thiểu cấp độ 3.

- **Nghị quyết 05/2025/NQ-CP** về thí điểm thị trường tài sản mã hoá, hiệu lực từ 9/9/2025. Yêu cầu xác minh danh tính (KYC) chặt chẽ cho giao dịch từ 1,000 USD trở lên, và lưu trữ dữ liệu giao dịch cùng lịch sử truy cập tối thiểu 10 năm.
  - Kỳ vọng: cơ chế xác thực qua VNeID phải ghi log đầy đủ (thời điểm, phương thức, kết quả đối khớp) và lưu trữ theo đúng thời hạn này.

- **Nghị định 284/2026/NĐ-CP**, hiệu lực từ 1/9/2026, quy định mức phạt 50 đến 70 triệu đồng đối với tổ chức cung cấp dịch vụ bỏ sót việc xác minh danh tính người mở tài khoản tài sản mã hoá.
  - Kỳ vọng: bước xác thực danh tính, dù qua VNeID hay phương thức khác, là điều kiện bắt buộc phải hoàn tất trước khi tài khoản được cấp quyền giao dịch.

- **Luật Bảo vệ dữ liệu cá nhân số 91/2025/QH15** và **Nghị định 356/2025/NĐ-CP**, cùng hiệu lực từ 1/1/2026. Quy định bên kiểm soát dữ liệu chỉ được xử lý dữ liệu cá nhân khi có sự đồng ý chủ động, rõ ràng của chủ thể dữ liệu; các thiết lập mặc định không được xem là đã đồng ý.

## **4. Kỳ vọng về trạng thái tài khoản**

### **4.1 Trạng thái liên kết VNeID**

| **Trạng thái**                           | **Mô tả**                                                                                                                          | **Quyền hạn tương ứng**                                                                                      |
| ---------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| Chưa có tài khoản                        | Chưa qua giai đoạn Tạo tài khoản (Signup)                                                                                          | Không áp dụng                                                                                                |
| Đã Signup, chưa xác thực định danh       | Đã hoàn tất Signup (xác thực phone và email qua OTP, đặt mật khẩu), chưa thực hiện giai đoạn Xác thực định danh qua NFC hoặc VNeID | Đăng nhập được bằng tài khoản Signup, Hồ sơ giao dịch chưa hoàn tất theo mục 4.2 Được dùng feature Simulator |
| Đang xác thực VNeID                      | Đã điều hướng sang VNeID, chờ người dùng xác nhận chia sẻ dữ liệu và chờ đối khớp                                                  | Chưa hoàn tất giai đoạn Xác thực định danh                                                                   |
| Tài khoản hoạt động, đã liên kết VNeID   | Đã xác thực định danh thành công qua VNeID                                                                                         | Dùng được cả VNeID lẫn tài khoản Signup để đăng nhập                                                         |
| Tài khoản hoạt động, chưa liên kết VNeID | Đã xác thực định danh qua NFC Căn cước công dân, chưa liên kết VNeID                                                               | Đăng nhập bằng tài khoản Signup, có thể liên kết VNeID sau                                                   |
| Liên kết VNeID đã huỷ                    | Người dùng chủ động huỷ liên kết, hoặc tài khoản định danh điện tử bị khoá phía VNeID                                              | Đăng nhập bằng phương thức khác, không dùng được VNeID cho đến khi liên kết lại                              |
| Yêu cầu xác minh lại (PCRT)              | Tài khoản không giao dịch 6 tháng, quay lại giao dịch trên 400 triệu đồng, hệ thống tự động yêu cầu eKYC lại                       | Bị khoá dịch vụ giao dịch (rút nộp fiat, đặt lệnh) cho đến khi xác minh lại thành công                       |

### **4.2 Trạng thái Hồ sơ giao dịch (Tài khoản giao dịch TSMH)**

| **Trạng thái**                                       | **Mô tả**                                                                                                  | **Quyền hạn tương ứng**                                                                         |
| ---------------------------------------------------- | ---------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| Chưa hoàn tất Hồ sơ giao dịch                        | Chưa hoàn tất giai đoạn Xác thực định danh (qua VNeID hoặc NFC), hoặc chưa ký hợp đồng giao dịch           | Chưa truy cập được các tính năng trên SCEX                                                      |
| Đã hoàn tất Hồ sơ giao dịch, chưa nạp tài sản mã hoá | Đã xác thực định danh và đã ký hợp đồng giao dịch, không phân biệt đã đăng ký tài khoản ngân hàng hay chưa | Truy cập được các tính năng trên SCEX, trừ giao dịch tài sản mã hoá, theo yêu cầu KYC tại mục 3 |
| Đã nạp tài sản mã hoá                                | Tài khoản đã nạp tài sản mã hoá vào SCEX                                                                   | Đầy đủ tính năng, gồm giao dịch tài sản mã hoá                                                  |

## **5. Luồng nghiệp vụ chi tiết**

### **5.1 Tổng quan luồng Mở tài khoản Giao dịch TSMH và vị trí của VNeID**

- Luồng Mở tài khoản Giao dịch TSMH (MTK) tại SCEX gồm bốn giai đoạn:
  - Tạo tài khoản (Signup: nhập số điện thoại và email, xác thực OTP riêng từng phần, đặt mật khẩu),
  - Xác thực định danh,
  - Ký hợp đồng giao dịch (econtract),
  - Đăng ký tài khoản ngân hàng (optional).

- Ba giai đoạn Tạo tài khoản, Xác thực định danh, và Ký hợp đồng giao dịch là bắt buộc. Đăng ký tài khoản ngân hàng không bắt buộc, người dùng được phép bỏ qua và thực hiện sau.

- Giai đoạn Tạo tài khoản phải hoàn tất liên tục cả ba bước (xác thực OTP điện thoại, xác thực OTP email, đặt mật khẩu) trong một lượt, không lưu tiến trình dở dang. Nếu người dùng dừng lại giữa chừng, phải thực hiện lại từ đầu.

- VNeID được tích hợp vào giai đoạn Xác thực định danh, đóng vai trò một phương thức thay thế NFC Căn cước công dân, không thay đổi các giai đoạn còn lại của luồng MTK.

- Chi tiết luồng xác thực định danh qua VNeID mô tả ở mục 5.2. Giai đoạn Ký hợp đồng giao dịch và Đăng ký tài khoản ngân hàng không thuộc phạm vi tài liệu này, chỉ được nhắc tới ở mức là giai đoạn tiếp theo sau khi xác thực định danh hoàn tất.

### **5.2 Xác thực định danh qua VNeID trong luồng Mở tài khoản (thay thế bước NFC)**

- Tại giai đoạn Xác thực định danh của luồng MTK, người dùng thấy phương thức xác thực qua VNeID ngang hàng với phương thức NFC Căn cước công dân hiện tại.
- Người dùng chọn xác thực qua VNeID, hệ thống điều hướng sang app VNeID trên cùng thiết bị.
- Trường hợp thiết bị chưa cài VNeID, hoặc tài khoản định danh điện tử chưa đạt mức 2, hệ thống thông báo rõ điều kiện chưa đáp ứng và đưa người dùng quay lại phương thức NFC.
- Trên app VNeID, người dùng xác thực bằng passcode hoặc sinh trắc học theo cấu hình tài khoản của họ.
- App VNeID hiển thị yêu cầu xác nhận chia sẻ dữ liệu định danh sang SCEX, nêu rõ những trường thông tin sẽ được chia sẻ và mục đích sử dụng.
- Người dùng chủ động xác nhận đồng ý chia sẻ. Hệ thống không được suy đoán hoặc mặc định là đã đồng ý.
- Sau khi xác nhận, dữ liệu định danh được đối khớp và gửi về app SCEX kèm kết quả xác thực thành công hoặc thất bại.
- App SCEX yêu cầu người dùng quay mặt vào camera để thực hiện Face Matching và Liveness Detection: hệ thống gửi ảnh khuôn mặt tới dịch vụ C06 (đã đăng ký sử dụng) để lấy điểm Face Matching so với ảnh sinh trắc học trên Căn cước công dân, đồng thời xác nhận người đang thao tác là người thật (Liveness Detection), không phải ảnh chụp hoặc video giả mạo.
- App SCEX hiển thị lại thông tin đã nhận để người dùng xác nhận trước khi hoàn tất giai đoạn Xác thực định danh, không tự động xem là đã xác thực khi chưa có bước xác nhận cuối này.
- Sau khi xác thực thành công, tài khoản chuyển sang trạng thái "Tài khoản hoạt động, đã liên kết VNeID" theo mục 4.1. Người dùng tiếp tục giai đoạn kế tiếp của luồng MTK: Ký hợp đồng giao dịch, rồi tới Đăng ký tài khoản ngân hàng.
- Trường hợp đối khớp thất bại, ví dụ thông tin không trùng khớp, tài khoản VNeID bị khoá, hoặc phiên xác thực hết hạn, hệ thống thông báo lý do cụ thể và đề xuất người dùng chuyển sang phương thức NFC.

### **5.3 Liên kết VNeID cho tài khoản đã tồn tại**

- Người dùng đã có tài khoản SCEX, đã hoàn tất giai đoạn Xác thực định danh trước đó bằng NFC Căn cước công dân, có thể chủ động liên kết VNeID trong phần cài đặt tài khoản.
- Luồng này cũng áp dụng cho trường hợp tài khoản bị yêu cầu xác minh lại theo quy định Phòng chống rửa tiền (PCRT): tài khoản không có giao dịch trong 6 tháng, sau đó quay lại giao dịch với giá trị trên 400 triệu đồng, hệ thống tự động chuyển trạng thái tài khoản và yêu cầu eKYC lại bắt buộc. Nếu không xác minh lại, tài khoản bị khoá các dịch vụ giao dịch (rút nộp fiat, đặt lệnh giao dịch). Khi ở trạng thái này, app dẫn luồng để người dùng thực hiện xác minh lại bằng VNeID.
- Bước xác thực và xin phép chia sẻ dữ liệu thực hiện tương tự mục 5.2: người dùng đăng nhập vào app VNeID, sau đó xác nhận đồng ý chia sẻ dữ liệu định danh sang SCEX ngay trên app VNeID. Bước xác nhận này không diễn ra trên app SCEX.
- Hệ thống đối chiếu thông tin định danh nhận từ VNeID với thông tin đã có trên tài khoản SCEX hiện tại, chỉ cho phép liên kết khi thông tin định danh cốt lõi (họ tên, số định danh cá nhân, ngày sinh) trùng khớp, nhằm tránh liên kết nhầm giữa hai chủ thể khác nhau.
- Trường hợp thông tin cốt lõi trùng khớp nhưng một số thông tin phụ khác nhau, ví dụ địa chỉ liên hệ đã thay đổi trên VNeID nhưng chưa được cập nhật trên tài khoản SCEX, hệ thống thông báo cho người dùng biết có sự khác biệt, sau đó điều hướng sang luồng thay đổi thông tin trên app SCEX để người dùng cập nhật.
- Sau khi liên kết thành công, tài khoản chuyển sang trạng thái "Tài khoản hoạt động, đã liên kết VNeID", dùng được VNeID cho các lần đăng nhập sau theo luồng ở mục 5.4.

### **5.4 Đăng nhập lại bằng VNeID cho tài khoản đã liên kết**

- Tại màn hình đăng nhập, người dùng chọn đăng nhập bằng VNeID, hiển thị bên cạnh phương thức đăng nhập hiện tại.

- Hệ thống điều hướng sang app VNeID để xác thực bằng passcode hoặc sinh trắc học.
  - Bước này sẽ lặp lại màn hình xin phép chia sẻ dữ liệu do VNeID cung cấp hay không phụ thuộc vào nền tảng VNeID cung cấp.

- Sau khi xác thực thành công trên VNeID, dữ liệu định danh được đối khớp với thông tin tài khoản SCEX đã liên kết trước đó.

- Nếu khớp, người dùng được đăng nhập vào SCEX.

- Nếu không khớp, ví dụ do tài khoản VNeID đổi thiết bị hoặc thông tin định danh đã thay đổi, hệ thống từ chối đăng nhập và đề xuất phương thức đăng nhập thay thế hoặc hướng dẫn liên hệ hỗ trợ.

### **5.5 Tạo tài khoản và đăng nhập trên nền web SCEX qua VNeID**

- Tại giai đoạn Xác thực định danh hoặc màn hình đăng nhập trên web SCEX, hệ thống hiển thị mã QR bên cạnh phương thức hiện tại, thay cho nút điều hướng sang app VNeID như trên di động.
- Người dùng mở app VNeID trên điện thoại, chọn chức năng quét mã QR, quét mã đang hiển thị trên màn hình web.
- App VNeID hiển thị thông báo xác nhận đang thực hiện xác thực định danh hoặc đăng nhập trên SCEX, người dùng xác thực bằng passcode hoặc sinh trắc học, sau đó xác nhận đồng ý chia sẻ dữ liệu ngay trên app VNeID, tương tự bước xác nhận ở mục 5.2.
- Sau khi xác nhận trên app VNeID, phiên làm việc trên web SCEX tự động cập nhật, hoàn tất giai đoạn Xác thực định danh hoặc đăng nhập mà không cần thao tác thêm trên trình duyệt.
- Trường hợp mã QR hết hạn trước khi người dùng quét xong, hệ thống hiển thị mã QR mới, không dùng lại mã cũ.
- Các bước còn lại, gồm xác nhận thông tin nhận được, đối khớp dữ liệu, và xử lý các trường hợp lỗi, áp dụng tương tự luồng di động tương ứng ở mục 5.2 đến 5.4.

### **5.6 Xác thực danh tính qua VNeID trong luồng chuyển đổi tài khoản Simulator sang Real Trading**

Dự án chuyển đổi tài khoản Simulator hiện hữu sang Real Trading có bộ tài liệu riêng (BRD, URD, IRD của dự án Simulator lên Real Trading) - xem tài liệu: [Dự án chuyển simulator user sang Real trade](https://confluence.lpbs.com.vn/spaces/SCEX/pages/70368807/BRD+T%C3%ADch+h%E1%BB%A3p+t%C3%ADnh+n%C4%83ng+Giao+d%E1%BB%8Bch+T%C3%A0i+s%E1%BA%A3n+m%C3%A3+ho%C3%A1+th%E1%BA%ADt+v%C3%A0o+%E1%BB%A9ng+d%E1%BB%A5ng+SCEX+Simulator) . Mục này chỉ mô tả điểm chạm nơi VNeID được dùng làm phương thức xác thực định danh trong luồng chuyển đổi đó, không mô tả lại toàn bộ luồng chuyển đổi.

- Khi người dùng chọn Mở tài khoản thật từ feature Simulator, hệ thống tạo một bản ghi Tạo tài khoản (Signup) mới cho tài khoản dưới feature Giao dịch thật. User sẽ được chuyển sang luồng tiếp nối Mở tài khoản giao dịch thật để người dùng được chọn xác thực qua VNeID như một phương thức ngang hàng với NFC Căn cước công dân. (tức là được điều hướng về luồng MTK như mục 5.2)
- Nếu tài khoản Simulator chưa liên kết VNeID, giai đoạn này thực hiện theo đúng luồng liên kết ở mục 5.3. Nếu tài khoản đã liên kết VNeID từ trước, hệ thống dùng lại dữ liệu định danh đã có, không yêu cầu xác thực lại qua VNeID.
- Sau khi xác thực thành công, tài khoản chuyển sang trạng thái "Tài khoản hoạt động, đã liên kết VNeID" theo mục 4.1. Người dùng tiếp tục giai đoạn kế tiếp của luồng MTK: Ký hợp đồng giao dịch, rồi tới Đăng ký tài khoản ngân hàng.
- Trường hợp đối khớp thất bại, ví dụ thông tin không trùng khớp, tài khoản VNeID bị khoá, hoặc phiên xác thực hết hạn, hệ thống thông báo lý do cụ thể và đề xuất người dùng chuyển sang phương thức NFC.

## **6. Yêu cầu hệ thống cần đáp ứng để triển khai**

- **Kết nối với VNeID:** thực hiện thông qua một tổ chức trung gian đã được Bộ Công an cấp phép cung cấp dịch vụ xác thực điện tử, không tự xây dựng kết nối trực tiếp vào hệ thống định danh quốc gia. Áp dụng cho cả hai cơ chế kết nối: app-to-app trên di động, và quét mã QR trên web.
- **Cơ chế đối khớp dữ liệu:** so khớp thông tin định danh nhận từ VNeID với dữ liệu đã có trên hệ thống SCEX, áp dụng cho luồng liên kết và luồng đăng nhập, trên cả di động và web.
- **Dịch vụ Face Matching và Liveness Detection:** tích hợp dịch vụ C06 để so khớp khuôn mặt người dùng với ảnh sinh trắc học trên Căn cước công dân và xác nhận người thật đang thao tác, áp dụng tại bước xác thực định danh qua VNeID.
- **Cơ chế xin phép chia sẻ dữ liệu:** hiển thị rõ mục đích sử dụng và phạm vi dữ liệu trước khi nhận dữ liệu từ VNeID, yêu cầu người dùng xác nhận chủ động.
- **Cơ chế lưu vết:** ghi nhận log xác thực gồm thời gian, phương thức, kết quả, phục vụ yêu cầu lưu trữ dữ liệu giao dịch và lịch sử truy cập theo Nghị quyết 05/2025/NQ-CP.
- **Cơ chế dự phòng:** khi VNeID không khả dụng, ví dụ chưa cài đặt, tài khoản chưa đạt mức 2, hoặc lỗi kết nối, người dùng vẫn hoàn tất được việc tạo tài khoản hoặc đăng nhập qua phương thức hiện có, không bị chặn luồng chính.

## **7. Yêu cầu phi chức năng**

- Dữ liệu trao đổi giữa app SCEX và VNeID phải mã hoá trên đường truyền.
- Thời gian xác thực và đối khớp không kéo dài quá vài giây, để không làm gián đoạn trải nghiệm người dùng.
- Log xác thực lưu trữ tối thiểu 10 năm, theo yêu cầu của Nghị quyết 05/2025/NQ-CP.
- Nếu kết nối với VNeID gián đoạn giữa chừng, dù qua app-to-app hay quét mã QR, người dùng không mất dữ liệu đã nhập trước đó và có thể tiếp tục hoặc quay lại bước trước, không phải làm lại từ đầu.
- Cho phép admin nghiệp vụ truy vấn lịch sử chi tiết việc chia sẻ thông tin từ VNeID (thời điểm, trường dữ liệu đã chia sẻ, kết quả đối khớp) và truy vấn điểm Face Matching của từng lần xác thực.

## **8. Rủi ro và giả định**

**Giả định đang dùng:**

- SCEX ký hợp tác với VNeID cung cấp dịch vụ xác thực điện tử qua VNeID.
- Nhóm người dùng mục tiêu của luồng VNeID là người đã có tài khoản định danh điện tử mức 2.

**Rủi ro pháp lý:**

- Nếu màn hình xin phép chia sẻ dữ liệu không đáp ứng yêu cầu đồng ý chủ động, rõ ràng theo Luật Bảo vệ dữ liệu cá nhân, có thể dẫn đến vi phạm quy định.

**Rủi ro vận hành:**

- Tỷ lệ người dùng thực tế có tài khoản VNeID mức 2 và cài app trên cùng thiết bị có thể chưa đủ cao ở giai đoạn đầu, cần duy trì song song các phương thức khác.
- Thời gian phản hồi và độ ổn định của kết nối app-to-app phụ thuộc vào hạ tầng của tổ chức trung gian và hệ thống định danh quốc gia, nằm ngoài tầm kiểm soát của SCEX.
