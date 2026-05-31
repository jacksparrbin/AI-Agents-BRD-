# Eval Test Suite — po-brd-creator (VietQR Domain)

**Mục đích**: Đo lường chất lượng BRD mà Agent po-brd-creator tạo ra cho domain Chuyển khoản VietQR.
**Baseline tham chiếu**: `Samples/BRD-[Daily Banking] [Payment] Quét QR chuyển khoản VietQR.md`
**Skill được test**: `Guides/po_brd_creator_skill.md`
**Tác giả**: Bình Nguyễn Thanh
**Ngày tạo**: 24/05/2026

---

## Hướng dẫn chạy Eval

### Quy trình chạy

1. Kích hoạt skill `po-brd-creator` trên một Claude session mới (không có context cũ).
2. Paste **Input Prompt** của từng Test Case vào chat.
3. Tương tác với Agent qua các Phase (trả lời câu hỏi ở Phase 2 theo **Suggested Answers** trong mỗi TC).
4. Khi Agent xuất BRD hoàn chỉnh, chấm điểm theo **Scoring Rubric** của Test Case đó.
5. Ghi nhận kết quả vào bảng tổng hợp cuối file.

### Nguyên tắc chấm

| Ký hiệu | Ý nghĩa | Điểm |
| :---: | :--- | :---: |
| ✅ | Đạt hoàn toàn — đúng và đủ theo tiêu chí | 1 |
| ⚠ | Đạt một phần — có nhưng thiếu sót nhỏ | 0.5 |
| ❌ | Không đạt — thiếu hoặc sai hoàn toàn | 0 |

**Ngưỡng PASS**: ≥ 80/100 điểm tổng (hoặc ≥ 80% trên mỗi nhóm tiêu chí).

---

## TEST CASE 1: Happy Path — Input đầy đủ, QR chuyển khoản cơ bản

### TC-01 Meta

| Trường | Giá trị |
| :--- | :--- |
| **ID** | TC-01 |
| **Tên** | VietQR Scan & Transfer — Happy Path Full Input |
| **Mục đích** | Kiểm tra Agent tạo BRD hoàn chỉnh khi input đầu vào rõ ràng, đủ thông tin |
| **Độ khó** | ★★☆☆☆ (Easy) |
| **Domain** | Daily Banking → Payment → QR Transfer |
| **Template kỳ vọng** | `brd_template_transaction_lending.md` |

### Input Prompt

```text
Viết BRD cho tính năng quét mã QR chuyển khoản theo chuẩn VietQR trên ứng dụng MSB Digibank. 

Đối tượng sử dụng: Khách hàng cá nhân hiện hữu (ETB) đã kích hoạt app.
Mục tiêu: Khách hàng quét mã VietQR (tĩnh hoặc động) để tự động điền thông tin chuyển khoản liên ngân hàng qua NAPAS 24/7, giảm thời gian giao dịch xuống dưới 15 giây.
Phạm vi: Quét QR bằng camera và chọn từ Gallery. Không bao gồm tạo mã QR cá nhân.
Tuân thủ QĐ 2345/QĐ-NHNN về xác thực sinh trắc học.
```

### Suggested Answers cho Phase 2

Khi Agent hỏi làm rõ, trả lời:
- Hạn mức: Áp dụng hạn mức gói dịch vụ IBMB hiện tại, tối đa 499,999,999 VND/giao dịch NAPAS
- Xác thực: Smart OTP mặc định, Face Authen khi > 10tr/lần hoặc tích lũy > 20tr/ngày
- OTP retry: Tối đa 3 lần, khóa 2 giờ nếu sai 3 lần
- Timeout NAPAS: 30 giây, tự động Reversal hoàn tiền

### Expected Behavior theo Phase

