# Đặc tả tính năng: Cổng thanh toán

**Nhánh tính năng**: `009-payment-gateway`  
**Ngày tạo**: 2026-05-13  
**Trạng thái**: Bản nháp  
**Đầu vào**: Mô tả người dùng: "BRD/Payment-gatewray.md day la mo ta payment gateway khach cung cap giup toi viet specs"

## Clarifications

### Session 2026-05-13

- Q: Khi đơn hàng đã được xác nhận thanh toán thành công, callback đến sau có được thay đổi trạng thái đơn hàng không? → A: Không. Trạng thái đã thanh toán là trạng thái cuối cho luồng này; callback đến sau không được đổi đơn hàng sang thất bại hoặc đã hủy, chỉ được ghi nhận để rà soát.
- Q: Trạng thái đang chờ thanh toán được giữ tối đa bao lâu trước khi cho phép tạo lần thanh toán mới? → A: Tối đa 30 phút; sau đó người dùng được tạo lần thanh toán mới nếu đơn hàng còn hiệu lực.
- Q: Sau khi thanh toán, cơ chế chính để đưa người dùng về ứng dụng là gì? → A: Backend nhận callback, xác minh kết quả, rồi redirect người dùng về ứng dụng bằng deep link hoặc app link với trạng thái kết quả.
- Q: Một đơn hàng có được có nhiều lần thanh toán đang chờ cùng lúc không? → A: Không. Mỗi đơn hàng chỉ được có 1 lần thanh toán đang chờ tại một thời điểm.
- Q: Lịch sử thanh toán cần được lưu tối thiểu bao lâu? → A: Tối thiểu 24 tháng sau ngày thanh toán hoặc lần thanh toán cuối cùng.
- Q: Khi lưu dữ liệu callback để rà soát, hệ thống cần lưu mức dữ liệu nào? → A: Lưu trạng thái, thời điểm, mã tham chiếu, mã lỗi hoặc lý do thất bại, và payload callback đã che hoặc loại bỏ dữ liệu nhạy cảm.
- Q: Nếu backend không redirect được người dùng về ứng dụng sau xác minh, hệ thống cần xử lý thế nào? → A: Hiển thị trang kết quả trên trình duyệt với trạng thái đã xác minh hoặc đang chờ và nút mở lại ứng dụng hoặc thử lại.
- Q: Ai được truy cập lịch sử thanh toán? → A: Người dùng chỉ xem thanh toán của chính mình; admin hoặc support có quyền phù hợp mới được tra cứu để hỗ trợ hoặc đối soát.
- Q: Nếu nhà cung cấp báo thành công nhưng số tiền hoặc mã tham chiếu không khớp đơn hàng thì xử lý thế nào? → A: Không xác nhận thanh toán; ghi nhận sự kiện cần rà soát và không phát hành vé.
- Q: Khi backend chưa xác minh được với nhà cung cấp thì hệ thống xử lý trạng thái thanh toán thế nào? → A: Giữ trạng thái đang chờ cho đến khi xác minh được hoặc hết thời hạn 30 phút.
- Q: Nếu đơn hàng hết hiệu lực trước khi thanh toán được xác nhận thì xử lý thế nào? → A: Không phát hành vé; ghi nhận sự kiện cần rà soát dù nhà cung cấp báo thanh toán thành công.
- Q: Sau khi thanh toán thành công, người dùng có cần xem biên nhận không? → A: Có. Ứng dụng hiển thị biên nhận hoặc tóm tắt giao dịch gồm mã đơn hàng, số tiền, thời gian và trạng thái.
- Q: Biên nhận cần hiển thị mã tham chiếu thanh toán nào cho người dùng? → A: Hiển thị mã đơn hàng và mã giao dịch hoặc trace rút gọn.
- Q: Khi có sự kiện thanh toán cần rà soát, hệ thống có cần thông báo cho vận hành không? → A: Có. Tạo cảnh báo hoặc hàng đợi rà soát cho admin hoặc support.
- Q: Sự kiện thanh toán cần rà soát phải được xử lý trong bao lâu? → A: Admin hoặc support cần xử lý trong vòng 24 giờ.

