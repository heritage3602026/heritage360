# Feature Specification: Chia sẻ Deeplink và QR Code (Deeplink & QR Code Sharing)

**Feature Branch**: `004-deeplink-qr-share`
**Created**: 2026-05-06
**Status**: Draft
**Input**: User description: "tao spec share deeplink va quet QR de open app" (create spec for sharing deeplink and scanning QR to open app)

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Người dùng chia sẻ nội dung qua Deeplink (Priority: P1)

Người dùng đang xem nội dung trên app (di tích, tour, sản phẩm làng nghề, bài viết) và muốn chia sẻ cho bạn bè. Người dùng nhấn nút "Chia sẻ", chọn "Sao chép link" hoặc "Gửi qua app khác" (Zalo, Messenger, Telegram...), và hệ thống tạo một deeplink độc nhất cho nội dung đó. Khi người nhận bấm vào link, app sẽ mở trực tiếp đến nội dung tương ứng.

**Why this priority**: Đây là nền tảng của toàn bộ tính năng chia sẻ. Không có deeplink thì người dùng không thể chia sẻ nội dung một cách thuận tiện, làm giảm khả năng lan truyền (viral) của app.

**Independent Test**: Có thể test đầy đủ bằng cách: xem nội dung → nhấn chia sẻ → sao chép link → gửi cho người khác → người nhận bấm link → app mở đúng nội dung. Delivers giá trị: người dùng dễ dàng chia sẻ nội dung với bạn bè.

**Acceptance Scenarios**:

1. **Given** người dùng đang xem nội dung (di tích, tour, sản phẩm, bài viết), **When** người dùng nhấn nút "Chia sẻ", **Then** hệ thống hiển thị tùy chọn chia sẻ (Sao chép link, Chia sẻ qua Zalo, Messenger, v.v.)
2. **Given** người dùng chọn "Sao chép link", **When** link được sao chép, **Then** hệ thống tạo deeplink độc nhất cho nội dung và sao chép vào clipboard, hiển thị thông báo "Đã sao chép link"
3. **Given** người dùng chọn "Chia sẻ qua Zalo", **When** hệ thống mở Zalo, **Then** deeplink và thông tin tóm tắt nội dung được hiển thị để gửi
4. **Given** người nhận bấm vào deeplink, **When** người nhận đã cài app, **Then** app mở trực tiếp đến nội dung tương ứng
5. **Given** người nhận bấm vào deeplink, **When** người nhận chưa cài app, **Then** hệ thống chuyển hướng đến App Store/Google Play để tải app

---

### User Story 2 - Người dùng chia sẻ nội dung qua QR Code (Priority: P1)

Người dùng đang xem nội dung trên app (hoặc CMS) và muốn chia sẻ qua QR Code để người khác có thể quét và mở. Người dùng nhấn "Chia sẻ QR", hệ thống tạo QR Code chứa deeplink, người dùng có thể tải ảnh QR về hoặc hiển thị để người khác quét trực tiếp.

**Why this priority**: QR Code là phương thức chia sẻ phổ biến tại Việt Nam, đặc biệt trong môi trường offline (tại điểm du lịch, sự kiện, triển lãm). Không có QR thì mất đi kênh chia sẻ quan trọng này.

**Independent Test**: Có thể test đầy đủ bằng cách: xem nội dung → nhấn chia sẻ QR → tải ảnh QR → người khác quét → app mở đúng nội dung. Delivers giá trị: người dùng có thể chia sẻ trong môi trường offline.

**Acceptance Scenarios**:

1. **Given** người dùng đang xem nội dung, **When** người dùng nhấn "Chia sẻ QR", **Then** hệ thống tạo QR Code chứa deeplink của nội dung
2. **Given** QR Code được tạo, **When** người dùng nhấn "Tải ảnh", **Then** hệ thống tải ảnh QR về thiết bị
3. **Given** người dùng hiển thị QR trên màn hình, **When** người khác quét bằng camera điện thoại, **Then** hệ thống nhận diện deeplink và mở app đến nội dung tương ứng
4. **Given** nhân viên tại điểm du lịch, **When** in QR Code và dán tại vị trí, **Then** khách du lịch quét và mở app để xem thông tin di tích
5. **Given** QR Code được in/in ra, **When** QR bị mờ hoặc hỏng, **Then** hệ thống vẫn có thể quét được nếu QR còn đọc được (mức độ纠错)

---

### User Story 3 - Admin/CMS tạo và quản lý QR Code (Priority: P1)