| Phase | Agent phải làm | Tiêu chí đánh giá |
| :--- | :--- | :--- |
| **Phase 1** | Viết User Story INVEST cho ETB quét VietQR. Chọn template `transaction_lending`. Xác định Primary Actor = KH ETB, Secondary = NAPAS, Core Banking, DIP. Scope rõ In/Out. Gate Check 6/6. | US đúng INVEST, persona không generic, scope có In + Out, KPI đo lường được (≤15s) |
| **Phase 2** | Hỏi ≤ 5 câu: hạn mức, xác thực, timeout, loại QR (tĩnh/động), retry OTP. Có lựa chọn Recommended. Có vòng lặp xác nhận. | Câu hỏi đúng trọng tâm VietQR, không hỏi thừa (VD: không hỏi eKYC), có mandatory feedback loop |
| **Phase 3** | Sinh BRD đầy đủ: Glossary (VietQR, NAPAS, EMVCo, CRC-16, DIP, DBS...), Matrix Table ≥ 7 bước, Mermaid sequenceDiagram, ≥ 5 popup lỗi, Data Mapping Request/Response. | Matrix Table có đủ 3 cột, popup có Title+Content+CTA, Mermaid có đủ actors |
| **Phase 4** | Validation Report 20-point, đạt ≥ 18/20. | Có report table, ≥ 18 ✅ |

### Scoring Rubric (20 tiêu chí × 5 điểm = 100 điểm)

#### A. Phase 1 Compliance (5 tiêu chí × 5đ = 25đ)

| # | Tiêu chí | 5đ (✅) | 2.5đ (⚠) | 0đ (❌) |
| :---: | :--- | :--- | :--- | :--- |
| A1 | User Story format đúng 3 dòng, persona cụ thể "KH ETB đã kích hoạt Digibank" | Đúng cả 3 dòng + persona cụ thể | Đúng format nhưng persona generic | Thiếu dòng hoặc sai format |
| A2 | INVEST self-check hiển thị (bảng 6 tiêu chí) | Hiện bảng 6/6, đạt ≥ 5 | Có đề cập INVEST nhưng không show bảng | Không nhắc INVEST |
| A3 | Scope In/Out rõ ràng | In ≥ 2 luồng + Out ≥ 1 luồng | Có nhưng < 2 In hoặc thiếu Out | Không có scope |
| A4 | Actors xác định đủ | Primary + ≥ 2 Secondary (NAPAS, Core, DIP) | Có Primary nhưng thiếu Secondary | Không xác định actors |
| A5 | Template mapping đúng | Chọn `transaction_lending` + có lý do | Chọn đúng nhưng không giải thích | Chọn sai template |

#### B. Phase 2 Quality (3 tiêu chí × 5đ = 15đ)

| # | Tiêu chí | 5đ (✅) | 2.5đ (⚠) | 0đ (❌) |
| :---: | :--- | :--- | :--- | :--- |
| B1 | Hỏi đúng trọng tâm VietQR (hạn mức, xác thực, timeout, QR type) | ≥ 3 câu hỏi đúng domain | 1-2 câu đúng domain | Hỏi lạc đề hoặc không hỏi |
| B2 | Format câu hỏi chuẩn (trắc nghiệm + Recommended + cú pháp nhanh) | Đủ 3 yếu tố format | Có trắc nghiệm nhưng thiếu Recommended | Free-form hoặc không hỏi |
| B3 | Mandatory Feedback Loop trước khi draft | Có câu chốt hỏi xác nhận trước Phase 3 | Hỏi nhưng không đợi trả lời | Nhảy thẳng Phase 3 |

#### C. BRD Content — Matrix Table (5 tiêu chí × 5đ = 25đ)

| # | Tiêu chí | 5đ (✅) | 2.5đ (⚠) | 0đ (❌) |
| :---: | :--- | :--- | :--- | :--- |
| C1 | Matrix Table ≥ 7 bước (Mở QR → Quét → Giải mã → Inquiry NAPAS → Nhập tiền → Xác thực → Chuyển tiền → Kết quả) | ≥ 7 bước đầy đủ | 5-6 bước | < 5 bước |
| C2 | Mỗi bước có đủ 3 cột (User Action / System Logic / Result) | 100% bước đủ 3 cột | ≥ 70% bước đủ | < 70% |
| C3 | System Check đầy đủ: Root/JB, CRC-16, Velocity, Face Authen QĐ 2345, OTP retry | ≥ 4/5 checks | 3/5 checks | ≤ 2/5 checks |
| C4 | Logic QR Tĩnh vs QR Động (khóa/mở ô số tiền) | Phân biệt rõ Static/Dynamic + lock/unlock logic | Có nhắc nhưng không rõ lock/unlock | Không phân biệt |
| C5 | Exception Path (NAPAS lỗi, timeout 30s → Reversal, OTP sai 3 lần → khóa 2h) | ≥ 3 exception paths đầy đủ | 1-2 exception paths | Không có exception |

