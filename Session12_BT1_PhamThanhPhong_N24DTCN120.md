# BÀI TẬP 01: Phân tích yêu cầu nghiệp vụ mở tài khoản ngân hàng trực tuyến (eKYC) bằng Prompt

* **Họ và tên:** Phạm Thanh Phong
* **Mã sinh viên:** N24DTCN120
* **Lớp:** IT212 - AI Application in Action

---

## 1. Tình huống nghiệp vụ

Ngân hàng số **ABC Bank** muốn số hóa hoàn toàn quy trình mở thẻ qua **eKYC (Electronic Know Your Customer)**. Luồng cơ bản: Tải app → Đăng ký thông tin cơ bản → Chụp ảnh 2 mặt CCCD → Quét khuôn mặt (Liveness check) → Hệ thống đối chiếu dữ liệu → Cấp số tài khoản tự động nếu hợp lệ. Mục tiêu cốt lõi: **"Zero manual operation"** — giảm thiểu tối đa sự can thiệp của giao dịch viên.

---

## FILE 01 — Toàn bộ chuỗi Prompt đã sử dụng trên Antigravity

### Prompt 1 — Thiết lập vai trò & Phân tích hệ thống

```text
[Vai trò - Role]
Hãy đóng vai một Senior Business Analyst có 10 năm kinh nghiệm trong lĩnh vực FinTech/Ngân hàng số, từng tham gia triển khai nhiều dự án eKYC cho các ngân hàng số tại Việt Nam.

[Mục tiêu - Goal]
Phân tích nghiệp vụ quy trình mở tài khoản ngân hàng trực tuyến qua eKYC của ABC Bank, xuất ra đầy đủ 5 thành phần: Actors, Business Flow, Functional Requirements, Non-Functional Requirements, Assumptions & Business Rules — làm nền tảng để đội phát triển xây dựng hệ thống.

[Ngữ cảnh - Context]
- Ngân hàng: ABC Bank — ngân hàng số (digital bank), không có chi nhánh vật lý cho quy trình mở thẻ này.
- Luồng nghiệp vụ cơ bản: Khách hàng tải app → Đăng ký thông tin cơ bản → Chụp ảnh 2 mặt CCCD → Quét khuôn mặt (Liveness check) → Hệ thống đối chiếu dữ liệu → Cấp số tài khoản tự động nếu hợp lệ.
- Mục tiêu chiến lược cốt lõi: "Zero manual operation" — giảm thiểu tối đa sự can thiệp thủ công của giao dịch viên (teller) trong toàn bộ quy trình.
- Đây là nghiệp vụ tài chính - ngân hàng, chịu ràng buộc pháp lý về phòng chống rửa tiền (AML) và định danh khách hàng điện tử theo quy định của Ngân hàng Nhà nước.

[Ràng buộc - Constraints]
- Actors: liệt kê đầy đủ actor con người lẫn actor hệ thống/dịch vụ (kể cả actor ở chế độ nền như hệ thống chấm điểm rủi ro, cơ quan xác thực dữ liệu dân cư).
- Business Flow: trình bày dạng các bước tuần tự có đánh số, kèm mô tả input/output từng bước và nêu rõ bước nào có thể khiến luồng bị từ chối (rejection point).
- Functional Requirements: mã hóa theo FR-EKYC-XXX, đủ chi tiết để đội Dev implement, phải bao quát cả các bước OCR CCCD, Liveness Check, đối chiếu dữ liệu, cấp số tài khoản tự động.
- Non-Functional Requirements: phải có ít nhất các nhóm Bảo mật (Security), Hiệu năng (Performance), Khả dụng (Availability), Tuân thủ pháp lý (Compliance) — mỗi mục nêu chỉ tiêu cụ thể có thể đo lường được, mã hóa theo NFR-EKYC-XXX.
- Assumptions & Business Rules: nêu rõ các giả định về hạ tầng (đã tích hợp OCR/Liveness/dữ liệu dân cư từ bên thứ 3) và các quy tắc nghiệp vụ cứng của ngân hàng (ví dụ độ tuổi tối thiểu, giới hạn số lần thử lại xác thực khuôn mặt, ngưỡng điểm rủi ro tự động từ chối).

[Định dạng - Format]
Trình bày kết quả theo cấu trúc Markdown với 5 heading tương ứng 5 thành phần trên (## Actors, ## Business Flow, ## Functional Requirements, ## Non-Functional Requirements, ## Assumptions & Business Rules), dùng bảng hoặc danh sách có mã số rõ ràng cho từng mục.
```