Admin hoặc nhân viên CMS tạo QR Code cho các di tích, tour, sản phẩm, hoặc campaign marketing. Hệ thống hỗ trợ tạo QR với tùy chọn (kích thước, màu sắc, logo), quản lý danh sách QR đã tạo, theo dõi số lần quét, và xuất file ảnh để in ấn.

**Why this priority**: Không có công cụ tạo QR trên CMS thì nhân viên không thể tạo QR cho các mục đích marketing/offline. Đây là điều kiện tiên quyết để sử dụng QR trong hoạt động kinh doanh.

**Independent Test**: Có thể test đầy đủ bằng cách: đăng nhập CMS → tạo QR → tùy chọn kích thước/màu → tải ảnh → in ấn → quét thử. Delivers giá trị: nhân viên tự chủ tạo QR cho marketing.

**Acceptance Scenarios**:

1. **Given** nhân viên đăng nhập CMS, **When** vào mục "Quản lý QR Code", **Then** hệ thống hiển thị danh sách QR đã tạo với bộ lọc (theo loại, trạng thái, ngày tạo)
2. **Given** nhân viên tạo QR mới, **When** chọn nội dung (di tích/tour/sản phẩm), **Then** hệ thống tạo QR Code chứa deeplink của nội dung đó
3. **Given** nhân viên tạo QR, **When** chọn tùy chọn (kích thước, màu sắc, thêm logo), **Then** hệ thống tạo QR theo tùy chọn và cho phép xem trước
4. **Given** QR đã tạo, **When** nhân viên tải ảnh, **Then** hệ thống xuất file ảnh chất lượng cao (PNG/SVG) để in ấn
5. **Given** danh sách QR, **When** nhân viên xem thống kê, **Then** hệ thống hiển thị số lần quét, ngày quét gần nhất, thiết bị quét

---

### User Story 4 - Người dùng quét QR Code từ camera app (Priority: P2)

Người dùng mở app và nhấn "Quét QR" trong menu hoặc từ màn hình chính. Camera mở ra, người dùng hướng camera vào QR Code, app tự động nhận diện và mở nội dung tương ứng mà không cần chụp ảnh.

**Why this priority**: Quét QR trực tiếp trong app tiện hơn khi người dùng đã có app. Nhưng không phải điều kiện bắt buộc vì người dùng có thể quét bằng camera hệ thống.

**Independent Test**: Có thể test đầy đủ bằng cách: mở app → nhấn "Quét QR" → hướng camera vào QR → app nhận diện và mở nội dung. Delivers giá trị: người dùng quét QR nhanh chóng trong app.

**Acceptance Scenarios**:

1. **Given** người dùng mở app, **When** người dùng nhấn "Quét QR" trong menu, **Then** camera mở với khung hình vuông để quét QR
2. **Given** camera đang mở, **When** người dùng hướng vào QR Code, **Then** hệ thống tự động nhận diện và highlight QR được nhận diện
3. **Given** QR được nhận diện, **When** hệ thống đọc được deeplink, **Then** app mở trực tiếp đến nội dung tương ứng (không cần người dùng xác nhận)
4. **Given** người dùng quét QR không phải của Heritage360, **When** hệ thống nhận diện, **Then** hiển thị thông báo "QR này không thuộc Heritage360" và hỏi người dùng có muốn mở bằng trình duyệt không
5. **Given** camera đang mở, **When** người dùng nhấn nút đóng hoặc back, **Then** camera đóng và quay về màn hình trước đó

---

### User Story 5 - Hệ thống theo dõi và phân tích deeplink/QR (Priority: P3)

Admin xem báo cáo về hiệu quả của deeplink và QR Code: số lần click/quét, nguồn gốc (ứng dụng chia sẻ), thiết bị, vị trí địa lý, tỷ lệ chuyển đổi (số click → số cài app → số người dùng active). Hệ thống hỗ trợ lọc theo thời gian, loại nội dung, campaign cụ thể.

**Why this priority**: Phân tích giúp đánh giá hiệu quả marketing và tối ưu hóa chiến dịch. Nhưng không ảnh hưởng đến luồng vận hành cốt lõi.

**Independent Test**: Có thể test đầy đủ bằng cách: tạo một số deeplink/QR → chia sẻ/quét → vào báo cáo → xác nhận số liệu khớp. Delivers giá trị: có cái nhìn tổng quan về hiệu quả chia sẻ.

**Acceptance Scenarios**:

1. **Given** admin vào "Báo cáo deeplink/QR", **When** chọn khoảng thời gian, **Then** hệ thống hiển thị: tổng click/quét, nguồn gốc, thiết bị, vị trí, tỷ lệ chuyển đổi
2. **Given** admin lọc theo loại nội dung, **When** chọn di tích/tour/sản phẩm, **Then** hệ thống hiển thị số liệu cho loại nội dung đó
3. **Given** admin lọc theo campaign, **When** chọn campaign marketing cụ thể, **Then** hệ thống hiển thị hiệu quả campaign đó
4. **Given** có dữ liệu theo nhiều ngày, **When** admin xem biểu đồ xu hướng, **Then** dữ liệu được hiển thị theo ngày/tuần/tháng

---

### Edge Cases

- Nếu người dùng bấm deeplink nhưng app đang mở ở background? Hệ thống đưa app lên foreground và mở đến nội dung.
- Nếu nội dung của deeplink đã bị xóa hoặc ẩn? Hệ thống hiển thị thông báo "Nội dung không còn tồn tại" và chuyển về trang chủ.
- Nếu QR Code bị bẩn hoặc mờ một phần? Hệ thống vẫn có thể đọc được nhờ cơ chế纠错 (error correction) của QR.
- Nếu người dùng quét QR nhưng không có mạng? Hệ thống lưu deeplink từ QR và thử mở lại khi có mạng.
- Nếu cùng một nội dung được tạo nhiều QR khác nhau? Hệ thống theo dõi từng QR riêng để biết nguồn quét từ đâu.
- Nếu deeplink được tạo từ app phiên bản cũ nhưng app đã update? Deeplink vẫn phải hoạt động tương thích ngược (backward compatible).
- Nếu người dùng chặn quyền camera? Hệ thống hướng dẫn người dùng cấp quyền trong settings.

## Requirements *(mandatory)*

### Functional Requirements

**Tạo và chia sẻ Deeplink (App)**

- **FR-001**: Hệ thống PHẢI tạo deeplink độc nhất cho từng nội dung (di tích, tour, sản phẩm, bài viết)
- **FR-002**: Hệ thống PHẢI cho phép người dùng sao chép deeplink vào clipboard với một thao tác
- **FR-003**: Hệ thống PHẢI tích hợp native share sheet để chia sẻ qua các app khác (Zalo, Messenger, Telegram, v.v.)
- **FR-004**: Hệ thống PHẢI tạo deeplink với cấu trúc chuẩn (domain, path, parameters) để dễ quản lý
- **FR-005**: Hệ thống NÊN bao gồm thông tin tóm tắt (title, description, image) khi chia sẻ để preview đẹp

**Tạo và chia sẻ QR Code (App)**

- **FR-006**: Hệ thống PHẢI tạo QR Code chứa deeplink cho từng nội dung
- **FR-007**: Hệ thống PHẢI cho phép người dùng tải ảnh QR về thiết bị
- **FR-008**: Hệ thống PHẢI tạo QR với chất lượng đủ để in ấn (độ phân giải cao)
- **FR-009**: Hệ thống NÊN cho phép tùy chọn QR (kích thước, màu sắc, thêm logo)

**Xử lý Deeplink khi mở (App)**

- **FR-010**: Hệ thống PHẢI mở app và điều hướng đến nội dung tương ứng khi người dùng bấm deeplink
- **FR-011**: Hệ thống PHẢI xử lý trường hợp app chưa cài: chuyển hướng đến App Store/Google Play
- **FR-012**: Hệ thống PHẢI xử lý trường hợp nội dung đã xóa/ẩn: hiển thị thông báo và chuyển về trang chủ
- **FR-013**: Hệ thống PHẢI hỗ trợ deeplink tương thích ngược (với các phiên bản app cũ hơn)
- **FR-014**: Hệ thống NÊN theo dõi số lần click deeplink để phân tích

**Quét QR Code từ camera (App)**

- **FR-015**: Hệ thống NÊN cho phép người dùng mở camera từ app để quét QR
- **FR-016**: Hệ thống NÊN tự động nhận diện QR Code khi camera hướng vào (không cần chụp ảnh)
- **FR-017**: Hệ thống NÊN mở trực tiếp đến nội dung khi nhận diện deeplink của Heritage360
- **FR-018**: Hệ thống NÊN hiển thị thông báo khi QR không thuộc Heritage360 và hỏi có muốn mở bằng trình duyệt không

**Quản lý QR Code (CMS)**

