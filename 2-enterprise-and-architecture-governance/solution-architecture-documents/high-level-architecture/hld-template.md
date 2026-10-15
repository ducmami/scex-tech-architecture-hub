---
document_type: High Level Design (HLD)
template_version: "1.1"
document_status: Template
classification: Internal
owner: Enterprise Architecture / Solution Architecture
last_updated: 2026-09-03
---

# HIGH LEVEL DESIGN — [TÊN GIẢI PHÁP]

> **Mã tài liệu:** [HLD-{DOMAIN}-{SYSTEM}]  
> **Phiên bản:** [0.1]  
> **Hồ sơ áp dụng:** [Lite | Standard | Enhanced]  
> **Trạng thái:** [Draft | In Review | Approved | Superseded]  
> **Mức độ bảo mật:** [Public | Internal | Confidential | Restricted]

---

## Hướng dẫn sử dụng template

### Mục đích

Template này chuẩn hóa nội dung tối thiểu của một tài liệu **High Level Design (HLD)** tại SCEX. HLD là mô tả kiến trúc đủ để:

1. các bên liên quan hiểu giải pháp và phạm vi thay đổi;
2. các nhóm triển khai độc lập không đưa ra các quyết định kiến trúc mâu thuẫn;
3. Architecture Review Board (ARB), An toàn thông tin, Hạ tầng/Vận hành và chủ hệ thống có thể đánh giá tính phù hợp, rủi ro và khả năng vận hành;
4. truy vết được từ yêu cầu đến thành phần, kiểm soát, quyết định và phương thức xác minh;
5. làm đầu vào cho Low Level Design (LLD), kế hoạch triển khai, kiểm thử và vận hành.

HLD mô tả **hệ thống/phân hệ, trách nhiệm, quan hệ, giao diện, luồng dữ liệu, ranh giới tin cậy, mô hình triển khai và các quyết định có ảnh hưởng chéo**. HLD không thay thế tài liệu yêu cầu, đặc tả API, mô hình dữ liệu vật lý, runbook hoặc LLD.

> **Phạm vi áp dụng tiêu chuẩn:** Các tiêu chuẩn/framework tại mục 12 là căn cứ để thiết kế cấu trúc và tiêu chí review, không phải tuyên bố chứng nhận hoặc tuân thủ tự động. Nhãn **[BẮT BUỘC]** là yêu cầu quản trị của template SCEX, không có nghĩa mọi nguồn chuẩn đều bắt buộc đúng nguyên văn nội dung đó. Mỗi HLD phải ghi đúng phiên bản baseline nội bộ đang áp dụng tại mục 1.4; khi có khác biệt, chính sách đã phê duyệt và yêu cầu pháp lý của SCEX được ưu tiên, đồng thời ngoại lệ phải được ghi nhận.

### Nguyên tắc “just enough architecture”

Chỉ đưa vào HLD nội dung cần thiết để thống nhất các quyết định có ảnh hưởng hệ thống. Một nội dung thuộc HLD khi câu trả lời cho cả ba câu hỏi sau là **Có**:

1. Hai nhóm triển khai độc lập có thể chọn hai cách không tương thích không?
2. Quyết định này không thể suy ra rõ ràng từ tiêu chuẩn hoặc hệ thống hiện hữu không?
3. Quyết định này có trade-off thực sự về nghiệp vụ, dữ liệu, kỹ thuật, an toàn, vận hành hoặc chi phí không?

Nếu không, tham chiếu đến tiêu chuẩn/LLD liên quan thay vì mô tả sâu trong HLD.

### Quy ước mức độ bắt buộc

| Nhãn               | Cách áp dụng                                                                                 |
| ------------------ | -------------------------------------------------------------------------------------------- |
| **[BẮT BUỘC]**     | Phải hoàn thành cho mọi HLD.                                                                 |
| **[CÓ ĐIỀU KIỆN]** | Phải hoàn thành khi điều kiện ghi tại mục đó xảy ra; nếu không áp dụng, ghi `N/A — <lý do>`. |
| **[TÙY CHỌN]**     | Chỉ giữ khi giúp giải quyết concern cụ thể của stakeholder.                                  |

Không xóa im lặng một mục **[BẮT BUỘC]** hoặc **[CÓ ĐIỀU KIỆN]**. Dùng `N/A — <lý do>` khi không áp dụng. Xóa toàn bộ hướng dẫn trong ngoặc vuông và các comment HTML trước khi phát hành bản Approved.

### Hồ sơ áp dụng và tailoring

Chọn một hồ sơ ngay khi khởi tạo HLD; Solution Architect xác nhận lựa chọn này trong mục 1.3. Hồ sơ quyết định **độ sâu**, không làm mất các concern bắt buộc. Có thể trả lời ngắn hoặc tham chiếu tài liệu nguồn thay vì sao chép.

| Hồ sơ        | Dùng khi                                                                                                                                           | Mức tài liệu kỳ vọng                                                                                                                                         |
| ------------ | -------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Lite**     | Thay đổi cục bộ, ít thành phần, không tạo system/trust/data boundary mới, không đổi system of record hoặc NFR tier, rủi ro thấp                    | Hoàn thành các mục bắt buộc thật ngắn; ưu tiên context, delta, NFR bị ảnh hưởng, risk và verification; các view không đổi được phép tham chiếu HLD hiện hữu. |
| **Standard** | Dịch vụ/tích hợp mới hoặc thay đổi liên hệ thống có rủi ro trung bình; đây là hồ sơ mặc định                                                       | Hoàn thành toàn bộ mục bắt buộc và mọi mục có điều kiện áp dụng.                                                                                             |
| **Enhanced** | Hệ thống trọng yếu, dữ liệu nhạy cảm/cá nhân, ranh giới tổ chức mới, thay đổi lớn về nền tảng/DR, migration phức tạp hoặc yêu cầu audit/regulatory | Standard cộng threat model, quality/DR/performance evidence, transition architecture, compliance mapping và review độc lập phù hợp.                          |

Không dùng **Lite** nếu thay đổi làm phát sinh external exposure, dữ liệu Restricted, thanh toán/tài sản khách hàng, shared platform, RTO/RPO nghiêm ngặt hơn hoặc blast radius nhiều hệ thống. Khi không chắc, chọn **Standard**.

### Ranh giới HLD và LLD

| Thuộc HLD                                                 | Thuộc LLD hoặc đặc tả chuyên biệt                   |
| --------------------------------------------------------- | --------------------------------------------------- |
| System/subsystem/container và trách nhiệm                 | Class, method, package, internal module chi tiết    |
| Giao diện ở mức capability, protocol, ownership, SLA      | Toàn bộ endpoint, field, schema và mã lỗi           |
| Logical/conceptual data entity, ownership, classification | Physical schema, DDL, index, partition chi tiết     |
| Ranh giới mạng/trust zone và topology triển khai          | Manifest, Terraform, Helm values, cấu hình từng máy |
| Luồng chính, luồng lỗi quan trọng, consistency pattern    | Thuật toán và pseudocode chi tiết                   |
| Quality scenario, SLO, RTO/RPO, capacity envelope         | Test case chi tiết và script kiểm thử               |
| Quyết định kiến trúc và trade-off                         | Quyết định cục bộ có thể suy ra từ code             |
| Kiểm soát an toàn ở mức kiến trúc                         | Rule/config chi tiết của từng sản phẩm bảo mật      |

### Quy ước định danh và truy vết

Sử dụng ID ổn định, không tái sử dụng ID đã bỏ:

| Loại                          | Mẫu ID          | Ví dụ        |
| ----------------------------- | --------------- | ------------ |
| Tài liệu tham chiếu           | `REF-xx`        | `REF-02`     |
| Business requirement/outcome  | `BR-xx`         | `BR-01`      |
| Stakeholder concern           | `CON-xx`        | `CON-03`     |
| Architecture requirement      | `AR-xx`         | `AR-03`      |
| Non-functional requirement    | `NFR-<nhóm>-xx` | `NFR-AVL-01` |
| Giả định                      | `ASM-xx`        | `ASM-02`     |
| Ràng buộc                     | `CST-xx`        | `CST-01`     |
| Nguyên tắc/tiêu chuẩn áp dụng | `STD-xx`        | `STD-04`     |
| Viewpoint                     | `VP-xx`         | `VP-02`      |
| Architecture view             | `VIEW-xx`       | `VIEW-03`    |
| Diagram/model thuộc view      | `HLD-DIA-xx`    | `HLD-DIA-03` |
| Business capability/actor     | `BUS-xx`        | `BUS-02`     |
| Component/subsystem           | `CMP-xx`        | `CMP-05`     |
| Interface/integration         | `INT-xx`        | `INT-02`     |
| Data entity/domain            | `DATA-xx`       | `DATA-04`    |
| Data flow                     | `DF-xx`         | `DF-03`      |
| Baseline/target gap           | `GAP-xx`        | `GAP-01`     |
| Security/privacy control      | `CTRL-xx`       | `CTRL-07`    |
| Architecture decision         | `ADR-xxx`       | `ADR-012`    |
| Risk                          | `RISK-xx`       | `RISK-03`    |
| Work package                  | `WP-xx`         | `WP-02`      |
| Technical debt                | `TD-xx`         | `TD-01`      |
| Open issue                    | `OI-xx`         | `OI-05`      |

