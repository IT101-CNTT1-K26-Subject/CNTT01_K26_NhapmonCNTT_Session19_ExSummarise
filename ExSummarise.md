# PHÂN TÍCH & RA QUYẾT ĐỊNH TRIỂN KHAI XÁC THỰC 2FA CHO PIXELFORGE STUDIO

### **1. Xử lý sự cố & Nhận diện rủi ro**

* **Tình huống 1 (Link lạ):** Là **Phishing / Typosquatting** (giả mạo tên miền như `githuh-security.com`).


* *Cách xử lý:* Áp dụng Quy tắc 5 KHÔNG — **Không bấm vào link khẩn cấp** từ nguồn lạ.




* **Tình huống 2 (Tắt Antivirus tải file crack):** Là **Cạm bẫy phần mềm crack**. Tắt Windows Defender sẽ mở cửa cho mã độc tàng hình (Infostealer) xâm nhập.


* *Cách xử lý:* Áp dụng Quy tắc 5 KHÔNG — **Không tải phần mềm crack** hay tắt trình bảo vệ.




* **Lỡ chạy file độc hại (Quy trình 4 bước):**
1. **Ngắt mạng (Isolate):** Rút Wi-Fi/LAN ngay lập tức, tắt máy.


2. **Thu hồi (Revoke):** Dùng thiết bị khác đăng xuất toàn bộ tài khoản và token.


3. **Đổi mật khẩu & 2FA:** Đổi toàn bộ pass qua Password Manager và bật Authenticator.


4. **Cài lại máy (Clean Install):** Format ổ đĩa cài lại hệ điều hành để sạch triệt để.





---

### **2. Chuyển sang Password Manager**

* **Nguy hiểm khi dùng chung 1 mật khẩu:** Nếu lộ email, hacker sẽ dùng **Credential Stuffing** (bot tự động) để chiếm luôn GitHub và Vercel của nhóm.
* **Vì sao Bitwarden an toàn (Zero-Knowledge):** Mật khẩu được mã hóa (AES-256) ngay trên máy bạn trước khi đẩy lên cloud, server không thể đọc được.


* **Tạo mật khẩu mới:** Bật đầy đủ độ dài trên 20 ký tự, gồm Chữ hoa, Chữ thường, Số và Ký tự đặc biệt.



---

### **3. Kích hoạt 2FA GitHub**

* **Tại sao không dùng SMS OTP:** Dễ dính tấn công **SIM Swapping** (tráo SIM chiếm số điện thoại) và mất sóng khi ở nước ngoài. Hãy dùng **Authenticator App (TOTP)**.


* **Cách bật:** Vào GitHub `Settings` > `Account security` > Bật 2FA > Dùng Google Authenticator quét mã QR > Nhập mã 6 số xác nhận.


* **Mã khôi phục (Backup Codes):** Lưu vào Bitwarden Secure Note hoặc in ra giấy. **Tuyệt đối không** chụp ảnh màn hình lưu vào thư viện ảnh điện thoại.



---

### **4. Bảo vệ Secrets trước khi Push Code**

* **Sai lầm:** Hardcode trực tiếp API Key và Password vào file code (`config.js`).


* **Cách sửa (Dùng `process.env`):**
```javascript
require('dotenv').config();
const API_KEY = process.env.API_KEY;
const DB_PASSWORD = process.env.DB_PASSWORD;

```


* **Ba file cần thiết:**
1. `.env`: Chứa giá trị thật (Giữ kín trên máy cục bộ).
2. `.gitignore`: Khai báo dòng `.env` để Git bỏ qua, không đẩy lên public.
3. `.env.example`: File mẫu trống giá trị để hướng dẫn nhóm (`API_KEY=your_key_here`).