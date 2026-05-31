# [TÊN DỰ ÁN / TÊN TÍNH NĂNG]
## TÀI LIỆU YÊU CẦU NGHIỆP VỤ - HÀNH TRÌNH ĐĂNG KÝ MỚI & ĐỊNH DANH KHÁCH HÀNG (ONBOARDING & EKYC JOURNEY)
**Mã tài liệu:** BRD-[CUS-XXXX] Onboarding [Tên hành trình/MVP]  
**Phiên bản:** Ver X.X  
**Ngày cập nhật:** DD/MM/YYYY  
**Trạng thái:** [Draft / Under Review / Released]

---

## 1. LỊCH SỬ THAY ĐỔI TÀI LIỆU
Bảng ghi nhận lịch sử phát triển các tính năng định danh số.

| Phiên bản | Ngày cập nhật | Người thực hiện | Người phê duyệt | Mô tả chi tiết thay đổi (A/M/D) |
| :--- | :--- | :--- | :--- | :--- |
| Ver 1.0 | DD/MM/YYYY | [Tên PO] | [Tên Approver] | Tạo mới tài liệu cho hành trình Onboarding - MVP1 |
| Ver 2.0 | DD/MM/YYYY | [Tên PO] | [Tên Approver] | Bổ sung phương thức xác thực GTTT qua liên kết ứng dụng VNeID |

---

## 2. BỘ THUẬT NGỮ CHUYÊN NGÀNH EKYC

| STT | Thuật ngữ | Ý nghĩa nghiệp vụ |
| :--- | :--- | :--- |
| 1 | **eKYC** | Electronic Know Your Customer (Định danh điện tử khách hàng trực tuyến) |
| 2 | **NFC** | Near-Field Communication (Kết nối không dây đọc thông tin lưu trong chip CCCD) |
| 3 | **VNeID** | Ứng dụng định danh điện tử của Bộ Công An liên kết để chia sẻ thông tin định danh |
| 4 | **AML/PCRT** | Hệ thống rà soát phòng chống rửa tiền và tài trợ khủng bố |
| 5 | **Drop-off** | Khách hàng rớt luồng nửa chừng, chưa hoàn thành bước cuối cùng để mở CIF |

---

## 3. TỔNG QUAN HÀNH TRÌNH ONBOARDING
*   **Mô tả US**: Là một Khách hàng mới (NTB), tôi muốn đăng ký mở tài khoản ngân hàng trực tuyến 100% trên thiết bị di động bằng căn cước công dân gắn chip để có thể sử dụng dịch vụ thanh toán tức thời mà không cần ra quầy giao dịch.
*   **Mục đích**: Tối ưu hóa tỷ lệ chuyển đổi khách hàng đăng ký mới trên kênh số, đảm bảo tuân thủ nghiêm ngặt các quy định về xác thực danh tính điện tử của NHNN và Bộ Công An.
*   **Các kênh tiếp cận (Entrypoints)**:
    1.  Tải app trực tiếp từ cửa hàng ứng dụng (App Store/Google Play) - luồng chủ động.
    2.  Quét mã QR giới thiệu / bấm Link giới thiệu (Mã giới thiệu được tự động gắn và lưu vết suốt hành trình đăng ký).

---

## 4. LUỒNG QUY TRÌNH HÀNH TRÌNH ĐĂNG KÝ (TO-BE JOURNEY)
*   **Link Miro (Quy trình nghiệp vụ tổng quan)**: [Miro Link]
*   **Link Figma (Bản vẽ UI/UX)**: [Figma Link]

