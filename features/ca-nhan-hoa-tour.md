# Feature: Cá Nhân Hóa Tour — AI Chatbot

**Ngày tạo:** 2026-05-04  
**Phiên bản:** 1.0  
**Trạng thái:** Draft  
**Tham khảo:** Mindtrip.ai (mindtrip.ai)

---

## 1. Tổng quan

Tính năng **Cá Nhân Hóa Tour** cho phép người dùng trò chuyện với AI chatbot để tạo ra lịch trình tham quan (tour) được cá nhân hóa hoàn toàn theo sở thích, ngân sách, thời gian và phong cách của từng cá nhân. Thay vì chọn tour có sẵn, người dùng mô tả nhu cầu bằng ngôn ngữ tự nhiên và AI sẽ tự động xây dựng hành trình phù hợp.

### Vấn đề cần giải quyết
- Tour du lịch truyền thống "one-size-fits-all" không đáp ứng nhu cầu đa dạng của từng người
- Người dùng mất nhiều thời gian nghiên cứu, tổng hợp thông tin từ nhiều nguồn
- Khó cân bằng sở thích khi đi nhóm (gia đình, bạn bè)
- Thiếu tính linh hoạt để điều chỉnh hành trình theo thời gian thực

### Mục tiêu
- Giảm thời gian lập kế hoạch tour từ vài giờ xuống còn vài phút
- Tăng tỷ lệ hài lòng nhờ nội dung được cá nhân hóa
- Tăng tỷ lệ chuyển đổi (đặt tour) thông qua trải nghiệm tư vấn tự nhiên

---

## 2. Người dùng mục tiêu

| Nhóm | Đặc điểm | Nhu cầu chính |
|------|-----------|---------------|
| Khách tự túc (FIT) | Không muốn tour cố định, thích tự sắp xếp | Hành trình linh hoạt, gợi ý cụ thể |
| Gia đình có trẻ nhỏ | Cần điểm đến phù hợp mọi lứa tuổi | Lọc theo độ tuổi, an toàn, tiện nghi |
| Cặp đôi | Muốn trải nghiệm lãng mạn, riêng tư | Gợi ý nhà hàng, hoạt động đặc biệt |
| Nhóm bạn | Nhiều sở thích khác nhau, cần cân bằng | Lập kế hoạch cộng tác, voting |
| Khách doanh nhân | Ít thời gian, muốn tối ưu lịch trình | Tour nhanh, hiệu quả, tích hợp lịch |

---

## 3. Luồng tính năng chính

### 3.1 Onboarding — Xây dựng hồ sơ du lịch cá nhân

**Mô tả:** Khi người dùng lần đầu sử dụng tính năng, hệ thống thu thập thông tin cơ bản để xây dựng "Travel Profile" (hồ sơ du lịch).

**Phương thức thu thập:**
- Quiz ngắn (5–7 câu hỏi) về phong cách du lịch
- Hoặc hội thoại tự nhiên ngay với chatbot

**Thông tin cần thu thập:**

| Thông tin | Câu hỏi gợi ý | Ví dụ giá trị |
|-----------|---------------|---------------|
| Phong cách du lịch | "Bạn thích du lịch kiểu nào?" | Phiêu lưu / Thư giãn / Văn hóa / Ẩm thực |
| Ngân sách | "Ngân sách trung bình mỗi ngày?" | Tiết kiệm / Trung bình / Cao cấp |
| Thành phần đoàn | "Bạn đi cùng ai?" | Một mình / Cặp đôi / Gia đình / Bạn bè |
| Sở thích ẩm thực | "Bạn có kiêng ăn gì không?" | Chay / Không hải sản / Không hạn chế |
| Tốc độ di chuyển | "Bạn thích lịch trình dày đặc hay thoải mái?" | Nhiều điểm / Vừa phải / Ít điểm chất lượng |
| Sở thích đặc biệt | "Bạn yêu thích điều gì khi du lịch?" | Nhiếp ảnh / Mua sắm / Lịch sử / Thiên nhiên |

**Kết quả:** Tạo **Travel Persona** — nhân cách du lịch của người dùng, dùng xuyên suốt các cuộc trò chuyện.

---

### 3.2 Khởi tạo tour qua hội thoại

**Mô tả:** Người dùng mô tả nhu cầu và AI tạo lịch trình tour theo thời gian thực.

**Thông tin chatbot cần khai thác:**