`VP-xx`, `VIEW-xx` và `HLD-DIA-xx` không đồng nghĩa: **viewpoint** định nghĩa quy ước để tạo/đọc một view; **view** biểu diễn kiến trúc theo viewpoint đó; diagram/model là một phần tử nội dung của view. Một view có thể gồm nhiều diagram, bảng và narrative.

---

# 1. Quản trị tài liệu

## 1.1 Thông tin tài liệu **[BẮT BUỘC]**

| Thuộc tính                         | Giá trị                                                                                     |
| ---------------------------------- | ------------------------------------------------------------------------------------------- |
| Tên giải pháp/hệ thống             | [Điền]                                                                                      |
| Mã tài liệu                        | [Điền]                                                                                      |
| Hồ sơ áp dụng                      | [Lite/Standard/Enhanced]                                                                    |
| Lý do chọn hồ sơ/tailoring         | [Trigger, phạm vi rút gọn, tài liệu được tham chiếu thay thế; không dùng `N/A` chung chung] |
| Người xác nhận tailoring           | [Solution Architect/ARB theo thẩm quyền]                                                    |
| Business Owner                     | [Điền]                                                                                      |
| System/Service Owner               | [Điền]                                                                                      |
| Solution Architect                 | [Điền]                                                                                      |
| Tech Lead(s)                       | [Điền]                                                                                      |
| Security/Privacy Reviewer          | [Điền]                                                                                      |
| Infrastructure/Operations Reviewer | [Điền]                                                                                      |
| Repository/đường dẫn nguồn         | [Điền]                                                                                      |
| Tài liệu thay thế bởi/thay thế cho | [N/A hoặc mã tài liệu]                                                                      |
| Ngày review tiếp theo              | [YYYY-MM-DD hoặc điều kiện kích hoạt review]                                                |

## 1.2 Lịch sử thay đổi **[BẮT BUỘC]**

| Phiên bản | Ngày         | Tác giả | Thay đổi chính | Trạng thái |
| --------- | ------------ | ------- | -------------- | ---------- |
| 0.1       | [YYYY-MM-DD] | [Tên]   | Khởi tạo       | Draft      |

## 1.3 Review và phê duyệt **[BẮT BUỘC]**

| Vai trò                          | Người/đơn vị | Phạm vi review                    | Kết quả                     | Ngày   | Bằng chứng/link |
| -------------------------------- | ------------ | --------------------------------- | --------------------------- | ------ | --------------- |
| Business Owner                   | [Điền]       | Mục tiêu, phạm vi, ưu tiên        | [Pending/Approved/Rejected] | [Điền] | [Điền]          |
| Solution/Enterprise Architecture | [Điền]       | Kiến trúc, tiêu chuẩn, quyết định | [Điền]                      | [Điền] | [Điền]          |
| An toàn thông tin                | [Điền]       | Security, privacy, risk/control   | [Điền]                      | [Điền] | [Điền]          |
| Hạ tầng/Vận hành/SRE             | [Điền]       | Deployability, operability, DR    | [Điền]                      | [Điền] | [Điền]          |
| Data Owner/DPO                   | [Điền/N/A]   | Data governance, privacy          | [Điền]                      | [Điền] | [Điền]          |
| ARB                              | [Điền/N/A]   | Quyết định vượt thẩm quyền dự án  | [Điền]                      | [Điền] | [Điền]          |

## 1.4 Tài liệu tham chiếu **[BẮT BUỘC]**

| ID     | Tài liệu/tiêu chuẩn               | Phiên bản/ngày | Vai trò trong HLD          | Link   |
| ------ | --------------------------------- | -------------- | -------------------------- | ------ |
| REF-01 | Business Requirements/PRD         | [Điền]         | Nguồn yêu cầu              | [Điền] |
| REF-02 | Architecture Principles/Standards | [Điền]         | Ràng buộc kiến trúc        | [Điền] |
| REF-03 | NFR Baseline                      | [Điền]         | Baseline chất lượng        | [Điền] |
| REF-04 | Security/Privacy Policy           | [Điền]         | Baseline kiểm soát         | [Điền] |
| REF-05 | API/Data/Integration Standard     | [Điền]         | Chuẩn giao tiếp và dữ liệu | [Điền] |

## 1.5 Thuật ngữ và từ viết tắt **[BẮT BUỘC]**

| Thuật ngữ | Định nghĩa trong phạm vi tài liệu     |
| --------- | ------------------------------------- |
| [Điền]    | [Điền; tránh chỉ mở rộng từ viết tắt] |

---

# 2. Tóm tắt điều hành

## 2.1 Vấn đề và giá trị giải pháp **[BẮT BUỘC]**

[Trong 1–3 đoạn, mô tả vấn đề hiện tại, đối tượng chịu ảnh hưởng, giá trị mong đợi và lý do cần thay đổi ở thời điểm này. Không mô tả chi tiết kỹ thuật tại đây.]

## 2.2 Giải pháp đề xuất **[BẮT BUỘC]**

[Mô tả ngắn cách tiếp cận, phạm vi thay đổi lớn, thành phần mới/thay đổi/tái sử dụng và điểm khác biệt chính so với hiện trạng.]

## 2.3 Kết quả và chỉ số thành công **[BẮT BUỘC]**

| ID    | Kết quả mong đợi | Chỉ số/đơn vị | Baseline | Target | Thời điểm đo | Owner  |
| ----- | ---------------- | ------------- | -------- | ------ | ------------ | ------ |
| BR-01 | [Điền]           | [Điền]        | [Điền]   | [Điền] | [Điền]       | [Điền] |

## 2.4 Tóm tắt phạm vi **[BẮT BUỘC]**

| Trong phạm vi | Ngoài phạm vi | Giai đoạn sau |
| ------------- | ------------- | ------------- |
| [Điền]        | [Điền]        | [Điền]        |

## 2.5 Tác động và quyết định cần phê duyệt **[BẮT BUỘC]**

| Hạng mục                                   | Tóm tắt                        |
| ------------------------------------------ | ------------------------------ |
| Đơn vị/người dùng bị ảnh hưởng             | [Điền]                         |
| Hệ thống/dữ liệu bị ảnh hưởng              | [Điền]                         |
| Thay đổi quy trình vận hành                | [Điền]                         |
| Rủi ro cao nhất                            | [Điền]                         |
| Chi phí/capacity chính                     | [Điền hoặc link business case] |
| Quyết định cần cấp có thẩm quyền phê duyệt | [Điền hoặc N/A]                |

---

# 3. Bối cảnh và architecture drivers

## 3.1 Mục đích, phạm vi và đối tượng đọc **[BẮT BUỘC]**

### Mục đích tài liệu

[Điền mục đích cụ thể của HLD và quyết định mà tài liệu hỗ trợ.]

### Phạm vi hệ thống

[Định nghĩa system-of-interest, ranh giới trách nhiệm và vòng đời/giai đoạn được bao phủ.]

### Đối tượng đọc

| Nhóm đọc                      | Quyết định/concern cần tài liệu trả lời |
| ----------------------------- | --------------------------------------- |
| Business/Product Owner        | [Điền]                                  |
| Architect/ARB                 | [Điền]                                  |
| Engineering                   | [Điền]                                  |
| Security/Privacy              | [Điền]                                  |
| Infrastructure/Operations/SRE | [Điền]                                  |
| QA/Compliance/Audit           | [Điền]                                  |

## 3.2 Stakeholder, concern, viewpoint và view **[BẮT BUỘC]**

### 3.2.1 Stakeholder–concern–view map

> Mỗi concern quan trọng phải được giải quyết bởi ít nhất một view, quyết định, control hoặc tài liệu tham chiếu. Không tạo view/sơ đồ nếu không biết nó trả lời concern nào.

| ID     | Stakeholder | Concern/câu hỏi | Mức ưu tiên       | View/section giải quyết | Acceptance/evidence | Owner xác nhận | Trạng thái                |
| ------ | ----------- | --------------- | ----------------- | ----------------------- | ------------------- | -------------- | ------------------------- |
| CON-01 | [Điền]      | [Điền]          | [High/Medium/Low] | [VIEW/Section/ADR/CTRL] | [Điền]              | [Điền]         | [Open/Addressed/Accepted] |

### 3.2.2 Viewpoint catalog và correspondence rules