### Tóm tắt các bước chính trong hành trình:
1.  **Bước 1**: Kiểm tra tính hợp lệ của thiết bị di động (Jailbreak/Root Check).
2.  **Bước 2**: Kiểm tra giờ hệ thống (Batch Time Check) & Xác minh điều kiện tham gia Whitelist.
3.  **Bước 3**: Hiển thị Điều khoản & Điều kiện (T&C) và lấy Consent đồng ý bảo vệ dữ liệu cá nhân.
4.  **Bước 4**: Nhập số điện thoại -> Xác thực mã SMS OTP (Giới hạn thử lại tối đa 3 lần/phiên).
5.  **Bước 5**: Kiểm tra rớt luồng cũ (Drop-off Recovery) trong vòng 15 ngày để phục hồi đi tiếp.
6.  **Bước 6**: Lựa chọn hình thức xác minh giấy tờ tùy thân (Quét chip NFC hoặc liên kết VNeID).
7.  **Bước 7**: Chụp ảnh khuôn mặt để đối chiếu sinh trắc học (Face Authen).
8.  **Bước 8**: Xác nhận thông tin định danh trích xuất từ chip/VNeID.
9.  **Bước 9**: Kiểm tra phòng chống rửa tiền (AML Check) và kiểm tra trùng lặp thông tin.
10. **Bước 10**: Nhập thông tin bổ sung (Nghề nghiệp, chức vụ, email, mã người giới thiệu).
11. **Bước 11**: Hệ thống mở CIF, tạo tài khoản IBMB, cấp mật khẩu tạm thời.
12. **Bước 12**: Điều hướng khách hàng đăng nhập lần đầu và kích hoạt combo sản phẩm thành công.

---

## 5. ĐẶC TẢ CHI TIẾT CÁC BƯỚC THỦ TỤC & ĐIỀU KIỆN KIỂM TRA HỆ THỐNG

Áp dụng pattern **Sub-step + Branching Matrix** (xem cẩm nang `Guides/po_writing_guide_for_ai_agents.md` Section IV). Mỗi bước cha là 1 bảng nhỏ với 7 cột chuẩn.

### 5.1. Bảng Chi Tiết Luồng Đăng Ký và Xác Thực (eKYC Journey Matrix)

#### Bước 1: Kiểm tra thiết bị & Giờ vận hành

| Sub-step | PIC | Thao tác / Check | Logic Business Rule | Pass → | Fail → | Mã sự kiện log |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **1.1**<br>Device Check | Hệ thống | KH mở app lần đầu sau khi tải về | Quét Jailbreak/Root + kiểm tra kết nối mạng ổn định | Thiết bị an toàn → **Bước 1.2** | Jailbreak/Root phát hiện → Popup `[ERR_DEVICE_01]` "Thiết bị không an toàn" → đóng app | Pass: `EVT_ONB_DEVICE_OK`<br>Fail: `EVT_ONB_DEVICE_JAILBREAK` |
| **1.2**<br>Batch Time Check | Hệ thống | KH bấm `[Đăng ký/Mở tài khoản]` | So sánh server time với cửa sổ vận hành (07h00 - 22h00 hàng ngày) | Trong giờ → **Bước 2.1** | Ngoài giờ → Popup `[ERR_BATCH_02]` "Hệ thống chạy batch" → hướng dẫn quay lại sau | Pass: `EVT_ONB_BATCH_OK`<br>Fail: `EVT_ONB_BATCH_BLOCKED` |

#### Bước 2: Điều khoản & Xác thực SĐT

| Sub-step | PIC | Thao tác / Check | Logic Business Rule | Pass → | Fail → | Mã sự kiện log |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **2.1**<br>Read T&C | Khách hàng | KH đọc và tích chọn 3 checkbox T&C | Bắt buộc tích đủ: Bảo vệ DLCN + Điều khoản TKTT + Điều khoản eBank | Tích đủ → enable `[Tiếp tục]` → **Bước 2.2** | Thiếu checkbox → disable `[Tiếp tục]` | Pass: `EVT_ONB_TNC_ACCEPTED`<br>Fail: `EVT_ONB_TNC_INCOMPLETE` |
| **2.2**<br>Phone Verify | Khách hàng | KH nhập SĐT → nhận SMS OTP → nhập mã 6 chữ số | Validate SĐT VN + OTP hiệu lực 180 giây + retry ≤ 3 lần | OTP đúng → **Bước 3.1** | Sai ≥3 lần / hết hạn resend → Popup `[ERR_OTP_03]` khóa luồng trong ngày | Pass: `EVT_ONB_OTP_VERIFIED`<br>Fail: `EVT_ONB_OTP_LOCKED_DAY` |

#### Bước 3: Kiểm tra Drop-off & Duplicate

