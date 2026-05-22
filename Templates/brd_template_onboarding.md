# [TÊN DỰ ÁN / TÊN TÍNH NĂNG]
## TÀI LIỆU YÊU CẦU NGHIỆP VỤ - HÀNH TRÌNH ĐĂNG KÝ MỚI & ĐỊNH DANH KHÁCH HÀNG (ONBOARDING & EKYC JOURNEY)
**Mã tài liệu:** DCTBR-[CUS-XXXX] BRD Onboarding [Tên hành trình/MVP]  
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

### 5.1. Bảng Chi Tiết Luồng Đăng Ký và Xác Thực
```markdown
| STT | Tên Bước | PIC | Thao tác & Mô tả chi tiết hành vi | Điều kiện & Logic kiểm tra hệ thống (Business Rules) | Kết quả & Chuyển bước tiếp theo |
| :---: | :--- | :--- | :--- | :--- | :--- |
| **1** | Kiểm tra thiết bị | Hệ thống | Khách hàng mở ứng dụng lần đầu tiên sau khi tải về. | - Hệ thống quét thiết bị xem có dấu hiệu bị bẻ khóa (Jailbreak/Root). <br>- Thiết bị của khách hàng có kết nối mạng ổn định. | - Nếu an toàn: Hiển thị màn hình Welcome -> Chuyển sang **Bước 2**. <br>- Nếu phát hiện Jailbreak/Root: Khóa ứng dụng, hiển thị Popup báo lỗi bảo mật [ERR_DEVICE_01] -> Đóng app. |
| **2** | Kiểm tra giờ batch | Hệ thống | Khách hàng bấm nút `[Đăng ký/Mở tài khoản]` | - Hệ thống kiểm tra thời gian hiện tại có nằm ngoài khung giờ làm việc của dịch vụ trực tuyến không (quy định từ 7h - 22h hàng ngày). | - Nếu trong giờ hoạt động: Chuyển sang **Bước 3**. <br>- Nếu ngoài giờ hoạt động: Hiển thị Popup thông báo hệ thống ngoài giờ làm việc [ERR_BATCH_02] -> Đóng thông báo. |
| **3** | Đọc Điều khoản (T&C) | Khách hàng | Khách hàng đọc và tích chọn đồng ý các điều khoản sử dụng. | - Khách hàng bắt buộc phải tích chọn đầy đủ 3 checkbox đồng ý T&C (Bảo vệ dữ liệu cá nhân, Điều khoản tài khoản thanh toán, Điều khoản ngân hàng điện tử). | - Nếu tích đủ: Mở khóa (Enable) nút `[Tiếp tục]` -> Chuyển sang **Bước 4**. <br>- Nếu thiếu bất kỳ checkbox nào: Khóa (Disable) nút `[Tiếp tục]`. |
| **4** | Xác thực SĐT & OTP | Khách hàng / Hệ thống | Khách hàng nhập số điện thoại -> Nhận SMS OTP -> Nhập mã OTP gồm 6 chữ số. | - Số điện thoại phải đúng định dạng mạng viễn thông Việt Nam. <br>- Kiểm tra số lần nhập sai OTP liên tiếp $\le 3$ lần. <br>- Kiểm tra hiệu lực mã OTP (trong vòng 180 giây). | - Nếu đúng OTP: Chuyển sang **Bước 5** (Kiểm tra trùng lặp và drop-off). <br>- Nếu nhập sai $\ge 3$ lần hoặc hết hạn gửi lại: Khóa luồng trong ngày [ERR_OTP_03] -> Trở lại màn hình nhập SĐT. |
| **5** | Kiểm tra Drop-off | Hệ thống | Sau khi xác thực OTP thành công. | - Hệ thống kiểm tra xem Số điện thoại này đã từng bắt đầu luồng đăng ký nhưng bị rớt (Drop-off) trong vòng 15 ngày gần nhất không. | - Nếu có hồ sơ drop-off $\le 15$ ngày: Hiển thị màn hình chào mừng trở lại -> Cho phép Khách hàng đi tiếp các bước dở dang với thông tin tự động điền sẵn (Auto-fill). <br>- Nếu không có hoặc $> 15$ ngày: Chuyển sang **Bước 6** (Đăng ký mới từ đầu). |
| **6** | Xác minh GTTT | Khách hàng | Khách hàng chọn 1 trong 2 phương thức xác thực: <br>1. Quét chip NFC giấy tờ (Mặc định). <br>2. Liên kết qua ứng dụng VNeID. | **Trường hợp NFC**: <br>- Kiểm tra thiết bị có hỗ trợ tính năng NFC và đang bật NFC không. <br>- Đọc dữ liệu từ chip thẻ CCCD gắn chip. <br>**Trường hợp VNeID**: <br>- Gọi liên kết sang cổng ứng dụng VNeID để lấy Consent chia sẻ thông tin định danh cá nhân của Bộ Công An. | - Nếu thành công: Chuyển sang **Bước 7** (Xác thực khuôn mặt). <br>- Nếu quét NFC thất bại 3 lần liên tiếp: Tự động điều hướng Khách hàng sang phương thức Liên kết ứng dụng VNeID [ERR_NFC_04]. |
```

### 5.2. Logic Kiểm tra Trùng lặp & Phân loại Khách hàng (Duplicate Check Rules)
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
