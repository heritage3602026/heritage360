# Feature Specification: CMS Role-Based Access Control (Module Phân Quyền)

**Feature Branch**: `007-cms-phan-quyen`  
**Created**: 2026-05-13  
**Status**: Draft  
**Source**: BRD – Module phân quyền CMS v1.0 (ngày soạn 12/05/2026)

---

## User Scenarios & Testing *(mandatory)*

### User Story 1 - SUPER_ADMIN Quản Trị Tài Khoản và Phân Vai Trò (Priority: P1)

Với tư cách là SUPER_ADMIN, tôi cần tạo mới tài khoản, khóa tài khoản và gán vai trò cho người dùng để họ chỉ truy cập được đúng phần hệ thống thuộc trách nhiệm của mình.

**Why this priority**: Đây là điều kiện tiên quyết để toàn bộ hệ thống phân quyền hoạt động — không có SUPER_ADMIN tạo và gán role, các người dùng khác không thể sử dụng CMS đúng chức năng.

**Independent Test**: Có thể kiểm tra độc lập bằng cách tạo tài khoản mới, gán role EVENT_OWNER, đăng nhập với tài khoản đó và xác nhận chỉ thấy menu/chức năng của nhánh Sự kiện.

**Acceptance Scenarios**:

1. **Given** SUPER_ADMIN đã đăng nhập, **When** tạo tài khoản mới và gán role EVENT_OWNER, **Then** tài khoản được tạo thành công và người dùng mới chỉ thấy chức năng của nhánh Sự kiện khi đăng nhập.
2. **Given** SUPER_ADMIN đã đăng nhập, **When** khóa một tài khoản đang hoạt động, **Then** tài khoản đó không thể đăng nhập vào hệ thống.
3. **Given** SUPER_ADMIN đã đăng nhập, **When** gán role HERITAGE_OWNER cho người dùng, **Then** người dùng đó chỉ thấy chức năng của nhánh Di tích và không thể truy cập chức năng nhánh Sự kiện.
4. **Given** người dùng không có role SUPER_ADMIN, **When** cố truy cập chức năng quản trị tài khoản, **Then** hệ thống từ chối truy cập.

---

### User Story 2 - EVENT_OWNER Quản Lý Sự Kiện, Vé và Nhân Viên (Priority: P1)

Với tư cách là EVENT_OWNER, tôi cần tạo, chỉnh sửa và xóa các sự kiện, cấu hình vé và quản lý nhân viên trong phạm vi sự kiện của mình, mà không bị ảnh hưởng hay nhìn thấy dữ liệu của nhánh Di tích.

**Why this priority**: EVENT_OWNER là vai trò nghiệp vụ cốt lõi của nhánh Sự kiện — đây là actor chính quản lý toàn bộ nội dung sự kiện.

**Independent Test**: Kiểm tra bằng cách đăng nhập với account EVENT_OWNER, thực hiện CRUD sự kiện, kiểm tra menu không hiển thị mục Di tích.

**Acceptance Scenarios**:

1. **Given** EVENT_OWNER đã đăng nhập, **When** truy cập CMS, **Then** chỉ thấy menu và chức năng thuộc nhánh Sự kiện; menu Di tích không xuất hiện.
2. **Given** EVENT_OWNER đã đăng nhập, **When** tạo sự kiện mới với đầy đủ thông tin, **Then** sự kiện được lưu và xuất hiện trong danh sách sự kiện của mình.
3. **Given** EVENT_OWNER đã đăng nhập, **When** thêm nhân viên (EVENT_STAFF) vào sự kiện, **Then** nhân viên được gán vào sự kiện đó và có thể xem lịch / quét vé.
4. **Given** EVENT_OWNER đã đăng nhập, **When** cố truy cập URL hoặc chức năng thuộc nhánh Di tích, **Then** hệ thống từ chối và hiển thị thông báo không có quyền.
5. **Given** EVENT_OWNER đã đăng nhập, **When** xem danh sách sự kiện, **Then** chỉ thấy sự kiện thuộc quyền sở hữu của mình, không thấy sự kiện của EVENT_OWNER khác.

