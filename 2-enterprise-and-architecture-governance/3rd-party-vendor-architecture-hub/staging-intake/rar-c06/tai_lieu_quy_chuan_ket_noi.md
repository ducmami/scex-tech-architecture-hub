|  |
| --- |
| 1. **CỤC CẢNH SÁT QUẢN LÝ HÀNH CHÍNH VỀ TRẬT TỰ XÃ HỘI**<br>2. TRUNG TÂM NGHIÊN CỨU, ỨNG DỤNG VÀ DỮ LIỆU DÂN CƯ VÀ CĂN CƯỚC CÔNG DÂN |
| 4. C:\\Users\\P4\\Documents\\bo_cong_an_0.jpg<br>**TÀI LIỆU TIÊU CHUẨN KỸ THUẬT KẾT NỐI VỚI HỆ THỐNG TRUNG TÂM XÁC THỰC ĐIỆN TỬ** |
|  |
| 7. **Hà Nội – 2024** |

**MỤC LỤC**

[**THUẬT NGỮ, TỪ VIẾT TẮT VÀ KHÁI NIỆM 4**](#_heading-h-li4adfkzq6a3)

[**I. CĂN CỨ PHÁP LÝ 5**](#_heading-h-5ybis8uqydvt)

[**II. MỤC ĐÍCH, PHẠM VI ĐỐI TƯỢNG ÁP DỤNG 5**](#_heading-h-6a2h4nyw4zrj)

[II.1. Mục đích 5](#_heading-h-x2p76gsixvu)

[II.2.](#_heading-h-6i4xh48nh3ic)[ Đối tượng áp dụng](#_heading-h-6i4xh48nh3ic)[ 5](#_heading-h-6i4xh48nh3ic)

[II.3.](#_heading-h-kp5aw6tp067z)[ Nguyên tắc áp dụng](#_heading-h-kp5aw6tp067z)[ 5](#_heading-h-kp5aw6tp067z)

[**III. MÔ HÌNH KẾT NỐI HỆ THỐNG 5**](#_heading-h-6v2p1wr1ibky)

[III.1. Mô hình tổng quan 5](#_heading-h-8lma1cqq8aed)

[III.2. Mô hình hạ tầng CNTT 6](#_heading-h-6onv6elzi7zp)

[III.2.1. Mô hình đấu nối Agent Gateway 6](#_heading-h-rkmryu7nhyn1)

[III.2.2. Thiết kế mở rộng hệ thống 7](#_heading-h-trti7aphs9ak)

[III.3. Mô hình tổng quan kết nối TTXT 7](#_heading-h-e54xhouylmup)

[III.3.1. Mô hình kết nối 7](#_heading-h-nvu9j77jvn0u)

[III.3.2. Thuyết minh mô hình kết nối: 7](#_heading-h-f6bkop5a4t7m)

[III.3.3. Mô hình kết nối DC và DR TTXT 8](#_heading-h-v0bqnby36yk9)

[III.3.4. Thuyết minh mô hình DC DR TTXT: 9](#_heading-h-kcf6lo1symnx)

[**IV. YÊU CẦU, ĐIỀU KIỆN KẾT NỐI 10**](#_heading-h-8j8pl4vxhzqe)

[IV.1. Yêu cầu về hệ thống, kỹ thuật hạ tầng CNTT 10](#_heading-h-gndrd0cwluwy)

[IV.1.1. Tiêu chuẩn về thiết bị máy chủ 10](#_heading-h-q10mnehhtz7e)

[IV.1.2. Tiêu chuẩn và thiết bị HSM 10](#_heading-h-e2u2bzun19i7)

[IV.1.3. Tiêu chuẩn về hệ thống kết nối 12](#_heading-h-9zvypnsrjk8k)

[IV.2. Yêu cầu về an ninh an toàn thông tin 12](#_heading-h-px7558f955kh)

[IV.2.1. Các tiêu chí đánh giá ATTT 12](#_heading-h-wih5qa750cxm)

[IV.2.2. Các tiêu chí đánh giá ATTT chi tiết 13](#_heading-h-cb66t5uxd5mu)

[**V. CÁC BƯỚC THỰC HIỆN 14**](#_heading-h-7eiapv4t0yzc)

[V.1. Cách thức thực hiện cài đặt đấu nối hệ thống CNTT 14](#_heading-h-4aeejn3mwerm)

[V.2. Các bước thực hiện rà quét đánh giá ATTT 14](#_heading-h-h95iab54bruv)

[V.2.1. Các nội dung cần chuẩn bị 14](#_heading-h-c09mb2mjgbr0)

[V.2.2. Các bước kiểm tra, đánh giá 15](#_heading-h-7wu19n9eip8u)

[**VI. CÁC DỊCH VỤ CHIA SẺ DỮ LIỆU ĐỊNH DANH ĐIỆN TỬ 15**](#_heading-h-d6vxa96ruuj3)

[VI.1. LUỒNG DỮ LIỆU CHIA SẺ 15](#_heading-h-bn0v55b9ubkw)

[VI.1.1. Dịch vụ yêu cầu chia sẻ thông tin định danh thông qua TTXT 15](#_heading-h-8g6nn6jh05qx)

[VI.1.2. Dịch vụ cung cấp thông tin định danh thông qua TTXT 17](#_heading-h-joqr7kc4qr6t)

[VI.1.3. Dịch vụ xác thực thông tin định danh cho TTXT 18](#_heading-h-pogo6f9ll8r6)

[VI.1.4. Dịch vụ đăng nhập bằng tài khoản VNeID (SSO) 20](#_heading-h-41oh1i47yi39)

[**VII. CÁC DỊCH VỤ XÁC THỰC SINH TRẮC 21**](#_heading-h-lcf0hth377px)

[VII.1. Dịch vụ xác thực chống giả mạo khuôn mặt và xác thực khuôn mặt 21](#_heading-h-nu0u72dj1c15)

[VII.1.1. Yêu cầu kỹ thuật khi sử dụng dịch vụ: 21](#_heading-h-3shrmmxx181q)

[VII.1.2. Quy trình nghiệp vụ 21](#_heading-h-ai8lna33lexq)

[VII.2. Dịch vụ xác thực ảnh khuôn mặt 23](#_heading-h-7x07qvkkgb7y)

[VII.2.1. Yêu cầu kỹ thuật khi sử dụng dịch vụ: 23](#_heading-h-m0n21h1riuf5)

[VII.2.2. Quy trình nghiệp vụ 23](#_heading-h-iwmz6c6g1v7k)

<a id="_heading-h-li4adfkzq6a3"></a>**THUẬT NGỮ, TỪ VIẾT TẮT VÀ KHÁI NIỆM**

| Thuật ngữ/ Từ viết tắt | Định nghĩa |
| --- | --- |
| BCA | Bộ Công An |
| C06 | Cục Cảnh sát quản lý hành chính về trật tự xã hội |
| TCDN | Tổ chức doanh nghiệp |
| TCKT | Tổ chức khai thác dịch vụ - ở đây được hiểu là TCDN |
| TCCCDV | Tổ chức cung cấp dịch vụ - Trung tâm nghiên cứu, ứng dụng dữ liệu dân cư và căn cước công dân (Trung tâm RAR) |
| TTXT | Trung tâm xác thực |
| NTĐD | Nền tảng định danh và xác thực điện tử |
| HSM | Thiết bị vật lý được sử dụng để bảo vệ và quản lý chữ ký số và các khóa mã hóa trong môi trường an toàn |

- I. <a id="_heading-h-5ybis8uqydvt"></a>**CĂN CỨ PHÁP LÝ**

     - 1 (C06 bổ sung)

- II. <a id="_heading-h-6a2h4nyw4zrj"></a>**MỤC ĐÍCH, PHẠM VI ĐỐI TƯỢNG ÁP DỤNG**

      - II.1. <a id="_heading-h-x2p76gsixvu"></a>**Mục đích**

      - 2 Tài liệu mô tả các tiêu chuẩn cần có để các bên khai thác dịch vụ bao gồm tổ chức khai thác sử dụng khi kết nối với hệ thống Trung tâm xác thực.

      - II.2. <a id="_heading-h-6i4xh48nh3ic"></a>**Đối tượng áp dụng**

      - 3 Áp dụng cho các khách hàng, doanh nghiệp có kết nối trực tiếp với TTXT

      - II.3. <a id="_heading-h-kp5aw6tp067z"></a>**Nguyên tắc áp dụng**

      - 4 Các đối tượng được áp dụng theo nguyên tắc chung dưới đây

- III. <a id="_heading-h-6v2p1wr1ibky"></a>**MÔ HÌNH KẾT NỐI HỆ THỐNG**

       - III.1. <a id="_heading-h-8lma1cqq8aed"></a>**Mô hình tổng quan**

       - 5 
       - 6 **Diễn giải mô hình**
       - 7 Cài đặt trên nền của hạ tầng DC và DR, Hệ thống CNTT Trung tâm Xác thực cung cấp các mô hình dịch vụ cho các đối tượng khai thác là các cá nhân và tổ chức, các dịch vụ cụ thể như sau:
       - 8 Được quản lý và vận hành trên nền tảng Container, Dịch vụ dùng chung, Phần mềm quản lý và các Cơ sở dữ liệu, Trung tâm Xác thực cung cấp dịch vụ API platform
       - 9 Trong khi đó, các Ứng dụng dịch vụ của trung tâm được cung cấp: Ứng dụng xác thực cho các khối ngành kinh tế - xã hội (Xác thực phục vụ cho viễn thông; Xác thực phục vụ cho tài chính ngân hàng; Xác thực cho khối thương mại điện tử v…v…); Ứng dụng quản lý giao dịch SSO; Ứng dụng xác thực ảnh mặt.
       - 10 Các dịch vụ của trung tâm xác thực sẽ được cung cấp tới các tổ chức cá nhân sử dụng dịch vụ.

       - III.2. <a id="_heading-h-6onv6elzi7zp"></a>**Mô hình hạ tầng CNTT**

                - III.2.1. <a id="_heading-h-rkmryu7nhyn1"></a>**Mô hình đấu nối Agent Gateway**

A screenshot of a computer  Description automatically generated

Hình 1: Mô hình tổng quan kết nối Server Agent Gateway

**Thuyết minh mô hình**

- - 11 Server vật lý sẽ được cài đặt ESXI để ảo hóa và cài đặt 3 Server ảo chức năng:

- Cụm 2 server ảo Agent Gateway (Agent Gateway VM);

- Cụm 1 server Nginx;

  - 12 Server vật lý sẽ sử dụng 1 cổng vật lý để kết nối các mạng subnet ảo tới mạng vật lý bên ngoài: Kết nối tới server dịch vụ từ Agent Gateway; sử dụng để quản trị ESXI; cài đặt VM; cài đặt và cập nhật Code Agent Gateway;

  - 13 Server vật lý này sẽ được đặt tại 1 vùng riêng biệt của TCDN (tạm lấy tên là Agent Gateway Zone)s

  - 14 Một HSM vật lý sẽ kết nối với Agent GW VM để thực hiện giải mã bản tin.

       - III.2.2. <a id="_heading-h-trti7aphs9ak"></a>**Thiết kế mở rộng hệ thống**

  - 15 Tùy vào nhu cầu sử dụng của TCKT, Agent Gateway sẽ có thể triển khai mở rộng để tăng tính sẵn sàng và tăng hiệu năng.

  - 16 Hệ thống Agent Gateway có thể mở rộng theo 2 cách:

- Thêm tài nguyên Ram và lưu trữ cho server vật lý cài Agent Gateway;

- Thêm server vật lý Agent Gateway khác (mở rộng theo chiều rộng –scale out).

  - 17 Hệ thống sẽ cần thiết bị hoặc giải pháp cân bằng tải của tổ chức khai thác để cân bằng tải vào server Agent Gateway.
  - 18 Việc triển khai DR cũng tùy thuộc vào nhu cầu của TCKT. Các giải pháp chuyển luồng, cân bằng tải DC & DR cho Agent Gateway sẽ phụ thuộc vào giải pháp của TCKT.

  - III.3. <a id="_heading-h-e54xhouylmup"></a>**Mô hình tổng quan kết nối TTXT**

           - III.3.1. <a id="_heading-h-nvu9j77jvn0u"></a>**Mô hình kết nối**

  - 19 - III.3.2. <a id="_heading-h-f6bkop5a4t7m"></a>**Thuyết minh mô hình kết nối:**

- Ứng dụng khai thác dịch vụ TTXT: Ứng dụng của tổ chức danh nghiệp xậy dựng nhằm khác thác dịch vụ định danh và xác thực điện tử trên nền tảng CNTT của doanh nghiệp. Ứng dụng này có thể kết nối với ứng dụng máy chủ, hoặc ứng dụng di động thông qua cổng trao đổi bản tin với Agent Gateway.

- *AgentGateway:* Cổng tiếp nhận thông tin định danh: là hệ thống nằm trong Nền tảng và Hệ thống xác thực đặt tại Tổ chức cung cấp dịch vụ, làm nhiệm vụ quản lý điểm kết nối cuối cùng tới tổ chức doanh nghiệp và đảm bảo chuyển tải thông tin định danh theo giao thức mật tới AgentGateway của đúng đơn vị đăng ký sử dụng. Agent Gateway được triển khai trên hạ tầng đặt tại TCDN, làm nhiệm vụ tiếp nhận các giao dịch của người dùng từ Nền tảng định danh và ứng dụng VneID, chuyển tải yêu cầu và thu nhận kết quả tới ứng dụng nghiệp vụ khac thác dịch vụ định danh và xác thực điện tử. Agent Gateway bao gồm các thành phần chính như sau:

  - Agent Gateway: làm nhiệm vụ tiếp nhận, xử lý yêu cầu, thu nhận kết quả.
  - Cổng API cho phép Ứng dụng gọi các API nghiệp vụ liên quan đến định danh công dân số, ký số, xác thực giao dịch, tra cứu thông tin. Cổng API giao tiếp với Ứng dụng khai thác được bảo vệ bằng giao thức HTTPS và xác thực bằng phương thức API Key hoặc Basic Authentication
  - Mã hoá/giải mã đầu cuối bản tin chứa thông tin định danh (End-to-End Encryption) gửi NTDD Core Stack và AgentGateway.
  - AgentGateway giao tiếp với Module HSM, cho phép quản lý khoá doanh nghiệp, đảm bảo bảo mật an toàn thông tin. Module HSM này được đồng bộ khoá với Cụm HSM tại hệ thống CNTT Nền tảng định danh và xác thực điện tử.

- *API Gateway:* là thành phần nằm trong TTXT làm nhiệm vụ chuyển mạch kết nối và điều phối kết nối kênh tới các AgentGateway khác nhau cài đặt tại Tổ chức cung cấp dịch vụ. API Gateway cài đặt các khoá dưới dạng chứng thư số cho phép giao tiếp an toàn tới AgentGateway.

- *Các dịch vụ TTXT:* là thành phần nằm trong TTXT làm nhiệm vụ quản lý các tổ chức khai thác dịch vụ, quản lý gói cước, quản lý và đếm các giao dịch đi qua TTXT và một số các dịch vụ khác. Hệ thống cũng sẽ giữ vai trò cung cấp giao diện cho tổ chức khai thác để thực hiện đối soát, tính chi phí của các giao dịch qua TTXT.

- *Hệ thống nền tảng:* là hệ thống CNTT của Nền tảng định danh và xác thực điện tử có nhiệm vụ cung cấp thông tin được xác thực. Kết nối giữa TTXT với hệ thống Nền tảng định danh và xác thực điện tử được thực hiện trên kênh truyền cấp bởi H04 và mã hóa SSL/mTLS. Cũng tại đây, HSM được sử dụng để lưu khóa mã hóa gói tin E2E tới Agent Gateway đặt tại các tổ chức khai thác dịch vụ.

  - - III.3.3. <a id="_heading-h-v0bqnby36yk9"></a>**Mô hình kết nối DC và DR TTXT**

  - 20 - III.3.4. <a id="_heading-h-kcf6lo1symnx"></a>**Thuyết minh mô hình DC DR TTXT:**

- DC Hà Nội kết nối tới TTDLQGvDC DC qua đường quang trắng/Metronet có dự phòng của H04

- DR  HCM kết nối tới TTDLQGvDC DC qua đường VPN Site to Site trên hạ tầng Internet

- DC và DR TTXT kết nối với nhau bằng đường VPN Site to Site trên hạ tầng Internet. Dữ liệu giữa DC và DR chỉ là số liệu báo cáo đã tổng hợp để giảm thiểu băng thông chiếm dụng trên đường Internet.

- DC và DR TTXT đều kết nối sẵn 1 kết nối VPN Site to Site về TTDLQGvDC DR để phòng trường hợp thảm họa

- DC và DR TTXT hoạt động theo mô hình Active / Active. Các hệ thống DC và DR hoạt động độc lập, phân tải cho nhau (khách hàng 01 kết nối về DC; khách hàng 02 kết nối về DR). Trường hợp 1 trong 2 DC hoặc DR gặp sự cố, các khách hàng sẽ kết nối về TTDL còn lại để đảm bảo tính liên tục của dịch vụ.

- Tại site khách hàng, 2 kết nối VPN Site to Site qua Internet được thiết lập. 1 kết nối tới DC  và 1 kết nối tới DR. Tùy theo vị trí địa lý, khách hàng sẽ sử dụng kết nối chính và phụ tới từng DC & DR tương ứng. Việc chuyển kết nối từ DC qua DR và ngược lại sẽ được thiết lập tại ứng dụng (Agent Gateway) trong khi hạ tầng VPN sẽ chạy duy trì 2 kênh liên tục.

- Tại nghiệp vụ đối soát, dữ liệu tổng hợp sẽ được ứng dụng gộp lại tại một TTDL theo định kỳ và cung cấp cổng portal để đối soát giữa khách hàng và TTXT. Portal này được public ra Internet và được áp dụng GSLB để đảm bảo tính sẵn sàng khi DC hoặc DR gặp sự cố.

- IV. <a id="_heading-h-8j8pl4vxhzqe"></a>**YÊU CẦU, ĐIỀU KIỆN KẾT NỐI**

      - IV.1. <a id="_heading-h-gndrd0cwluwy"></a>**Yêu cầu về hệ thống, kỹ thuật hạ tầng CNTT**

              - IV.1.1. <a id="_heading-h-q10mnehhtz7e"></a>**Tiêu chuẩn về thiết bị máy chủ**

Máy chủ vật lý cần có các thông số tài nguyên tương ứng với 01 dịch vụ cụ thể như sau:

- CPU tối thiểu 32 Core; tốc độ tối thiểu 2Ghz

- Ram tối thiểu 128 GB

- Ổ cứng tối thiểu 2 ổ cứng SSD 1TB (Chạy Raid 1 – dung lượng sử dụng là 1TB)

- Tối thiểu 2 cổng kết nối 1Gbps

- Hypervisor cài đặt sẵn yêu cầu tối thiểu Esxi 7.0

  - - IV.1.2. <a id="_heading-h-e2u2bzun19i7"></a>**Tiêu chuẩn và thiết bị HSM**

  - 21 Thiết bị HSM vật lý cho hệ thống
  - 22 A screenshot of a computer device  Description automatically generated
  - 23 Tuỳ thuộc vào thiết bị vật lý HSM sử dụng, hệ thống đáp ứng theo ngưỡng TPS tương ứng (Xem mục sizing HSM).
  - 24 Thuật toán sử dụng:

- Mã hoá bản tin:

- Định dạng bản tin trao đổi theo lược đồ JWE

- Thuật toán: Key Agreement with ECDH Ephemeral Static: ECDHES+A128KW (Thoả thuận khoá, sinh khoá phiên theo ECDH – P256, Mã hoá theo thuật toán AES 128 – GCM )

- Ký số cho bản tin:

  - Định dạng bản tin trao đổi theo lược đồ JWS
  - Thuật toán: ECDSA using P-256 and SHA-384 (Ký số theo ECDSA với hàm băm SHA-384)

- Danh mục thiết bị HSM hỗ trợ:

  - *USB HSM: YubiHSM (FIPS 140 -2 Level 3) \~ 600$ https://www.yubico.com/products/hardware-security-module/*
  - *USB HSM: NitroHSM (PCI-DSS)\~ 150$ https://shop.*nitrokey*.com/shop/product/nkhs2-nitrokey-hsm-2-7*
  - *PCI HSM: Thales Luna PCIe HSM \~10K$ https://cpl.thalesgroup.com/encryption/hardware-security-modules/pciehsms*
  - *PCI HSM: Utimaco https://utimaco.com/products/technologies/globalcompliance/pci-hsm*
  - *HSM Server: Utimaco / Thales*

*https://utimaco.com/solutions/applications/general-purpose-hardware-security-modules*

*+ nCipher Entrust: nShield HSM*

*https://www.entrust.com/products/hsm*

- HSM Sizing:

- - IV.1.3. <a id="_heading-h-9zvypnsrjk8k"></a>**Tiêu chuẩn về hệ thống kết nối**

- 2 dải IP Private để đặt vào hệ thống theo mô hình nêu trên: dải IP MGMT Subnet; dải IP Prod Subnet.
- Kết nối Server subnet sử dụng để kết nối tới thiết bị HSM vật lý và ứng dụng dịch vụ của doanh nghiệp/tổ chức.
- Kết nối quản trị để quản trị các thành phần máy chủ ảo và Hypervisor
- Kết nối VPN Client to Site để truy cập mạng quản trị nói trên
- Server AgentGW sẽ phải được bảo vệ bởi chính sách bảo mật của tổ chức sử dụng dịch vụ và thừa hưởng tiêu chuẩn bảo mật của tổ chức.
- Kết nối hệ thống:

| **Nguồn** | **Đích** | **Giao thức (Protocol)** | **Chiều** |
| --- | --- | --- | --- |
| Địa chỉ Server Agent Gateway ( Server subnet) | Địa chỉ Server dịch vụ của nhà cung cấp | TCP 9000<br>TCP 9001 | 2 chiều |

- IV.2. <a id="_heading-h-px7558f955kh"></a>**Yêu cầu về an ninh an toàn thông tin**

        - IV.2.1. <a id="_heading-h-wih5qa750cxm"></a>**Các tiêu chí đánh giá ATTT**

|  |  |  |
| --- | --- | --- |
| **STT** | **Hạng mục các phương án ATTT cho các hệ thống sử dụng dịch vụ xác thực và khai thác danh tính điện tử** |  |
|  | Có phương án quản lý truy cập giữa các vùng mạng và phòng chống xâm nhập | Tường lửa; hệ thống IDS/IPS. |
|  | Có phương án phòng, chống tấn công mạng cho web app API service | Tường lửa ứng dụng Web. |
|  | Có phương án chặn lọc phần mềm độc hại trên môi trường mạng | Giải pháp/Thiết bị phát hiện và ngăn chặn mã độc lớp mạng. |
|  | Có phương án quản lý phần mềm phòng chống mã độc trên các máy chủ/máy tính người dùng vận hành quản trị. | Đầu tư giải pháp AntiVirus có chức năng quản lý tập trung hoặc hệ thống EDR. |
|  | Có kế hoạch kiểm tra đánh giá An toàn thông tin định kỳ hàng năm. | (1) Rà quét lỗ hổng bảo mật (Vulnerabilityassessment)<br>(2) Kiểm thử App/Service<br>(3) Kiểm tra tiêu chuẩn cấu hình an toàn (Audit) |

- IV.2.2. <a id="_heading-h-cb66t5uxd5mu"></a>**Các tiêu chí đánh giá ATTT chi tiết**

|  |  |  |
| --- | --- | --- |
| **TT** | **Danh mục đánh giá ATTT trong quá trình kiểm tra** |  |
| **I** | **Phạm vi đánh giá lỗ hổng bảo mật** |  |
| **1** | **Rà soát kiểm tra lỗ hổng bảo mật của hệ thống máy chủ** |  |
|  | Tiêu chuẩn đánh giá | Đánh giá được thực hiện dựa trên bộ tiêu chuẩn PTES (Penetration Testing Execution Standard) đây là bộ tiêu chuẩn uy tín áp dụng cho kiểm tra đánh giá bảo mật hệ thống máy chủ, máy tính, thiết bị và dịch vụ được các tổ chức, chính phủ và doanh nghiệp tin dùng. |
| **2** | **Rà soát bảo mật ứng dụng API và Web Services** |  |
|  | Tiêu chuẩn đánh giá | Đánh giá được thực hiện dựa trên bộ tiêu chuẩn OWASP Top 10 và OWASP Testing Guide.<br>Đây là bộ tiêu chuẩn uy tín áp dụng cho kiểm tra đánh giá bảo mật ứng dụng, được các tổ chức, chính phủ và doanh nghiệp tin dùng. |
| **3** | **Đánh giá rà soát bảo mật cho các thiết bị mạng, thiết bị bảo mật** |  |
|  | Tiêu chuẩn đánh giá | Đánh giá được thực hiện dựa trên bộ tiêu chuẩn PTES (Penetration Testing Execution Standard) đây là bộ tiêu chuẩn uy tín áp dụng cho kiểm tra đánh giá bảo mật hệ thống máy chủ, máy tính, thiết bị và dịch vụ được các tổ chức, chính phủ và doanh nghiệp tin dùng. |
| **II** | **Kiểm tra tiêu chuẩn cấu hình an toàn** |  |
|  | Các kỹ sư bảo mật sẽ kiểm tra tiêu chuẩn cấu hình an toàn cho các máy chủ, máy trạm, CSDL, ứng dụng Web, thiết bị bảo mật… tuân theo tiêu chuẩn CIS Benchmarks, đồng thời tham chiếu đến các tài liệu cấu hình bảo mật tiêu chuẩn của các công nghệ liên quan. |  |
| 1 | Đánh giá tiêu chuẩn cấu hình an toàn cho nền tảng, hệ điều hành |  |
| 2 | Đánh giá tiêu chuẩn cấu hình an toàn cho cơ sở dữ liệu |  |
| 3 | Đánh giá tiêu chuẩn cấu hình an toàn cho máy chủ ứng dụng, máy chủ Web |  |
| 4 | Đánh giá tiêu chuẩn cấu hình an toàn cho thiết bị mạng, thiết bị bảo mật |  |

- V. <a id="_heading-h-7eiapv4t0yzc"></a>**CÁC BƯỚC THỰC HIỆN**

     - V.1. <a id="_heading-h-4aeejn3mwerm"></a>**Cách thức thực hiện cài đặt đấu nối hệ thống CNTT**

     - 25 Tài khoản quản trị Server vật lý (Idrac; Ilom); Tài khoản quản trị ESXI; Tài khoản quản trị OS của máy chủ ảo; Tài khoản quản trị ứng dụng trên máy chủ ảo sẽ được C06 toàn quyền nắm giữ.
     - 26 Nhằm đảm bảo việc cấu hình; update code và xử lý lỗi được nhanh chóng, cần cung cấp cho hệ thống một kết nối Remote (VPN; Ultra view; v..v…) tại vùng quản trị MGMT & Monitor.
     - 27 Kết nối từ Agent Gateway Server vào hệ thống TTXT sẽ cần qua kết nối VPN Site to Site để đảm bảo tránh lộ lọt.
     - 28 Thiết bị HSM vật lý sẽ được kết nối với Server Agent GW qua Lan Subnet.

     - V.2. <a id="_heading-h-h95iab54bruv"></a>**Các bước thực hiện rà quét đánh giá ATTT**

            - V.2.1. <a id="_heading-h-c09mb2mjgbr0"></a>**Các nội dung cần chuẩn bị**

     - 29 (1) Xây dựng tài liệu quy trình nghiệp vụ triển khai trên hệ thống

     - 30 (2) Triển khai phân vùng mạng độc lập trên hệ thống (tạm gọi là AgentGW Zone), Tại Agent GW Zone sẽ đặt 1 máy chủ proxy để làm cầu nối giữa ứng dụng của TCDN và Server Agent Gateway

     - 31 (3) Cung cấp máy chủ ảo hóa cài đặt Agent Gateway tại hệ thống sử dụng dịch vụ

     - 32 (4) Tích hợp thiết bị HSM với Agent Gateway theo tiêu chuẩn do C06 cung cấp

     - 33 (5) Triển khai tối thiểu “Hạng mục các phương án ATTT cho các hệ thống sử dụng dịch vụ xác thực và khai thác danh tính điện tử” (dự kiến như bảng dưới, có thể triển khai thêm các giải pháp tùy thuộc vào điều kiện đơn vị).

     - 34 (6) Xây dựng hồ sơ phương án triển khai ATTT bao gồm: (1) Quy trình nghiệp vụ, mục đích triển khai hệ thống; (2) Sơ đồ hệ thống, danh sách thiết bị, máy chủ có trong hệ thống; (3) Quy định, quy chế đảm bảo ATTT trong hệ thống.

     - 35 (7) Cấu hình các thiết bị, máy chủ trước khi kiểm tra

          - V.2.2. <a id="_heading-h-7wu19n9eip8u"></a>**Các bước kiểm tra, đánh giá**

Đơn vị kết nối sẽ thực hiện thuê đơn vị doanh nghiệp đã được cấp phép kinh doanh dịch vụ an toàn thông tin mạng (cụ thể được cấp phép cung cấp dịch vụ kiểm tra, đánh giá an toàn thông tin mạng) để đánh giá hệ thống được kết nối tới hệ thống định danh và xác thực điện tử theo các bước:

**Bước 1: Họp triển khai kiểm tra ATTT:** (1) Tư vấn lựa chọn đơn vị kiểm tra; (2) Các đơn vị thống nhất các nội dung về phạm vi, thời gian, mục đích kiểm tra.

**Bước 2: Chuẩn bị kiểm tra:** Đơn vị kiểm tra thực hiện phối hợp doanh nghiệp, đơn vị kết nối chuẩn bị môi trường kiểm tra, đưa công cụ cần thiết vào phân vùng hệ thống cần kiểm tra.

**Bước 3: Thực hiện kiểm tra:** Thực hiện kiểm tra theo các nội dung trong phụ lục *“Nội dung kiểm tra ATTT cần đáp ứng”*

**Bước 4: Khắc phục hệ thống:** Đơn vị kết nối cấu hình bảo mật các thiết bị đảm bảo ATTT (theo khuyến nghị), khắc phục các lỗ hổng bảo mật được đưa ra sau khi rà quét (nếu có).

**Bước 5: Xuất báo cáo:** Sau khi đơn vị kiểm tra ATTT thực hiện rà quét đảm bảo không còn lỗ hổng bảo mật mức độ cao trở lên.

**Bước 6: Thực hiện kết nối, lưu hồ sơ.**

- VI. <a id="_heading-h-d6vxa96ruuj3"></a>**CÁC DỊCH VỤ CHIA SẺ DỮ LIỆU ĐỊNH DANH ĐIỆN TỬ**

      - VI.1. <a id="_heading-h-bn0v55b9ubkw"></a>**LUỒNG DỮ LIỆU CHIA SẺ**

              - VI.1.1. <a id="_heading-h-8g6nn6jh05qx"></a>**Dịch vụ yêu cầu chia sẻ thông tin định danh thông qua TTXT**

1. Embedded object: Visio.Drawing.15
2. Mô tả:

| Mã bước | Thực hiện | Mô tả các bước |
| --- | --- | --- |
| B1 | Hệ thống CNTT của TCKT | Hệ thống CNTT của TCKT gửi yêu cầu chia sẻ thông tin định danh của công dân tới Hệ thống CNTT của TTXT |
| B2 | Hệ thống CNTT của TTXT | Hệ thống CNTT của TTXT tiếp nhận và chuyển tiếp yêu cầu chia sẻ thông tin định danh của công dân tới Nền tảng định danh và xác thực điện tử. |
| B3 | Nền tảng định danh và xác thực điện tử | Nền tảng định danh và xác thực điện tử tiếp nhận yêu cầu chia sẻ thông tin định danh công dân từ Hệ thống CNTT của TTXT<br>Hệ thống sẽ kiểm tra tính hợp lệ của gói tin và ghi nhận giao dịch<br>Nền tảng định danh và xác thực điện tử chuyển tiếp yêu cầu sang Hệ thống định danh và xác thực điện tử |
| B4 | Hệ thống định danh và xác thực điện tử | Xử lý thông tin và mã hóa & giải mã |
| B5 | Hệ thống định danh và xác thực điện tử | Hệ thống định danh và xác thực điện tử thực hiện gửi yêu cầu công dân xác nhận chia sẻ thông tin định danh trên ứng dụng VNeID |
| B6 | Công dân | Công dân thực hiện đăng nhập vào ứng dụng VneID và chọn xác nhận (Đồng ý/Không đồng ý) chia sẻ thông tin công dân |
| B7 | Hệ thống định danh và xác thực điện tử | Hệ thống định danh và xác thực điện tử tiếp nhận kết quả xác nhận của công dân và thực hiện xử lý thông tin.<br>Sau đó Hệ thống định danh và xác thực điện tử sẽ thực hiện trả kết quả yêu cầu chia sẻ thông tin công dân tới Nền tảng định danh và xác thực điện tử |
| B8 | Nền tảng định danh và xác thực điện tử | Nền tảng định danh và xác thực điện tử tiếp nhận kết quả chia sẻ thông tin định danh từ Hệ thống định danh và xác thực điện tử.<br>Hệ thống ghi nhận giao dịch<br>Nền tảng định danh và xác thực điện tử gửi kết quả chia sẻ cho Hệ thống CNTT của TTXT |
| B9 | Hệ thống CNTT của TTXT | Hệ thống CNTT của TTXT tiếp nhận và chuyển tiếp kết quả chia sẻ thông tin định danh công dân tới Hệ thống CNTT của TCKT<br>Tính cước giao dịch |
| B10 | Hệ thống CNTT của TCKT | Hệ thống CNTT của TCKT nhận kết quả chia sẻ thông tin định danh công dân |

- - VI.1.2. <a id="_heading-h-joqr7kc4qr6t"></a>**Dịch vụ cung cấp thông tin định danh thông qua TTXT**

3. Quy trình:
4. Embedded object: Visio.Drawing.15
5. Mô tả:

| Mã bước | Thực hiện | Mô tả các bước |
| --- | --- | --- |
| B1 | Công dân | Công dân đăng nhập vào ứng dụng VNeID bằng tài khoản định danh điện tử mức độ 2 của công dân, sau đó chọn chức năng nghiệp vụ tương ứng.<br>Công dân thực hiện xác nhận cung cấp thông tin định danh tới đơn vị xử lý nghiệp vụ tương ứng |
| B2 | Hệ thống định danh và xác thực điện tử | Hệ thống định danh và xác thực điện tử tiếp nhận yêu cầu cung cấp thông tin định danh của công dân tới đơn vị xử lý nghiệp vụ tương ứng |
| B3 | Hệ thống định danh và xác thực điện tử | Hệ thống định danh và xác thực điện tử gửi thông tin định danh công dân đến Nền tảng định danh và xác thực điện tử |
| B4 | Nền tảng định danh và xác thực điện tử | Nền tảng định danh và xác thực điện tử tiếp nhận thông tin định danh của công dân.<br>Xác nhận tính hợp lệ của gói tin<br>Sau đó Nền tảng định danh và xác thực điện tử thực hiện gửi thông tin định danh của công dân tới Hệ thống CNTT của TTXT tương ứng với nghiệp vụ đã chọn của công dân |
| B5 | Hệ thống CNTT của TTXT | Hệ thống CNTT của TTXT tiếp nhận và chuyển tiếp kết quả chia sẻ thông tin định danh công dân tới hệ thống CNTT của TCKT<br>Hệ thống sẽ ghi nhận giao dịch và tính cước |
| B6 | Hệ thống CNTT của TCKT | Hệ thống CNTT của TCKT nhận kết quả chia sẻ thông tin định danh công dân |

- - VI.1.3. <a id="_heading-h-pogo6f9ll8r6"></a>**Dịch vụ xác thực thông tin định danh cho TTXT**

6. Quy trình:
7. Embedded object: Visio.Drawing.15
8. Mô tả:

| Mã bước | Thực hiện | Mô tả các bước |
| --- | --- | --- |
| B1 | Hệ thống CNTT của TCKT | Hệ thống CNTT của TCKT gửi yêu cầu xác thực thông tin định danh của công dân tới Hệ thống CNTT của TTXT |
| B2 | Hệ thống CNTT của TTXT | Hệ thống CNTT của TTXT tiếp nhận và chuyển tiếp yêu cầu xác thực thông tin định danh của công dân tới Nền tảng định danh và xác thực điện tử.<br>Ghi nhận giao dịch |
| B3 | Nền tảng định danh và xác thực điện tử | Nền tảng định danh và xác thực điện tử tiếp nhận yêu cầu xác thực thông tin định danh công dân từ Hệ thống CNTT của TTXT<br>Xác nhận tính hợp lệ của gói tin<br>Nền tảng định danh và xác thực điện tử chuyển tiếp yêu cầu sang Hệ thống định danh và xác thực điện tử |
| B4 | Hệ thống định danh và xác thực điện tử | Hệ thống định danh và xác thực điện tử tiếp nhận yêu cầu xác thực thông tin định danh công dân.<br>Xử lý thông tin, mã hóa & giải mã<br>Hệ thống định danh và xác thực điện tử |
| B5 | Công dân | Công dân thực hiện đăng nhập vào ứng dụng VneID và chọn xác nhận (Đồng ý/Không đồng ý) xác thực thông tin công dân |
| B6 | Hệ thống định danh và xác thực điện tử | Hệ thống định danh và xác thực điện tử nhận kết quả xác nhận của công dân, và thực hiện xử lý thông tin. Sau đó trả kết quả xác thực tới Nền tảng định danh và xác thực điện tử |
| B7 | Nền tảng định danh và xác thực điện tử | Nền tảng định danh và xác thực điện tử tiếp nhận kết quả xác thực thông tin định danh từ Hệ thống định danh và xác thực điện tử.<br>Nền tảng định danh và xác thực điện tử gửi kết quả xác thực cho Hệ thống CNTT của TTXT<br>Ghi nhận giao dịch |
| B8 | Hệ thống CNTT của TTXT | Hệ thống CNTT của TTXT tiếp nhận và chuyển tiếp kết quả xác thực thông tin định danh công dân tới Hệ thống CNTT của TCKT <br>Tính cước và ghi nhận giao dịch |
| B9 | Hệ thống CNTT của TCKT | Hệ thống CNTT của TCKT nhận kết quả xác thực thông tin định danh công dân |

- - VI.1.4. <a id="_heading-h-41oh1i47yi39"></a>**Dịch vụ đăng nhập bằng tài khoản VNeID (SSO)**

9. Embedded object: Visio.Drawing.15

Mô tả:

- - 1 Luồng đăng nhập:

- Người dùng yêu cầu đăng nhập trên ứng dụng (*Web/App mobile****)*** thuộc đơn vị kết nối cần tích hợp SSO với hệ thống VNeID

- Web/App của đơn vị kết nối sẽ thực hiện chuyển hướng (redirect) tới hệ thống TTXT. Từ hệ thống TCCCDC sẽ chuyển hướng sang Web/App VNeID thông qua Nền tảng định danh và xác thực điện tử để tiến hành đăng nhập

- Sau khi người dùng đăng nhập và xác thực thành công trên Web/App VNeID,  hệ thống Định danh và xác thực điện tử trả về mã Code cho Hệ thống TTXT thông qua Nền tảng định danh và xác thực điện tử. Hệ thống TTXT sẽ thực hiện trả mã Code cho Web/App của đơn vị kết nối.

- Web/App của đơn vị kết nối sử dụng mã Code này để lấy Access-Token từ Web/App VNeID thông qua Hệ thống TTXT và Nền tảng định danh và xác thực điện tử

- Web/App của đơn vị kết nối khởi tạo phiên đăng nhập, sử dụng Access-Token để truy vấn thông tin chia sẻ từ hệ thống Định danh và xác thực điện tử bằng hình thức qua Hệ thống TTXT và Nền tảng định danh và xác thực điện tử

  - 2 Luồng đăng xuất:

- Người dùng yêu cầu đăng xuất trên Web của đơn vị kết nối

- Hệ thống của đơn vị kết nối sẽ thực hiện gọi yêu cầu đăng xuất sang Web VNeID

- Web VNeID đăng xuất và trả lại thông báo đăng xuất cho Web của đơn vị kết nối

- Web của đơn vị kết nối thực hiện xóa phiên đăng nhập của người dùng.

  - 3 ***Lưu ý:*** Luồng đăng xuất chỉ áp dụng đối với Web**SSO**, không áp dụng cho App mobileSSO.

- VII. <a id="_heading-h-lcf0hth377px"></a>**CÁC DỊCH VỤ XÁC THỰC SINH TRẮC**

       - VII.1. <a id="_heading-h-nu0u72dj1c15"></a>**Dịch vụ xác thực chống giả mạo khuôn mặt và xác thực khuôn mặt**

                - VII.1.1. <a id="_heading-h-3shrmmxx181q"></a>**Yêu cầu kỹ thuật khi sử dụng dịch vụ:**

       - 1 \- Tích hợp SDK trên hệ điều hành di động Android và iOS để sử dụng dịch vụ theo hướng dẫn của TTXT.

       - 2 \- Có hệ thống trung gian kết nối giữa ứng dụng đã tích hợp SDK và hệ thống của TTXT.

       - 3 \- Tích hợp luồng xác nhận đồng thuận với ứng dụng VNeID để lấy đồng thuận của người dùng trước khi thực hiện xác thực.

       - 4 \- Có giải pháp bảo vệ ứng dụng và luồng xác thực khuôn mặt đảm bảo toàn vẹn dữ liệu.

           - VII.1.2. <a id="_heading-h-ai8lna33lexq"></a>**Quy trình nghiệp vụ**

       - 4 
       - 5 Mô tả luồng nghiệp vụ:
       - 6 Người dùng bắt đầu thực hiện xác thực chống giả mạo khuôn mặt và xác thực khuôn mặt trên ứng dụng của Tổ chức sử dụng dịch vụ.
       - 7 Ứng dụng Tổ chức sử dụng dịch vụ hiển thị thông báo yêu cầu đồng thuận sử dụng VNeID.
       - 8 Người dùng đồng ý thực hiện và Ứng dụng Tổ chức sử dụng dịch vụ chuyển sang ứng dụng VNeID.
       - 9 Người dùng thực hiện đăng nhập vào VNeID.
       - 10 VNeID hiển thị thông báo yêu cầu xác nhận đồng thuận.
       - 11 Người dùng xác nhận đồng thuận trên VNeID.
       - 12 VNeID nhận kết quả và trở lại màn hình của Ứng dụng Tổ chức sử dụng dịch vụ.
       - 13 Ứng dụng Tổ chức sử dụng dịch vụ, khi có xác nhận đồng ý thực hiện xác thực từ VNeID, thực hiện gọi SDK xác thực chống giả mạo khuôn mặt và xác thực khuôn mặt.
       - 14 SDK thực hiện chụp ảnh mặt công dân, tiền xử lý hình ảnh, đóng gói và gửi về Hệ thống Tổ chức sử dụng dịch vụ.
       - 15 Hệ thống Tổ chức sử dụng dịch vụ tạo yêu cầu xác thực chống giả mạo khuôn mặt và xác thực khuôn mặt, gửi gói tin nhận được từ SDK tới Trung tâm xác thực.
       - 16 Trung tâm xác thực xử lý yêu cầu xác thực chống giả mạo khuôn mặt, xác thực khuôn mặt và trả kết quả cho Hệ thống Tổ chức sử dụng dịch vụ.
       - 17 Hệ thống Tổ chức sử dụng dịch vụ tiếp nhận kết quả từ Trung tâm xác thực, xử lý theo nghiệp vụ và trả kết quả về SDK.
       - 18 SDK nhận kết quả và trả về cho Ứng dụng Tổ chức sử dụng dịch vụ.

       - VII.2. <a id="_heading-h-7x07qvkkgb7y"></a>**Dịch vụ xác thực ảnh khuôn mặt**

                - VII.2.1. <a id="_heading-h-m0n21h1riuf5"></a>**Yêu cầu kỹ thuật khi sử dụng dịch vụ:**

       - 5 \- Tích hợp SDK trên hệ điều hành di động Android và iOS để sử dụng dịch vụ theo hướng dẫn của TTXT.

       - 6 \- Có hệ thống trung gian kết nối giữa ứng dụng đã tích hợp SDK và hệ thống của TTXT.

       - 7 \- Tích hợp luồng xác nhận đồng thuận với ứng dụng VNeID để lấy đồng thuận của người dùng trước khi thực hiện xác thực.

       - 8 \- Có giải pháp bảo vệ ứng dụng và luồng xác thực khuôn mặt đảm bảo toàn vẹn dữ liệu.

       - 9 \- Ảnh khuôn mặt phục vụ xác thực khuôn mặt phải được kiểm tra chống giả mạo khuôn mặt bằng giải pháp chống giả mạo khuôn mặt được chứng nhận iBeta PAD Level 2 theo tiêu chuẩn ISO/IEC 30107.

           - VII.2.2. <a id="_heading-h-iwmz6c6g1v7k"></a>**Quy trình nghiệp vụ**

       - 19 
       - 20 Mô tả luồng nghiệp vụ:
       - 21 Người dùng bắt đầu thực hiện xác thực khuôn mặt trên ứng dụng của Tổ chức sử dụng dịch vụ.
       - 22 Ứng dụng Tổ chức sử dụng dịch vụ hiển thị thông báo yêu cầu đồng thuận sử dụng VNeID.
       - 23 Người dùng đồng ý thực hiện và Ứng dụng Tổ chức sử dụng dịch vụ chuyển sang ứng dụng VNeID.
       - 24 Người dùng thực hiện đăng nhập vào VNeID.
       - 25 VNeID hiển thị thông báo yêu cầu xác nhận đồng thuận.
       - 26 Người dùng xác nhận đồng thuận trên VNeID.
       - 27 VNeID nhận kết quả và trở lại màn hình của Ứng dụng Tổ chức sử dụng dịch vụ.
       - 28 Ứng dụng Tổ chức sử dụng dịch vụ, khi có xác nhận đồng ý thực hiện xác thực từ VNeID, thực hiện gọi SDK xác thực khuôn mặt.
       - 29 SDK thực hiện chụp ảnh mặt công dân, tiền xử lý hình ảnh, đóng gói và gửi về Hệ thống Tổ chức sử dụng dịch vụ.
       - 30 Hệ thống Tổ chức sử dụng dịch vụ tạo yêu cầu xác thực khuôn mặt, gửi gói tin nhận được từ SDK tới Trung tâm xác thực.
       - 31 Trung tâm xác thực xử lý yêu cầu xác thực khuôn mặt và trả kết quả cho Hệ thống Tổ chức sử dụng dịch vụ.
       - 32 Hệ thống Tổ chức sử dụng dịch vụ tiếp nhận kết quả từ Trung tâm xác thực, xử lý theo nghiệp vụ và trả kết quả về SDK.
       - 33 SDK nhận kết quả và trả về cho Ứng dụng Tổ chức sử dụng dịch vụ.

**MẪU CAM KẾT BẢO ĐẢM AN NINH, AN TOÀN VÀ BẢO MẬT THÔNG TIN TRONG KẾT NỐI ĐẾN HỆ THỐNG TRUNG TÂM XÁC THỰC ĐIỆN TỬ**

**(Theo yêu cầu, hướng dẫn của Trung tâm RAR)**

**MẪU ĐĂNG KÝ SỬ DỤNG DỊCH VỤ KẾT NỐI VỚI HỆ THỐNG XÁC THỰC ĐIỆN TỬ**

**(Theo yêu cầu, hướng dẫn của Trung tâm RAR)**
