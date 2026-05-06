# Feature: Quản lý Vé (Ticket Management) - Web CMS

**Ngày tạo:** 2026-05-04
**Phiên bản:** 1.0
**Trạng thái:** Draft

---

## 1. Tổng quan

Hệ thống Quản lý Vé trên Web CMS cho phép admin/operator tạo, cấu hình, phân phối và theo dõi vé (tickets) cho các sự kiện, tour, địa điểm tham quan thuộc hệ sinh thái Heritage360. Mục tiêu là tập trung hóa toàn bộ quy trình vận hành vé từ một giao diện quản trị duy nhất.

---

## 2. Người dùng mục tiêu (Target Users)

| Role | Mô tả |
|------|--------|
| **Super Admin** | Toàn quyền quản lý tất cả vé, sự kiện, địa điểm |
| **Event Manager** | Tạo và quản lý vé cho sự kiện/tour được phân quyền |
| **Finance Staff** | Xem doanh thu, xuất báo cáo, quản lý hoàn tiền |
| **Gate Staff** | Xác nhận/check-in vé tại cửa (read-only + check-in) |

---

## 3. User Stories

### 3.1 Quản lý Loại Vé (Ticket Type)
- Là **Event Manager**, tôi muốn **tạo nhiều loại vé** (VIP, Standard, Early Bird) cho một sự kiện để phân khúc khách hàng theo nhu cầu.
- Là **Event Manager**, tôi muốn **đặt giá và số lượng** cho từng loại vé để kiểm soát doanh thu và công suất.
- Là **Event Manager**, tôi muốn **thiết lập thời gian mở/đóng bán** cho từng loại vé để tự động hóa quy trình bán.

### 3.2 Quản lý Sự kiện / Tour
- Là **Event Manager**, tôi muốn **tạo sự kiện mới** kèm thông tin địa điểm, ngày giờ, hình ảnh để publish lên web.
- Là **Event Manager**, tôi muốn **liên kết nhiều loại vé** với một sự kiện để cung cấp lựa chọn cho khách hàng.
- Là **Event Manager**, tôi muốn **đặt quota theo ngày/suất** để quản lý sức chứa.

### 3.3 Quản lý Đơn hàng (Orders)
- Là **Super Admin**, tôi muốn **xem danh sách tất cả đơn hàng** có filter và search để nhanh chóng tra cứu.
- Là **Finance Staff**, tôi muốn **xem chi tiết từng đơn hàng** (người mua, loại vé, phương thức thanh toán) để đối soát.
- Là **Finance Staff**, tôi muốn **xử lý hoàn tiền** (refund) cho đơn hàng đủ điều kiện theo chính sách.
- Là **Event Manager**, tôi muốn **hủy đơn hàng thủ công** và gửi thông báo cho khách hàng.

### 3.4 Check-in / Xác thực Vé
- Là **Gate Staff**, tôi muốn **quét mã QR/barcode** để xác nhận vé hợp lệ tại cổng vào.
- Là **Gate Staff**, tôi muốn **tra cứu vé theo tên/email** để hỗ trợ khách không có vé điện tử.
- Là **Super Admin**, tôi muốn **xem lịch sử check-in** (ai check-in, thời gian, cổng) để kiểm soát.

### 3.5 Báo cáo & Thống kê
- Là **Finance Staff**, tôi muốn **xem dashboard doanh thu** theo ngày/tuần/tháng/sự kiện để báo cáo lãnh đạo.
- Là **Super Admin**, tôi muốn **xuất báo cáo CSV/Excel** về danh sách người tham dự và doanh thu.
- Là **Event Manager**, tôi muốn **xem tỉ lệ bán vé real-time** để quyết định mở thêm quota hay dừng bán.

### 3.6 Cấu hình & Chính sách
- Là **Super Admin**, tôi muốn **cấu hình chính sách hoàn tiền** (% hoàn, thời hạn trước sự kiện) theo từng sự kiện.
- Là **Super Admin**, tôi muốn **tùy chỉnh mẫu email xác nhận** gửi cho khách sau khi mua vé thành công.
- Là **Super Admin**, tôi muốn **cấu hình phương thức thanh toán** (VNPay, Momo, thẻ ngân hàng) cho từng sự kiện.

---

## 4. Breakdown Tính năng (Feature Breakdown)

### Module 1: Ticket Type Management
| ID | Tính năng | Mức độ ưu tiên | Phức tạp |
|----|-----------|----------------|----------|
| TT-01 | Tạo / Chỉnh sửa / Xóa loại vé | Must Have | Low |
| TT-02 | Đặt giá (fixed / dynamic pricing) | Must Have | Medium |
| TT-03 | Đặt số lượng tối đa (quota) | Must Have | Low |
| TT-04 | Thiết lập thời gian mở/đóng bán | Must Have | Low |
| TT-05 | Giới hạn số lượng mua / người dùng | Should Have | Medium |
| TT-06 | Mã giảm giá / voucher | Should Have | High |
| TT-07 | Vé theo gói (bundle tickets) | Nice to Have | High |