Viewpoint định nghĩa **cách tạo và diễn giải** một view; view là **biểu diễn cụ thể** của kiến trúc cho một tập concern. Chỉ khai báo viewpoint thực sự dùng trong HLD. Có thể tham chiếu viewpoint chuẩn nội bộ thay vì lặp lại toàn bộ quy ước.

| Viewpoint ID | Tên/mục đích | Stakeholder và concern | Model kind/notation              | Quy ước tạo/đọc                                              | Correspondence/consistency rules                                       | View/model tạo ra     |
| ------------ | ------------ | ---------------------- | -------------------------------- | ------------------------------------------------------------ | ---------------------------------------------------------------------- | --------------------- |
| VP-01        | [Điền]       | [CON-xx]               | [C4/UML/ArchiMate/DFD/Table/...] | [Legend, mức trừu tượng, direction, bắt buộc/không cho phép] | [Quy tắc đối chiếu ID, ownership, interface, data class giữa các view] | [VIEW-xx, HLD-DIA-xx] |

Mọi correspondence rule bị vi phạm có chủ đích phải được nêu rõ trong view liên quan và dẫn tới ADR, risk hoặc open issue; không hợp thức hóa mâu thuẫn bằng cách đổi tên cùng một phần tử giữa các sơ đồ.

## 3.3 Yêu cầu kiến trúc và truy vết nguồn **[BẮT BUỘC]**

| ID    | Yêu cầu | Nguồn                      | Ưu tiên             | Tiêu chí chấp nhận   | Thành phần/view liên quan |
| ----- | ------- | -------------------------- | ------------------- | -------------------- | ------------------------- |
| AR-01 | [Điền]  | [BR/Policy/Regulation/ADR] | [Must/Should/Could] | [Đo/kiểm chứng được] | [CMP/DIA/Section]         |

## 3.4 Giả định **[BẮT BUỘC]**

| ID     | Giả định | Cơ sở  | Owner xác nhận | Hạn xác nhận | Tác động nếu sai | Trạng thái               |
| ------ | -------- | ------ | -------------- | ------------ | ---------------- | ------------------------ |
| ASM-01 | [Điền]   | [Điền] | [Điền]         | [Điền]       | [Điền]           | [Open/Validated/Invalid] |

Không phê duyệt HLD nếu một giả định chưa xác nhận có thể làm thay đổi kiến trúc nền tảng, mô hình dữ liệu, ranh giới tin cậy hoặc RTO/RPO.

## 3.5 Ràng buộc **[BẮT BUỘC]**

| ID     | Ràng buộc | Loại                                           | Nguồn/owner | Hệ quả kiến trúc | Có thể thay đổi?     |
| ------ | --------- | ---------------------------------------------- | ----------- | ---------------- | -------------------- |
| CST-01 | [Điền]    | [Business/Legal/Technology/Time/Budget/Legacy] | [Điền]      | [Điền]           | [Có/Không/Điều kiện] |

## 3.6 Nguyên tắc và tiêu chuẩn áp dụng **[BẮT BUỘC]**

| ID     | Nguyên tắc/tiêu chuẩn | Cách giải pháp tuân thủ | Ngoại lệ/waiver           |
| ------ | --------------------- | ----------------------- | ------------------------- |
| STD-01 | [Điền]                | [Điền]                  | [N/A hoặc link phê duyệt] |

## 3.7 Các phương án đã xem xét **[BẮT BUỘC đối với quyết định lớn]**

| Phương án | Sourcing/ownership        | Mô tả  | Lợi ích | Hạn chế/rủi ro                  | Chi phí vòng đời/độ phức tạp    | Kết luận          |
| --------- | ------------------------- | ------ | ------- | ------------------------------- | ------------------------------- | ----------------- |
| A         | [Reuse/Build/Buy/Partner] | [Điền] | [Điền]  | [Điền, gồm lock-in/exit nếu có] | [Build + run + change + retire] | [Chọn/Không chọn] |
| B         | [Reuse/Build/Buy/Partner] | [Điền] | [Điền]  | [Điền, gồm lock-in/exit nếu có] | [Build + run + change + retire] | [Chọn/Không chọn] |

[Giải thích ngắn tiêu chí và trade-off dẫn đến phương án được chọn, bao gồm năng lực nội bộ, time-to-value, tổng chi phí sở hữu, supplier risk và khả năng thoát/chuyển đổi. Tạo ADR cho quyết định khó đảo ngược hoặc ảnh hưởng nhiều hệ thống/team.]

---

# 4. Kiến trúc hiện tại, đích và khoảng cách

## 4.1 Baseline architecture **[CÓ ĐIỀU KIỆN: thay đổi hệ thống hiện hữu]**

### 4.1.1 Baseline context/landscape

> **HLD-DIA-01 — Baseline System Context/Landscape**  
> [Chèn sơ đồ hoặc link nguồn sơ đồ. Chỉ thể hiện thành phần và quan hệ cần thiết để hiểu thay đổi.]

### 4.1.2 Vấn đề của hiện trạng

| ID     | Vấn đề | Bằng chứng                            | Ảnh hưởng | Root cause/constraint liên quan |
| ------ | ------ | ------------------------------------- | --------- | ------------------------------- |
| GAP-01 | [Điền] | [Metric/incident/audit/user feedback] | [Điền]    | [Điền]                          |

## 4.2 Target architecture **[BẮT BUỘC]**

> **HLD-DIA-02 — Target Solution Overview**  
> [Chèn sơ đồ tổng quan giải pháp đích; phân biệt rõ thành phần mới, thay đổi, giữ nguyên, ngừng sử dụng.]

[Mô tả 5–10 quyết định/đặc điểm quan trọng nhất của target architecture. Không lặp lại nội dung có thể đọc trực tiếp từ sơ đồ.]

## 4.3 Gap analysis **[CÓ ĐIỀU KIỆN: có baseline]**

| ID     | Miền/thành phần | Baseline | Target | Gap    | Hành động/work package | Owner  |
| ------ | --------------- | -------- | ------ | ------ | ---------------------- | ------ |
| GAP-01 | [Điền]          | [Điền]   | [Điền] | [Điền] | [WP-xx]                | [Điền] |

## 4.4 Transition architecture **[CÓ ĐIỀU KIỆN: chuyển đổi nhiều giai đoạn]**

| Giai đoạn | Trạng thái kiến trúc | Thành phần tạm thời | Rủi ro/giới hạn | Exit criteria |
| --------- | -------------------- | ------------------- | --------------- | ------------- |
| T1        | [Điền]               | [Điền/N/A]          | [Điền]          | [Đo được]     |

---

# 5. Các architecture view

## 5.1 Quy chuẩn sơ đồ **[BẮT BUỘC]**

Mỗi architecture view và model/diagram thuộc view phải có:

- `VIEW-xx`, tên view, `VP-xx` được áp dụng, concern được giải quyết và phạm vi;
- ID/tên riêng cho từng model hoặc diagram (`HLD-DIA-xx`) và trạng thái `Baseline`, `Transition` hoặc `Target`;
- mức trừu tượng nhất quán (không trộn system, class và server instance trong cùng một model nếu không có chú giải);
- legend cho màu, kiểu đường, ký hiệu và trạng thái thành phần;
- owner/đơn vị vận hành của thành phần; không suy luận trust boundary chỉ từ vị trí mạng;
- hướng giao tiếp, protocol/interaction style và sync/async khi có ý nghĩa;
- trust zone, ranh giới tổ chức và data classification khi liên quan;
- ID phần tử ổn định và dùng nhất quán giữa context, logical, runtime, data, deployment và control view;
- kiểm tra correspondence rules tại mục 3.2.2; mọi khác biệt có chủ đích phải dẫn tới ADR, risk hoặc open issue;
- nguồn có thể chỉnh sửa (`.drawio`, DSL, Mermaid...) được lưu cùng tài liệu hoặc có link;
- bảng mô tả ngay dưới model/diagram cho thông tin không nên nhồi vào hình.

Khuyến nghị sử dụng C4 System Context/Container cho cấu trúc tổng quan; UML/ArchiMate/Draw.io/Mermaid cho view chuyên biệt. Component-level chỉ dùng khi cần khóa một ranh giới quan trọng cho nhiều nhóm; chi tiết hơn chuyển sang LLD.

## 5.2 Business and process view **[CÓ ĐIỀU KIỆN: có thay đổi capability, actor hoặc quy trình]**

### 5.2.1 Business capabilities/actors

| ID     | Capability/actor | Trách nhiệm | Thay đổi                        | Owner  |
| ------ | ---------------- | ----------- | ------------------------------- | ------ |
| BUS-01 | [Điền]           | [Điền]      | [New/Changed/Unchanged/Retired] | [Điền] |

### 5.2.2 High-level process/workflow