| Sub-step | PIC | Thao tác / Check | Logic Business Rule | Pass → | Fail → | Mã sự kiện log |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **3.1**<br>Drop-off Recovery | Hệ thống | Sau khi OTP success, kiểm tra hồ sơ drop-off cũ | SĐT có hồ sơ rớt luồng ≤ 15 ngày? | Có hồ sơ cũ → màn "Chào mừng quay lại" + auto-fill → **tiếp bước dở dang** | Không có / >15 ngày → khởi tạo mới → **Bước 4.1** | Pass: `EVT_ONB_DROPOFF_RECOVERED` / `EVT_ONB_DROPOFF_NONE` |

#### Bước 4: Xác minh giấy tờ tùy thân (eKYC)

| Sub-step | PIC | Thao tác / Check | Logic Business Rule | Pass → | Fail → | Mã sự kiện log |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **4.1**<br>Choose Method | Khách hàng | KH chọn 1 trong 2: (1) Quét chip NFC CCCD, (2) Liên kết VNeID | Mặc định đề xuất NFC nếu thiết bị hỗ trợ | KH chọn NFC → **Bước 4.2a**.<br>KH chọn VNeID → **Bước 4.2b** | — | Pass: `EVT_ONB_KYC_METHOD_NFC` / `EVT_ONB_KYC_METHOD_VNEID` |
| **4.2a**<br>NFC Scan | Khách hàng | KH áp CCCD vào lưng máy để đọc chip | Thiết bị bật NFC + đọc dữ liệu chip CCCD | Đọc thành công → **Bước 4.3** | Thất bại 3 lần liên tiếp → Popup `[ERR_NFC_04]` tự động chuyển sang VNeID (**Bước 4.2b**) | Pass: `EVT_ONB_NFC_READ_OK`<br>Fail: `EVT_ONB_NFC_FAILED_FALLBACK` |
| **4.2b**<br>VNeID Link | Khách hàng | Hệ thống mở deeplink sang app VNeID lấy Consent | OAuth2 với Bộ Công An | Consent OK → nhận thông tin định danh → **Bước 4.3** | KH từ chối / OAuth timeout → Popup `[ERR_VNEID_05]` | Pass: `EVT_ONB_VNEID_CONSENT_OK`<br>Fail: `EVT_ONB_VNEID_CONSENT_DENIED` |
| **4.3**<br>Face Authen | Khách hàng | Chụp ảnh khuôn mặt live so khớp ảnh chân dung chip/VNeID | Liveness detection + tuân thủ QĐ 2345/QĐ-NHNN | Match ≥ ngưỡng AI → **Bước 5.1** | Mismatch ≥3 lần → Popup `[ERR_FACE_06]` hướng dẫn ra quầy | Pass: `EVT_ONB_FACE_MATCH`<br>Fail: `EVT_ONB_FACE_MISMATCH_LOCK` |

#### Bước 5: AML Check & Mở CIF

| Sub-step | PIC | Thao tác / Check | Logic Business Rule | Pass → | Fail → | Mã sự kiện log |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **5.1**<br>AML Screening | Hệ thống | Đối soát CCCD với danh sách AML/PCRT | Blacklist + PEP + Sanctions list | Sạch → **Bước 5.2** | Hit AML → Popup `[ERR_AML_07]` + escalate Khối QTRR + đóng luồng | Pass: `EVT_ONB_AML_CLEAN`<br>Fail: `EVT_ONB_AML_HIT_ESCALATE` |
| **5.2**<br>Open CIF | Hệ thống | Tạo CIF mới + cấp tài khoản IBMB + mật khẩu tạm | Đồng bộ qua DIP → Core Banking → DBS | CIF mở thành công → **Bước 5.3** | API timeout: Popup `[ERR_SYS_999]` (Thử lại / Hotline) | Pass: `EVT_ONB_CIF_CREATED`<br>Fail: `EVT_ONB_CIF_API_ERROR` |
| **5.3**<br>First Login | Khách hàng | KH đăng nhập lần đầu bằng mật khẩu tạm, đổi mật khẩu mới | Validate password policy (8 ký tự, có số + ký tự đặc biệt) | Đăng nhập thành công → kết thúc luồng eKYC → kích hoạt combo sản phẩm | Sai mật khẩu/policy → cho retry | Pass: `EVT_ONB_FIRST_LOGIN_OK`<br>Fail: `EVT_ONB_PWD_POLICY_FAILED` |

