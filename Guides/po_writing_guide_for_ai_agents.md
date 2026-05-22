# CẨM NANG QUY HOẠCH & VIẾT TÀI LIỆU BRD DÀNH CHO PO & AI AGENTS (MSB CTB STANDARD)

Tài liệu này đóng vai trò là **"Requirement Skill"** chuẩn hóa dành cho Product Owner (PO) và các AI Agents thế hệ tiếp theo. Hướng dẫn này đúc kết toàn bộ phương pháp quy hoạch, cấu trúc logic nghiệp vụ, quy chuẩn kỹ thuật và quy tắc hành trình người dùng dựa trên thực tế phân tích 27 bộ tài liệu BRD thuộc dự án MSB CTB (Card, Lending, Limit, Onboarding, Payment).

---

## I. MỤC TIÊU & VAI TRÒ CỦA SẢN PHẨM PO
Mục tiêu cốt lõi của tài liệu BRD do PO hoặc AI Agent viết ra là **truyền tải trọn vẹn, chính xác, không mơ hồ** các yêu cầu nghiệp vụ từ Stakeholders (Đơn vị Kinh doanh, Vận hành, Pháp chế) tới Đội ngũ Phát triển (Dev, QA/QC, Solution Architect).

Một BRD chuẩn MSB CTB **KHÔNG** chỉ mô tả luồng UI/UX tĩnh, mà bắt buộc phải mô tả chi tiết:
1. **As-is (Hiện trạng) vs To-be (Luồng mong muốn)**.
2. **Logic hệ thống (System Business Rules)**: các điều kiện kiểm tra ngầm (back-end validation), tham chiếu bảng mã lỗi, hạn mức và phân loại khách hàng.
3. **UI Copy & Popup Standard**: viết rõ nội dung text của từng màn hình thông báo, tiêu đề, nội dung mô tả chi tiết và các nút hành động (CTA).
4. **Bản đồ tích hợp (Data Mapping)**: cấu trúc truyền nhận thông tin giữa Mobile App (Front-end), DIP (Middleware) và các Core Services (Core Banking, DigiLending, BPM Ops).

---

## II. BỘ THUẬT NGỮ NGHIỆP VỤ CHUẨN HÓA (GLOSSARY)
Khi viết BRD hoặc đóng vai PO Agent, bắt buộc phải sử dụng chính xác các thuật ngữ sau:

| Thuật ngữ | Tên đầy đủ | Ý nghĩa nghiệp vụ |
| :--- | :--- | :--- |
| **CIF** | Customer Information File | Mã định danh khách hàng duy nhất trên hệ thống Core Banking. |
| **NTB** | New to Bank | Khách hàng mới hoàn toàn, chưa từng có thông tin (CIF) tại ngân hàng. |
| **ETB** | Existing to Bank | Khách hàng hiện hữu, đã có mã CIF hoặc sản phẩm tại ngân hàng. |
| **IBMB** | Internet Banking Mobile Banking | Gói dịch vụ ngân hàng điện tử tích hợp. |
| **DBS** | Digibank Service | Hệ thống quản trị/cung cấp dịch vụ ngân hàng số. |
| **NFC** | Near-Field Communication | Công nghệ đọc chip CCCD không dây cự ly ngắn trên thiết bị di động. |
| **VNeID** | Định danh điện tử Quốc gia | Luồng liên kết tài khoản định danh điện tử của Bộ Công An để xác thực thay thế NFC. |
| **Face Authen (FA)** | Xác thực khuôn mặt | Xác thực sinh trắc học so khớp khuôn mặt live với ảnh trên chip CCCD/VNeID. |
| **AML/PCRT** | Anti-Money Laundering | Kiểm tra tuân thủ phòng chống rửa tiền và danh sách đen (Blacklist). |
| **Drop-off** | Rớt luồng | Trạng thái khách hàng dừng hành trình khi chưa hoàn tất toàn bộ quy trình. |
| **PA / non-PA** | Pre-Approved / Non-Pre-Approved | Khách hàng được phê duyệt hạn mức trước / Khách hàng thông thường. |
| **PS / non-PS** | Pre-screening / Non-Pre-screening | Nhóm khách hàng được sàng lọc hồ sơ trước theo chính sách ưu đãi trực tuyến. |
| **DIP** | Digital Integration Platform | Cổng tích hợp dịch vụ số (Middleware trung gian điều phối API). |
| **BPM Ops** | Business Process Management | Hệ thống phê duyệt hồ sơ và vận hành quy trình nghiệp vụ nội bộ. |

---

## III. PHƯƠNG PHÁP QUY HOẠCH LUỒNG NGHIỆP VỤ & HÀNH TRÌNH

### 1. Phân tích Hiện trạng (As-is) và Đề xuất (To-be)
*   **As-is**: Mô tả quy trình hiện tại, chỉ rõ các "điểm gãy" luồng (pain points).
    *   *Ví dụ thực tế*: Khách hàng rớt luồng đăng ký vay phải điền lại từ đầu; Link nhắc nhở qua SMS bị lỗi 404; thời gian phản hồi phê duyệt hồ sơ chậm trễ.
