---
title: "[BRD] Transaction Monitoring"
allowTitleToDifferFromFilename: true
confluence_url: "https://confluence.scex.com.vn/spaces/TAH/pages/10561960/BRD+Transaction+Monitoring"
confluence_status: current
confluence_id: "10561960"
confluence_space: TAH
version: 1
---

**SCEX — TÀI LIỆU YÊU CẦU NGHIỆP VỤ**

**Case Management cho Compliance / AML / Transaction Monitoring**

# **1. Bối cảnh, mục tiêu, và phạm vi**

SCEX hiện đã có bộ rule Transaction Monitoring (TM) chạy để phát hiện giao dịch nghi vấn, nhưng chưa có nơi tập trung để tiếp nhận và xử lý.

Tài liệu này mô tả yêu cầu nghiệp vụ cho một hệ thống Case Management tổng, nơi mọi alert từ rule TM được tạo case, gán người xử lý, điều tra, ra quyết định qua cơ chế kiểm soát chéo (maker/checker), và lưu vết đầy đủ phục vụ audit nội bộ cũng như báo cáo cơ quan quản lý khi cần.

Tài liệu tập trung thuần vào yêu cầu nghiệp vụ, không đề cập kiến trúc hay giải pháp kỹ thuật.

# **2. Luồng nghiệp vụ tổng quan**

Khi một giao dịch (hoặc chuỗi giao dịch) khớp điều kiện trigger của một rule TM, hệ thống tạo ra một alert. Case Management tiếp nhận alert này theo nguyên tắc gộp theo user:

- Nếu user đang có một case ở trạng thái mở (chưa đóng), alert mới được gộp vào case đó thay vì tạo case riêng.
- Nếu user không có case nào đang mở, hệ thống tạo một case mới.

Case sau đó đi qua quá trình điều tra, đề xuất biện pháp xử lý, và mọi biện pháp có tác động lên tài khoản của user (khóa tính năng, khóa tài khoản, báo cáo SAR) đều phải qua cơ chế kiểm soát maker/checker.

Toàn bộ log thao tác trên case management là bất biến, chỉ được bổ sung, không được sửa hoặc xóa sau khi đã lưu.

# **3. Cấu trúc dữ liệu của một Case**

Mỗi case cần thể hiện đầy đủ các nhóm thông tin sau:

## **3.1. Thông tin định danh case**

| **Trường thông tin** | **Mô tả**                                 |
| -------------------- | ----------------------------------------- |
| Mã case              | Định danh duy nhất, dễ tra cứu/tham chiếu |
| Thời điểm tạo        | Ngày giờ case được sinh ra                |
| Trạng thái case      | Theo vòng đời tại mục 4                   |
| Mức độ rủi ro        | Thấp / Trung bình / Cao / Nghiêm trọng    |
| Người phụ trách      | Admin đang được gán xử lý case            |

## **3.2. Thông tin user bị flag**

- Định danh user (user ID), có liên kết trực tiếp sang hồ sơ KYC/profile để đối chiếu.
- Trạng thái tài khoản hiện tại: hoạt động bình thường / đã bị hạn chế một phần / đã khóa.
- Lịch sử các case đã đóng trước đó của user này, để đánh giá vi phạm lần đầu hay tái phạm khi ra quyết định biện pháp xử lý.

## **3.3. Danh sách rule vi phạm và alert**

Mỗi case chứa một hoặc nhiều alert (ứng với các rule đã trigger). Mỗi alert cần có trạng thái riêng, độc lập với trạng thái tổng của case, để phân biệt rõ phần nào mới phát sinh và phần nào đang/đã xử lý:

| **Trạng thái alert**     | **Ý nghĩa**                                                                                               |
| ------------------------ | --------------------------------------------------------------------------------------------------------- |
| Mới (New)                | Alert vừa phát sinh, chưa ai xem/xử lý — cần hiển thị nổi bật, ưu tiên ở đầu danh sách để không bị bỏ sót |
| Đang xử lý (In Progress) | Đã có người tiếp nhận và đang điều tra                                                                    |
| Đã xử lý (Resolved)      | Đã kết luận và xử lý xong cho riêng alert này                                                             |