---

### 5.2. Logic Kiểm tra Trùng lặp & Phân loại Khách hàng (Duplicate Check Rules)

> **Cross-cutting Rule:** Được kiểm tra ngầm tại Bước 4.3 (sau khi có CCCD từ NFC/VNeID). Routing 4 trường hợp dưới đây.

Sau khi thu thập số CCCD của khách hàng từ NFC hoặc VNeID, hệ thống bắt buộc phải kiểm tra thông tin đối soát trên toàn ngân hàng:

| Trường hợp | Đã tồn tại App Mới | Đã tồn tại App Cũ | Đã có CIF tại Core | Hành động hệ thống mong muốn (Next Action) & Copywriting tương ứng |
| :---: | :---: | :---: | :---: | :--- |
| **TH1** | Có | - | - | **Thông báo:** CCCD/Số điện thoại này đã đăng ký Digibank. <br>**CTA:** `[Đăng nhập]` -> Điều hướng về màn hình đăng nhập. |
| **TH2** | Không | Có | - | **Thông báo:** Khách hàng hiện hữu trên kênh cũ. <br>**CTA:** `[Kích hoạt dịch vụ]` -> Điều hướng người dùng sang luồng kích hoạt tài khoản trên App mới (ETB Flow). |
| **TH3** | Không | Không | Có | **Thông báo:** Khách hàng đã có thông tin tại quầy giao dịch nhưng chưa đăng ký eBank. <br>**CTA:** `[Đăng ký eBank]` -> Chuyển hướng sang luồng đăng ký tài khoản eBank dành cho khách hàng hiện hữu (ETB Landing Flow). |
| **TH4** | Không | Không | Không | **Khách hàng mới hoàn toàn (NTB)** -> Cho phép đi tiếp hành trình Onboarding. |

---

## 6. QUY CHUẨN THÔNG BÁO POPUP LỖI ONBOARDING & EKYC

Dưới đây là đặc tả các popup lỗi nghiêm trọng ảnh hưởng tới tỷ lệ chuyển đổi định danh trực tuyến.

### Popup lỗi ERR_DEVICE_01: Thiết bị không an toàn
*   **Tiêu đề (Title)**: Thiết bị không đạt điều kiện bảo mật
*   **Nội dung mô tả (Content)**: Vì lý do an toàn thông tin tài khoản, ứng dụng Digibank không hỗ trợ hoạt động trên các thiết bị đã bị bẻ khóa (Jailbreak hoặc Root). Quý khách vui lòng kiểm tra lại cài đặt thiết bị hoặc sử dụng thiết bị khác để thực hiện dịch vụ.
*   **Nút hành động (CTA)**: `[Đóng ứng dụng]` -> Đóng app ngay lập tức.

### Popup lỗi ERR_BATCH_02: Hệ thống chạy batch
*   **Tiêu đề (Title)**: Hệ thống ngoài giờ làm việc
*   **Nội dung mô tả (Content)**: Dịch vụ đăng ký mở tài khoản trực tuyến của Digibank hiện đang tạm ngưng để xử lý đối soát cuối ngày. Quý khách vui lòng thực hiện đăng ký dịch vụ trong khung giờ từ 07:00 đến 22:00 hàng ngày. Xin chân thành cảm ơn!
*   **Nút hành động (CTA)**: `[Đóng]` -> Đóng thông báo và tắt app.

---

## 7. TIÊU CHÍ PHI CHỨC NĂNG CHO HỆ THỐNG ONBOARDING
*   **Tốc độ phản hồi chụp ảnh sinh trắc học**: Thời gian so khớp khuôn mặt giữa ảnh Live và ảnh trong chip CCCD (Face Authen) $\le 3$ giây.
*   **Tỷ lệ thành công OCR & đọc chip NFC**: Đạt tối thiểu $95\%$ trong điều kiện ánh sáng thông thường.
*   **Độ ổn định đường truyền liên kết VNeID**: API chuyển hướng và lấy Consent với Bộ Công An qua luồng Oauth2 phải duy trì tỷ lệ hoạt động (Uptime) $\ge 99.9\%$.
