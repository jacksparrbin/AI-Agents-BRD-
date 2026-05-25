# HÀNH TRÌNH QUÉT MÃ QR CHUYỂN KHOẢN THEO TIÊU CHUẨN VIETQR TRÊN MOBILE APP (VIETQR SCAN & TRANSFER)
## TÀI LIỆU YÊU CẦU NGHIỆP VỤ (BUSINESS REQUIREMENT DOCUMENT - BRD)
**Mã tài liệu:** DCTBR-[PAY-008] BRD VietQR Scan & Transfer  
**Phân hệ:** Daily Banking - Payment  
**Phiên bản:** Ver 2.0  
**Ngày cập nhật:** 25/05/2026  
**Trạng thái:** Released

---

## 1. LẠCH SỬ THAY ĐỔI TÀI LIỆU

Bảng ghi nhận toàn bộ quá trình cập nhật, chỉnh sửa nội dung tài liệu qua các phiên bản phát triển (MVP).

| Phiên bản | Ngày cập nhật | Người thực hiện | Người phê duyệt | Mô tả chi tiết thay đổi (A/M/D) |
| :--- | :--- | :--- | :--- | :--- |
| Ver 1.0 | 22/05/2026 | AI PO Agent | Stakeholder Approved | **[NEW]** Khởi tạo tài liệu đặc tả hoàn chỉnh cho tính năng quét mã QR chuyển khoản liên ngân hàng 24/7 theo tiêu chuẩn VietQR (NAPAS) trên ứng dụng di động MSB Digibank. |
| Ver 2.0 | 25/05/2026 | Antigravity AI PO Agent | Stakeholder Approved | **[MODIFIED]** Cập nhật 10 thay đổi nghiệp vụ quan trọng đã được thống nhất qua luồng phỏng vấn Stakeholder, bao gồm: tích hợp kiểm tra Root/Jailbreak thiết bị; giải mã VietQR trực tiếp tại Front-end; cơ chế khóa cứng số tiền QR động & tự động xóa dấu Tiếng Việt nội dung chuyển khoản; bổ sung Tài khoản thấu chi làm tài khoản nguồn trích nợ; chốt chặn Face Authen theo QĐ 2345/QĐ-NHNN; nút gợi ý bật Đèn Flash khi tối < 10 lux; nút "Lưu danh bạ" thông minh chỉ hiện khi số tài khoản thụ hưởng chưa lưu; cơ chế tự động hoàn tiền nguồn tức thì (Reversal) khi NAPAS quá hạn phản hồi (Timeout 30s); và cơ chế chặn ảnh Gallery chứa nhiều mã QR. |

---

## 2. BỘ THUẬT NGỮ NGHIỆP VỤ & GIẢI THÍCH TỪ NGỮ

| STT | Thuật ngữ / Từ viết tắt | Giải thích ý nghĩa nghiệp vụ |
| :--- | :--- | :--- |
| 1 | **VietQR** | Tiêu chuẩn mã QR chung cho các dịch vụ thanh toán và chuyển khoản tại Việt Nam do NAPAS ban hành, xây dựng trên tiêu chuẩn quốc tế EMVCo. |
| 2 | **NAPAS** | National Payment Corporation of Vietnam (Công ty Cổ phần Thanh toán Quốc gia Việt Nam) - Đơn vị chuyển mạch tài chính quốc gia. |
| 3 | **EMVCo** | Tổ chức tiêu chuẩn hóa quốc tế về công nghệ thẻ thanh toán và thanh toán QR di động (đồng sáng lập bởi Europay, Mastercard, Visa, v.v.). |
| 4 | **CIF** | Customer Information File (Mã hồ sơ thông tin khách hàng duy nhất trên Core Banking MSB). |
| 5 | **TKTT** | Tài khoản thanh toán nguồn trích nợ tiền chuyển khoản của khách hàng tại MSB. |
| 6 | **Overdraft (Tài khoản thấu chi)** | Tài khoản được ngân hàng cấp một hạn mức chi vượt số tiền thực có trên tài khoản thanh toán để chi tiêu, chuyển khoản. |
| 7 | **DBS** | Digibank Service (Hệ thống quản trị và xử lý nghiệp vụ kênh ngân hàng số MSB). |
| 8 | **DIP** | Digital Integration Platform (Cổng middleware trung gian điều phối và tích hợp API giữa Mobile App, DBS và hệ thống Core Banking/NAPAS). |
| 9 | **Face Authen (FA)** | Xác thực sinh trắc học khuôn mặt live so khớp trực tiếp với dữ liệu lưu trữ đã được xác thực eKYC/NFC của Bộ Công An theo Quyết định 2345/QĐ-NHNN. |
| 10 | **CRC-16** | Cyclic Redundancy Check 16-bit. Thuật toán kiểm tra tính toàn vẹn của chuỗi dữ liệu mã QR nhằm phát hiện sai số hoặc giả mạo. |
| 11 | **Static QR** | Mã QR tĩnh: chứa thông tin ngân hàng và số tài khoản thụ hưởng, KH phải tự nhập số tiền chuyển. |
| 12 | **Dynamic QR** | Mã QR động: chứa đầy đủ thông tin ngân hàng, số tài khoản thụ hưởng, số tiền chuyển khoản và nội dung chuyển khoản được cố định sẵn. |
| 13 | **HMGDT** | Hạn mức giao dịch ngày (Tổng số tiền tối đa được chuyển khoản trong một ngày của gói dịch vụ khách hàng). |
| 14 | **Beneficiary Directory (Danh bạ thụ hưởng)** | Danh sách các tài khoản thụ hưởng đã được lưu trữ của khách hàng trên hệ thống nhằm phục vụ chuyển tiền nhanh không cần nhập lại. |

---

## 3. TỔNG QUAN HÀNH TRÌNH GIAO DỊCH

### 3.1. Mô tả User Story (US)
*   **As a** Khách hàng hiện hữu của MSB (ETB) đã kích hoạt ứng dụng di động MSB Digibank,
*   **I want to** Sử dụng tính năng quét mã QR để quét các mã VietQR chuẩn của mọi ngân hàng thành viên NAPAS tại Việt Nam (bằng camera hoặc ảnh thư viện),
*   **So that** Hệ thống tự động nhận diện thông tin thụ hưởng và tự động điền các thông tin này để chuyển khoản liên ngân hàng 24/7 cực nhanh, chính xác 100% không sợ chuyển nhầm tài khoản.