- **FR-019**: Hệ thống PHẢI cho phép tạo QR Code cho các nội dung (di tích, tour, sản phẩm, campaign)
- **FR-020**: Hệ thống PHẢI cho phép tùy chọn QR (kích thước, màu sắc, thêm logo)
- **FR-021**: Hệ thống PHẢI cho phép xem trước QR trước khi tải
- **FR-022**: Hệ thống PHẢI xuất file ảnh chất lượng cao (PNG/SVG) để in ấn
- **FR-023**: Hệ thống PHẢI cho phép quản lý danh sách QR đã tạo (xem, sửa, xóa)
- **FR-024**: Hệ thống NÊN cho phép gán tên/chú thích cho QR để dễ quản lý

**Theo dõi và phân tích (CMS)**

- **FR-025**: Hệ thống NÊN theo dõi số lần quét cho mỗi QR Code
- **FR-026**: Hệ thống NÊN theo dõi số lần click cho mỗi deeplink
- **FR-027**: Hệ thống NÊN hiển thị thông tin chi tiết: thiết bị, vị trí, ngày giờ, nguồn gốc (nếu có)
- **FR-028**: Hệ thống NÊN cho phép lọc theo thời gian, loại nội dung, campaign
- **FR-029**: Hệ thống NÊN hiển thị biểu đồ xu hướng theo thời gian

### Key Entities

- **Deeplink**: Link độc nhất đại diện cho một nội dung trong app, bao gồm URL, tham số (content type, content ID), campaign tags (nếu có), ngày tạo, số lần click, trạng thái (hoạt động/hết hạn).
- **QR Code**: Mã QR chứa deeplink, bao gồm ID, deeplink liên kết, hình ảnh (file), tùy chọn (kích thước, màu sắc, logo), tên/chú thích, số lần quét, ngày tạo, người tạo, trạng thái (hoạt động/ẩn).
- **QR Scan Event**: Sự kiện quét QR, bao gồm QR ID, deeplink được quét, thiết bị (type, OS), vị trí (nếu có权限), ngày giờ, nguồn (app quét hay camera hệ thống), kết quả (thành công/thất bại, lỗi nếu có).
- **Deeplink Click Event**: Sự kiện click deeplink, bao gồm deeplink ID, thiết bị, vị trí, ngày giờ, nguồn (app chia sẻ), kết quả (mở app/chuyển app store/tải app), referral code (nếu có).
- **Campaign**: Chiến dịch marketing sử dụng deeplink/QR, bao gồm tên, mô tả, danh sách deeplink/QR, thời gian chạy, mục tiêu (số click/quét), trạng thái (hoạt động/đã kết thúc).

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: Người dùng có thể tạo và chia sẻ deeplink trong 3 giây
- **SC-002**: 95% deeplink được mở thành công (app mở đúng nội dung)
- **SC-003**: 90% QR Code được quét thành công (đọc được và mở nội dung)
- **SC-004**: Người dùng nhận được nội dung đúng trong 2 giây sau khi bấm deeplink/quét QR
- **SC-005**: Admin có thể tạo QR Code trong 2 phút
- **SC-006**: Hệ thống theo dõi chính xác 100% sự kiện click/quét
- **SC-007**: Tỷ lệ chuyển đổi (click → cài app → active) tăng ít nhất 20% so với trước khi có deeplink
- **SC-008**: 80% người dùng hài lòng với tính năng chia sẻ (khảo sát)

## Assumptions

- Ứng dụng sử dụng Universal Link (iOS) và App Link (Android) cho deeplink
- Domain chính thức cho deeplink: heritage360.vn hoặc subdomain như go.heritage360.vn
- [NEEDS CLARIFICATION: Cấu trúc deeplink — Định dạng URL như thế nào? Ví dụ: heritage360.vn/di-tít/{id} hay heritage360.vn://di-tit/{id}?]
- [NEEDS CLARIFICATION: Quản lý deeplink — Cần hệ thống tạo short URL (bit.ly style) hay dùng URL đầy đủ?]
- [NEEDS CLARIFICATION: Tracking — Cần tracking chi tiết đến mức nào? Có cần referral code, UTM parameters không?]
- QR Code sử dụng mức độ纠错 thấp (Level L hoặc M) để cân bằng giữa kích thước và khả năng đọc
- Hệ thống có sẵn cơ chế xác thực để phân biệt deeplink hợp lệ
- App đã có cơ chế deep linking cơ bản (handle URL scheme)
- Camera chỉ dùng để quét QR, không lưu ảnh
- Theo dõi vị trí cần permission từ người dùng (không bắt buộc)
