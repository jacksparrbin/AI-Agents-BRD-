# [TÊN DỰ ÁN / TÊN HÀNH TRÌNH]
## TÀI LIỆU YÊU CẦU NGHIỆP VỤ (BUSINESS REQUIREMENT DOCUMENT - BRD)
**Mã tài liệu:** BRD-[J-XXXX] [Tên tính năng]  
**Phiên bản:** Ver X.X  
**Ngày cập nhật:** DD/MM/YYYY  
**Trạng thái:** [Draft / Under Review / Released]

---

## 1. LỊCH SỬ THAY ĐỔI TÀI LIỆU
Bảng ghi nhận toàn bộ quá trình cập nhật, chỉnh sửa nội dung tài liệu qua các phiên bản phát triển (MVP).

| Phiên bản | Ngày cập nhật | Người thực hiện | Người phê duyệt | Mô tả chi tiết thay đổi (A/M/D) |
| :--- | :--- | :--- | :--- | :--- |
| Ver 1.0 | DD/MM/YYYY | [Tên PO] | [Tên Approver] | Tạo mới tài liệu cho hành trình [Tên tính năng] - MVP1 |
| Ver 1.1 | DD/MM/YYYY | [Tên PO] | [Tên Approver] | - [Modify] Cập nhật logic kiểm tra điều kiện bước X.Y <br> - [Add] Bổ sung mã lỗi error code [E-001] |

---

## 2. BỘ THUẬT NGỮ & GIẢI THÍCH TỪ NGỮ
Liệt kê và định nghĩa toàn bộ thuật ngữ chuyên ngành tài chính - ngân hàng hoặc kỹ thuật được sử dụng trong phạm vi tài liệu này.

| STT | Thuật ngữ / Từ viết tắt | Giải thích ý nghĩa nghiệp vụ |
| :--- | :--- | :--- |
| 1 | **IBMB** | Internet Banking Mobile Banking (Ngân hàng trực tuyến và Ngân hàng số di động) |
| 2 | **CIF** | Customer Information File (Mã hồ sơ thông tin khách hàng trên Core Banking) |
| 3 | [Thuật ngữ mới] | [Giải thích chi tiết] |

---

## 3. TỔNG QUAN HÀNH TRÌNH
Cung cấp bối cảnh tổng quát về tính năng cần phát triển và các đối tượng liên quan.

### 3.1. Mô tả User Story (US)
*   **As a** [Loại khách hàng: Khách hàng cá nhân NTB/ETB, Giao dịch viên,...]
*   **I want to** [Hành động/Tính năng muốn thực hiện trên App]
*   **So that** [Lợi ích nghiệp vụ/Trải nghiệm mang lại]

### 3.2. Mục đích & Yêu cầu Kinh doanh (Business Objectives)
*   Mô tả chi tiết bài toán kinh doanh cần giải quyết.
*   Các chỉ số đo lường hiệu quả (KPIs, SLA mong muốn) sau khi phát hành tính năng.

### 3.3. Phạm vi Yêu cầu (Scope)
*   **In-Scope (Nằm trong phạm vi)**: Liệt kê các luồng hành trình, chức năng sẽ phát triển trong phiên bản/MVP hiện tại.
*   **Out-of-Scope (Nằm ngoài phạm vi)**: Liệt kê các luồng hành trình hoãn lại cho các phiên bản/MVP sau.

### 3.4. Đối tượng Hưởng lợi & Đối tượng Sử dụng
*   **Đối tượng hưởng lợi**: [Khách hàng cá nhân, Ngân hàng bán lẻ, Khối quản trị rủi ro,...]
*   **Đối tượng sử dụng trực tiếp**: [Khách hàng sử dụng Mobile App, Giao dịch viên sử dụng DigiBank Service,...]

---

## 4. TÀI LIỆU LIÊN QUAN & THAM CHIẾU
Liệt kê các tài liệu nghiệp vụ, văn bản quy định pháp chế của Ngân hàng Nhà nước hoặc tài liệu kỹ thuật liên quan làm căn cứ viết BRD này.

