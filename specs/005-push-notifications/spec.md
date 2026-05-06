# Feature Specification: Thông báo đẩy (Push Notifications)

**Feature Branch**: `005-push-notifications`
**Created**: 2026-05-06
**Status**: Draft
**Input**: User description: "Làm chức năng thông báo ở mobile app — gửi push notification khi có sự kiện mới phù hợp với khu vực hoặc sở thích, nhắc trước khi sự kiện đã lưu sắp diễn ra, Trung tâm Thông báo với trạng thái đã đọc/chưa đọc, deep link từ thông báo"

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Người dùng nhận thông báo về sự kiện mới (Priority: P1)

Người dùng đã cài app và cấp quyền thông báo. Khi hệ thống có sự kiện mới phù hợp với khu vực địa lý hoặc sở thích của người dùng, hệ thống gửi push notification đến thiết bị. Người dùng nhận thông báo trên màn hình khóa hoặc trong trung tâm thông báo, có thể bấm để mở app đến chi tiết sự kiện.

**Why this priority**: Đây là luồng nghiệp vụ cốt lõi nhất — không có thông báo thì người dùng không biết về sự kiện mới, làm giảm tương tác và doanh thu.

**Independent Test**: Có thể test đầy đủ bằng cách: tạo sự kiện mới phù hợp với sở thích người dùng → gửi thông báo → người dùng nhận → bấm thông báo → app mở đúng sự kiện. Delivers giá trị: người dùng được thông báo về sự kiện phù hợp.

**Acceptance Scenarios**:

1. **Given** người dùng đã cấp quyền thông báo, **When** hệ thống tạo sự kiện mới phù hợp với khu vực/sở thích, **Then** hệ thống gửi push notification với tiêu đề, nội dung tóm tắt, hình ảnh (nếu có)
2. **Given** người dùng nhận thông báo, **When** người dùng bấm vào thông báo, **Then** app mở và điều hướng đến màn hình chi tiết sự kiện tương ứng
3. **Given** người dùng nhận thông báo, **When** người dùng xóa/thao tác khác (không bấm), **Then** thông báo được lưu trong Trung tâm Thông báo với trạng thái "chưa đọc"
4. **Given** người dùng đã tắt thông báo trong settings, **When** hệ thống gửi thông báo, **Then** thông báo không được hiển thị nhưng vẫn được lưu trong Trung tâm Thông báo
5. **Given** người dùng có nhiều thiết bị, **When** hệ thống gửi thông báo, **Then** thông báo được gửi đến tất cả thiết bị đang đăng nhập

---

### User Story 2 - Người dùng nhận thông báo nhắc sự kiện đã lưu (Priority: P1)

Người dùng đã lưu một sự kiện (thêm vào mục "Sự kiện của tôi"). Hệ thống tự động gửi thông báo nhắc trước khi sự kiện diễn ra: trước 1 ngày và trước 1 giờ (người dùng có thể tùy chỉnh). Thông báo chứa thông tin thời gian, địa điểm và nút "Xem chi tiết" để mở app.

**Why this priority**: Thông báo nhắc giúp người dùng không bỏ lỡ sự kiện đã quan tâm, tăng tỷ lệ tham gia và hài lòng.

**Independent Test**: Có thể test đầy đủ bằng cách: lưu sự kiện → đợi đến thời điểm nhắc → nhận thông báo → bấm → app mở đúng sự kiện. Delivers giá trị: người dùng được nhắc kịp thời về sự kiện đã lưu.

**Acceptance Scenarios**:

1. **Given** người dùng đã lưu sự kiện, **When** đến thời điểm nhắc (trước 1 ngày/1 giờ), **Then** hệ thống gửi thông báo nhắc với nội dung "[Tên sự kiện] sắp diễn ra vào [thời gian]"
2. **Given** người dùng nhận thông báo nhắc, **When** người dùng bấm, **Then** app mở đến màn hình chi tiết sự kiện với vé/đăng ký (nếu có)
3. **Given** người dùng đã lưu sự kiện, **When** người dùng chỉnh sửa thời gian nhắc trong settings, **Then** hệ thống gửi thông báo theo thời gian mới
4. **Given** người dùng xóa sự kiện đã lưu, **When** sự kiện sắp diễn ra, **Then** hệ thống KHÔNG gửi thông báo nhắc
5. **Given** người dùng tắt thông báo nhắc cho một sự kiện cụ thể, **When** đến thời điểm nhắc, **Then** hệ thống KHÔNG gửi thông báo cho sự kiện đó

