# Feature Specification: Thương mại điện tử làng nghề (Craft Village E-commerce)

**Feature Branch**: `003-craft-village-commerce`
**Created**: 2026-05-06
**Status**: Draft
**Input**: User description: "Doc file @features/HaNoi360_Phase2_FeatureBreakdown.xlsx tao spec cho feature thuong mai dien tu shop lang nghe" (Create spec for craft village e-commerce shop feature)

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Khách du lịch xem và tìm kiếm sản phẩm làng nghề (Priority: P1)

Khách du lịch mở app Hà Nội 360, vào mục "Shop làng nghề" và xem danh sách các sản phẩm thủ công mỹ nghệ từ các làng nghề truyền thống. Khách có thể tìm kiếm theo tên sản phẩm, lọc theo loại (gốm, nồm, mây tre đan, lụa, đồng, giấy dó...), lọc theo làng nghề, lọc theo mức giá, và xem chi tiết sản phẩm với hình ảnh, mô tả, giá, thông tin làng nghề nguồn gốc.

**Why this priority**: Đây là nền tảng của toàn bộ tính năng thương mại điện tử. Không có chức năng xem/tìm kiếm sản phẩm thì không có giao dịch nào diễn ra.

**Independent Test**: Có thể test đầy đủ bằng cách: mở app → vào Shop làng nghề → tìm kiếm/lọc → xem chi tiết sản phẩm. Delivers giá trị: khách khám phá sản phẩm làng nghề một cách dễ dàng.

**Acceptance Scenarios**:

1. **Given** khách vào mục "Shop làng nghề", **When** trang tải xong, **Then** hệ thống hiển thị danh sách sản phẩm với hình ảnh, tên, giá, tên làng nghề
2. **Given** khách đang xem danh sách, **When** khách nhập từ khóa tìm kiếm, **Then** hệ thống lọc và hiển thị sản phẩm phù hợp
3. **Given** khách đang xem danh sách, **When** khách chọn lọc theo loại/làng nghề/giá, **Then** hệ thống cập nhật danh sách theo bộ lọc
4. **Given** khách xem danh sách, **When** khách nhấn vào sản phẩm, **Then** hệ thống chuyển sang trang chi tiết với hình ảnh đầy đủ, mô tả, giá, thông tin làng nghề, đánh giá từ khách hàng
5. **Given** khách đang xem sản phẩm, **When** làng nghề sản phẩm có liên quan đến di tích, **Then** hệ thống hiển thị thông tin về di tích liên quan để khách tham khảo

---

### User Story 2 - Khách du lịch thêm sản phẩm vào giỏ hàng và thanh toán (Priority: P1)

Khách du lịch chọn sản phẩm, chọn số lượng, thêm vào giỏ hàng, và tiến hành thanh toán. Hệ thống hỗ trợ giỏ hàng, tính phí vận chuyển, và tích hợp thanh toán online qua cổng thanh toán. Shop làng nghề hoạt động độc lập với hệ thống đặt vé.

**Why this priority**: Đây là luồng nghiệp vụ cốt lõi để tạo doanh thu. Không có chức năng thêm vào giỏ và thanh toán thì không có giao dịch nào hoàn tất.

**Independent Test**: Có thể test đầy đủ bằng cách: chọn sản phẩm → thêm vào giỏ → thanh toán → nhận xác nhận. Delivers giá trị: khách mua sản phẩm làng nghề một cách thuận tiện.

**Acceptance Scenarios**:

1. **Given** khách đang xem chi tiết sản phẩm, **When** khách chọn số lượng và nhấn "Thêm vào giỏ", **Then** hệ thống thêm sản phẩm vào giỏ hàng và hiển thị thông báo thành công
2. **Given** khách đã thêm sản phẩm vào giỏ, **When** khách vào giỏ hàng, **Then** hệ thống hiển thị danh sách sản phẩm, số lượng, giá từng sản phẩm, tổng tiền, phí vận chuyển
4. **Given** khách ở giỏ hàng, **When** khách thay đổi số lượng hoặc xóa sản phẩm, **Then** hệ thống cập nhật tổng tiền
5. **Given** khách ở giỏ hàng, **When** khách nhấn "Thanh toán", **Then** hệ thống chuyển đến trang nhập thông tin giao hàng (họ tên, SĐT, địa chỉ)
6. **Given** khách đã nhập thông tin giao hàng, **When** khách chọn phương thức thanh toán và thanh toán thành công, **Then** hệ thống tạo đơn hàng, gửi xác nhận, và giảm số lượng tồn kho

