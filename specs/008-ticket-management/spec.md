# Feature Specification: Hệ thống bán vé sự kiện (User mua vé & CMS quản lý vé)

**Feature Branch**: `008-ticket-management`  
**Created**: 2026-05-13  
**Status**: Draft  
**Source**: BRD Module 1 (User mua vé) + BRD Module 2 (Admin quản lý vé)

---

## Tổng quan

Feature này bao gồm hai mảng chức năng liên kết chặt chẽ:

- **Module 1 – User mua vé**: Toàn bộ hành trình người mua từ tìm kiếm sự kiện, chọn ghế/hạng vé, giữ chỗ tạm thời, thanh toán trực tuyến, đến nhận vé điện tử qua email.
- **Module 2 – CMS quản lý vé**: Toàn bộ năng lực vận hành sự kiện từ phía admin: tạo sự kiện, cấu hình hạng vé/sơ đồ ghế, phát hành, theo dõi doanh thu, báo cáo, và quản lý vòng đời sự kiện.

---

## User Scenarios & Testing *(mandatory)*

### User Story 1 — Admin tạo và phát hành sự kiện (Priority: P1)

Quản trị viên sự kiện cần khởi tạo một sự kiện mới, cấu hình cơ cấu vé (hạng vé hoặc sơ đồ ghế), và phát hành để người mua có thể tìm kiếm và đặt vé.

**Why this priority**: Đây là điều kiện tiên quyết của toàn bộ hệ thống — không có sự kiện được phát hành thì không có giao dịch nào xảy ra.

**Independent Test**: Có thể kiểm thử độc lập bằng cách đăng nhập Admin Portal, tạo sự kiện với ít nhất một hạng vé, và nhấn "Phát hành" — sau đó xác nhận sự kiện xuất hiện trên kênh bán trong ≤ 60 giây.

**Acceptance Scenarios**:

1. **Given** admin đăng nhập với quyền tạo sự kiện, **When** nhập đầy đủ thông tin (tên, mô tả, thể loại, thời gian, địa điểm, đơn vị tổ chức, ảnh đại diện) và bấm Lưu, **Then** sự kiện được tạo ở trạng thái "Bản nháp" với mã sự kiện duy nhất và xuất hiện trong danh sách của admin.
2. **Given** sự kiện ở trạng thái Bản nháp, **When** admin cấu hình ít nhất một hạng vé (có giá ≥ 0, giới hạn số lượng, quy tắc bán) và lưu, **Then** cấu trúc tồn vé được khởi tạo đúng số lượng cho từng hạng/khu vực.
3. **Given** sự kiện có đủ thông tin bắt buộc và ít nhất một hạng vé hợp lệ, **When** admin bấm "Phát hành" và xác nhận, **Then** trạng thái chuyển thành "Đã phát hành" và sự kiện xuất hiện trên kênh bán cho người mua trong tối đa 60 giây.
4. **Given** sự kiện đang phát hành, **When** admin cố sửa trường nhạy cảm (thời gian bắt đầu) khi đã có giao dịch thành công, **Then** hệ thống hiển thị cảnh báo và yêu cầu xác nhận, mọi thay đổi cấu hình giá phải qua kiểm tra hai lớp.
5. **Given** admin muốn phát hành sự kiện có sức chứa ≥ 5.000 vé, **When** nhấn Phát hành, **Then** hệ thống yêu cầu phê duyệt hai lớp trước khi chuyển trạng thái.

---

### User Story 2 — Người dùng tìm kiếm và xem chi tiết sự kiện (Priority: P1)

Người dùng (kể cả chưa đăng nhập) có thể tìm kiếm sự kiện theo từ khóa, thể loại, địa điểm, khoảng thời gian và xem đầy đủ thông tin. Để mua vé, người dùng bắt buộc phải đăng nhập tài khoản.

**Why this priority**: Điểm vào đầu tiên của hành trình mua vé — nếu không tìm thấy sự kiện phù hợp, không có giao dịch nào được tạo ra.

**Independent Test**: Có thể kiểm thử độc lập bằng cách tìm kiếm theo tên sự kiện đã phát hành — kết quả đầu tiên phải là sự kiện đó; trang chi tiết tải đầy đủ trong ≤ 2 giây ở 4G.

**Acceptance Scenarios**:

1. **Given** hệ thống đang hoạt động và có sự kiện đã phát hành, **When** người mua nhập đúng tên sự kiện vào thanh tìm kiếm, **Then** kết quả đầu tiên là sự kiện đó, hiển thị tên, thời gian, địa điểm, khoảng giá, ảnh đại diện và trạng thái còn vé.
2. **Given** người mua không nhập từ khóa, **When** truy cập trang tìm kiếm, **Then** hệ thống hiển thị danh sách sự kiện nổi bật theo gợi ý.
3. **Given** người mua kết hợp nhiều bộ lọc (thể loại + địa điểm + khoảng thời gian), **When** thực hiện tìm kiếm, **Then** kết quả chỉ hiển thị sự kiện đáp ứng tất cả tiêu chí, không gây lỗi, thời gian phản hồi ≤ 1,5 giây với 95% truy vấn.
4. **Given** người mua mở trang chi tiết sự kiện, **When** trang tải xong, **Then** hiển thị đầy đủ: mô tả, lịch trình, địa điểm (kèm múi giờ địa phương), ban tổ chức, chính sách hoàn/đổi vé, danh sách hạng vé kèm giá và trạng thái tồn kho (trễ tối đa 5 giây so với thực tế).
5. **Given** sự kiện đã hủy hoặc chưa mở bán, **When** người mua xem chi tiết, **Then** nút "Mua vé" bị ẩn; nếu chưa mở bán hiển thị bộ đếm ngược.

---

### User Story 3 — Người dùng chọn vé, giữ chỗ và thanh toán (Priority: P1)

Người mua chọn ghế hoặc số lượng vé theo hạng, giữ chỗ trong 10 phút, hoàn tất thanh toán và nhận email xác nhận kèm vé điện tử QR.

**Why this priority**: Đây là core transaction flow — quyết định doanh thu và trải nghiệm người dùng.

**Independent Test**: Có thể kiểm thử end-to-end: chọn vé → giữ chỗ → thanh toán → nhận email với vé QR trong ≤ 60 giây sau khi thanh toán thành công.

**Acceptance Scenarios**:

1. **Given** người mua ở trang chi tiết sự kiện đang mở bán, **When** chọn số lượng vé theo hạng (hoặc chọn ghế trên sơ đồ), **Then** hệ thống hiển thị tổng tiền tạm tính kèm phí và thuế; không cho phép chọn ghế đã bán hoặc đang bị giữ.
2. **Given** người mua bấm "Tiếp tục" sau khi chọn vé, **When** hệ thống tạo bản ghi giữ chỗ, **Then** vé/ghế bị khóa tạm với TTL 10 phút, không người mua khác nào có thể chọn cùng ghế đó; bộ đếm ngược chính xác đến từng giây.
3. **Given** vé đang được giữ và chưa hết TTL, **When** người mua hoàn tất thanh toán qua cổng (thẻ quốc tế, ví điện tử, chuyển khoản nhanh), **Then** trạng thái đơn hàng phản ánh kết quả từ cổng thanh toán trong ≤ 30 giây; vé chuyển sang trạng thái "Đã bán" và mã QR được sinh ra.
4. **Given** thanh toán thành công, **When** hệ thống xử lý xác nhận, **Then** email xác nhận kèm PDF vé điện tử có mã QR được gửi đến người mua trong ≤ 60 giây; email có thể mở trên iOS, Android và desktop.
5. **Given** bộ đếm ngược về 0 trước khi thanh toán xong, **When** TTL hết hạn, **Then** vé được giải phóng tự động và sẵn sàng cho người mua khác trong ≤ 60 giây; người mua thấy thông điệp rõ ràng và được hướng dẫn chọn lại.
6. **Given** thanh toán thất bại hoặc cổng trả lỗi, **When** ngoại lệ xảy ra, **Then** tiền không bị trừ, người mua thấy thông điệp lỗi chính xác kèm hướng dẫn (thử lại / chọn lại / liên hệ CSKH).

---

### User Story 4 — Admin theo dõi doanh thu và báo cáo (Priority: P2)

Quản trị viên sự kiện, nhân viên kinh doanh và kế toán có thể xem dashboard doanh thu thời gian thực, tạo báo cáo theo nhu cầu và xuất dữ liệu để đối soát.

**Why this priority**: Cung cấp khả năng giám sát kinh doanh và đối soát tài chính — quan trọng nhưng không chặn luồng mua vé.

**Independent Test**: Có thể kiểm thử độc lập bằng cách mở Dashboard sau khi có ít nhất một giao dịch thành công — KPI phải cập nhật trong ≤ 60 giây; xuất báo cáo CSV/XLSX mở được trên Excel và Google Sheets.

**Acceptance Scenarios**:

1. **Given** có ít nhất một giao dịch thành công, **When** admin mở trang Dashboard, **Then** hiển thị đầy đủ KPI: tổng doanh thu, số vé bán, số đơn hàng, tỉ lệ hủy, trung bình giá vé; dữ liệu cập nhật trong ≤ 60 giây sau giao dịch.
2. **Given** admin có quyền hạn chế (chỉ quản lý một số sự kiện), **When** xem Dashboard, **Then** chỉ thấy dữ liệu thuộc phạm vi quản lý của mình.
3. **Given** nhân viên kinh doanh/kế toán muốn tạo báo cáo, **When** chọn loại báo cáo, cấu hình tham số (khoảng thời gian, bộ lọc, định dạng), **Then** hệ thống tạo báo cáo và cho phép tải xuống CSV hoặc XLSX; file mở được trên Excel/Google Sheets, không lỗi mã hóa tiếng Việt (UTF-8 BOM).
4. **Given** khối lượng dữ liệu lớn khi tạo báo cáo, **When** hệ thống cần xử lý lâu, **Then** chuyển sang chế độ chạy nền và gửi email khi hoàn tất; hỗ trợ tối đa 1.000.000 bản ghi/lần xuất.
5. **Given** báo cáo đối soát doanh thu, **When** so sánh với báo cáo của cổng thanh toán, **Then** sai số ≤ 0.01%.

---

### User Story 5 — Admin quản lý vòng đời sự kiện (Priority: P2)

Quản trị viên có thể tạm dừng bán vé, hủy sự kiện, kết thúc sự kiện, và xuất dữ liệu đối soát; mọi hành động nhạy cảm đều có kiểm tra hai lớp và ghi nhật ký kiểm toán.

**Why this priority**: Cần thiết cho vận hành nhưng ít tần suất hơn luồng chính.

**Independent Test**: Có thể kiểm thử bằng cách tạm dừng bán một sự kiện đang phát hành — trạng thái phải đồng bộ giữa Admin Portal và kênh bán trong ≤ 60 giây; mọi thay đổi xuất hiện trong nhật ký kiểm toán với người thực hiện và thời điểm.

**Acceptance Scenarios**:

1. **Given** sự kiện đang ở trạng thái Đã phát hành, **When** admin chọn "Tạm dừng bán" và nhập lý do (bắt buộc), **Then** trạng thái cập nhật và đồng bộ chính xác giữa Admin Portal và kênh bán trong ≤ 60 giây; lý do được lưu vào audit log và không hiển thị cho người mua; kênh bán chỉ hiển thị thông điệp chung "Tạm ngừng bán vé".
2. **Given** admin muốn hủy sự kiện đã có giao dịch, **When** chọn "Hủy sự kiện" và xác nhận, **Then** hệ thống gửi email thông báo hủy tới toàn bộ khách hàng đã mua vé; việc hoàn tiền được xử lý thủ công bởi team vận hành (ngoài phạm vi hệ thống).
3. **Given** admin muốn xuất dữ liệu, **When** chọn phạm vi (vé, đơn hàng, doanh thu) và định dạng, **Then** hệ thống tạo tệp CSV/XLSX với đường dẫn tải xuống có hết hạn ≤ 24 giờ và bảo vệ bằng mật khẩu (nếu chứa dữ liệu cá nhân).
4. **Given** admin thực hiện bất kỳ thao tác cấu hình nào, **When** lưu thay đổi, **Then** mọi thay đổi đều xuất hiện trong nhật ký kiểm toán với người thực hiện và thời điểm; nhật ký không thể chỉnh sửa và được lưu tối thiểu 24 tháng.

---

### Edge Cases

- Thanh toán webhook nhận được nhưng chữ ký sai: hệ thống ghi log và treo đơn hàng để rà soát thủ công trong vòng 24 giờ.
- Vé hết trong khi đang giữ chỗ: ưu tiên giữ vé cho người mua hiện tại theo thứ tự FIFO.
- Một người mua không được giữ đồng thời quá 2 đơn cho cùng một sự kiện.
- Cổng thanh toán trả lỗi tạm thời: cho phép thử lại trong thời gian giữ chỗ còn lại.
- Email bị bounce do địa chỉ sai: ghi log, gắn cờ đơn hàng và thông báo CSKH.
- Email service gặp sự cố (không phải bounce): hệ thống retry tối đa 3 lần với exponential backoff trong vòng 30 phút; nếu vẫn thất bại thì gắn cờ đơn hàng và thông báo CSKH xử lý thủ công.
- Khi hủy sự kiện đã phát hành: không được xóa hoàn toàn; chỉ thay đổi trạng thái; hoàn tiền xử lý thủ công bởi team vận hành; toàn bộ QR code đã phát hành bị vô hiệu hóa tự động.
- Khi admin "Tạm dừng bán": Hold hiện có được giữ nguyên (người đang giữ chỗ vẫn có thể hoàn tất thanh toán); hệ thống chặn tạo Hold mới cho đến khi sự kiện được tiếp tục bán.
- Reporting Service trả lỗi khi xem Dashboard: hiển thị dữ liệu cache cuối cùng và cảnh báo.
- Tổng giới hạn vé không được vượt quá sức chứa địa điểm.
- Sơ đồ ghế lỗi định dạng (không phải JSON/SVG hợp lệ): hệ thống từ chối và yêu cầu chỉnh sửa.
- Đã có giao dịch: chỉ cho phép tăng giới hạn số lượng vé, không cho giảm xuống dưới mức đã bán.
- Người mua cố tạo Hold vượt cap 20 vé/user/event: hệ thống từ chối ngay tại bước Hold với thông điệp rõ ràng; không cho tiếp tục sang bước thanh toán.