| STT | Tên tài liệu | Mã tài liệu / Phiên bản | Tác giả / Đơn vị ban hành | Đường dẫn tham chiếu |
| :--- | :--- | :--- | :--- | :--- |
| 1 | Quyết định hạn mức giao dịch kênh số | [Mã văn bản] / QĐ-2025 | Khối Quản trị Rủi ro | [Link tài liệu] |
| 2 | Tài liệu đặc tả API tích hợp hệ thống | [Mã đặc tả] / Ver 2.0 | Đội ngũ Solution Architect | [Link tài liệu] |

---

## 5. THIẾT KẾ LUỒNG HÀNH TRÌNH ĐỀ XUẤT (TO-BE JOURNEY)
Mô tả sơ đồ luồng quy trình tổng quan và các điểm tích hợp.

*   **Sơ đồ luồng nghiệp vụ tổng quan (Link Miro)**: [Chèn Link Miro quy trình nghiệp vụ]
*   **Thiết kế giao diện chi tiết (Link Figma UI/UX)**: [Chèn Link Figma bản vẽ thiết kế]

---

## 6. ĐẶC TẢ CHI TIẾT LUỒNG HÀNH TRÌNH NGƯỜI DÙNG & HỆ THỐNG

Mô tả chi tiết hành trình theo pattern **Sub-step + Branching Matrix** (tham chiếu cẩm nang `Guides/po_writing_guide_for_ai_agents.md` Section IV). Mỗi bước cha là 1 bảng nhỏ, sub-step xếp thành row với 7 cột chuẩn.

### 6.1. Bảng Chi Tiết Hành Trình (To-be Journey Matrix)

- **Kênh thực hiện**: [Mobile App / Web Digibank]
- **Điều kiện đầu vào**: [Ví dụ: KH đã đăng nhập thành công + có TKTT hoạt động]

#### Bước 1: Truy cập & Kiểm tra thiết bị

| Sub-step | PIC | Thao tác / Check | Logic Business Rule | Pass → | Fail → | Mã sự kiện log |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **1.1**<br>Device Check | Hệ thống | KH bấm vào menu `[Cài đặt hạn mức]` | Quét Jailbreak/Root + kiểm tra gói dịch vụ IBMB còn hoạt động | Thiết bị + gói OK → **Bước 2.1** | Jailbreak/Root: chặn vào màn, Popup `[ERR_DEVICE_001]`.<br>Gói bị khóa: Popup `[ERR_PKG_002]` hướng dẫn liên hệ Hotline | Pass: `EVT_LIMIT_DEVICE_OK`<br>Fail: `EVT_LIMIT_DEVICE_BLOCKED` |

#### Bước 2: Nhập hạn mức mới & Validate

| Sub-step | PIC | Thao tác / Check | Logic Business Rule | Pass → | Fail → | Mã sự kiện log |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **2.1**<br>Input Limit | Khách hàng | KH nhập hạn mức mới mong muốn | Validate định dạng số + ngưỡng tối đa của gói dịch vụ IBMB | ≤ max của gói → **Bước 2.2** | > max: Popup `[ERR_LIMIT_003]` hiển thị ngưỡng tối đa cho phép | Pass: `EVT_LIMIT_INPUT_OK`<br>Fail: `EVT_LIMIT_INPUT_EXCEEDED` |
| **2.2**<br>Biometric Trigger Check | Hệ thống | Kiểm tra ngưỡng QĐ 2345/QĐ-NHNN | Hạn mức mới > 10tr/lần hoặc > 20tr/ngày → bắt buộc Face Authen | Cần biometric → **Bước 2.3** Face Authen.<br>Không cần → **Bước 3.1** trực tiếp | — | Pass: `EVT_LIMIT_BIOMETRIC_REQUIRED` / `EVT_LIMIT_BIOMETRIC_SKIPPED` |
| **2.3**<br>Face Authen | Khách hàng | Chụp ảnh live so khớp chip CCCD/VNeID | Tuân thủ QĐ 2345/QĐ-NHNN | Match → **Bước 3.1** | Mismatch ≥3 lần: Popup `[ERR_FACE_004]` dừng luồng | Pass: `EVT_LIMIT_FACE_MATCH`<br>Fail: `EVT_LIMIT_FACE_MISMATCH_LOCK` |