#### D. UI Copy & Popup + Mermaid (4 tiêu chí × 5đ = 20đ)

| # | Tiêu chí | 5đ (✅) | 2.5đ (⚠) | 0đ (❌) |
| :---: | :--- | :--- | :--- | :--- |
| D1 | ≥ 5 popup lỗi (ERR_INVALID_QR, ERR_INVALID_CRC, ERR_INQUIRY_FAILED, ERR_LIMIT/BALANCE, ERR_FACE_AUTH, ERR_OTP_LOCKED, ERR_TRANS_TIMEOUT) | ≥ 5 popup đúng format | 3-4 popup | < 3 popup |
| D2 | Mỗi popup có Title + Content (dynamic variables) + CTA (nút + điều hướng) | 100% popup đủ 3 phần | ≥ 70% đủ | < 70% |
| D3 | Mermaid sequenceDiagram tồn tại với đủ actors (KH, App, DBS, DIP, NAPAS, Core) | Đủ ≥ 5 actors + autonumber | Có sơ đồ nhưng thiếu actors | Không có sơ đồ |
| D4 | Mermaid nhất quán với Matrix Table (thứ tự bước khớp) | Khớp 100% | Khớp ≥ 70% | Sai lệch nhiều |

#### E. Data & Validation (3 tiêu chí × 5đ = 15đ)

| # | Tiêu chí | 5đ (✅) | 2.5đ (⚠) | 0đ (❌) |
| :---: | :--- | :--- | :--- | :--- |
| E1 | Data Mapping Request Schema (≥ 8 trường: CIF, source_account, BIN, beneficiary_account, amount, description, qr_type, otp_signature) | ≥ 8 trường với đủ Type + M/O + Source | 5-7 trường | < 5 trường |
| E2 | Data Mapping Response Schema (≥ 4 trường: response_code, reference_id, balance, timestamp) | ≥ 4 trường | 2-3 trường | < 2 trường |
| E3 | Phase 4 Validation Report hiển thị, đạt ≥ 18/20 | Report đầy đủ 20 tiêu chí + ≥ 18 ✅ | Report có nhưng < 18 ✅ | Không có report |

---

## TEST CASE 2: Input mơ hồ — Thiếu thông tin, buộc Agent hỏi lại

### TC-02 Meta

| Trường | Giá trị |
| :--- | :--- |
| **ID** | TC-02 |
| **Tên** | VietQR — Vague Input, Force Clarification |
| **Mục đích** | Kiểm tra Agent có tự bịa thông tin không, hay bắt buộc hỏi lại khi input thiếu |
| **Độ khó** | ★★★☆☆ (Medium) |
| **Domain** | Daily Banking → Payment → QR Transfer |
| **Template kỳ vọng** | Agent phải hỏi thêm trước khi chọn template |

### Input Prompt

```text
Làm tính năng quét QR chuyển tiền trên app cho tôi.
```

### Expected Behavior theo Phase

| Phase | Agent phải làm | Tiêu chí đánh giá |
| :--- | :--- | :--- |
| **Phase 1** | KHÔNG viết User Story ngay. Phải nhận diện input thiếu ≥ 3/4 thông tin bắt buộc (Persona chỉ có "tôi" → generic, Goal mơ hồ "quét QR chuyển tiền" → chưa rõ chuẩn nào, Business Value → hoàn toàn thiếu, Context → chưa rõ phân hệ). Agent PHẢI hỏi lại. | Agent KHÔNG tự bịa persona/value/scope |
| **Phase 2** | Hỏi ≥ 4 câu: (1) Đối tượng KH? ETB hay NTB? (2) Chuẩn QR nào? VietQR NAPAS hay QR Payment? (3) Mục tiêu kinh doanh? (4) Phạm vi tính năng? Có tạo QR không? | Câu hỏi bao phủ 4 thông tin bắt buộc thiếu |
| **Phase 3** | Chỉ draft sau khi nhận đủ thông tin + qua feedback loop | Không nhảy thẳng draft |
| **Phase 4** | Validation Report đầy đủ | Đạt ≥ 18/20 |

### Suggested Answers cho Phase 2