---

## Requirements *(mandatory)*

### Functional Requirements

#### Module 1 — Người dùng mua vé

- **FR-001**: Hệ thống PHẢI cho phép mọi người dùng (kể cả chưa đăng nhập) tìm kiếm sự kiện theo từ khóa, thể loại, địa điểm và khoảng thời gian, và xem chi tiết sự kiện. Việc mua vé **bắt buộc đăng nhập tài khoản**; người chưa đăng nhập khi bấm "Mua vé" sẽ được chuyển đến màn hình đăng nhập/đăng ký.
- **FR-002**: Hệ thống PHẢI chỉ hiển thị sự kiện ở trạng thái "Đã phát hành" trong kết quả tìm kiếm; sự kiện hết vé vẫn hiển thị nhưng đánh dấu rõ ràng.
- **FR-003**: Hệ thống PHẢI hiển thị đầy đủ thông tin sự kiện trên trang chi tiết: mô tả, lịch trình, địa điểm (kèm múi giờ địa phương), chính sách hoàn/đổi vé, danh sách hạng vé kèm giá và tình trạng tồn kho.
- **FR-004**: Hệ thống PHẢI cho phép người mua chọn ghế cụ thể (sự kiện có sơ đồ ghế) hoặc số lượng theo hạng vé; không cho phép chọn ghế đã bán hoặc đang bị giữ. Cơ chế locking theo loại sự kiện: **(A) Sự kiện có sơ đồ ghế** — pessimistic locking hai giai đoạn: Pre-lock TTL 2 phút khi click ghế → Hold TTL 10 phút khi bấm "Tiếp tục". **(B) Sự kiện theo hạng vé** — không có pre-lock; Hold TTL 10 phút được tạo ngay khi bấm "Tiếp tục".
- **FR-005**: Hệ thống PHẢI giới hạn tối đa 20 vé/giao dịch với người mua đã đăng nhập và tối đa 20 vé/user/event (tính cộng dồn từ các đơn đã thanh toán thành công; đơn bị hủy không làm giảm con số này). Cả hai giới hạn được kiểm tra tại thời điểm tạo Hold — từ chối ngay nếu vượt, không chờ đến bước thanh toán. Mua vé bắt buộc đăng nhập — không hỗ trợ guest checkout.
- **FR-006**: Hệ thống PHẢI tạo bản ghi giữ chỗ (hold) với TTL 10 phút khi người mua bắt đầu thanh toán; không người mua khác nào có thể chọn cùng ghế/vé trong thời gian hold.
- **FR-006b**: Khi lượng người truy cập vượt ngưỡng tải sự kiện lớn, hệ thống PHẢI kích hoạt Virtual Waiting Room — xếp hàng người mua theo thứ tự đến trước (FIFO), hiển thị vị trí hàng đợi và thời gian ước tính; người dùng trong hàng được giữ phiên ổn định mà không cần tải lại trang. Ngưỡng kích hoạt được hardcode default trong backend; cấu hình per-event là việc tương lai (deferred).
- **FR-007**: Một người mua KHÔNG ĐƯỢC có quá 2 Hold chính thức (10 phút) đồng thời cho cùng một sự kiện; pre-lock không tính vào giới hạn này.
- **FR-008**: Hệ thống PHẢI xử lý thanh toán qua ít nhất ba phương thức: thẻ quốc tế, ví điện tử và chuyển khoản nhanh.
- **FR-009**: Hệ thống PHẢI xác minh chữ ký webhook từ cổng thanh toán trước khi xử lý kết quả; hệ thống PHẢI deduplicate theo transaction ID từ cổng — nếu transaction ID đã được xử lý thành công thì bỏ qua webhook trùng (idempotency).
- **FR-009b**: Hệ thống PHẢI áp dụng rate limiting trên các API endpoint mua vé — giới hạn theo per IP và per user để chống bot/scalping; không dùng CAPTCHA.
- **FR-010**: Hệ thống KHÔNG ĐƯỢC lưu thông tin thẻ thanh toán; chỉ lưu mã token nếu khách lựa chọn.
- **FR-011**: Hệ thống PHẢI gửi email xác nhận kèm PDF vé điện tử (có mã QR) trong vòng 60 giây sau khi đơn hàng chuyển sang "Đã thanh toán". Khi email service gặp sự cố, hệ thống retry tối đa 3 lần với exponential backoff trong 30 phút; nếu vẫn thất bại thì gắn cờ đơn hàng và thông báo CSKH.
- **FR-012**: Hệ thống PHẢI tự động giải phóng vé khi hết TTL và hiển thị thông điệp rõ ràng cho người mua.
- **FR-013**: Hệ thống PHẢI hỗ trợ giao diện song ngữ Tiếng Việt (mặc định) và Tiếng Anh.