*   **To-be**: Thiết kế quy trình tự động hóa tối đa (Straight-Through Processing - STP), giảm điểm chạm vật lý, lưu trạng thái drop-off để phục hồi luồng.

### 2. Cấu trúc Sơ đồ luồng (Flowchart & Wireframe Links)
Trong mỗi BRD, bắt buộc phải cung cấp:
*   **Link Miro**: Cho sơ đồ luồng nghiệp vụ tổng quan và chi tiết trạng thái (State Diagram).
*   **Link Figma**: Cho thiết kế chi tiết các màn hình (UI/UX Hub).

---

## IV. QUY CHUẨN MÔ TẢ CHI TIẾT TỪNG BƯỚC HÀNH TRÌNH (THE PO MATRIX)
Đây là cấu phần quan trọng nhất trong BRD. Khi mô tả một bước hành trình, PO Agent phải sử dụng cấu trúc bảng chuẩn hóa sau:

### Bảng cấu trúc mô tả bước nghiệp vụ:
```markdown
### Bước X.Y: [Tên Bước Nghiệp Vụ]
- **Kênh / PIC**: [Mobile App | Hệ thống DBS | Hệ thống Core | Khách hàng]
- **Entry Point**: [Mô tả cách thức/màn hình dẫn khách hàng vào bước này]

| Thao tác người dùng | Logic & Kiểm tra hệ thống (Business Rules) | Kết quả & Chuyển bước (Next Action) |
| :--- | :--- | :--- |
| Khách hàng bấm [Tên Button] | 1. Hệ thống thực hiện kiểm tra ngầm... <br> 2. Các quy tắc xác thực dữ liệu đầu vào... | - Nếu Đạt: Chuyển sang Bước X.Z <br> - Nếu Lỗi: Hiển thị popup lỗi [Mã lỗi] |
```

### Quy tắc kiểm tra ngầm hệ thống bắt buộc (System Check Checklists):
1.  **Device Validation (Kiểm tra thiết bị)**:
    *   *Root/Jailbreak Check*: Ngăn chặn cài đặt app trên thiết bị đã root hoặc bẻ khoá.
    *   *Device Registration Limit*: Kiểm tra giới hạn đăng ký tài khoản trên cùng 1 thiết bị.
2.  **Time & Maintenance Check (Kiểm tra giờ vận hành)**:
    *   *Batch Time Check*: Kiểm tra nếu khách hàng đăng ký ngoài giờ làm việc hệ thống (ví dụ: giờ chạy batch thanh toán/đối soát từ 22h đêm - 7h sáng).
3.  **Hạn mức & Giới hạn thử lại (Limits & Retry Controls)**:
    *   *SMS OTP Retry Limit*: Nhập sai OTP tối đa 3 lần liên tiếp -> Khoá giao dịch hiện tại, yêu cầu gửi lại OTP. Gửi lại tối đa 3 lần/phiên -> huỷ luồng bắt đăng ký lại.
    *   *Transaction Velocity Limits*: Hạn mức giao dịch tối đa theo lần, theo ngày của gói dịch vụ IBMB (ví dụ: chuyển khoản nhanh Napas 24/7 tối đa 499,999,999 VND/lần).
4.  **Kiểm tra Trùng lặp (Duplicate Check)**:
    *   *Phone / ID Duplicate Check*: Kiểm tra xem Số điện thoại hoặc CCCD đã được đăng ký tài khoản IBMB trên cả App mới và App cũ chưa để điều hướng thích hợp (Login hay gọi Call Center).
5.  **Drop-off & Recovery (Thu hồi rớt luồng)**:
    *   Nếu khách hàng rớt luồng và quay lại trong vòng $\le 15$ ngày: Hiển thị màn hình chào mừng quay lại, tự động điền (Auto-fill) toàn bộ thông tin đã điền trước đó.
    *   Nếu $> 15$ ngày: Huỷ hồ sơ tạm, bắt đầu luồng đăng ký mới.

---

## V. QUY CHUẨN VIẾT UI COPY & POPUP THÔNG BÁO LỖI
Một lỗi rất phổ biến của PO là chỉ viết: *"Hệ thống báo lỗi cho khách hàng"*. 
**Quy chuẩn MSB CTB yêu cầu viết rõ chính xác nội dung hiển thị**:

### Cấu trúc chuẩn của một đặc tả Thông báo (Notification / Popup Spec):
1.  **Title (Tiêu đề)**: Ngắn gọn, nêu bật bản chất sự việc (Ví dụ: `Thiết bị không đạt điều kiện`, `Mã OTP hết hiệu lực`).
2.  **Content (Nội dung chi tiết)**: Trực quan, dễ hiểu, hướng dẫn cách giải quyết. Sử dụng biến dynamic `{n}` nếu có (Ví dụ: `Quý khách còn {n} lần thử lại (n = 3 - số lần nhập sai)`).
3.  **CTA (Call To Action)**: Danh sách các nút bấm và hành động điều hướng cụ thể (Ví dụ: `CTA1: Gọi tổng đài (Hotline: 1900xxxx)`, `CTA2: Đóng (Quay về trang chủ)`).