```text
1) Đối tượng: Khách hàng ETB đã kích hoạt Digibank
2) Chuẩn QR: VietQR theo NAPAS, hỗ trợ QR tĩnh + động
3) Mục tiêu: Giảm thời gian chuyển khoản, giảm sai sót nhập tay
4) Phạm vi: Chỉ quét QR để chuyển khoản, không tạo QR. Tuân thủ QĐ 2345.
```

### Scoring Rubric (10 tiêu chí × 10đ = 100đ)

| # | Tiêu chí | 10đ (✅) | 5đ (⚠) | 0đ (❌) |
| :---: | :--- | :--- | :--- | :--- |
| F1 | Agent KHÔNG viết User Story ngay khi nhận input mơ hồ | Dừng lại + nêu rõ thiếu gì | Viết US nhưng ghi ⚠ cần xác nhận | Viết US luôn không hỏi |
| F2 | Agent nhận diện ≥ 3/4 thông tin thiếu | Nêu rõ ≥ 3 thiếu | Nêu 2 thiếu | Nêu ≤ 1 |
| F3 | Câu hỏi Phase 2 bao phủ persona, chuẩn QR, business value, scope | ≥ 4 câu bao phủ | 2-3 câu | ≤ 1 câu |
| F4 | Format câu hỏi chuẩn (trắc nghiệm + Recommended) | Đủ format | Có trắc nghiệm thiếu Recommended | Free-form |
| F5 | Mandatory Feedback Loop | Có câu chốt xác nhận | Hỏi nhưng không đợi | Nhảy thẳng draft |
| F6 | Sau khi nhận answer → viết User Story INVEST đúng | US đúng INVEST + persona cụ thể | US đúng format nhưng INVEST không check | US sai hoặc không viết |
| F7 | Template chọn đúng `transaction_lending` | Đúng + có lý do | Đúng nhưng không giải thích | Sai template |
| F8 | BRD có Matrix Table ≥ 7 bước | ≥ 7 bước đầy đủ | 5-6 bước | < 5 |
| F9 | BRD có ≥ 5 popup đúng format | ≥ 5 popup | 3-4 popup | < 3 |
| F10 | Phase 4 Validation Report ≥ 18/20 | ≥ 18 ✅ | 15-17 ✅ | < 15 |

---

## TEST CASE 3: Edge Case — Input nhầm domain (Onboarding keyword trong VietQR)

### TC-03 Meta

| Trường | Giá trị |
| :--- | :--- |
| **ID** | TC-03 |
| **Tên** | VietQR — Domain Confusion (Onboarding keywords mixed in) |
| **Mục đích** | Kiểm tra Agent phân loại template đúng khi input chứa keyword gây nhiễu |
| **Độ khó** | ★★★★☆ (Hard) |
| **Domain** | Daily Banking → Payment (KHÔNG phải Onboarding) |
| **Template kỳ vọng** | `brd_template_transaction_lending.md` (KHÔNG phải onboarding) |

### Input Prompt

```text
Tạo BRD cho tính năng quét mã VietQR chuyển khoản. Lưu ý: khách hàng mới đăng ký tài khoản online (NTB) vừa hoàn thành eKYC lần đầu cũng cần sử dụng được tính năng này. Đảm bảo xác thực sinh trắc học theo QĐ 2345 cho giao dịch vượt hạn mức.
```

### Expected Behavior theo Phase

| Phase | Agent phải làm | Tiêu chí đánh giá |
| :--- | :--- | :--- |
| **Phase 1** | Nhận diện đúng: Tính năng chính = Chuyển khoản QR (Transaction domain), việc NTB/eKYC chỉ là **điều kiện đầu vào** (precondition), KHÔNG phải nghiệp vụ Onboarding. Chọn đúng `transaction_lending`. | KHÔNG chọn nhầm `onboarding` template |
| **Phase 2** | Hỏi: (1) NTB mới eKYC có hạn mức khác ETB không? (2) NTB có cần Face Authen ngay từ giao dịch đầu tiên? (3) Có giới hạn thời gian sau eKYC mới được dùng QR? | Câu hỏi phân biệt được NTB precondition vs Onboarding domain |
| **Phase 3** | BRD viết cho Chuyển khoản QR. Scope ghi rõ: "Hỗ trợ cả ETB và NTB đã hoàn thành eKYC". Out-of-Scope: "Luồng eKYC/mở tài khoản (xem BRD Onboarding)" | Scope rõ ràng, không bị domain creep |
| **Phase 4** | Validation Report, V5 (Scope nhất quán) phải ✅ | Không có content vượt scope |

