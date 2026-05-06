# Feature Specification: Cá nhân hóa tour du lịch (Tour Personalization)

**Feature Branch**: `002-tour-personalization`
**Created**: 2026-05-06
**Status**: Draft
**Input**: User description: "Viet spec cho feature ca nhan hoa tour" (Write spec for tour personalization feature)

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Người dùng tạo hồ sơ du lịch cá nhân (Travel Profile) (Priority: P1)

Người dùng mở app Hà Nội 360, truy cập tính năng Cá nhân hóa tour và hoàn thành quiz ngắn (5-7 câu hỏi) về phong cách du lịch, ngân sách, thành phần đoàn, sở thích ẩm thực, tốc độ di chuyển và sở thích đặc biệt. Thông tin được lưu trữ thành Travel Persona — nhân cách du lịch dùng xuyên suốt các cuộc trò chuyện với AI để tạo lịch trình phù hợp.

**Why this priority**: Đây là nền tảng cho toàn bộ tính năng cá nhân hóa. Không có Travel Profile thì AI không có đủ ngữ cảnh để tạo lịch trình phù hợp với từng cá nhân.

**Independent Test**: Có thể test đầy đủ bằng cách: mở app → vào tính năng Cá nhân hóa tour → làm quiz → xác nhận Travel Profile được lưu và hiển thị đúng. Delivers giá trị: người dùng có hồ sơ du lịch được hệ thống ghi nhớ để cá nhân hóa trải nghiệm.

**Acceptance Scenarios**:

1. **Given** người dùng truy cập tính năng Cá nhân hóa tour lần đầu, **When** người dùng mở quiz, **Then** hệ thống hiển thị 5-7 câu hỏi về phong cách du lịch, ngân sách, thành phần đoàn, sở thích ẩm thực, tốc độ di chuyển, sở thích đặc biệt
2. **Given** người dùng đang làm quiz, **When** người dùng trả lời từng câu hỏi, **Then** hệ thống lưu câu trả lời và chuyển đến câu tiếp theo
3. **Given** người dùng đã hoàn thành quiz, **When** người dùng nhấn "Lưu hồ sơ", **Then** hệ thống tạo Travel Profile với Travel Persona và hiển thị xác nhận
4. **Given** người dùng đã có Travel Profile, **When** người dùng quay lại tính năng, **Then** hệ thống hiển thị hồ sơ hiện tại và cho phép chỉnh sửa
5. **Given** người dùng không muốn làm quiz ngay, **When** người dùng chọn "Bỏ qua", **Then** hệ thống cho phép tiếp tục và thu thập thông tin dần qua hội thoại

---

### User Story 2 - Người dùng tạo lịch trình tour qua hội thoại với AI (Priority: P1)

Người dùng mở chatbot và mô tả nhu cầu du lịch bằng ngôn ngữ tự nhiên (ví dụ: "Tôi muốn đi Hội An 3 ngày cuối tháng 5, 2 người lớn + 1 bé 8 tuổi, thích ăn uống và chụp ảnh, ngân sách vừa phải"). AI khai thác thông tin cần thiết (điểm đến, ngày đi về, số lượng người, ngân sách, phương tiện, chỗ ở, ưu tiên đặc biệt), sau đó tạo lịch trình chi tiết theo từng ngày (Sáng/Chiều/Tối) với danh sách điểm tham quan, nhà hàng, ước tính chi phí và bản đồ hiển thị hành trình.

**Why this priority**: Đây là luồng nghiệp vụ cốt lõi nhất — không có chức năng tạo lịch trình thì toàn bộ tính năng cá nhân hóa tour không có giá trị. Mọi feature khác đều phụ thuộc vào luồng này.

**Independent Test**: Có thể test đầy đủ bằng cách: mở chatbot → mô tả nhu cầu du lịch → trả lời câu hỏi từ AI → nhận lịch trình chi tiết. Delivers giá trị ngay: người dùng có lịch trình du lịch cá nhân hóa trong vài phút thay vì hàng giờ nghiên cứu.

**Acceptance Scenarios**:

1. **Given** người dùng mở chatbot, **When** người dùng mô tả nhu cầu du lịch, **Then** AI xác nhận đã hiểu và bắt đầu khai thác thông tin còn thiếu
2. **Given** AI cần thêm thông tin, **When** AI đặt câu hỏi, **Then** câu hỏi không quá 2 câu cùng lúc và có gợi ý nhanh (quick replies)
3. **Given** người dùng đã cung cấp đủ thông tin bắt buộc (điểm đến, ngày đi về, số lượng người), **When** AI tạo lịch trình, **Then** hệ thống hiển thị lịch trình trong vòng 10 giây với ít nhất 3 slot mỗi ngày (Sáng/Chiều/Tối)
4. **Given** lịch trình được tạo, **When** người dùng xem kết quả, **Then** mỗi slot có: tên địa điểm, ảnh, thời gian tham quan ước tính, ghi chú ngắn, chi phí ước tính
5. **Given** người dùng có Travel Profile, **When** AI tạo lịch trình, **Then** lịch trình được cá nhân hóa theo Travel Persona (ví dụ: khách thích ẩm thực → nhiều gợi ý nhà hàng độc đáo)

---

### User Story 3 - Người dùng tinh chỉnh lịch trình qua hội thoại (Priority: P1)

Người dùng xem lịch trình đã được tạo và yêu cầu điều chỉnh bằng ngôn ngữ tự nhiên mà không cần làm lại từ đầu. Các yêu cầu điều chỉnh bao gồm: thêm điểm đến, xóa điểm đến, hoán đổi thứ tự, thay thế gợi ý, điều chỉnh thời gian, thay đổi ngân sách, thêm yêu cầu đặc biệt. AI xử lý yêu cầu và cập nhật lịch trình trong vòng 5 giây, giữ nguyên các phần không được yêu cầu thay đổi.

**Why this priority**: Tinh chỉnh là yếu tố quan trọng để đạt được lịch trình hoàn hảo theo ý người dùng. Không có tính năng này thì người dùng không thể điều chỉnh khi kết quả ban đầu chưa hoàn toàn ưng ý.

**Independent Test**: Có thể test đầy đủ bằng cách: tạo lịch trình → gửi yêu cầu điều chỉnh → xác nhận lịch trình được cập nhật đúng. Delivers giá trị: người dùng tinh chỉnh lịch trình một cách tự nhiên mà không cần thao tác phức tạp.

**Acceptance Scenarios**:

1. **Given** người dùng đã có lịch trình, **When** người dùng yêu cầu "Thêm làng gốm Thanh Hà vào ngày 2 buổi sáng", **Then** AI thêm điểm đến vào slot được yêu cầu và cập nhật lịch trình
2. **Given** người dùng đã có lịch trình, **When** người dùng yêu cầu "Bỏ tham quan bảo tàng ra, bé không thích", **Then** AI xóa điểm đến và điều chỉnh lịch trình cho hợp lý
3. **Given** người dùng đã có lịch trình, **When** người dùng yêu cầu "Đổi ngày 1 và ngày 2 cho tôi", **Then** AI hoán đổi thứ tự các ngày và cập nhật lịch trình
4. **Given** người dùng đã có lịch trình, **When** người dùng yêu cầu "Có nhà hàng nào khác không, tôi không thích hải sản", **Then** AI thay thế gợi ý nhà hàng bằng lựa chọn phù hợp khác
5. **Given** người dùng yêu cầu điều chỉnh, **When** AI xử lý, **Then** các phần không được yêu cầu thay đổi được giữ nguyên

---

### User Story 4 - Người dùng nhận tư vấn thông minh theo ngữ cảnh (Priority: P1)

Trong quá trình tạo lịch trình, AI chủ động cảnh báo và tư vấn dựa trên thông tin thực tế như ngày lễ/Tết, thời tiết, độ tuổi trẻ em, giờ mở cửa, thời gian di chuyển, yêu cầu đặt trước. Các cảnh báo/gợi ý này giúp người dùng tránh được các vấn đề thường gặp khi đi du lịch.

**Why this priority**: Tư vấn thông minh là yếu tố tạo nên sự khác biệt so với các công cụ lập kế hoạch du lịch thông thường. Nó giúp người dùng tránh được các rủi ro và có trải nghiệm tốt hơn.

**Independent Test**: Có thể test đầy đủ bằng cách: tạo lịch trình vào ngày lễ/điểm đến có vấn đề → xác nhận AI cảnh báo phù hợp. Delivers giá trị: người dùng được cảnh báo về các vấn đề tiềm ẩn trước khi đi.

**Acceptance Scenarios**:

1. **Given** người dùng tạo lịch trình vào ngày lễ/Tết, **When** AI tạo lịch trình, **Then** AI cảnh báo "Ngày này điểm này rất đông, gợi ý đến sớm 7h sáng"
2. **Given** người dùng tạo lịch trình vào mùa mưa, **When** AI tạo lịch trình, **Then** AI gợi ý "Tháng này hay mưa, nên chuẩn bị áo mưa"
3. **Given** người dùng đi với trẻ nhỏ, **When** AI gợi ý điểm đến, **Then** AI lọc ra các điểm không phù hợp (có cầu thang cao, nguy hiểm) và cảnh báo nếu cần
4. **Given** người dùng chọn bảo tàng, **When** ngày tham quan là thứ 2, **Then** AI cảnh báo "Bảo tàng này đóng cửa thứ 2, lịch trình ngày 1 của bạn cần điều chỉnh"
5. **Given** người dùng di chuyển giữa hai điểm, **When** khoảng cách xa, **Then** AI cảnh báo "Đoạn đường này mất 45 phút, cần khởi hành trước 9h"

---

### User Story 5 - Người dùng chia sẻ và lập kế hoạch nhóm (Priority: P2)

Người dùng chia sẻ lịch trình đã tạo qua link hoặc mã QR để thành viên khác trong nhóm có thể xem, thêm gợi ý hoặc bình chọn. AI tổng hợp và cân bằng sở thích của tất cả thành viên, thông báo realtime khi lịch trình được cập nhật. Phân quyền cho phép Admin chỉnh sửa và Member xem và đề xuất.

**Why this priority**: Lập kế hoạch nhóm là nhu cầu thực tế khi đi du lịch cùng bạn bè/gia đình. Nhưng không phải điều kiện để luồng cá nhân hóa cơ bản hoạt động.

**Independent Test**: Có thể test đầy đủ bằng cách: tạo lịch trình → chia sẻ link → thành viên khác tham gia → thêm gợi ý → xác nhận AI tổng hợp. Delivers giá trị: nhóm có thể cùng lập kế hoạch một cách dễ dàng.

**Acceptance Scenarios**:

1. **Given** người dùng đã có lịch trình, **When** người dùng chọn "Chia sẻ nhóm", **Then** hệ thống tạo link hoặc mã QR để chia sẻ
2. **Given** thành viên khác mở link chia sẻ, **When** thành viên tham gia, **Then** thành viên có thể xem lịch trình và thêm gợi ý/bình chọn
3. **Given** nhiều thành viên đã thêm gợi ý, **When** AI tổng hợp, **Then** AI cân bằng sở thích của tất cả thành viên và cập nhật lịch trình
4. **Given** lịch trình được cập nhật, **When** thay đổi xảy ra, **Then** tất cả thành viên nhận thông báo realtime
5. **Given** thành viên tham gia, **When** vai trò được gán, **Then** Admin có thể chỉnh sửa, Member có thể xem và đề xuất

---

### User Story 6 - Người dùng lấy cảm hứng từ link để tạo lịch trình (Priority: P3)

Người dùng chia sẻ link YouTube, TikTok, bài blog, hay ảnh và AI tự động trích xuất địa điểm để tạo lịch trình. Hệ thống xác nhận với người dùng danh sách địa điểm, sắp xếp theo trình tự địa lý hợp lý và tạo lịch trình đầy đủ kèm thông tin bổ sung.

**Why this priority**: Đây là tính năng nâng cao giúp người dùng dễ dàng tạo lịch trình từ nội dung họ tìm thấy trên mạng. Nhưng không phải điều kiện để luồng cá nhân hóa cơ bản hoạt động.

**Independent Test**: Có thể test đầy đủ bằng cách: chia sẻ link YouTube → xác nhận AI trích xuất địa điểm → tạo lịch trình. Delivers giá trị: người dùng chuyển đổi nội dung truyền thông thành lịch trình du lịch thực tế.

**Acceptance Scenarios**:

1. **Given** người dùng chia sẻ link YouTube/TikTok/blog, **When** AI xử lý, **Then** AI trích xuất danh sách địa điểm được đề cập trong nội dung
2. **Given** AI đã trích xuất địa điểm, **When** người dùng xác nhận, **Then** AI sắp xếp theo trình tự địa lý hợp lý và tạo lịch trình
3. **Given** người dùng chia sẻ ảnh, **When** AI xử lý, **Then** AI nhận diện địa điểm trong ảnh và gợi ý tạo lịch trình
4. **Given** AI tạo lịch trình từ link, **When** lịch trình được tạo, **Then** hệ thống hiển thị nguồn gốc (link gốc) để người dùng có thể tham khảo