---

### User Story 3 - EVENT_STAFF Xem Lịch và Quét Vé (Priority: P2)

Với tư cách là EVENT_STAFF, tôi chỉ cần xem danh sách sự kiện được giao và thực hiện quét vé tại sự kiện đó, không cần và không được phép chỉnh sửa thông tin cấu hình sự kiện.

**Why this priority**: Đây là luồng vận hành thực tế tại sự kiện — EVENT_STAFF không quản lý nội dung nhưng là người trực tiếp thực hiện nghiệp vụ quét vé.

**Independent Test**: Đăng nhập EVENT_STAFF, kiểm tra chỉ thấy sự kiện được assign, thực hiện quét vé và xác nhận không thể chỉnh sửa cấu hình sự kiện.

**Acceptance Scenarios**:

1. **Given** EVENT_STAFF đã đăng nhập, **When** xem danh sách sự kiện, **Then** chỉ thấy các sự kiện được EVENT_OWNER gán cho mình.
2. **Given** EVENT_STAFF được gán vào sự kiện, **When** thực hiện quét vé, **Then** vé hợp lệ được xác nhận thành công.
3. **Given** EVENT_STAFF đã đăng nhập, **When** cố chỉnh sửa thông tin cấu hình sự kiện (tên, ngày, vé), **Then** hệ thống từ chối và không có nút/tùy chọn chỉnh sửa hiển thị.
4. **Given** EVENT_STAFF đã đăng nhập, **When** cố truy cập chức năng Di tích, **Then** hệ thống từ chối truy cập.

---

### User Story 4 - HERITAGE_OWNER Quản Lý Di Tích và Cụm (Priority: P2)

Với tư cách là HERITAGE_OWNER, tôi cần tạo, chỉnh sửa và xóa nội dung di tích và cụm trong phạm vi phụ trách, mà không nhìn thấy hoặc ảnh hưởng đến nhánh Sự kiện.

**Why this priority**: HERITAGE_OWNER là vai trò nghiệp vụ cốt lõi của nhánh Di tích, tương đương với EVENT_OWNER trong nhánh Sự kiện.

**Independent Test**: Đăng nhập HERITAGE_OWNER, thực hiện CRUD di tích và cụm, xác nhận không thấy menu Sự kiện.

**Acceptance Scenarios**:

1. **Given** HERITAGE_OWNER đã đăng nhập, **When** truy cập CMS, **Then** chỉ thấy menu và chức năng của nhánh Di tích; menu Sự kiện không xuất hiện.
2. **Given** HERITAGE_OWNER đã đăng nhập, **When** tạo mới di tích, **Then** di tích được lưu và hiển thị trong phạm vi phụ trách của mình.
3. **Given** HERITAGE_OWNER đã đăng nhập, **When** xem danh sách di tích, **Then** chỉ thấy di tích thuộc phạm vi của mình, không thấy di tích của HERITAGE_OWNER khác.
4. **Given** HERITAGE_OWNER đã đăng nhập, **When** cố truy cập chức năng Sự kiện, **Then** hệ thống từ chối truy cập.

---

### User Story 5 - CLUSTER_MANAGER Chỉnh Sửa Nội Dung Cụm Được Giao (Priority: P3)

Với tư cách là CLUSTER_MANAGER, tôi cần chỉnh sửa nội dung của các cụm được giao cho mình, mà không có quyền quản trị toàn bộ nhánh Di tích hay tạo/xóa di tích.

**Why this priority**: Đây là vai trò có quyền hạn hẹp nhất trong nhánh Di tích, phù hợp cho người quản lý từng cụm nội dung cụ thể.

**Independent Test**: Đăng nhập CLUSTER_MANAGER, chỉnh sửa nội dung cụm được giao, xác nhận không thể tạo/xóa di tích hay xem cụm không được giao.

**Acceptance Scenarios**:

1. **Given** CLUSTER_MANAGER đã đăng nhập, **When** xem danh sách cụm, **Then** chỉ thấy các cụm được HERITAGE_OWNER gán cho mình (HERITAGE_OWNER là người thực hiện việc gán này).
2. **Given** CLUSTER_MANAGER được gán cụm A, **When** chỉnh sửa nội dung cụm A, **Then** thay đổi được lưu thành công.
3. **Given** CLUSTER_MANAGER đã đăng nhập, **When** cố tạo di tích mới hoặc xóa di tích, **Then** chức năng đó không hiển thị hoặc hệ thống từ chối.
4. **Given** CLUSTER_MANAGER đã đăng nhập, **When** cố truy cập cụm không được gán cho mình, **Then** hệ thống từ chối truy cập.

---

### Edge Cases

- Điều gì xảy ra khi người dùng bị đổi role trong khi đang đăng nhập? → Hệ thống tái kiểm tra quyền mỗi request; quyền mới có hiệu lực ngay lập tức tại request kế tiếp mà không cần đăng nhập lại.
- Điều gì xảy ra khi SUPER_ADMIN khóa tài khoản đang đăng nhập? → Tài khoản bị đăng xuất ngay lập tức; mọi phiên hoạt động của người đó chấm dứt tức thì, không có grace period.
- Điều gì xảy ra khi EVENT_STAFF bị remove khỏi sự kiện trong khi đang quét vé? → Hệ thống từ chối tiếp tục thao tác và thông báo không còn quyền truy cập sự kiện đó.
- Điều gì xảy ra khi một người dùng cố trực tiếp truy cập URL của chức năng không thuộc quyền? → Hệ thống trả về trang thông báo không có quyền truy cập, không để lộ nội dung.
- Điều gì xảy ra khi SUPER_ADMIN cố tạo tài khoản với email đã tồn tại? → Hệ thống thông báo email đã được sử dụng và không tạo bản ghi trùng lặp.
- Điều gì xảy ra khi EVENT_STAFF hoặc CLUSTER_MANAGER đăng nhập nhưng chưa được gán vào sự kiện/cụm nào? → Hệ thống hiển thị danh sách trống kèm thông báo hướng dẫn liên hệ quản lý; không hiển thị lỗi hệ thống.

---

## Requirements *(mandatory)*

### Functional Requirements

**Nhóm: Phân tách nhánh (Domain Separation)**

- **FR-001**: Hệ thống PHẢI tách biệt hoàn toàn chức năng và dữ liệu giữa nhánh Sự kiện và nhánh Di tích ở mức truy cập nghiệp vụ — người dùng thuộc nhánh này không thể xem hoặc thao tác dữ liệu nhánh kia.
- **FR-002**: Hệ thống PHẢI ẩn toàn bộ menu, màn hình và thao tác không thuộc quyền của role hiện tại — người dùng không thấy chức năng mà mình không có quyền.
- **FR-003**: Hệ thống PHẢI từ chối yêu cầu truy cập trực tiếp (qua URL hoặc API của web CMS) vào chức năng/dữ liệu ngoài phạm vi role, ngay cả khi người dùng biết đường dẫn. Phạm vi áp dụng: chỉ web CMS — mobile app dùng API riêng, ngoài phạm vi giai đoạn 1.

**Nhóm: Quản trị tài khoản (Account Management — SUPER_ADMIN only)**

- **FR-004**: SUPER_ADMIN PHẢI có khả năng tạo mới tài khoản người dùng trong hệ thống. Khi tạo, hệ thống tự sinh mật khẩu tạm thời và gửi email thông báo đến địa chỉ email của tài khoản mới; người dùng buộc phải đổi mật khẩu khi đăng nhập lần đầu tiên.
- **FR-005**: SUPER_ADMIN PHẢI có khả năng khóa/mở khóa tài khoản người dùng.
- **FR-006**: SUPER_ADMIN PHẢI có khả năng gán một trong 5 role hệ thống cho bất kỳ tài khoản nào.
- **FR-007**: SUPER_ADMIN PHẢI có quyền xem (read-only) toàn bộ dữ liệu của cả hai nhánh Sự kiện và Di tích — bao gồm sự kiện, vé, di tích và cụm của mọi người dùng khác.
- **FR-007a**: SUPER_ADMIN KHÔNG có quyền tạo, chỉnh sửa hoặc xóa nội dung nghiệp vụ (sự kiện, vé, di tích, cụm) — trách nhiệm CRUD nội dung thuộc về EVENT_OWNER và HERITAGE_OWNER tương ứng.
- **FR-008**: Chỉ SUPER_ADMIN mới có quyền thực hiện quản trị tài khoản (tạo, khóa, gán role) — các role khác không được phép. Hệ thống cho phép nhiều SUPER_ADMIN; tài khoản SUPER_ADMIN đầu tiên được seed qua script/config khi deploy, các tài khoản SUPER_ADMIN tiếp theo do SUPER_ADMIN hiện tại tạo.
- **FR-008a**: SUPER_ADMIN KHÔNG thể tự khóa chính mình — hệ thống phải ngăn thao tác này để tránh mất khả năng quản trị.