### Suggested Answers cho Phase 2

```text
1) NTB mới eKYC: Áp dụng cùng hạn mức gói mặc định như ETB, không phân biệt
2) Face Authen: Tuân theo QĐ 2345 chung cho cả ETB và NTB, không thêm lớp kiểm tra riêng
3) Thời gian: Không giới hạn, NTB dùng được QR ngay sau khi kích hoạt app thành công
```

### Scoring Rubric (10 tiêu chí × 10đ = 100đ)

| # | Tiêu chí | 10đ (✅) | 5đ (⚠) | 0đ (❌) |
| :---: | :--- | :--- | :--- | :--- |
| G1 | Agent chọn đúng template `transaction_lending` (KHÔNG phải onboarding) | Đúng + giải thích lý do | Đúng nhưng không giải thích | Chọn nhầm onboarding |
| G2 | Agent nhận diện NTB/eKYC là precondition, không phải core scope | Nêu rõ precondition vs scope | Nhắc nhưng không tách rõ | Gộp eKYC vào scope chính |
| G3 | Hỏi câu hỏi phân biệt NTB vs ETB trong context chuyển khoản | ≥ 2 câu hỏi đúng NTB context | 1 câu | Không hỏi gì về NTB |
| G4 | User Story viết cho chuyển khoản QR, persona bao gồm "ETB và NTB đã kích hoạt app" | Persona đúng + rõ ràng | Persona chỉ nói ETB hoặc chỉ nói NTB | Persona generic |
| G5 | Scope In ghi rõ "hỗ trợ ETB + NTB"; Scope Out ghi "luồng eKYC/mở TK" | Cả In + Out đúng | Có In đúng nhưng thiếu Out | Scope không phân biệt |
| G6 | Matrix Table không chứa bước eKYC/mở tài khoản | Không có bước OOScope | Có nhắc nhưng ghi "xem BRD khác" | Có cả luồng eKYC trong matrix |
| G7 | System Check có Face Authen QĐ 2345 áp dụng chung cho cả ETB+NTB | Ghi rõ áp dụng chung | Có FA nhưng không nói rõ ETB/NTB | Không có FA |
| G8 | Mermaid diagram KHÔNG có luồng eKYC | Không có | Có nhắc nhưng skip | Có luồng eKYC đầy đủ |
| G9 | Glossary có NTB, ETB, eKYC (dù chỉ là precondition) | Đủ 3 thuật ngữ | 2/3 | ≤ 1 |
| G10 | Phase 4 Validation V20 (cross-check scope): không có nội dung vượt scope | ✅ V20 | ⚠ V20 | ❌ V20 |

---

## TEST CASE 4: Stress Test — Yêu cầu rất chi tiết, kiểm tra Agent xử lý thông tin dư

### TC-04 Meta

| Trường | Giá trị |
| :--- | :--- |
| **ID** | TC-04 |
| **Tên** | VietQR — Overloaded Input with Technical Details |
| **Mục đích** | Kiểm tra Agent xử lý tốt khi input quá nhiều chi tiết kỹ thuật lẫn nghiệp vụ |
| **Độ khó** | ★★★★☆ (Hard) |
| **Domain** | Daily Banking → Payment → QR Transfer |
| **Template kỳ vọng** | `brd_template_transaction_lending.md` |

### Input Prompt

