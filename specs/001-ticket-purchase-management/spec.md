# Feature Specification: Mua vé & Quản lý vé (Ticket Purchase & Management)

**Feature Branch**: `001-ticket-purchase-management`
**Created**: 2026-05-06
**Status**: Draft
**Input**: User description: "Chức năng mua vé và quản lý vé cho hệ thống Hà Nội 360 Phase 2 — bao gồm hệ thống loại vé, luồng đặt vé trên App, thanh toán, sau đặt vé, và quản lý vé trên CMS"

## Clarifications

### Session 2026-05-06

- Q: Một đơn đặt vé có thể chứa vé cho nhiều địa điểm hay chỉ 1 địa điểm? → A: Mỗi đơn chỉ cho 1 địa điểm (khách tạo nhiều đơn riêng cho từng nơi)
- Q: Cơ chế hoàn tiền khi khách hủy vé? → A: Tự động qua API cổng thanh toán nếu trong chính sách; nhân viên CMS duyệt thủ công nếu ngoài chính sách (trường hợp đặc biệt)
- Q: Khung giờ slot cố định (sáng/chiều/tối) hay admin tự tạo tùy chỉnh? → A: Admin tự tạo khung giờ dynamic (nhập giờ bắt đầu - kết thúc, số lượng slot không giới hạn)
- Q: Check-in từng vé riêng lẻ hay cả đơn một lần? → A: Check-in cả đơn một lần — quét 1 QR = check-in toàn bộ vé trong đơn
- Q: App đặt vé có cần hỗ trợ đa ngôn ngữ? → A: Hỗ trợ 2 ngôn ngữ: Tiếng Việt + Tiếng Anh

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Khách du lịch đặt vé vào cửa theo ngày/khung giờ (Priority: P1)

Khách mở app Hà Nội 360, chọn địa điểm tham quan, xem lịch slot còn trống theo ngày (calendar view hiển thị màu theo trạng thái: còn chỗ / sắp hết / hết). Khách chọn ngày và khung giờ (các slot do admin cấu hình, ví dụ: 8h-10h, 10h-12h, 14h-17h), chọn loại vé theo đối tượng (người lớn, trẻ em, người cao tuổi), nhập số lượng, điền thông tin người đặt (họ tên, số điện thoại, email), sau đó tiến hành thanh toán online. Sau khi thanh toán thành công, khách nhận vé QR qua email và xem được vé QR trong app (hỗ trợ offline).

**Why this priority**: Đây là luồng nghiệp vụ cốt lõi nhất — không có chức năng đặt vé thì toàn bộ hệ thống vé không có giá trị. Mọi feature khác đều phụ thuộc vào luồng này.

**Independent Test**: Có thể test đầy đủ bằng cách: mở app → chọn địa điểm → xem lịch → chọn slot → chọn vé → nhập thông tin → thanh toán → nhận QR. Delivers giá trị ngay: khách có thể đặt vé online thay vì xếp hàng mua tại quầy.

**Acceptance Scenarios**:

1. **Given** khách đã chọn địa điểm tham quan, **When** khách mở lịch đặt vé, **Then** hệ thống hiển thị calendar với trạng thái slot theo ngày (còn chỗ = xanh, sắp hết = vàng, hết = đỏ)
2. **Given** khách chọn ngày có slot còn trống, **When** khách chọn khung giờ, **Then** hệ thống hiển thị các loại vé khả dụng với giá theo từng đối tượng (người lớn/trẻ em/người cao tuổi)
3. **Given** khách đã chọn loại vé và số lượng, **When** khách nhấn "Tiếp tục", **Then** hệ thống giữ slot (hold) trong 15 phút và chuyển đến trang nhập thông tin
4. **Given** khách đã nhập đầy đủ thông tin bắt buộc (họ tên, SĐT, email), **When** khách chọn phương thức thanh toán và thanh toán thành công, **Then** hệ thống tạo đơn vé, gửi vé QR qua email, và hiển thị vé QR trong app
5. **Given** khách đã nhận vé QR, **When** khách mở vé trong app ở nơi không có mạng, **Then** mã QR vẫn hiển thị đầy đủ để xuất trình tại cổng