**Nhóm: Nhánh Sự kiện**

- **FR-009**: EVENT_OWNER PHẢI có khả năng tạo, xem, chỉnh sửa và xóa sự kiện thuộc phạm vi sở hữu của mình.
- **FR-010**: EVENT_OWNER PHẢI có khả năng tạo, xem, chỉnh sửa và xóa cấu hình vé của sự kiện mình quản lý.
- **FR-011**: EVENT_OWNER PHẢI có khả năng gán và quản lý nhân viên (EVENT_STAFF) cho các sự kiện của mình.
- **FR-012**: EVENT_OWNER CHỈ thấy sự kiện thuộc phạm vi sở hữu của mình, không thấy sự kiện của EVENT_OWNER khác.
- **FR-013**: EVENT_STAFF PHẢI có khả năng xem lịch của các sự kiện được gán.
- **FR-014**: EVENT_STAFF PHẢI có khả năng thực hiện quét vé tại các sự kiện được gán; đây là quyền dành riêng cho EVENT_STAFF.
- **FR-014a**: EVENT_OWNER KHÔNG có quyền quét vé, kể cả tại sự kiện thuộc phạm vi sở hữu của mình — quyền quét vé chỉ thuộc về EVENT_STAFF.
- **FR-015**: EVENT_STAFF KHÔNG được phép tạo, chỉnh sửa hoặc xóa bất kỳ thông tin cấu hình sự kiện hoặc vé nào.

**Nhóm: Nhánh Di tích**

- **FR-016**: HERITAGE_OWNER PHẢI có khả năng tạo, xem, chỉnh sửa và xóa di tích trong phạm vi phụ trách.
- **FR-017**: HERITAGE_OWNER PHẢI có khả năng tạo, xem, chỉnh sửa và xóa cụm trong phạm vi phụ trách.
- **FR-017a**: HERITAGE_OWNER PHẢI có khả năng gán và hủy gán CLUSTER_MANAGER vào các cụm thuộc phạm vi phụ trách của mình.
- **FR-017b**: SUPER_ADMIN PHẢI có khả năng gán HERITAGE_OWNER vào heritage(s) cụ thể qua màn hình quản trị; phạm vi phụ trách của HERITAGE_OWNER được xác định bởi danh sách heritage được SUPER_ADMIN gán — không tự động nhận quyền sở hữu khi tạo mới.
- **FR-018**: HERITAGE_OWNER CHỈ thấy di tích và cụm thuộc phạm vi phụ trách của mình.
- **FR-019**: CLUSTER_MANAGER CHỈ được xem và chỉnh sửa nội dung của những cụm được gán, không có quyền tạo hoặc xóa di tích/cụm.
- **FR-020**: CLUSTER_MANAGER KHÔNG thấy và KHÔNG được phép truy cập cụm không thuộc danh sách được gán.

**Nhóm: Nguyên tắc tối thiểu quyền (Least Privilege)**

- **FR-021**: Mỗi role chỉ được cấp đúng các quyền cần thiết để thực hiện nhiệm vụ được giao, theo nguyên tắc RBAC tối thiểu quyền.
- **FR-022**: Hệ thống sử dụng 5 role cố định trong giai đoạn 1: `SUPER_ADMIN`, `EVENT_OWNER`, `EVENT_STAFF`, `HERITAGE_OWNER`, `CLUSTER_MANAGER`.