```text
Viết BRD cho tính năng quét QR chuyển khoản VietQR trên MSB Digibank.

Chi tiết kỹ thuật:
- Chuỗi QR theo EMVCo TLV format, kiểm tra CRC-16 CCITT (polynomial 0x1021, seed 0xFFFF)
- Tag 00 = 01 (Payload Format), Tag 01 = 11 (Static) hoặc 12 (Dynamic)
- Tag 38 chứa sub-tags: 38.00 = A000000727 (NAPAS GUID), 38.01 = BIN 6 số, 38.02 = Số TK thụ hưởng
- Tag 53 = 704 (VND), Tag 54 = Số tiền (chỉ có ở Dynamic QR), Tag 58 = VN
- Tag 62.08 = Nội dung chuyển khoản (max 70 ký tự không dấu)
- Tag 63 = CRC-16 checksum 4 hex chars

Nghiệp vụ:
- KH ETB đã kích hoạt app, quét QR tĩnh (nhập tiền) hoặc QR động (lock tiền)
- Gọi NAPAS Inquiry để lấy tên chủ TK thụ hưởng
- Hạn mức: 2,000 VND tối thiểu, 499,999,999 VND/GD tối đa, hạn mức gói IBMB
- Face Authen QĐ 2345: > 10tr/lần hoặc tích lũy > 20tr/ngày
- Smart OTP xác thực, sai 3 lần khóa 2 giờ
- NAPAS timeout 30s → auto Reversal
- Push Noti + SMS Banking biến động số dư

Không bao gồm: tạo QR cá nhân, QR thanh toán thẻ quốc tế, QR ví điện tử.
```

### Expected Behavior theo Phase

| Phase | Agent phải làm | Tiêu chí đánh giá |
| :--- | :--- | :--- |
| **Phase 1** | Xử lý input dài: tách phần kỹ thuật → để Mục 5.1 (VietQR Technical Spec), tách phần nghiệp vụ → Matrix Table, tách Out-of-Scope → Section 3.3. Viết US INVEST đúng. Gate Check 6/6 có thể pass mà KHÔNG cần hỏi thêm nhiều. | Agent không bị choáng bởi input dài, phân loại đúng thông tin |
| **Phase 2** | Hỏi ít (0-2 câu) vì input đã rất đầy đủ. Hoặc chỉ hỏi xác nhận. Vẫn phải có Feedback Loop. | Không hỏi thừa những gì đã cung cấp |
| **Phase 3** | BRD phải có section VietQR Technical Spec (bảng Tag EMVCo) riêng biệt. Data Mapping đầy đủ ≥ 10 trường Request. Matrix Table ≥ 8 bước chi tiết. ≥ 7 popup lỗi. Noti strategy (Push + SMS). | Chất lượng BRD phải gần bằng Baseline (VietQR Sample) |
| **Phase 4** | Validation Report 20/20 hoặc 19/20 | Gần perfect score |

### Suggested Answers cho Phase 2

```text
defaults (chọn tất cả Recommended nếu Agent có hỏi)
```

### Scoring Rubric (10 tiêu chí × 10đ = 100đ)

| # | Tiêu chí | 10đ (✅) | 5đ (⚠) | 0đ (❌) |
| :---: | :--- | :--- | :--- | :--- |
| H1 | Agent tách đúng phần kỹ thuật EMVCo → section riêng (không trộn vào Matrix Table) | Có section VietQR Technical Spec riêng biệt | Có bảng Tag nhưng nằm trong Matrix Table | Không có bảng Tag EMVCo |
| H2 | Bảng Tag EMVCo đầy đủ ≥ 10 tags (00, 01, 38, 38.00, 38.01, 38.02, 53, 54, 58, 62.08, 63) | ≥ 10 tags | 7-9 tags | < 7 tags |
| H3 | Agent KHÔNG hỏi lại những thông tin đã cung cấp trong input | 0 câu hỏi trùng lặp | 1 câu trùng | ≥ 2 câu trùng |
| H4 | Agent vẫn có Feedback Loop dù input đầy đủ | Có câu chốt xác nhận | Hỏi nhưng ngắn | Nhảy thẳng draft |
| H5 | Matrix Table ≥ 8 bước (thêm bước Noti so với TC-01) | ≥ 8 bước | 6-7 bước | < 6 bước |
| H6 | ≥ 7 popup lỗi (thêm ERR_DEVICE_UNSECURE, ERR_BENEFICIARY_BLOCKED so với TC-01) | ≥ 7 popup | 5-6 popup | < 5 popup |
| H7 | Data Mapping Request ≥ 10 trường (thêm raw_qr_payload, face_auth_token) | ≥ 10 trường | 8-9 trường | < 8 trường |
| H8 | Có Noti Strategy section (Push Noti + SMS + Reversal noti) | ≥ 3 loại noti | 2 loại | ≤ 1 loại |
| H9 | Glossary ≥ 10 thuật ngữ (VietQR, NAPAS, EMVCo, CRC-16, CIF, TKTT, DBS, DIP, FA, HMGDT, Static/Dynamic QR) | ≥ 10 | 7-9 | < 7 |
| H10 | Phase 4 Validation ≥ 19/20 ✅ | ≥ 19 | 18 | < 18 |

