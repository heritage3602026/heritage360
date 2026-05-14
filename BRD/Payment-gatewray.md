Đúng rồi! Cả 2 API đó phải gọi từ **backend của bạn**, không phải từ app.

---

## Tại sao phải từ Backend?

```
❌ Sai: App → gọi thẳng API OnePay
✅ Đúng: App → Backend của bạn → API OnePay
```

Vì:
- API OnePay thường có **secret key / signature** → không thể để lộ ở client
- `check-payment` cần verify param → phải làm server-side để tránh bị giả mạo

---

## Flow chuẩn

```
[Mobile App]                [Backend bạn]           [OnePay]

Bấm "Thanh toán"
      │
      ▼
POST /your-api/init-payment ──► create-payment ──────► trả về payment_url
                            ◄── trả payment_url ◄──────
      │
      ▼
Mở payment_url (browser/webview)
                                                   User thanh toán
                                                        │
                                          redirect về return_url?param=xxx
      │
      ▼ (deep link hoặc webview detect URL)
Bắt được param
      │
      ▼
POST /your-api/verify-payment
      (gửi param lên backend)   ──► check-payment ────► status
                            ◄── trả kết quả ◄──────────
      │
      ▼
Hiện màn thành công/thất bại
```

---

## Return URL nên là gì?

Đây là điểm quan trọng — `return_url` bạn gửi lên OnePay nên là **URL của backend bạn**, không phải deep link app:

```
return_url = "https://your-backend.com/payment/callback"
```

Rồi backend nhận callback → xử lý → sau đó **redirect hoặc push notification** về app:

```
[OnePay] → redirect → your-backend.com/payment/callback?param=xxx
                              │
                       verify với check-payment API
                              │
                    ┌─────────┴──────────┐
                    ▼                    ▼
             Lưu DB = PAID        Push notification
                                  hoặc redirect về
                                  yourapp://payment/success
```

---

## Tóm lại phân chia rõ ràng

| Bên | Làm gì |
|---|---|
| **Mobile App** | Mở payment URL, bắt deep link, hiện kết quả |
| **Backend bạn** | Gọi create-payment, nhận callback, gọi check-payment, lưu DB |
| **OnePay** | Xử lý thanh toán, redirect về return_url |

Backend bạn đang dùng tech stack gì? Node, Java, hay gì khác?