---

### User Story 3 - Khách du lịch theo dõi đơn hàng và lịch sử mua hàng (Priority: P2)

Khách du lịch sau khi đặt hàng có thể theo dõi trạng thái đơn hàng (chờ xác nhận, đang chuẩn bị, đang giao, đã giao, đã hủy) và xem lịch sử mua hàng với chi tiết từng đơn. Khách có thể hủy đơn khi chưa giao và yêu cầu hoàn tiền.

**Why this priority**: Theo dõi đơn hàng là yếu tố quan trọng cho trải nghiệm sau mua. Nhưng không phải điều kiện để luồng mua hàng cơ bản hoạt động.

**Independent Test**: Có thể test đầy đủ bằng cách: đặt hàng → xem trạng thái đơn → hủy đơn (nếu chưa giao). Delivers giá trị: khách tự chủ theo dõi và quản lý đơn hàng.

**Acceptance Scenarios**:

1. **Given** khách đã đặt hàng thành công, **When** khách vào mục "Đơn hàng của tôi", **Then** hệ thống hiển thị danh sách đơn theo trạng thái với đầy đủ thông tin (mã đơn, ngày, sản phẩm, tổng tiền, trạng thái)
2. **Given** khách xem chi tiết đơn, **When** đơn đang xử lý, **Then** hệ thống hiển thị timeline trạng thái (đã đặt → đang chuẩn bị → đang giao → đã giao)
3. **Given** đơn chưa giao, **When** khách nhấn "Hủy đơn", **Then** hệ thống hủy đơn, hoàn tiền, và tăng lại tồn kho
4. **Given** đơn đang giao, **When** khách xem chi tiết, **Then** hệ thống hiển thị thông tin shipper và liên hệ nếu cần
5. **Given** đơn đã giao, **When** khách xem lịch sử, **Then** khách có thể đánh giá sản phẩm và viết nhận xét

---

### User Story 4 - Khách du lịch xem và đánh giá sản phẩm (Priority: P2)

Khách du lịch sau khi nhận sản phẩm có thể đánh giá sản phẩm (số sao) và viết nhận xét. Khách khác có thể xem các đánh giá này khi quyết định mua.

**Why this priority**: Đánh giá sản phẩm giúp khách khác đưa ra quyết định và tăng độ tin cậy. Nhưng không phải điều kiện để luồng mua hàng cơ bản hoạt động.

**Independent Test**: Có thể test đầy đủ bằng cách: mua và nhận hàng → đánh giá sản phẩm → xem đánh giá hiển thị. Delivers giá trị: khách có thông tin từ người mua trước để quyết định.

**Acceptance Scenarios**:

1. **Given** khách đã nhận hàng (trạng thái "đã giao"), **When** khách vào chi tiết đơn hoặc lịch sử, **Then** hệ thống hiển thị nút "Đánh giá sản phẩm"
2. **Given** khách nhấn "Đánh giá sản phẩm", **When** khách chọn số sao và viết nhận xét, **Then** hệ thống lưu đánh giá và hiển thị trên trang sản phẩm
3. **Given** khách đang xem sản phẩm, **When** sản phẩm có đánh giá, **Then** hệ thống hiển thị đánh giá trung bình (sao) và danh sách nhận xét
4. **Given** khách xem đánh giá, **When** khách nhấn "Xem thêm", **Then** hệ thống hiển thị tất cả đánh giá với bộ lọc (theo số sao, có hình ảnh, mới nhất)
5. **Given** khách đã đánh giá, **When** khách muốn chỉnh sửa, **Then** hệ thống cho phép sửa hoặc xóa đánh giá trong vòng 7 ngày

---

### User Story 5 - Admin/CMS quản lý sản phẩm và kho (Priority: P1)

Admin hoặc nhân viên CMS đăng nhập và quản lý danh mục sản phẩm, thêm/sửa/xóa sản phẩm, quản lý tồn kho, cấu hình giá, khuyến mãi, và gắn sản phẩm với làng nghề/di tích. Hệ thống hỗ trợ quản lý hình ảnh, thông số kỹ thuật, và mô tả chi tiết.