---

### Edge Cases

- Nếu người dùng không cung cấp đủ thông tin bắt buộc (điểm đến, ngày đi về, số lượng người) thì sao? Hệ thống phải chủ động khai thác thông tin còn thiếu trước khi tạo lịch trình.
- Nếu AI không hiểu yêu cầu của người dùng? Hệ thống phải hỏi làm rõ thay vì đoán sai (graceful fallback).
- Nếu người dùng yêu cầu điều chỉnh mâu thuẫn (thêm điểm nhưng rút ngắn thời gian)? Hệ thống phải cảnh báo về sự mâu thuẫn và gợi ý giải pháp.
- Nếu hai người cùng chỉnh sửa lịch trình nhóm trong chế độ cộng tác? Hệ thống phải có cơ chế xử lý conflict hoặc lock khi chỉnh sửa.
- Nếu dữ liệu địa điểm không đầy đủ hoặc lỗi thời? Hệ thống phải cảnh báo người dùng và gợi ý xác nhận thông tin.
- Nếu AI tạo lịch trình không thực tế (khoảng cách quá xa, không đủ thời gian)? Hệ thống phải tích hợp dữ liệu địa lý để tính thời gian di chuyển và cảnh báo.

## Requirements *(mandatory)*

### Functional Requirements

**Hồ sơ du lịch cá nhân (Travel Profile)**

- **FR-001**: Hệ thống PHẢI cho phép người dùng hoàn thành quiz ngắn (5-7 câu hỏi) về phong cách du lịch, ngân sách, thành phần đoàn, sở thích ẩm thực, tốc độ di chuyển, sở thích đặc biệt
- **FR-002**: Hệ thống PHẢI lưu trữ thông tin quiz thành Travel Profile với Travel Persona — nhân cách du lịch dùng xuyên suốt các cuộc trò chuyện
- **FR-003**: Hệ thống PHẢI cho phép người dùng xem và chỉnh sửa Travel Profile đã tạo
- **FR-004**: Hệ thống NÊN cho phép người dùng bỏ qua quiz và thu thập thông tin dần qua hội thoại

**Hội thoại với AI Chatbot**

- **FR-005**: Hệ thống PHẢI cho phép người dùng mô tả nhu cầu du lịch bằng ngôn ngữ tự nhiên
- **FR-006**: Hệ thống PHẢI khai thác thông tin bắt buộc: điểm đến, ngày đi về, số lượng người
- **FR-007**: Hệ thống NÊN khai thác thông tin tùy chọn để cải thiện chất lượng: ngân sách, phương tiện di chuyển, chỗ ở ưa thích, ưu tiên đặc biệt
- **FR-008**: Hệ thống PHẢI không hỏi quá 2 câu cùng lúc
- **FR-009**: Hệ thống PHẢI cung cấp gợi ý nhanh (quick replies) để giảm gõ phím
- **FR-010**: Hệ thống PHẢI hiển thị tiến độ rõ ràng (ví dụ: "Đang tìm kiếm nhà hàng phù hợp...")
- **FR-011**: Hệ thống PHẢI xác nhận trước khi thực hiện các thay đổi lớn
- **FR-012**: Hệ thống PHẢI hỏi làm rõ khi không hiểu yêu cầu (graceful fallback)

**Tạo lịch trình**

- **FR-013**: Hệ thống PHẢI tạo lịch trình trong vòng 10 giây sau khi có đủ thông tin bắt buộc
- **FR-014**: Lịch trình PHẢI có ít nhất 3 slot mỗi ngày (Sáng/Chiều/Tối)
- **FR-015**: Mỗi slot PHẢI có: tên địa điểm, ảnh, thời gian tham quan ước tính, ghi chú ngắn, chi phí ước tính
- **FR-016**: Hệ thống PHẢI cá nhân hóa lịch trình theo Travel Persona nếu người dùng đã có Travel Profile
- **FR-017**: Hệ thống PHẢI hiển thị bản đồ với toàn bộ hành trình
- **FR-018**: Hệ thống PHẢI lọc ra các điểm không phù hợp với thành phần đoàn (ví dụ: có trẻ nhỏ → không gợi ý bar, club)

**Tinh chỉnh lịch trình**