## Kịch bản người dùng & Kiểm thử *(bắt buộc)*

### Câu chuyện người dùng 1 - Bắt đầu thanh toán từ ứng dụng di động (Độ ưu tiên: P1)

Là người mua vé trên ứng dụng di động, tôi muốn bắt đầu thanh toán cho một đơn hàng và được chuyển đến trang thanh toán bảo mật của nhà cung cấp thanh toán, để có thể thanh toán mà không làm lộ xử lý thanh toán nhạy cảm trong ứng dụng.

**Lý do ưu tiên**: Đây là năng lực thanh toán tối thiểu cần có để người dùng hoàn tất luồng mua vé.

**Kiểm thử độc lập**: Có thể kiểm thử bằng cách tạo một đơn hàng đủ điều kiện thanh toán trong ứng dụng, chọn thanh toán, và xác nhận người dùng nhận được trang thanh toán hợp lệ cho đúng số tiền và mã đơn hàng.

**Kịch bản chấp nhận**:

1. **Cho trước** người dùng có một đơn hàng chưa thanh toán và đủ điều kiện thanh toán, **Khi** người dùng bấm "Thanh toán", **Thì** ứng dụng khởi tạo yêu cầu thanh toán và mở trang thanh toán của nhà cung cấp cho đơn hàng đó.
2. **Cho trước** yêu cầu thanh toán được tạo thành công, **Khi** trang thanh toán được mở, **Thì** thông tin thanh toán hiển thị khớp với số tiền và mã tham chiếu của đơn hàng.
3. **Cho trước** việc tạo yêu cầu thanh toán thất bại, **Khi** người dùng thử thanh toán, **Thì** ứng dụng hiển thị thông báo lỗi rõ ràng và cho phép thử lại mà không tạo trùng đơn hàng đã thanh toán.

---

### Câu chuyện người dùng 2 - Xác minh kết quả thanh toán (Độ ưu tiên: P1)

Là người mua vé, tôi muốn ứng dụng hiển thị thanh toán của tôi thành công, thất bại hay đang chờ xử lý sau khi hoàn tất trang thanh toán, để biết việc mua vé đã hoàn tất hay chưa.

**Lý do ưu tiên**: Kết quả thanh toán phải được xác minh trước khi phát hành vé hoặc xác nhận mua hàng thành công.

**Kiểm thử độc lập**: Có thể kiểm thử bằng cách hoàn tất các kết quả thanh toán từ nhà cung cấp như thành công, thất bại, hủy và đang chờ, sau đó xác nhận ứng dụng hiển thị đúng trạng thái cho người dùng.

**Kịch bản chấp nhận**:

1. **Cho trước** nhà cung cấp thanh toán chuyển hướng về sau khi thanh toán thành công, **Khi** kết quả thanh toán được xác minh, **Thì** đơn hàng được đánh dấu đã thanh toán và ứng dụng hiển thị trạng thái thành công.
2. **Cho trước** nhà cung cấp thanh toán chuyển hướng về sau khi thanh toán thất bại hoặc bị hủy, **Khi** kết quả thanh toán được xác minh, **Thì** đơn hàng vẫn ở trạng thái chưa thanh toán và ứng dụng hiển thị trạng thái thất bại hoặc đã hủy kèm lựa chọn thử lại.
3. **Cho trước** kết quả thanh toán chưa thể xác minh ngay, **Khi** ứng dụng kiểm tra kết quả đơn hàng, **Thì** ứng dụng hiển thị trạng thái đang chờ và không phát hành quyền lợi đã thanh toán cho đến khi thanh toán được xác nhận.

---

### Câu chuyện người dùng 3 - Nhận callback từ nhà cung cấp một cách an toàn (Độ ưu tiên: P2)

Là đơn vị vận hành hệ thống bán vé, tôi muốn callback thanh toán được backend nhận và xác minh, để trạng thái thanh toán không thể bị giả mạo từ ứng dụng di động.

**Lý do ưu tiên**: Xác minh phía backend bảo vệ doanh thu, ngăn xác nhận thanh toán giả và tạo nguồn dữ liệu tin cậy cho trạng thái đơn hàng.

