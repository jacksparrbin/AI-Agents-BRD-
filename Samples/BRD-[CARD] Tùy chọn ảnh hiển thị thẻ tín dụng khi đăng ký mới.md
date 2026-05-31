# TÀI LIỆU YÊU CẦU NGHIỆP VỤ (BUSINESS REQUIREMENT DOCUMENT - BRD)
## HÀNH TRÌNH TÙY CHỌN ẢNH HIỂN THỊ THẺ TÍN DỤNG KHI KHÁCH HÀNG ĐĂNG KÝ MỚI (PERSONALIZED CARD ART SELECTION)
**Mã tài liệu:** BRD-[CARD-008] Personalized Card Art Selection  
**Phân hệ:** Card Issuance - Onboarding  
**Phiên bản:** Ver 1.2  
**Ngày cập nhật:** 31/05/2026  
**Trạng thái:** Draft

---

## 1. LỊCH SỬ THAY ĐỔI TÀI LIỆU

Bảng ghi nhận toàn bộ quá trình cập nhật, chỉnh sửa nội dung tài liệu qua các phiên bản phát triển (MVP).

| Phiên bản | Ngày cập nhật | Người thực hiện | Người phê duyệt | Mô tả chi tiết thay đổi (A/M/D) |
| :--- | :--- | :--- | :--- | :--- |
| Ver 1.0 | 31/05/2026 | AI Senior PO Agent | Stakeholder | **[NEW]** Khởi tạo tài liệu đặc tả yêu cầu nghiệp vụ cho tính năng tùy chọn ảnh hiển thị (Card Art) của thẻ tín dụng khi đăng ký mới trực tuyến. |
| Ver 1.1 | 31/05/2026 | AI Senior PO Agent | Stakeholder | **[MODIFY]** Cập nhật toàn diện dựa trên phản hồi của QA Validator (Agent 2):<br>- Bổ sung đầy đủ 9 thuật ngữ mới vào bảng Glossary (Section 2).<br>- Tích hợp các chốt chặn ngầm bắt buộc (Face Authen QĐ 2345, Batch Time Check, OTP limits, Velocity Limits) vào Matrix Table (Section 5.1) và Business Rules (Section 5.2).<br>- Chuẩn hóa định dạng mã lỗi các Popup thông báo về chuẩn `[ERR_CARD_XXX]` (Section 6).<br>- Bổ sung thêm Section 10 "Tài liệu tham chiếu" hoàn chỉnh. |
| Ver 1.2 | 31/05/2026 | AI Senior PO Agent | Stakeholder | **[MODIFY]** Refactor Section 5+6 theo pattern "Sub-step + Branching Matrix":<br>- **Section 5.1:** Mở rộng từ 5 bước flat → 11 sub-step (1.1 → 5.2), bố cục 5 bảng nhỏ theo bước cha; mỗi sub-step là 1 row với 7 cột (Sub-step / PIC / Thao tác / Logic / Pass / Fail / Mã sự kiện log).<br>- Tích hợp 4 runtime check (Batch Time, Face Authen, OTP Retry, API Core Card) trực tiếp vào matrix tại Bước 4.1-4.4.<br>- Bổ sung naming convention Mã sự kiện log `EVT_<MODULE>_<ACTION>_<RESULT>` để Dev/Ops đối soát.<br>- **Section 5.2:** Giảm từ 8 rule → 4 rule (chỉ giữ design-time + cross-cutting + post-issuance). Runtime check đã inline vào matrix.<br>- **Section 6:** Bổ sung 3 popup mới `[ERR_CARD_010]` Batch maintenance, `[ERR_CARD_011]` Face mismatch lock, `[ERR_CARD_012]` OTP locked 24h. |

---

## 2. BỘ THUẬT NGỮ NGHIỆP VỤ & GIẢI THÍCH TỪ NGỮ

Liệt kê và định nghĩa toàn bộ thuật ngữ chuyên ngành tài chính - ngân hàng và kỹ thuật được sử dụng trong phạm vi tài liệu này.