```
BẮT BUỘC:
- Điểm đến (tỉnh/thành, di tích, khu vực)
- Ngày đi và ngày về (hoặc số ngày)
- Số lượng người

TÙY CHỌN (cải thiện chất lượng):
- Ngân sách mong muốn
- Phương tiện di chuyển
- Chỗ ở ưa thích (gần trung tâm, gần biển, v.v.)
- Ưu tiên đặc biệt ("có trẻ 5 tuổi", "đi xe lăn", "kỵ leo núi")
```

**Ví dụ hội thoại:**

```
Người dùng: "Tôi muốn đi Hội An 3 ngày cuối tháng 5, 2 người lớn + 1 bé 8 tuổi, 
              thích ăn uống và chụp ảnh, ngân sách vừa phải"

Chatbot: "Tuyệt vời! Tôi sẽ tạo lịch trình Hội An 3 ngày cho gia đình bạn. 
          Lưu ý tôi sẽ ưu tiên những điểm đẹp để chụp ảnh và phù hợp với bé 8 tuổi nhé.
          
          [Tạo lịch trình...]"
```

**Output được tạo ra:**
- Lịch trình chi tiết theo từng ngày (Sáng / Chiều / Tối)
- Danh sách điểm tham quan có hình ảnh, mô tả, thời gian tham quan
- Gợi ý nhà hàng / quán ăn phù hợp tại từng khu vực
- Ước tính chi phí từng hạng mục
- Bản đồ hiển thị toàn bộ hành trình

---

### 3.3 Tinh chỉnh lịch trình qua hội thoại

**Mô tả:** Sau khi tạo lịch trình ban đầu, người dùng có thể yêu cầu điều chỉnh bằng ngôn ngữ tự nhiên mà không cần làm lại từ đầu.

**Các loại yêu cầu điều chỉnh:**

| Loại | Ví dụ câu hỏi |
|------|--------------|
| Thêm điểm đến | "Thêm làng gốm Thanh Hà vào ngày 2 buổi sáng" |
| Xóa điểm đến | "Bỏ tham quan bảo tàng ra, bé không thích" |
| Hoán đổi thứ tự | "Đổi ngày 1 và ngày 2 cho tôi" |
| Thay thế gợi ý | "Có nhà hàng nào khác không, tôi không thích hải sản" |
| Điều chỉnh thời gian | "Rút ngắn lại, tôi chỉ có 2 ngày thôi" |
| Thay đổi ngân sách | "Nâng cấp chỗ ở lên cao cấp hơn được không?" |
| Thêm yêu cầu đặc biệt | "Thêm lớp học nấu ăn cho bé vào chiều ngày 2" |

---

### 3.4 Tính năng "Lấy cảm hứng → Tạo lịch trình"

**Mô tả:** Người dùng chia sẻ link YouTube, TikTok, bài blog, hay ảnh và AI tự động trích xuất địa điểm để tạo lịch trình.

**Input được hỗ trợ:**
- Link video YouTube/TikTok
- Link bài viết blog du lịch
- Link bài đăng mạng xã hội (Facebook, Instagram)
- Ảnh chụp màn hình nội dung

**Quy trình xử lý:**
1. AI trích xuất danh sách địa điểm được đề cập trong nội dung
2. Xác nhận với người dùng danh sách địa điểm
3. Sắp xếp theo trình tự địa lý hợp lý
4. Tạo lịch trình đầy đủ kèm thông tin bổ sung

---

### 3.5 Lập kế hoạch nhóm (Collaborative Planning)

**Mô tả:** Nhiều người trong nhóm cùng tham gia xây dựng lịch trình.

**Tính năng:**
- Chia sẻ lịch trình qua link / mã QR
- Thành viên nhóm có thể thêm gợi ý hoặc bình chọn
- AI tổng hợp và cân bằng sở thích của tất cả thành viên
- Thông báo realtime khi lịch trình được cập nhật
- Phân quyền: Admin (chỉnh sửa) / Member (xem và đề xuất)

---

### 3.6 Tư vấn theo ngữ cảnh thông minh

**Mô tả:** AI chủ động cảnh báo và tư vấn dựa trên thông tin thực tế.

**Ví dụ tư vấn proactive:**