### 3.2. Mục đích & Yêu cầu Kinh doanh (Business Objectives)
*   Tối ưu hóa thời gian thực hiện một giao dịch chuyển khoản liên ngân hàng trên App MSB Digibank xuống $\le 15$ giây (giảm hơn 60% thời gian thực hiện so với nhập tay).
*   Giảm thiểu hoàn toàn tỷ lệ rủi ro khách hàng chuyển nhầm tiền do nhập sai số tài khoản hoặc chọn sai ngân hàng thụ hưởng.
*   Nâng cao trải nghiệm khách hàng, gia tăng lượng giao dịch thanh toán không dùng tiền mặt (chuyển khoản qua QR tại các điểm bán lẻ).
*   Tuân thủ nghiêm ngặt Quyết định 2345/QĐ-NHNN về xác thực sinh trắc học bắt buộc trên kênh số đối với các ngưỡng hạn mức lớn.

### 3.3. Phạm vi Yêu cầu (Scope)
*   **In-Scope (Nằm trong phạm vi)**:
    *   Tính năng quét mã VietQR bằng Camera tích hợp trên App hoặc chọn ảnh mã QR lưu sẵn từ thư viện ảnh thiết bị (Gallery).
    *   Cơ chế chặn ảnh từ Gallery chứa nhiều mã QR hoặc không nhận dạng được mã QR nào (báo lỗi `[ERR_PAY_002]`).
    *   Hỗ trợ giải mã cả **Mã QR Tĩnh (Static QR)** và **Mã QR Động (Dynamic QR)** theo đúng tiêu chuẩn kỹ thuật VietQR của NAPAS.
    *   Cơ chế giải mã dữ liệu EMVCo và kiểm tra tính toàn vẹn **CRC-16** trực tiếp tại Front-end (Mobile App).
    *   Xử lý logic tự động điền thông tin ngân hàng thụ hưởng (qua mapping mã BIN NAPAS), số tài khoản thụ hưởng, số tiền chuyển khoản và nội dung chuyển khoản.
    *   Hỗ trợ trích nợ từ **Tài khoản thanh toán (TKTT)** và **Tài khoản thấu chi (Overdraft)** hoạt động bằng VND. Mặc định chọn tài khoản có số dư khả dụng lớn nhất; cung cấp danh sách dropdown để chuyển đổi.
    *   Tự động khóa cứng số tiền không cho phép chỉnh sửa đối với mã QR Động nhằm bảo vệ tính toàn vẹn của hóa đơn/đơn hàng.
    *   Tự động lọc bỏ hoàn toàn các ký tự Tiếng Việt có dấu của nội dung chuyển tiền (Tag 62.08 hoặc KH nhập tay) trước khi gửi qua API.
    *   Xử lý luồng chốt chặn xác thực sinh trắc học Face Authen đối với giao dịch vượt ngưỡng hạn mức quyết định 2345/QĐ-NHNN.
    *   Cơ chế đối soát và xử lý Timeout 30 giây: Tự động hoàn tiền (Reversal) nguồn tức thì nếu NAPAS quá hạn phản hồi.
    *   Tích hợp nút **"Lưu danh bạ"** thông minh tại màn hình Giao dịch Thành công **chỉ khi số tài khoản thụ hưởng này chưa tồn tại trong Danh bạ thụ hưởng** của khách hàng.
    *   Tích hợp nút bật/tắt Đèn Flash thủ công và **tự động gợi ý bật Flash** nếu cảm biến ánh sáng thiết bị nhận diện môi trường quá tối ($< 10$ lux).
*   **Out-of-Scope (Nằm ngoài phạm vi)**:
    *   Quét các loại mã QR không thuộc tiêu chuẩn VietQR (như QR thanh toán thẻ quốc tế EMVCo Merchant-Presented, QR ví điện tử nội bộ không liên kết NAPAS).
    *   Tính năng tự tạo mã VietQR cá nhân (sẽ được đặc tả ở tài liệu BRD khác).

### 3.4. Đối tượng Hưởng lợi & Đối tượng Sử dụng trực tiếp
*   **Đối tượng hưởng lợi**: Khách hàng cá nhân hiện hữu của MSB, khối Khách hàng Bán lẻ (Retail Banking), bộ phận Kiểm soát Rủi ro Giao dịch.
*   **Đối tượng sử dụng trực tiếp**: Khách hàng sử dụng ứng dụng di động MSB Digibank trên thiết bị iOS/Android.

---

## 4. SƠ ĐỒ LUỒNG QUY TRÌNH NGHIỆP VỤ (MERMAID SEQUENCE FLOW)

Dưới đây là sơ đồ chi tiết luồng dữ liệu To-be thể hiện việc giải mã VietQR tại Front-end, truy vấn tài khoản thụ hưởng tại NAPAS, kiểm tra chốt chặn bảo mật (Jailbreak, Face Authen QĐ 2345) và hoàn tất hạch toán chuyển khoản liên ngân hàng kèm cơ chế Reversal & Lưu danh bạ thông minh:

```mermaid
sequenceDiagram
    autonumber
    actor KH as Khách hàng
    participant APP as Mobile App (FE)
    participant DBS as Hệ thống DBS (BE)
    participant DIP as Cổng Tích hợp DIP
    participant NAPAS as Hệ thống NAPAS
    participant CORE as Core Banking MSB
    participant FA as Server Face Authen

    KH->>APP: Đăng nhập app & Chọn "Quét QR chuyển khoản"
    
    rect rgb(240, 248, 255)
        Note over APP: Giai đoạn kiểm tra thiết bị & Cấp quyền
        APP->>APP: Chạy ngầm kiểm tra thiết bị Root/Jailbreak
        alt Thiết bị đã Root/Jailbreak
            APP-->>KH: Hiển thị popup lỗi [ERR_PAY_001] & Chặn truy cập
        else Thiết bị An toàn
            APP->>KH: Kích hoạt camera quét mã / Hiển thị nút chọn ảnh từ Gallery
        end
    end

    alt Quét trực tiếp bằng Camera
        KH->>APP: Di chuyển camera quét mã VietQR (Tĩnh hoặc Động)
        alt Cảm biến ánh sáng < 10 Lux
            APP-->>KH: Hiển thị nút Flash gợi ý nhấp nháy bật đèn hỗ trợ quét
        end
    else Chọn ảnh từ Gallery
        KH->>APP: Nhấn chọn ảnh từ thư viện thiết bị
        alt Ảnh chứa nhiều mã QR hoặc không nhận dạng được mã nào
            APP-->>KH: Hiển thị popup lỗi [ERR_PAY_002] & Chặn luồng
        end
    end
    
    rect rgb(240, 248, 255)
        Note over APP: Giai đoạn giải mã QR trực tiếp tại FE (EMVCo & CRC-16 Check)
        APP->>APP: Kiểm tra tính toàn vẹn dữ liệu (CRC-16 validation)
        alt CRC-16 không khớp hoặc sai cấu trúc EMVCo
            APP-->>KH: Hiển thị Popup lỗi [ERR_PAY_003] & Chấm dứt luồng
        end
        APP->>APP: Giải mã các Tag dữ liệu kỹ thuật VietQR (BIN, Số TK, Số tiền...)
    end

    APP->>DBS: Gửi yêu cầu truy vấn thông tin (Mã BIN Napas, Số TK thụ hưởng)
    DBS->>DIP: Chuyển tiếp yêu cầu tra soát tài khoản thụ hưởng
    DIP->>NAPAS: Gọi API Tra cứu Tài khoản Thụ hưởng (Napas Inquiry Service)
    
    alt NAPAS trả lỗi (Tài khoản không tồn tại / Ngân hàng lỗi)
        NAPAS-->>DBS: Phản hồi lỗi tra cứu tài khoản thụ hưởng
        DBS-->>APP: Trả mã lỗi tra cứu tài khoản [ERR_PAY_004]
        APP-->>KH: Hiển thị popup "Tài khoản thụ hưởng không tồn tại/Hệ thống lỗi"
    else NAPAS thành công
        NAPAS-->>DIP: Trả về Tên chủ tài khoản thụ hưởng (e.g. NGUYEN VAN A)
        DIP-->>DBS: Phản hồi dữ liệu tra cứu thành công
        DBS-->>APP: Trả thông tin tên chủ tài khoản & mã ngân hàng thụ hưởng
    end

    Note over APP: Hiển thị màn hình nhập thông tin chuyển khoản
    APP->>APP: Tải thông tin tài khoản nguồn (TKTT & Overdraft VND)<br/>Mặc định hiển thị TK có số dư khả dụng lớn nhất
    
    alt QR Động (Dynamic QR - Tag 01 = '12')
        APP-->>APP: Autofill Số tiền từ QR & Khóa cứng ô số tiền (Read-only)
    else QR Tĩnh (Static QR - Tag 01 = '11')
        APP-->>APP: Mở khóa ô số tiền cho Khách hàng chủ động nhập
    end
    
    APP-->>APP: Autofill Nội dung chuyển khoản (Tag 62.08)
    APP->>APP: Tự động lọc bỏ các ký tự Tiếng Việt có dấu trong ô Nội dung

    KH->>APP: Kiểm tra Số tiền/Nội dung, Chọn TK nguồn khác (nếu có) & Bấm [Tiếp tục]
    APP->>DBS: Yêu cầu kiểm tra Hạn mức (Velocity) & Số dư tài khoản trích nợ
    
    rect rgb(255, 240, 245)
        Note over APP, DBS: Kiểm tra chốt chặn Sinh trắc học theo QĐ 2345/QĐ-NHNN
        alt Số tiền GD > 10,000,000 VND HOẶC Tổng GD tích lũy trong ngày vượt 20,000,000 VND
            APP->>KH: Yêu cầu quét khuôn mặt xác thực (Face Authen)
            KH->>APP: Xác thực khuôn mặt live trực tiếp trên App
            APP->>FA: Gửi dữ liệu so khớp 3D với ảnh CCCD gắn chip
            alt Xác thực thất bại / Không chính chủ
                FA-->>APP: Phản hồi lỗi xác thực sinh trắc học
                APP-->>KH: Báo lỗi sinh trắc học [ERR_PAY_007] & Chặn giao dịch
            end
        end
    end

    APP->>KH: Yêu cầu ký số bằng Smart OTP
    KH->>APP: Nhập mã PIN Smart OTP thành công
    APP->>DBS: Gửi Lệnh Chuyển tiền kèm Chữ ký số Smart OTP hợp lệ
    
    DBS->>DIP: Yêu cầu thực thi hạch toán và chuyển mạch
    DIP->>CORE: Trích nợ (Debit) tài khoản nguồn (TKTT / Overdraft) của Khách hàng
    
    alt Số dư không đủ hoặc tài khoản bị khóa
        CORE-->>DIP: Phản hồi giao dịch trích nợ thất bại
        DIP-->>APP: Trả mã lỗi số dư/tài khoản [ERR_PAY_005/006]
        APP-->>KH: Hiển thị thông báo tài khoản nguồn không đủ điều kiện
    else Core Trích nợ thành công
        CORE-->>DIP: Trích nợ thành công
        DIP->>NAPAS: Gọi API Chuyển tiền nhanh 24/7 (Napas Credit Service)
        
        alt Chuyển mạch NAPAS thành công
            NAPAS-->>DIP: Phản hồi Giao dịch thành công (Mã trả về: 00)
            DIP-->>DBS: Phản hồi ghi nhận thành công
            DBS->>DBS: Kiểm tra số tài khoản thụ hưởng trong danh bạ của KH
            alt Số tài khoản thụ hưởng CHƯA tồn tại trong danh bạ
                DBS-->>APP: Trả trạng thái thành công + Flag [is_new_beneficiary = true]
                APP-->>KH: Hiển thị màn hình Chuyển khoản thành công kèm NÚT [Lưu danh bạ]
            else Số tài khoản thụ hưởng ĐÃ tồn tại trong danh bạ
                DBS-->>APP: Trả trạng thái thành công + Flag [is_new_beneficiary = false]
                APP-->>KH: Hiển thị màn hình Chuyển khoản thành công (ẨN NÚT [Lưu danh bạ])
            end
            DBS->>APP: Đẩy thông báo Push Noti biến động số dư tài khoản
        else NAPAS quá tải / Nghi vấn treo kết nối (Timeout 30s)
            NAPAS-->>DIP: Không phản hồi trạng thái giao dịch quá 30 giây
            DIP->>CORE: Gửi lệnh Hoàn trả (Reversal) tiền vào tài khoản nguồn ngay lập tức
            CORE-->>DIP: Hạch toán hoàn tiền nguồn thành công
            DIP-->>DBS: Trả trạng thái giao dịch lỗi nghi vấn và đã hoàn trả tiền nguồn
            DBS-->>APP: Hiển thị Popup lỗi [ERR_PAY_008] (Giao dịch gián đoạn - Đã hoàn tiền)
        end
    end

    alt KH nhấn nút [Lưu danh bạ] (Chỉ hiển thị khi số TK chưa có trong danh bạ)
        KH->>APP: Nhấn nút [Lưu danh bạ] & Nhập Nickname gợi nhớ
        APP->>DBS: Gửi yêu cầu lưu thông tin người nhận vào danh bạ
        DBS-->>APP: Xác nhận lưu danh bạ thành công
        APP-->>KH: Hiển thị Toast thông báo "Lưu danh bạ thành công"
    end
```

---