| STT | Thuật ngữ / Từ viết tắt | Giải thích ý nghĩa nghiệp vụ |
| :--- | :--- | :--- |
| 1 | **Card Art (CA)** | Thiết kế hình ảnh hiển thị ở mặt trước của thẻ tín dụng (cả thẻ vật lý và thẻ ảo). |
| 2 | **Card Art ID** | Mã định danh duy nhất của thiết kế ảnh thẻ trong hệ thống cơ sở dữ liệu để đối khớp in ấn và hiển thị. |
| 3 | **NTB** | New to Bank (Khách hàng mới hoàn toàn, chưa từng có thông tin tại ngân hàng). |
| 4 | **ETB** | Existing to Bank (Khách hàng hiện hữu, đã có mã CIF hoặc tài khoản tại ngân hàng). |
| 5 | **DBS** | Digibank Service (Hệ thống quản trị/cung cấp dịch vụ ngân hàng số). |
| 6 | **DIP** | Digital Integration Platform (Cổng kết nối tích hợp dữ liệu API / Middleware trung gian). |
| 7 | **Core Card** | Hệ thống quản lý thẻ cốt lõi của ngân hàng (ví dụ: Way4 hoặc SmartVista). |
| 8 | **CPS** | Card Personalization System (Hệ thống cá nhân hóa thẻ / Trung tâm in ấn phôi thẻ nhựa vật lý). |
| 9 | **STP** | Straight-Through Processing (Luồng xử lý tự động xuyên suốt 100%, không cần phê duyệt thủ công). |
| 10 | **Instant Digital Card** | Thẻ tín dụng phi vật lý (thẻ ảo) được mở và kích hoạt sử dụng ngay trên App Digibank sau khi duyệt hạn mức thành công. |
| 11 | **Full Back-side Printing** | Quy chuẩn in toàn bộ thông tin nhạy cảm của thẻ (Số thẻ, Ngày hết hạn, CVV) ở mặt sau thẻ vật lý nhằm tăng tính bảo mật và thẩm mỹ mặt trước. |
| 12 | **CIF** | Customer Information File (Mã định danh hồ sơ thông tin khách hàng duy nhất trên hệ thống Core Banking). |
| 13 | **OTP (One-Time Password)** | Mật khẩu xác thực sử dụng một lần gửi qua SMS hoặc sinh từ Smart OTP để xác nhận ký số giao dịch mở thẻ. |
| 14 | **API (Application Programming Interface)** | Giao diện lập trình ứng dụng dùng để kết nối và truyền nhận dữ liệu đồng bộ giữa các phân hệ hệ thống (App, DBS, DIP, Core Card). |
| 15 | **FE (Front-End)** | Giao diện người dùng hiển thị trực quan trên ứng dụng di động Mobile App Digibank. |
| 16 | **BE (Back-End)** | Hệ thống xử lý logic nghiệp vụ và dữ liệu phía sau (DBS, Core). |
| 17 | **PAN (Primary Account Number)** | Số tài khoản thẻ tín dụng duy nhất gồm 16 chữ số được in/khắc trên phôi thẻ. |
| 18 | **CVV (Card Verification Value)** | Mã bảo mật gồm 3 chữ số in ở mặt sau thẻ dùng để xác thực các giao dịch trực tuyến. |
| 19 | **JCB / Visa** | Các Tổ chức thẻ quốc tế liên kết với ngân hàng để phát hành thẻ và thanh toán toàn cầu. |
| 20 | **VIP (Very Important Person)** | Phân khúc khách hàng ưu tiên (Priority Banking) được cấp các đặc quyền dịch vụ và mẫu Card Art cao cấp. |

---

## 3. TỔNG QUAN HÀNH TRÌNH NGHIỆP VỤ

### 3.1. Mô tả User Story (US)
*   **As a** Khách hàng cá nhân thực hiện đăng ký mở thẻ tín dụng mới trên ứng dụng Mobile App Digibank.
*   **I want to** tùy chọn mẫu thiết kế ảnh hiển thị (Card Art) của thẻ tín dụng từ thư viện thiết kế độc quyền của ngân hàng.
*   **So that** sở hữu một chiếc thẻ tín dụng có diện mạo cá nhân hóa theo phong cách riêng, hiển thị đồng nhất cả trên ứng dụng ngân hàng số và bản thẻ vật lý được in ra.

### 3.2. Mục đích & Yêu cầu Kinh doanh (Business Objectives)
*   **Tối ưu hóa tỷ lệ chuyển đổi:** Thu hút đối tượng khách hàng trẻ tuổi (Gen Z, Millennials) bằng cách đưa yếu tố thời trang và phong cách cá nhân vào sản phẩm tài chính truyền thống, dự kiến tăng **15% - 20%** lượng thẻ mở mới trong năm đầu tiên.
*   **Số hóa toàn diện trải nghiệm khách hàng (STP):** Cung cấp giải pháp mở thẻ 100% online, cho phép kích hoạt và sử dụng thẻ ảo tức thì với Card Art đã chọn để tăng mức độ tương tác ban đầu.
*   **Nâng cao tính bảo mật vật lý:** Áp dụng công nghệ in thông tin nhạy cảm ở mặt sau giúp ngăn chặn rủi ro lộ thông tin khi thanh toán tại quầy hoặc rút tiền tại ATM.

### 3.3. Phạm vi Yêu cầu (Scope)
*   **In-Scope (Nằm trong phạm vi):**
    1.  Tích hợp bước lựa chọn thiết kế Card Art vào luồng đăng ký mở thẻ tín dụng trên Mobile App Digibank (cho cả luồng NTB và ETB).
    2.  Hiển thị kho thư viện ảnh thẻ (Card Art Library) phân chia theo các chủ đề: GenZ năng động, Tối giản (Minimalist), Sang trọng (Luxury/Priority), Nghệ thuật (Art)...
    3.  Thiết lập bộ lọc phân hạng hiển thị: Các thiết kế cao cấp (Luxury, Priority) chỉ hiển thị khi khách hàng đăng ký dòng thẻ Bạch Kim (Platinum) hoặc thuộc phân khúc khách hàng VIP.
    4.  Lưu trữ thông tin mã thiết kế thẻ `card_art_id` đi kèm suốt vòng đời của thẻ.
    5.  Tích hợp API chuyển tiếp thông tin `card_art_id` từ Mobile App -> DBS -> DIP -> Core Card -> CPS (Hệ thống in thẻ) để in ấn phôi thẻ vật lý cá nhân hóa.
    6.  Áp dụng quy chuẩn in thông tin bảo mật ở mặt sau thẻ vật lý (Full Back-side Printing) cho tất cả thẻ tùy chọn Card Art.
    7.  Cấp thẻ ảo (Instant Digital Card) hiển thị đúng hình Card Art đã chọn ngay sau khi hồ sơ được phê duyệt thành công trên App để liên kết Apple Pay và chi tiêu tức thì.
    8.  Cho phép khách hàng thay đổi thiết kế ảnh thẻ hiển thị trên App (miễn phí đối với thẻ ảo). Đối với thẻ vật lý, nếu muốn đổi mẫu ảnh mới sẽ thực hiện qua luồng yêu cầu phát hành lại thẻ (áp dụng phí phát hành lại tiêu chuẩn).
