|                                                                                                                                                                                                                                                                                   |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **BRD - YÊU CẦU KẾT NỐI CORE GIAO DỊCH TÀI SẢN MÃ HÓA (CCTS) VỚI PHẦN MỀM KẾ TOÁN BRAVO**<br>**Đơn vị yêu cầu:** Công ty SCEX<br>**Hệ thống nguồn:** CCTS (Remi)<br>**Hệ thống đích:** Bravo<br>**Người soạn:** Phạm Thị Huyền Trâm<br>**Phiên bản:** 2.0<br>**Ngày:** 03/06/2026 |

**MỤC LỤC**

[LỊCH SỬ PHIÊN BẢN 3](#lịch-sử-phiên-bản)

[THUẬT NGỮ VÀ TỪ VIẾT TẮT 3](#thuật-ngữ-và-từ-viết-tắt)

[BỐI CẢNH 4](#bối-cảnh)

[I. KẾT NỐI CORE GIAO DỊCH TÀI SẢN MÃ HÓA 4](#i-kết-nối-core-giao-dịch-tài-sản-mã-hóa)

[Nguyên tắc chung 4](#nguyên-tắc-chung)

[1\. 100 - CAM: Danh mục tài sản mã hóa (Crypto Asset Master) 4](#1-100---cam-danh-mục-tài-sản-mã-hóa-crypto-asset-master)

[2\. 101 - INM: Danh mục nhà đầu tư (Investor Master) 4](#2-101---inm-danh-mục-nhà-đầu-tư-investor-master)

[3\. 102 - APR: Bảng giá (Asset Pricing Report) 5](#3-102---apr-bảng-giá-asset-pricing-report)

[4\. 200 - CBR: Dữ liệu giao dịch MUA tài sản mã hóa (Crypto Buy Report) 5](#4-200---cbr-dữ-liệu-giao-dịch-mua-tài-sản-mã-hóa-crypto-buy-report)

[5\. 201 - CSR: Dữ liệu giao dịch BÁN tài sản mã hóa (Crypto Sell Report) 6](#5-201---csr-dữ-liệu-giao-dịch-bán-tài-sản-mã-hóa-crypto-sell-report)

[6\. 300 - FDR: Dữ liệu giao dịch NẠP TIỀN PHÁP ĐỊNH vào TK NĐT (Fiat Deposit Report) 6](#6-300---fdr-dữ-liệu-giao-dịch-nạp-tiền-pháp-định-vào-tk-nđt-fiat-deposit-report)

[7\. 301 - FWR: Dữ liệu giao dịch RÚT TIỀN PHÁP ĐỊNH ra từ TK NĐT (Fiat Withdrawal Report) 6](#7-301---fwr-dữ-liệu-giao-dịch-rút-tiền-pháp-định-ra-từ-tk-nđt-fiat-withdrawal-report)

[8\. 400 - CDR: Dữ liệu giao dịch NẠP TIỀN MÃ HÓA vào TK NĐT (Crypto Deposit Report) 7](#8-400---cdr-dữ-liệu-giao-dịch-nạp-tiền-mã-hóa-vào-tk-nđt-crypto-deposit-report)

[9\. 401 - CWR: Dữ liệu giao dịch RÚT TIỀN MÃ HÓA ra từ TK NĐT (Crypto Withdrawal Report) 7](#9-401---cwr-dữ-liệu-giao-dịch-rút-tiền-mã-hóa-ra-từ-tk-nđt-crypto-withdrawal-report)

[10\. 500 - FCR: Thu phí (Fee Charge Report) 8](#10-500---fcr-thu-phí-fee-charge-report)

[10.1. 500a - Phí lưu ký 8](#101-500a---phí-lưu-ký)

[10.2. 500b - Phí hoàn 8](#102-500b---phí-hoàn)

[10.3. 500c - Phí mạng 9](#103-500c---phí-mạng)

[11\. 600 - ATR: Giao dịch chuyển tài sản nội bộ (Asset Transfer Report) 9](#11-600---atr-giao-dịch-chuyển-tài-sản-nội-bộ-asset-transfer-report)

[12\. 601 - FTR: Giao dịch chuyển tiền pháp định nội bộ (Fiat Transfer Report) 10](#12-601---ftr-giao-dịch-chuyển-tiền-pháp-định-nội-bộ-fiat-transfer-report)

[13\. 700 - RCR: Báo cáo đối soát dữ liệu (Reconciliation Report) 10](#13-700---rcr-báo-cáo-đối-soát-dữ-liệu-reconciliation-report)

[PHỤ LỤC 01 (PL01): DANH MỤC MÃ GIAO DỊCH (TRANSACTION CODE) 11](#phụ-lục-01-pl01-danh-mục-mã-giao-dịch-transaction-code)

# LỊCH SỬ PHIÊN BẢN

| **Phiên bản** | **Ngày**   | **Nội dung thay đổi**                                                                                                                                                                   |
| ------------- | ---------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| v1.0          | —          | Phiên bản đầu tiên. Đồng bộ 1 chiều CCTS → Bravo, KHÔNG gộp giao dịch (truyền chi tiết từng GD).                                                                                        |
| v2.0          | 03/06/2026 | Thay đổi phương án: Gom giao dịch tại Core trước khi truyền sang Bravo (theo NĐT + Ngày + Loại doanh thu/phí). Bổ sung mã giao dịch (Transaction Code). Tách hạch toán Nợ/Có sang PL01. |

# THUẬT NGỮ VÀ TỪ VIẾT TẮT

| **Viết tắt** | **Đầy đủ**                 | **Giải thích**                                              |
| ------------ | -------------------------- | ----------------------------------------------------------- |
| CCTS         | Core Crypto Trading System | Hệ thống Core giao dịch tài sản mã hóa (tên hệ thống: Remi) |
| SCEX         | SCEX Company               | Công ty vận hành sàn giao dịch tài sản mã hóa               |
| NĐT          | Nhà đầu tư                 | Khách hàng giao dịch trên sàn                               |
| GD           | Giao dịch                  |                                                             |
| GTTT         | Giấy tờ tùy thân           |                                                             |
| MSTDN        | Mã số thuế doanh nghiệp    |                                                             |
| CCCD         | Căn cước công dân          |                                                             |
| TNCN         | Thu nhập cá nhân           | Thuế thu nhập cá nhân                                       |
| VAT          | Value Added Tax            | Thuế giá trị gia tăng                                       |
| NH           | Ngân hàng                  |                                                             |
| TK           | Tài khoản                  |                                                             |
| FA           | Financial Accountant       | Kế toán tài chính (bên SCEX)                                |
| VND          | Việt Nam Đồng              | Đơn vị tiền tệ pháp định                                    |
| HĐTG         | Hợp đồng tiền gửi          |                                                             |
| CCTG         | Chứng chỉ tiền gửi         |                                                             |
| PĐ           | Pháp định                  | Tiền pháp định (fiat)                                       |
| GĐ           | Giai đoạn                  | Giai đoạn triển khai                                        |

# BỐI CẢNH

Công ty SCEX đang triển khai hệ thống Core giao dịch tài sản mã hóa (CCTS - tên hệ thống: Remi) và có nhu cầu kết nối, đồng bộ dữ liệu từ CCTS sang phần mềm kế toán Bravo để phục vụ công tác hạch toán, quản lý tài chính.

Dưới đây là chi tiết yêu cầu kết nối, kính đề nghị Bravo xem xét và phản hồi phương án triển khai.

# I. KẾT NỐI CORE GIAO DỊCH TÀI SẢN MÃ HÓA

## Nguyên tắc chung

| **Hạng mục**      | **Nội dung**                                                                                                                          |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| Quy trình đồng bộ | 1 chiều từ CCTS sang Bravo                                                                                                            |
| Phương thức       | CCTS gọi API POST của Bravo để đồng bộ dữ liệu                                                                                        |
| Nguyên tắc gom GD | Gom dữ liệu giao dịch tại Core trước khi truyền sang Bravo để phát hành hóa đơn theo nhà đầu tư, theo ngày và theo loại doanh thu/phí |
| Tiêu chí gom      | Mã NĐT + Ngày GD + Transtype (một số loại GD gom thêm theo Mã tài sản)                                                                |

## 1. 100 - CAM: Danh mục tài sản mã hóa (Crypto Asset Master)

| **Hạng mục**      | **Nội dung**                                                                                 |
| ----------------- | -------------------------------------------------------------------------------------------- |
| Mã GD             | 100                                                                                          |
| Code              | CAM                                                                                          |
| Quy trình         | Đồng bộ 1 chiều từ CCTS sang Bravo                                                           |
| Số lượng dự kiến  | < 1.000 mã                                                                                   |
| Tần suất tích hợp | Khi phát sinh thêm mới                                                                       |
| Key               | Mã tài sản                                                                                   |
| Tiêu chí gom      | Không áp dụng (danh mục)                                                                     |
| Thông tin đồng bộ | • Mã tài sản: Key định danh<br>• Tên tài sản<br>• Loại tài sản: Tiền mã hóa / Tiền pháp định |

## 2. 101 - INM: Danh mục nhà đầu tư (Investor Master)

| **Hạng mục**      | **Nội dung**                                                                                                                                                                                                                                                                                                                                                                       |
| ----------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Mã GD             | 101                                                                                                                                                                                                                                                                                                                                                                                |
| Code              | INM                                                                                                                                                                                                                                                                                                                                                                                |
| Quy trình         | Đồng bộ 1 chiều từ CCTS sang Bravo                                                                                                                                                                                                                                                                                                                                                 |
| Số lượng dự kiến  | < 1.000                                                                                                                                                                                                                                                                                                                                                                            |
| Tần suất tích hợp | Khi phát sinh thêm mới (realtime)                                                                                                                                                                                                                                                                                                                                                  |
| Key               | Mã định danh của NĐT                                                                                                                                                                                                                                                                                                                                                               |
| Tiêu chí gom      | Không áp dụng (danh mục)                                                                                                                                                                                                                                                                                                                                                           |
| Thông tin đồng bộ | • Mã định danh: Key định danh<br>• Tên nhà đầu tư<br>• Giấy tờ tùy thân (GTTT)<br>• Loại GTTT<br>• Địa chỉ: Cá nhân: thường trú; Tổ chức: địa chỉ công ty<br>• Mã số thuế: Tổ chức: MSTDN; Cá nhân: Số CCCD<br>• Loại đối tượng: Cá nhân / Tổ chức<br>• Số điện thoại<br>• Email<br>• Quốc tịch<br>• MG/ Chi nhánh /Trung tâm kinh doanh (KH thuộc MG nào? Chi nhánh/Trung tâm KD) |

## 3. 102 - APR: Bảng giá (Asset Pricing Report)

| **Hạng mục**      | **Nội dung**                                                                                                                  |
| ----------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| Mã GD             | 102                                                                                                                           |
| Code              | APR                                                                                                                           |
| Quy trình         | Đồng bộ 1 chiều từ CCTS sang Bravo                                                                                            |
| Số lượng dự kiến  | < 1.000                                                                                                                       |
| Tần suất tích hợp | Cuối ngày giao dịch (giá đóng nến 23h hàng ngày, khung 1h)                                                                    |
| Lưu ý             | Kỳ đồng bộ cần phù hợp thời gian cần giá để xử lý các nghiệp vụ liên quan. Cuối kỳ đánh giá lại, không cần trích lập tự động. |
| Tiêu chí gom      | Không áp dụng (bảng giá)                                                                                                      |
| Thông tin đồng bộ | • Mã tài sản<br>• Giá đóng nến lúc 23h khung 1h: Giá cuối cùng của ngày, 1 ngày có 24 nến                                     |

## 4. 200 - CBR: Dữ liệu giao dịch MUA tài sản mã hóa (Crypto Buy Report)

| **Hạng mục**                           | **Nội dung**                                                                                                                                                                                                                                                                                                |
| -------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Mã GD                                  | 200                                                                                                                                                                                                                                                                                                         |
| Code                                   | CBR                                                                                                                                                                                                                                                                                                         |
| Quy trình                              | Đồng bộ 1 chiều từ CCTS sang Bravo                                                                                                                                                                                                                                                                          |
| Số lượng GD trung bình/ngày (tại Core) | 4.000.000 GD/ngày                                                                                                                                                                                                                                                                                           |
| Tần suất tích hợp                      | Ngay khi kết thúc thời gian giao dịch, theo ngày                                                                                                                                                                                                                                                            |
| Tiêu chí gom                           | Mã NĐT + Ngày GD + Mã tài sản                                                                                                                                                                                                                                                                               |
| Thông tin đồng bộ (sau gom)            | • Mã tài sản: Key gom<br>• Mã NĐT: Key gom<br>• Ngày GD: Key gom<br>• Tổng giá trị mua tài sản: SUM(giá trị mua)<br>• Tổng khối lượng mua: SUM(khối lượng mua)<br>• Tiểu khoản mua<br>• Tổng phí GD: SUM(phí GD)<br>• Tổng thuế VAT của phí mua: SUM(thuế VAT)<br>• Số lượng GD gốc: COUNT<br>• Nội dung GD |

## 5. 201 - CSR: Dữ liệu giao dịch BÁN tài sản mã hóa (Crypto Sell Report)

| **Hạng mục**                           | **Nội dung**                                                                                                                                                                                                                                                                                                                                                       |
| -------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Mã GD                                  | 201                                                                                                                                                                                                                                                                                                                                                                |
| Code                                   | CSR                                                                                                                                                                                                                                                                                                                                                                |
| Quy trình                              | Đồng bộ 1 chiều từ CCTS sang Bravo                                                                                                                                                                                                                                                                                                                                 |
| Số lượng GD trung bình/ngày (tại Core) | 4.000.000 GD/ngày                                                                                                                                                                                                                                                                                                                                                  |
| Tần suất tích hợp                      | Khi kết thúc thời gian giao dịch, theo ngày                                                                                                                                                                                                                                                                                                                        |
| Tiêu chí gom                           | Mã NĐT + Ngày GD + Mã tài sản                                                                                                                                                                                                                                                                                                                                      |
| Thông tin đồng bộ (sau gom)            | • Mã tài sản: Key gom<br>• Mã NĐT: Key gom<br>• Ngày GD: Key gom<br>• Tổng giá trị bán tài sản: SUM (giá trị bán)<br>• Tổng khối lượng bán: SUM (khối lượng bán)<br>• Tiểu khoản bán<br>• Tổng phí GD: SUM (phí GD)<br>• Tổng thuế VAT phí GD: SUM (thuế VAT)<br>• Tổng thuế TNCN: SUM (thuế TNCN) — chỉ có ở cá nhân<br>• Số lượng GD gốc: COUNT<br>• Nội dung GD |

## 6. 300 - FDR: Dữ liệu giao dịch NẠP TIỀN PHÁP ĐỊNH vào TK NĐT (Fiat Deposit Report)

| **Hạng mục**                           | **Nội dung**                                                                                                                                                                                                                                                                                      |
| -------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Mã GD                                  | 300                                                                                                                                                                                                                                                                                               |
| Code                                   | FDR                                                                                                                                                                                                                                                                                               |
| Quy trình                              | Đồng bộ 1 chiều từ CCTS sang Bravo                                                                                                                                                                                                                                                                |
| Số lượng GD trung bình/ngày (tại Core) | < 10.000 GD/ngày                                                                                                                                                                                                                                                                                  |
| Tần suất tích hợp                      | Cuối ngày giao dịch, theo ngày                                                                                                                                                                                                                                                                    |
| Tiêu chí gom                           | Mã NĐT + Ngày GD                                                                                                                                                                                                                                                                                  |
| Thông tin đồng bộ (sau gom)            | • Ngày giao dịch: Key gom<br>• Mã NĐT: Key gom<br>• Tiểu khoản nhận của KH<br>• Tổng giá trị nộp tiền: SUM (giá trị nộp)<br>• Số tài khoản NH chuyên thu<br>• Ngân hàng chuyên thu (của SCEX)<br>• Loại tiền: Mặc định VND<br>• Tỷ giá: Mặc định = 1<br>• Số lượng GD gốc: COUNT<br>• Nội dung GD |

## 7. 301 - FWR: Dữ liệu giao dịch RÚT TIỀN PHÁP ĐỊNH ra từ TK NĐT (Fiat Withdrawal Report)

| **Hạng mục**                           | **Nội dung**                                                                                                                                                                                                                                                                                                                                                                          |
| -------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Mã GD                                  | 301                                                                                                                                                                                                                                                                                                                                                                                   |
| Code                                   | FWR                                                                                                                                                                                                                                                                                                                                                                                   |
| Quy trình                              | Đồng bộ 1 chiều từ CCTS sang Bravo                                                                                                                                                                                                                                                                                                                                                    |
| Số lượng GD trung bình/ngày (tại Core) | < 10.000 GD/ngày                                                                                                                                                                                                                                                                                                                                                                      |
| Tần suất tích hợp                      | Cuối ngày giao dịch                                                                                                                                                                                                                                                                                                                                                                   |
| Tiêu chí gom                           | Mã NĐT + Ngày GD                                                                                                                                                                                                                                                                                                                                                                      |
| Thông tin đồng bộ (sau gom)            | • Ngày giao dịch: Key gom<br>• Mã NĐT: Key gom<br>• Tiểu khoản rút của KH<br>• Tổng giá trị rút tiền: SUM (giá trị rút)<br>• Số tài khoản NH chuyên chi<br>• Ngân hàng chuyên chi (của SCEX)<br>• Loại tiền: Mặc định VND<br>• Tỷ giá: Mặc định = 1<br>• Tổng phí rút tiền: SUM (phí rút)<br>• Tổng thuế VAT của phí rút: SUM (thuế VAT)<br>• Số lượng GD gốc: COUNT<br>• Nội dung GD |

## 8. 400 - CDR: Dữ liệu giao dịch NẠP TIỀN MÃ HÓA vào TK NĐT (Crypto Deposit Report)

| **Hạng mục**                           | **Nội dung**                                                                                                                                                                                                                                                                              |
| -------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Mã GD                                  | 400                                                                                                                                                                                                                                                                                       |
| Code                                   | CDR                                                                                                                                                                                                                                                                                       |
| Quy trình                              | Đồng bộ 1 chiều từ CCTS sang Bravo                                                                                                                                                                                                                                                        |
| Số lượng GD trung bình/ngày (tại Core) | < 10.000 GD/ngày                                                                                                                                                                                                                                                                          |
| Tần suất tích hợp                      | Cuối ngày giao dịch, theo ngày                                                                                                                                                                                                                                                            |
| Tiêu chí gom                           | Mã NĐT + Ngày GD + Mã tài sản (loại tiền mã hóa)                                                                                                                                                                                                                                          |
| Thông tin đồng bộ (sau gom)            | • Ngày giao dịch: Key gom<br>• Mã NĐT: Key gom<br>• Loại tiền (tiền mã hóa): Key gom<br>• Tiểu khoản nhận<br>• Tổng số lượng: SUM (số lượng)<br>• Tổng giá trị quy đổi ra VND: SUM (giá trị VND)<br>• Ví nhận (blockchain → kho nào ở Bravo)<br>• Số lượng GD gốc: COUNT<br>• Nội dung GD |

## 9. 401 - CWR: Dữ liệu giao dịch RÚT TIỀN MÃ HÓA ra từ TK NĐT (Crypto Withdrawal Report)

| **Hạng mục**                           | **Nội dung**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| -------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Mã GD                                  | 401                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| Code                                   | CWR                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| Quy trình                              | Đồng bộ 1 chiều từ CCTS sang Bravo                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| Số lượng GD trung bình/ngày (tại Core) | < 10.000 GD/ngày                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| Tần suất tích hợp                      | Cuối ngày giao dịch, theo ngày                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| Tiêu chí gom                           | Mã NĐT + Ngày GD + Mã tài sản (loại tiền mã hóa)                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| Thông tin đồng bộ (sau gom)            | • Ngày giao dịch: Key gom<br>• Mã NĐT: Key gom<br>• Loại tiền (tiền mã hóa): Key gom<br>• Tiểu khoản rút<br>• Tổng số lượng: SUM (số lượng)<br>• Tổng giá trị quy đổi ra VND: SUM (giá trị VND)<br>• Ví rút (blockchain → kho nào ở Bravo)<br>• Tổng phí rút: SUM (phí rút)<br>• Tổng thuế VAT của phí rút (VND): SUM (thuế VAT)<br>• Tổng phí mạng: SUM (phí mạng)<br>• Đồng tiền trả phí mạng<br>• Tổng giá trị quy đổi của phí mạng ra VND: SUM (giá trị VND phí mạng)<br>• Số lượng GD gốc: COUNT<br>• Nội dung GD |

## 10. 500 - FCR: Thu phí (Fee Charge Report)

Các loại phí thu tại Core: Phí lưu ký, Phí hoàn, Phí mạng. Cần có transtype riêng cho từng loại phí.

### 10.1. 500a - Phí lưu ký

| **Hạng mục**                | **Nội dung**                                                                                                                                                                               |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Mã GD                       | 500                                                                                                                                                                                        |
| Code                        | FCR                                                                                                                                                                                        |
| Phân loại                   | Phí lưu ký                                                                                                                                                                                 |
| Tiêu chí gom                | Mã NĐT + Ngày GD                                                                                                                                                                           |
| Thông tin đồng bộ (sau gom) | • Ngày GD: Key gom<br>• Mã KH: Key gom<br>• Tiểu khoản<br>• Tổng phí lưu ký: SUM(phí lưu ký)<br>• Tổng thuế VAT của phí lưu ký: SUM(thuế VAT)<br>• Số lượng GD gốc: COUNT<br>• Nội dung GD |

### 10.2. 500b - Phí hoàn

| **Hạng mục**                | **Nội dung**                                                                                                                                                                                    |
| --------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Mã GD                       | 500                                                                                                                                                                                             |
| Code                        | FCR                                                                                                                                                                                             |
| Phân loại                   | Phí hoàn                                                                                                                                                                                        |
| Tiêu chí gom                | Mã NĐT + Ngày GD                                                                                                                                                                                |
| Thông tin đồng bộ (sau gom) | • Ngày GD: Key gom<br>• Mã KH: Key gom<br>• Tiểu khoản<br>• Tổng phí hoàn: SUM (phí hoàn)<br>• Tổng thuế VAT của phí hoàn (nếu có): SUM (thuế VAT)<br>• Số lượng GD gốc: COUNT<br>• Nội dung GD |

### 10.3. 500c - Phí mạng

| **Hạng mục**                | **Nội dung**                                                                                                                                                                                                                                                                                                                    |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Mã GD                       | 500                                                                                                                                                                                                                                                                                                                             |
| Code                        | FCR                                                                                                                                                                                                                                                                                                                             |
| Phân loại                   | Phí mạng                                                                                                                                                                                                                                                                                                                        |
| Tiêu chí gom                | Ví chịu phí + Ngày GD + Đồng tiền trả phí                                                                                                                                                                                                                                                                                       |
| Lưu ý                       | Phí mạng chi từ TK tự doanh của công ty, không gom theo NĐT                                                                                                                                                                                                                                                                     |
| Thông tin đồng bộ (sau gom) | • Ngày GD: Key gom<br>• Ví chịu phí: Key gom<br>• Đồng tiền trả phí (VD: BTC, ETH, NBN…): Key gom<br>• Tổng số lượng: SUM (số lượng)<br>• Đối tượng chi: TK tự doanh của công ty<br>• Tỷ giá quy đổi: Giá tại thời điểm cuối ngày<br>• Tổng giá trị VND quy đổi: SUM (giá trị VND)<br>• Số lượng GD gốc: COUNT<br>• Nội dung GD |

## 11. 600 - ATR: Giao dịch chuyển tài sản nội bộ (Asset Transfer Report)

| **Hạng mục**                           | **Nội dung**                                                                                                                                                                                                                                                                                                                                                                                                    |
| -------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Mã GD                                  | 600                                                                                                                                                                                                                                                                                                                                                                                                             |
| Code                                   | ATR                                                                                                                                                                                                                                                                                                                                                                                                             |
| Quy trình                              | Đồng bộ 1 chiều từ CCTS sang Bravo                                                                                                                                                                                                                                                                                                                                                                              |
| Số lượng GD trung bình/ngày (tại Core) | < 10.000 GD/ngày                                                                                                                                                                                                                                                                                                                                                                                                |
| Tần suất tích hợp                      | Cuối ngày giao dịch, theo ngày                                                                                                                                                                                                                                                                                                                                                                                  |
| Tiêu chí gom                           | Mã KH gửi + Mã KH nhận + Ngày GD + Mã tài sản                                                                                                                                                                                                                                                                                                                                                                   |
| Thông tin đồng bộ (sau gom)            | • Ngày giao dịch: Key gom<br>• Mã KH gửi: Key gom<br>• Tiểu khoản KH gửi<br>• Mã KH nhận: Key gom<br>• Tiểu khoản KH nhận<br>• Tài sản chuyển (Mã tài sản): Key gom<br>• Tổng số lượng chuyển: SUM (số lượng)<br>• Tổng giá trị tài sản quy đổi ra VND: SUM (giá trị VND)<br>• Tổng phí chuyển: SUM (phí chuyển)<br>• Tổng thuế VAT của phí chuyển: SUM (thuế VAT)<br>• Số lượng GD gốc: COUNT<br>• Nội dung GD |

## 12. 601 - FTR: Giao dịch chuyển tiền pháp định nội bộ (Fiat Transfer Report)

| **Hạng mục**                           | **Nội dung**                                                                                                                                                                                                                                                                                            |
| -------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Mã GD                                  | 601                                                                                                                                                                                                                                                                                                     |
| Code                                   | FTR                                                                                                                                                                                                                                                                                                     |
| Quy trình                              | Đồng bộ 1 chiều từ CCTS sang Bravo                                                                                                                                                                                                                                                                      |
| Số lượng GD trung bình/ngày (tại Core) | < 10.000 GD/ngày                                                                                                                                                                                                                                                                                        |
| Tần suất tích hợp                      | Cuối ngày giao dịch, theo ngày                                                                                                                                                                                                                                                                          |
| Tiêu chí gom                           | Mã KH gửi + Mã KH nhận + Ngày GD                                                                                                                                                                                                                                                                        |
| Thông tin đồng bộ (sau gom)            | • Ngày giao dịch: Key gom<br>• Mã KH gửi: Key gom<br>• Tiểu khoản KH gửi<br>• Mã KH nhận: Key gom<br>• Tiểu khoản KH nhận<br>• Tổng số tiền chuyển: SUM (số tiền)<br>• Tổng phí chuyển: SUM (phí chuyển)<br>• Tổng thuế VAT của phí chuyển: SUM (thuế VAT)<br>• Số lượng GD gốc: COUNT<br>• Nội dung GD |

## 13. 700 - RCR: Báo cáo đối soát dữ liệu (Reconciliation Report)

Tiêu chí đối soát giữa Bravo và CCTS (Remi):

| **STT** | **Tiêu chí**                                                                                       |
| ------- | -------------------------------------------------------------------------------------------------- |
| 1       | Tổng phát sinh tiền nộp/rút                                                                        |
| 2       | Tổng giá trị mua/bán tài sản trong ngày giao dịch                                                  |
| 3       | Tổng doanh thu, thuế ghi nhận tương ứng với phí giao dịch của NĐT                                  |
| 4       | Tổng số dư tiền cuối ngày của NĐT (tiền pháp định)                                                 |
| 5       | Số dư tiền mã hóa giữa 2 phần mềm (Remi xây dựng thêm công cụ đối soát giữa Remi và Ví Blockchain) |

Yêu cầu báo cáo đối soát trên Bravo:

| **Hạng mục**      | **Nội dung**                                                                                                                                                                                             |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Thông tin đồng bộ | Ngày giao dịch<br>Mã NĐT<br>Transtype (Code)<br>Giá trị tại Core (sau gom)<br>Giá trị đã hạch toán lên Bravo<br>Giá trị chênh lệch<br>Số lượng GD gốc tại Core: Để đối chiếu với COUNT trong bản ghi gom |

# PHỤ LỤC 01 (PL01): DANH MỤC MÃ GIAO DỊCH (TRANSACTION CODE)

| **Mã giao dịch** | **Code** | **Full text**            | **Hạng mục**                                             |
| ---------------- | -------- | ------------------------ | -------------------------------------------------------- |
| 100              | CAM      | Crypto Asset Master      | Danh mục tài sản mã hóa                                  |
| 101              | INM      | Investor Master          | Danh mục nhà đầu tư                                      |
| 102              | APR      | Asset Pricing Report     | Bảng giá                                                 |
| 200              | CBR      | Crypto Buy Report        | Dữ liệu giao dịch mua tài sản mã hóa                     |
| 201              | CSR      | Crypto Sell Report       | Dữ liệu giao dịch bán tài sản mã hóa                     |
| 300              | FDR      | Fiat Deposit Report      | Dữ liệu giao dịch nạp tiền pháp định vào tài khoản NĐT   |
| 301              | FWR      | Fiat Withdrawal Report   | Dữ liệu giao dịch rút tiền pháp định ra từ tài khoản NĐT |
| 400              | CDR      | Crypto Deposit Report    | Dữ liệu giao dịch nạp tiền mã hóa vào tài khoản NĐT      |
| 401              | CWR      | Crypto Withdrawal Report | Dữ liệu giao dịch rút tiền mã hóa ra từ tài khoản NĐT    |
| 500              | FCR      | Fee Charge Report        | Thu phí                                                  |
| 600              | ATR      | Asset Transfer Report    | GD chuyển tài sản nội bộ                                 |
| 601              | FTR      | Fiat Transfer Report     | GD chuyển tiền pháp định nội bộ                          |
| 700              | RCR      | Reconciliation Report    | Báo cáo đối soát dữ liệu                                 |

**Chú thích nhóm mã:**

- 1xx = Master/Danh mục
- 2xx = Mua bán
- 3xx = Fiat nạp/rút
- 4xx = Crypto nạp/rút
- 5xx = Phí
- 6xx = Chuyển nội bộ
- 7xx = Đối soát