---

## TEST CASE 5: Negative Test — Agent KHÔNG được tạo BRD khi yêu cầu ngoài scope

### TC-05 Meta

| Trường | Giá trị |
| :--- | :--- |
| **ID** | TC-05 |
| **Tên** | VietQR — Out-of-Scope Request (QR Payment, not Transfer) |
| **Mục đích** | Kiểm tra Agent nhận diện đúng Out-of-Scope và từ chối / hỏi lại thay vì tạo BRD sai domain |
| **Độ khó** | ★★★☆☆ (Medium) |
| **Domain** | KHÔNG THUỘC scope VietQR Transfer |
| **Template kỳ vọng** | Agent phải hỏi lại hoặc tạo BRD riêng cho QR Payment |

### Input Prompt

```text
Viết BRD cho tính năng quét mã VietQR thanh toán tại cửa hàng (Merchant QR Payment). Khách hàng quét mã QR của merchant để thanh toán tiền hàng hóa/dịch vụ, trừ tiền từ TKTT.
```

### Expected Behavior theo Phase

| Phase | Agent phải làm | Tiêu chí đánh giá |
| :--- | :--- | :--- |
| **Phase 1** | Nhận diện: Đây là QR **Payment** (thanh toán hàng hóa tại POS), KHÔNG phải QR **Transfer** (chuyển khoản P2P). Agent phải xác nhận lại với user: "Đây là nghiệp vụ QR Payment (Merchant-Presented), khác với QR Transfer (P2P). Em sẽ tạo BRD riêng cho QR Payment." | Agent phân biệt rõ Payment vs Transfer |
| **Phase 2** | Hỏi thêm: (1) Merchant acquiring qua NAPAS hay đối tác nào? (2) Có refund/void không? (3) Hạn mức thanh toán? | Câu hỏi đúng cho QR Payment domain |
| **Phase 3** | Nếu user xác nhận, tạo BRD cho QR Payment (khác BRD VietQR Transfer) | BRD đúng domain Payment |
| **Phase 4** | Validation Report | Đạt chuẩn |

### Scoring Rubric (5 tiêu chí × 20đ = 100đ)

| # | Tiêu chí | 20đ (✅) | 10đ (⚠) | 0đ (❌) |
| :---: | :--- | :--- | :--- | :--- |
| I1 | Agent nhận diện đây là QR Payment, KHÔNG phải QR Transfer | Nêu rõ sự khác biệt + hỏi xác nhận | Nhận diện nhưng không giải thích | Coi như QR Transfer bình thường |
| I2 | Agent KHÔNG copy luồng VietQR Transfer vào BRD QR Payment | Tạo luồng riêng phù hợp Merchant QR | Copy một phần luồng Transfer | Copy nguyên BRD Transfer |
| I3 | Matrix Table có bước Merchant-specific (kiểm tra merchant, tính phí acquiring, receipt) | ≥ 2 bước merchant-specific | 1 bước | Không có |
| I4 | Scope phân biệt rõ: In = QR Payment at POS; Out = QR Transfer P2P | Cả In + Out đúng | Có In thiếu Out | Không phân biệt |
| I5 | Glossary có thuật ngữ Payment-specific (Merchant, POS, Acquiring, MDR...) | ≥ 3 thuật ngữ mới | 1-2 | Dùng lại glossary Transfer |

---

## TEST CASE 6: Regression Test — Kiểm tra Agent đọc đúng Writing Guide

### TC-06 Meta

| Trường | Giá trị |
| :--- | :--- |
| **ID** | TC-06 |
| **Tên** | VietQR — Writing Guide Compliance Check |
| **Mục đích** | Kiểm tra BRD output tuân thủ các quy chuẩn trong `po_writing_guide_for_ai_agents.md` |
| **Độ khó** | ★★★☆☆ (Medium) |
| **Domain** | Daily Banking → Payment → QR Transfer |
| **Template kỳ vọng** | `brd_template_transaction_lending.md` |

### Input Prompt