*   **Out-of-Scope (Nằm ngoài phạm vi MVP1):**
    1.  Tính năng cho phép khách hàng tự tải ảnh cá nhân từ thư viện ảnh điện thoại (để kiểm soát rủi ro bản quyền, thuần phong mỹ tục - hoãn lại MVP2).
    2.  Luồng đăng ký thẻ tại quầy giao dịch vật lý (tính năng chỉ áp dụng độc quyền trên ứng dụng Mobile App Digibank).
    3.  Thu phí tùy chọn Card Art khi mở mới thẻ (miễn phí cho tất cả KH đăng ký mới ở MVP1).

---

## 4. SƠ ĐỒ LUỒNG QUY TRÌNH NGHIỆP VỤ (MERMAID SEQUENCE DIAGRAM)

Dưới đây là sơ đồ chuỗi hành trình nghiệp To-be, thể hiện sự tương tác và luồng truyền nhận dữ liệu giữa Khách hàng, Mobile App, Hệ thống quản trị DBS, Cổng tích hợp DIP, Core Card và Trung tâm In thẻ vật lý (CPS):

```mermaid
sequenceDiagram
    autonumber
    actor KH as Khách hàng
    participant APP as Mobile App (FE)
    participant DBS as Hệ thống DBS (BE)
    participant DIP as Cổng Tích hợp DIP
    participant CORE as Core Card (Way4/SmartVista)
    participant CPS as Trung tâm In Thẻ (CPS)

    KH->>APP: Chọn "Đăng ký mở thẻ tín dụng" & Chọn dòng thẻ muốn mở
    APP->>DBS: Truy vấn danh sách mẫu Card Art (Thông tin dòng thẻ, Segment KH)
    DBS->>DBS: Lọc danh mục ảnh khả dụng theo Hạng thẻ & Segment KH (Mass/VIP Priority)
    DBS-->>APP: Trả về danh sách Card Art hợp lệ (Gồm: card_art_id, tên ảnh, URL hình ảnh)
    KH->>APP: Xem danh mục theo chủ đề, chọn mẫu Card Art ưa thích
    APP->>DBS: Tạm lưu thông tin hồ sơ đăng ký kèm card_art_id đã chọn
    KH->>APP: Điền các thông tin bổ sung & Xác nhận hợp đồng (Ký số OTP/Face Authen)
    APP->>DBS: Gửi hồ sơ đăng ký phát hành thẻ chính thức lên hệ thống
    DBS->>DIP: Gửi yêu cầu phát hành thẻ trực tuyến (Chứa card_art_id)
    DIP->>CORE: Đồng bộ hồ sơ, khởi tạo số thẻ (PAN) kết nối với card_art_id
    CORE-->>DBS: Phản hồi kết quả tạo thẻ thành công (Mở thẻ ảo ngay tức thì)
    DBS-->>APP: Cập nhật trạng thái và điều hướng sang màn hình "Mở thẻ thành công"
    APP->>KH: Hiển thị Thẻ ảo trên App với hình ảnh Card Art tùy chọn đã thiết kế
    Note over KH,APP: KH kích hoạt thẻ ảo & liên kết Apple Pay chi tiêu lập tức
    DBS->>CPS: Đẩy dữ liệu lệnh in thẻ vật lý (Số thẻ, Tên KH, Mã card_art_id)
    CPS->>CPS: Máy in nhận diện mã thiết kế, thực hiện in ấn thẻ vật lý:<br/>- Mặt trước: Ảnh Card Art tùy chọn + Tên khách hàng<br/>- Mặt sau: Số thẻ, Hạn sử dụng, mã CVV, dải từ
    CPS-->>KH: Chuyển phát nhanh thẻ vật lý cá nhân hóa đến địa chỉ KH trong 3-5 ngày
```

---

## 5. ĐẶC TẢ CHI TIẾT LUỒNG HÀNH TRÌNH & BUSINESS RULES

- **Kênh thực hiện:** Mobile App Digibank.
- **Điều kiện đầu vào:**
    1.  Khách hàng đã đăng nhập ứng dụng Mobile App Digibank thành công.
    2.  Thiết bị di động của khách hàng vượt qua chốt chặn bảo mật (Jailbreak/Root Check).
    3.  Khách hàng được phê duyệt cấp hạn mức thẻ tín dụng trực tuyến (luồng Instant Approval) hoặc hoàn tất khai báo hồ sơ mở thẻ.

### 5.1. Bảng Chi Tiết Hành Trình Đăng Ký và Chọn Card Art (To-be Journey Matrix)