## 5. ĐẶC TẢ CHI TIẾT LUỒNG HÀNH TRÌNH & BUSINESS RULES

### 5.1. Mô tả Quy tắc Kỹ thuật Giải mã VietQR tại Front-End (VietQR Technical Spec)
Khi camera quét được hoặc chọn ảnh từ thư viện, hệ thống Front-end phải thực hiện bộ thư viện giải mã chuỗi ký tự theo cấu trúc EMVCo dạng: `[Tag 2 ký tự][Length 2 ký tự][Value chiều dài tương ứng]`.

Bảng sau mô tả các Tag bắt buộc phải phân tách và kiểm tra logic nghiệp vụ:

| Tag EMVCo | Độ dài tối đa | Ý nghĩa nghiệp vụ & Quy tắc giải mã dữ liệu | Điều kiện kiểm tra kỹ thuật & Chốt chặn |
| :---: | :---: | :--- | :--- |
| **00** | 02 | **Payload Format Indicator**: Phiên bản cấu trúc dữ liệu. | Giá trị bắt buộc phải là **`01`**. Nếu khác, báo lỗi không đúng cấu trúc VietQR. |
| **01** | 02 | **Point of Initiation Method**: Phân loại mã QR tĩnh/động. | - Giá trị **`11`**: Static QR (Mã QR tĩnh). <br>- Giá trị **`12`**: Dynamic QR (Mã QR động). <br>Hệ thống dùng giá trị này để điều khiển UI (Mở/Khóa ô số tiền). |
| **38** | 99 | **Merchant Account Information**: Thông tin tài khoản thụ hưởng. Chứa các sub-tags lồng bên trong. | Bắt buộc phải có. Nếu không phân tích được, báo lỗi không đọc được mã. |
| **38.00** | 10 | **Global Unique Identifier (GUID)**: Định danh tổ chức chuyển mạch. | Giá trị bắt buộc phải là **`A000000727`** (Mã định danh NAPAS). Nếu khác, báo lỗi QR không thuộc liên minh NAPAS. |
| **38.01** | 06 | **Acquiring Bank BIN / Merchant ID**: Mã BIN ngân hàng nhận. | Bắt buộc 6 chữ số (Ví dụ: `970426` cho MSB). Hệ thống dùng mã này để truy tìm Tên Ngân hàng thụ hưởng trong thư viện. |
| **38.02** | 19 | **Beneficiary Account Number**: Số tài khoản thụ hưởng nhận tiền. | Bắt buộc là chuỗi số, độ dài tối đa 19 ký tự. |
| **53** | 03 | **Transaction Currency**: Mã tiền tệ giao dịch. | Giá trị bắt buộc phải là **`704`** (Việt Nam Đồng). Nếu khác, báo lỗi không hỗ trợ chuyển ngoại tệ trực tuyến. |
| **54** | 13 | **Transaction Amount**: Số tiền giao dịch cần chuyển. | - Với QR tĩnh (`01` = `11`): Không có Tag này. <br>- Với QR động (`01` = `12`): Bắt buộc phải có Tag này, giá trị số thực dương $> 0$. |
| **58** | 02 | **Country Code**: Quốc gia thực hiện giao dịch. | Giá trị bắt buộc phải là **`VN`**. Nếu khác, báo lỗi giao dịch quốc tế không hợp lệ. |
| **59** | 25 | **Beneficiary Account Name**: Tên tài khoản thụ hưởng. | Tự chọn (Optional). Nếu có, hiển thị gợi ý. Tuy nhiên kết quả cuối cùng phải hiển thị theo kết quả API tra cứu thực tế từ NAPAS. |
| **62** | 99 | **Additional Data Field Template**: Thông tin bổ sung giao dịch. | Chứa các sub-tags thông tin bổ sung. |
| **62.08** | 70 | **Purpose of Transaction**: Nội dung lời nhắn chuyển tiền. | Tự chọn (Optional). Nếu có, hệ thống tự động điền vào ô nội dung chuyển khoản và giới hạn tối đa 70 ký tự không dấu. |
| **63** | 04 | **CRC-16 Checksum**: Mã kiểm tra an toàn dữ liệu. | Bắt buộc 4 ký tự Hexa (Ví dụ: `AD3E`). Nằm ở cuối chuỗi payload. |

> [!CAUTION]
> **Quy tắc tính toán và kiểm thử CRC-16 tại Front-End**:
> Hệ thống Front-end bắt buộc phải tính toán mã CRC-16 của chuỗi payload tính từ ký tự đầu tiên đến hết Tag `63` và Length `04` (loại bỏ 4 ký tự cuối cùng của mã kiểm tra). Thuật toán áp dụng là **CRC-16 CCITT** (Đa thức/Polynomial: `0x1021`, Giá trị khởi tạo/Seed: `0xFFFF`). 
> Nếu mã CRC-16 tính ra không khớp với giá trị 4 ký tự cuối cùng của mã QR -> Hệ thống dừng luồng ngay lập tức và hiển thị báo lỗi **`ERR_PAY_003`** để phòng ngừa rủi ro mã QR bị thay đổi thông tin trái phép.

---

### 5.2. Bảng Chi Tiết Luồng Hành Trình Quét QR & Giao Dịch Chuyển Tiền (The Matrix Table)

*   **Kênh thực hiện**: Ứng dụng di động MSB Digibank (iOS & Android).
*   **Điều kiện đầu vào**:
    1.  Khách hàng đã đăng nhập thành công vào ứng dụng MSB Digibank.
    2.  Khách hàng có ít nhất một Tài khoản thanh toán (TKTT) hoặc Tài khoản thấu chi (Overdraft) hoạt động bằng VND, có số dư khả dụng tối thiểu là 2.000 VND (Số tiền tối thiểu chuyển khoản nhanh liên ngân hàng 24/7).
    3.  Thiết bị di động có kết nối Internet ổn định và camera hoạt động bình thường.

#### Bảng đặc tả chi tiết từng bước nghiệp vụ:

| STT Bước | PIC | Thao tác Người dùng | Logic hệ thống & Quy tắc kiểm tra (Business Rules) | Kết quả & Chuyển bước tiếp theo |
| :---: | :--- | :--- | :--- | :--- |
| **Bước 1** | Khách hàng & App (FE) | Khách hàng bấm chọn biểu tượng `[Quét mã QR]` tại trang chủ hoặc màn hình Quick Access. | 1. **Kiểm tra an toàn thiết bị (Rooted/Jailbreak Check):** Hệ thống chạy ngầm kiểm tra thiết bị. Nếu phát hiện thiết bị đã Root/Jailbreak -> Chặn truy cập và hiển thị màn hình thông báo lỗi [ERR_PAY_001]. <br> 2. **Cấp quyền truy cập Camera:** Hệ thống kiểm tra quyền truy cập Camera của ứng dụng. Nếu chưa được cấp quyền, hiển thị popup yêu cầu cấp quyền truy cập thiết bị. | - Nếu hợp lệ: Kích hoạt camera quét mã trực tiếp, hiển thị nút `[Bật Flash]` thủ công, nút `[Chọn ảnh từ thư viện]` -> Chuyển sang **Bước 2**. <br> - Nếu bị Rooted: Ngắt luồng và báo lỗi thiết bị bảo mật [ERR_PAY_001]. |
| **Bước 2** | Khách hàng & App (FE) | Khách hàng đưa camera quét mã VietQR hoặc chọn ảnh QR lưu trong Gallery. | 1. **Quét trực tiếp qua Camera:** Camera thu hình ảnh mã QR. Hệ thống tích hợp cảm biến ánh sáng thiết bị. Nếu độ sáng môi trường quá tối ($< 10$ lux) -> Nút bật đèn Flash trên giao diện quét sẽ tự động nhấp nháy đèn viền nhẹ gợi ý khách hàng nhấn bật hỗ trợ quét. <br> 2. **Chọn ảnh từ Gallery:** Khách hàng chọn hình ảnh từ thư viện thiết bị. Hệ thống kiểm tra ảnh được chọn: <br> - Nếu ảnh chứa từ 2 mã QR trở lên HOẶC không nhận dạng được mã QR nào -> Hệ thống lập tức báo lỗi định dạng không hỗ trợ và hiển thị popup [ERR_PAY_002]. <br> 3. **Kiểm tra CRC-16 và định dạng:** Thực hiện giải mã EMVCo trực tiếp tại FE và kiểm thử mã CRC-16 theo tiêu chuẩn NAPAS (Mục 5.1). | - Nếu QR Hợp lệ: Phân tách chuỗi payload thành công và lưu trữ tạm các thông tin giải mã -> Chuyển sang **Bước 3**. <br> - Nếu CRC sai: Hiện popup lỗi [ERR_PAY_003]. <br> - Nếu sai cấu trúc VietQR chung hoặc Gallery chứa nhiều mã/không có mã: Hiện popup [ERR_PAY_002]. |
| **Bước 3** | Hệ thống App & DIP | Hệ thống tự động xử lý yêu cầu ngầm sau khi giải mã QR thành công. | 1. Hệ thống trích xuất mã BIN NAPAS (Tag `38.01`) và Số tài khoản thụ hưởng (Tag `38.02`). <br> 2. **Gọi API NAPAS Inquiry**: DBS gửi yêu cầu thông qua DIP gọi sang dịch vụ Napas Inquiry để xác thực số tài khoản và lấy Tên chủ tài khoản thụ hưởng thực tế tại Ngân hàng đích. <br> 3. Tự động ánh xạ mã BIN NAPAS sang Tên ngân hàng thành viên (Ví dụ: BIN `970426` -> Ngân hàng MSB). | - Nếu NAPAS trả tên hợp lệ: Tải dữ liệu tên chủ tài khoản và điều hướng sang màn hình Giao dịch (**Bước 4**). <br> - Nếu NAPAS báo lỗi tài khoản không tồn tại / Ngân hàng lỗi: Hiển thị popup báo lỗi tra cứu [ERR_PAY_004]. |
| **Bước 4** | Khách hàng & App (FE) | Khách hàng kiểm tra thông tin thụ hưởng hiển thị và chọn tài khoản trích nợ, số tiền, nội dung. | 1. **Hiển thị thông tin thụ hưởng**: Hệ thống hiển thị rõ ràng: Tên ngân hàng nhận, Số tài khoản nhận, Tên chủ tài khoản thụ hưởng (lấy từ API NAPAS). <br> 2. **Lựa chọn tài khoản nguồn**: Hệ thống tự động truy vấn danh sách Tài khoản thanh toán (TKTT) và Tài khoản thấu chi (Overdraft) hoạt động bằng VND của khách hàng. Mặc định hiển thị tài khoản có số dư khả dụng lớn nhất. Cho phép người dùng nhấn vào để hiển thị danh sách dropdown và lựa chọn tài khoản nguồn khác. <br> 3. **Autofill & Lock Số tiền đối với QR Động**: Kiểm tra nếu Tag `01` = `12` (Dynamic QR), hệ thống tự động điền số tiền từ Tag `54` và **Khóa cứng không cho người dùng chỉnh sửa ô nhập tiền** (Read-only). <br> 4. **Autofill Số tiền đối với QR Tĩnh**: Nếu Tag `01` = `11` (Static QR), hệ thống để ô nhập số tiền rỗng để KH tự nhập (Số tiền phải $\ge 2.000$ VND và $\le$ HHM giao dịch hoặc tối đa 499.999.999 VND/giao dịch). <br> 5. **Autofill & Lọc dấu Nội dung**: Tự động điền nội dung chuyển tiền từ Tag `62.08` (nếu có). Cho phép người dùng chỉnh sửa nội dung chuyển khoản tối đa 70 ký tự. Hệ thống tự động lọc bỏ hoàn toàn các ký tự Tiếng Việt có dấu (Ví dụ: "Chuyen khoan" thay vì "Chuyển khoản") trước khi truyền dữ liệu qua API. | Khách hàng kiểm tra thông tin tài khoản trích nợ, số tiền và nội dung, sau đó nhấn nút `[Tiếp tục]` -> Chuyển sang **Bước 5**. |
| **Bước 5** | Hệ thống App (BE) | Hệ thống tự động kiểm tra số dư và hạn mức giao dịch trước khi xác thực. | 1. **Kiểm tra số dư khả dụng**: Số dư khả dụng của tài khoản nguồn được chọn (TKTT hoặc tài khoản thấu chi) phải $\ge$ Số tiền chuyển khoản + Phí giao dịch (Miễn phí 100% dịch vụ). Nếu số dư không đủ -> Hiển thị popup báo lỗi [ERR_PAY_005]. <br> 2. **Kiểm tra Hạn mức Giao dịch (Velocity)**: Kiểm tra số tiền chuyển không vượt quá HHM ngày của gói dịch vụ khách hàng đang sở hữu hoặc tối đa 499,999,999 VND/giao dịch. Nếu vượt quá -> Hiển thị popup lỗi [ERR_PAY_006]. <br> 3. **Chốt chặn Quyết định 2345/QĐ-NHNN**: Hệ thống thực hiện kiểm tra hạn mức xác thực sinh trắc học bắt buộc: <br> - Nếu Số tiền giao dịch một lần **$> 10.000.000$ VND** <br> HOẶC Tổng số tiền chuyển khoản trực tuyến tích lũy trong ngày **$> 20.000.000$ VND** -> Hệ thống kích hoạt camera xác thực khuôn mặt sinh trắc học Face Authen (FA). So khớp dữ liệu khuôn mặt live với dữ liệu CCCD gắn chip lưu trên server của MSB. | - Nếu thỏa mãn và nằm dưới ngưỡng 2345: Chuyển thẳng sang bước Smart OTP (**Bước 6**). <br> - Nếu vượt ngưỡng 2345: Kích hoạt luồng Face Authen. Người dùng xác thực so khớp khuôn mặt live thành công -> Chuyển sang **Bước 6**. <br> - Nếu Số dư không đủ: Báo lỗi [ERR_PAY_005]. <br> - Nếu Hạn mức vượt gói: Báo lỗi [ERR_PAY_006]. <br> - Nếu Xác thực khuôn mặt thất bại: Báo lỗi [ERR_PAY_007]. |
| **Bước 6** | Khách hàng & App (FE) | Khách hàng thực hiện xác nhận giao dịch bằng mã Smart OTP. | 1. Hệ thống hiển thị màn hình tóm tắt thông tin giao dịch cuối cùng trước khi chuyển tiền. <br> 2. Người dùng nhập mã PIN Smart OTP (hoặc xác thực bằng vân tay/khuôn mặt FaceID đã được đăng ký liên kết Smart OTP trên thiết bị). <br> 3. **Kiểm soát giới hạn số lần nhập sai**: Khách hàng nhập sai Smart OTP tối đa 3 lần liên tiếp. | - Nếu Smart OTP Đúng: Sinh chữ ký số bảo mật và truyền yêu cầu chuyển mạch tiền gửi -> Chuyển sang **Bước 7**. <br> - Nếu Sai OTP lần 1, 2: Báo lỗi cho phép nhập lại [ERR_OTP_RETRY]. <br> - Nếu Sai OTP lần 3: Khóa luồng chuyển tiền và tính năng tài chính của App trong 2 giờ [ERR_OTP_LOCKED]. |
| **Bước 7** | Hệ thống & Đối tác | Hệ thống thực thi xử lý lệnh chuyển tiền nhanh liên ngân hàng qua NAPAS. | 1. **Trích nợ Core**: DIP gọi API trích nợ tài khoản nguồn của KH tại Core Banking MSB. <br> 2. **Chuyển tiền NAPAS**: DIP gửi lệnh gọi API chuyển tiền Napas Credit đến hệ thống NAPAS để ghi có cho tài khoản thụ hưởng tại ngân hàng đích. <br> 3. **Cơ chế xử lý Timeout 30 giây**: Hệ thống thiết lập thời gian chờ phản hồi tối đa là 30 giây. Quá 30 giây hệ thống NAPAS không phản hồi kết quả chuyển tiền thành công -> Hệ thống tự động thực hiện hoàn tiền (Reversal) vào tài khoản trích nợ của khách hàng ngay lập tức để phòng ngừa giam giữ tiền vô thời hạn. | - Nếu Chuyển tiền thành công (Mã trả về `00`): Giao dịch hoàn tất -> Chuyển sang **Bước 8**. <br> - Nếu gặp lỗi kết nối quá hạn (Timeout): Hệ thống thực hiện hoàn tiền tự động ngay lập tức và chuyển sang màn hình báo lỗi giao dịch [ERR_PAY_008]. |
| **Bước 8** | Hệ thống App (FE) | Hệ thống hiển thị màn hình kết quả giao dịch chuyển khoản cho người dùng. | 1. **Hiển thị màn hình Giao dịch Thành Công**: Với đầy đủ biên lai điện tử: Mã giao dịch, Số TK trích nợ, Ngân hàng thụ hưởng, Tên người nhận, Số tiền, Nội dung chuyển, Ngày giờ thực hiện. <br> 2. **Kiểm tra Danh bạ thụ hưởng (Smart Beneficiary Check)**: Hệ thống chạy ngầm kiểm tra xem số tài khoản thụ hưởng này đã có trong Danh bạ thụ hưởng của khách hàng hay chưa. <br> - Nếu số TK thụ hưởng **CHƯA được lưu** -> Hiển thị nút **`[Lưu danh bạ]`** trực quan trên giao diện kết quả giao dịch. <br> - Nếu số TK thụ hưởng **ĐÃ được lưu** -> **Ẩn hoàn toàn** nút `[Lưu danh bạ]`. <br> 3. Kích hoạt gửi tin nhắn Push Noti biến động số dư tài khoản của khách hàng qua thiết bị di động. | - Khách hàng bấm nút `[Lưu danh bạ]` (nếu hiển thị): Nhập Nickname gợi nhớ và hệ thống xác nhận lưu vào danh bạ qua Toast thành công. <br> - Khách hàng bấm nút `[Thực hiện giao dịch mới]` hoặc `[Quay về trang chủ]` -> Kết thúc trọn vẹn hành trình. |

