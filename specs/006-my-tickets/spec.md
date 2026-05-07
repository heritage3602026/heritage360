# Feature Specification: My Tickets (Vé của tôi)

**Feature Branch**: `006-my-tickets`  
**Created**: 2026-05-07  
**Status**: Draft  

## User Scenarios & Testing *(mandatory)*

### User Story 1 - View Upcoming Tickets (Priority: P1)

Người dùng đã đặt vé tham quan muốn xem danh sách các vé sắp diễn ra để chuẩn bị cho chuyến đi. Họ mở màn "Vé của tôi", thấy tab "Sắp diễn ra" hiển thị tất cả vé chưa đến ngày sự kiện với thông tin đầy đủ: tên địa điểm, ngày giờ, số lượng vé và giá tiền.

**Why this priority**: Đây là màn hình trung tâm giúp người dùng quản lý hành trình tham quan. Không có tính năng này, người dùng không thể tra cứu thông tin vé đã mua.

**Independent Test**: Đặt ít nhất 1 vé có ngày tham quan trong tương lai, mở "Vé của tôi" → tab "Sắp diễn ra", kiểm tra thẻ vé hiển thị đầy đủ thông tin và badge "Sắp diễn ra".

**Acceptance Scenarios**:

1. **Given** người dùng đã đặt vé cho sự kiện chưa diễn ra, **When** họ mở "Vé của tôi" và chọn tab "Sắp diễn ra", **Then** danh sách hiển thị tất cả vé sắp tới với tên sự kiện, ngày, khung giờ, số người, mã vé và tổng tiền.
2. **Given** người dùng không có vé nào sắp diễn ra, **When** họ mở tab "Sắp diễn ra", **Then** màn hình hiển thị trạng thái rỗng với thông báo phù hợp.
3. **Given** người dùng có nhiều vé sắp diễn ra, **When** họ xem danh sách, **Then** các vé được sắp xếp theo thứ tự ngày tham quan gần nhất lên đầu.

---

### User Story 2 - View Past Tickets (Priority: P2)

Người dùng muốn xem lại lịch sử các vé đã sử dụng hoặc đã hủy để đánh giá trải nghiệm hoặc đặt lại vé cho lần tới. Họ chuyển sang tab "Đã kết thúc" và thấy danh sách vé đã hoàn thành hoặc bị hủy, mỗi vé có trạng thái rõ ràng và các hành động phù hợp.

**Why this priority**: Hỗ trợ người dùng theo dõi lịch sử tham quan và tạo cơ hội đặt lại, cũng như viết đánh giá để cải thiện chất lượng dịch vụ.

**Independent Test**: Có ít nhất 1 vé đã hoàn thành và 1 vé đã hủy, mở tab "Đã kết thúc", kiểm tra trạng thái hiển thị đúng và các nút hành động phù hợp với từng trạng thái.

**Acceptance Scenarios**:

1. **Given** người dùng có vé đã hoàn thành, **When** họ xem tab "Đã kết thúc", **Then** vé hiển thị badge "Đã hoàn thành" với hai nút "Đánh giá" và "Đặt lại".
2. **Given** người dùng có vé đã bị hủy, **When** họ xem tab "Đã kết thúc", **Then** vé hiển thị badge "Đã hủy" với chỉ nút "Đặt lại" (không có nút "Đánh giá").
3. **Given** người dùng nhấn "Đánh giá" trên vé đã hoàn thành, **When** hành động được thực hiện, **Then** hệ thống chuyển đến màn hình viết đánh giá cho sự kiện đó.
4. **Given** người dùng nhấn "Đặt lại" trên bất kỳ vé nào, **When** hành động được thực hiện, **Then** hệ thống điều hướng đến trang đặt vé của sự kiện tương ứng.

---

### User Story 3 - View Ticket Detail & QR Code (Priority: P1)

Người dùng tại điểm tham quan cần xuất trình vé điện tử. Họ nhấn "Xem QR" từ danh sách hoặc "Xem vé" để vào màn chi tiết vé, nơi hiển thị mã QR lớn để nhân viên quét cùng toàn bộ thông tin vé: tên sự kiện, ngày, khung giờ, loại vé, số lượng, thông tin người đặt và chính sách hoàn vé.

**Why this priority**: Đây là điểm chạm quan trọng nhất trong hành trình người dùng — kiểm soát vào cổng. Nếu QR không hiển thị được, toàn bộ trải nghiệm thất bại.

**Independent Test**: Mở chi tiết vé đã đặt, kiểm tra mã QR hiển thị rõ ràng, mã vé đúng, và tất cả thông tin chi tiết chính xác.

**Acceptance Scenarios**:

1. **Given** người dùng nhấn "Xem QR" từ thẻ vé, **When** hành động được thực hiện, **Then** một modal/màn full-screen mở ra chỉ hiển thị mã QR lớn, mã vé dạng text và nút đóng — không có thông tin chi tiết khác.
2. **Given** màn chi tiết vé đang mở, **When** người dùng xem thông tin, **Then** hiển thị đầy đủ: mã vé (#VE...), tên sự kiện, ngày tham quan, khung giờ, loại vé, số lượng, đơn giá, tổng thanh toán, tên người đặt, SĐT, email và chính sách hoàn vé.
3. **Given** người dùng xem chính sách hoàn vé, **When** thời điểm hiện tại rơi vào khung giờ cụ thể, **Then** hệ thống hiển thị thông báo rõ ràng về mức hoàn tiền áp dụng (ví dụ: "Hủy trước 12:00 hôm nay được hoàn 50%").
4. **Given** thiết bị đang offline, **When** người dùng mở chi tiết vé, **Then** vé đã được lưu cục bộ vẫn hiển thị được mã QR và thông tin đầy đủ.

---

### User Story 4 - Cancel Ticket (Priority: P2)

Người dùng không thể tham dự sự kiện và muốn hủy vé để được hoàn tiền theo chính sách. Từ màn chi tiết vé, họ nhấn "Hủy vé", xem lại chính sách hoàn tiền áp dụng cho thời điểm hiện tại, xác nhận và nhận phản hồi về kết quả hủy.

**Why this priority**: Tính năng quan trọng ảnh hưởng trực tiếp đến tài chính người dùng. Cần xử lý chính xác theo chính sách hoàn tiền theo thời gian.

**Independent Test**: Với vé sắp diễn ra còn trong thời hạn hủy, nhấn "Hủy vé", xác nhận hủy, kiểm tra vé chuyển sang trạng thái "Đã hủy" và thông báo hoàn tiền hiển thị đúng.

**Acceptance Scenarios**:

1. **Given** người dùng nhấn "Hủy vé" trên vé sắp diễn ra, **When** xác nhận hủy, **Then** hệ thống hiển thị dialog xác nhận với thông tin hoàn tiền cụ thể dựa trên thời điểm hủy.
2. **Given** người dùng hủy vé trước 48 giờ, **When** xác nhận thành công, **Then** vé chuyển trạng thái "Đã hủy" và thông báo hoàn 100% giá trị.
3. **Given** người dùng hủy vé trong khoảng 24–48 giờ trước, **When** xác nhận thành công, **Then** thông báo hoàn 50% giá trị.
4. **Given** người dùng hủy vé trong vòng 24 giờ trước sự kiện, **When** xác nhận thành công, **Then** thông báo không được hoàn tiền.

---

### User Story 5 - Reschedule Ticket (Priority: P3)

Người dùng muốn thay đổi ngày hoặc khung giờ tham quan mà không cần hủy và đặt lại vé. Từ chi tiết vé, họ nhấn "Đổi lịch", chọn ngày và khung giờ mới còn trống và xác nhận thay đổi.

**Why this priority**: Tính năng tiện lợi bổ sung, giúp giảm tỷ lệ hủy vé, nhưng không thiết yếu cho MVP.

**Independent Test**: Từ chi tiết vé sắp diễn ra, nhấn "Đổi lịch", chọn ngày/giờ mới, xác nhận và kiểm tra thông tin vé cập nhật đúng.

**Acceptance Scenarios**:

1. **Given** người dùng nhấn "Đổi lịch", **When** màn hình đổi lịch mở, **Then** hệ thống hiển thị lịch chỉ với các khung giờ còn chỗ trống cho sự kiện đó.
2. **Given** người dùng chọn khung giờ mới và xác nhận, **When** thay đổi thành công, **Then** thông tin vé cập nhật ngày và giờ mới, mã QR vẫn hợp lệ.
3. **Given** không còn khung giờ nào khác available, **When** người dùng nhấn "Đổi lịch", **Then** hệ thống thông báo không có lịch thay thế và đề xuất hủy vé nếu cần.
4. **Given** còn dưới 24 giờ trước sự kiện, **When** người dùng vào màn chi tiết vé, **Then** nút "Đổi lịch" bị vô hiệu hóa kèm thông báo "Đã hết thời hạn đổi lịch".

---

### User Story 6 - Download Ticket (Priority: P3)

Người dùng muốn lưu vé về thiết bị dưới dạng hình ảnh hoặc PDF để sử dụng khi không có mạng hoặc in ra.

**Why this priority**: Tiện ích bổ sung, người dùng đã có thể dùng QR online, nhưng download giúp trong tình huống không có internet.

**Independent Test**: Nhấn "Tải về" từ chi tiết vé, kiểm tra file vé được lưu thành công vào thư viện ảnh hoặc bộ nhớ thiết bị.

**Acceptance Scenarios**:

1. **Given** người dùng nhấn "Tải về", **When** quá trình lưu hoàn tất, **Then** file vé (hình ảnh hoặc PDF) được lưu vào thiết bị với thông báo thành công.
2. **Given** thiết bị không đủ dung lượng lưu trữ, **When** người dùng nhấn "Tải về", **Then** hệ thống hiển thị thông báo lỗi rõ ràng.

---

### Edge Cases

- Người dùng cố hủy vé khi sự kiện đã bắt đầu hoặc đã kết thúc → nút "Hủy vé" bị vô hiệu hóa hoặc không hiển thị.
- Người dùng mở danh sách vé khi không có kết nối mạng → hiển thị dữ liệu cache từ lần truy cập gần nhất.
- Vé cùng một sự kiện cho nhiều ngày khác nhau → mỗi vé là một thẻ riêng biệt.
- Màn hình sáng khi hiển thị QR → tự động tăng độ sáng màn hình để đảm bảo QR dễ quét.
- Người dùng cố đổi lịch khi còn dưới 24 giờ trước sự kiện → nút "Đổi lịch" bị vô hiệu hóa, hiển thị thông báo "Đã hết thời hạn đổi lịch".
- Hoàn tiền sau khi hủy được xử lý qua phương thức thanh toán gốc → thời gian hoàn tiền phụ thuộc nhà cung cấp thanh toán.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: Hệ thống PHẢI hiển thị danh sách vé của người dùng chia thành 2 tab: "Sắp diễn ra" và "Đã kết thúc".
- **FR-002**: Mỗi thẻ vé trong danh sách PHẢI hiển thị: tên sự kiện, ngày tham quan, khung giờ, số lượng vé, mã vé và tổng tiền.
- **FR-003**: Vé trong tab "Sắp diễn ra" PHẢI có badge trạng thái "Sắp diễn ra" và hai nút hành động: "Xem vé" (mở trang chi tiết đầy đủ) và "Xem QR" (mở modal full-screen chỉ hiển thị mã QR).
- **FR-003a**: Màn full-screen QR PHẢI hiển thị tối giản: mã QR lớn chiếm phần lớn màn hình, mã vé dạng text bên dưới, và nút đóng để quay lại danh sách.
- **FR-004**: Vé hoàn thành trong tab "Đã kết thúc" PHẢI có badge "Đã hoàn thành" và hai nút: "Đánh giá" và "Đặt lại".
- **FR-005**: Vé đã hủy trong tab "Đã kết thúc" PHẢI có badge "Đã hủy" và chỉ nút "Đặt lại".
- **FR-006**: Màn chi tiết vé PHẢI hiển thị mã QR ở vị trí nổi bật, đủ kích thước để nhân viên quét bằng máy đọc mã.
- **FR-007**: Màn chi tiết vé PHẢI hiển thị đầy đủ: mã vé, tên sự kiện, ngày tham quan, khung giờ, loại vé, số lượng, đơn giá, tổng thanh toán, tên người đặt, số điện thoại, email.
- **FR-008**: Màn chi tiết vé PHẢI hiển thị chính sách hoàn vé với 3 mốc thời gian: trước 48h (100%), trước 24h (50%), muộn hơn (0%).
- **FR-009**: Hệ thống PHẢI hiển thị thông báo động cho biết mức hoàn tiền áp dụng dựa trên thời điểm hiện tại so với sự kiện.
- **FR-010**: Chức năng hủy vé PHẢI yêu cầu người dùng xác nhận trước khi thực hiện, hiển thị rõ mức hoàn tiền sẽ nhận được.
- **FR-011**: Chức năng đổi lịch PHẢI chỉ hiển thị các khung giờ còn chỗ trống cho cùng sự kiện.
- **FR-011a**: Nút "Đổi lịch" PHẢI bị vô hiệu hóa (disabled) hoặc ẩn nếu thời điểm hiện tại còn dưới 24 giờ trước giờ bắt đầu sự kiện, kèm thông báo lý do.
- **FR-012**: Chức năng tải về PHẢI lưu vé dưới dạng có thể đọc được khi offline (ảnh hoặc PDF).
- **FR-013**: Danh sách vé "Sắp diễn ra" PHẢI được sắp xếp theo ngày tham quan tăng dần (ngày gần nhất lên đầu).
- **FR-016**: Cả hai tab danh sách vé PHẢI sử dụng infinite scroll — tải thêm vé khi người dùng cuộn đến cuối, hiển thị loading indicator trong khi tải.
- **FR-014**: Vé PHẢI được lưu cục bộ để có thể xem QR ngay cả khi thiết bị không có kết nối mạng.
- **FR-015**: Mã QR là tĩnh (static), được tạo một lần khi đặt vé thành công và không thay đổi trong suốt vòng đời vé.

### Key Entities

- **Ticket (Vé)**: Đại diện cho một lượt đặt vé. Thuộc tính: mã vé, trạng thái (sắp diễn ra / đã hoàn thành / đã hủy), ngày tham quan, khung giờ, số lượng, tổng tiền, dữ liệu QR tĩnh (cố định theo booking ID), thông tin người đặt.
- **Event (Sự kiện)**: Địa điểm/sự kiện tham quan. Thuộc tính: tên, địa chỉ, ảnh, danh sách khung giờ còn trống.
- **RefundPolicy (Chính sách hoàn vé)**: Quy tắc hoàn tiền theo mốc thời gian trước sự kiện. Thuộc tính: ngưỡng giờ, tỉ lệ hoàn (%).
- **TimeSlot (Khung giờ)**: Khung giờ tham quan có sẵn. Thuộc tính: giờ bắt đầu, giờ kết thúc, số chỗ còn lại.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: Người dùng có thể tìm và mở chi tiết vé cần xuất trình trong vòng dưới 10 giây từ màn hình "Vé của tôi".
- **SC-002**: Mã QR hiển thị và quét được ngay khi màn chi tiết vé mở, không cần tải thêm.
- **SC-003**: 95% người dùng hoàn thành quy trình hủy vé thành công trong lần thử đầu tiên mà không cần hỗ trợ.
- **SC-004**: Vé và mã QR hiển thị được trong chế độ offline trong 100% trường hợp đã xem vé khi có mạng trước đó.
- **SC-005**: Tỉ lệ hủy vé giảm ít nhất 20% sau khi tính năng đổi lịch được triển khai.
- **SC-006**: Danh sách vé (batch đầu tiên) tải và hiển thị trong vòng dưới 2 giây trong điều kiện mạng bình thường; mỗi lần tải thêm (infinite scroll) hoàn thành trong dưới 1 giây.

## Clarifications

### Session 2026-05-07

- Q: Mã QR có thay đổi theo thời gian (time-limited) hay cố định cho suốt vòng đời của vé? → A: QR tĩnh — mã cố định gắn với booking ID, hợp lệ cho đến ngày sự kiện.
- Q: Hai nút "Xem vé" và "Xem QR" trên thẻ vé có hành vi khác nhau như thế nào? → A: "Xem QR" mở modal/màn full-screen chỉ hiện mã QR (tối giản, dễ quét); "Xem vé" mở trang chi tiết vé đầy đủ.
- Q: Người dùng có thể đổi lịch tham quan đến khi nào trước sự kiện? → A: Chỉ cho phép đổi lịch khi còn ít nhất 24 giờ trước giờ bắt đầu sự kiện.
- Q: Sau khi hủy vé, tiền hoàn được xử lý như thế nào? → A: [DEFERRED — cần xác nhận với khách hàng: hoàn tiền tự động về kênh thanh toán gốc, hoặc không hoàn tiền, hoặc hoàn vào ví app]
- Q: Danh sách vé được tải như thế nào khi có nhiều vé? → A: Infinite scroll — tải thêm khi người dùng cuộn đến cuối danh sách.

## Assumptions

- Người dùng đã đăng nhập vào tài khoản — màn "Vé của tôi" chỉ hiển thị vé gắn với tài khoản đang đăng nhập.
- Chính sách hoàn vé (48h/24h/0h) là đồng nhất cho tất cả sự kiện trong giai đoạn đầu; có thể mở rộng per-event sau.
- **[PENDING — cần xác nhận với khách hàng]** Cơ chế hoàn tiền khi hủy vé: tự động hoàn về kênh thanh toán gốc / hoàn vào ví app / không hoàn tiền. Quyết định này ảnh hưởng trực tiếp đến FR-010 và acceptance scenarios của User Story 4.
- Tính năng đổi lịch chỉ áp dụng cho cùng một sự kiện (không đổi sang sự kiện khác).
- Mỗi lượt đặt vé là một vé riêng biệt trong danh sách, kể cả khi đặt nhiều vé cùng một sự kiện.
- Ứng dụng là mobile-first (iOS và Android); không có phiên bản web trong phạm vi spec này.
- Vé được cache cục bộ sau lần xem đầu tiên để hỗ trợ truy cập offline.
