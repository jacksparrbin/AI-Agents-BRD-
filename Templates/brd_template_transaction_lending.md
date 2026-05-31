# [TÊN DỰ ÁN / TÊN TÍNH NĂNG]
## TÀI LIỆU YÊU CẦU NGHIỆP VỤ - HÀNH TRÌNH THANH TOÁN DỊCH VỤ, HẠN MỨC & TÍN DỤNG SỐ (TRANSACTION, PAYMENT & LENDING)
**Mã tài liệu:** BRD-[LEN/PAY-XXXX] [Tên hành trình/MVP]  
**Phiên bản:** Ver X.X  
**Ngày cập nhật:** DD/MM/YYYY  
**Trạng thái:** [Draft / Under Review / Released]

---

## 1. LỊCH SỬ THAY ĐỔI TÀI LIỆU
Bảng theo dõi các lần cải tiến luồng giao dịch tài chính trực tuyến qua các phiên bản (MVP).

| Phiên bản | Ngày cập nhật | Người thực hiện | Người phê duyệt | Mô tả chi tiết thay đổi (A/M/D) |
| :--- | :--- | :--- | :--- | :--- |
| Ver 1.0 | DD/MM/YYYY | [Tên PO] | [Tên Approver] | Tạo mới tài liệu cho hành trình [Tên hành trình] - MVP1 |
| Ver 2.0 | DD/MM/YYYY | [Tên PO] | [Tên Approver] | Cập nhật logic tích hợp công cụ Tính toán hạn mức tự động |

---

## 2. BỘ THUẬT NGỮ CHUYÊN NGÀNH GIAO DỊCH & TÍN DỤNG

| STT | Thuật ngữ | Ý nghĩa nghiệp vụ |
| :--- | :--- | :--- |
| 1 | **TKTT** | Tài khoản thanh toán của khách hàng tại ngân hàng. |
| 2 | **LDP** | Landing Page (Trang đích tiếp nhận nhu cầu vay/thanh toán của khách hàng). |
| 3 | **DigiLending** | Hệ thống khởi tạo và quản lý hồ sơ khoản vay trực tuyến. |
| 4 | **DIP** | Digital Integration Platform (Cổng kết nối tích hợp dữ liệu API). |
| 5 | **Criteria Management** | Hệ thống quản lý tập luật và tiêu chí xét duyệt hồ sơ tự động. |
| 6 | **Calculator Engine** | Công cụ tính toán hạn mức phê duyệt dự kiến dựa trên khai báo thu nhập. |

---

## 3. TỔNG QUAN HÀNH TRÌNH GIAO DỊCH & LENDING
*   **Mô tả US**: Là một Khách hàng hiện hữu (ETB) hoặc Khách hàng mới (NTB) đã định danh, tôi muốn thực hiện các giao dịch thanh toán dịch vụ (Điện, Nước, Nạp tiền điện thoại) hoặc đăng ký cấp hạn mức thấu chi/thẻ tín dụng online 100% để có thể tiếp cận tài chính nhanh chóng phục vụ tiêu dùng cá nhân.
*   **Mục đích**: Số hóa toàn diện quy trình cấp tín dụng, tối ưu hóa tốc độ xử lý giao dịch thanh toán hóa đơn, đảm bảo các chốt chặn kiểm soát rủi ro về hạn mức giao dịch trực tuyến.

---

## 4. LUỒNG QUY TRÌNH HÀNH TRÌNH (TO-BE JOURNEY)
*   **Link Miro (Quy trình nghiệp vụ tổng quan)**: [Miro Link]
*   **Link Figma (Bản vẽ UI/UX)**: [Figma Link]

---

## 5. ĐẶC TẢ CHI TIẾT LUỒNG NGHIỆP VỤ & KIỂM TRA ĐIỀU KIỆN (BUSINESS RULES)

Áp dụng pattern **Sub-step + Branching Matrix** (xem cẩm nang `Guides/po_writing_guide_for_ai_agents.md` Section IV). Mỗi bước cha là 1 bảng nhỏ với 7 cột chuẩn.

### 5.1. Bảng Chi Tiết Luồng Đăng Ký Cấp Hạn Mức Tín Dụng (Lending Journey Matrix)

#### Bước 1: Khám phá hạn mức & Tính toán