---

### User Story 3 - Người dùng xem Trung tâm Thông báo (Priority: P1)

Người dùng mở app và vào mục "Trung tâm Thông báo" từ menu hoặc icon thông báo trên thanh điều hướng. Hệ thống hiển thị danh sách tất cả thông báo đã nhận, được nhóm theo thời gian (hôm nay, tuần này, tháng này, cũ hơn). Mỗi thông báo hiển thị tiêu đề, nội dung tóm tắt, thời gian, hình ảnh (nếu có) và trạng thái (chưa đọc/vừa đọc/đã đọc).

**Why this priority**: Trung tâm Thông báo là nơi người dùng xem lại toàn bộ thông báo, không thể xem được thông báo đã xóa trên hệ điều hành.

**Independent Test**: Có thể test đầy đủ bằng cách: mở app → vào Trung tâm Thông báo → xem danh sách → xem chi tiết thông báo. Delivers giá trị: người dùng không bỏ lỡ thông báo đã xóa nhầm.

**Acceptance Scenarios**:

1. **Given** người dùng vào "Trung tâm Thông báo", **When** trang tải xong, **Then** hệ thống hiển thị danh sách thông báo nhóm theo thời gian với chỉ thị trạng thái (chưa đọc: đậm, icon; đã đọc: nhạt)
2. **Given** người dùng xem danh sách, **When** có thông báo chưa đọc, **Then** hệ thống hiển thị số lượng thông báo chưa đọc trên icon/trung tâm
3. **Given** người dùng bấm vào thông báo, **When** thông báo chưa được đọc, **Then** hệ thống đánh dấu đã đọc và điều hướng đến nội dung liên quan (sự kiện, vé, v.v.)
4. **Given** người dùng bấm vào thông báo, **When** thông báo đã được đọc, **Then** hệ thống chỉ điều hướng đến nội dung liên quan
5. **Given** người dùng ở trong Trung tâm Thông báo, **When** có thông báo mới đến, **Then** danh sách tự động cập nhật và thông báo mới xuất hiện ở đầu danh sách

---

### User Story 4 - Người dùng quản lý trạng thái đã đọc/chưa đọc (Priority: P2)

Người dùng trong Trung tâm Thông báo muốn quản lý trạng thái thông báo. Người dùng có thể đánh dấu từng thông báo là đã đọc, đánh dấu tất cả là đã đọc, hoặc xóa thông báo. Hệ thống cập nhật trạng thái realtime và đồng bộ trên tất cả thiết bị.

**Why this priority**: Quản lý trạng thái giúp người dùng tổ chức thông báo, nhưng không phải điều kiện để luồng thông báo cơ bản hoạt động.

**Independent Test**: Có thể test đầy đủ bằng cách: có thông báo chưa đọc → đánh dấu đã đọc/xóa → kiểm tra trạng thái. Delivers giá trị: người dùng tự chủ quản lý thông báo.

**Acceptance Scenarios**:

1. **Given** người dùng trong Trung tâm Thông báo, **When** người dùng vuốt trái thông báo hoặc nhấn giữ, **Then** hệ thống hiển thị tùy chọn (Đánh dấu đã đọc, Xóa)
2. **Given** người dùng chọn "Đánh dấu đã đọc", **When** người dùng chọn, **Then** thông báo chuyển sang trạng thái đã đọc (màu nhạt hơn)
3. **Given** người dùng có nhiều thông báo chưa đọc, **When** người dùng nhấn "Đánh dấu tất cả đã đọc", **Then** tất cả thông báo chuyển sang trạng thái đã đọc và số lượng chưa đọc về 0
4. **Given** người dùng xóa thông báo, **When** người dùng xác nhận xóa, **Then** thông báo bị xóa khỏi danh sách và không thể khôi phục
5. **Given** người dùng xóa hoặc đánh dấu đã đọc, **When** người dùng mở app trên thiết bị khác, **Then** trạng thái đã được đồng bộ