**Why this priority**: Không có quản lý sản phẩm trên CMS thì App không có dữ liệu để hiển thị. Đây là nền tảng cho toàn bộ luồng thương mại điện tử.

**Independent Test**: Có thể test đầy đủ bằng cách: đăng nhập CMS → tạo sản phẩm → cấu hình tồn kho → gắn làng nghề → kiểm tra App hiển thị. Delivers giá trị: nhân viên tự chủ quản lý sản phẩm mà không cần lập trình viên.

**Acceptance Scenarios**:

1. **Given** nhân viên đăng nhập CMS, **When** vào mục "Quản lý sản phẩm", **Then** hệ thống hiển thị danh sách sản phẩm với bộ lọc (theo danh mục, làng nghề, trạng thái)
2. **Given** nhân viên tạo sản phẩm mới, **When** nhập thông tin (tên, mô tả, giá, danh mục, làng nghề, hình ảnh, thông số, tồn kho), **Then** sản phẩm được lưu và hiển thị trên App
3. **Given** sản phẩm đã tồn tại, **When** nhân viên chỉnh sửa thông tin hoặc tồn kho, **Then** thay đổi được cập nhật realtime trên App
4. **Given** sản phẩm hết hàng, **When** tồn kho = 0, **Then** sản phẩm hiển thị "Hết hàng" trên App và không cho phép thêm vào giỏ
5. **Given** nhân viên xóa sản phẩm, **When** xóa, **Then** sản phẩm không còn hiển thị trên App, đơn hàng đã đặt không bị ảnh hưởng

---

### User Story 6 - Admin/CMS quản lý đơn hàng và vận chuyển (Priority: P1)

Admin hoặc nhân viên CMS xem danh sách đơn hàng, lọc theo trạng thái/ngày, xử lý đơn (xác nhận, chuẩn bị hàng, bàn giao shipper, hoàn tất), và in phiếu giao hàng. Hệ thống hỗ trợ quản lý trạng thái vận chuyển và thông báo khách hàng.

**Why this priority**: Không có quản lý đơn hàng trên CMS thì không thể xử lý và hoàn tất đơn. Đây là điều kiện tiên quyết để hoạt động thương mại điện tử.

**Independent Test**: Có thể test đầy đủ bằng cách: xem danh sách đơn → lọc → xác nhận đơn → cập nhật trạng thái → kiểm tra khách nhận thông báo. Delivers giá trị: nhân viên xử lý đơn hàng hiệu quả.

**Acceptance Scenarios**:

1. **Given** nhân viên vào "Quản lý đơn hàng", **When** xem danh sách, **Then** hệ thống hiển thị đơn với thông tin (mã đơn, khách, sản phẩm, tổng tiền, trạng thái, ngày)
2. **Given** nhân viên lọc đơn, **When** chọn theo trạng thái/ngày/khách, **Then** danh sách cập nhật theo bộ lọc
3. **Given** đơn mới tạo, **When** nhân viên nhấn "Xác nhận", **Then** hệ thống cập nhật trạng thái thành "Đang chuẩn bị" và gửi thông báo cho khách
4. **Given** đơn đang chuẩn bị, **When** nhân viên nhấn "Bàn giao shipper", **Then** hệ thống cập nhật trạng thái, cho phép nhập thông tin shipper, và gửi thông báo cho khách
5. **Given** đơn đã giao, **When** nhân viên nhấn "Hoàn tất", **Then** hệ thống cập nhật trạng thái, gửi thông báo cảm ơn, và cho phép khách đánh giá

---

### User Story 7 - Admin/CMS quản lý danh mục và làng nghề (Priority: P2)

Admin hoặc nhân viên CMS quản lý danh mục sản phẩm (gốm, nồm, mây tre đan, lụa, đồng, giấy dó...) và quản lý thông tin làng nghề (tên, địa chỉ, mô tả, hình ảnh, di tích liên quan). Hệ thống cho phép thêm/sửa/xóa và gắn sản phẩm với danh mục/làng nghề.

**Why this priority**: Quản lý danh mục và làng nghề cần thiết để tổ chức sản phẩm. Nhưng không phải điều kiện để luồng mua hàng cơ bản hoạt động (có thể tạo sẵn danh mục ban đầu).

**Independent Test**: Có thể test đầy đủ bằng cách: tạo danh mục → tạo làng nghề → gắn sản phẩm → kiểm tra hiển thị. Delivers giá trị: nhân viên tổ chức sản phẩm một cách có cấu trúc.