#### Module 2 — Admin CMS quản lý vé

- **FR-014**: Hệ thống PHẢI cho phép Quản trị viên sự kiện tạo sự kiện với các trường: tên, mô tả, thể loại, thời gian bắt đầu/kết thúc, địa điểm, đơn vị tổ chức, ngôn ngữ, ảnh đại diện.
- **FR-015**: Tên sự kiện PHẢI là duy nhất trong phạm vi đơn vị tổ chức; thời gian kết thúc PHẢI sau thời gian bắt đầu.
- **FR-016**: Hệ thống PHẢI cho phép cấu hình sơ đồ ghế (tải file JSON/SVG, gán hạng giá cho từng khu/ghế) hoặc danh sách hạng vé (tên, giá ≥ 0, giới hạn số lượng, quy tắc bán).
- **FR-017**: Tổng giới hạn vé KHÔNG ĐƯỢC vượt quá sức chứa địa điểm; không trùng mã ghế trong cùng sự kiện.
- **FR-018**: Mỗi sự kiện PHẢI có tối thiểu một hạng vé hợp lệ trước khi có thể phát hành.
- **FR-019**: Hệ thống PHẢI chạy danh sách kiểm tra trước khi phát hành; nếu không đạt phải hiển thị rõ các mục cần khắc phục và chặn phát hành.
- **FR-020**: Sự kiện có sức chứa ≥ 5.000 vé PHẢI qua phê duyệt hai lớp trước khi phát hành: Quản trị viên sự kiện khởi tạo yêu cầu, Quản trị viên hệ thống là người phê duyệt lớp hai.
- **FR-021**: Sau khi phát hành, sự kiện KHÔNG THỂ bị xóa; chỉ có thể tạm dừng bán hoặc hủy.
- **FR-022**: Mọi thay đổi cấu hình giá PHẢI được kiểm tra hai lớp (bốn mắt): Quản trị viên sự kiện thực hiện thay đổi, Quản trị viên hệ thống phê duyệt; hệ thống PHẢI gửi thông báo tới Quản trị viên hệ thống khi có yêu cầu chờ duyệt qua **email và in-app notification** trên Admin Portal. Yêu cầu phê duyệt **không có timeout** — giữ trạng thái chờ cho đến khi được xử lý thủ công.
- **FR-023**: Hệ thống PHẢI cung cấp dashboard doanh thu với KPI cập nhật trong ≤ 60 giây sau giao dịch: tổng doanh thu, số vé bán, số đơn hàng, tỉ lệ hủy, trung bình giá vé.
- **FR-024**: Hệ thống PHẢI cho phép tạo báo cáo bán hàng theo sự kiện, kênh, phương thức thanh toán, thời gian; hỗ trợ xuất CSV và XLSX với tối đa 1.000.000 bản ghi.
- **FR-025**: Hệ thống PHẢI hỗ trợ lập lịch báo cáo định kỳ (hằng ngày, hằng tuần, hằng tháng); báo cáo hoàn tất được lưu trên portal để admin tự vào xem và tải xuống — không gửi email tự động. Quyền truy cập báo cáo theo RBAC: Quản trị viên hệ thống thấy tất cả; Quản trị viên sự kiện và Nhân viên kinh doanh/Kế toán chỉ thấy báo cáo trong phạm vi quản lý của mình.
- **FR-026**: Mọi thao tác cấu hình PHẢI được ghi vào nhật ký kiểm toán không thể chỉnh sửa, lưu trữ tối thiểu 24 tháng.
- **FR-027**: Hệ thống PHẢI áp dụng RBAC với ít nhất ba vai trò mặc định (Quản trị viên hệ thống, Quản trị viên sự kiện, Nhân viên kinh doanh/Kế toán).
- **FR-028**: Hủy sự kiện đã có giao dịch PHẢI gửi thông báo email tới toàn bộ khách hàng đã mua vé; quy trình hoàn tiền nằm ngoài phạm vi và được xử lý thủ công bởi team vận hành.
- **FR-029**: Tệp xuất chứa dữ liệu cá nhân PHẢI được bảo vệ bằng đường dẫn có hết hạn ≤ 24 giờ và mật khẩu.
- **FR-030**: Trạng thái sự kiện PHẢI đồng bộ chính xác giữa Admin Portal và kênh bán (Module 1) trong ≤ 60 giây.

