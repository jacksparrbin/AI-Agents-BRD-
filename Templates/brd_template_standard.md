# [TÊN DỰ ÁN / TÊN HÀNH TRÌNH]
## TÀI LIỆU YÊU CẦU NGHIỆP VỤ (BUSINESS REQUIREMENT DOCUMENT - BRD)
**Mã tài liệu:** DCTBR-[J-XXXX] BRD [Tên tính năng]  
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
Mô tả chi tiết hành trình từ đầu vào (Entrypoint) cho tới điểm kết thúc tốt đẹp (Happy Path) và các luồng lỗi rẽ nhánh (Alternative/Exception Paths).

### Luồng hành trình chính: [Tên luồng - ví dụ: Thay đổi hạn mức giao dịch]
- **Kênh thực hiện**: [Mobile App / Web Digibank]
- **Điều kiện đầu vào**: [Ví dụ: Khách hàng đã đăng nhập thành công vào ứng dụng và có tài khoản thanh toán hoạt động (Active)]

| STT Bước | Thao tác Người dùng | Logic hệ thống & Quy tắc kiểm tra (Business Rules) | Kết quả & Chuyển bước tiếp theo |
| :---: | :--- | :--- | :--- |
| **Bước 1** | Khách hàng truy cập vào menu `[Cài đặt hạn mức]` | 1. Hệ thống thực hiện kiểm tra ngầm trạng thái gói dịch vụ của khách hàng. <br> 2. Kiểm tra thiết bị có thỏa mãn quy tắc bảo mật (Jailbreak/Root Check). | - Nếu Hợp lệ: Hiển thị màn hình cấu hình hạn mức -> Chuyển sang **Bước 2**. <br> - Nếu Không hợp lệ: Hiển thị popup báo lỗi [Mã lỗi]. |
| **Bước 2** | Khách hàng nhập hạn mức mới mong muốn và bấm `[Tiếp tục]` | 1. Hệ thống kiểm tra hạn mức nhập vào không được vượt quá hạn mức tối đa quy định của gói dịch vụ. <br> 2. Kiểm tra điều kiện sinh trắc học bắt buộc theo quy định NHNN đối với hạn mức thay đổi. | - Nếu Thỏa mãn: Gửi SMS OTP đến số điện thoại đăng ký -> Chuyển sang **Bước 3**. <br> - Nếu Không thỏa mãn: Hiển thị thông báo hướng dẫn. |
| **Bước 3** | Khách hàng nhập mã SMS OTP và bấm `[Xác nhận]` | 1. Hệ thống kiểm tra tính khớp của mã OTP. <br> 2. Kiểm tra giới hạn số lần nhập sai OTP (không quá 3 lần). | - Nếu Đúng OTP: Lưu hạn mức mới thành công -> Chuyển sang màn hình kết quả thành công (**Bước 4**). <br> - Nếu Sai OTP < 3 lần: Báo lỗi nhập sai và cho phép nhập lại. <br> - Nếu Sai OTP = 3 lần: Khoá luồng thay đổi hạn mức trong ngày. |
| **Bước 4** | Khách hàng xem màn hình thông báo thành công | Hệ thống cập nhật hạn mức mới xuống DB DBS và Core Banking. | Kết thúc luồng hành trình. |

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