> **HLD-DIA-03 — Business Process/Workflow**  
> [Thể hiện happy path, điểm quyết định và hand-off quan trọng; không đưa logic màn hình hoặc xử lý code-level.]

| Bước | Actor  | Input  | Hoạt động/kết quả | Business rule | Ngoại lệ chính |
| ---- | ------ | ------ | ----------------- | ------------- | -------------- |
| 1    | [Điền] | [Điền] | [Điền]            | [BR-xx]       | [Điền]         |

## 5.3 System context view **[BẮT BUỘC]**

> **HLD-DIA-04 — System Context**  
> [System-of-interest ở trung tâm; thể hiện người dùng, external systems, organizational ownership và quan hệ chính.]

| External actor/system | Owner/trust domain | Mục đích tương tác | Dữ liệu trao đổi | Criticality            |
| --------------------- | ------------------ | ------------------ | ---------------- | ---------------------- |
| [Điền]                | [Điền]             | [Điền]             | [DATA-xx]        | [Critical/High/Normal] |

## 5.4 Logical application/container view **[BẮT BUỘC]**

> **HLD-DIA-05 — Logical Application/Container Architecture**  
> [Thể hiện subsystem/service/data store và quan hệ; không thể hiện class hoặc package.]

| ID     | Component/subsystem | Trách nhiệm duy nhất | Owns data     | Exposes/consumes | Technology/runtime       | Owner  |
| ------ | ------------------- | -------------------- | ------------- | ---------------- | ------------------------ | ------ |
| CMP-01 | [Điền]              | [Điền]               | [DATA-xx/N/A] | [INT-xx]         | [Điền hoặc TBD có owner] | [Điền] |

### Quy tắc boundary và dependency

- [Ai được gọi ai; dependency direction.]
- [Thành phần nào là system of record/source of truth.]
- [Ai được phép thay đổi shared state và qua giao diện nào.]
- [Những coupling/dependency bị cấm.]
- [Cơ chế tương thích phiên bản và nguyên tắc deprecation.]

## 5.5 Runtime/scenario view **[BẮT BUỘC cho luồng quan trọng]**

Chọn các scenario làm thay đổi quyết định kiến trúc: giao dịch chính, lỗi downstream, timeout/retry, duplicate/replay, partial failure, recovery, batch window hoặc failover. Không cần sequence diagram cho mọi use case.

> **HLD-DIA-06 — [Tên scenario]**  
> [Chèn sequence/activity/data-flow diagram. Với sequence diagram, tuân thủ guideline nội bộ và đặt bảng chi tiết ngay dưới hình.]

| Step | From        | To    | Interface | Sync/Async | Timeout | Retry/backoff | Idempotency | Error/fallback | Data classification |
| ---- | ----------- | ----- | --------- | ---------- | ------- | ------------- | ----------- | -------------- | ------------------- |
| 1    | [CMP/Actor] | [CMP] | [INT-xx]  | [Điền]     | [Điền]  | [Điền]        | [Điền]      | [Điền]         | [Điền]              |

## 5.6 Integration and interface view **[BẮT BUỘC]**

| ID     | Provider | Consumer | Purpose | Protocol/pattern          | AuthN/AuthZ | Contract/source                | SLA/SLO | Versioning | Data class |
| ------ | -------- | -------- | ------- | ------------------------- | ----------- | ------------------------------ | ------- | ---------- | ---------- |
| INT-01 | [Điền]   | [Điền]   | [Điền]  | [REST/Event/File/DB/etc.] | [Điền]      | [OpenAPI/AsyncAPI/schema/link] | [Điền]  | [Điền]     | [Điền]     |

### Integration policies

| Chủ đề                        | Quyết định mức cao                            |
| ----------------------------- | --------------------------------------------- |
| Timeout budget                | [End-to-end và phân bổ theo hop]              |
| Retry/backoff/jitter          | [Điền; chỉ retry thao tác an toàn/idempotent] |
| Idempotency/deduplication     | [Điền]                                        |
| Ordering/consistency          | [Điền]                                        |
| Rate limit/quota/backpressure | [Điền]                                        |
| Circuit breaker/bulkhead      | [Điền]                                        |
| Error model/correlation ID    | [Điền]                                        |
| Contract compatibility        | [Điền]                                        |
| Dead-letter/reconciliation    | [Điền/N/A]                                    |

## 5.7 Data architecture view **[BẮT BUỘC]**

### 5.7.1 Conceptual/logical data model

> **HLD-DIA-07 — Conceptual/Logical Data Model**  
> [Chỉ thể hiện domain/entity và quan hệ quan trọng; physical schema thuộc LLD.]

| ID      | Data domain/entity | Business owner | System of record | Producer/consumer | Classification                            | Residency | Retention/disposal         |
| ------- | ------------------ | -------------- | ---------------- | ----------------- | ----------------------------------------- | --------- | -------------------------- |
| DATA-01 | [Điền]             | [Điền]         | [CMP-xx]         | [CMP/INT]         | [Public/Internal/Confidential/Restricted] | [Điền]    | [Policy/thời hạn/cách xóa] |

### 5.7.2 Data flow and lifecycle

> **HLD-DIA-08 — Data Flow/Lifecycle**  
> [Thể hiện collection → processing → storage → sharing → archival → deletion; đánh dấu crossing trust/organization boundary.]

| Flow ID | Source → Destination | Data      | Purpose/legal basis | Transform  | Encryption        | Retention | Reconciliation |
| ------- | -------------------- | --------- | ------------------- | ---------- | ----------------- | --------- | -------------- |
| DF-01   | [Điền]               | [DATA-xx] | [Điền]              | [Điền/N/A] | [Transit/At rest] | [Điền]    | [Điền]         |

### 5.7.3 Data consistency, quality and migration

| Chủ đề                   | Quyết định                             |
| ------------------------ | -------------------------------------- |
| Consistency model        | [Strong/Eventual/Session/... và lý do] |
| Transaction boundary     | [Điền]                                 |
| Master/reference data    | [Owner và cách đồng bộ]                |
| Data quality rules       | [Metric, threshold, owner]             |
| Lineage/auditability     | [Điền]                                 |
| Schema evolution         | [Điền]                                 |
| Migration/reconciliation | [Link mục 9 hoặc N/A]                  |

## 5.8 Technology, deployment and network view **[BẮT BUỘC]**

> **HLD-DIA-09 — Deployment/Technology Architecture**  
> [Thể hiện environment, region/site, zone, cluster/node class, network/security zone, ingress/egress, shared platform và dependency hạ tầng. Không đưa instance ID hoặc secret.]

| Node/platform | Environment       | Location/zone | Workload | HA/scaling | Network/trust zone | Managed by | Version/lifecycle |
| ------------- | ----------------- | ------------- | -------- | ---------- | ------------------ | ---------- | ----------------- |
| [Điền]        | [DEV/UAT/PROD/DR] | [Điền]        | [CMP-xx] | [Điền]     | [Điền]             | [Điền]     | [Điền]            |

### Technology choices

| Layer/capability | Technology/service | Version policy        | Lý do/ADR | Standard status              | Exit/replacement trigger |
| ---------------- | ------------------ | --------------------- | --------- | ---------------------------- | ------------------------ |
| [Điền]           | [Điền]             | [Pinned/LTS/N-1/etc.] | [ADR-xxx] | [Adopt/Trial/Hold/Exception] | [Điền]                   |

## 5.9 Environment and configuration strategy **[BẮT BUỘC]**

| Chủ đề                              | DEV                                     | TEST/UAT                        | PROD                                              | DR                                                 |
| ----------------------------------- | --------------------------------------- | ------------------------------- | ------------------------------------------------- | -------------------------------------------------- |
| Topology parity                     | [Điền]                                  | [Điền]                          | [Điền]                                            | [Điền]                                             |
| Data policy                         | [Synthetic/Masked/etc.]                 | [Điền]                          | [Điền]                                            | [Điền]                                             |
| Configuration source                | [Điền]                                  | [Điền]                          | [Điền]                                            | [Điền]                                             |
| Secrets/key ownership               | [Không ghi secret; ghi cơ chế và owner] | [Điền]                          | [Điền]                                            | [Điền]                                             |
| External dependency strategy        | [Mock/Sandbox/Real]                     | [Điền]                          | [Điền]                                            | [Điền]                                             |
| Build/artifact source và provenance | [Repo/pipeline/artifact ID]             | [Promotion rule/SBOM/signature] | [Immutable artifact/trusted registry/attestation] | [Cùng artifact hoặc quy tắc tái tạo đã kiểm chứng] |
| IaC/deployment mechanism            | [Điền]                                  | [Điền]                          | [Approval/separation of duties/rollback]          | [Điền]                                             |
| Policy và promotion gates           | [Điền]                                  | [Test/security/license gates]   | [Go/no-go authority + evidence]                   | [DR activation authority + evidence]               |

