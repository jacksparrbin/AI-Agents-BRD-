# CẨM NANG QUY HOẠCH & VIẾT TÀI LIỆU BRD DÀNH CHO PO & AI AGENTS (Digital Banking Standard)

Tài liệu này đóng vai trò là **"Requirement Skill"** chuẩn hóa dành cho Product Owner (PO) và các AI Agents thế hệ tiếp theo. Hướng dẫn này đúc kết toàn bộ phương pháp quy hoạch, cấu trúc logic nghiệp vụ, quy chuẩn kỹ thuật và quy tắc hành trình người dùng dựa trên thực tế phân tích 27 bộ tài liệu BRD thuộc dự án Digital Banking (Card, Lending, Limit, Onboarding, Payment).

---

## I. MỤC TIÊU & VAI TRÒ CỦA SẢN PHẨM PO
Mục tiêu cốt lõi của tài liệu BRD do PO hoặc AI Agent viết ra là **truyền tải trọn vẹn, chính xác, không mơ hồ** các yêu cầu nghiệp vụ từ Stakeholders (Đơn vị Kinh doanh, Vận hành, Pháp chế) tới Đội ngũ Phát triển (Dev, QA/QC, Solution Architect).

Một BRD chuẩn Digital Banking **KHÔNG** chỉ mô tả luồng UI/UX tĩnh, mà bắt buộc phải mô tả chi tiết:
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

## IV. QUY CHUẨN MÔ TẢ CHI TIẾT TỪNG BƯỚC HÀNH TRÌNH (PATTERN: SUB-STEP + BRANCHING MATRIX)

Đây là cấu phần quan trọng nhất trong BRD. Áp dụng pattern **Sub-step + Branching Matrix** để tích hợp toàn bộ corner case vào 1 luồng hành trình tuần tự, tránh tách rời rule khỏi luồng.

### 1. Nguyên tắc cốt lõi

1.  **Bẻ nhỏ bước cha thành sub-step `X.Y`** (Bước X.Y: bước cha là X, sub-step là Y). Mỗi sub-step thực hiện **đúng 1 check / hành động duy nhất**. Tuyệt đối không nhồi nhiều check ngầm vào 1 row.
2.  **Mỗi sub-step có Pass/Fail branching rõ ràng** trong cell "Kết quả & Chuyển bước". Reader nhìn cell là biết: pass → step nào, fail → popup nào / dừng luồng / retry.
3.  **Runtime check phải inline vào matrix tại đúng sub-step**, KHÔNG được tách ra Section "Business Rules bổ sung" rời rạc. Section "Business Rules bổ sung" chỉ chứa rule **design-time**, **cross-cutting** hoặc **post-issuance**.
4.  **Mỗi sub-step có Mã sự kiện log** theo convention `EVT_<MODULE>_<ACTION>_<RESULT>` để Dev/Ops đối soát log khi rule trigger.

### 2. Cấu trúc bảng chuẩn hóa (7 cột)

Mỗi bước cha (Bước 1, 2, 3, ...) là 1 bảng nhỏ riêng. Sub-step xếp thành **row**, attribute là **cột**:

```markdown
#### Bước X: [Tên Bước Cha — VD: Hoàn tất hồ sơ & ký số]

| Sub-step | PIC | Thao tác / Check | Logic Business Rule | Pass → | Fail → | Mã sự kiện log |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **X.1**<br>[Tên ngắn] | [Hệ thống / Khách hàng] | [Mô tả thao tác hoặc check 1 dòng] | [Rule chi tiết: ngưỡng, công thức, tham chiếu QĐ] | [Điều kiện pass] → **Bước X.2** | [Điều kiện fail] → Popup `[ERR_<MOD>_<NUM>]` + action cụ thể | Pass: `EVT_<MOD>_<ACT>_OK`<br>Fail: `EVT_<MOD>_<ACT>_BLOCKED` |
| **X.2**<br>... | ... | ... | ... | ... | ... | ... |
```

### 3. Convention Mã sự kiện log

Format: `EVT_<MODULE>_<ACTION>_<RESULT>`

| Phần | Giá trị | Ví dụ |
| :--- | :--- | :--- |
| `<MODULE>` | Mã phân hệ viết tắt | `CARD`, `LEND`, `PAY`, `ONB`, `LIMIT`, `AUTH` |
| `<ACTION>` | Tên check/thao tác | `BATCH`, `FACE`, `OTP`, `ISSUE`, `KYC`, `VELOCITY` |
| `<RESULT>` | Kết quả | `OK` / `BLOCKED` / `LOCKED_24H` / `MISMATCH_LOCK` / `API_ERROR` / `RETRY` |

**Ví dụ thực tế:**
- `EVT_CARD_BATCH_OK` — Batch Time Check pass
- `EVT_CARD_FACE_MISMATCH_LOCK` — Face Authen mismatch ≥ 3 lần, khóa luồng
- `EVT_ONB_OTP_LOCKED_24H` — OTP sai ≥ 5 lần khóa 24h
- `EVT_LEND_VELOCITY_EXCEEDED` — Vượt hạn mức giao dịch/ngày