- **FR-019**: Hệ thống PHẢI cho phép người dùng yêu cầu thêm/xóa/hoán đổi điểm đến qua chat
- **FR-020**: Hệ thống PHẢI cho phép người dùng yêu cầu thay thế gợi ý (nhà hàng, điểm đến khác)
- **FR-021**: Hệ thống PHẢI cho phép người dùng yêu cầu điều chỉnh thời gian (rút ngắn/dài thêm)
- **FR-022**: Hệ thống PHẢI cho phép người dùng yêu cầu thay đổi ngân sách
- **FR-023**: Hệ thống PHẢI cho phép người dùng thêm yêu cầu đặc biệt (lớp học nấu ăn, thuê xe, v.v.)
- **FR-024**: Hệ thống PHẢI cập nhật lịch trình sau điều chỉnh trong vòng 5 giây
- **FR-025**: Hệ thống PHẢI giữ nguyên các phần không được yêu cầu thay đổi

**Tư vấn thông minh**

- **FR-026**: Hệ thống PHẢI cảnh báo khi ngày tham quan là ngày lễ/Tết và điểm đến có khả năng đông
- **FR-027**: Hệ thống PHẢI gợi ý dựa trên thời tiết (mùa mưa, nắng nóng, v.v.)
- **FR-028**: Hệ thống PHẢI lọc và cảnh báo khi điểm đến không phù hợp với độ tuổi trẻ em
- **FR-029**: Hệ thống PHẢI cảnh báo khi điểm tham quan đóng cửa vào ngày được chọn
- **FR-030**: Hệ thống PHẢI tính toán và cảnh báo về thời gian di chuyển giữa các điểm
- **FR-031**: Hệ thống PHẢI gợi ý thứ tự địa lý hợp lý để tiết kiệm di chuyển
- **FR-032**: Hệ thống PHẢI cảnh báo khi cần đặt trước (nhà hàng, vé vào cửa, v.v.)

**Lập kế hoạch nhóm**

- **FR-033**: Hệ thống NÊN cho phép chia sẻ lịch trình qua link hoặc mã QR
- **FR-034**: Hệ thống NÊN cho phép thành viên nhóm xem lịch trình, thêm gợi ý, bình chọn
- **FR-035**: Hệ thống NÊN cho phép AI tổng hợp và cân bằng sở thích của tất cả thành viên
- **FR-036**: Hệ thống NÊN thông báo realtime khi lịch trình được cập nhật
- **FR-037**: Hệ thống NÊN phân quyền: Admin (chỉnh sửa) / Member (xem và đề xuất)

**Lấy cảm hứng từ link**

- **FR-038**: Hệ thống NÊN cho phép chia sẻ link YouTube/TikTok/blog/ảnh
- **FR-039**: Hệ thống NÊN trích xuất danh sách địa điểm từ nội dung được chia sẻ
- **FR-040**: Hệ thống NÊN xác nhận với người dùng danh sách địa điểm được trích xuất
- **FR-041**: Hệ thống NÊN sắp xếp địa điểm theo trình tự địa lý hợp lý
- **FR-042**: Hệ thống NÊN hiển thị nguồn gốc (link gốc) khi tạo lịch trình từ link

**Lưu trữ và quản lý**

- **FR-043**: Hệ thống NÊN lưu lịch sử các lịch trình đã tạo vào tài khoản người dùng
- **FR-044**: Hệ thống NÊN cho phép người dùng xem lại, chỉnh sửa, hoặc xóa lịch trình đã lưu
- **FR-045**: Hệ thống NÊN cho phép người dùng chuyển trạng thái lịch trình (draft, confirmed, ongoing, completed)

### Key Entities