### Key Entities

- **Sự kiện (Event)**: Thực thể trung tâm — tên, mô tả, thể loại, thời gian, địa điểm, đơn vị tổ chức. State machine: `Bản nháp → Đã phát hành ↔ Tạm dừng`; từ "Đã phát hành" hoặc "Tạm dừng" có thể chuyển sang "Đã hủy"; "Đã kết thúc" được hệ thống tự động set khi qua giờ kết thúc. Sự kiện chờ phê duyệt lớp hai (FR-020) vẫn ở "Bản nháp" cho đến khi được duyệt.
- **Hạng vé (Ticket Type)**: Thuộc về sự kiện — tên, giá, giới hạn số lượng, quy tắc bán, trạng thái tồn kho; có thể gắn với khu/ghế trên sơ đồ.
- **Ghế (Seat)**: Thuộc về sự kiện có sơ đồ ghế — mã ghế duy nhất, khu vực, hạng giá, trạng thái (Khả dụng / Đang giữ / Đã bán). Trạng thái "Đang giữ" áp dụng cho cả hai giai đoạn: pre-lock (2 phút) và Hold chính thức (10 phút) — người mua khác không phân biệt được hai giai đoạn này trên sơ đồ ghế.
- **Đơn hàng (Order)**: Ghi nhận giao dịch người mua — danh sách vé, tổng tiền, trạng thái (Đã giữ chỗ → Đã thanh toán / Thất bại / Đã hủy), thông tin người mua. Order được tạo khi Hold chính thức được khởi tạo (khi người mua bấm "Tiếp tục"); không có Order trong giai đoạn pre-lock.
- **Giữ chỗ (Hold)**: Bản ghi tạm thời — hold ID duy nhất, danh sách ghế/vé, TTL 10 phút, liên kết với đơn hàng.
- **Vé điện tử (E-Ticket)**: Phát sinh sau thanh toán thành công — mã QR duy nhất, thông tin người mua, thông tin sự kiện/ghế, trạng thái (Hợp lệ / Đã dùng / Vô hiệu). QR bị chuyển sang trạng thái "Vô hiệu" tự động khi sự kiện chuyển sang "Đã hủy".
- **Giao dịch thanh toán (Payment Transaction)**: Ghi nhận kết quả từ cổng — mã giao dịch (transaction ID từ cổng, dùng làm idempotency key), số tiền, phương thức, trạng thái, timestamp; mã giao dịch PHẢI là duy nhất để ngăn xử lý webhook trùng.
- **Nhật ký kiểm toán (Audit Log)**: Ghi nhận mọi thao tác admin — hành động, người thực hiện, timestamp, dữ liệu trước/sau thay đổi.

---

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: Người mua có thể hoàn thành toàn bộ hành trình từ tìm kiếm sự kiện đến nhận vé điện tử trong dưới 15 phút trong điều kiện mạng bình thường.
- **SC-002**: 99% email xác nhận và vé điện tử đến hộp thư người mua trong vòng 60 giây sau khi thanh toán thành công.
- **SC-003**: Tỉ lệ thanh toán thành công trong môi trường ổn định đạt tối thiểu 98%.
- **SC-004**: Hệ thống hỗ trợ tối thiểu 50.000 phiên mua vé đồng thời mà không có hiện tượng mất vé hoặc bán trùng; khi vượt ngưỡng tải, hệ thống kích hoạt Virtual Waiting Room để xếp hàng người mua có thứ tự thay vì từ chối kết nối.
- **SC-005**: Vé giữ chỗ được giải phóng tự động và sẵn sàng cho người mua khác trong tối đa 60 giây sau khi hết TTL.
- **SC-006**: Trang chi tiết sự kiện tải đầy đủ trong tối đa 2 giây với kết nối 4G chuẩn (p95).
- **SC-007**: Báo cáo đối soát doanh thu khớp với báo cáo cổng thanh toán với sai số ≤ 0.01% trong 30 ngày vận hành.
- **SC-008**: Doanh thu cập nhật trên dashboard trong tối đa 60 giây sau khi giao dịch hoàn tất.
- **SC-009**: Trạng thái sự kiện đồng bộ chính xác giữa Admin Portal và kênh bán người mua trong tối đa 60 giây.
- **SC-010**: 100% thao tác nhạy cảm của admin được ghi nhật ký kiểm toán và truy xuất được trong tối đa 5 giây.
- **SC-014**: Hệ thống PHẢI thu thập metrics vận hành (latency p95/p99, error rate, queue depth của email/hold job) và kích hoạt alert khi vượt ngưỡng SLA; distributed tracing là deferred.
- **SC-011**: Admin Portal hoạt động ổn định với uptime ≥ 99.9%/tháng và phục vụ ≥ 1.000 quản trị viên đồng thời.
- **SC-012**: Tệp xuất CSV/XLSX mở được trên Excel và Google Sheets, không lỗi mã hóa tiếng Việt.
- **SC-013**: Khi xảy ra sự cố, hệ thống khôi phục hoạt động trong tối đa 30 phút (RTO ≤ 30 phút) và không mất dữ liệu giao dịch quá 5 phút trước thời điểm sự cố (RPO ≤ 5 phút).