**Kiểm thử độc lập**: Có thể kiểm thử bằng cách gửi dữ liệu callback hợp lệ và không hợp lệ, sau đó xác minh chỉ các kết quả đã được nhà cung cấp xác nhận hợp lệ mới cập nhật đơn hàng.

**Kịch bản chấp nhận**:

1. **Cho trước** nhà cung cấp thanh toán gửi callback hợp lệ, **Khi** backend xác minh kết quả thanh toán, **Thì** đơn hàng tương ứng được cập nhật sang trạng thái thanh toán đã xác minh.
2. **Cho trước** dữ liệu callback bị thiếu, bị trùng, hết hạn hoặc không hợp lệ, **Khi** backend nhận dữ liệu đó, **Thì** trạng thái đơn hàng không bị đánh dấu sai là đã thanh toán và sự kiện được ghi nhận để rà soát.
3. **Cho trước** một thanh toán hợp lệ đã được xử lý, **Khi** callback tương tự được gửi lại, **Thì** đơn hàng vẫn giữ trạng thái cuối cùng đúng và không phát hành vé trùng.

---

### Câu chuyện người dùng 4 - Thông báo cho ứng dụng sau khi backend xác nhận (Độ ưu tiên: P3)

Là người mua vé, tôi muốn quay lại ứng dụng từ luồng thanh toán và thấy trạng thái đơn hàng mới nhất, để có thể tiếp tục đến thông tin vé mà không cần làm mới thủ công.

**Lý do ưu tiên**: Tính năng này cải thiện trải nghiệm người dùng sau khi luồng thanh toán và xác minh cốt lõi đã ổn định.

**Kiểm thử độc lập**: Có thể kiểm thử bằng cách hoàn tất thanh toán từ trang nhà cung cấp và xác nhận người dùng được đưa về ứng dụng hoặc nhận được cập nhật trạng thái trong ứng dụng cho cùng đơn hàng.

**Kịch bản chấp nhận**:

1. **Cho trước** thanh toán đã được xác nhận, **Khi** người dùng quay lại ứng dụng, **Thì** ứng dụng hiển thị đơn hàng đã thanh toán và thông tin vé khả dụng.
2. **Cho trước** thanh toán chưa hoàn tất, **Khi** người dùng quay lại ứng dụng, **Thì** ứng dụng hiển thị trạng thái chưa thanh toán và cho phép người dùng bắt đầu lại thanh toán nếu đơn hàng còn đủ điều kiện.
3. **Cho trước** thanh toán đã được xác nhận, **Khi** người dùng xem kết quả thanh toán trong ứng dụng, **Thì** ứng dụng hiển thị biên nhận hoặc tóm tắt giao dịch gồm mã đơn hàng, mã giao dịch hoặc trace rút gọn, số tiền, thời gian và trạng thái.

### Trường hợp biên

- Người dùng đóng trang thanh toán trước khi hoàn tất thanh toán.
- Người dùng thanh toán thành công nhưng mất kết nối mạng trước khi ứng dụng nhận kết quả.
- Nhà cung cấp thanh toán chuyển hướng về với tham số bị thiếu, sai định dạng, hết hạn hoặc bị chỉnh sửa.
- Callback thanh toán được gửi nhiều lần cho cùng một đơn hàng.
- Nhà cung cấp thanh toán trả về số tiền hoặc mã tham chiếu đơn hàng khác với đơn hàng ban đầu.
- Người dùng cố thanh toán một đơn hàng đã hết hạn, đã hủy, đã thanh toán hoặc không còn khả dụng.
- Người dùng cố tạo thêm một lần thanh toán khi đơn hàng đã có một lần thanh toán đang chờ.
- Việc xác minh phía backend tạm thời không khả dụng hoặc quá thời gian chờ.
- Người dùng mở URL callback hoặc URL kết quả bên ngoài ứng dụng di động.
- Trạng thái đang chờ vượt quá 30 phút mà chưa có kết quả xác minh từ nhà cung cấp.
- Backend không thể redirect người dùng về ứng dụng sau khi xác minh do deep link hoặc app link không khả dụng.