Mỗi sub-step ÍT NHẤT có 1 mã log cho Pass; nếu có nhánh Fail thì mã log Fail phải khác mã log Pass.

### 4. Ví dụ minh họa — Bước "Ký số mở thẻ tín dụng"

#### Bước 4: Hoàn tất hồ sơ & ký số phát hành thẻ

| Sub-step | PIC | Thao tác / Check | Logic Business Rule | Pass → | Fail → | Mã sự kiện log |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **4.1**<br>Batch Time Check | Hệ thống | So sánh server time với cửa sổ batch 22h-01h | KH ký số trong batch window → chặn giao dịch | Ngoài batch → **Bước 4.2** | Trong batch → Popup `[ERR_CARD_010]` + lưu draft → KH retry sau 01h | Pass: `EVT_CARD_BATCH_OK`<br>Fail: `EVT_CARD_BATCH_BLOCKED` |
| **4.2**<br>Face Authen | Khách hàng | Chụp ảnh live vs chip CCCD/VNeID | Tuân thủ QĐ 2345/QĐ-NHNN, đếm mismatch | Match → **Bước 4.3** | <3 lần: retry. ≥3 lần: dừng luồng + Popup `[ERR_CARD_011]` | Pass: `EVT_CARD_FACE_MATCH`<br>Fail (lock): `EVT_CARD_FACE_MISMATCH_LOCK` |
| **4.3**<br>OTP Verify | Khách hàng | Nhập OTP SMS/Smart OTP | Đếm sai liên tiếp + resend | Đúng → **Bước 4.4** | Sai ≥5 lần: khóa 24h + Popup `[ERR_CARD_012]` | Pass: `EVT_CARD_OTP_VERIFIED`<br>Fail: `EVT_CARD_OTP_LOCKED_24H` |

### 5. Thư viện kiểm tra ngầm hệ thống bắt buộc (System Check Library)

Khi viết matrix, PO Agent phải kiểm tra xem hành trình có dính các check sau không, và nếu CÓ thì BẮT BUỘC mỗi check là 1 sub-step riêng trong matrix (không gom vào 1 row):

1.  **Device Validation** — Root/Jailbreak Check, Device Registration Limit. EVT prefix: `EVT_<MOD>_DEVICE_*`.
2.  **Batch Time Check** — Cửa sổ batch đối soát Core Banking (thường 22h-01h hoặc 22h-07h). EVT prefix: `EVT_<MOD>_BATCH_*`.
3.  **Face Authen / Biometric** (QĐ 2345/QĐ-NHNN) — Liveness vs chip CCCD/VNeID. Bắt buộc khi giao dịch vượt ngưỡng 10tr/lần hoặc 20tr/ngày, hoặc khi mở sản phẩm tài chính. EVT: `EVT_<MOD>_FACE_*`.
4.  **OTP Retry Limit** — Mặc định sai liên tiếp ≤ 5 lần (thẻ tín dụng) hoặc ≤ 3 lần (giao dịch nhỏ); resend ≤ 3 lần/phiên. EVT: `EVT_<MOD>_OTP_*`.
5.  **Velocity Limits** — Hạn mức giao dịch tối đa theo lần / theo ngày (VD: chuyển khoản Napas 24/7 tối đa 499,999,999 VND/lần). EVT: `EVT_<MOD>_VELOCITY_*`.
6.  **Duplicate Check** — SĐT / CCCD đã đăng ký? CIF tồn tại? EVT: `EVT_<MOD>_DUPLICATE_*`.
7.  **Drop-off & Recovery** — KH rớt luồng quay lại ≤ 15 ngày → auto-fill. > 15 ngày → khởi tạo lại. EVT: `EVT_<MOD>_DROPOFF_*`.

### 6. Vị trí của "Business Rules bổ sung"

Sau bảng matrix tổng (Section X.1), tạo Section X.2 **"Quy tắc nghiệp vụ bổ sung (Design-time, Cross-cutting & Post-issuance Rules)"**. Trong section này:

✅ **ĐƯỢC PHÉP** liệt kê:
- Design-time rules (VD: quy chuẩn in ấn, schema hợp đồng cố định)
- Cross-cutting rules (VD: drop-off recovery áp xuyên suốt mọi step)
- Post-issuance rules (VD: vòng đời sản phẩm sau khi đã phát hành)

❌ **KHÔNG ĐƯỢC PHÉP** liệt kê:
- Runtime check (Batch, Face, OTP, Velocity, Device) — đã inline vào matrix
- Bất kỳ rule nào gắn với 1 sub-step cụ thể trong matrix

Tiêu chí tự kiểm tra: nếu rule có thể chỉ rõ "rule này được kiểm tra ở Bước X.Y" → rule đó PHẢI nằm trong matrix, không phải Section X.2.

---

## V. QUY CHUẨN VIẾT UI COPY & POPUP THÔNG BÁO LỖI
Một lỗi rất phổ biến của PO là chỉ viết: *"Hệ thống báo lỗi cho khách hàng"*. 
**Quy chuẩn Digital Banking yêu cầu viết rõ chính xác nội dung hiển thị**:

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