### Module 2: Event / Tour Management
| ID | Tính năng | Mức độ ưu tiên | Phức tạp |
|----|-----------|----------------|----------|
| EV-01 | Tạo / Chỉnh sửa / Ẩn/Hiện sự kiện | Must Have | Medium |
| EV-02 | Upload hình ảnh, banner sự kiện | Must Have | Low |
| EV-03 | Liên kết loại vé với sự kiện | Must Have | Low |
| EV-04 | Quản lý nhiều suất (multiple sessions) | Must Have | High |
| EV-05 | Clone sự kiện (tái sử dụng cấu hình) | Should Have | Medium |
| EV-06 | Preview trang sự kiện trước publish | Should Have | Medium |

### Module 3: Order Management
| ID | Tính năng | Mức độ ưu tiên | Phức tạp |
|----|-----------|----------------|----------|
| OR-01 | Danh sách đơn hàng có filter, search, sort | Must Have | Medium |
| OR-02 | Chi tiết đơn hàng (thông tin người mua, vé) | Must Have | Low |
| OR-03 | Hủy đơn hàng thủ công + gửi email | Must Have | Medium |
| OR-04 | Xử lý hoàn tiền (full / partial refund) | Must Have | High |
| OR-05 | Tạo đơn hàng thủ công (walk-in / phone) | Should Have | Medium |
| OR-06 | Gửi lại email xác nhận / vé | Should Have | Low |
| OR-07 | Export danh sách đơn hàng (CSV/Excel) | Should Have | Low |

### Module 4: Check-in Management
| ID | Tính năng | Mức độ ưu tiên | Phức tạp |
|----|-----------|----------------|----------|
| CI-01 | Tra cứu vé theo mã/tên/email | Must Have | Low |
| CI-02 | Check-in thủ công (1-click confirm) | Must Have | Low |
| CI-03 | Quét QR code trên web/mobile | Must Have | Medium |
| CI-04 | Ngăn check-in trùng lặp | Must Have | Medium |
| CI-05 | Lịch sử check-in theo sự kiện | Should Have | Low |
| CI-06 | Đặt check-in theo cổng (gate) | Nice to Have | Medium |

### Module 5: Reports & Analytics
| ID | Tính năng | Mức độ ưu tiên | Phức tạp |
|----|-----------|----------------|----------|
| RP-01 | Dashboard tổng quan (doanh thu, số vé bán) | Must Have | High |
| RP-02 | Thống kê theo sự kiện / loại vé | Must Have | Medium |
| RP-03 | Biểu đồ doanh thu theo thời gian | Should Have | Medium |
| RP-04 | Báo cáo danh sách tham dự (attendance) | Must Have | Low |
| RP-05 | Export báo cáo CSV/Excel/PDF | Should Have | Medium |
| RP-06 | Real-time ticket sales tracker | Should Have | High |

### Module 6: Configuration & Settings
| ID | Tính năng | Mức độ ưu tiên | Phức tạp |
|----|-----------|----------------|----------|
| CF-01 | Cấu hình phương thức thanh toán | Must Have | High |
| CF-02 | Chính sách hoàn tiền theo sự kiện | Must Have | Medium |
| CF-03 | Tùy chỉnh mẫu email (confirmation, reminder) | Should Have | Medium |
| CF-04 | Cấu hình thuế / phí dịch vụ | Should Have | Medium |
| CF-05 | Tích hợp webhook (Zapier, CRM) | Nice to Have | High |

---

## 5. Acceptance Criteria (Tiêu chí chấp nhận)

### AC-01: Tạo loại vé
- [ ] Admin có thể tạo loại vé với tên, mô tả, giá, số lượng
- [ ] Hệ thống validate: giá >= 0, số lượng > 0
- [ ] Loại vé hiển thị ngay trong danh sách sau khi tạo
- [ ] Có thể edit hoặc deactivate (không xóa cứng nếu đã có đơn hàng)

### AC-02: Quản lý đơn hàng
- [ ] Danh sách load trong vòng < 2 giây với 10.000+ đơn hàng
- [ ] Filter hoạt động: theo trạng thái, ngày, sự kiện, phương thức thanh toán
- [ ] Search theo tên, email, mã đơn hàng
- [ ] Phân trang (pagination) hoặc infinite scroll

### AC-03: Hoàn tiền
- [ ] Chỉ Finance Staff và Super Admin mới có quyền thực hiện
- [ ] Hệ thống kiểm tra chính sách hoàn tiền trước khi cho phép
- [ ] Ghi log đầy đủ: ai hoàn, thời gian, lý do
- [ ] Gửi email thông báo cho khách hàng sau khi hoàn tiền

