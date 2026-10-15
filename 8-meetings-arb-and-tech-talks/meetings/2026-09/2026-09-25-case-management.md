## II. THẢO LUẬN GIẢI PHÁP

### Phân định phạm vi quản lý Case:

Data Warehouse: Đóng vai trò tổng hợp dữ liệu, chạy các Rule định kỳ (mặc định lúc 0:05 đêm) để quét và phát hiện các case vi phạm. Sau khi quét xong, Data Warehouse đẩy dữ liệu sang DB cho Portal.
Portal: Tiếp nhận dữ liệu từ Data Warehouse và chủ động xử lý, quản lý toàn bộ vòng đời của Case.

### Cơ chế đồng bộ Cấu hình Rule:

Người dùng chỉnh sửa tham số Rule trên Portal, dữ liệu được lưu xuống cơ sở dữ liệu của Portal.
Data Warehouse sẽ kéo (load) bảng cấu hình tham số mới nhất từ DB của Portal về để phục vụ chạy tính toán Store Procedure định kỳ. Hai bên thống nhất cấu trúc dữ liệu/syntax.
Đối với các Rule mới hoặc logic mới hoàn toàn, Data Warehouse bắt buộc phải lập trình/phát triển thêm.

### Cơ chế đồng bộ Case, Giao dịch & Thông tin User:

Sau khi chạy tổng hợp dữ liệu xong, Data Warehouse đẩy trực tiếp các case vi phạm cùng thông tin Alert, Giao dịch liên quan và Thông tin User (KYC, trạng thái tài khoản) vào bảng trong DB để Portal truy xuất.
Lịch chạy tổng hợp mặc định là 1 lần/ngày lúc 0:05 đêm (có thể điều chỉnh mốc thời gian để tránh trùng lịch đối soát).

### Tương tác với hệ thống Core (Khóa/Mở khóa tài khoản):

Khi Checker phê duyệt đề xuất khóa/mở khóa tài khoản trên Portal, Portal sẽ gọi API trực tiếp sang Core.

## III. CHI TIẾT Q&A

- Q1 (Team Portal): Data Warehouse có quản lý vòng đời của các Case sau khi filter ra không? Nếu có thì Portal có phải đồng bộ ngược lại không?
  - Trả lời (DW): Data Warehouse chỉ đóng vai trò quét dữ liệu và đẩy ra các case vi phạm theo ngày. Data Warehouse không quản lý vòng đời Case, toàn bộ việc quản lý và xử lý Case sẽ do Portal đảm nhận, không cần đồng bộ ngược lại.
- Q2 (Team Portal): Cơ chế đồng bộ cấu hình Rule giữa Portal và Data Warehouse được thực hiện như thế nào (qua API hay DB)?
  - Trả lời (DW): Không bắt buộc dùng API. Portal lưu cấu hình tham số xuống DB, Data Warehouse sẽ kéo bảng dữ liệu đó về trước mỗi lần chạy tính toán. Hai bên chỉ cần thống nhất syntax và cấu trúc bảng.
- Q3 (Team Portal): Portal lấy dữ liệu các Case vi phạm từ Data Warehouse bằng cách nào?
  - Trả lời (DW): Sau khi tổng hợp xong, Data Warehouse sẽ đẩy dữ liệu thẳng vào một bảng trong DB theo cấu trúc đã định nghĩa, Portal chủ động đọc dữ liệu từ bảng này ra để xử lý.
- Q4 (Team Portal): Khi nào phía SA (Anh Đức) có thể hoàn thành Sơ đồ/Tài liệu Kiến trúc hệ thống?
  - Trả lời (Anh Đức - SA & Đạt - BA): Do timeline dự án phải go-live vào ngày 30 nên cần kiến trúc sớm để ghép nối hệ thống. Anh Đức (SA) sẽ ngồi lại với Anh Hiệp vào Thứ 2 - Thứ 3 tuần sau và chốt bản kiến trúc hoàn chỉnh muộn nhất vào Thứ 3 tuần sau.