| Ngữ cảnh | Cảnh báo / Gợi ý |
|----------|-----------------|
| Ngày lễ / Tết | "Ngày 30/4 điểm này rất đông, gợi ý đến sớm 7h sáng" |
| Thời tiết | "Tháng 10 Đà Nẵng hay mưa, nên chuẩn bị áo mưa" |
| Độ tuổi trẻ em | "Điểm này có cầu thang cao, không phù hợp với bé dưới 6 tuổi" |
| Giờ mở cửa | "Bảo tàng này đóng cửa thứ 2, lịch trình ngày 1 của bạn cần điều chỉnh" |
| Di chuyển | "Đoạn đường từ A đến B mất 45 phút, cần khởi hành trước 9h" |
| Đặt trước | "Nhà hàng này cần đặt bàn trước ít nhất 2 ngày" |

---

## 4. Cấu trúc dữ liệu lịch trình

### 4.1 Travel Profile (Hồ sơ du lịch cá nhân)
```
TravelProfile {
  userId
  travelStyle: [adventure, relax, culture, food, shopping, photography]
  budgetRange: [budget, mid-range, luxury]
  groupType: [solo, couple, family, friends, business]
  dietaryRestrictions: [vegetarian, halal, no-seafood, ...]
  mobilityNeeds: [wheelchair, elderly, toddler, ...]
  pace: [packed, moderate, relaxed]
  interests: [history, nature, art, nightlife, ...]
  createdAt
  updatedAt
}
```

### 4.2 Tour Itinerary (Lịch trình tour)
```
Itinerary {
  id
  userId
  title
  destination
  startDate / endDate
  totalDays
  estimatedBudget
  days: [
    {
      dayNumber
      date
      theme (optional)
      slots: [
        {
          period: [morning, afternoon, evening]
          time
          type: [attraction, restaurant, transport, accommodation, activity]
          placeId
          placeName
          placeImage
          duration (minutes)
          estimatedCost
          notes
          bookingRequired: boolean
          bookingUrl (optional)
        }
      ]
    }
  ]
  sharedWith: [userId]
  status: [draft, confirmed, ongoing, completed]
  createdAt / updatedAt
}
```

---

## 5. Conversation Design — Thiết kế hội thoại

### 5.1 Nguyên tắc thiết kế

- **Ngắn gọn:** Chatbot không hỏi quá 2 câu cùng lúc
- **Tiến độ rõ ràng:** Người dùng biết AI đang làm gì ("Đang tìm kiếm nhà hàng phù hợp...")
- **Xác nhận trước khi thực hiện:** Với các thay đổi lớn, hỏi lại trước khi xử lý
- **Gợi ý nhanh:** Cung cấp các lựa chọn nhanh (quick replies) để giảm gõ phím
- **Graceful fallback:** Khi AI không hiểu, hỏi làm rõ thay vì đoán sai

### 5.2 Cấu trúc tin nhắn chatbot

```
[Xác nhận đã hiểu yêu cầu]
[Hành động đang thực hiện]
[Kết quả / Output]
[Câu hỏi tiếp theo hoặc gợi ý tiếp theo]
```

### 5.3 Quick Replies theo ngữ cảnh

Khi tạo lịch trình xong:
```
[ Điều chỉnh lịch trình ] [ Xem bản đồ ] [ Chia sẻ nhóm ] [ Đặt tour ngay ]
```

Khi gợi ý điểm đến:
```
[ Thêm vào lịch trình ] [ Xem thêm ảnh ] [ Bỏ qua ] [ Gợi ý khác ]
```

---

## 6. Phân tầng tính năng (Phased Rollout)

### Phase 1 — MVP (Ưu tiên cao)
- [ ] Quiz xây dựng Travel Profile cơ bản
- [ ] Hội thoại tạo lịch trình theo ngày (văn bản)
- [ ] Tinh chỉnh lịch trình qua chat
- [ ] Hiển thị lịch trình dạng danh sách có ảnh
- [ ] Tư vấn giờ mở cửa, thời tiết cơ bản

### Phase 2 — Enhanced (Ưu tiên trung bình)
- [ ] Bản đồ tương tác hiển thị hành trình
- [ ] Ước tính chi phí chi tiết từng hạng mục
- [ ] Tính năng chia sẻ lịch trình (link / QR)
- [ ] Lập kế hoạch nhóm cơ bản
- [ ] Cảnh báo thông minh theo ngữ cảnh (lễ hội, mùa vụ)

### Phase 3 — Advanced (Ưu tiên thấp)
- [ ] "Lấy cảm hứng từ link" (YouTube, TikTok, blog)
- [ ] Collaborative planning với voting
- [ ] Tích hợp đặt chỗ trực tiếp (khách sạn, vé vào cửa)
- [ ] Lịch sử tour và scrapbook sau chuyến đi
- [ ] AI nhận diện địa điểm qua ảnh chụp