**Acceptance Scenarios**:

1. **Given** nhân viên vào "Quản lý danh mục", **When** tạo danh mục mới, **Then** danh mục được lưu và có thể gán cho sản phẩm
2. **Given** nhân viên vào "Quản lý làng nghề", **When** tạo làng nghề mới, **Then** làng nghề được lưu với thông tin (tên, địa chỉ, mô tả, hình ảnh, di tích liên quan)
3. **Given** sản phẩm đã tồn tại, **When** nhân viên gắn danh mục và làng nghề, **Then** sản phẩm hiển thị trong danh mục và làng nghề tương ứng trên App
4. **Given** khách đang xem di tích, **When** di tích có làng nghề liên quan, **Then** hệ thống hiển thị section "Sản phẩm từ làng nghề gần đây" để khách có thể mua

---

### User Story 8 - Admin/CMS quản lý khuyến mãi và voucher (Priority: P2)

Admin hoặc nhân viên CMS tạo và quản lý mã khuyến mãi/voucher (giảm giá theo % hoặc số tiền cố định, miễn phí vận chuyển, cấu hình giới hạn lần dùng, thời hạn, áp dụng cho sản phẩm/danh mục cụ thể).

**Why this priority**: Khuyến mãi là công cụ marketing quan trọng. Nhưng không phải điều kiện để luồng mua hàng cơ bản hoạt động.

**Independent Test**: Có thể test đầy đủ bằng cách: tạo mã khuyến mãi → khách áp dụng khi mua → xác nhận giá được giảm. Delivers giá trị: hỗ trợ chiến dịch marketing.

**Acceptance Scenarios**:

1. **Given** nhân viên vào "Quản lý khuyến mãi", **When** tạo mã mới (mã, loại giảm, giá trị, giới hạn, thời hạn, áp dụng cho), **Then** mã được lưu và khả dụng cho khách sử dụng
2. **Given** khách đang thanh toán, **When** khách nhập mã khuyến mãi hợp lệ, **Then** hệ thống hiển thị số tiền giảm và cập nhật tổng tiền
3. **Given** khách nhập mã khuyến mãi, **When** mã không tồn tại hoặc đã hết hạn hoặc hết lượt, **Then** hệ thống hiển thị thông báo lỗi cụ thể
4. **Given** mã khuyến mãi có giới hạn sản phẩm, **When** khách áp dụng cho sản phẩm không nằm trong danh sách, **Then** hệ thống từ chối và thông báo rõ ràng

---

### User Story 9 - Admin/CMS xem báo cáo doanh thu và thống kê (Priority: P3)

Admin hoặc nhân viên CMS xem dashboard tổng quan về doanh thu bán hàng, số đơn hàng, sản phẩm bán chạy, tồn kho, và thống kê theo thời gian.

**Why this priority**: Dashboard hỗ trợ ra quyết định kinh doanh. Nhưng không ảnh hưởng đến luồng vận hành cốt lõi.

**Independent Test**: Có thể test đầy đủ bằng cách: tạo một số đơn → vào dashboard → xác nhận số liệu khớp. Delivers giá trị: có cái nhìn tổng quan về hoạt động kinh doanh.

**Acceptance Scenarios**:

1. **Given** nhân viên vào "Báo cáo doanh thu", **When** chọn khoảng thời gian, **Then** hệ thống hiển thị: tổng doanh thu, số đơn, trung bình giá trị đơn, sản phẩm bán chạy
2. **Given** nhân viên xem "Thống kê tồn kho", **When** xem danh sách, **Then** hệ thống hiển thị sản phẩm với tồn kho hiện tại, đã bán, còn lại
3. **Given** có dữ liệu bán hàng theo nhiều ngày, **When** nhân viên xem biểu đồ xu hướng, **Then** dữ liệu được hiển thị theo ngày/tuần/tháng

---

### Edge Cases