---

# 6. Quality attributes và yêu cầu phi chức năng

## 6.1 Quality model coverage và priorities **[BẮT BUỘC]**

Dùng mô hình chất lượng sản phẩm của **ISO/IEC 25010:2023** như một completeness lens, không phải checklist buộc mọi thuộc tính đều có mức ưu tiên cao. Với từng nhóm dưới đây, ghi scenario ID hoặc `N/A — <lý do>`. Các concern mở rộng như privacy, resilience, operability, sustainability và cost vẫn phải xem xét khi bối cảnh yêu cầu, dù không phải tên characteristic độc lập trong mô hình sản phẩm.

| Quality characteristic/concern       | Áp dụng?                                      | Scenario/section chứng minh | Ghi chú/trade-off                                        |
| ------------------------------------ | --------------------------------------------- | --------------------------- | -------------------------------------------------------- |
| Functional suitability               | [Yes/No + lý do]                              | [BR/AR/NFR/Section]         | [Điền]                                                   |
| Performance efficiency               | [Yes/No + lý do]                              | [NFR/6.3]                   | [Điền]                                                   |
| Compatibility/interoperability       | [Yes/No + lý do]                              | [NFR/INT/5.6]               | [Điền]                                                   |
| Interaction capability/accessibility | [Yes/No + lý do]                              | [NFR/6.6]                   | [Điền]                                                   |
| Reliability/resilience               | [Yes/No + lý do]                              | [NFR/6.4]                   | [Điền]                                                   |
| Security                             | [Yes/No + lý do]                              | [NFR/CTRL/7]                | [Điền]                                                   |
| Maintainability                      | [Yes/No + lý do]                              | [NFR/6.5]                   | [Điền]                                                   |
| Flexibility/portability              | [Yes/No + lý do]                              | [NFR/ADR/5.8]               | [Điền]                                                   |
| Safety/harm avoidance                | [Yes/No + lý do]                              | [NFR/RISK/CTRL]             | [Con người, tài sản, dữ liệu, hoạt động hoặc môi trường] |
| Concern mở rộng theo bối cảnh        | [Privacy/Operability/Cost/Sustainability/...] | [Section/NFR]               | [Điền]                                                   |

Sau khi kiểm tra độ bao phủ, xếp hạng các thuộc tính thực sự chi phối thiết kế. Không tuyên bố mọi thuộc tính đều “cao”. Mỗi thuộc tính ưu tiên phải có quality scenario đo được.

| Rank | Quality attribute                        | Business rationale | Trade-off chấp nhận | Owner  |
| ---- | ---------------------------------------- | ------------------ | ------------------- | ------ |
| 1    | [Availability/Performance/Security/etc.] | [Điền]             | [Điền]              | [Điền] |

## 6.2 Quality scenarios **[BẮT BUỘC]**

Dùng cấu trúc: **Source → Stimulus → Environment → Artifact → Response → Response measure**.

| ID         | Source | Stimulus | Environment           | Artifact           | Expected response | Measure/threshold  | Verification            |
| ---------- | ------ | -------- | --------------------- | ------------------ | ----------------- | ------------------ | ----------------------- |
| NFR-AVL-01 | [Điền] | [Điền]   | [Normal/Peak/Failure] | [System/component] | [Điền]            | [SLO/RTO/RPO/etc.] | [Test/monitor/evidence] |

## 6.3 Performance, scalability and capacity **[BẮT BUỘC]**

| Metric/workload                      | Current/baseline | Forecast horizon | Normal | Peak/burst | Target                        | Test model/source |
| ------------------------------------ | ---------------- | ---------------- | ------ | ---------- | ----------------------------- | ----------------- |
| TPS/RPS/concurrent users/data volume | [Điền]           | [6/12/36 tháng]  | [Điền] | [Điền]     | [p95/p99 latency, throughput] | [Điền]            |

| Chủ đề                         | Quyết định |
| ------------------------------ | ---------- |
| Scaling unit and trigger       | [Điền]     |
| Capacity headroom              | [Điền]     |
| Bottleneck/resource limit      | [Điền]     |
| Load shedding/backpressure     | [Điền]     |
| Performance degradation policy | [Điền]     |

## 6.4 Availability, resilience and disaster recovery **[BẮT BUỘC]**

| Service/data tier | Availability SLO | RTO    | RPO    | Failure domain              | Redundancy/failover  | Backup/restore | DR test frequency |
| ----------------- | ---------------- | ------ | ------ | --------------------------- | -------------------- | -------------- | ----------------- |
| [Điền]            | [Điền]           | [Điền] | [Điền] | [Process/Node/AZ/Site/etc.] | [Auto/Manual + time] | [Điền]         | [Điền]            |

### Failure mode summary

| Failure | Detection | Containment | Degraded behavior | Recovery | Owner/runbook |
| ------- | --------- | ----------- | ----------------- | -------- | ------------- |
| [Điền]  | [Điền]    | [Điền]      | [Điền]            | [Điền]   | [Điền]        |

## 6.5 Maintainability, testability and evolvability **[BẮT BUỘC]**

| Concern                    | Quyết định/target                |
| -------------------------- | -------------------------------- |
| Independent deployability  | [Điền]                           |
| Backward compatibility     | [Điền]                           |
| Test pyramid/contract test | [Mức chiến lược, link test plan] |
| Feature flag/configuration | [Điền]                           |
| Upgrade/patch policy       | [Điền]                           |
| Technical debt budget      | [Điền]                           |
| End-of-life/deprecation    | [Điền]                           |

## 6.6 Usability and accessibility **[CÓ ĐIỀU KIỆN: có kênh người dùng]**

| ID        | User/context | Requirement/standard | Measure | Verification |
| --------- | ------------ | -------------------- | ------- | ------------ |
| NFR-UX-01 | [Điền]       | [Điền]               | [Điền]  | [Điền]       |

## 6.7 Cost efficiency and sustainability **[CÓ ĐIỀU KIỆN: kiến trúc làm thay đổi đáng kể chi phí/tài nguyên]**

| Cost driver                            | Sizing assumption | Unit cost/model | Guardrail/budget | Monitoring/owner |
| -------------------------------------- | ----------------- | --------------- | ---------------- | ---------------- |
| [Compute/Storage/License/Network/etc.] | [Điền]            | [Điền]          | [Điền]           | [Điền]           |

---

# 7. Security, privacy và compliance architecture

## 7.1 Security/privacy context **[BẮT BUỘC]**

| Nội dung                           | Mô tả                                             |
| ---------------------------------- | ------------------------------------------------- |
| Tài sản cần bảo vệ                 | [Data, service, key, transaction, audit trail...] |
| Threat actors/threat events chính  | [Điền hoặc link threat model]                     |
| Ranh giới tổ chức/trust domain     | [Điền; xác nhận owner/operator từng thành phần]   |
| Dữ liệu cá nhân/nhạy cảm           | [Điền hoặc N/A]                                   |
| Quy định/chính sách áp dụng        | [Điền]                                            |
| Risk appetite/acceptance authority | [Điền]                                            |

> Trust boundary phải dựa trên **ownership, control plane, identity, policy enforcement và mức tin cậy**, không chỉ dựa trên network segment.

## 7.2 Identity and access architecture **[BẮT BUỘC]**

| Actor/workload | Identity source | Authentication       | Authorization model | Privilege boundary | Session/token policy | Owner  |
| -------------- | --------------- | -------------------- | ------------------- | ------------------ | -------------------- | ------ |
| [Điền]         | [Điền]          | [MFA/mTLS/OIDC/etc.] | [RBAC/ABAC/etc.]    | [Điền]             | [Điền]               | [Điền] |

## 7.3 Security/privacy control matrix **[BẮT BUỘC]**

| ID      | Risk/requirement | Prevent/Detect/Respond/Recover | Control design | Enforcement point  | Owner  | Evidence/verification       | Residual risk |
| ------- | ---------------- | ------------------------------ | -------------- | ------------------ | ------ | --------------------------- | ------------- |
| CTRL-01 | [RISK/AR/NFR]    | [Điền]                         | [Điền]         | [CMP/INT/Platform] | [Điền] | [Test/config/audit/monitor] | [Điền]        |

Tối thiểu xem xét:

- least privilege, segregation of duties, privileged access;
- encryption in transit/at rest, key/secret/certificate lifecycle;
- API/service protection, input validation, replay/abuse/rate-limit controls;
- network segmentation và egress control;
- secure configuration, vulnerability/patch/dependency management;
- software supply chain: trusted source/repository, CI/CD identity, SBOM, provenance/attestation, artifact signing và promotion policy khi áp dụng;
- audit logging, integrity, time synchronization và non-repudiation khi cần;
- data minimization, purpose limitation, consent/legal basis, retention/deletion và data-subject rights khi áp dụng;
- incident detection, containment, forensics và notification;
- third-party concentration, contractual, data-access và exit risk.

