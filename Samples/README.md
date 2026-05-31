# 🚀 Real-World Markdown BRD Samples Directory

Chào mừng bạn đến với thư mục chứa **Các tài liệu mẫu BRD thực tế**. Đây là các tài liệu đặc tả hoàn chỉnh được viết hoàn toàn bằng Markdown, thể hiện trực quan việc áp dụng toàn bộ bộ tiêu chuẩn, sơ đồ luồng Mermaid và cấu trúc ma trận của MSB CTB PO Standard.

---

## 📂 Danh sách các tài liệu mẫu

Chúng tôi cung cấp **3 tài liệu đặc tả thực tế cao cấp** thuộc phân hệ Ngân hàng số:

### 1. [BRD Quét mã VietQR chuyển khoản liên ngân hàng 24/7](file:///Users/minhphuong/Documents/Ta%CC%80i%20lie%CC%A3%CC%82u%20Brd%20MSB%20CTB/Samples/DCTBR-%5BDaily%20Banking%5D%20%5BPayment%5D%20Que%CC%81t%20QR%20chuye%CC%82%CC%89n%20khoa%CC%81n%20VietQR.md) (`DCTBR-[Daily Banking] [Payment] Quét QR chuyển khoản VietQR.md`)
*   **Hành trình:** Khách hàng sử dụng camera quét mã VietQR (Tĩnh hoặc Động), tự động truy vấn thông tin NAPAS, điền dữ liệu hóa đơn, kiểm tra bảo mật 2345 và Smart OTP để thực hiện hạch toán chuyển khoản.
*   **Điểm nổi bật để tham khảo:**
    *   **VietQR Technical Spec:** Bảng phân tích chi tiết toàn bộ các Tag dữ liệu tiêu chuẩn EMVCo (Tag 00, 01, 38, 54, 62.08, 63) và thuật toán tính mã kiểm tra an toàn dữ liệu **CRC-16 CCITT**.
    *   **Mermaid Sequence Flow:** Sơ đồ chi tiết luồng tương tác giữa Khách hàng - App - DBS - DIP - NAPAS - Core Banking.
    *   **Popup Standard:** Đặc tả chi tiết 8 trường hợp popup thông báo lỗi thực tế trên giao diện di động.
    *   **Data Mapping:** Khung dữ liệu truyền nhận request/response của dịch vụ chuyển tiền.

### 2. [BRD Rút tiền bằng mã QR tại cây ATM](file:///Users/minhphuong/Documents/Ta%CC%80i%20lie%CC%A3%CC%82u%20Brd%20MSB%20CTB/Samples/sample_brd_atm_qr_withdrawal.md) (`sample_brd_atm_qr_withdrawal.md`)
*   **Hành trình:** Khách hàng rút tiền tại cây ATM vật lý không cần mang theo thẻ nhựa bằng cách sử dụng ứng dụng di động để quét mã QR hiển thị tại ATM.
*   **Điểm nổi bật để tham khảo:**
    *   Mô tả hành trình đa thiết bị (tương tác chéo giữa Màn hình ATM vật lý và Ứng dụng Mobile App).
    *   Logic kiểm soát rủi ro, giới hạn hạn mức rút tiền ngày và chốt chặn sinh trắc học.
    *   Quy trình đồng bộ trạng thái giao dịch giữa ATM - Core Banking - Mobile App trong trường hợp rút tiền mặt lỗi.

### 3. [BRD Đăng ký mở thẻ tín dụng mới tự chọn/cá nhân hóa thiết kế Card Art](file:///Users/minhphuong/Documents/AI%20folder/AI%20Agents%20vie%CC%82%CC%81t%20ta%CC%80i%20lie%CC%A3%CC%82u%20BRD/Samples/DCTBR-[CARD]%20BRD%20Tu%CC%80y%20cho%CC%A3n%20a%CC%89nh%20hie%CC%82%CC%89n%20thi%CC%A3%20the%CC%89%20ti%CC%81n%20du%CC%A3ng%20khi%20%C4%91a%CC%86ng%20ky%CC%81%20mo%CC%9B%CC%81i.md) (`DCTBR-[CARD] BRD Tùy chọn ảnh hiển thị thẻ tín dụng khi đăng ký mới.md`)
*   **Hành trình:** Khách hàng mới thực hiện đăng ký mở thẻ tín dụng trực tuyến trên App, chọn các mẫu thiết kế thẻ có sẵn (Card Art) hoặc tự upload ảnh cá nhân để cá nhân hóa thẻ ảo trên App và in thẻ vật lý.
*   **Điểm nổi bật để tham khảo (Ver 2.2 Standard):**
    *   **Bảng luồng đặc tả chuẩn 7 cột (Sub-step & Branching Matrix):** Tách bạch rõ ràng Sub-step, PIC, Thao tác, Logic, Pass, Fail, và Mã sự kiện log `EVT_` của ngân hàng.
    *   **Chốt chặn inline hệ thống bắt buộc:** Jailbreak/Root, Batch Time, OTP Retry, Velocity Limits, và sinh trắc học Face Authen theo QĐ 2345/QĐ-NHNN.
    *   **Quy trình thẩm định đạt chuẩn chất lượng:** Báo cáo thẩm định chất lượng độc lập đạt tuyệt đối 20/20 tiêu chí tiêu chuẩn.

---

## 🛠️ Cách đọc và kế thừa các tài liệu mẫu

1.  **Học hỏi cách dựng sơ đồ luồng:** Hãy sao chép phần mã `mermaid` trong các tài liệu mẫu này dán vào các trình duyệt hiển thị Mermaid (hoặc xem trực tiếp trên GitHub/VS Code) để học cách biểu diễn luồng API phức tạp của ngân hàng.
2.  **Kế thừa bảng đặc tả chi tiết:** Sao chép cấu trúc mô tả bước nghiệp vụ (The Matrix Table) để áp dụng vào các dự án ví điện tử, ngân hàng số hoặc sản phẩm fintech khác của bạn.
3.  **Tối ưu hóa UI Copy:** Sử dụng các popup lỗi chuẩn trong tài liệu này làm kim chỉ nam để cải thiện trải nghiệm viết vi sao chép (UX writing) cho các thông báo lỗi hệ thống.