#### Bước 3: Xác thực OTP & Cập nhật

| Sub-step | PIC | Thao tác / Check | Logic Business Rule | Pass → | Fail → | Mã sự kiện log |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **3.1**<br>Send OTP | Hệ thống | Gửi SMS OTP đến SĐT đăng ký | Hiệu lực OTP 180 giây | OTP gửi thành công → **Bước 3.2** | Lỗi gateway SMS: Popup `[ERR_OTP_005]` (Thử lại / Hotline) | Pass: `EVT_LIMIT_OTP_SENT`<br>Fail: `EVT_LIMIT_OTP_GATEWAY_ERROR` |
| **3.2**<br>Verify OTP | Khách hàng | KH nhập mã OTP và bấm `[Xác nhận]` | Đếm số lần sai liên tiếp ≤ 3 | OTP đúng → **Bước 3.3** | Sai liên tiếp ≥3 lần: Popup `[ERR_OTP_006]` khóa luồng thay đổi hạn mức trong ngày | Pass: `EVT_LIMIT_OTP_VERIFIED`<br>Fail: `EVT_LIMIT_OTP_LOCKED_DAY` |
| **3.3**<br>Update DB | Hệ thống | Cập nhật hạn mức mới xuống DBS + Core Banking | Đồng bộ qua DIP | Cập nhật thành công → **Bước 4.1** | API timeout: Popup `[ERR_SYS_999]` (Thử lại / Hotline) | Pass: `EVT_LIMIT_UPDATE_SUCCESS`<br>Fail: `EVT_LIMIT_UPDATE_API_ERROR` |

#### Bước 4: Hiển thị kết quả

| Sub-step | PIC | Thao tác / Check | Logic Business Rule | Pass → | Fail → | Mã sự kiện log |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **4.1**<br>Show Success | Khách hàng | KH xem màn hình thông báo cập nhật thành công | Hiển thị hạn mức mới + ngày hiệu lực | Kết thúc luồng | — | Pass: `EVT_LIMIT_FLOW_COMPLETED` |

---

### 6.2. Quy tắc nghiệp vụ bổ sung (Design-time, Cross-cutting & Post-issuance Rules)

> **Lưu ý:** Các runtime check trong luồng (Device Check, Biometric Trigger, OTP Retry) đã inline vào matrix 6.1. Section này chỉ liệt kê rule **design-time / cross-cutting / post-issuance**.

1.  **[Quy tắc cross-cutting]** — VD: Drop-off Recovery 15 ngày: KH thoát giữa chừng → auto-save draft → quay lại trong 15 ngày khôi phục từ bước rớt.
2.  **[Quy tắc design-time]** — VD: Hạn mức tối đa của từng gói IBMB cố định theo bảng cấu hình của Khối Quản trị Rủi ro.
3.  **[Quy tắc post-issuance]** — VD: Sau khi đổi hạn mức thành công, KH không được đổi tiếp trong 24h kế tiếp.

---

## 7. TIÊU CHÍ PHI CHỨC NĂNG (NON-FUNCTIONAL REQUIREMENTS)
Các tiêu chí kỹ thuật hệ thống bắt buộc phải đáp ứng để đảm bảo tính ổn định và bảo mật.

### 7.1. Hiệu năng & Dung lượng (Performance & Scalability)
*   **Thời gian phản hồi (SLA Response Time)**: API kiểm tra hạn mức và thay đổi thông tin phải phản hồi trong vòng $\le 2$ giây.
*   **Khả năng chịu tải (Concurrent Users)**: Hệ thống đáp ứng tối thiểu `{X}` giao dịch đồng thời trong giờ cao điểm.