### Prompt 2 — Chuyển đổi Artifacts (User Story & Use Case)

```text
[Vai trò - Role]
Tiếp tục vai trò Senior Business Analyst FinTech ở trên, giờ chuyển sang vai trò biên soạn Agile Artifacts từ bản phân tích nghiệp vụ đã có.

[Mục tiêu - Goal]
Dựa trên toàn bộ Actors, Business Flow, Functional Requirements đã phân tích ở lượt trước, chuyển đổi thành: (1) Danh sách User Story theo chuẩn "As a... I want to... So that...", (2) Danh sách Use Case tương ứng.

[Ngữ cảnh - Context]
- Giữ nguyên toàn bộ ngữ cảnh dự án eKYC của ABC Bank đã cung cấp ở Prompt 1.
- User Story và Use Case cần bao phủ đủ các actor chính: Khách hàng (End User), Hệ thống eKYC, Hệ thống chấm điểm rủi ro (Risk Engine), và (ở vai trò giám sát ngoại lệ) Giao dịch viên/Compliance Officer.

[Ràng buộc - Constraints]
- User Story: mỗi story phải gắn với đúng 1 actor, viết đúng khuôn mẫu "As a [actor], I want to [action], so that [benefit]", đánh mã US-EKYC-XXX, bao phủ cả luồng thành công lẫn ít nhất 2 luồng ngoại lệ (bị từ chối do CCCD không hợp lệ, bị từ chối do liveness check thất bại).
- Use Case: mỗi use case gồm Mã (UC-EKYC-XXX), Tên use case, Actor chính, Precondition, Main Flow (tóm tắt), Postcondition. Không cần vẽ sơ đồ, chỉ liệt kê dạng bảng/văn bản.
- Đảm bảo tính truy vết (traceability): mỗi User Story/Use Case nên tham chiếu tới đúng FR-EKYC-XXX tương ứng đã sinh ở Prompt 1.

[Định dạng - Format]
Trình bày 2 phần rõ ràng bằng heading Markdown: "## User Story" (dạng bảng: Mã | Actor | User Story | FR liên quan) và "## Use Case" (dạng bảng: Mã | Tên Use Case | Actor chính | Precondition | Main Flow tóm tắt | Postcondition).
```

---

## FILE 02 — Tài liệu kết quả phân tích nghiệp vụ (do AI sinh ra)

```text
# TÀI LIỆU PHÂN TÍCH NGHIỆP VỤ — QUY TRÌNH MỞ TÀI KHOẢN eKYC
## Ngân hàng: ABC Bank | Phiên bản: 1.0 | Ngày: 2026-07-16
```

### Actors

| Actor | Loại | Vai trò trong hệ thống |
|---|---|---|
| Khách hàng (End User) | Con người | Người thực hiện mở tài khoản qua app di động |
| Hệ thống eKYC App | Hệ thống | Giao diện di động thu thập thông tin, ảnh CCCD, khuôn mặt |
| OCR Engine (bên thứ 3) | Dịch vụ | Trích xuất dữ liệu từ ảnh CCCD (họ tên, số CCCD, ngày sinh...) |
| Liveness Detection Service | Dịch vụ | Xác minh khuôn mặt thật (chống giả mạo bằng ảnh/video) |
| Hệ thống Cơ sở dữ liệu Quốc gia về Dân cư (kết nối API) | Dịch vụ bên ngoài | Đối chiếu thông tin CCCD với dữ liệu dân cư chính thống |
| Risk Scoring Engine | Hệ thống nội bộ | Chấm điểm rủi ro hồ sơ (gian lận, trùng khớp danh sách đen AML) |
| Core Banking System | Hệ thống nội bộ | Sinh số tài khoản, khởi tạo hồ sơ khách hàng sau khi duyệt |
| Compliance Officer | Con người (giám sát ngoại lệ) | Xử lý các hồ sơ bị hệ thống gắn cờ nghi vấn (không nằm trong luồng chính "zero manual") |