---

### User Story 2 - Khách thanh toán qua nhiều cổng thanh toán (Priority: P1)

Khách chọn phương thức thanh toán phù hợp (VNPay hoặc Momo) để hoàn tất đơn đặt vé. Hệ thống chuyển sang giao diện thanh toán của cổng được chọn, xử lý giao dịch, và trả kết quả về app.

**Why this priority**: Không có thanh toán thì không hoàn tất được đơn vé. Đây là điều kiện tiên quyết để luồng đặt vé hoạt động.

**Independent Test**: Có thể test bằng cách thực hiện thanh toán qua từng cổng (VNPay, Momo) và xác nhận đơn vé được tạo thành công sau mỗi giao dịch.

**Acceptance Scenarios**:

1. **Given** khách đang ở bước thanh toán, **When** khách chọn VNPay, **Then** hệ thống chuyển hướng đến trang thanh toán VNPay và xử lý kết quả trả về (thành công/thất bại)
2. **Given** khách đang ở bước thanh toán, **When** khách chọn Momo, **Then** hệ thống mở deeplink Momo hoặc trang web Momo để hoàn tất giao dịch
3. **Given** giao dịch thanh toán thất bại, **When** hệ thống nhận callback thất bại, **Then** thông báo lỗi cho khách và giữ nguyên slot để khách thử lại (trong thời gian hold còn hiệu lực)
4. **Given** thời gian hold slot (15 phút) đã hết mà chưa thanh toán, **When** khách quay lại, **Then** hệ thống giải phóng slot và yêu cầu khách chọn lại

---

### User Story 3 - Khách xem lịch sử và quản lý đơn vé (Priority: P2)

Khách mở app vào mục "Vé của tôi" để xem danh sách đơn vé theo trạng thái: sắp tới, đã sử dụng, đã hủy. Khách có thể xem chi tiết từng đơn, hủy vé (được hoàn tiền theo chính sách), hoặc đổi lịch tham quan.

**Why this priority**: Khách cần quản lý vé đã mua — xem lại thông tin, hủy khi cần, đổi lịch. Đây là yếu tố quan trọng cho trải nghiệm sau mua.

**Independent Test**: Có thể test bằng cách: đặt vé thành công → vào lịch sử đơn → xem chi tiết → hủy vé → xác nhận hoàn tiền. Delivers giá trị: khách tự chủ quản lý vé mà không cần liên hệ hỗ trợ.

**Acceptance Scenarios**:

1. **Given** khách đã đặt vé thành công, **When** khách mở mục "Vé của tôi", **Then** đơn vé hiển thị trong danh sách "Sắp tới" với đầy đủ thông tin (địa điểm, ngày, khung giờ, loại vé, số lượng)
2. **Given** khách muốn hủy vé trong chính sách (trước 48h hoặc 24h), **When** khách nhấn "Hủy vé" và xác nhận, **Then** hệ thống hủy đơn, tự động hoàn tiền qua cổng thanh toán gốc (100% trước 48h, 50% trước 24h), và giải phóng slot
   - **Given** khách muốn hủy vé ngoài chính sách (trong ngày), **When** khách gửi yêu cầu, **Then** hệ thống tạo yêu cầu hoàn tiền chờ nhân viên CMS duyệt
3. **Given** khách muốn đổi lịch, **When** khách nhấn "Đổi lịch" và chọn ngày/slot mới còn trống, **Then** hệ thống cập nhật đơn vé sang lịch mới và giải phóng slot cũ
4. **Given** vé đã được check-in tại cổng, **When** khách xem lịch sử, **Then** đơn vé chuyển sang trạng thái "Đã sử dụng" và không thể hủy/đổi lịch

---

### User Story 4 - Khách nhập mã voucher để được giảm giá (Priority: P2)

Trước khi thanh toán, khách nhập mã voucher/giảm giá. Hệ thống xác thực mã ngay lập tức, hiển thị số tiền được giảm và tổng tiền mới.