- **Travel Profile (Hồ sơ du lịch)**: Hồ sơ cá nhân của người dùng bao gồm phong cách du lịch (phiêu lưu/thư giãn/văn hóa/ẩm thực/mua sắm/nhiếp ảnh), ngân sách (tiết kiệm/trung bình/cao cấp), thành phần đoàn (một mình/cặp đôi/gia đình/bạn bè/doanh nhân), hạn chế ẩm thực (chay/halal/không hải sản...), nhu cầu di chuyển (xe lăn/người cao tuổi/trẻ nhỏ), tốc độ (dày đặc/vừa phải/ít điểm chất lượng), sở thích đặc biệt (lịch sử/thiên nhiên/nghệ thuật/đời về đêm...).
- **Travel Persona (Nhân cách du lịch)**: Bản tóm tắt du lịch của người dùng được trích xuất từ Travel Profile, dùng làm ngữ cảnh cho AI để cá nhân hóa các cuộc hội thoại và gợi ý.
- **Tour Itinerary (Lịch trình tour)**: Lịch trình du lịch được tạo bởi AI cho một người dùng, bao gồm tiêu đề, điểm đến, ngày bắt đầu/kết thúc, tổng số ngày, ước tính ngân sách, danh sách các ngày (mỗi ngày có theme tùy chọn và các slot theo thời điểm — sáng/chiều/tối), danh sách người được chia sẻ, trạng thái (draft, confirmed, ongoing, completed).
- **Itinerary Slot (Slot lịch trình)**: Một hoạt động trong lịch trình thuộc về một ngày cụ thể, bao gồm thời điểm (morning/afternoon/evening), thời gian cụ thể, loại (điểm tham quan/nhà hàng/phương tiện/chỗ ở/hoạt động), thông tin địa điểm (ID, tên, ảnh), thời lượng (phút), chi phí ước tính, ghi chú, yêu cầu đặt trước.
- **Itinerary Share (Chia sẻ lịch trình)**: Bản chia sẻ lịch trình cho lập kế hoạch nhóm, bao gồm mã chia sẻ duy nhất, URL, quyền truy cập (view/edit), danh sách thành viên, trạng thái (active/expired).
- **Share Member (Thành viên chia sẻ)**: Người tham gia lập kế hoạch nhóm, bao gồm ID người dùng, vai trò (admin/member), trạng thái mời, thời gian tham gia.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: Người dùng có thể hoàn tất việc tạo lịch trình (từ mô tả nhu cầu đến nhận kết quả) trong vòng 2 phút
- **SC-002**: Hệ thống tạo lịch trình trong vòng 10 giây sau khi có đủ thông tin bắt buộc
- **SC-003**: Hệ thống cập nhật lịch trình sau điều chỉnh trong vòng 5 giây
- **SC-004**: 70% người dùng hoàn thành hội thoại và nhận được lịch trình (tỷ lệ hoàn thành session)
- **SC-005**: Số lần chỉnh sửa trung bình sau khi tạo lịch trình là dưới 3 lần
- **SC-006**: 20% lịch trình được chia sẻ với người khác (tỷ lệ chia sẻ)
- **SC-007**: 90% cảnh báo thông minh (ngày lễ, giờ đóng cửa, thời tiết) được hiển thị chính xác
- **SC-008**: Điểm hài lòng người dùng (CSAT) đạt trên 4.2/5.0

## Assumptions

- Người dùng có thiết bị di động với kết nối internet
- **Phạm vi địa điểm**: Feature chỉ áp dụng cho các điểm di sản (heritage sites) trong hệ thống Heritage360 — không bao gồm toàn bộ các điểm đến du lịch Việt Nam
- **Dữ liệu địa điểm**: Hệ thống tự xây dựng database hoàn toàn về các địa điểm di sản — không sử dụng Google Places hay API bên thứ ba
- **Mô hình AI**: Sử dụng API do bên khách hàng cung cấp — họ đã train model riêng dựa trên Gemini
- Ứng dụng hỗ trợ tiếng Việt là chính, tiếng Anh là phụ
- Quiz Travel Profile có thể bỏ qua — người dùng có thể thu thập thông tin dần qua hội thoại
- Lịch trình được lưu vào tài khoản người dùng để xem lại sau
- Tính năng "Lấy cảm hứng từ link" (YouTube, TikTok, blog) là Nice-to-have, không phải MVP
- Tích hợp đặt chỗ trực tiếp (khách sạn, vé vào cửa) là Nice-to-have, không phải MVP
- Lập kế hoạch nhóm với voting là Nice-to-have, không phải MVP
- Hệ thống có sẵn cơ chế xác thực và quản lý user account
- Hệ thống có sẵn dữ liệu cơ bản về các địa điểm du lịch (tên, mô tả, ảnh, giờ mở cửa, vị trí) trong database Heritage360

## Clarifications

### Session 2026-05-06

- Q: Phạm vi địa điểm — toàn quốc hay chỉ heritage sites? → A: Chỉ các điểm di sản trong hệ thống Heritage360
- Q: Nguồn dữ liệu địa điểm — tự xây, Google Places, hay có sẵn? → A: Tự xây dựng database hoàn toàn
- Q: Mô hình AI — dùng API bên thứ ba hay tự xây? → A: Dùng API từ khách hàng cung cấp (họ train với Gemini)