---

### User Story 5 - Người dùng tùy chỉnh cài đặt thông báo (Priority: P2)

Người dùng vào Cài đặt → Thông báo để tùy chỉnh các loại thông báo muốn nhận: sự kiện mới (theo khu vực, sở thích), nhắc sự kiện đã lưu, khuyến mãi, tin tức. Người dùng cũng có thể tùy chỉnh thời gian nhắc (trước 1 ngày, 1 giờ, 24 giờ, tùy chỉnh) và tắt thông báo hoàn toàn.

**Why this priority**: Tùy chỉnh giúp người dùng chỉ nhận thông báo thực sự cần thiết, giảm làm phiền và tăng tương tác. Nhưng không phải điều kiện bắt buộc.

**Independent Test**: Có thể test đầy đủ bằng cách: vào Cài đặt → tắt thông báo sự kiện mới → tạo sự kiện mới → không nhận thông báo. Delivers giá trị: người dùng kiểm soát được thông báo nhận.

**Acceptance Scenarios**:

1. **Given** người dùng vào Cài đặt → Thông báo, **When** người dùng xem, **Then** hệ thống hiển thị các loại thông báo với toggle (bật/tắt): sự kiện mới, nhắc sự kiện, khuyến mãi, tin tức
2. **Given** người dùng tắt "Sự kiện mới", **When** hệ thống gửi thông báo sự kiện mới, **Then** người dùng KHÔNG nhận thông báo
3. **Given** người dùng chỉnh thời gian nhắc, **When** người dùng chọn "Trước 24 giờ", **Then** hệ thống gửi thông báo nhắc 24 giờ trước sự kiện (thay vì 1 ngày/1 giờ mặc định)
4. **Given** người dùng tắt "Nhắc sự kiện", **When** người dùng lưu sự kiện, **Then** hệ thống KHÔNG gửi thông báo nhắc cho sự kiện đó
5. **Given** người dùng tắt tất cả thông báo, **When** bất kỳ thông báo nào được gửi, **Then** người dùng KHÔNG nhận nhưng thông báo vẫn được lưu trong Trung tâm Thông báo

---

### User Story 6 - Admin/CMS gửi thông báo và quản lý template (Priority: P1)

Admin hoặc nhân viên CMS tạo và gửi thông báo push cho tất cả người dùng hoặc phân đoạn cụ thể (theo khu vực, sở thích, hành vi). Hệ thống hỗ trợ tạo template thông báo, lên lịch gửi, theo dõi tỷ lệ gửi/thành công/thất bại, và báo cáo hiệu quả (số người nhận, số mở, số bấm).

**Why this priority**: Không có công cụ gửi thông báo trên CMS thì không thể gửi thông báo. Đây là điều kiện tiên quyết để hoạt động thông báo.

**Independent Test**: Có thể test đầy đủ bằng cách: đăng nhập CMS → tạo thông báo → chọn phân đoạn → gửi → kiểm tra người dùng nhận. Delivers giá trị: admin tự chủ gửi thông báo.

**Acceptance Scenarios**:

1. **Given** admin đăng nhập CMS, **When** vào "Gửi thông báo", **Then** hệ thống hiển thị form: tiêu đề, nội dung, hình ảnh, phân đoạn người dùng, thời gian gửi (ngay/lên lịch)
2. **Given** admin tạo thông báo, **When** admin chọn phân đoạn (khu vực, sở thích), **Then** hệ thống chỉ gửi thông báo cho người dùng phù hợp
3. **Given** admin chọn gửi ngay, **When** admin nhấn "Gửi", **Then** hệ thống gửi thông báo ngay và hiển thị tiến trình gửi
4. **Given** admin lên lịch gửi, **When** đến thời điểm, **Then** hệ thống tự động gửi thông báo
5. **Given** thông báo đã gửi, **When** admin xem báo cáo, **Then** hệ thống hiển thị: số người nhận, số thành công, số thất bại, số mở, số bấm