### AC-04: Check-in
- [ ] Ngăn check-in vé đã được check-in trước đó (hiển thị cảnh báo)
- [ ] Xác nhận check-in trong < 1 giây
- [ ] Hoạt động offline (caching local nếu mất mạng tạm thời)

---

## 6. Luồng nghiệp vụ (Business Flows)

### Flow 1: Tạo sự kiện và bán vé
```
Admin tạo sự kiện
    → Thêm loại vé (type, giá, quota)
    → Cấu hình thanh toán & chính sách
    → Publish sự kiện
    → Khách hàng mua vé (frontend)
    → Hệ thống tạo Order + gửi email xác nhận + QR code
```

### Flow 2: Check-in tại sự kiện
```
Gate Staff mở màn hình check-in
    → Quét QR hoặc tìm kiếm tên/email
    → Hệ thống xác thực vé (hợp lệ / đã dùng / không tồn tại)
    → Xác nhận check-in → ghi log
```

### Flow 3: Hoàn tiền
```
Khách yêu cầu hoàn tiền
    → Finance Staff tra cứu đơn hàng
    → Kiểm tra chính sách hoàn tiền
    → Xử lý hoàn tiền (manual trigger qua payment gateway)
    → Cập nhật trạng thái đơn hàng → gửi email xác nhận
```

---

## 7. Yêu cầu phi chức năng (Non-Functional Requirements)

| NFR | Yêu cầu |
|-----|---------|
| **Performance** | Danh sách vé/đơn hàng load < 2s; Check-in response < 1s |
| **Security** | Phân quyền RBAC; Log tất cả thao tác nhạy cảm (refund, cancel) |
| **Scalability** | Hỗ trợ 10.000+ đơn hàng/sự kiện; 500 concurrent check-ins |
| **Availability** | Uptime 99.9%; Offline mode cho check-in |
| **Audit Trail** | Lưu lịch sử thay đổi: ai sửa, khi nào, giá trị cũ/mới |

---

## 8. Rủi ro & Phụ thuộc

| Rủi ro | Mức độ | Giảm thiểu |
|--------|--------|------------|
| Tích hợp payment gateway phức tạp | High | Chọn gateway có SDK tốt (VNPay, Stripe); spike trước |
| Xung đột số lượng vé (race condition) | High | Dùng database-level locking hoặc queue |
| Check-in offline không đồng bộ đúng | Medium | Sync khi có mạng + UI hiển thị trạng thái sync |
| Dữ liệu cũ không có cấu trúc chuẩn | Medium | Data migration plan trước khi launch |

---

## 9. Thứ tự triển khai đề xuất (Phased Rollout)

### Phase 1 - MVP (4-6 tuần)
- TT-01, TT-02, TT-03, TT-04
- EV-01, EV-02, EV-03
- OR-01, OR-02, OR-03
- CI-01, CI-02, CI-03, CI-04
- CF-01

### Phase 2 - Core (4-6 tuần)
- OR-04 (Refund), OR-05, OR-06, OR-07
- RP-01, RP-02, RP-04
- CF-02, CF-03
- EV-04 (Multiple sessions)

### Phase 3 - Advanced (4-6 tuần)
- TT-06 (Voucher/discount)
- RP-03, RP-05, RP-06
- CI-05, CI-06
- CF-04, CF-05

---

## 10. Bước tiếp theo

- [ ] Review và xác nhận với stakeholders
- [ ] Chuyển sang Product Manager để tạo PRD chi tiết
- [ ] UX Designer thiết kế wireframes cho các màn hình chính
- [ ] Tech Lead đánh giá technical feasibility và estimate
- [ ] Xác định payment gateway partner


### 11. Câu hỏi & Ghi chú
- Cần xác định rõ ràng phân quyền cho từng role (Super Admin, Event Manager, Finance Staff, Gate Staff) để tránh lẫn lộn quyền hạn.

- Ai sẽ là chịu trách nhiệm quản lý và vận hành hệ thống sau khi triển khai? sở văn hoá hà nội hay bên vietsoftpro, hay là cả hai bên cùng quản lý? Cần phân định rõ ràng để tránh xung đột và đảm bảo hiệu quả vận hành.

- Cần thảo luận về việc có nên xây dựng hệ thống quản lý vé này như một module độc lập hay tích hợp trực tiếp vào hệ thống quản lý sự kiện hiện tại của Heritage360. Việc này sẽ ảnh hưởng đến kiến trúc hệ thống và cách triển khai sau này.

- Cần xác định rõ ràng các chính sách liên quan đến vé như hoàn tiền, hủy vé, chuyển nhượng vé để xây dựng hệ thống phù hợp với các quy định này.