**Why this priority**: Mã giảm giá là công cụ marketing quan trọng để thu hút khách, nhưng không phải điều kiện để luồng đặt vé cơ bản hoạt động.

**Independent Test**: Có thể test bằng cách nhập mã voucher hợp lệ/không hợp lệ/hết hạn tại bước thanh toán và xác nhận giá được cập nhật đúng.

**Acceptance Scenarios**:

1. **Given** khách ở bước thanh toán, **When** khách nhập mã voucher hợp lệ, **Then** hệ thống hiển thị số tiền giảm và cập nhật tổng tiền thanh toán
2. **Given** khách nhập mã voucher, **When** mã không tồn tại hoặc đã hết hạn, **Then** hệ thống hiển thị thông báo lỗi cụ thể (mã không tồn tại / đã hết hạn / đã dùng hết lượt)
3. **Given** mã voucher có giới hạn số lần sử dụng, **When** mã đã dùng hết lượt, **Then** hệ thống từ chối áp dụng và thông báo rõ ràng

---

### User Story 5 - Nhân viên CMS cấu hình hệ thống vé (Priority: P1)

Admin/nhân viên đăng nhập CMS để tạo và quản lý các loại vé (vé vào cửa theo ngày, theo khung giờ, vé xe điện, vé cáp treo, vé thuyền...), cấu hình slot và sức chứa theo ngày, cấu hình giá theo đối tượng (người lớn/trẻ em/người cao tuổi/sinh viên/người khuyết tật/khách nước ngoài), và bật/tắt bán vé theo địa điểm.

**Why this priority**: Không có cấu hình loại vé và slot trên CMS thì App không có dữ liệu để hiển thị. Đây là nền tảng cho toàn bộ luồng đặt vé phía khách hàng.

**Independent Test**: Có thể test bằng cách: đăng nhập CMS → tạo loại vé → cấu hình slot/sức chứa → cấu hình giá → bật bán vé → kiểm tra App hiển thị đúng thông tin. Delivers giá trị: nhân viên tự chủ quản lý hệ thống vé mà không cần lập trình viên.

**Acceptance Scenarios**:

1. **Given** nhân viên đăng nhập CMS, **When** nhân viên tạo loại vé mới (tên, mô tả, loại: theo ngày/theo slot/cố định), **Then** loại vé được lưu và khả dụng để cấu hình giá và slot
2. **Given** loại vé đã tồn tại, **When** nhân viên cấu hình giá theo đối tượng (người lớn: X đồng, trẻ em: Y đồng...), **Then** giá hiển thị đúng trên App theo từng đối tượng
3. **Given** loại vé theo thời gian (ngày/slot), **When** nhân viên tạo slot tùy chỉnh (nhập giờ bắt đầu - kết thúc) và cấu hình sức chứa (số vé tối đa mỗi slot), **Then** App chỉ cho phép đặt khi slot chưa đầy
4. **Given** nhân viên muốn ngừng bán vé cho một địa điểm/sự kiện, **When** nhân viên tắt toggle "Bán vé", **Then** vé không còn hiển thị trên App, đơn đã đặt không bị ảnh hưởng

---

### User Story 6 - Nhân viên CMS quản lý đơn đặt vé và check-in (Priority: P1)

Nhân viên CMS xem danh sách đơn đặt vé (lọc theo ngày, địa điểm, trạng thái), xử lý đơn (xác nhận/hủy), và quét QR check-in tại cổng để xác thực vé.

**Why this priority**: Check-in tại cổng là bước vận hành thiết yếu — nếu không quét được QR thì vé điện tử không có ý nghĩa.

**Independent Test**: Có thể test bằng cách: xem danh sách đơn trên CMS → lọc theo điều kiện → quét QR của vé đã đặt → xác nhận check-in thành công.

**Acceptance Scenarios**:

1. **Given** nhân viên mở danh sách đơn đặt vé trên CMS, **When** nhân viên lọc theo ngày/địa điểm/trạng thái, **Then** danh sách hiển thị các đơn phù hợp với đầy đủ thông tin (mã đơn, tên khách, ngày, loại vé, trạng thái, số tiền)
2. **Given** nhân viên tại cổng check-in, **When** quét mã QR của vé hợp lệ chưa sử dụng, **Then** hệ thống hiển thị "Hợp lệ" kèm thông tin vé, và đánh dấu vé đã check-in
3. **Given** nhân viên quét mã QR, **When** vé đã được check-in trước đó, **Then** hệ thống hiển thị cảnh báo "Vé đã sử dụng" kèm thời gian check-in trước
4. **Given** nhân viên quét mã QR, **When** mã QR không hợp lệ hoặc vé đã hủy, **Then** hệ thống hiển thị "Không hợp lệ" với lý do cụ thể

---

### User Story 7 - Nhân viên CMS cấu hình chính sách hủy/hoàn tiền và voucher (Priority: P2)

Admin cấu hình chính sách hủy vé (% hoàn tiền theo mốc thời gian trước ngày tham quan), và quản lý mã voucher/giảm giá (tạo, sửa, xóa mã; cấu hình loại giảm giá, giới hạn lần dùng, thời hạn).

**Why this priority**: Chính sách hủy/hoàn tiền cần được cấu hình linh hoạt theo từng địa điểm. Voucher hỗ trợ chiến dịch marketing.

**Independent Test**: Có thể test bằng cách: cấu hình policy hoàn tiền trên CMS → khách hủy vé trên App → xác nhận số tiền hoàn đúng theo policy. Tương tự cho voucher.

**Acceptance Scenarios**:

1. **Given** admin mở cấu hình chính sách hủy, **When** admin thiết lập các mốc hoàn tiền (ví dụ: trước 48h hoàn 100%, trước 24h hoàn 50%, trong ngày không hoàn), **Then** khi khách hủy vé, hệ thống tự động tính và hoàn đúng số tiền
2. **Given** admin tạo mã voucher mới, **When** admin nhập thông tin (mã, loại giảm: % hoặc số tiền, giới hạn lần dùng, thời hạn), **Then** mã voucher khả dụng cho khách sử dụng trên App
3. **Given** mã voucher đã hết hạn hoặc hết lượt, **When** khách nhập mã trên App, **Then** hệ thống từ chối và hiển thị lý do

---

### User Story 8 - Nhân viên CMS xem dashboard doanh thu và thống kê vé (Priority: P3)

Nhân viên/admin xem dashboard tổng quan về doanh thu bán vé, tỷ lệ lấp đầy slot, tỷ lệ check-in, và số liệu bán vé theo thời gian.

**Why this priority**: Dashboard hỗ trợ ra quyết định kinh doanh nhưng không ảnh hưởng đến luồng vận hành cốt lõi.

**Independent Test**: Có thể test bằng cách: tạo một số đơn vé, check-in một số → vào dashboard → xác nhận số liệu khớp.

**Acceptance Scenarios**:

1. **Given** nhân viên mở dashboard trên CMS, **When** chọn khoảng thời gian, **Then** hệ thống hiển thị: tổng vé bán ra, doanh thu, tỷ lệ lấp đầy slot, tỷ lệ check-in
2. **Given** có dữ liệu bán vé theo nhiều ngày, **When** nhân viên xem biểu đồ xu hướng, **Then** dữ liệu được hiển thị theo ngày/tuần/tháng với khả năng lọc theo địa điểm

---

### Edge Cases