### 7.2. An toàn & Bảo mật (Security & Compliance)
*   Mọi thông tin cá nhân nhạy cảm (Số điện thoại, số CCCD, hạn mức tài khoản) khi truyền qua API hoặc lưu trữ dưới DB phải được mã hóa (Encrypted/Masked).
*   Tuân thủ quy định xác thực sinh trắc học bắt buộc theo quyết định của Ngân hàng Nhà nước đối với giao dịch vượt ngưỡng hạn mức quy định.

---

## 8. BẢN ĐỒ DỮ LIỆU TRUYỀN NHẬN (DATA MAPPING SCHEMAS)
Bảng đặc tả cấu trúc dữ liệu truyền nhận giữa các hệ thống Front-end và Back-end thông qua cổng DIP.

### Bảng 8.1: Dữ liệu Yêu cầu gửi đi (Request Schema)
| Tên Trường (Tiếng Việt) | Mã trường hệ thống | Kiểu dữ liệu | Thuộc tính (M/O) | Nguồn dữ liệu | Mô tả logic / Quy tắc biến đổi |
| :--- | :--- | :--- | :---: | :--- | :--- |
| Số CIF Khách hàng | `cif_number` | String | M | Core Banking | Mã số CIF được định danh sau khi KH đăng nhập thành công. |
| Hạn mức đề xuất | `proposed_limit` | Decimal | M | Nhập từ UI | Số tiền hạn mức mới do người dùng nhập vào. |

### Bảng 8.2: Dữ liệu Trả về (Response Schema)
| Tên Trường (Tiếng Việt) | Mã trường hệ thống | Kiểu dữ liệu | Thuộc tính (M/O) | Nguồn dữ liệu | Mô tả logic / Quy tắc biến đổi |
| :--- | :--- | :--- | :---: | :--- | :--- |
| Mã kết quả xử lý | `response_code` | String | M | DBS Core | Trả về `00` nếu thành công, hoặc mã lỗi tương ứng nếu thất bại. |
| Nội dung thông báo | `message` | String | M | DBS Core | Nội dung mô tả ngắn kết quả xử lý. |

---

## 9. QUY CHUẨN THÔNG BÁO & MÃ LỖI (MESSAGING & ERROR CODES)
Bảng tổng hợp nội dung các popup/thông báo hiển thị trên giao diện người dùng dựa trên phản hồi của hệ thống.

| Mã lỗi hệ thống | Trạng thái hiển thị | Tiêu đề popup (Title) | Nội dung mô tả chi tiết (Content) | Nút hành động (CTA) | Điều hướng chi tiết |
| :---: | :--- | :--- | :--- | :--- | :--- |
| `ERR-001` | Sai OTP lần 1, 2 | Mã OTP không chính xác | Quý khách đã nhập sai mã xác thực. Vui lòng thử lại. Quý khách còn `{n}` lần thử lại. | `[Thử lại]` | Cho phép khách hàng tắt popup và nhập lại mã OTP tại màn hình hiện tại. |
| `ERR-002` | Sai OTP lần 3 | Xác thực OTP thất bại | Quý khách đã nhập sai mã OTP quá 03 lần liên tiếp. Vì lý do bảo mật, giao dịch thay đổi hạn mức tạm thời bị khóa. | `[Đóng]` | Tự động điều hướng người dùng quay trở về màn hình Home/Dashboard. |
| `ERR-999` | Lỗi kết nối hệ thống | Giao dịch không thành công | Hệ thống đang gặp gián đoạn kết nối. Quý khách vui lòng thử lại sau ít phút hoặc liên hệ Hotline 1900xxxx để được hỗ trợ. | `[Hotline]` / `[Đóng]` | - `[Hotline]`: Tự động gọi tổng đài. <br> - `[Đóng]`: Quay về trang chủ. |