---

## 6. QUY CHUẨN THÔNG BÁO POPUP LỖI TRÊN GIAO DIỆN DI ĐỘNG

Dưới đây là chi tiết các thiết kế UI Copy hiển thị dành cho các tình huống ngoại lệ/rẽ luồng lỗi xảy ra trong hành trình.

### 6.1. Popup lỗi [ERR_PAY_001]: Thiết bị không an toàn (Root/Jailbreak Check)
*   **Tiêu đề (Title)**: Thiết bị không an toàn
*   **Nội dung mô tả (Content)**: Để bảo vệ an toàn cho tài khoản và thông tin cá nhân của Quý khách, ứng dụng MSB Digibank tạm dừng hoạt động trên các thiết bị đã bị can thiệp hệ thống (Root/Jailbreak). Vui lòng sử dụng thiết bị di động an toàn để tiếp tục giao dịch.
*   **Nút hành động (CTA)**: `[Đóng ứng dụng]` -> Thoát ứng dụng ngay lập tức để bảo vệ dòng tiền.

### 6.2. Popup lỗi [ERR_PAY_002]: Mã QR từ thư viện không hợp lệ / Nhiều mã QR
*   **Tiêu đề (Title)**: Hình ảnh QR không hợp lệ
*   **Nội dung mô tả (Content)**: Hình ảnh Quý khách chọn không chứa mã QR, chứa nhiều hơn 1 mã QR hoặc mã QR bị nhòe/không thể nhận diện. Vui lòng quét trực tiếp bằng camera hoặc chọn hình ảnh chứa duy nhất một mã VietQR rõ nét hơn.
*   **Nút hành động (CTA)**: `[Quét lại]` -> Đóng popup và kích hoạt lại camera để quét trực tiếp tại Bước 1.

