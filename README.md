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