---

### User Story 7 - Hệ thống tự động tạo thông báo từ các sự kiện (Priority: P1)

Hệ thống tự động tạo và gửi thông báo khi có sự kiện mới (tự động dựa trên cấu hình) và nhắc sự kiện đã lưu (tự động dựa trên thời gian). Admin có thể cấu hình quy tắc tự động: loại thông báo, điều kiện kích hoạt, nội dung template, thời gian gửi.

**Why this priority**: Tự động hóa giảm công việc thủ công cho admin và đảm bảo thông báo được gửi kịp thời.

**Independent Test**: Có thể test đầy đủ bằng cách: cấu hình quy tắc tự động → tạo sự kiện mới → hệ thống tự gửi thông báo. Delivers giá trị: không cần can thiệp thủ công.

**Acceptance Scenarios**:

1. **Given** admin cấu hình quy tắc "Sự kiện mới", **When** hệ thống tạo sự kiện mới, **Then** hệ thống tự động tạo và gửi thông báo cho người dùng phù hợp
2. **Given** người dùng lưu sự kiện, **When** sự kiện sắp diễn ra, **Then** hệ thống tự động gửi thông báo nhắc theo thời gian cấu hình (trước 1 ngày, 1 giờ)
3. **Given** admin chỉnh sửa template thông báo, **When** quy tắc tự động kích hoạt, **Then** hệ thống gửi thông báo với template mới
4. **Given** admin tắt quy tắc tự động, **When** điều kiện kích hoạt xảy ra, **Then** hệ thống KHÔNG gửi thông báo
5. **Given** thông báo tự động gửi thất bại, **When** hệ thống ghi log, **Then** admin có thể xem danh sách thất bại và gửi lại

---

### Edge Cases

- Nếu người dùng tắt quyền thông báo trong settings hệ điều hành? App vẫn lưu thông báo trong Trung tâm Thông báo nhưng không hiển thị push.
- Nếu người dùng offline khi có thông báo gửi? Thông báo được lưu trên server và gửi khi người dùng online lại.
- Nếu người dùng có nhiều thiết bị? Thông báo gửi đến tất cả, khi đọc trên một thiết bị thì đánh dấu đã đọc trên tất cả.
- Nếu thông báo gửi quá nhiều (spam)? Hệ thống có cơ chế rate limiting để tránh làm phiền người dùng.
- Nếu nội dung liên quan của thông báo đã bị xóa? Hệ thống hiển thị thông báo "Nội dung không còn tồn tại" thay vì crash.
- Nếu thông báo nhắc được gửi nhưng sự kiện bị hủy? Hệ thống nên gửi thông báo xin lỗi hoặc cập nhật.
- Nếu người dùng tắt app/gỡ cài? Thông báo vẫn được gửi nhưng sẽ không thể mở app (chuyển hướng đến App Store).

## Requirements *(mandatory)*

### Functional Requirements

**Nhận thông báo Push (App)**

- **FR-001**: Hệ thống PHẢI gửi push notification khi có sự kiện mới phù hợp với khu vực hoặc sở thích của người dùng
- **FR-002**: Hệ thống PHẢI gửi thông báo nhắc trước khi sự kiện đã lưu sắp diễn ra (mặc định: trước 1 ngày, trước 1 giờ)
- **FR-003**: Hệ thống PHẢY yêu cầu quyền thông báo từ người dùng khi lần đầu sử dụng
- **FR-004**: Hệ thống PHẢI hiển thị thông báo với tiêu đề, nội dung tóm tắt, hình ảnh (nếu có)
- **FR-005**: Hệ thống PHẢI điều hướng đến đúng màn hình nội dung liên quan khi người dùng bấm thông báo (deeplink)
- **FR-006**: Hệ thống NÊN hỗ trợ rich notification với nút hành động (Xem chi tiết, Lưu, Đóng)
- **FR-007**: Hệ thống NÊN hỗ trợ notification grouping để gom nhiều thông báo cùng loại

**Trung tâm Thông báo (App)**