> **Pattern: Sub-step + Branching Matrix.** Mỗi bước cha được bẻ thành các sub-step (X.Y) trình bày dưới dạng row, với 7 cột attribute. Mỗi sub-step có Pass/Fail branching rõ ràng kèm Mã sự kiện log (`EVT_<MODULE>_<ACTION>_<RESULT>`) để Dev/Ops đối soát.

#### Bước 1: Khởi tạo & Tải thư viện Card Art

| Sub-step | PIC | Thao tác / Check | Logic Business Rule | Pass → | Fail → | Mã sự kiện log |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **1.1**<br>Load Card Art Gallery | Hệ thống | KH chọn dòng thẻ (VD: JCB, Visa Signature) và bấm `[Tiếp tục]`. DBS gọi API `/api/v1/card/art-gallery` | DBS nhận `card_product_code` + `customer_segment` từ CIF (Mass/Priority), lọc danh sách Card Art khả dụng theo dòng thẻ + segment | Trả về danh sách Card Art hợp lệ → hiển thị màn "Diện mạo thẻ của riêng bạn" → **Bước 2.1** | API timeout/lỗi → Popup `[ERR_CARD_009]` (Thử lại/Hotline) | Pass: `EVT_CARD_GALLERY_LOADED`<br>Fail: `EVT_CARD_GALLERY_API_ERROR` |

#### Bước 2: Khách hàng duyệt & chọn thiết kế Card Art

| Sub-step | PIC | Thao tác / Check | Logic Business Rule | Pass → | Fail → | Mã sự kiện log |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **2.1**<br>Duyệt thư viện + chọn mẫu | Khách hàng | Vuốt duyệt thư viện theo chủ đề (GenZ, Tối giản, VIP...), chạm chọn mẫu ưng ý | **Segment Check:** Nếu KH Mass chạm mẫu thuộc bộ "Signature & Priority" → khóa mờ + chặn chọn | KH chọn mẫu hợp lệ → render mô phỏng 3D mặt trước thẻ + tên KH in chìm góc trái → **Bước 2.2** | KH Mass chạm mẫu VIP → khóa mờ + Popup `[ERR_CARD_002]` "Bộ sưu tập đặc quyền" → quay lại 2.1 | Pass: `EVT_CARD_ART_SELECTED`<br>Fail: `EVT_CARD_SEGMENT_DENIED` |
| **2.2**<br>Lưu `card_art_id` vào draft | Hệ thống | DBS ghi nhận giá trị `card_art_id` được chọn vào draft application | Lưu vào draft đăng ký + enable nút `[Xác nhận thiết kế]` (Enable button) | → **Bước 3.1** | — | Pass: `EVT_CARD_ART_DRAFT_SAVED` |

#### Bước 3: Xác nhận thiết kế & lưu vết drop-off

| Sub-step | PIC | Thao tác / Check | Logic Business Rule | Pass → | Fail → | Mã sự kiện log |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **3.1**<br>Confirm Design | Khách hàng | KH bấm `[Xác nhận thiết kế]` để đi tiếp hành trình | Lưu vết drop-off recovery 15 ngày kèm `card_art_id` (cho phép khôi phục khi rớt luồng các bước sau) | Sang màn "Nhập địa chỉ nhận thẻ vật lý + cài đặt thẻ" → **Bước 4.1** | KH thoát giữa chừng → auto-save draft → KH quay lại trong 15 ngày → khôi phục hành trình từ Bước 3.1 với `card_art_id` đã chọn | Pass: `EVT_CARD_DESIGN_CONFIRMED`<br>Fail: `EVT_CARD_DROPOFF_SAVED` |

#### Bước 4: Hoàn tất hồ sơ & ký số phát hành thẻ