- Nếu hai khách cùng đặt vé cuối cùng của slot cùng lúc thì sao? Hệ thống phải đảm bảo chỉ một người được giữ slot (cơ chế lock/race condition).
- Nếu thanh toán thành công ở phía cổng thanh toán nhưng callback về hệ thống bị lỗi mạng? Hệ thống cần cơ chế đối soát (reconciliation) để cập nhật trạng thái đơn.
- Nếu khách đổi lịch sang slot đã gần đầy và có người khác cũng đang chọn slot đó? Áp dụng cùng cơ chế hold slot như đặt mới.
- Nếu khách hủy vé combo gồm nhiều loại vé thành phần? Hủy toàn bộ combo, không hủy từng phần.
- Nếu nhân viên quét QR tại nơi không có mạng? Cần cơ chế cache offline cho check-in hoặc yêu cầu kết nối mạng.
- Nếu khách mua vé cho ngày hôm nay nhưng slot sáng đã qua? Chỉ hiển thị các slot chưa qua thời gian hiện tại.
- **[Capacity overbook]** Nếu slot có capacity > 1 và nhiều khách cùng vượt qua bước kiểm tra capacity rồi cùng hold? Cơ chế hold phải dùng atomic counter (không phải per-user lock) để đảm bảo tổng số hold không vượt quá capacity.
- **[Webhook muộn]** Nếu khách bắt đầu thanh toán sát hạn 15 phút, TTL hết trước khi payment gateway callback về? Hệ thống phải phát hiện trường hợp này và hoàn tiền ngay, không để booking ở trạng thái mâu thuẫn (tiền đã trừ nhưng booking EXPIRED).
- **[Phantom hold]** Nếu hệ thống đã trừ capacity trong Redis nhưng INSERT booking vào DB thất bại? Capacity phải được khôi phục ngay (compensating operation) để không mất slot "ảo".
- **[Auto-release conflict]** Nếu Redis TTL expire event kích hoạt sau khi booking đã được CONFIRMED? Handler auto-release phải kiểm tra trạng thái hiện tại của booking trước khi cập nhật — bỏ qua nếu đã CONFIRMED.

## Requirements *(mandatory)*

### Functional Requirements

**Hệ thống loại vé (CMS)**

- **FR-001**: Hệ thống PHẢI hỗ trợ các loại vé theo thời gian: vé vào cửa theo ngày, vé vào cửa theo khung giờ (slot do admin tự cấu hình)
- **FR-002**: Hệ thống PHẢI hỗ trợ các loại vé cố định (không cần ngày/giờ): vé xe điện/xe trung chuyển, vé cáp treo (1 chiều/2 chiều), vé thuyền/đò
- **FR-003**: Hệ thống PHẢI cho phép cấu hình giá vé theo đối tượng: người lớn, trẻ em (dưới 15 tuổi), người cao tuổi (từ 60 tuổi), sinh viên, người khuyết tật/thương bệnh binh, khách nước ngoài
- **FR-004**: Hệ thống PHẢI cho phép tạo/sửa/xóa loại vé với thông tin: tên (Việt + Anh), mô tả (Việt + Anh), giá theo đối tượng, thời hạn hiệu lực
- **FR-005**: Hệ thống PHẢI cho phép admin tự tạo khung giờ slot tùy chỉnh (nhập giờ bắt đầu - giờ kết thúc), không giới hạn số lượng slot mỗi ngày, và cấu hình sức chứa (số vé tối đa) riêng cho từng slot tại từng địa điểm
- **FR-006**: Hệ thống PHẢI cho phép bật/tắt bán vé theo địa điểm hoặc sự kiện, bao gồm khả năng đặt lịch tự bật/tắt

**Luồng đặt vé (App)**