- **FR-008**: Hệ thống PHẢI có mục "Trung tâm Thông báo" để xem lại toàn bộ thông báo
- **FR-009**: Hệ thống PHẢI hiển thị danh sách thông báo nhóm theo thời gian (hôm nay, tuần này, tháng này, cũ hơn)
- **FR-010**: Hệ thống PHẢI hiển thị trạng thái đã đọc/chưa đọc cho từng thông báo
- **FR-011**: Hệ thống PHẢI hiển thị số lượng thông báo chưa đọc trên icon/trung tâm
- **FR-012**: Hệ thống PHẢI điều hướng đến nội dung liên quan và đánh dấu đã đọc khi bấm thông báo
- **FR-013**: Hệ thống PHẢI cập nhật danh sách realtime khi có thông báo mới
- **FR-014**: Hệ thống NÊN cho phép vuốt để thực hiện hành động nhanh (đánh dấu đã đọc, xóa)

**Quản lý trạng thái (App)**

- **FR-015**: Hệ thống PHẢI cho phép đánh dấu từng thông báo là đã đọc
- **FR-016**: Hệ thống PHẢI cho phép đánh dấu tất cả thông báo là đã đọc
- **FR-017**: Hệ thống PHẢI cho phép xóa thông báo
- **FR-018**: Hệ thống PHẢI đồng bộ trạng thái đã đọc/chưa đọc trên tất cả thiết bị của người dùng
- **FR-019**: Hệ thống NÊN cho phép xóa tất cả thông báo đã đọc

**Cài đặt thông báo (App)**

- **FR-020**: Hệ thống NÊN cho phép tùy chỉnh các loại thông báo muốn nhận (sự kiện mới, nhắc sự kiện, khuyến mãi, tin tức)
- **FR-021**: Hệ thống NÊN cho phép tùy chỉnh thời gian nhắc (trước 1 ngày, 1 giờ, 24 giờ, tùy chỉnh)
- **FR-022**: Hệ thống NÊN cho phép tắt thông báo hoàn toàn
- **FR-023**: Hệ thống NÊN cho phép tắt thông báo nhắc cho từng sự kiện cụ thể

**Gửi thông báo (CMS)**

- **FR-024**: Hệ thống PHẢI cho phép tạo và gửi thông báo push từ CMS
- **FR-025**: Hệ thống PHẢI cho phép chọn phân đoạn người dùng (theo khu vực, sở thích, hành vi)
- **FR-026**: Hệ thống PHẢI cho phép gửi ngay hoặc lên lịch gửi
- **FR-027**: Hệ thống PHẢI hiển thị tiến trình gửi (số người nhận, số thành công, số thất bại)
- **FR-028**: Hệ thống PHẢI cho phép tạo template thông báo (tiêu đề, nội dung, hình ảnh)
- **FR-029**: Hệ thống NÊN cho phép xem báo cáo hiệu quả (số người nhận, số mở, số bấm)
- **FR-030**: Hệ thống NÊN cho phép gửi lại thông báo cho trường hợp thất bại

**Tự động thông báo (System)**

- **FR-031**: Hệ thống PHẢI tự động gửi thông báo khi có sự kiện mới phù hợp
- **FR-032**: Hệ thống PHẢI tự động gửi thông báo nhắc trước sự kiện đã lưu
- **FR-033**: Hệ thống PHẢI cho phép cấu hình quy tắc tự động (loại, điều kiện, template, thời gian)
- **FR-034**: Hệ thống PHẢI ghi log khi gửi thất bại và cho phép gửi lại
- **FR-035**: Hệ thống NÊN có cơ chế rate limiting để tránh spam

### Key Entities