---

## Clarifications

### Session 2026-05-13

- Q: Hệ thống xử lý lưu lượng cao khi mở bán sự kiện lớn theo cơ chế nào? → A: Virtual Waiting Room (hàng đợi ảo) khi vượt ngưỡng tải — người mua xếp hàng theo FIFO, hiển thị vị trí và thời gian ước tính.
- Q: TTL của pessimistic pre-lock (trước khi tạo Hold chính thức) là bao lâu? → A: 2 phút — nếu người mua không bấm "Tiếp tục" trong 2 phút, ghế tự động được giải phóng; sau đó Hold chính thức 10 phút mới được tạo.
- Q: Hold/Order đang tồn tại xử lý thế nào khi admin "Tạm dừng bán"? → A: Giữ nguyên Hold hiện có (người mua đang giữ chỗ vẫn có thể thanh toán nốt); chặn không cho tạo Hold mới.
- Q: Timeout yêu cầu phê duyệt giá (FR-022) là bao lâu? → A: Không có timeout — yêu cầu giữ nguyên trạng thái chờ cho đến khi Quản trị viên hệ thống xử lý thủ công.
- Q: Chính sách retry khi email service gặp sự cố là gì? → A: Retry tối đa 3 lần với exponential backoff trong 30 phút; nếu vẫn thất bại thì gắn cờ đơn hàng và thông báo CSKH xử lý thủ công.
- Q: Yêu cầu observability vận hành là gì? → A: Metrics (latency p95/p99, error rate, queue depth) + alerting theo ngưỡng SLA; distributed tracing deferred.
- Q: Khách chưa đăng nhập có thể mua vé không? → A: Không — bắt buộc đăng nhập tài khoản mới được phép mua vé; khách chưa đăng nhập chỉ có thể tìm kiếm và xem chi tiết sự kiện.
- Q: Xử lý idempotency cho webhook thanh toán thế nào? → A: Deduplicate theo transaction ID từ cổng — nếu transaction ID đã được xử lý thành công thì bỏ qua webhook trùng.
- Q: Báo cáo định kỳ (FR-025) được giao đến đâu? → A: Lưu vào portal — admin tự vào xem và tải thủ công; không gửi email tự động.
- Q: Ghế đang trong pre-lock hiển thị trạng thái gì với người mua khác? → A: "Đang giữ" — pre-lock và Hold đều dùng chung trạng thái này; Seat giữ nguyên 3 trạng thái (Khả dụng / Đang giữ / Đã bán).
- Q: Có giới hạn tổng số vé một người mua có thể sở hữu cho cùng một sự kiện không? → A: Có — tối đa 20 vé/user/event (tính tổng các đơn đã thanh toán), nhất quán với giới hạn per-transaction FR-005.
- Q: Trạng thái "Đang chuẩn bị" của Event dùng để làm gì? → A: Bỏ trạng thái này — gộp vào "Bản nháp"; sự kiện chờ phê duyệt lớp hai (FR-020) vẫn ở trạng thái "Bản nháp".
- Q: FR-007 (giới hạn 2 đơn đồng thời) áp dụng cho pre-lock hay Hold? → A: Chỉ Hold chính thức (10 phút) tính vào giới hạn; pre-lock không tính.
- Q: Trạng thái "Đã kết thúc" được set thủ công hay tự động? → A: Tự động — hệ thống chuyển event sang "Đã kết thúc" khi thời gian kết thúc sự kiện qua.
- Q: Từ "Tạm dừng" có thể chuyển thẳng sang "Đã hủy" không? → A: Có — admin được phép hủy sự kiện trực tiếp từ trạng thái "Tạm dừng" mà không cần phục hồi về "Đã phát hành" trước.
- Q: Lý do khi "Tạm dừng bán" — bắt buộc hay tùy chọn? Có hiển thị cho người mua không? → A: Bắt buộc nhập; chỉ lưu vào audit log nội bộ — không hiển thị cho người mua; kênh bán chỉ hiển thị thông điệp chung "Tạm ngừng bán vé".
- Q: Order được tạo ở giai đoạn nào — pre-lock hay Hold? → A: Order tạo tại Hold (khi người mua bấm "Tiếp tục") với trạng thái khởi đầu là "Đã giữ chỗ"; trạng thái "Tạm thời" bị bỏ khỏi Order state machine.
- Q: Pre-lock hoạt động thế nào với sự kiện theo hạng vé (không có sơ đồ ghế)? → A: Không có pre-lock — với sự kiện hạng vé, Hold được tạo ngay khi người mua bấm "Tiếp tục"; pre-lock chỉ áp dụng cho sự kiện có sơ đồ ghế.
- Q: Vé từ đơn đã hủy có trả lại vào cap 20 vé/user/event không? → A: Không — cap 20 tính cộng dồn từ các đơn đã thanh toán, không giảm khi đơn bị hủy.
- Q: Ai được truy cập báo cáo định kỳ lưu trên portal (FR-025)? → A: Theo RBAC — Quản trị viên hệ thống thấy tất cả báo cáo; Quản trị viên sự kiện và Kế toán chỉ thấy báo cáo trong phạm vi quản lý của mình.
- Q: Cap 20 vé/user/event được kiểm tra ở bước nào? → A: Tại thời điểm tạo Hold (bước "Tiếp tục") — từ chối ngay nếu vượt giới hạn; không chờ đến bước thanh toán.
- Q: Người phê duyệt lớp hai trong quy trình "bốn mắt" là ai? → A: Quản trị viên hệ thống phê duyệt (vai trò cao hơn Quản trị viên sự kiện).
- Q: Khi admin hủy sự kiện, quy trình hoàn tiền diễn ra thế nào? → A: Không có luồng hoàn tiền trong phạm vi; hệ thống chỉ gửi email thông báo hủy tới khách hàng, hoàn tiền xử lý thủ công bởi team vận hành.
- Q: Chỉ tiêu khôi phục hệ thống khi có sự cố là gì? → A: RTO ≤ 30 phút, RPO ≤ 5 phút.
- Q: Cơ chế khóa ghế khi nhiều người chọn đồng thời là gì? → A: Pessimistic locking — ghế bị khóa tạm ngay khi người dùng click chọn, sau đó mới tạo Hold chính thức 10 phút.
- Q: Ngưỡng kích hoạt Virtual Waiting Room là bao nhiêu? → A: Deferred — backend hardcode ngưỡng default trong code; cấu hình per-event sẽ được xem xét sau.
- Q: Cơ chế bảo vệ chống bot/scalping là gì? → A: Rate limiting API per IP và per user; không dùng CAPTCHA.
- Q: Kênh thông báo khi có yêu cầu phê duyệt chờ duyệt (FR-022) là gì? → A: Email + in-app notification trên Admin Portal.
- Q: QR code đã phát hành xử lý thế nào khi sự kiện bị hủy? → A: QR bị vô hiệu hóa tự động khi sự kiện chuyển sang trạng thái "Đã hủy".