## 7.4 Cryptography and secrets **[BẮT BUỘC]**

| Use case | Data/key                       | In transit/at rest/in use | Mechanism/profile                 | Key owner/custodian | Rotation/revocation | Exception         |
| -------- | ------------------------------ | ------------------------- | --------------------------------- | ------------------- | ------------------- | ----------------- |
| [Điền]   | [Không ghi giá trị secret/key] | [Điền]                    | [Algorithm/profile theo standard] | [Điền]              | [Điền]              | [N/A/link waiver] |

## 7.5 Logging, audit and privacy **[BẮT BUỘC]**

| Event category                   | Purpose | Required fields                          | Prohibited/masked data | Retention | Access/integrity | Consumer             |
| -------------------------------- | ------- | ---------------------------------------- | ---------------------- | --------- | ---------------- | -------------------- |
| [Security/Business/Admin/System] | [Điền]  | [Correlation, actor, action, outcome...] | [Secret/PII/etc.]      | [Điền]    | [Điền]           | [SOC/Audit/Ops/etc.] |

## 7.6 Compliance mapping **[CÓ ĐIỀU KIỆN: có regulatory/policy obligations]**

| Obligation/control ref | Applicability            | Architecture response | Evidence | Gap/waiver | Owner  |
| ---------------------- | ------------------------ | --------------------- | -------- | ---------- | ------ |
| [Điền]                 | [Applicable/N/A + lý do] | [CTRL/Section/ADR]    | [Điền]   | [Điền]     | [Điền] |

---

# 8. Observability, vận hành và service readiness

## 8.1 Service ownership and support model **[BẮT BUỘC]**

| Service/component | Business owner | Technical owner | Operations owner | Support hours/tier | Escalation | Vendor dependency |
| ----------------- | -------------- | --------------- | ---------------- | ------------------ | ---------- | ----------------- |
| [Điền]            | [Điền]         | [Điền]          | [Điền]           | [24x7/8x5/etc.]    | [Điền]     | [Điền/N/A]        |

## 8.2 Observability architecture **[BẮT BUỘC]**

| Signal  | Source/instrumentation | Collection/path | Storage/retention | Key SLI/use                       | Alert/dashboard owner |
| ------- | ---------------------- | --------------- | ----------------- | --------------------------------- | --------------------- |
| Metrics | [Điền]                 | [Điền]          | [Điền]            | [Availability/latency/etc.]       | [Điền]                |
| Logs    | [Điền]                 | [Điền]          | [Điền]            | [Điền]                            | [Điền]                |
| Traces  | [Điền/N/A]             | [Điền]          | [Điền]            | [Điền]                            | [Điền]                |
| Events  | [Điền]                 | [Điền]          | [Điền]            | [Deploy/config/business/security] | [Điền]                |

### Alerting principles

| Condition | Signal/threshold | Severity    | Window | Routing | Automated/manual response | Runbook               |
| --------- | ---------------- | ----------- | ------ | ------- | ------------------------- | --------------------- |
| [Điền]    | [Điền]           | [Sev1–Sev4] | [Điền] | [Điền]  | [Điền]                    | [Link/TBD owner+date] |

## 8.3 Operability **[BẮT BUỘC]**

| Capability                  | Thiết kế/đầu ra | Owner  | Ready criteria             |
| --------------------------- | --------------- | ------ | -------------------------- |
| Health/readiness checks     | [Điền]          | [Điền] | [Điền]                     |
| Start/stop/restart          | [Điền]          | [Điền] | [Điền]                     |
| Configuration change        | [Điền]          | [Điền] | [Audit/rollback]           |
| Deployment/rollback         | [Điền]          | [Điền] | [Điền]                     |
| Backup/restore              | [Điền]          | [Điền] | [Restore test đạt RTO/RPO] |
| Capacity management         | [Điền]          | [Điền] | [Điền]                     |
| Certificate/secret rotation | [Điền]          | [Điền] | [Điền]                     |
| Data repair/reconciliation  | [Điền]          | [Điền] | [Điền]                     |
| Incident/problem management | [Điền]          | [Điền] | [Điền]                     |

## 8.4 Dependency and failure ownership **[BẮT BUỘC]**

| Dependency | Owner/SLA | Failure symptom | Detection | Fallback/degradation | Escalation | Exit strategy |
| ---------- | --------- | --------------- | --------- | -------------------- | ---------- | ------------- |
| [Điền]     | [Điền]    | [Điền]          | [Điền]    | [Điền]               | [Điền]     | [Điền]        |

---

# 9. Migration, triển khai và roadmap

## 9.1 Work packages and roadmap **[BẮT BUỘC]**

| ID    | Work package/milestone | Scope/outcome | Dependency | Owner  | Target | Entry/exit criteria |
| ----- | ---------------------- | ------------- | ---------- | ------ | ------ | ------------------- |
| WP-01 | [Điền]                 | [Điền]        | [Điền]     | [Điền] | [Điền] | [Đo được]           |

## 9.2 Data migration **[CÓ ĐIỀU KIỆN: chuyển đổi dữ liệu]**

| Dataset   | Source → Target | Volume/window | Mapping/quality | Reconciliation     | Rollback | Owner  |
| --------- | --------------- | ------------- | --------------- | ------------------ | -------- | ------ |
| [DATA-xx] | [Điền]          | [Điền]        | [Link rule]     | [Metric/threshold] | [Điền]   | [Điền] |

## 9.3 Release, cutover and rollback **[BẮT BUỘC]**

| Chủ đề                     | Quyết định/kế hoạch mức cao               |
| -------------------------- | ----------------------------------------- |
| Release strategy           | [Rolling/Blue-green/Canary/Big bang/etc.] |
| Feature activation         | [Điền]                                    |
| Compatibility window       | [Điền]                                    |
| Cutover prerequisites      | [Điền]                                    |
| Go/no-go authority         | [Điền]                                    |
| Rollback trigger           | [Metric/threshold/timebox]                |
| Rollback/recovery approach | [Điền]                                    |
| Hypercare and handover     | [Thời gian, owner, exit criteria]         |

## 9.4 Decommissioning **[CÓ ĐIỀU KIỆN: thay thế/ngừng hệ thống]**

| Asset/service | Dependency check | Data archive/delete | Access/key revoke | Monitoring/billing stop | Approval/evidence |
| ------------- | ---------------- | ------------------- | ----------------- | ----------------------- | ----------------- |
| [Điền]        | [Điền]           | [Điền]              | [Điền]            | [Điền]                  | [Điền]            |

---

# 10. Quyết định, rủi ro và vấn đề mở

## 10.1 Architecture Decision Record index **[BẮT BUỘC]**

| ADR     | Decision | Status                                    | Scope/binds | Trade-off/consequence | Link   |
| ------- | -------- | ----------------------------------------- | ----------- | --------------------- | ------ |
| ADR-001 | [Điền]   | [Proposed/Accepted/Deprecated/Superseded] | [Điền]      | [Điền]                | [Điền] |

Một ADR tối thiểu có: **Context → Decision → Alternatives → Consequences → Status**. Không chép toàn bộ ADR vào HLD; HLD chỉ tóm tắt và tham chiếu.

## 10.2 Risk and control register **[BẮT BUỘC]**

| ID      | Risk statement (cause–event–impact) | Likelihood                    | Impact                        | Inherent rating | Treatment/control                            | Residual rating | Acceptance authority/expiry             | Owner  | Due/status |
| ------- | ----------------------------------- | ----------------------------- | ----------------------------- | --------------- | -------------------------------------------- | --------------- | --------------------------------------- | ------ | ---------- |
| RISK-01 | [Điền]                              | [Theo enterprise risk matrix] | [Theo enterprise risk matrix] | [Điền]          | [Avoid/Reduce/Transfer/Accept + CTRL/WP/ADR] | [Điền]          | [Đúng thẩm quyền + ngày review/hết hạn] | [Điền] | [Điền]     |

Không tự tạo thang điểm riêng trong HLD. Dẫn chiếu đúng enterprise risk matrix, risk appetite và cấp có thẩm quyền đang hiệu lực; nếu chấp nhận rủi ro có thời hạn, phải có trigger review trước ngày hết hạn.

## 10.3 Technical debt **[BẮT BUỘC nếu chấp nhận giải pháp tạm thời]**

| ID    | Debt/compromise | Lý do chấp nhận | Tác động | Revisit trigger/deadline | Owner  | Backlog link |
| ----- | --------------- | --------------- | -------- | ------------------------ | ------ | ------------ |
| TD-01 | [Điền]          | [Điền]          | [Điền]   | [Không dùng “sau này”]   | [Điền] | [Điền]       |