_Lưu ý: Case chỉ đủ điều kiện đóng khi toàn bộ alert bên trong đã ở trạng thái Đã xử lý._

## **3.4. Thông tin giao dịch liên quan**

Mỗi giao dịch được liệt kê trong case cần gắn rõ với alert/rule nào đã tham chiếu đến nó, kèm loại giao dịch (nạp/rút/giao dịch spot), số tiền (VNĐ/TSMH), loại tiền (VNĐ/TSMH), thời gian, đối tác giao dịch, và trạng thái hiện tại của giao dịch đó (đã hoàn tất / đang treo).

## **3.5. Log điều tra và thao tác**

Lịch sử đầy đủ theo trình tự thời gian: ghi chú điều tra, thay đổi trạng thái, đề xuất của maker, quyết định phê duyệt/từ chối của checker, biện pháp đã thực thi, và người/thời điểm thực hiện mỗi hành động.

Log này là bất biến (append-only), phục vụ trực tiếp cho công tác audit.

# **4. Vòng đời của Case và Alert**

## **4.1. Trạng thái Case**

| **Trạng thái**                                     | **Mô tả**                                                              |
| -------------------------------------------------- | ---------------------------------------------------------------------- |
| Mới tạo (New)                                      | Case vừa được sinh ra, chưa có ai tiếp nhận                            |
| Đang điều tra (Under Investigation)                | Đã gán cho một admin xử và đang điều tra                               |
| Chờ phê duyệt (Pending Approval)                   | Maker đã đề xuất biện pháp xử lý, đang chờ checker duyệt               |
| Leo thang (Escalated)                              | Case vượt thẩm quyền xử lý thông thường, chuyển cấp cao hơn quyết định |
| Đã đóng — Xác nhận vi phạm (Closed – Confirmed)    | Đã áp dụng biện pháp xử lý và đóng case                                |
| Đã đóng — False Positive (Closed – False Positive) | Sau điều tra xác định không phải vi phạm thật                          |

## **4.2. Ràng buộc bắt buộc**

- Không cho đóng case khi còn alert ở trạng thái Mới hoặc Đang xử lý.
- Mọi biện pháp xử lý có tác động thật lên user (khóa tính năng/tài khoản, report SAR) chỉ được thực thi sau khi có phê duyệt của checker.
- Đóng case ở diện False Positive không bắt buộc qua checker, nhưng bắt buộc phải nhập lý do/căn cứ cụ thể vào log, phục vụ rà soát và cải thiện rule sau này.

# **5. Biện pháp xử lý (do maker đề xuất, checker phê duyệt trước khi thực thi)**

- Khóa một tính năng cụ thể của user (rút tiền, nạp tiền, giao dịch spot, tạo lệnh mới...), các tính năng khác vẫn hoạt động bình thường.
- Khóa toàn bộ tài khoản, có hai mức: khóa thao tác (vẫn đăng nhập được nhưng không thực hiện được hành động nào) và khóa hoàn toàn (không đăng nhập được).
- Thời hạn khóa: vô thời hạn, và chỉ có thể mở khoá thủ công bằng admin.
- Đưa user vào danh sách theo dõi đặc biệt (watchlist) mà không áp dụng khóa, giám sát chặt hơn các giao dịch tiếp theo.
- Mở khóa lại: cũng cần maker đề xuất, checker duyệt, và ghi rõ lý do mở khóa.

# **6. Cơ chế Maker – Checker**

Mọi tác vụ có tác động thật lên user phải qua maker - checker:

- maker điều tra và đề xuất hành động
- checker review đề xuất kèm bằng chứng và quyết định phê duyệt hoặc từ chối (kèm lý do, trả case về cho maker).

Maker và checker của cùng một đề xuất bắt buộc là hai người khác nhau, hệ thống cần chặn việc tự phê duyệt đề xuất của chính mình.

## **6.1. Nâng cấp độ checker theo mức độ nghiêm trọng**

Với các tác vụ có mức độ tác động cao hơn, checker được nâng lên một cấp phê duyệt cao hơn thay vì checker thông thường:

| **Loại tác vụ**                                                 | **Cấp checker**                                          |
| --------------------------------------------------------------- | -------------------------------------------------------- |
| Khóa một tính năng, đưa vào watchlist, case mức Thấp/Trung bình | Checker thông thường (ví dụ: Duy, Linh, Trinh,...)       |
| Khóa toàn bộ tài khoản, report SAR, case mức Nghiêm trọng       | Admin cấp cao hơn phê duyệt (ví dụ: anh Hải, Minh Trung) |

## **6.2. Đóng case diện False Positive**

Maker được phép tự đóng case ở diện False Positive mà không cần qua checker phê duyệt, do đây không phải là hành động có tác động lên user.

Tuy nhiên, bắt buộc phải nhập lý do/căn cứ chi tiết vào log tại thời điểm đóng, dữ liệu này phục vụ rà soát định kỳ và cải thiện rule TM.

# **7. Luồng báo cáo SAR (Suspicious Activity Report)**

- Case được checker gắn cờ “Đủ điều kiện SAR” sẽ export ra một báo cáo SAR nháp gắn với case đó.
- Báo cáo SAR tổng hợp tự động từ dữ liệu case sẵn có: thông tin định danh user (theo hồ sơ KYC), tóm tắt hành vi nghi vấn, danh sách rule đã trigger, danh sách giao dịch liên quan (thời gian, số tiền, đối tác giao dịch,...), tóm tắt quá trình điều tra và ghi chú compliance, biện pháp đã áp dụng (nếu có), người phê duyệt gắn cờ SAR.
- Trước mắt: cho phép export toàn bộ thông tin trên ra file (theo mẫu của pháp chế / nghiệp vụ cung cấp) để hoàn thiện thủ công theo đúng biểu mẫu quy định hiện hành trước khi gửi cơ quan quản lý.
- Case sau khi export SAR cần lưu vết: đã report hay chưa, thời điểm export, người thực hiện nhằm hạn chế báo cáo trùng hoặc bỏ sót.

# **8. SLA xử lý**

Yêu cầu tối thiểu: hệ thống có trường config cho SLA theo từng mức độ rủi ro (đơn vị giờ/ngày, để trống hoặc nhập sau), và khả năng đánh dấu case “quá hạn SLA” một khi con số được cấu hình.

# **9. Dashboard tổng quan**

Ngoài màn hình xử lý từng case theo vòng đời, cần một dashboard tổng quan (đặt phía trên màn hình danh sách case hoặc một trang riêng) để quản lý nắm nhanh tình hình chung, ví dụ (chưa đầy đủ) gồm:

- Tổng số case đang mở, chia theo trạng thái (Mới / Đang điều tra / Chờ phê duyệt / Leo thang).
- Số case theo mức độ rủi ro, ưu tiên làm nổi bật số case mức Nghiêm trọng chưa xử lý.
- Số alert mới phát sinh trong ngày/tuần, đối chiếu với số case tạo mới (để thấy tỷ lệ alert được gộp vào case cũ).
- Số case đang chờ checker phê duyệt, theo từng cấp checker.
- Số case đã report SAR trong kỳ.
- Số case quá hạn SLA.

# **10. Phân quyền**

| **Vai trò**        | **Vị trí trong quy trình** | **Quyền hạn chính**                                                                               |
| ------------------ | -------------------------- | ------------------------------------------------------------------------------------------------- |
| Admin vận hành     | Maker                      | Tiếp nhận, điều tra, ghi chú, đề xuất biện pháp xử lý; tự đóng case diện False Positive kèm lý do |
| Compliance Manager | Checker (cấp thông thường) | Phê duyệt/từ chối đề xuất của maker đối với tác vụ mức thường; xử lý case leo thang cấp 1         |
| Admin cấp cao      | Checker (cấp nâng cao)     | Phê duyệt các tác vụ tác động cao: khóa toàn bộ tài khoản, report SAR, case mức Nghiêm trọng      |
| Admin/Audit        | Chỉ xem                    | Xem toàn bộ case và log phục vụ audit, không thao tác nghiệp vụ                                   |
