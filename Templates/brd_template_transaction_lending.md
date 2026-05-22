# [TÊN DỰ ÁN / TÊN TÍNH NĂNG]
## TÀI LIỆU YÊU CẦU NGHIỆP VỤ - HÀNH TRÌNH THANH TOÁN DỊCH VỤ, HẠN MỨC & TÍN DỤNG SỐ (TRANSACTION, PAYMENT & LENDING)
**Mã tài liệu:** DCTBR-[LEN/PAY-XXXX] BRD [Tên hành trình/MVP]  
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

### 5.1. Mô Tả Luồng Đăng Ký Cấp Hạn Mức Tín Dụng (Lending Flow)
```markdown
| STT | Tên Bước | PIC | Thao tác người dùng | Logic nghiệp vụ & Kiểm tra ngầm hệ thống | Kết quả & Chuyển bước tiếp theo |
| :---: | :--- | :--- | :--- | :--- | :--- |
| **1** | Khảo sát & Tính toán | Khách hàng | Khách hàng chọn `[Khám phá hạn mức]` -> Trả lời bộ câu hỏi khảo sát nhanh về thu nhập. | - Gọi dịch vụ Calculator Engine để tính hạn mức thấu chi dự kiến dựa trên bộ tiêu chí của Criteria Management. | - Hiển thị hạn mức phê duyệt dự kiến và các đề xuất gói thẻ phù hợp -> Chuyển sang **Bước 2**. |
| **2** | Nhập SĐT & Check eBank | Khách hàng / Hệ thống | Khách hàng xác thực Số điện thoại. | - Kiểm tra xem SĐT này đã đăng ký sử dụng dịch vụ ngân hàng điện tử (eBank) chưa. | - Nếu chưa: Chuyển sang bước Onboarding. <br>- Nếu có: Yêu cầu Đăng nhập tài khoản hiện hữu -> Chuyển sang **Bước 3**. |
| **3** | Khai báo hồ sơ vay | Khách hàng | Khách hàng kiểm tra thông tin tự động điền (Auto-fill) và khai báo thêm các trường bắt buộc. | - Tự động điền các thông tin đã lưu gần nhất trong hệ thống (nếu có hồ sơ drop-off). <br>- Kiểm tra định dạng dữ liệu đầu vào. | - Nếu hợp lệ: Hiển thị Đơn đề nghị cấp hạn mức kèm TnC -> Chuyển sang **Bước 4**. |
| **4** | Phê duyệt & Trả kết quả | Hệ thống | Khách hàng ký số Đơn đề nghị -> Xác nhận gửi hồ sơ. | - Hệ thống gửi hồ sơ sang cổng BPM Ops xét duyệt tự động. <br>- Nếu thời gian xử lý $\le 5$ giây: Trả kết quả trực tiếp trên màn hình. <br>- Nếu thời gian xử lý $> 5$ giây: Điều hướng sang trang theo dõi hồ sơ. | - Thành công: Hiển thị màn hình tiếp nhận và cấp hạn mức -> Chuyển bước phát hành thẻ. <br>- Thất bại: Hiển thị thông báo hướng dẫn liên hệ Hotline hỗ trợ. |
```

### 5.2. Chốt Chặn Kiểm Tra Logic Cho Luồng Thanh Toán Dịch Vụ (Utility Payments Validation Rules)
Khi khách hàng thực hiện khởi tạo yêu cầu thanh toán hóa đơn hoặc nạp tiền dịch vụ (Billing Topup), hệ thống bắt buộc phải kiểm tra qua các điều kiện sau:
1.  **Gói IBMB Khách hàng đang sử dụng**:
    *   Kiểm tra gói dịch vụ có bị chặn tính năng thanh toán trực tuyến không.
2.  **Tài khoản thanh toán (TKTT) nguồn**:
    *   Trạng thái TKTT phải là hoạt động (`Active`).
    *   Số dư khả dụng trong tài khoản phải lớn hơn hoặc bằng Số tiền giao dịch + Phí giao dịch (nếu có).
3.  **Tính hợp lệ của Mã hóa đơn / Mã khách hàng**:
    *   Gửi lệnh truy vấn sang đối tác nhà cung cấp dịch vụ để lấy thông tin hóa đơn nợ cước.
    *   Hệ thống kiểm tra nếu hóa đơn đã được thanh toán trước đó -> Báo lỗi hóa đơn đã được thanh toán.
4.  **Hạn mức xác thực sinh trắc học theo quy định Nhà nước**:
    *   Nếu Số tiền giao dịch $> 10,000,000$ VND/lần hoặc Tổng hạn mức giao dịch trong ngày $> 20,000,000$ VND -> Bắt buộc kích hoạt camera xác thực khuôn mặt sinh trắc học (Face Authen) trước khi hiển thị màn hình nhập OTP.

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