---

## 7. Tiêu chí chấp nhận (Acceptance Criteria)

### Tạo lịch trình
- [ ] Từ thông tin đầu vào cơ bản (điểm đến + số ngày), AI tạo lịch trình trong < 10 giây
- [ ] Lịch trình có ít nhất 3 slot mỗi ngày (sáng / chiều / tối)
- [ ] Mỗi slot có: tên địa điểm, ảnh, thời gian tham quan ước tính, ghi chú ngắn
- [ ] Lịch trình phù hợp với Travel Profile đã tạo

### Tinh chỉnh
- [ ] Người dùng có thể yêu cầu thêm / xóa / hoán đổi điểm qua chat
- [ ] AI giữ nguyên các phần không được yêu cầu thay đổi
- [ ] Sau điều chỉnh, lịch trình được cập nhật trong < 5 giây

### Cá nhân hóa
- [ ] Lịch trình ưu tiên loại hình phù hợp với Travel Persona (ví dụ: khách thích ẩm thực → nhiều gợi ý nhà hàng độc đáo)
- [ ] Lịch trình lọc ra các điểm không phù hợp (ví dụ: có trẻ nhỏ → không gợi ý bar, club)
- [ ] AI chủ động hỏi thêm thông tin nếu thiếu thông tin quan trọng

### Tư vấn thông minh
- [ ] AI cảnh báo khi điểm tham quan đóng cửa vào ngày được chọn
- [ ] AI gợi ý thứ tự địa lý hợp lý để tiết kiệm di chuyển
- [ ] AI đề xuất khoảng cách an toàn giữa các điểm

---

## 8. Chỉ số đo lường thành công (KPIs)

| Chỉ số | Mục tiêu | Cách đo |
|--------|----------|---------|
| Thời gian tạo lịch trình | < 2 phút từ đầu đến cuối | Timestamp hội thoại |
| Tỷ lệ hoàn thành hội thoại | > 70% | Sessions có output lịch trình / tổng sessions |
| Tỷ lệ chỉnh sửa sau tạo | < 3 lần chỉnh sửa trung bình | Số lượng yêu cầu điều chỉnh / session |
| Tỷ lệ chia sẻ lịch trình | > 20% | Lịch trình được share / tổng lịch trình tạo |
| Tỷ lệ chuyển đổi đặt tour | > 15% | Lịch trình dẫn đến booking / tổng lịch trình |
| Điểm hài lòng (CSAT) | > 4.2 / 5.0 | Rating sau mỗi session |

---

## 9. Rủi ro và phụ thuộc

| Rủi ro | Mức độ | Giải pháp |
|--------|--------|-----------|
| Dữ liệu địa điểm không đầy đủ hoặc lỗi thời | Cao | Xây dựng pipeline cập nhật dữ liệu định kỳ |
| AI tạo lịch trình không thực tế (khoảng cách quá xa) | Trung bình | Tích hợp dữ liệu địa lý để tính thời gian di chuyển |
| Người dùng không hoàn thành quiz onboarding | Trung bình | Cho phép bỏ qua quiz, thu thập dần qua hội thoại |
| Chất lượng phản hồi AI không nhất quán | Cao | Định nghĩa prompt templates chặt chẽ, test coverage |
| Hội thoại tiếng Việt kém chất lượng | Trung bình | Fine-tune với dữ liệu du lịch Việt Nam, test kỹ |

---

## 10. Câu hỏi mở cần làm rõ

1. **Phạm vi địa điểm:** Feature này áp dụng cho toàn quốc hay chỉ các điểm di sản (heritage sites) mà Heritage360 đang quản lý?
2. **Tích hợp đặt chỗ:** Giai đoạn đầu có tích hợp đặt vé / phòng không, hay chỉ gợi ý?
3. **Dữ liệu địa điểm:** Nguồn dữ liệu điểm tham quan từ đâu — tự xây, Google Places, hay có sẵn?
4. **Ngôn ngữ:** Hỗ trợ tiếng Việt + tiếng Anh từ đầu hay chỉ tiếng Việt trước?
5. **Lưu trữ lịch trình:** Có lưu lịch sử các lịch trình đã tạo vào tài khoản người dùng không?
6. **Mô hình AI:** Dùng API của bên thứ ba (OpenAI, Gemini) hay tự xây?

---

*Tài liệu này là cơ sở để tiếp tục chuyển sang giai đoạn PRD (Product Requirements Document) và lập kế hoạch kỹ thuật chi tiết.*