- **Thông báo (Notification)**: Thông báo đẩy gửi đến người dùng, bao gồm ID, loại (sự kiện mới, nhắc sự kiện, khuyến mãi, tin tức), tiêu đề, nội dung, hình ảnh, deeplink, người nhận (tất cả hoặc phân đoạn), thời gian gửi, trạng thái (chờ gửi, đang gửi, đã gửi, thất bại), số người nhận, số thành công, số thất bại.
- **Trạng thái thông báo người dùng (User Notification Status)**: Trạng thái của thông báo đối với từng người dùng, bao gồm thông báo ID, người dùng ID, trạng thái đã đọc (chưa đọc/đã đọc), thời gian đọc, thời gian bấm (nếu có), thiết bị đã đọc/bấm.
- **Cài đặt thông báo (Notification Settings)**: Cài đặt thông báo của người dùng, bao gồm người dùng ID, loại thông báo (sự kiện mới, nhắc sự kiện, khuyến mãi, tin tức), bật tắt, thời gian nhắc mặc định, tùy chỉnh nhắc theo sự kiện.
- **Template thông báo (Notification Template)**: Mẫu thông báo dùng cho tự động, bao gồm ID, loại, tên template, template tiêu đề, template nội dung, biến động (event_name, time, location...), hình ảnh mặc định, trạng thái (hoạt động/ẩn).
- **Quy tắc tự động (Auto Notification Rule)**: Quy tắc kích hoạt thông báo tự động, bao gồm ID, loại, điều kiện kích hoạt (mục kích hoạt, bộ lọc), template sử dụng, phân đoạn người dùng, thời gian gửi, trạng thái (bật/tắt).
- **Lịch gửi thông báo (Notification Schedule)**: Lịch gửi thông báo, bao gồm ID, thông báo, thời gian gửi, phân đoạn, trạng thái (chờ gửi, đang gửi, đã gửi, thất bại), kết quả (số người nhận, số thành công, số thất bại).
- **Sự kiện đã lưu (Saved Event Reminder)**: Sự kiện người dùng đã lưu và cần nhắc, bao gồm người dùng ID, sự kiện ID, thời gian nhắc (trước 1 ngày, 1 giờ, tùy chỉnh), trạng thái (chờ nhắc, đã nhắc, đã hủy).

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: 95% thông báo được gửi thành công đến người dùng đã cấp quyền
- **SC-002**: 80% người dùng mở thông báo trong vòng 5 phút sau khi nhận
- **SC-003**: Tỷ lệ bấm vào thông báo (CTR) đạt ít nhất 20%
- **SC-004**: Người dùng nhận thông báo nhắc đúng thời điểm (trước 1 ngày/1 giờ) trong 99% trường hợp
- **SC-005**: Trung tâm Thông báo tải trong vòng 2 giây
- **SC-006**: Trạng thái đã đọc/chưa đọc đồng bộ trên tất cả thiết bị trong vòng 5 giây
- **SC-007**: Admin có thể tạo và gửi thông báo trong vòng 3 phút
- **SC-008**: 70% người dùng bật ít nhất một loại thông báo

## Assumptions

- Hệ thống sử dụng Firebase Cloud Messaging (FCM) làm nền tảng push notification chính thức — FCM sẽ tự động xử lý việc gửi đến cả Android (trực tiếp) và iOS (định tuyến qua APNs)
- Người dùng có thể tắt thông báo trong settings của hệ điều hành
- Thông báo được lưu tối thiểu 30 ngày trong Trung tâm Thông báo
- Mỗi người dùng có thể nhận tối đa 10 thông báo mỗi ngày (rate limiting)
- Thông báo nhắc mặc định: trước 1 ngày và trước 1 giờ
- Hệ thống có sẵn dữ liệu về sở thích và khu vực của người dùng (từ Travel Profile, user settings)
- Hệ thống có sẵn dịch vụ gửi email (fallback khi push thất bại)
- Deeplink từ thông báo sử dụng cấu trúc đã được xác định trong feature deeplink/QR
- Admin có thể gửi thông báo mass cho tối đa 100.000 người dùng cùng lúc
- Thông báo được ưu tiên gửi trong giờ hành động (8h-20h) trừ thông báo khẩn cấp

## Clarifications

### Session 2026-05-06

- Q: Hệ thống push notification sử dụng dịch vụ nào? → A: Sử dụng Firebase Cloud Messaging (FCM) làm nền tảng push notification chính thức
- Q: Xử lý thông báo cho iOS như thế nào? → A: FCM sẽ tự động định tuyến tới APNs (Apple Push Notification Service) khi gửi đến thiết bị iOS