## Yêu cầu *(bắt buộc)*

### Yêu cầu chức năng

- **FR-001**: Hệ thống PHẢI cho phép người dùng ứng dụng di động bắt đầu thanh toán cho một đơn hàng chưa thanh toán và đủ điều kiện.
- **FR-002**: Hệ thống PHẢI chỉ tạo yêu cầu thanh toán từ ngữ cảnh backend đáng tin cậy, không tạo trực tiếp từ ứng dụng di động.
- **FR-003**: Hệ thống PHẢI cung cấp cho ứng dụng di động một điểm đến thanh toán để người dùng hoàn tất thanh toán qua nhà cung cấp thanh toán.
- **FR-004**: Hệ thống PHẢI liên kết mỗi yêu cầu thanh toán với đúng một đơn hàng, số tiền, đơn vị tiền tệ, mã tham chiếu đơn hàng và trạng thái thanh toán.
- **FR-005**: Hệ thống PHẢI gửi cho nhà cung cấp thanh toán một điểm nhận kết quả thuộc backend để xử lý callback và chuyển hướng.
- **FR-006**: Hệ thống PHẢI xác minh dữ liệu callback hoặc dữ liệu trả về trước khi thay đổi bất kỳ đơn hàng nào sang trạng thái đã thanh toán.
- **FR-007**: Hệ thống PHẢI xác minh kết quả từ nhà cung cấp thanh toán khớp với mã tham chiếu và số tiền phải thanh toán của đơn hàng ban đầu trước khi xác nhận thanh toán.
- **FR-008**: Hệ thống PHẢI ngăn dữ liệu kết quả thanh toán do client gửi lên trực tiếp đánh dấu đơn hàng là đã thanh toán khi chưa có xác minh từ backend.
- **FR-009**: Hệ thống PHẢI hỗ trợ các kết quả thanh toán hiển thị cho người dùng: thành công, thất bại, đã hủy, đang chờ xử lý và lỗi xác minh.
- **FR-010**: Hệ thống PHẢI chỉ cập nhật đơn hàng và quyền nhận vé sau khi thanh toán được xác nhận thành công.
- **FR-011**: Hệ thống PHẢI xử lý callback trùng hoặc các lần kiểm tra kết quả lặp lại theo cách không tạo trùng vé, biên nhận hoặc bản ghi thanh toán.
- **FR-012**: Hệ thống PHẢI lưu lịch sử các lần thanh toán cho mỗi đơn hàng, bao gồm mã tham chiếu nhà cung cấp, trạng thái, thời điểm và lý do thất bại khi có.
- **FR-013**: Hệ thống PHẢI thông báo hoặc chuyển hướng người dùng về một kết quả thanh toán hiển thị được trong ứng dụng sau khi backend xác minh xong hoặc xác định trạng thái đang chờ.
- **FR-014**: Hệ thống PHẢI cho phép người dùng thử thanh toán lại khi đơn hàng vẫn chưa thanh toán và còn đủ điều kiện thanh toán.
- **FR-015**: Hệ thống PHẢI ghi nhận các sự kiện thanh toán không hợp lệ, đáng ngờ hoặc không thể xác minh để phục vụ rà soát vận hành.
- **FR-016**: Hệ thống PHẢI tránh để lộ khóa bí mật, thông tin xác minh hoặc vật liệu ký của nhà cung cấp thanh toán cho ứng dụng di động.
- **FR-017**: Hệ thống PHẢI coi trạng thái đã thanh toán là trạng thái cuối trong phạm vi tính năng này; các callback đến sau không được đổi đơn hàng đã thanh toán sang thất bại hoặc đã hủy, và phải được ghi nhận để rà soát.
- **FR-018**: Hệ thống PHẢI giữ trạng thái đang chờ thanh toán tối đa 30 phút; sau thời hạn này, người dùng được tạo lần thanh toán mới nếu đơn hàng vẫn còn hiệu lực.
- **FR-019**: Hệ thống PHẢI dùng backend làm điểm nhận callback chính; sau khi xác minh, backend PHẢI redirect người dùng về ứng dụng bằng deep link hoặc app link với trạng thái kết quả đã xác minh hoặc đang chờ.
- **FR-020**: Hệ thống PHẢI chỉ cho phép mỗi đơn hàng có 1 lần thanh toán đang chờ tại một thời điểm; yêu cầu tạo lần thanh toán mới phải bị từ chối cho đến khi lần đang chờ thành công, thất bại, bị hủy hoặc hết thời hạn 30 phút.
- **FR-021**: Hệ thống PHẢI lưu lịch sử thanh toán tối thiểu 24 tháng sau ngày thanh toán hoặc sau lần thanh toán cuối cùng của đơn hàng, tùy thời điểm nào muộn hơn.
- **FR-022**: Hệ thống PHẢI lưu dữ liệu callback phục vụ rà soát gồm trạng thái, thời điểm, mã tham chiếu, mã lỗi hoặc lý do thất bại khi có, và payload callback đã che hoặc loại bỏ dữ liệu nhạy cảm.
- **FR-023**: Khi backend không thể redirect người dùng về ứng dụng bằng deep link hoặc app link, hệ thống PHẢI hiển thị trang kết quả trên trình duyệt với trạng thái đã xác minh hoặc đang chờ và cung cấp lựa chọn mở lại ứng dụng hoặc thử lại khi đủ điều kiện.
- **FR-024**: Hệ thống PHẢI giới hạn quyền truy cập lịch sử thanh toán: người dùng chỉ được xem thanh toán của chính mình, còn admin hoặc support chỉ được tra cứu khi có quyền phù hợp để hỗ trợ hoặc đối soát.
- **FR-025**: Khi nhà cung cấp báo thành công nhưng số tiền hoặc mã tham chiếu không khớp đơn hàng ban đầu, hệ thống KHÔNG ĐƯỢC xác nhận thanh toán, KHÔNG ĐƯỢC phát hành vé, và PHẢI ghi nhận sự kiện cần rà soát.
- **FR-026**: Khi backend chưa xác minh được kết quả thanh toán với nhà cung cấp, hệ thống PHẢI giữ lần thanh toán ở trạng thái đang chờ cho đến khi xác minh được hoặc hết thời hạn 30 phút; hệ thống KHÔNG ĐƯỢC đánh dấu thất bại chỉ vì lỗi xác minh tạm thời.
- **FR-027**: Khi đơn hàng hết hiệu lực trước thời điểm thanh toán được backend xác nhận, hệ thống KHÔNG ĐƯỢC phát hành vé tự động dù nhà cung cấp báo thanh toán thành công, và PHẢI ghi nhận sự kiện cần rà soát.
- **FR-028**: Sau khi thanh toán được xác nhận thành công, hệ thống PHẢI hiển thị cho người dùng biên nhận hoặc tóm tắt giao dịch trong ứng dụng, gồm mã đơn hàng, mã giao dịch hoặc trace rút gọn, số tiền, thời gian thanh toán và trạng thái.
- **FR-029**: Khi phát sinh sự kiện thanh toán cần rà soát, hệ thống PHẢI tạo cảnh báo hoặc đưa sự kiện vào hàng đợi rà soát cho admin hoặc support có quyền phù hợp.
- **FR-030**: Các sự kiện thanh toán cần rà soát PHẢI có mục tiêu xử lý trong vòng 24 giờ kể từ khi được ghi nhận hoặc đưa vào hàng đợi rà soát.