### Business Flow

1. **Tải app & Đăng ký thông tin cơ bản:** Khách hàng nhập số điện thoại, email, tạo mật khẩu → hệ thống gửi OTP xác thực.
2. **Chụp ảnh 2 mặt CCCD:** Khách hàng chụp mặt trước/sau CCCD → OCR Engine trích xuất dữ liệu (họ tên, số CCCD, ngày sinh, ngày hết hạn).
   * *Rejection point:* Ảnh mờ/thiếu góc/CCCD hết hạn → từ chối, yêu cầu chụp lại.
3. **Quét khuôn mặt (Liveness Check):** Khách hàng thực hiện các cử chỉ theo hướng dẫn (chớp mắt, quay đầu) → Liveness Service xác minh là người thật, không phải ảnh/video giả mạo.
   * *Rejection point:* Liveness score dưới ngưỡng, hoặc quá số lần thử cho phép → từ chối tạm thời, chuyển hướng dẫn hỗ trợ.
4. **Đối chiếu dữ liệu:** Hệ thống so khớp (a) ảnh khuôn mặt vừa quét với ảnh trên CCCD (face matching), (b) thông tin CCCD với Cơ sở dữ liệu Quốc gia về Dân cư.
   * *Rejection point:* Face matching không khớp, hoặc thông tin CCCD không tồn tại/không khớp dữ liệu dân cư → từ chối.
5. **Chấm điểm rủi ro (Risk Scoring):** Risk Engine đối chiếu hồ sơ với danh sách đen AML/sanction list, tính điểm rủi ro.
   * *Rejection point:* Điểm rủi ro vượt ngưỡng cho phép, hoặc trùng khớp danh sách đen → từ chối tự động hoặc chuyển Compliance Officer xử lý thủ công (ngoại lệ hiếm, ngoài luồng "zero manual").
6. **Cấp số tài khoản tự động:** Nếu qua hết các bước trên, Core Banking System tự động sinh số tài khoản, kích hoạt tài khoản và gửi thông báo hoàn tất cho khách hàng.

### Functional Requirements

| Mã | Tên chức năng | Mô tả |
|---|---|---|
| FR-EKYC-001 | Đăng ký thông tin cơ bản & xác thực OTP | Thu thập số điện thoại/email, gửi và xác thực mã OTP trước khi cho phép tiếp tục |
| FR-EKYC-002 | Thu nhận & OCR ảnh CCCD | Cho phép chụp/tải ảnh 2 mặt CCCD, gọi OCR Engine trích xuất dữ liệu, kiểm tra chất lượng ảnh (độ nét, đủ 4 góc, không hết hạn) |
| FR-EKYC-003 | Liveness Check khuôn mặt | Hướng dẫn khách hàng thực hiện cử chỉ theo yêu cầu, gọi Liveness Service, giới hạn tối đa 3 lần thử |
| FR-EKYC-004 | Đối chiếu Face Matching | So khớp ảnh khuôn mặt vừa quét với ảnh trên CCCD, trả về độ tương đồng (similarity score) |
| FR-EKYC-005 | Đối chiếu dữ liệu dân cư | Gọi API Cơ sở dữ liệu Quốc gia về Dân cư để xác thực thông tin CCCD là hợp lệ và thuộc đúng chủ thể |
| FR-EKYC-006 | Chấm điểm rủi ro tự động | Gọi Risk Scoring Engine, đối chiếu danh sách đen AML/sanction, trả về điểm rủi ro và quyết định (APPROVE/REJECT/MANUAL_REVIEW) |
| FR-EKYC-007 | Cấp số tài khoản tự động | Nếu quyết định là APPROVE, tự động gọi Core Banking System sinh số tài khoản và kích hoạt, không cần giao dịch viên can thiệp |
| FR-EKYC-008 | Thông báo kết quả | Gửi thông báo (push notification/SMS/email) cho khách hàng ở mỗi trạng thái: đang xử lý, bị từ chối (kèm lý do), hoặc mở tài khoản thành công |

### Non-Functional Requirements