- Nếu khách đặt sản phẩm nhưng thanh toán thất bại? Hệ thống giữ đơn trong trạng thái "chờ thanh toán" trong 30 phút, sau đó tự động hủy và trả lại tồn kho.
- Nếu khách hủy đơn sau khi đã đóng gói? Nhân viên CMS có quyền từ chối hủy và yêu cầu khách nhận hàng hoặc trả phí hủy.
- Nếu tồn kho không đủ khi có nhiều đơn cùng lúc? Hệ thống có cơ chế lock kho (đặt hàng thì giữ số lượng trong 30 phút) để tránh bán vượt.
- Nếu shipper giao hàng nhưng khách không nhận? Hệ thống có quy trình xử lý trả hàng và hoàn tiền/cấn trừ phí vận chuyển.
- Nếu sản phẩm bị lỗi khi khách nhận? Hệ thống có quy trình đổi trả và hoàn tiền.
- Nếu khách thêm quá nhiều sản phẩm vào giỏ? Hệ thống có giới hạn số lượng mỗi sản phẩm và tổng số loại sản phẩm trong giỏ.
- Nếu admin xóa sản phẩm đang có đơn hàng chưa hoàn tất? Hệ thống cảnh báo và từ chối xóa hoặc cho phép ẩn (hide) thay vì xóa.

## Requirements *(mandatory)*

### Functional Requirements

**Xem và tìm kiếm sản phẩm (App)**

- **FR-001**: Hệ thống PHẢI hiển thị danh sách sản phẩm với hình ảnh, tên, giá, tên làng nghề
- **FR-002**: Hệ thống PHẢI cho phép tìm kiếm sản phẩm theo tên
- **FR-003**: Hệ thống PHẢI cho phép lọc theo loại sản phẩm (danh mục)
- **FR-004**: Hệ thống PHẢI cho phép lọc theo làng nghề
- **FR-005**: Hệ thống PHẢI cho phép lọc theo mức giá (khoảng giá)
- **FR-006**: Hệ thống PHẢI hiển thị trang chi tiết sản phẩm với hình ảnh đầy đủ, mô tả, giá, thông tin làng nghề, đánh giá
- **FR-007**: Hệ thống NÊN hiển thị sản phẩm gợi ý liên quan khi xem chi tiết

**Giỏ hàng và thanh toán (App)**

- **FR-008**: Hệ thống PHẢI cho phép thêm sản phẩm vào giỏ hàng với số lượng
- **FR-009**: Hệ thống PHẢI hiển thị giỏ hàng với danh sách sản phẩm, số lượng, giá từng sản phẩm, tổng tiền, phí vận chuyển
- **FR-010**: Hệ thống PHẢI cho phép thay đổi số lượng hoặc xóa sản phẩm trong giỏ hàng
- **FR-011**: Hệ thống PHẢI yêu cầu nhập thông tin giao hàng (họ tên, SĐT, địa chỉ) trước khi thanh toán
- **FR-012**: Hệ thống PHẢI tích hợp thanh toán online qua cổng thanh toán (VNPay, Momo)
- **FR-013**: Hệ thống PHẢI tính phí vận chuyển (miễn phí nếu đạt mức tối thiểu hoặc có mã miễn phí vận chuyển)
- **FR-014**: Hệ thống PHẢI giữ đơn trong trạng thái "chờ thanh toán" trong 30 phút, sau đó tự động hủy và trả lại tồn kho

**Theo dõi đơn hàng (App)**

- **FR-016**: Hệ thống PHẢI hiển thị danh sách đơn hàng theo trạng thái (chờ xác nhận, đang chuẩn bị, đang giao, đã giao, đã hủy)
- **FR-017**: Hệ thống PHẢI hiển thị chi tiết đơn với timeline trạng thái
- **FR-018**: Hệ thống PHẢI cho phép hủy đơn khi chưa giao (hoàn tiền, trả lại tồn kho)
- **FR-019**: Hệ thống PHẢI thông báo cho khách khi trạng thái đơn thay đổi
- **FR-020**: Hệ thống NÊN cho phép đánh giá sản phẩm sau khi nhận hàng

**Đánh giá sản phẩm (App)**

- **FR-021**: Hệ thống NÊN cho phép khách đánh giá sản phẩm (số sao) và viết nhận xét sau khi nhận hàng
- **FR-022**: Hệ thống NÊN hiển thị đánh giá trung bình và danh sách nhận xét trên trang sản phẩm
- **FR-023**: Hệ thống NÊN cho phép lọc đánh giá (theo số sao, có hình ảnh, mới nhất)
- **FR-024**: Hệ thống NÊN cho phép sửa hoặc xóa đánh giá trong vòng 7 ngày