| Sub-step | PIC | Thao tác / Check | Logic Business Rule | Pass → | Fail → | Mã sự kiện log |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **4.1**<br>Batch Time Check | Hệ thống | So sánh server time với cửa sổ batch 22h00 - 01h00 trước khi nhận hồ sơ ký số | KH ký số trong batch window → chặn giao dịch (Core Banking & Core Card đang chạy đối soát/batch cuối ngày) | Ngoài cửa sổ batch → **Bước 4.2** | Trong cửa sổ batch → Popup `[ERR_CARD_010]` "Hệ thống bảo trì" + lưu draft tạm + nhắc KH retry sau 01h00 | Pass: `EVT_CARD_BATCH_OK`<br>Fail: `EVT_CARD_BATCH_BLOCKED` |
| **4.2**<br>Face Authen (QĐ 2345) | Khách hàng | Chụp ảnh khuôn mặt live (liveness detection) so khớp chip CCCD (qua NFC) hoặc dữ liệu VNeID | Tuân thủ Quyết định 2345/QĐ-NHNN, đếm số lần mismatch trong phiên | Match → **Bước 4.3** | Mismatch < 3 lần: cho retry tại chỗ.<br>Mismatch ≥ 3 lần: dừng luồng + Popup `[ERR_CARD_011]` hướng dẫn ra quầy/Hotline | Pass: `EVT_CARD_FACE_MATCH`<br>Fail: `EVT_CARD_FACE_RETRY`<br>Fail (lock): `EVT_CARD_FACE_MISMATCH_LOCK` |
| **4.3**<br>OTP Verify | Khách hàng | Nhập OTP SMS hoặc Smart OTP để ký số hợp đồng phát hành thẻ | Đếm số lần nhập sai liên tiếp + đếm số lần resend trong 1 phiên giao dịch | OTP đúng → **Bước 4.4** | Sai liên tiếp ≥ 5 lần: khóa giao dịch 24h + Popup `[ERR_CARD_012]`.<br>Resend OTP > 3 lần: auto drop-off 15 ngày | Pass: `EVT_CARD_OTP_VERIFIED`<br>Fail: `EVT_CARD_OTP_LOCKED_24H`<br>Fail: `EVT_CARD_OTP_RESEND_EXCEEDED` |
| **4.4**<br>API Core Card | Hệ thống | Gọi API `/api/v1/card/issue-online` qua DIP → Core Card phát hành PAN | Core Card đồng bộ `card_art_id` ↔ PAN mới phát hành, trả `response_code` | `response_code = 00` → **Bước 4.5** | Timeout / lỗi kết nối → Popup `[ERR_CARD_009]` (Thử lại / Hotline) | Pass: `EVT_CARD_ISSUE_SUCCESS`<br>Fail: `EVT_CARD_ISSUE_API_ERROR` |
| **4.5**<br>Activate Digital Card | Hệ thống | Kích hoạt Instant Digital Card + apply Velocity Limit 50,000,000 VND/ngày cho thẻ ảo | Hạn mức tạm cho thẻ ảo đến khi KH kích hoạt thẻ vật lý (giảm rủi ro gian lận online sau phát hành) | Hiển thị màn "Mở thẻ thành công" + In-app Noti → **Bước 5.1** | — | Pass: `EVT_CARD_DIGITAL_ACTIVATED` |

#### Bước 5: Sử dụng thẻ ảo & In thẻ vật lý

| Sub-step | PIC | Thao tác / Check | Logic Business Rule | Pass → | Fail → | Mã sự kiện log |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **5.1**<br>Xem thẻ ảo + link Apple Pay | Khách hàng | Vào màn `[Quản lý thẻ]` xem thẻ ảo, liên kết Apple Pay, sao chép thông tin thẻ | Hiển thị thẻ ảo đúng `card_art_id` đã chọn ở 2.1; cho phép xem chi tiết PAN/CVV sau khi KH xác thực OTP | KH dùng được thẻ ảo ngay lập tức → **Bước 5.2** (tự động đóng luồng đăng ký online) | — | Pass: `EVT_CARD_VIRTUAL_VIEWED` |
| **5.2**<br>Send Print Order to CPS | Hệ thống | DBS gửi lệnh in thẻ vật lý cá nhân hóa sang CPS | Đẩy `card_art_id` + `embossed_name` + `delivery_address` sang CPS; máy in nhận diện in Card Art mặt trước + Full Back-side Printing | Lệnh in được CPS tiếp nhận → chuyển phát thẻ tới KH 3-5 ngày → SMS thông báo khi giao thành công | — | Pass: `EVT_CARD_CPS_PRINT_QUEUED` |

---

### 5.2. Các Quy Tắc Nghiệp Vụ Bổ Sung (Design-time, Cross-cutting & Post-issuance Rules)

> **Lưu ý:** Các runtime check trong luồng đăng ký (Batch Time Check, Face Authen, OTP Retry Limit, Velocity Limit) đã được tích hợp trực tiếp vào Bảng Matrix 5.1 ở các sub-step tương ứng (Bước 4.1 - 4.5). Section này chỉ liệt kê các quy tắc **design-time** (quy chuẩn thiết kế cố định), **cross-cutting** (áp xuyên suốt nhiều luồng) và **post-issuance** (áp dụng sau khi đã phát hành thẻ).

1.  **Quy chuẩn in ấn vật lý (Full Back-side Printing Constraint) — Design-time Rule cho CPS:**
    *   Mặt trước (Front-side) của phôi thẻ vật lý CHỈ in duy nhất: Hình thiết kế Card Art đã chọn (toàn màn hình thẻ, không viền) và Họ tên khách hàng in chìm/nổi bằng chữ không dấu. KHÔNG in bất cứ thông tin nào khác bao gồm cả Số thẻ tín dụng (PAN), Ngày hết hạn và mã bảo mật (CVV).
    *   Mặt sau (Back-side) thẻ bắt buộc chứa đầy đủ: Dải từ, Chip thanh toán, Dải chữ ký khách hàng, và in chìm toàn bộ Số thẻ (16 chữ số), Ngày hết hạn (MM/YY), mã bảo mật CVV (3 chữ số) và Hotline hỗ trợ của ngân hàng.

2.  **Quy tắc khôi phục hành trình rớt luồng (Drop-off Recovery 15 ngày) — Cross-cutting Rule:**
    *   Khi KH thoát hành trình giữa chừng (bất kỳ Bước 1.1 → 4.5), DBS auto-save draft kèm `card_art_id` và toàn bộ dữ liệu form đã nhập.
    *   Trong vòng **15 ngày**, KH quay lại → hệ thống tự động khôi phục hành trình từ đúng bước rớt, pre-fill mọi thông tin bao gồm `card_art_id` đã chọn.
    *   Sau **15 ngày**, draft bị purge tự động, KH phải khởi tạo luồng đăng ký mới từ đầu.