---

## Assumptions

- Hệ thống đã có sẵn các dịch vụ nền: Event Management Service, Ticket Inventory Service, Reporting Service và cổng thanh toán (ít nhất 3 phương thức: thẻ quốc tế, ví điện tử, chuyển khoản nhanh).
- Hệ thống tuân thủ Nghị định 13/2023/NĐ-CP về bảo vệ dữ liệu cá nhân và PCI-DSS cho dữ liệu thanh toán.
- Mã vé điện tử phát hành dưới dạng QR code, tuân theo chuẩn nội bộ của tổ chức.
- Người mua truy cập qua trình duyệt hiện đại (Chrome, Safari, Edge) và ứng dụng di động iOS/Android.
- Admin truy cập qua Admin Portal trên trình duyệt máy tính; MFA bắt buộc cho vai trò Admin.
- Feature này bao gồm cả hai Module 1 và Module 2; spec 006-my-tickets đã xử lý chức năng xem vé đã mua của người dùng — feature này tập trung vào luồng mua mới và vận hành CMS.
- Spec 007-cms-phan-quyen đã đặc tả phân quyền RBAC — feature này kế thừa hệ thống phân quyền đó.
- Bán vé tại quầy, hệ thống loyalty, chương trình tiếp thị nâng cao và xử lý hoàn vé thủ công nằm ngoài phạm vi.
- Giao diện hỗ trợ song ngữ Tiếng Việt (mặc định) và Tiếng Anh.