- **FR-007**: Khách PHẢI có thể xem lịch slot còn trống theo ngày dưới dạng calendar view, với chỉ thị trực quan về trạng thái slot (còn chỗ/sắp hết/hết)
- **FR-008**: Khách PHẢI có thể chọn loại vé và nhập số lượng theo nhiều đối tượng trong cùng một đơn (ví dụ: 2 người lớn + 1 trẻ em)
- **FR-009**: Khách PHẢI nhập thông tin người đặt gồm: họ tên, số điện thoại, email
- **FR-009a**: App đặt vé PHẢI hỗ trợ 2 ngôn ngữ: Tiếng Việt và Tiếng Anh. Nội dung loại vé, thông báo, email vé QR đều cần có bản song ngữ
- **FR-010**: Hệ thống PHẢI giữ slot (hold) trong 15 phút khi khách đang thanh toán; tự động giải phóng nếu hết thời gian. Việc giữ slot phải dùng cơ chế atomic counter để đảm bảo tổng số hold active không vượt quá capacity — không dùng per-user lock vì không kiểm soát được tổng số lượng
- **FR-010a**: Hệ thống PHẢI xóa hold record ngay khi đơn chuyển sang CONFIRMED, và handler auto-release PHẢI kiểm tra trạng thái booking trước khi cập nhật để tránh ghi đè dữ liệu đơn hợp lệ
- **FR-010b**: Nếu thao tác tạo booking trong DB thất bại sau khi đã giảm capacity, hệ thống PHẢI khôi phục capacity ngay lập tức (compensating operation)
- **FR-011**: Khách CÓ THỂ nhập mã voucher/giảm giá trước khi thanh toán; hệ thống xác thực realtime và hiển thị số tiền tiết kiệm

**Thanh toán**

- **FR-012**: Hệ thống PHẢI tích hợp thanh toán qua VNPay
- **FR-013**: Hệ thống PHẢI tích hợp thanh toán qua Momo
- **FR-014**: Hệ thống PHẢI xử lý callback thanh toán (thành công/thất bại) và cập nhật trạng thái đơn vé tương ứng. Webhook processor PHẢI dùng idempotency key để chống duplicate callback từ payment gateway
- **FR-014a**: Nếu callback SUCCESS đến sau khi booking đã EXPIRED (TTL hết trước khi gateway trả về), hệ thống PHẢI từ chối confirm và kích hoạt hoàn tiền tự động ngay — không để trạng thái mâu thuẫn giữa giao dịch tài chính và trạng thái đơn
- **FR-014b**: Hệ thống PHẢI gia hạn thời gian hold (reset TTL) khi khách bấm xác nhận thanh toán và được redirect sang payment gateway, để tránh race condition giữa TTL expire và thời gian gateway xử lý
- **FR-015**: Hệ thống PHẢI có cơ chế đối soát (reconciliation) cho trường hợp callback bị lỗi

**Sau đặt vé (App)**

- **FR-016**: Hệ thống PHẢI gửi vé QR qua email ngay sau thanh toán thành công
- **FR-017**: Hệ thống PHẢI cho phép xem vé QR trong app ở chế độ offline
- **FR-018**: Hệ thống PHẢI hiển thị lịch sử đơn đặt vé theo trạng thái: sắp tới, đã sử dụng, đã hủy, với chi tiết từng đơn
- **FR-019**: Hệ thống PHẢI cho phép hủy vé với hoàn tiền tự động qua API cổng thanh toán (VNPay/Momo) nếu yêu cầu hủy nằm trong chính sách đã cấu hình. Trường hợp ngoài chính sách (hủy muộn, hoàn cảnh đặc biệt), nhân viên CMS duyệt và kích hoạt hoàn tiền thủ công
- **FR-020**: Hệ thống NÊN cho phép đổi lịch (reschedule) tối đa 1 lần, miễn phí nếu đổi trước 24h

**Quản lý & Vận hành (CMS)**

- **FR-021**: Hệ thống PHẢI cho phép quét QR check-in tại cổng (1 QR mỗi đơn, check-in toàn bộ vé trong đơn) với phản hồi tức thì (hợp lệ/đã dùng/không hợp lệ)
- **FR-022**: Hệ thống PHẢI cho phép xem và xử lý danh sách đơn đặt vé trên CMS, hỗ trợ lọc theo ngày, địa điểm, trạng thái
- **FR-023**: Hệ thống PHẢI cho phép cấu hình chính sách hủy/hoàn tiền (% hoàn theo mốc thời gian trước ngày tham quan)
- **FR-024**: Hệ thống NÊN cho phép quản lý voucher/mã giảm giá (tạo, sửa, xóa; cấu hình loại giảm, giới hạn, thời hạn)
- **FR-025**: Hệ thống NÊN cung cấp dashboard doanh thu và thống kê vé (vé bán ra, tỷ lệ lấp đầy slot, check-in rate, doanh thu)
- **FR-026**: Hệ thống NÊN cho phép cấu hình vé combo (gộp nhiều loại vé với giá ưu đãi)