3.  **Quy tắc vòng đời Card Art (Lifecycle & Deprecation Rules) — Post-issuance Rule:**
    *   Mẫu Card Art trong hệ thống DBS có 2 trạng thái: `ACTIVE` (đang hoạt động) hoặc `DEPRECATED` (ngừng hỗ trợ in vật lý do hết chiến dịch marketing).
    *   **Thẻ ảo đang hoạt động trên App:** Vẫn giữ nguyên hiển thị Card Art cũ ngay cả khi mẫu đó đã DEPRECATED, đảm bảo cam kết cá nhân hóa với KH.
    *   **Luồng phát hành lại thẻ vật lý** (Mất / Hỏng / Gia hạn): DBS kiểm tra `card_art_id` của thẻ hiện tại. Nếu DEPRECATED → Popup `[ERR_CARD_001]` yêu cầu KH chọn mẫu `ACTIVE` mới trước khi xác nhận yêu cầu phát hành lại thẻ vật lý.

4.  **Quy tắc đổi thiết kế thẻ (Change Card Art Rules) — Post-issuance Feature:**
    *   KH có thể thay đổi thiết kế ảnh hiển thị thẻ ảo trên App Digibank bất kỳ lúc nào thông qua menu `[Quản lý thẻ → Đổi thiết kế thẻ]`.
    *   **Đổi thiết kế thẻ ảo trên App:** Miễn phí, có hiệu lực ngay sau khi xác nhận.
    *   **Đổi thiết kế thẻ vật lý:** Phải đi qua luồng yêu cầu phát hành lại thẻ, áp dụng phí phát hành lại tiêu chuẩn (ví dụ: 100,000 VND trừ trực tiếp vào tài khoản thanh toán nguồn).

---

## 6. QUY CHUẨN THÔNG BÁO POPUP LỖI TRÊN GIAO DIỆN DI ĐỘNG

Đặc tả chi tiết nội dung văn bản hiển thị (UI Copy) của các trường hợp phát sinh ngoại lệ trong luồng để Dev và QA/QC thực hiện cấu hình chính xác:

### Popup lỗi [ERR_CARD_001]: Thiết kế thẻ cũ đã ngừng hỗ trợ in
*   **Tiêu đề (Title):** Thiết kế thẻ cũ đã ngừng hỗ trợ in
*   **Nội dung mô tả (Content):** Rất tiếc, mẫu thiết kế ảnh thẻ hiện tại của Quý khách đã hết hạn chương trình và không còn hỗ trợ in phôi vật lý mới. Để tiếp tục yêu cầu phát hành lại thẻ vật lý, Quý khách vui lòng bấm nút "Chọn mẫu mới" dưới đây để cập nhật diện mạo thẻ mới thời thượng hơn hoàn toàn miễn phí trên App.
*   **Nút hành động (CTA):** 
    *   `[Chọn mẫu mới]`: Điều hướng khách hàng về thư viện chọn Card Art đang khả dụng của dòng thẻ hiện tại.
    *   `[Đóng]`: Đóng popup và quay lại màn hình Quản lý thẻ hiện tại.

### Popup lỗi [ERR_CARD_002]: Mass Customer chọn mẫu VIP
*   **Tiêu đề (Title):** Bộ sưu tập thẻ đặc quyền
*   **Nội dung mô tả (Content):** Bộ sưu tập thiết kế thẻ "Signature & Priority" này chỉ áp dụng độc quyền cho hội viên MSB Priority hoặc dòng thẻ tín dụng hạng Bạch Kim. Quý khách vui lòng chọn các thiết kế thời thượng khác trong bộ sưu tập "GenZ" hoặc "Tối giản" để tiếp tục đăng ký mở thẻ.
*   **Nút hành động (CTA):** 
    *   `[Đã hiểu]`: Đóng popup và đưa người dùng quay lại màn hình thư viện ảnh thẻ để chọn mẫu khác.

### Popup lỗi [ERR_CARD_009]: Lỗi kết nối khởi tạo thẻ
*   **Tiêu đề (Title):** Đăng ký chưa hoàn tất
*   **Nội dung mô tả (Content):** Hệ thống đang gặp gián đoạn kết nối trong quá trình khởi tạo thông tin thẻ tín dụng của Quý khách. Hồ sơ của Quý khách đã được lưu tạm. Vui lòng thử lại sau ít phút hoặc liên hệ Hotline 1900xxxx để được hỗ trợ kiểm tra trạng thái phê duyệt.
*   **Nút hành động (CTA):**
    *   `[Thử lại]`: Tự động gọi lại API tạo thẻ.
    *   `[Đóng]`: Quay về trang chủ Digibank.

### Popup lỗi [ERR_CARD_010]: Hệ thống đang trong khung bảo trì batch
*   **Tiêu đề (Title):** Hệ thống đang bảo trì
*   **Nội dung mô tả (Content):** Hệ thống đang trong khung giờ bảo trì định kỳ (22h00 - 01h00 hàng ngày) để đối soát giao dịch cuối ngày của Core Banking & Core Card. Hồ sơ đăng ký mở thẻ của Quý khách đã được lưu nháp tự động. Vui lòng quay lại hoàn tất sau 01h00 hoặc đăng ký Nhận thông báo để Digibank nhắc lại khi hệ thống mở giao dịch.
*   **Nút hành động (CTA):**
    *   `[Nhận thông báo]`: Đăng ký push notification nhắc lại sau 01h00, tự đóng popup về trang chủ.
    *   `[Đóng]`: Quay về trang chủ Digibank.