```text
Viết BRD cho tính năng quét VietQR chuyển khoản liên ngân hàng trên MSB Digibank.
KH cá nhân ETB, quét QR tĩnh/động, chuyển tiền qua NAPAS 24/7.
Hạn mức theo gói dịch vụ, Face Authen theo QĐ 2345, Smart OTP xác thực.
```

### Suggested Answers cho Phase 2

```text
defaults
```

### Scoring Rubric — Chỉ chấm Writing Guide compliance (10 tiêu chí × 10đ = 100đ)

| # | Tiêu chí (theo Writing Guide) | 10đ (✅) | 5đ (⚠) | 0đ (❌) |
| :---: | :--- | :--- | :--- | :--- |
| J1 | **Glossary chuẩn**: Dùng đúng thuật ngữ từ Writing Guide (TKTT, DBS, DIP, CIF...) | ≥ 8 thuật ngữ đúng từ Guide | 5-7 | < 5 |
| J2 | **Matrix Table đúng format 3 cột**: Thao tác NSD / Logic hệ thống & Business Rules / Kết quả & Chuyển bước | 100% bước đúng 3 cột | ≥ 80% | < 80% |
| J3 | **System Check — Root/Jailbreak**: Bước đầu tiên có kiểm tra thiết bị root/jailbreak | Có ở bước 1 của Matrix | Có nhưng ở bước khác | Không có |
| J4 | **System Check — Batch Time**: Có nhắc đến kiểm tra thời gian batch ngân hàng (nếu applicable) hoặc ghi rõ "N/A — GD 24/7" | Có check hoặc ghi N/A với lý do | Không nhắc | Sai |
| J5 | **System Check — OTP retry**: Có giới hạn số lần nhập OTP + hành động khi vượt | Đúng (3 lần + khóa 2h) | Có giới hạn nhưng không nêu hành động | Không có |
| J6 | **System Check — Velocity Limits**: Kiểm tra hạn mức giao dịch ngày/lần | Có + nêu ngưỡng cụ thể | Có nhưng không nêu ngưỡng | Không có |
| J7 | **System Check — Face Authen QĐ 2345**: Có ngưỡng >10tr/lần, >20tr/ngày | Đúng 2 ngưỡng | Có 1 ngưỡng | Không có |
| J8 | **UI Copy chuẩn**: Popup có Title + Content (biến dynamic `{variable}`) + CTA | 100% popup đúng format | ≥ 70% | < 70% |
| J9 | **Data Mapping chuẩn**: Bảng có cột Tên trường / Mã trường / Kiểu / M-O / Nguồn / Mô tả | Đủ 6 cột | 4-5 cột | < 4 cột |
| J10 | **Mermaid chuẩn**: sequenceDiagram hoặc flowchart, có autonumber, có rect cho logic block | Có autonumber + rect | Có sơ đồ nhưng thiếu autonumber/rect | Không có sơ đồ |

---

## BẢNG TỔNG HỢP KẾT QUẢ EVAL

| Test Case | Mục đích | Điểm (/100) | Kết quả | Ghi chú |
| :--- | :--- | :---: | :---: | :--- |
| **TC-01** | Happy Path — Full Input | /100 | PASS/FAIL | |
| **TC-02** | Vague Input — Force Clarification | /100 | PASS/FAIL | |
| **TC-03** | Domain Confusion — Template Selection | /100 | PASS/FAIL | |
| **TC-04** | Overloaded Input — Stress Test | /100 | PASS/FAIL | |
| **TC-05** | Out-of-Scope — QR Payment vs Transfer | /100 | PASS/FAIL | |
| **TC-06** | Writing Guide Compliance | /100 | PASS/FAIL | |
| **TỔNG** | **Average Score** | **/100** | | |

### Tiêu chí đánh giá tổng thể Skill

| Mức | Average Score | Đánh giá |
| :---: | :---: | :--- |
| 🟢 **Excellent** | ≥ 90 | Skill sẵn sàng production, output gần bằng Senior BA |
| 🟡 **Good** | 80–89 | Skill hoạt động tốt, cần tinh chỉnh minor |
| 🟠 **Needs Work** | 70–79 | Output có lỗ hổng, cần sửa skill instructions |
| 🔴 **Critical** | < 70 | Skill chưa đủ chất lượng, cần redesign workflow |