### Key Entities

- **Loại vé (Ticket Type)**: Định nghĩa một loại vé bao gồm tên, mô tả, phân loại (theo thời gian/cố định), giá theo đối tượng, thời hạn hiệu lực, trạng thái bán. Gắn với một địa điểm hoặc sự kiện cụ thể.
- **Slot**: Khung thời gian do admin tự tạo (giờ bắt đầu - giờ kết thúc, dynamic, không cố định sáng/chiều/tối), bao gồm ngày, sức chứa tối đa, số lượng đã bán. Thuộc về một Loại vé tại một Địa điểm. Số lượng slot mỗi ngày không giới hạn.
- **Đơn đặt vé (Booking Order)**: Đơn hàng của khách gắn với **một địa điểm duy nhất**, bao gồm thông tin người đặt, danh sách vé (loại + số lượng + đối tượng), slot đã chọn, **1 mã QR duy nhất cho cả đơn** (dùng để check-in toàn bộ vé trong đơn), trạng thái (chờ thanh toán/đã thanh toán/đã check-in/đã hủy), thông tin thanh toán, voucher áp dụng. Khách muốn đặt vé nhiều địa điểm cần tạo nhiều đơn riêng.
- **Vé (Ticket)**: Mỗi vé đơn lẻ trong đơn, gắn với đối tượng cụ thể. Thuộc về một Đơn đặt vé. Check-in thực hiện ở cấp đơn (quét 1 mã QR của đơn = check-in toàn bộ vé), không cần QR riêng từng vé.
- **Voucher**: Mã giảm giá với thuộc tính: mã, loại giảm (% hoặc số tiền cố định), giới hạn lần sử dụng, thời hạn hiệu lực, trạng thái (hoạt động/hết hạn/tắt).
- **Chính sách hủy (Cancellation Policy)**: Quy tắc hoàn tiền gồm các mốc thời gian và % hoàn tương ứng. Gắn với Loại vé hoặc Địa điểm.
- **Đối tượng khách (Customer Category)**: Phân loại khách (người lớn, trẻ em, người cao tuổi, sinh viên, người khuyết tật, khách nước ngoài) dùng để xác định giá vé và chính sách ưu đãi.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: Khách có thể hoàn tất đặt vé (từ chọn địa điểm đến nhận QR) trong vòng 3 phút
- **SC-002**: Hệ thống giữ slot chính xác — không bán vượt quá sức chứa tối đa của slot trong mọi trường hợp (bao gồm đặt đồng thời)
- **SC-003**: Tỷ lệ thanh toán thành công đạt trên 95% (không tính lỗi do phía khách/ngân hàng)
- **SC-004**: Vé QR hiển thị được trong app khi không có kết nối mạng
- **SC-005**: Nhân viên quét QR check-in và nhận phản hồi (hợp lệ/không hợp lệ) trong vòng 2 giây
- **SC-006**: Nhân viên CMS có thể tạo loại vé mới và cấu hình slot/sức chứa trong vòng 5 phút
- **SC-007**: Hoàn tiền tự động được xử lý chính xác theo chính sách đã cấu hình (100% trường hợp đúng mốc thời gian)
- **SC-008**: 90% khách hàng tự hoàn tất đặt vé lần đầu mà không cần hỗ trợ

## Assumptions