## 10.4 Open issues and decisions pending **[BẮT BUỘC]**

| ID    | Câu hỏi/vấn đề | Kiến trúc bị ảnh hưởng | Options | Decision owner | Due date | Blocker? | Status/outcome |
| ----- | -------------- | ---------------------- | ------- | -------------- | -------- | -------- | -------------- |
| OI-01 | [Điền]         | [Section/CMP/INT]      | [Điền]  | [Điền]         | [Điền]   | [Yes/No] | [Open/Closed]  |

Không để issue `Blocker = Yes` mở khi chuyển trạng thái tài liệu sang Approved.

---

# 11. Assurance, verification, validation và governance

## 11.1 Requirement–design–assurance traceability **[BẮT BUỘC]**

| Requirement | Concern/risk | Design response        | Component/interface/control | Verification/validation/evidence            | Owner  | Status                     |
| ----------- | ------------ | ---------------------- | --------------------------- | ------------------------------------------- | ------ | -------------------------- |
| [BR/AR/NFR] | [CON/RISK]   | [Section/VIEW/DIA/ADR] | [CMP/INT/CTRL]              | [Test/Review/Monitor/Audit/Outcome measure] | [Điền] | [Planned/Pass/Fail/Waived] |

## 11.2 Architecture verification and validation plan **[BẮT BUỘC]**

- **Verification** trả lời: thiết kế/giải pháp có đáp ứng đúng architecture requirements và đặc tả đã phê duyệt không?
- **Validation** trả lời: trong bối cảnh vận hành dự kiến, giải pháp có giải quyết đúng stakeholder needs và tạo outcome mong đợi không?

Không dùng một bài test kỹ thuật để thay thế validation nghiệp vụ. Với claim trọng yếu, xác định rõ loại assurance, môi trường đại diện, tiêu chí và bằng chứng trước khi phê duyệt.

| Architecture/business claim       | Loại           | Method                                                                            | Environment/context                       | Success criteria       | Evidence location | Owner                   | Timing                       |
| --------------------------------- | -------------- | --------------------------------------------------------------------------------- | ----------------------------------------- | ---------------------- | ----------------- | ----------------------- | ---------------------------- |
| [Ví dụ: failover đạt RTO]         | [Verification] | [Test/Inspection/Analysis/Demonstration; hoặc Examine/Interview/Test cho control] | [Điền]                                    | [Đo được]              | [Điền]            | [Điền]                  | [Pre-prod/Go-live/Post-live] |
| [Ví dụ: outcome BR-01 đạt target] | [Validation]   | [Pilot/UAT/Business metric/Operational exercise]                                  | [Đối tượng và điều kiện sử dụng đại diện] | [Target + thời hạn đo] | [Điền]            | [Business/System Owner] | [Pre-go-live/Post-live]      |

## 11.3 Architecture compliance and exceptions **[BẮT BUỘC]**

| Standard/principle | Compliance                      | Evidence | Deviation | Compensating control | Waiver owner/expiry |
| ------------------ | ------------------------------- | -------- | --------- | -------------------- | ------------------- |
| [STD-xx]           | [Comply/Partial/Not comply/N/A] | [Điền]   | [Điền]    | [Điền]               | [Điền]              |

## 11.4 Architecture governance checkpoints **[BẮT BUỘC]**

Phê duyệt HLD xác nhận hướng kiến trúc, không tự động đồng nghĩa LLD, implementation hoặc production readiness đã đạt. Tailor số gate theo hồ sơ áp dụng, nhưng phải giữ các điểm kiểm soát đủ để phát hiện design drift.

| Gate/checkpoint            | Timing/trigger                                          | Scope/claim được review                           | Evidence bắt buộc                     | Decision authority                     | Outcome/follow-up             |
| -------------------------- | ------------------------------------------------------- | ------------------------------------------------- | ------------------------------------- | -------------------------------------- | ----------------------------- |
| Architecture approval      | [Trước cam kết build/buy lớn]                           | [HLD, option, risk, NFR]                          | [HLD/ADR/assessment]                  | [Architect/ARB]                        | [Approve/Conditional/Reject]  |
| Design/conformance review  | [Trước implementation milestone hoặc khi đổi kiến trúc] | [LLD/API/schema/IaC so với HLD]                   | [Diff/traceability/test design]       | [Điền]                                 | [Deviation/ADR/waiver/action] |
| Production readiness       | [Trước go-live]                                         | [Control, operability, DR, cutover]               | [Test/evidence/runbook/approval]      | [Service Owner + chức năng liên quan]  | [Go/No-go/Conditional]        |
| Post-implementation review | [Sau go-live theo timebox]                              | [Outcome, SLO, residual risk, debt, design drift] | [Metric/incident/audit/user evidence] | [Business/System Owner + Architecture] | [Accept/Remediate/Update HLD] |

## 11.5 Definition of Ready for review **[BẮT BUỘC]**

- [ ] Mục tiêu, system-of-interest, in-scope/out-of-scope, hồ sơ tailoring và owner đã rõ.
- [ ] Stakeholder concerns đã được ánh xạ đến view/section/evidence.
- [ ] Viewpoint, view, model và correspondence rules cần thiết đã được định nghĩa hoặc tham chiếu.
- [ ] Yêu cầu, constraint, assumption và architecture drivers có nguồn.
- [ ] Target architecture, system context, logical/container, data và deployment view đã có.
- [ ] Baseline/gap/transition đã có hoặc ghi N/A kèm lý do.
- [ ] Integration catalog có owner, contract, security, SLO và failure policy.
- [ ] Quality model coverage đã được kiểm tra; NFR quan trọng được viết thành quality scenario đo được.
- [ ] Trust domain/ownership, security/privacy controls và residual risk đã rõ.
- [ ] RTO/RPO, backup/restore, observability, support và dependency failure đã rõ.
- [ ] Các phương án và quyết định khó đảo ngược có ADR.
- [ ] Mọi blocker có owner và due date.
- [ ] Source của model/diagram được lưu; ID/legend và cross-view consistency đã được kiểm tra.

## 11.6 Definition of Done/approval gate **[BẮT BUỘC]**

- [ ] Không còn assumption hoặc open issue có thể làm thay đổi kiến trúc nền tảng mà chưa được xử lý.
- [ ] Requirement–design–control–verification/validation traceability đầy đủ cho requirement trọng yếu.
- [ ] Security/privacy, Infrastructure/Operations và owner liên quan đã review.
- [ ] Rủi ro còn lại có owner và đúng cấp thẩm quyền chấp nhận.
- [ ] NFR/SLO/RTO/RPO/capacity có giá trị cụ thể và phương thức verification.
- [ ] Business outcome quan trọng có validation owner, target, thời điểm đo và follow-up gate.
- [ ] Migration, cutover, rollback, reconciliation và decommissioning đã đủ cho phạm vi.
- [ ] Compliance deviation có waiver, compensating control và ngày hết hạn.
- [ ] Link đến LLD/spec/API/schema/runbook/test plan/backlog đã tồn tại hoặc có owner + hạn hoàn thành.
- [ ] Lịch sử thay đổi, version, trạng thái và approvals được cập nhật.
- [ ] HLD được lưu trong source control và có governance checkpoint/review trigger sau go-live.

## 11.7 Trigger cập nhật HLD **[BẮT BUỘC]**

Cập nhật/review lại HLD khi xảy ra ít nhất một trong các điều kiện:

- thay đổi system boundary, ownership hoặc trust domain;
- thêm/bỏ subsystem, external integration hoặc system of record;
- thay đổi data classification, residency, retention hoặc mục đích xử lý;
- thay đổi deployment model, region/site, failure domain, RTO/RPO hoặc availability tier;
- thay đổi protocol, consistency model, authentication/authorization hoặc cryptography profile;
- thay đổi công nghệ nền tảng/phiên bản lớn làm thay đổi risk hoặc operating model;
- implementation/LLD không còn phù hợp với correspondence rule hoặc architecture decision đã phê duyệt;
- incident/audit/validation finding chứng minh assumption, outcome hoặc control design không còn đúng;
- ADR mới supersede quyết định được viện dẫn trong HLD.

---

# 12. Phụ lục

## 12.1 Danh mục deliverable liên quan **[BẮT BUỘC]**

