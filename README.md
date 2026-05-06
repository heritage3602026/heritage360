Đúng rồi! Với vai trò BA ở repo specs riêng, chỉ cần 2 lệnh đó thôi.

---

## BA chỉ cần

```
/speckit-specify     ← mô tả feature, sinh ra spec.md
/speckit-clarify     ← làm rõ nếu spec còn mơ hồ
/speckit-git-commit  ← commit + push lên GitHub
```

---

## Những lệnh còn lại để cho team dev

| Lệnh | Ai chạy | Ở đâu |
|---|---|---|
| `/speckit-plan` | Tech Lead BE | `ticket-backend` |
| `/speckit-plan` | Tech Lead Mobile | `ticket-mobile` |
| `/speckit-plan` | Tech Lead CMS | `ticket-cms` |
| `/speckit-tasks` | Tech Lead từng team | repo tương ứng |
| `/speckit-implement` | Member | repo tương ứng |

---

## Flow BA cực gọn

```
Khách mô tả feature
       ↓
/speckit-specify
       ↓
đọc lại spec.md, thấy chỗ nào mơ hồ?
       ↓
/speckit-clarify  (nếu cần)
       ↓
/speckit-git-commit
       ↓
báo Tech Lead: "spec done, sync về làm đi"
```

Vậy là xong việc của BA. Sạch và rõ ràng.


# Khi có thay đổi

## `/speckit-baseline` — Chụp ảnh trạng thái hiện tại

Chạy **trước khi** có thay đổi gì, để Spec Kit biết "điểm xuất phát" là gì.

```
/speckit-baseline
```

Nó sẽ lưu lại snapshot của `spec.md`, `plan.md`, `tasks.md` tại thời điểm đó vào `.specify/memory/`.

**Khi nào chạy:**
- Sau khi `tasks.md` hoàn chỉnh và đã tạo issues
- Trước khi sprint bắt đầu
- Trước khi nhận requirement mới từ khách

---

## `/speckit-analyze` — Phân tích impact khi có thay đổi

Chạy **sau khi** BA update `spec.md` và team sync về.

```
/speckit-analyze
```

Claude so sánh spec mới với baseline, output ra:

```
## Impact Analysis

### Thay đổi phát hiện
- spec.md: Thêm tính năng mua vé nhóm (tối đa 10 người)

### Tasks bị ảnh hưởng
- T002 POST /api/tickets/purchase → cần update (thêm group_size)
- T008 Data model booking_order → cần update (thêm group fields)

### Tasks không bị ảnh hưởng
- T001 Setup scaffold → không đổi
- T015 Kafka topics → không đổi
...

### Tasks mới cần tạo
- POST /api/tickets/purchase-group (endpoint mới)
- Validate group size logic
- Split payment cho group
```

---

## Flow hoàn chỉnh khi requirement thay đổi

```
Sprint 1 bắt đầu
      ↓
/speckit-baseline    ← Tech Lead chạy 1 lần, lưu baseline
      ↓
... team implement ...
      ↓
Khách update requirement
      ↓
BA: /speckit-clarify → commit → push
      ↓
Tech Lead: sync spec về
      ↓
/speckit-analyze     ← so sánh với baseline, ra impact report
      ↓
      ├── Tasks bị ảnh hưởng
      │   → update GitHub Issues tương ứng thủ công
      │
      └── Tasks mới
          → /speckit-tasks (chỉ gen tasks mới)
          → /speckit-taskstoissues
      ↓
/speckit-baseline    ← chạy lại để lưu baseline mới
```

---

## Thực tế dùng như thế nào

| Tình huống | Lệnh |
|---|---|
| Trước sprint | `/speckit-baseline` |
| Khách đổi nhỏ (1-2 field) | `/speckit-analyze` → update issues thủ công |
| Khách đổi lớn (thêm feature) | `/speckit-analyze` → `/speckit-tasks` → `/speckit-taskstoissues` |
| Khách đổi rất lớn (đổi hướng) | Quay lại `/speckit-clarify` → làm lại từ `plan` |

---

Về cơ bản **`baseline` + `analyze`** là cặp đôi giúp bạn **không bao giờ bị mất kiểm soát** khi requirement thay đổi giữa chừng — biết chính xác cần làm gì thay vì đoán mò.

Bạn muốn hỏi thêm về skill nào khác trong danh sách không? Mình thấy còn có `speckit-checklist`, `speckit-constitution`, `speckit-implement`...