- Hệ thống Hà Nội 360 Phase 1 đã có sẵn hạ tầng: hệ thống user/auth, danh sách địa điểm, và app mobile cơ bản
- Ngưỡng tuổi mặc định: trẻ em dưới 15 tuổi, người cao tuổi từ 60 tuổi (dựa trên thông lệ ngành du lịch Việt Nam)
- Thời gian hold slot mặc định: 15 phút (thông lệ ngành e-commerce/booking)
- Chính sách hủy/hoàn tiền mặc định: 100% trước 48h, 50% trước 24h, 0% trong ngày — có thể cấu hình lại trên CMS
- Đổi lịch: tối đa 1 lần, miễn phí nếu đổi trước 24h — dựa trên thông lệ ngành
- Cổng thanh toán Phase 2: VNPay và Momo là bắt buộc (Must-have); ZaloPay sẽ được bổ sung sau nếu cần
- Vé ban đêm, vé đoàn, vé combo, vé thuê hướng dẫn viên, vé audio guide, vé VR/AR, vé chụp ảnh, vé thuê trang phục là các loại vé mở rộng — hệ thống thiết kế linh hoạt để hỗ trợ nhưng không bắt buộc trong phiên bản đầu tiên
- Thanh toán tiền mặt tại quầy (Nice-to-have) không nằm trong scope chính
- Waitlist khi slot đầy (Nice-to-have) không nằm trong scope chính
- Xuất hóa đơn VAT (Nice-to-have) không nằm trong scope chính
- Check-in QR thực hiện qua CMS web trên thiết bị di động hoặc tablet (không cần app riêng cho nhân viên)
- Dashboard doanh thu/thống kê là Recommended, có thể triển khai ở giai đoạn sau của Phase 2

## Backend Architecture Constraints

Section này ghi lại các quyết định kỹ thuật bắt buộc để đảm bảo tính đúng đắn của luồng hold slot. Không phải implementation guide — là ràng buộc thiết kế phải giữ nguyên trong suốt quá trình build.

### Slot capacity management

- **Atomic counter, không phải lock**: Capacity phải được quản lý bằng Redis atomic counter (`DECRBY`/`INCRBY`). Không dùng per-user key (`SETNX`) vì không kiểm soát được tổng số hold đồng thời trên cùng một slot.
- **Counter pattern**: `slot:{slot_id}:available` giữ số lượng slot còn lại. Mỗi hold decrement 1; nếu kết quả < 0 thì INCRBY restore và trả về lỗi 409.
- **Redis Persistence**: Phải bật AOF (Append Only File) cho Redis để không mất hold state khi Redis restart.

### Hold lifecycle

| Sự kiện | Hành động bắt buộc |
|---|---|
| Hold thành công | DECRBY counter + INSERT booking(HOLD) + set TTL 900s |
| Khách bấm "Thanh toán" | EXPIRE reset lại 900s (gia hạn hold) |
| Payment webhook SUCCESS | UPDATE CONFIRMED + DEL hold key ngay lập tức |
| Payment webhook FAIL | Giữ nguyên hold, khách retry trong thời gian còn lại |
| TTL expire | INCRBY restore counter + UPDATE booking EXPIRED (chỉ nếu status ≠ CONFIRMED) |
| INSERT booking thất bại | INCRBY restore counter ngay lập tức (compensating) |

### Webhook safety

- **Idempotency**: Mỗi webhook phải kiểm tra `payment_transaction_id` đã xử lý chưa trước khi cập nhật. Ghi idempotency key vào DB sau khi xử lý thành công.
- **Late webhook** (đến sau khi booking EXPIRED): Từ chối confirm, gọi API hoàn tiền của gateway ngay, ghi log để audit.
- **Timeout tự xử lý**: Reconciliation job chạy mỗi 5 phút, truy vấn các booking ở trạng thái PENDING_PAYMENT quá 20 phút mà chưa có webhook → chủ động hỏi gateway về trạng thái giao dịch.

### Booking state — nguồn sự thật duy nhất

Backend là single source of truth cho trạng thái đơn. Frontend không được tự suy luận hay ghi state. Mọi thay đổi trạng thái đều phải đi qua backend API với đủ validation.

Thứ tự trạng thái hợp lệ: `PENDING_HOLD → HOLD → PENDING_PAYMENT → CONFIRMED → CHECKED_IN`
Terminal states (không chuyển tiếp được): `FAILED`, `EXPIRED`, `CANCELLED`
Transition không hợp lệ (ví dụ CONFIRMED → HOLD) phải bị reject ở tầng service.
