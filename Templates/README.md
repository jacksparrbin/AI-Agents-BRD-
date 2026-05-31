# 📋 Digital Banking BRD Templates Directory

Chào mừng bạn đến với thư mục chứa các **Tài liệu biểu mẫu (BRD Templates)** chuẩn hóa. Bộ tài liệu này được đúc kết từ thực tế phân tích nghiệp vụ ngân hàng bán lẻ hiện đại nhằm cung cấp các tiêu chuẩn biểu diễn nghiệp vụ rõ ràng, không mơ hồ cho đội ngũ Phát triển (Dev) và Kiểm thử (QA/QC).

---

## 📂 Danh sách các biểu mẫu khả dụng

Chúng tôi cung cấp **3 mẫu tài liệu BRD chuyên biệt** phù hợp với từng phân hệ tính năng và hành trình cụ thể:

### 1. [Mẫu BRD Tiêu chuẩn](file:///Users/minhphuong/Documents/AI%20folder/AI%20Agents%20vie%CC%82%CC%81t%20ta%CC%80i%20lie%CC%A3%CC%82u%20BRD/Templates/brd_template_standard.md) (`brd_template_standard.md`)
*   **Mục đích:** Áp dụng cho các tính năng nghiệp vụ chung, các tiện ích bổ sung không thuộc nhóm đăng ký/chuyển tiền/tín dụng.
*   **Nội dung chính:** Cung cấp khung cấu trúc chuẩn hóa cho một BRD tiêu chuẩn, bao gồm Lịch sử phiên bản, Thuật ngữ, Yêu cầu nghiệp vụ chi tiết, và Yêu cầu phi chức năng.

### 2. [Mẫu BRD Onboarding & eKYC](file:///Users/minhphuong/Documents/AI%20folder/AI%20Agents%20vie%CC%82%CC%81t%20ta%CC%80i%20lie%CC%A3%CC%82u%20BRD/Templates/brd_template_onboarding.md) (`brd_template_onboarding.md`)
*   **Mục đích:** Chuyên biệt cho các hành trình **Đăng ký tài khoản mới (NTB)**, **Định danh khách hàng hiện hữu (eKYC)**, tích hợp dữ liệu dân cư quốc gia (VNeID/CCCD gắn chip), và kiểm tra bảo mật thiết bị.
*   **Nội dung chính:** Tập trung vào các chốt chặn eKYC, phân tích rớt luồng (Drop-off), thu hồi hành trình (Recovery) và logic kiểm tra trùng lặp thông tin (Duplicate Checks).

### 3. [Mẫu BRD Giao dịch & Tín dụng](file:///Users/minhphuong/Documents/AI%20folder/AI%20Agents%20vie%CC%82%CC%81t%20ta%CC%80i%20lie%CC%A3%CC%82u%20BRD/Templates/brd_template_transaction_lending.md) (`brd_template_transaction_lending.md`)
*   **Mục đích:** Thiết kế riêng cho hành trình tài chính bao gồm **Thanh toán hóa đơn (Billing)**, **Nạp tiền dịch vụ (Topup)**, **Chuyển tiền nhanh liên ngân hàng (VietQR)**, và **Mở hạn mức tín dụng trực tuyến (DigiLending)**.
*   **Nội dung chính:** Đặc tả chi tiết về hạn mức giao dịch (Velocity Limits), chốt chặn sinh trắc học theo Quyết định 2345/QĐ-NHNN, cơ chế Smart OTP, hạch toán Core Banking, và bản đồ tích hợp dữ liệu (Data Mapping Schema).

---

## 💡 Quy tắc vàng khi sử dụng Templates

Khi bạn biên soạn BRD dựa trên các mẫu này, hãy tuân thủ 3 quy tắc sau để đảm bảo chất lượng cao nhất:

1.  **Sử dụng Bảng Ma trận PO (The Matrix Table):** 
    Luôn mô tả hành trình chi tiết bằng cấu trúc 3 cột: *Thao tác người dùng* | *Logic hệ thống & Business Rules* | *Kết quả & Chuyển bước (Next Action)*. Tránh mô tả chung chung hoặc sơ sài.
2.  **Viết rõ đặc tả UI Copy & Popup:**
    Mọi thông báo popup lỗi hoặc xác nhận phải có đủ 3 thành phần: **Title** (Tiêu đề), **Content** (Nội dung chi tiết có chứa biến dynamic), và **CTA** (Nút hành động kèm điều hướng).
3.  **Vẽ Sơ đồ Sequence bằng Mermaid:**
    Tích hợp trực tiếp sơ đồ luồng dữ liệu hoặc hành trình bằng cú pháp Mermaid để lập trình viên dễ hình dung luồng trao đổi API giữa FE - Middleware - Core Banking.