**Quản lý sản phẩm và kho (CMS)**

- **FR-025**: Hệ thống PHẢI cho phép thêm/sửa/xóa sản phẩm với thông tin: tên, mô tả, giá, danh mục, làng nghề, hình ảnh, thông số, tồn kho
- **FR-026**: Hệ thống PHẢI cho phép quản lý tồn kho (tăng/giảm số lượng)
- **FR-027**: Hệ thống PHẢI hiển thị "Hết hàng" khi tồn kho = 0 và không cho phép thêm vào giỏ
- **FR-028**: Hệ thống PHẢI có cơ chế lock kho (giữ số lượng khi đặt hàng trong 30 phút)
- **FR-029**: Hệ thống PHẢI cho phép gắn sản phẩm với danh mục và làng nghề
- **FR-030**: Hệ thống NÊN cho phép ẩn sản phẩm thay vì xóa nếu có đơn hàng chưa hoàn tất

**Quản lý đơn hàng và vận chuyển (CMS)**

- **FR-031**: Hệ thống PHẢI hiển thị danh sách đơn hàng với bộ lọc (theo trạng thái, ngày, khách)
- **FR-032**: Hệ thống PHẢI cho phép cập nhật trạng thái đơn (xác nhận, đang chuẩn bị, bàn giao shipper, hoàn tất)
- **FR-033**: Hệ thống PHẢI cho phép nhập thông tin shipper khi bàn giao
- **FR-034**: Hệ thống PHẢI gửi thông báo cho khách khi trạng thái thay đổi
- **FR-035**: Hệ thống NÊN cho phép in phiếu giao hàng

**Quản lý danh mục và làng nghề (CMS)**

- **FR-036**: Hệ thống NÊN cho phép thêm/sửa/xóa danh mục sản phẩm
- **FR-037**: Hệ thống NÊN cho phép thêm/sửa/xóa làng nghề với thông tin: tên, địa chỉ, mô tả, hình ảnh, di tích liên quan
- **FR-038**: Hệ thống NÊN cho phép gắn sản phẩm với danh mục và làng nghề

**Quản lý khuyến mãi và voucher (CMS)**

- **FR-039**: Hệ thống NÊN cho phép tạo/sửa/xóa mã khuyến mãi (mã, loại giảm, giá trị, giới hạn, thời hạn, áp dụng cho)
- **FR-040**: Hệ thống NÊN cho phép áp dụng khuyến mãi: giảm giá theo % hoặc số tiền cố định, miễn phí vận chuyển
- **FR-041**: Hệ thống NÊN cho phép cấu hình giới hạn (số lần dùng, áp dụng cho sản phẩm/danh mục cụ thể)

**Báo cáo doanh thu và thống kê (CMS)**

- **FR-042**: Hệ thống NÊN hiển thị dashboard tổng quan: tổng doanh thu, số đơn, trung bình giá trị đơn, sản phẩm bán chạy
- **FR-043**: Hệ thống NÊN hiển thị thống kê tồn kho (tồn hiện tại, đã bán, còn lại)
- **FR-044**: Hệ thống NÊN hiển thị biểu đồ xu hướng theo ngày/tuần/tháng

### Key Entities