| Mã | Nhóm | Chỉ tiêu kỹ thuật |
|---|---|---|
| NFR-EKYC-001 | Bảo mật (Security) | Toàn bộ dữ liệu sinh trắc học (ảnh CCCD, khuôn mặt) phải được mã hóa end-to-end khi truyền và mã hóa at-rest (AES-256) khi lưu trữ |
| NFR-EKYC-002 | Bảo mật (Security) | Toàn bộ API kết nối tới Cơ sở dữ liệu Quốc gia về Dân cư và Risk Engine phải qua kênh riêng (private network/VPN), có xác thực mTLS |
| NFR-EKYC-003 | Hiệu năng (Performance) | Toàn bộ quy trình từ chụp CCCD đến khi có kết quả (approve/reject) phải hoàn tất trong ≤ 60 giây (p95) |
| NFR-EKYC-004 | Khả dụng (Availability) | Hệ thống eKYC phải đạt uptime tối thiểu 99.9%/tháng, có cơ chế fallback khi OCR/Liveness Service bên thứ 3 gián đoạn |
| NFR-EKYC-005 | Tuân thủ pháp lý (Compliance) | Toàn bộ log xử lý hồ sơ eKYC phải lưu trữ tối thiểu 5 năm theo quy định phòng chống rửa tiền (AML), có khả năng truy vết đầy đủ từng bước quyết định |
| NFR-EKYC-006 | Tuân thủ pháp lý (Compliance) | Hệ thống phải hỗ trợ audit trail cho mọi trường hợp bị từ chối tự động, ghi rõ lý do (mã lỗi) để phục vụ thanh tra khi cần |

### Assumptions & Business Rules

**Giả định (Assumptions):**
* Hạ tầng OCR Engine, Liveness Detection Service và API kết nối Cơ sở dữ liệu Quốc gia về Dân cư đã được ký hợp đồng tích hợp sẵn với nhà cung cấp bên thứ 3, không cần xây mới.
* Khách hàng sử dụng thiết bị di động có camera đạt độ phân giải tối thiểu để chụp CCCD và quét khuôn mặt.

**Quy tắc nghiệp vụ (Business Rules):**
* Khách hàng phải đủ **18 tuổi trở lên** tại thời điểm đăng ký (đối chiếu theo ngày sinh trên CCCD).
* Liveness Check được thử tối đa **3 lần**; quá số lần này, hồ sơ chuyển trạng thái "tạm khóa 24h" trước khi cho thử lại.
* Face Matching similarity score phải đạt tối thiểu **90%** mới được coi là khớp.
* Điểm rủi ro (Risk Score) vượt ngưỡng quy định hoặc trùng khớp danh sách đen AML/sanction → **tự động từ chối**, không cấp tài khoản trong mọi trường hợp (không có ngoại lệ thủ công để ghi đè).
* Mỗi số CCCD chỉ được liên kết với **duy nhất một tài khoản** đang hoạt động tại ABC Bank tại một thời điểm.
```

---

## FILE 02 — Artifacts chuyển đổi (User Story & Use Case)

```text
## User Story