| Deliverable                             | Bắt buộc khi                                              | Owner  | Link/status |
| --------------------------------------- | --------------------------------------------------------- | ------ | ----------- |
| BRD/PRD/Requirements                    | Mọi giải pháp                                             | [Điền] | [Điền]      |
| HLD diagram source                      | Có diagram                                                | [Điền] | [Điền]      |
| ADR                                     | Có quyết định lớn/trade-off                               | [Điền] | [Điền]      |
| Threat model/security assessment        | Có dữ liệu nhạy cảm, external exposure hoặc risk cao      | [Điền] | [Điền]      |
| Privacy impact assessment               | Xử lý dữ liệu cá nhân theo điều kiện chính sách/pháp luật | [Điền] | [Điền]      |
| API/Event contract                      | Có integration                                            | [Điền] | [Điền]      |
| Data model/dictionary                   | Có data ownership/transform mới                           | [Điền] | [Điền]      |
| LLD                                     | Cần chi tiết triển khai                                   | [Điền] | [Điền]      |
| Test/performance/DR plan                | Có NFR tương ứng                                          | [Điền] | [Điền]      |
| Runbook/operational readiness checklist | Dịch vụ đưa vào vận hành                                  | [Điền] | [Điền]      |
| Migration/cutover/rollback plan         | Có chuyển đổi                                             | [Điền] | [Điền]      |

## 12.2 Căn cứ xây dựng template

Template này tổng hợp và điều chỉnh các nguồn sau cho mục đích HLD; không tuyên bố tài liệu HLD tự động đạt chứng nhận hoặc tuân thủ toàn bộ nguồn:

1. **ISO/IEC/IEEE 42010:2022 — Architecture description:** stakeholder, concern, viewpoint, view, model, correspondence và rationale.
2. **The TOGAF Standard, 10th Edition — Fundamental Content/Architecture Content/ADM:** architecture drivers và requirements, business/data/application/technology domains, baseline/target, gap, transition architecture, roadmap và implementation governance.
3. **COBIT 2019 — chủ yếu BAI03 Managed Solutions Identification and Build:** high-level solution design, lựa chọn reuse/build/buy, thiết kế solution components, quality assurance và traceability; tailoring theo governance design factors và risk context thay vì one-size-fits-all.
4. **ISO/IEC/IEEE 15288:2023 — System life cycle processes:** stakeholder needs, system requirements, architecture definition, verification, validation, transition, operation, maintenance và disposal.
5. **ISO/IEC 25010:2023 — Product quality model:** completeness check cho quality requirements, measurable quality objectives và acceptance/evaluation criteria.
6. **NIST SP 800-160 Vol. 1 Rev. 1 — Engineering Trustworthy Secure Systems:** protection needs, security architecture/design, risk-informed engineering, verification và validation xuyên suốt vòng đời.
7. **NIST SP 800-53 Rev. 5 (Update 1) và SP 800-53A Rev. 5:** lựa chọn control theo risk/baseline và thiết kế assessment evidence; các control liên quan trực tiếp thường gồm SDLC (`SA-3`), security/privacy architectures (`PL-8`), developer security/privacy architecture and design (`SA-17`) cùng các control family phù hợp. Không xem một control riêng lẻ là “template HLD”.
8. **NIST SP 800-218 v1.1 — Secure Software Development Framework (SSDF):** secure development environment, provenance, third-party components, artifact integrity và vulnerability response khi giải pháp có software delivery lifecycle.
9. **ISO/IEC 20000-1:2018 — Service management system requirements:** service planning/ownership, transition, delivery, change, incident/problem, capacity, availability, continuity, supplier và continual improvement.
10. **arc42 và C4 Model — tham khảo thực hành, không phải control framework:** context, building block/container, runtime, deployment, cross-cutting concepts, quality scenarios, risks và technical debt.

Phiên bản trên là baseline dùng để thiết kế **template v1.1**. Mỗi dự án vẫn phải ghi phiên bản tiêu chuẩn/chính sách được SCEX chính thức áp dụng tại mục 1.4. Khi nguồn thay đổi, Template Owner phải impact-assess và phát hành phiên bản template mới; không tự động thay edition giữa vòng đời một HLD đã phê duyệt.

## 12.3 Standards-to-template crosswalk

| Nguồn                         | Khái niệm được áp dụng                                                                                   | Mục chính              | Giới hạn/diễn giải                                                                                                                                                                                          |
| ----------------------------- | -------------------------------------------------------------------------------------------------------- | ---------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ISO/IEC/IEEE 42010:2022       | Stakeholder, concern, viewpoint, view, model, correspondence, rationale                                  | 3.1–3.2, 5.1–5.9, 10.1 | HLD chỉ là architecture description phù hợp 42010 khi các phần tử và quan hệ cần thiết thực sự được hoàn thành; dùng template trống không tạo compliance.                                                   |
| TOGAF 10th Edition            | Drivers/requirements, baseline–target–gap, domain views, transition, roadmap, governance                 | 3–5, 9, 11             | Template tạo artifact tương thích; không thay thế việc thực hiện toàn bộ ADM hoặc Architecture Capability.                                                                                                  |
| COBIT 2019/BAI03              | Governance ownership, high-level design, option/sourcing decision, QA, acceptance và change traceability | 1, 2.3, 3.7, 9, 11     | HLD hỗ trợ evidence cho objective; không tự chứng minh capability/performance level của COBIT. Các objective COBIT khác phải được governance owner map theo scope.                                          |
| ISO/IEC/IEEE 15288:2023       | Needs/requirements, architecture definition, V&V, transition, operation, maintenance, disposal           | 3–4, 8–9, 11           | Áp dụng outcome ở mức HLD; project lifecycle plan chịu trách nhiệm task/role chi tiết. NIST SP 800-160 Rev. 1 dựa trên edition 15288 cũ hơn, nên không giả định clause number tương đương với edition 2023. |
| ISO/IEC 25010:2023            | Quality-model completeness, measurable scenarios và evaluation criteria                                  | 6, 11.2                | Không buộc mọi characteristic cùng mức ưu tiên; chọn theo context và ghi rõ N/A/trade-off.                                                                                                                  |
| NIST SP 800-160 Vol. 1 Rev. 1 | Protection needs, trustworthy secure design, evidence-based assurance                                    | 3.3, 6, 7, 10–11       | Là systems security engineering guidance; control selection vẫn theo risk và baseline được phê duyệt.                                                                                                       |
| NIST SP 800-53/53A Rev. 5     | Control design, enforcement point, owner, residual risk và assessment evidence                           | 7, 10.2, 11.2–11.3     | Không áp toàn bộ catalog mặc định; dùng control baseline/tailoring và assessment procedure có thẩm quyền.                                                                                                   |
| NIST SP 800-218 v1.1          | Secure SDLC và software supply-chain integrity                                                           | 5.9, 7.3, 8.3, 9.3     | Có điều kiện khi scope bao gồm build/acquire/integrate software; chi tiết pipeline thuộc LLD/SDLC standard.                                                                                                 |
| ISO/IEC 20000-1:2018          | Service planning, transition, operability, continuity, supplier và improvement                           | 6.4, 8–9, 11.4         | HLD là một đầu vào/evidence; không thay thế hoặc chứng nhận toàn bộ Service Management System.                                                                                                              |
| arc42/C4                      | Cấu trúc view thực dụng và notation dễ giao tiếp                                                         | 5, 10                  | Informative practice; không dùng làm compliance claim.                                                                                                                                                      |

## 12.4 Tài liệu nội bộ liên quan

- [Hướng dẫn chuẩn hóa Sequence Diagram](../../architecture-principles-and-guidelines/guidelines/huong-dan-chuan-hoa-sequence-diagram.md)
- [SCEX Architecture Library cho Draw.io](../../architecture-principles-and-guidelines/guidelines/drawio/scex-architecture-library.xml)
- [NFR Baseline](../../non-functional-requirements-baseline/)
- [Architecture Decision Records](../../architecture-decision-records/)
- [Architecture Governance and ARB Process](../../architecture-governance-and-arb-process/)

## 12.5 Tài liệu nguồn công khai

- [ISO/IEC/IEEE 42010:2022](https://www.iso.org/standard/74393.html)
- [The TOGAF Standard — Digital Edition](https://pubs.opengroup.org/togaf-standard/)
- [COBIT 2019 resources — ISACA](https://www.isaca.org/resources/cobit)
- [ISO/IEC/IEEE 15288:2023](https://www.iso.org/standard/81702.html)
- [ISO/IEC 25010:2023](https://www.iso.org/standard/78176.html)
- [NIST SP 800-160 Vol. 1 Rev. 1](https://csrc.nist.gov/pubs/sp/800/160/v1/r1/final)
- [NIST SP 800-53 Rev. 5, Update 1](https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final)
- [NIST SP 800-53A Rev. 5](https://csrc.nist.gov/pubs/sp/800/53/a/r5/final)
- [NIST SP 800-218 v1.1 — SSDF](https://csrc.nist.gov/pubs/sp/800/218/final)
- [ISO/IEC 20000-1:2018](https://www.iso.org/standard/70636.html)
- [arc42 Template Overview](https://arc42.org/overview/)
- [C4 Model](https://c4model.com/)

---

> **Kết thúc template — xóa các hướng dẫn không còn cần thiết trước khi phát hành bản Approved.**