| Sub-step | PIC | Thao tác / Check | Logic Business Rule | Pass → | Fail → | Mã sự kiện log |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **1.1**<br>Survey Input | Khách hàng | KH bấm `[Khám phá hạn mức]` → trả lời bộ câu hỏi khảo sát thu nhập | Validate dữ liệu khảo sát + Required fields | Đủ thông tin → **Bước 1.2** | Thiếu trường → highlight + cho phép edit | Pass: `EVT_LEND_SURVEY_OK`<br>Fail: `EVT_LEND_SURVEY_INCOMPLETE` |
| **1.2**<br>Calculator Engine | Hệ thống | Gọi Calculator Engine để tính hạn mức thấu chi dự kiến | Áp tập luật Criteria Management + lịch sử tín dụng CIC | Trả về hạn mức dự kiến → hiển thị màn đề xuất → **Bước 2.1** | API Calculator timeout: Popup `[ERR_CALC_001]` (Thử lại / Hotline) | Pass: `EVT_LEND_CALC_SUCCESS`<br>Fail: `EVT_LEND_CALC_API_ERROR` |

#### Bước 2: Xác thực SĐT & Phân nhánh ETB/NTB

| Sub-step | PIC | Thao tác / Check | Logic Business Rule | Pass → | Fail → | Mã sự kiện log |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **2.1**<br>Phone Verify | Khách hàng | KH nhập SĐT → nhận SMS OTP → xác thực | OTP retry ≤ 3 lần, resend ≤ 3 lần/phiên | OTP đúng → **Bước 2.2** | Sai ≥3 lần: Popup `[ERR_OTP_002]` khóa luồng | Pass: `EVT_LEND_OTP_VERIFIED`<br>Fail: `EVT_LEND_OTP_LOCKED` |
| **2.2**<br>eBank Check | Hệ thống | Kiểm tra SĐT đã đăng ký eBank chưa | Query CIF + tài khoản IBMB | Có eBank → yêu cầu KH đăng nhập → **Bước 3.1**.<br>Chưa có → chuyển sang luồng Onboarding | — | Pass: `EVT_LEND_EBANK_FOUND` / `EVT_LEND_EBANK_NONE_GOTO_ONB` |

#### Bước 3: Khai báo hồ sơ vay

| Sub-step | PIC | Thao tác / Check | Logic Business Rule | Pass → | Fail → | Mã sự kiện log |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **3.1**<br>Auto-fill Drop-off | Hệ thống | Tự động điền thông tin từ hồ sơ drop-off cũ (≤15 ngày) hoặc Core | Drop-off Recovery rule | Có data → pre-fill form → **Bước 3.2** | Không có data → form trống → **Bước 3.2** | Pass: `EVT_LEND_AUTOFILL_DROPOFF` / `EVT_LEND_AUTOFILL_NONE` |
| **3.2**<br>Form Input | Khách hàng | KH kiểm tra + khai báo các trường bắt buộc (nghề, thu nhập, mục đích vay) | Validate định dạng + ngưỡng tối thiểu (VD: thu nhập ≥ 5tr/tháng) | Đủ + hợp lệ → **Bước 4.1** | Thiếu/sai → highlight + cho phép edit | Pass: `EVT_LEND_FORM_VALID`<br>Fail: `EVT_LEND_FORM_INVALID` |

#### Bước 4: Ký số & Phê duyệt

| Sub-step | PIC | Thao tác / Check | Logic Business Rule | Pass → | Fail → | Mã sự kiện log |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **4.1**<br>E-sign Contract | Khách hàng | KH đọc đơn đề nghị cấp hạn mức + ký số bằng OTP/Smart OTP | Tuân thủ Luật GDDT 20/2023 | Ký thành công → **Bước 4.2** | Sai OTP ≥5 lần: Popup `[ERR_SIGN_003]` khóa 24h | Pass: `EVT_LEND_ESIGN_OK`<br>Fail: `EVT_LEND_ESIGN_LOCKED_24H` |
| **4.2**<br>BPM Approval | Hệ thống | Gửi hồ sơ sang BPM Ops xét duyệt tự động | Auto-rule engine xét duyệt | Xử lý ≤ 5 giây + APPROVED → **Bước 4.3** | PENDING (>5s): điều hướng màn theo dõi hồ sơ.<br>REJECTED: Popup `[ERR_LEND_004]` hướng dẫn liên hệ Hotline | Pass: `EVT_LEND_BPM_APPROVED`<br>Pending: `EVT_LEND_BPM_PENDING`<br>Fail: `EVT_LEND_BPM_REJECTED` |
| **4.3**<br>Show Result | Khách hàng | KH xem màn tiếp nhận hạn mức + đề xuất phát hành thẻ | Cross-sell flow | Kết thúc luồng cấp hạn mức → chuyển luồng phát hành thẻ | — | Pass: `EVT_LEND_FLOW_COMPLETED` |

---

### 5.2. Chốt Chặn Kiểm Tra Logic Cho Luồng Thanh Toán Dịch Vụ (Utility Payments Validation Rules)

> **Cross-cutting Rules:** Áp dụng cho mọi giao dịch thanh toán hóa đơn / nạp tiền dịch vụ. Một số rule được kiểm tra ngầm tại các sub-step trong matrix Payment riêng (không tích hợp vào matrix Lending ở 5.1).