### Key Entities

- **User (Tài khoản)**: Đại diện một người dùng CMS; có thuộc tính email, trạng thái (hoạt động/khóa) và role được gán; mỗi người dùng có đúng một role trong giai đoạn 1.
- **Role (Vai trò)**: Tập hợp quyền nghiệp vụ; gồm 5 role cố định; xác định nhánh (Hệ thống / Sự kiện / Di tích) và phạm vi dữ liệu người dùng có thể thao tác.
- **Event (Sự kiện)**: Thuộc nhánh Sự kiện; được sở hữu bởi EVENT_OWNER; có danh sách vé và nhân viên được gán.
- **Ticket (Vé)**: Thuộc một sự kiện cụ thể; được quản lý bởi EVENT_OWNER và được sử dụng (quét) bởi EVENT_STAFF.
- **Heritage (Di tích)**: Thuộc nhánh Di tích; được quản lý bởi HERITAGE_OWNER; có thể chứa nhiều cụm (cluster).
- **Cluster (Cụm)**: Đơn vị nội dung nằm trong Di tích; được quản lý bởi HERITAGE_OWNER và có thể được gán cho CLUSTER_MANAGER để chỉnh sửa nội dung.
- **Assignment (Giao việc)**: Liên kết giữa người dùng và phạm vi công việc (EVENT_STAFF ↔ Event; CLUSTER_MANAGER ↔ Cluster; HERITAGE_OWNER ↔ Heritage); quyết định dữ liệu nào người dùng có thể thấy. SUPER_ADMIN thực hiện gán HERITAGE_OWNER ↔ Heritage; HERITAGE_OWNER thực hiện gán CLUSTER_MANAGER ↔ Cluster; EVENT_OWNER thực hiện gán EVENT_STAFF ↔ Event.

---

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: 100% các yêu cầu truy cập vào chức năng ngoài phạm vi role bị từ chối — không có trường hợp người dùng truy cập được dữ liệu hoặc chức năng không thuộc quyền.
- **SC-002**: Người dùng thuộc nhánh Sự kiện không thể nhìn thấy bất kỳ mục menu hoặc dữ liệu nào của nhánh Di tích và ngược lại (0% cross-branch data leakage).
- **SC-003**: SUPER_ADMIN có thể hoàn thành thao tác tạo tài khoản và gán role trong vòng 2 phút.
- **SC-004**: Sau khi role được thay đổi, quyền mới có hiệu lực ngay tại request kế tiếp — không cần đăng nhập lại. Sau khi tài khoản bị khóa, người dùng bị đăng xuất ngay lập tức (0 giây grace period). Cơ chế thực thi: JWT + Redis token blacklist; middleware kiểm tra blacklist mỗi request.
- **SC-005**: Toàn bộ 5 role hoạt động đúng theo ma trận quyền đã xác định trong BRD — được xác nhận qua kiểm thử chấp nhận với tỉ lệ vượt qua 100%.
- **SC-006**: Người dùng chỉ thấy dữ liệu "của mình" hoặc "được assign" — không có dữ liệu của người dùng cùng role khác xuất hiện trong danh sách.

---

## Clarifications

### Session 2026-05-13