---

## VI. QUY CHUẨN BẢN ĐỒ DỮ LIỆU & TÍCH HỢP (DATA MAPPING SPECIFICATION)
Khi thiết kế luồng kết nối API hoặc xử lý hồ sơ, PO Agent phải cung cấp bảng Data Mapping để Dev/QA hiểu luồng thông tin:

| STT | Tên trường (Tiếng Việt) | Mã trường (System Field Name) | Phân loại dữ liệu | Thuộc tính (Bắt buộc/Tùy chọn) | Nguồn dữ liệu | Logic biến đổi / Mô tả |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | Số tài khoản vay | `accountId` | String | Bắt buộc | Core Banking | Trả về từ danh sách tài khoản liên kết với CIF khách hàng. |
| 2 | Hạn mức thấu chi | `credit_limit` | Decimal | Bắt buộc | DigiLending | Số tiền tối đa khách hàng được cấp. |
| 3 | Lãi suất thấu chi | `availableBalance` | Decimal | Bắt buộc | DigiLending | So sánh Ngày hết hạn (N1) với Ngày hiện tại (N2): N1 < N2 -> availableBalance = 0. |

---

## VII. HƯỚNG DẪN DÀNH CHO AI PO AGENT: QUY TRÌNH 4 BƯỚC TỪ INPUT ĐẾN BRD CHUẨN

Khi AI Agent nhận được yêu cầu sơ khai từ Stakeholder, hãy thực hiện theo đúng các bước sau để xuất ra tài liệu hoàn hảo:

### Bước 1: Khai phá & Phân nhóm
*   Xác định nghiệp vụ thuộc nhóm nào: **Onboarding (Đăng ký mới), Card (Thẻ), Lending (Tín dụng), Limit (Hạn mức), hay Payment (Thanh toán)**.
*   Xác định đối tượng khách hàng mục tiêu: **NTB** hay **ETB**.

### Bước 2: Thiết lập Sơ đồ Quy trình nghiệp vụ
*   Xác định hành trình cốt lõi (Happy Path) và các nhánh rẽ lỗi (Alternative/Exception Paths).
*   Định nghĩa rõ các cổng kiểm tra ngầm (Jailbreak, AML check, CIF check, SMS OTP retry limits).

### Bước 2.5: Thực hiện Vòng lặp phản hồi & Chốt chặn đầu vào (Mandatory Input Loop)
> [!IMPORTANT]
> **Tuyệt đối không được bỏ qua bước này để tự ý gen tài liệu ngay lập tức sau khi người dùng trả lời 1-5 câu hỏi làm rõ.**
> 
> Sau khi nhận được câu trả lời/phản hồi từ người dùng cho danh sách câu hỏi làm rõ ở Phase 2, AI Agent **bắt buộc** phải hỏi lại người dùng câu hỏi xác nhận sau trước khi chuyển sang bước viết tài liệu:
> *"Cảm ơn anh/chị. Em đã ghi nhận các phương án lựa chọn trên. Trước khi em tiến hành biên soạn đặc tả BRD chi tiết, anh/chị có cần chỉnh sửa, bổ sung hoặc thêm mới thông tin đầu vào nào nữa không?"*
> 
> **Logic Xử lý Đầu vào**:
> *   **Nếu người dùng chọn KHÔNG (Không bổ sung gì thêm)**: Xác nhận đầu vào đã chốt và chuyển sang **Bước 3** (Áp dụng mẫu và biên soạn).
> *   **Nếu người dùng chọn CÓ (Có bổ sung/chỉnh sửa)**: Lấy toàn bộ thông tin mới người dùng vừa cung cấp, quay lại **Bước 1** để cập nhật phân tích nghiệp vụ và chạy lại quy trình làm rõ thông tin nếu phát hiện khoảng trống nghiệp vụ mới. Lặp lại bước xác nhận này cho tới khi người dùng hoàn toàn chốt đầu vào.

### Bước 3: Áp dụng Mẫu Tài liệu Tương ứng
*   Nếu là đăng ký/định danh -> Sử dụng `brd_template_onboarding.md`.
*   Nếu là thanh toán/tín dụng -> Sử dụng `brd_template_transaction_lending.md`.
*   Các tính năng khác -> Sử dụng `brd_template_standard.md`.

### Bước 4: Soát lỗi & Hoàn thiện UI Copy
*   Kiểm tra lại toàn bộ các popup thông báo lỗi xem đã đầy đủ 3 thành phần: **Title, Content, CTA** chưa.
*   Đảm bảo không có placeholders rỗng (như `[cần điền vào đây]`, `...`). Tất cả thông tin phải được giả lập logic nghiệp vụ thực tế hoặc ghi chú tham chiếu cụ thể.