### 6.3. Popup lỗi [ERR_PAY_003]: Mã QR bị sai lệch hoặc không hợp lệ (EMVCo & CRC-16 Check)
*   **Tiêu đề (Title)**: Lỗi kiểm tra dữ liệu QR
*   **Nội dung mô tả (Content)**: Dữ liệu mã QR không toàn vẹn hoặc có dấu hiệu bị sai lệch thông tin bảo mật (Lỗi kiểm tra CRC-16). Vì sự an toàn của Quý khách, vui lòng không quét mã này và liên hệ đơn vị cung cấp mã QR để được hỗ trợ lại.
*   **Nút hành động (CTA)**: `[Quét mã khác]` -> Đóng popup và kích hoạt lại camera quét mã mới tại Bước 1.

### 6.4. Popup lỗi [ERR_PAY_004]: Tra cứu tài khoản thụ hưởng thất bại (NAPAS Inquiry Failed)
*   **Tiêu đề (Title)**: Không tìm thấy tài khoản nhận
*   **Nội dung mô tả (Content)**: Số tài khoản thụ hưởng không tồn tại tại Ngân hàng đã chọn hoặc Ngân hàng thụ hưởng đang tạm thời gián đoạn kết nối đối soát dịch vụ 24/7 qua hệ thống chuyển mạch NAPAS. Quý khách vui lòng kiểm tra lại thông tin thụ hưởng.
*   **Nút hành động (CTA)**: `[Quét lại]` -> Đóng popup và quay lại màn hình quét QR tại Bước 1; `[Về trang chủ]` -> Quay lại màn hình Home.

### 6.5. Popup lỗi [ERR_PAY_005]: Tài khoản không đủ số dư khả dụng (VND Balance Checked)
*   **Tiêu đề (Title)**: Số dư tài khoản không đủ
*   **Nội dung mô tả (Content)**: Số dư khả dụng của tài khoản nguồn được chọn không đủ để thực hiện giao dịch. Số dư khả dụng hiện tại là `{available_balance}` VND, số tiền yêu cầu chuyển khoản là `{requested_amount}` VND. Quý khách vui lòng nạp thêm tiền hoặc đổi sang tài khoản thanh toán khác.
*   **Nút hành động (CTA)**: `[Đổi tài khoản nguồn]` -> Đóng popup, đưa con trỏ về ô chọn tài khoản trích nợ ở Bước 4 để KH bấm mở dropdown chọn tài khoản khác.

### 6.6. Popup lỗi [ERR_PAY_006]: Giao dịch vượt hạn mức gói dịch vụ (Velocity Limit Checked)
*   **Tiêu đề (Title)**: Vượt hạn mức giao dịch quy định
*   **Nội dung mô tả (Content)**: Số tiền Quý khách yêu cầu chuyển vượt quá hạn mức tối đa cho phép trên một giao dịch của gói dịch vụ IBMB hiện tại hoặc vượt ngưỡng chuyển nhanh NAPAS 24/7 (Tối đa 499,999,999 VND/giao dịch). Quý khách vui lòng điều chỉnh lại số tiền hoặc liên hệ chi nhánh MSB gần nhất để nâng hạn mức giao dịch.
*   **Nút hành động (CTA)**: `[Chỉnh sửa số tiền]` -> Đóng popup và đặt con trỏ tại ô nhập số tiền ở Bước 4 (nếu là QR tĩnh).

### 6.7. Popup lỗi [ERR_PAY_007]: Xác thực khuôn mặt theo QĐ 2345 thất bại (Face Authen Failed)
*   **Tiêu đề (Title)**: Xác thực sinh trắc học thất bại
*   **Nội dung mô tả (Content)**: Khuôn mặt của Quý khách không khớp với thông tin đã được đăng ký eKYC/CCCD gắn chip tại MSB. Để đảm bảo an toàn giao dịch bảo mật hạn mức lớn theo QĐ 2345/QĐ-NHNN, vui lòng thực hiện lại trong điều kiện đủ ánh sáng, không đeo kính râm/khẩu trang hoặc liên hệ Hotline 1900xxxx để được cập nhật dữ liệu sinh trắc học.
*   **Nút hành động (CTA)**: `[Thử lại]` -> Kích hoạt lại camera chụp khuôn mặt Face Authen; `[Hủy giao dịch]` -> Đóng luồng quay lại màn hình Home.

### 6.8. Popup lỗi [ERR_PAY_008]: Quá thời gian chuyển khoản liên ngân hàng (NAPAS Timeout Reversal)
*   **Tiêu đề (Title)**: Giao dịch gặp gián đoạn kết nối
*   **Nội dung mô tả (Content)**: Kết nối chuyển mạch liên ngân hàng qua hệ thống NAPAS bị gián đoạn. Để bảo vệ quyền lợi của Quý khách, hệ thống MSB đã kích hoạt lệnh hoàn trả tiền tự động ngay lập tức vào tài khoản trích nợ `{source_account_id}`. Quý khách vui lòng kiểm tra lại biến động số dư và thực hiện lại sau.
*   **Nút hành động (CTA)**: `[Xác nhận]` -> Đóng popup, tự động cập nhật Noti hoàn tiền và đưa người dùng về màn hình Home.

---

## 7. ĐẶC TẢ TÍCH HỢP DỮ LIỆU & DATA MAPPING SCHEMAS

### Bảng 7.1: Dữ liệu Yêu cầu Chuyển khoản QR gửi từ App lên Cổng DIP/Core (Request Schema)

Bảng chi tiết thông tin truyền nhận API phục vụ luồng chuyển tiền VietQR:

| Tên Trường (Tiếng Việt) | Mã trường hệ thống | Kiểu dữ liệu | Thuộc tính (M/O) | Nguồn dữ liệu | Mô tả logic / Quy tắc nguồn |
| :--- | :--- | :--- | :---: | :--- | :--- |
| Mã số CIF khách hàng | `cif_number` | String | M | Core Banking | Mã định danh khách hàng đăng nhập. |
| Tài khoản trích nợ nguồn | `source_account_id` | String | M | Nhập từ UI | Số TKTT hoặc tài khoản thấu chi khách hàng chọn làm nguồn thanh toán. |
| Mã BIN ngân hàng nhận | `beneficiary_bank_bin` | String | M | Trích từ QR | Trích xuất từ Tag `38.01` trong mã QR. |
| Số tài khoản thụ hưởng | `beneficiary_account_id` | String | M | Trích từ QR | Trích xuất từ Tag `38.02` trong mã QR. |
| Tên chủ tài khoản nhận | `beneficiary_account_name`| String | M | API NAPAS | Tên chủ tài khoản trả về sau bước tra cứu tại NAPAS ở Bước 3. |
| Số tiền chuyển khoản | `transaction_amount` | Decimal | M | UI / Trích từ QR | Số tiền giao dịch (Lấy từ Tag `54` của QR động hoặc do người dùng nhập từ QR tĩnh). |
| Nội dung chuyển khoản | `transaction_description` | String | M | UI / Trích từ QR | Nội dung lời nhắn chuyển tiền (Lấy từ Tag `62.08` hoặc KH nhập tay). Hệ thống Front-end tự động lọc bỏ dấu tiếng Việt trước khi truyền API. |
| Loại mã QR sử dụng | `qr_initiation_type` | String | M | Trích từ QR | Mã `11` nếu quét QR Tĩnh, mã `12` nếu quét QR Động. |
| Toàn bộ chuỗi QR gốc | `raw_qr_payload` | String | M | Bộ quét QR | Chuỗi dữ liệu QR gốc quét được (dùng để lưu log tra soát giao dịch). |
| Chữ ký số Smart OTP | `smart_otp_signature` | String | M | Smart OTP | Chữ ký điện tử sinh ra trên thiết bị di động của KH sau khi nhập đúng Smart OTP nhằm thực hiện lệnh ký số. |
| Mã xác thực Face Authen | `face_auth_token` | String | O | Face Authen | Token sinh ra từ server Face Authen của MSB chứng nhận khuôn mặt hợp lệ (bắt buộc khi giao dịch vượt ngưỡng QĐ 2345). |

### Bảng 7.2: Dữ liệu Phản hồi từ Cổng DIP/Core về Mobile App (Response Schema)

| Tên Trường (Tiếng Việt) | Mã trường hệ thống | Kiểu dữ liệu | Thuộc tính (M/O) | Mô tả logic / Giá trị trả về |
| :--- | :--- | :--- | :---: | :--- |
| Mã kết quả giao dịch | `response_code` | String | M | Trả về `00` nếu chuyển tiền Core và liên ngân hàng thành công; Trả về mã lỗi tương ứng (Ví dụ: `09` - Số dư không đủ, `91` - NAPAS lỗi timeout) nếu giao dịch thất bại. |
| Nội dung thông báo lỗi | `response_message` | String | M | Mô tả chi tiết kết quả xử lý từ hệ thống phục vụ hiển thị/log. |
| Mã tham chiếu giao dịch | `transaction_reference_id` | String | M | Mã giao dịch duy nhất được sinh ra từ Core Banking phục vụ tra soát và đối soát định kỳ. |
| Số dư tài khoản khả dụng | `available_balance` | Decimal | M | Số dư khả dụng còn lại của tài khoản trích nợ nguồn sau khi đã trừ tiền chuyển khoản thành công. |
| Ngày giờ hoàn tất GD | `transaction_timestamp` | String | M | Thời gian giao dịch được hạch toán thành công dưới Core theo định dạng `YYYY-MM-DD HH:mm:ss`. |
| Trạng thái tài khoản thụ hưởng mới | `is_new_beneficiary` | Boolean | M | Trả về `true` nếu tài khoản thụ hưởng này chưa tồn tại trong danh bạ của khách hàng; Trả về `false` nếu đã tồn tại trong danh bạ. |

---

## 8. THÔNG BÁO ĐA KÊNH GIAO DỊCH (NOTI STRATEGY)

Để duy trì tính liên tục của trải nghiệm và bảo mật dòng tiền của khách hàng, hệ thống tích hợp các kênh thông báo tự động sau:

### 8.1. Thông báo biến động số dư qua Notification Center (Push Noti In-app)
*   **Thời điểm kích hoạt**: Ngay sau khi Core Banking ghi nhận hạch toán trích nợ tài khoản nguồn thành công tại Bước 7.
*   **Tiêu đề**: Biến động số dư tài khoản
*   **Nội dung tin nhắn**: `Tài khoản {source_account_id} của Quý khách vừa bị trừ -{transaction_amount} VND cho giao dịch chuyển khoản VietQR nhanh 24/7 đến tài khoản {beneficiary_account_id} ({beneficiary_account_name}) tại {beneficiary_bank_name} thành công. Số dư khả dụng hiện tại là {available_balance} VND. Cảm ơn Quý khách đã sử dụng dịch vụ của MSB!`

### 8.2. Thông báo SMS Biến động số dư (Dành cho KH đăng ký nhận SMS chủ động)
*   **Thời điểm kích hoạt**: Ngay sau khi hạch toán thành công nếu KH có đăng ký gói SMS Banking.
*   **Nội dung tin nhắn không dấu**: `MSB: TK {source_account_id} -{transaction_amount}VND luc {transaction_timestamp}. GD: CK VietQR 247 den {beneficiary_account_name} ({beneficiary_account_id}) cua bank {beneficiary_bank_bin}. So du kha dung: {available_balance}VND.`

### 8.3. Thông báo Push Noti hoàn tiền tự động (Dành cho trường hợp Timeout giao dịch)
*   **Thời điểm kích hoạt**: Ngay sau khi lệnh Hoàn trả (Reversal) được thực thi thành công tại Bước 7 do NAPAS bị timeout kết nối 30s.
*   **Tiêu đề**: Hoàn tiền giao dịch chuyển khoản liên ngân hàng
*   **Nội dung tin nhắn**: `MSB đã thực hiện hoàn tiền tự động +{transaction_amount} VND vào tài khoản {source_account_id} của Quý khách. Lý do: Giao dịch chuyển khoản VietQR nhanh 24/7 đến tài khoản {beneficiary_account_id} bị gián đoạn do sự cố kết nối từ hệ thống NAPAS. MSB thành thật xin lỗi vì sự bất tiện này!`

---

## 9. TÀI LIỆU THAM CHIẾU VÀ CÁC QUY ĐỊNH LIÊN QUAN

1.  **Quyết định số 2345/QĐ-NHNN** ban hành ngày 18/12/2023 của Thống đốc Ngân hàng Nhà nước Việt Nam về việc triển khai các giải pháp an toàn, bảo mật trong thanh toán trực tuyến và thanh toán thẻ ngân hàng.
2.  **Bộ tiêu chuẩn kỹ thuật VietQR v1.1** ban hành bởi Công ty Cổ phần Thanh toán Quốc gia Việt Nam (NAPAS), kế thừa đặc tả QR Merchant-Presented của tổ chức EMVCo.
3.  **Quy chế hoạt động cổng chuyển mạch tài chính liên ngân hàng** của Ngân hàng Nhà nước Việt Nam và các Quy chế nghiệp vụ NAPAS 24/7.