### Thực thể chính *(bao gồm khi tính năng có dữ liệu)*

- **Đơn hàng**: Yêu cầu mua hàng của người dùng có thể được thanh toán, gồm số tiền, đơn vị tiền tệ, mã tham chiếu đơn hàng, trạng thái đủ điều kiện và trạng thái thanh toán.
- **Lần thanh toán**: Một lần thử thanh toán cho một đơn hàng, gồm mã tham chiếu nhà cung cấp, điểm đến thanh toán, trạng thái, thời điểm, chi tiết kết quả và thời hạn lưu tối thiểu 24 tháng.
- **Callback thanh toán**: Dữ liệu kết quả thanh toán do nhà cung cấp trả về, cần được xác minh trước khi thay đổi trạng thái đơn hàng; bản ghi lưu trữ phải che hoặc loại bỏ dữ liệu nhạy cảm.
- **Kết quả thanh toán**: Kết quả hiển thị cho người dùng sau xác minh, ví dụ thành công, thất bại, đã hủy, đang chờ hoặc lỗi xác minh.
- **Biên nhận thanh toán**: Tóm tắt giao dịch hiển thị cho người dùng sau khi thanh toán thành công, gồm mã đơn hàng, mã giao dịch hoặc trace rút gọn, số tiền, thời gian thanh toán và trạng thái.
- **Quyền nhận vé**: Quyền của người dùng được nhận hoặc xem vé sau khi đơn hàng được xác nhận đã thanh toán.