Khi khách hàng thực hiện khởi tạo yêu cầu thanh toán hóa đơn hoặc nạp tiền dịch vụ (Billing Topup), hệ thống bắt buộc phải kiểm tra qua các điều kiện sau:

1.  **Gói IBMB Khách hàng đang sử dụng**: Kiểm tra gói dịch vụ có bị chặn tính năng thanh toán trực tuyến không.
2.  **Tài khoản thanh toán (TKTT) nguồn**: Trạng thái TKTT phải là `Active`; số dư khả dụng ≥ Số tiền GD + Phí GD.
3.  **Tính hợp lệ của Mã hóa đơn / Mã khách hàng**: Truy vấn đối tác lấy thông tin nợ cước; nếu hóa đơn đã thanh toán → báo lỗi.
4.  **Hạn mức xác thực sinh trắc học theo QĐ 2345**: Số tiền GD > 10,000,000 VND/lần hoặc tổng hạn mức GD/ngày > 20,000,000 VND → bắt buộc Face Authen trước khi nhập OTP.

---

## 6. ĐẶC TẢ TÍCH HỢP DỮ LIỆU GIỮA CÁC PHÂN HỆ HỆ THỐNG (DATA MAPPING DICTIONARY)

PO cần đặc tả rõ ràng luồng truyền nhận thông tin chi tiết giữa Mobile App (DC), Cổng Tích hợp (DIP) và hệ thống xử lý hồ sơ tín dụng (DigiLending).

### Bảng 6.1: Thông tin Yêu cầu Phê duyệt Hồ sơ gửi từ DC sang DigiLending / DIP
| Tên Trường (Tiếng Việt) | Mã trường (Field Name) | Kiểu dữ liệu | Thuộc tính (M/O) | Logic & Mô tả |
| :--- | :--- | :--- | :---: | :--- |
| Số điện thoại đăng ký | `registered_phone` | String | M | Số điện thoại chính chủ đã được xác thực qua OTP. |
| Hạn mức mong muốn | `requested_amount` | Decimal | M | Hạn mức tín dụng mong muốn cấp do KH khai báo trên UI. |
| Nghề nghiệp khách hàng | `occupation_code` | String | M | Mã nghề nghiệp chọn từ danh mục dropdown trên app. |
| Thu nhập trung bình tháng | `monthly_income` | Decimal | M | Số tiền thu nhập hàng tháng khai báo dùng làm căn cứ tính điểm tín dụng. |

### Bảng 6.2: Thông tin Trạng thái phê duyệt trả từ DigiLending về Mobile App
| Tên Trường (Tiếng Việt) | Mã trường (Field Name) | Kiểu dữ liệu | Ý nghĩa mã trả về | Điều hướng UI mong muốn |
| :--- | :--- | :--- | :---: | :--- |
| Trạng thái hồ sơ | `application_status` | String | `APPROVED` (Đã phê duyệt) | Hiển thị màn hình thành công -> Nút bấm điều hướng sang Ký hợp đồng phát hành thẻ. |
| | | | `PENDING` (Chờ thẩm định) | Hiển thị màn hình hồ sơ đang xử lý -> Điều hướng về danh sách theo dõi hồ sơ. |
| | | | `REJECTED` (Bị từ chối) | Hiển thị thông báo hồ sơ bị từ chối phê duyệt trực tuyến tự động -> Đóng luồng. |

---

## 7. CẤU TRÚC THÔNG BÁO HÀNH TRÌNH ĐA KÊNH (MULTI-CHANNEL NOTIFICATION STRATEGY)
Để đảm bảo trải nghiệm khách hàng liên tục trong suốt quá trình đăng ký khoản vay hoặc xử lý giao dịch thành công.

### 7.1. Thông báo đẩy trong ứng dụng (In-app Push Notification - Noti Center)
*   **Kích hoạt khi**: Trạng thái hồ sơ cấp hạn mức được phê duyệt thành công trên hệ thống DigiLending.
*   **Tiêu đề**: Hồ sơ cấp hạn mức thẻ tín dụng đã được phê duyệt!
*   **Nội dung**: Chúc mừng Quý khách! MSB đã tiếp nhận và phê duyệt hạn mức thẻ tín dụng của Quý khách là `{credit_limit}` VND. Vui lòng bấm vào đây để xác nhận điều khoản và phát hành thẻ trực tuyến ngay lập tức.

### 7.2. Thông báo qua tin nhắn viễn thông SMS
*   **Kích hoạt khi**: Khởi tạo gửi mã xác thực giao dịch tài chính hoặc đăng ký vay.
*   **Nội dung mẫu**: `MSB Digibank: Ma so OTP cua Quy khach la {otp_code} (co hieu luc trong 3 phut) de xac nhan giao dich {transaction_type}. Tuyet doi khong chia se OTP cho bat ky ai.`