### Popup lỗi [ERR_CARD_011]: Sinh trắc học khuôn mặt không khớp
*   **Tiêu đề (Title):** Xác thực khuôn mặt không thành công
*   **Nội dung mô tả (Content):** Quý khách đã thử xác thực khuôn mặt 3 lần nhưng không khớp với ảnh chân dung trên CCCD/VNeID. Vì lý do bảo mật theo Quyết định 2345/QĐ-NHNN, luồng đăng ký thẻ tín dụng online đã tạm dừng. Quý khách vui lòng đến chi nhánh MSB gần nhất hoặc liên hệ Hotline 1900xxxx để được hỗ trợ định danh và mở thẻ trực tiếp.
*   **Nút hành động (CTA):**
    *   `[Tìm chi nhánh gần nhất]`: Mở bản đồ chi nhánh MSB trong App.
    *   `[Gọi Hotline]`: Gọi 1900xxxx.

### Popup lỗi [ERR_CARD_012]: Khóa giao dịch do nhập sai OTP
*   **Tiêu đề (Title):** Giao dịch tạm khóa
*   **Nội dung mô tả (Content):** Quý khách đã nhập sai mã OTP 5 lần liên tiếp. Vì lý do bảo mật, giao dịch ký số mở thẻ tín dụng của Quý khách đã bị tạm khóa trong 24 giờ. Quý khách vui lòng quay lại sau hoặc liên hệ Hotline 1900xxxx nếu cần hỗ trợ gấp.
*   **Nút hành động (CTA):**
    *   `[Đã hiểu]`: Đóng popup, quay về trang chủ Digibank.
    *   `[Gọi Hotline]`: Gọi 1900xxxx.

---

## 7. ĐẶC TẢ TÍCH HỢP DỮ LIỆU & DATA MAPPING SCHEMAS

Đặc tả chi tiết cấu trúc truyền nhận thông tin API giữa các hệ thống FE Mobile App, BE DBS và Middleware DIP phục vụ tích hợp.

### Bảng 7.1: API Truy vấn danh sách Card Art khả dụng (Request & Response)
*   **API Endpoint:** `/api/v1/card/art-gallery`
*   **Method:** `GET`
*   **Request Params:**

| Tên Trường (Tiếng Việt) | Mã trường hệ thống | Kiểu dữ liệu | Thuộc tính (M/O) | Mô tả logic / Quy tắc nguồn |
| :--- | :--- | :--- | :---: | :--- |
| Mã dòng thẻ đăng ký | `card_product_code` | String | M | Mã dòng thẻ KH chọn mở (ví dụ: `VISA_PLATINUM`, `JCB_STANDARD`). |
| Phân khúc khách hàng | `customer_segment` | String | M | Phân khúc KH từ CIF lấy sau đăng nhập (giá trị: `MASS`, `PRIORITY`). |

*   **Response Body (JSON):**
```json
{
  "response_code": "00",
  "message": "Success",
  "categories": [
    {
      "category_name": "GenZ Năng Động",
      "designs": [
        {
          "card_art_id": "CA_GENZ_001",
          "card_art_name": "Hơi thở Vũ trụ (Cosmic Vibe)",
          "front_image_url": "https://cdn.msb.com.vn/card-art/cosmic-vibe.png",
          "status": "ACTIVE"
        },
        {
          "card_art_id": "CA_GENZ_002",
          "card_art_name": "Cyberpunk Neon",
          "front_image_url": "https://cdn.msb.com.vn/card-art/cyberpunk.png",
          "status": "ACTIVE"
        }
      ]
    },
    {
      "category_name": "Tối giản (Minimalist)",
      "designs": [
        {
          "card_art_id": "CA_MIN_001",
          "card_art_name": "Trắng Bắc Cực (Arctic White)",
          "front_image_url": "https://cdn.msb.com.vn/card-art/arctic-white.png",
          "status": "ACTIVE"
        }
      ]
    }
  ]
}
```

### Bảng 7.2: Dữ liệu API Khởi tạo/Phát hành thẻ gửi từ DBS lên DIP/Core Card (Request)
*   **API Endpoint:** `/api/v1/card/issue-online`
*   **Method:** `POST`

| Tên Trường (Tiếng Việt) | Mã trường hệ thống | Kiểu dữ liệu | Thuộc tính (M/O) | Mô tả logic / Quy tắc nguồn |
| :--- | :--- | :--- | :---: | :--- |
| Số CIF Khách hàng | `cif_number` | String | M | Mã định danh khách hàng trên hệ thống Core Banking. |
| Mã dòng thẻ đăng ký | `card_product_code` | String | M | Dòng thẻ khách hàng lựa chọn phát hành. |
| Mã thiết kế Card Art | `card_art_id` | String | M | Mã thiết kế ảnh thẻ khách hàng đã chọn tại màn hình đăng ký bước 2. |
| Họ tên in trên thẻ | `embossed_name` | String | M | Họ tên khách hàng in chìm/nổi không dấu (ví dụ: `NGUYEN VAN A`). |
| Địa chỉ nhận thẻ vật lý | `delivery_address` | String | M | Địa chỉ nhận thẻ vật lý của khách hàng đã khai báo. |