## Tiêu chí thành công *(bắt buộc)*

### Kết quả đo lường được

- **SC-001**: Ít nhất 95% người dùng đủ điều kiện có thể bắt đầu thanh toán và đến trang thanh toán của nhà cung cấp trong vòng 10 giây trong điều kiện vận hành bình thường.
- **SC-002**: 100% xác nhận đơn hàng đã thanh toán dựa trên kết quả đã được backend xác minh, không dựa trên dữ liệu chưa xác minh từ ứng dụng di động.
- **SC-003**: 99% callback thành công từ nhà cung cấp cập nhật đơn hàng tương ứng sang trạng thái đã thanh toán trong vòng 30 giây trong điều kiện vận hành bình thường.
- **SC-004**: Callback trùng hoặc các lần kiểm tra kết quả lặp lại tạo ra 0 đơn hàng đã thanh toán, vé hoặc biên nhận bị trùng trong kiểm thử chấp nhận.
- **SC-005**: Ít nhất 90% người dùng hiểu được kết quả thanh toán trên màn hình kết quả của ứng dụng mà không cần liên hệ hỗ trợ trong buổi đánh giá khả dụng.
- **SC-006**: Các yêu cầu hỗ trợ liên quan đến trạng thái thành công, thất bại hoặc đang chờ không rõ ràng duy trì dưới 2% tổng số lần thanh toán hoàn tất sau khi ra mắt.
- **SC-007**: 100% lần thanh toán ở trạng thái đang chờ quá 30 phút được xử lý để người dùng có thể thử thanh toán lại khi đơn hàng còn hiệu lực.
- **SC-008**: 100% trường hợp không thể redirect về ứng dụng vẫn hiển thị cho người dùng một kết quả thanh toán trên trình duyệt trong cùng phiên thanh toán.
- **SC-009**: Ít nhất 95% sự kiện thanh toán cần rà soát được admin hoặc support xử lý trong vòng 24 giờ.

## Giả định

- Nhà cung cấp thanh toán là OnePay hoặc một nhà cung cấp tương đương do khách hàng cung cấp, và thông tin xác thực riêng của nhà cung cấp được quản lý ngoài ứng dụng di động.
- Ứng dụng di động đã có luồng tạo đơn hàng hoặc mua vé có thể cung cấp đơn hàng chưa thanh toán và đủ điều kiện thanh toán.
- Backend là nguồn dữ liệu chính xác cho trạng thái thanh toán đơn hàng và phát hành vé.
- Điểm nhận kết quả thuộc backend được ưu tiên hơn deep link trực tiếp vào ứng dụng, vì việc xác minh và bảo vệ thông tin bí mật phải diễn ra phía server.
- Luồng chính sau thanh toán là backend xác minh callback rồi redirect người dùng về ứng dụng bằng deep link hoặc app link; thông báo đẩy hoặc kiểm tra trạng thái chỉ là cơ chế bổ trợ khi người dùng không quay lại ứng dụng ngay.
- Hoàn tiền, thanh toán một phần, trả góp, khiếu nại giao dịch và đối soát thủ công nằm ngoài phạm vi tính năng này, trừ khi được bổ sung trong đặc tả sau.