- **Sản phẩm (Product)**: Sản phẩm thủ công mỹ nghệ từ làng nghề, bao gồm tên, mô tả, giá, danh mục, làng nghề sở hữu (vendor), hình ảnh, thông số kỹ thuật, tồn kho (do làng nghề quản lý), trạng thái (hoạt động/hết hàng/ẩn), ngày tạo, ngày cập nhật.
- **Danh mục sản phẩm (Category)**: Phân loại sản phẩm theo loại hình (gốm, nồm, mây tre đan, lụa, đồng, giấy dó...), bao gồm tên, mô tả, thứ tự hiển thị, trạng thái.
- **Làng nghề (Craft Village/Vendor)**: Đối tác làng nghề tự quản lý sản phẩm và tồn kho, bao gồm tên, địa chỉ, mô tả, hình ảnh, di tích liên quan (nếu có), thông tin liên hệ, tài khoản đăng nhập, danh sách sản phẩm, trạng thái (hoạt động/ngừng hợp tác).
- **Giỏ hàng (Cart)**: Kho lưu trữ tạm sản phẩm khách đang chọn, bao gồm danh sách sản phẩm với số lượng, tổng tiền, phí vận chuyển, mã khuyến mãi (nếu có).
- **Đơn hàng (Order)**: Đơn hàng của khách, bao gồm thông tin khách (họ tên, SĐT, email, địa chỉ), danh sách sản phẩm (loại, số lượng, giá tại thời điểm mua, làng nghề sở hữu), tổng tiền, phí vận chuyển, mã khuyến mãi, trạng thái (chờ thanh toán, chờ xác nhận, đang chuẩn bị, đang giao, đã giao, đã hủy), thông tin shipper, ngày tạo, ngày cập nhật.
- **Trạng thái đơn hàng (Order Status)**: Trạng thái của đơn hàng bao gồm: chờ thanh toán (đã thêm giỏ nhưng chưa thanh toán), chờ xác nhận (đã thanh toán, chờ xử lý), đang chuẩn bị (đã xác nhận, đang chuẩn bị hàng), đang giao (đã bàn giao shipper), đã giao (khách đã nhận), đã hủy (đơn bị hủy).
- **Đánh giá sản phẩm (Product Review)**: Đánh giá của khách cho sản phẩm, bao gồm khách, sản phẩm, số sao, nhận xét, hình ảnh (nếu có), trạng thái (hiển thị/ẩn), ngày tạo.
- **Khuyến mãi/Voucher (Promotion)**: Mã giảm giá bao gồm mã, loại giảm (%, số tiền, miễn phí vận chuyển), giá trị, giới hạn số lần dùng, thời hạn hiệu lực, áp dụng cho (tất cả/sản phẩm cụ thể/danh mục cụ thể), trạng thái.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: Khách có thể tìm và xem sản phẩm trong vòng 30 giây
- **SC-002**: Khách có thể thêm sản phẩm vào giỏ và thanh toán trong vòng 3 phút
- **SC-003**: 90% đơn hàng được xử lý (xác nhận) trong vòng 2 giờ
- **SC-004**: Tỷ lệ hủy đơn sau khi đã xác nhận dưới 5%
- **SC-005**: 80% khách hàng đánh giá sản phẩm sau khi nhận hàng
- **SC-006**: Không bán vượt tồn kho trong mọi trường hợp (bao gồm đặt đồng thời)
- **SC-007**: Admin/CMS có thể thêm sản phẩm mới trong vòng 5 phút
- **SC-008**: 95% khách hàng tự hoàn tất mua hàng lần đầu mà không cần hỗ trợ

## Assumptions

- **Mô hình shop**: Shop làng nghề hoạt động riêng biệt, không tích hợp trực tiếp với trang detail di tích hoặc luồng đặt vé
- **Phân quyền**: Admin chung quản lý tất cả (sản phẩm, đơn hàng, danh mục, làng nghề) — không cần phân quyền chi tiết theo vai trò
- **Mô hình kinh doanh**: Hợp tác với làng nghề — các làng nghề tự quản lý sản phẩm và tồn kho của mình (đóng hàng), Heritage360 là nền tảng trung gian
- Ứng dụng hỗ trợ tiếng Việt là chính, tiếng Anh là phụ
- Phase 1: Hợp tác với làng nghề (multi-vendor cơ bản), các làng nghề tự quản lý sản phẩm/tồn kho
- Cổng thanh toán Phase 2: VNPay và Momo (tương tự feature đặt vé)
- Vận chuyển: Hợp tác với đơn vị vận chuyển thứ 3 (shipper bên ngoài)
- Đổi trả/hoàn tiền: Theo chính sách công ty (có thể cấu hình trên CMS)
- Hệ thống có sẵn cơ chế xác thực và quản lý user account
- Hệ thống có sẵn dữ liệu về các làng nghề và di sản (từ Heritage360 Phase 1)

## Clarifications

### Session 2026-05-06

- Q: Mô hình shop — Shop riêng biệt hay gắn sản phẩm trong detail bài viết di tích? → A: Shop riêng biệt hoàn toàn, không tích hợp với đặt vé
- Q: Phân quyền — Ai quản lý shop, sản phẩm, đơn hàng? → A: Admin chung quản lý tất cả, không cần phân quyền chi tiết
- Q: Mô hình kinh doanh — Hợp tác với shop bên ngoài hay tự quản lý? → A: Hợp tác với làng nghề (làng nghề tự quản lý sản phẩm và tồn kho)