- Q: "Phạm vi phụ trách" của HERITAGE_OWNER được thiết lập như thế nào? → A: SUPER_ADMIN gán HERITAGE_OWNER vào heritage(s) cụ thể qua màn hình quản trị (tương tự cơ chế EVENT_STAFF ↔ Event); không tự động nhận quyền sở hữu khi tạo mới.
- Q: Cơ chế vô hiệu hóa session khi khóa tài khoản hoặc đổi role là gì? → A: JWT + token blacklist/revocation store (Redis); middleware kiểm tra blacklist mỗi request để đảm bảo revoke tức thì.
- Q: Có thể có nhiều SUPER_ADMIN không, và tài khoản đầu tiên được tạo như thế nào? → A: Nhiều SUPER_ADMIN được phép; tài khoản đầu tiên được seed qua script/config khi deploy; các SUPER_ADMIN tiếp theo do SUPER_ADMIN hiện tại tạo.
- Q: Khi SUPER_ADMIN tạo tài khoản mới, thông tin đăng nhập ban đầu được cấp như thế nào? → A: Hệ thống tự tạo mật khẩu tạm thời và gửi qua email; người dùng buộc đổi mật khẩu khi đăng nhập lần đầu.
- Q: RBAC module này áp dụng cho web CMS, API backend, hay cả mobile app? → A: Chỉ web CMS; mobile app dùng API riêng và không thuộc phạm vi module này trong giai đoạn 1.
- Q: Hệ thống kiểm tra quyền mỗi request hay chỉ khi session refresh? Khi khóa tài khoản đang đăng nhập thì sao? → A: Quyền tái kiểm tra mỗi request; khóa tài khoản đăng xuất người dùng ngay lập tức, không có grace period.
- Q: EVENT_OWNER có quyền quét vé tại sự kiện của mình không? → A: Không. EVENT_OWNER không có quyền quét vé; chỉ EVENT_STAFF mới được thực hiện thao tác quét vé.
- Q: Ai có quyền gán/hủy gán CLUSTER_MANAGER vào cụm? → A: HERITAGE_OWNER gán/hủy gán CLUSTER_MANAGER vào cụm thuộc phạm vi phụ trách của mình; SUPER_ADMIN không can thiệp vào level này.
- Q: EVENT_STAFF / CLUSTER_MANAGER chưa được gán assignment thì hệ thống hiển thị gì? → A: Hiển thị danh sách trống kèm thông báo hướng dẫn liên hệ quản lý; không hiển thị lỗi hệ thống.
- Q: SUPER_ADMIN có quyền CRUD nội dung nghiệp vụ (sự kiện, di tích, vé, cụm) hay chỉ xem? → A: SUPER_ADMIN chỉ có quyền xem (read-only) toàn bộ dữ liệu hai nhánh; không CRUD nội dung nghiệp vụ trực tiếp. Trách nhiệm CRUD nội dung thuộc về EVENT_OWNER và HERITAGE_OWNER.

---

## Assumptions

- Hệ thống CMS đã có cơ chế xác thực người dùng (đăng nhập) — module phân quyền chỉ xử lý phần kiểm soát truy cập sau khi đăng nhập thành công.
- Hệ thống có khả năng gửi email transactional (ví dụ: SendGrid, SES) để phân phối mật khẩu tạm thời khi tạo tài khoản mới.
- Mỗi người dùng chỉ có một role trong giai đoạn 1; không hỗ trợ đa role hoặc role kết hợp.
- Cơ chế gán EVENT_STAFF vào sự kiện và CLUSTER_MANAGER vào cụm (assignment) đã có hoặc sẽ được xây dựng cùng module này — đây là điều kiện để kiểm tra quyền phạm vi được giao (FR-013, FR-019).
- Giai đoạn 1 không yêu cầu người dùng cuối tự cấu hình role động — chỉ SUPER_ADMIN quản lý role.
- Hệ thống hỗ trợ nhiều SUPER_ADMIN; tài khoản SUPER_ADMIN khởi đầu được provision qua database seed script khi deploy lần đầu.
- Thiết kế kỹ thuật chi tiết (database schema, API, thuật toán kiểm tra quyền) sẽ được đặc tả trong tài liệu kỹ thuật riêng sau khi spec này được xác nhận.
- Cơ chế revocation dùng JWT + Redis blacklist; token ID được ghi vào blacklist khi tài khoản bị khóa hoặc role thay đổi.
- Module RBAC này chỉ áp dụng cho web CMS; mobile app dùng API riêng và nằm ngoài phạm vi giai đoạn 1.
- Quyền "Quét vé" chỉ thuộc về EVENT_STAFF; EVENT_OWNER không có quyền này — đã xác nhận.
- Audit log cho các thao tác nhạy cảm (tạo tài khoản, gán role, khóa tài khoản) là yêu cầu của giai đoạn sau, không thuộc phạm vi giai đoạn 1.