| Mã | Actor | User Story | FR liên quan |
|---|---|---|---|
| US-EKYC-001 | Khách hàng | As a khách hàng, I want to đăng ký tài khoản bằng số điện thoại và xác thực OTP, so that tôi có thể bắt đầu quy trình mở thẻ mà không cần ra quầy. | FR-EKYC-001 |
| US-EKYC-002 | Khách hàng | As a khách hàng, I want to chụp ảnh 2 mặt CCCD ngay trên app, so that hệ thống tự động trích xuất thông tin của tôi mà không cần nhập tay. | FR-EKYC-002 |
| US-EKYC-003 | Khách hàng | As a khách hàng, I want to thực hiện quét khuôn mặt theo hướng dẫn, so that hệ thống xác minh tôi là người thật đang thực hiện giao dịch. | FR-EKYC-003 |
| US-EKYC-004 | Hệ thống eKYC | As a hệ thống eKYC, I want to tự động đối chiếu ảnh khuôn mặt với dữ liệu CCCD và dữ liệu dân cư, so that tôi có thể xác thực danh tính khách hàng mà không cần giao dịch viên kiểm tra thủ công. | FR-EKYC-004, FR-EKYC-005 |
| US-EKYC-005 | Risk Engine | As a Risk Scoring Engine, I want to tự động chấm điểm rủi ro và đối chiếu danh sách đen AML, so that ngân hàng ngăn chặn được gian lận/rửa tiền ngay từ bước mở tài khoản. | FR-EKYC-006 |
| US-EKYC-006 | Khách hàng | As a khách hàng, I want to nhận số tài khoản ngay sau khi hồ sơ được duyệt, so that tôi có thể sử dụng dịch vụ ngân hàng ngay lập tức mà không phải chờ đợi. | FR-EKYC-007, FR-EKYC-008 |
| US-EKYC-007 | Khách hàng | As a khách hàng, I want to nhận thông báo rõ lý do khi hồ sơ CCCD của tôi bị từ chối (ví dụ ảnh mờ, CCCD hết hạn), so that tôi biết cách khắc phục và thử lại. | FR-EKYC-002, FR-EKYC-008 |
| US-EKYC-008 | Khách hàng | As a khách hàng, I want to được thông báo khi Liveness Check thất bại và biết số lần thử còn lại, so that tôi không bị bất ngờ khi tài khoản tạm bị khóa sau nhiều lần thử sai. | FR-EKYC-003, FR-EKYC-008 |

## Use Case

| Mã | Tên Use Case | Actor chính | Precondition | Main Flow tóm tắt | Postcondition |
|---|---|---|---|---|---|
| UC-EKYC-001 | Đăng ký & xác thực OTP | Khách hàng | Khách hàng đã tải app, chưa có tài khoản | Nhập SĐT/email → nhận OTP → nhập OTP xác thực | Tài khoản đăng nhập tạm thời được tạo, sẵn sàng cho bước eKYC |
| UC-EKYC-002 | Thu nhận & OCR CCCD | Khách hàng | Đã hoàn tất UC-EKYC-001 | Chụp mặt trước/sau CCCD → hệ thống OCR trích xuất thông tin → hiển thị để khách hàng xác nhận | Thông tin CCCD (họ tên, số CCCD, ngày sinh...) được lưu tạm trong hồ sơ đăng ký |
| UC-EKYC-003 | Xác minh Liveness Check | Khách hàng | Đã hoàn tất UC-EKYC-002 | Thực hiện cử chỉ khuôn mặt theo hướng dẫn → hệ thống xác minh là người thật (tối đa 3 lần thử) | Kết quả Liveness (PASS/FAIL) được ghi nhận vào hồ sơ |
| UC-EKYC-004 | Đối chiếu danh tính tự động | Hệ thống eKYC | Liveness Check = PASS | Hệ thống so khớp khuôn mặt với ảnh CCCD; gọi API đối chiếu dữ liệu dân cư | Hồ sơ được gắn trạng thái "Đã xác thực danh tính" hoặc "Từ chối" |
| UC-EKYC-005 | Chấm điểm rủi ro & duyệt hồ sơ | Risk Scoring Engine | Hồ sơ đã ở trạng thái "Đã xác thực danh tính" | Đối chiếu danh sách đen AML, tính điểm rủi ro, ra quyết định APPROVE/REJECT/MANUAL_REVIEW | Hồ sơ chuyển sang trạng thái tương ứng quyết định |
| UC-EKYC-006 | Cấp số tài khoản tự động | Core Banking System | Hồ sơ ở trạng thái APPROVE | Tự động sinh số tài khoản, kích hoạt tài khoản, gửi thông báo cho khách hàng | Khách hàng có tài khoản ngân hàng hoạt động, không cần giao dịch viên can thiệp |
| UC-EKYC-007 | Xử lý hồ sơ nghi vấn (ngoại lệ) | Compliance Officer | Hồ sơ ở trạng thái MANUAL_REVIEW | Compliance Officer xem xét hồ sơ bị gắn cờ, ra quyết định thủ công cuối cùng | Hồ sơ được duyệt hoặc từ chối vĩnh viễn, ghi log audit trail |
```

---

## FILE 03 — Ảnh chụp màn hình minh họa (Screenshot)