---

## 8. CẤU TRÚC THÔNG BÁO HÀNH TRÌNH ĐA KÊNH (MULTI-CHANNEL NOTI)

Để đảm bảo trải nghiệm liền mạch và giữ chân khách hàng (User Engagement), thiết lập chiến lược thông báo như sau:

### 8.1. Thông báo đẩy trong ứng dụng (In-app Push Notification - Noti Center)
*   **Trường hợp kích hoạt:** Ngay sau khi hệ thống Core phê duyệt cấp hạn mức và phát hành thẻ ảo thành công (Bước 4).
*   **Tiêu đề:** Thẻ tín dụng cá nhân hóa của bạn đã sẵn sàng!
*   **Nội dung:** Chúc mừng Quý khách! Thẻ tín dụng `{card_product_code}` với thiết kế độc bản `{card_art_name}` của Quý khách đã được kích hoạt thành công dạng ảo. Quý khách có thể bắt đầu chi tiêu và liên kết Apple Pay ngay lập tức trên ứng dụng Digibank. Thẻ vật lý đang được in ấn và sẽ gửi tới Quý khách sau 3 - 5 ngày làm việc!

### 8.2. Thông báo qua tin nhắn viễn thông SMS (Khi hoàn tất giao thẻ vật lý)
*   **Trường hợp kích hoạt:** Khi đơn vị chuyển phát bàn giao thẻ vật lý cá nhân hóa thành công tới tay khách hàng.
*   **Nội dung:** `MSB Digibank: The tin dung vat ly ca nhan hoa {card_product_code} mau thiet ke {card_art_name} da duoc giao toi Quy khach thanh cong. Quy khach vui long mo App, quet ma QR de kich hoat the vat ly va trai nghiem ngay! Hotline 1900xxxx.`

---

## 9. TIÊU CHÍ PHI CHỨC NĂNG (NON-FUNCTIONAL REQUIREMENTS)

Các chỉ số kỹ thuật hệ thống bắt buộc phải đáp ứng để đảm bảo tính ổn định, tốc độ và trải nghiệm mượt mà cho hành trình:

*   **Thời gian tải ảnh mẫu (Image SLA):** Thời gian phản hồi của API truy vấn kho ảnh `/api/v1/card/art-gallery` phải $\le 1.5$ giây. Các đường link hình ảnh (`front_image_url`) phải được lưu trữ trên CDN hiệu năng cao của ngân hàng nhằm đảm bảo tải nhanh và tối ưu hóa dung lượng hình ảnh dưới 500KB để không gây đứng/lag ứng dụng trên thiết bị di động.
*   **Độ chính xác đối soát in ấn:** Tỷ lệ đối soát khớp đúng giữa mã `card_art_id` lưu trên Core Card và mã phôi in thực tế tại máy in CPS của Trung tâm cá nhân hóa thẻ phải đạt tuyệt đối **100%**. Quy trình đối soát mã phải được kiểm tra tự động qua mã vạch (Barcode/QR code) gắn trên thẻ trung gian trước khi chuyển giao vận chuyển.
*   **Khả năng bảo mật thông tin nhạy cảm:** Mọi thông tin thẻ truyền nhận qua API từ Mobile App lên DBS và Core Card bắt buộc phải được mã hóa đầu-cuối (End-to-End Encryption) và che giấu thông tin hiển thị (Masking) số thẻ trên UI (chỉ hiển thị dạng `**** **** **** 1234`), ngoại trừ khi khách hàng xác thực OTP thành công mới được xem thông tin chi tiết đầy đủ của thẻ ảo.

---

## 10. TÀI LIỆU THAM CHIẾU

Dưới đây là các tài liệu nghiệp vụ, văn bản quy định và tài liệu kỹ thuật liên quan làm căn cứ xây dựng và tích hợp tính năng:

| STT | Tên tài liệu / Văn bản pháp lý | Đơn vị ban hành | Mã hiệu tài liệu / Chi tiết tham chiếu |
| :---: | :--- | :--- | :--- |
| 1 | **Quyết định số 2345/QĐ-NHNN** | Ngân hàng Nhà nước Việt Nam | Ban hành ngày 18/12/2023 về việc triển khai các giải pháp an toàn, bảo mật trong thanh toán trực tuyến và thanh toán thẻ ngân hàng. |
| 2 | **Tài liệu Đặc tả API Tích hợp Phát hành Thẻ trực tuyến** | Khối Công nghệ Thông tin (IT) | `IT-SPEC-CARD-API` / Phiên bản Ver 2.4 |
| 3 | **Đặc tả Thiết kế Giao diện UI/UX Personalized Card Art Flow** | Khối Sáng tạo & Trải nghiệm khách hàng | [Figma Link (UI/UX Personalized Card Art Flow)](https://figma.com/file/personalize-card-art-flow) |
| 4 | **Sơ đồ Luồng Nghiệp vụ Card Issuance To-be Flow** | Đội ngũ Phân tích nghiệp vụ (BA/PO) | [Miro Link (Miro Workspace - Card Issuance To-be Flow)](https://miro.com/app/board/card-issuance-tobe-flow) |
