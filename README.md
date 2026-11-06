# Thư viện PDF trên Google Drive

Tải PDF (không giới hạn 50 MB), đọc trên điện thoại hoặc máy tính, highlight và chèn chữ, đồng bộ qua Google Drive của bạn.

Cần làm 3 phần: **A. Google Cloud** → **B. GitHub** → **C. Hoàn tất**. Tổng khoảng 15 phút, hoàn toàn miễn phí.

---

## PHẦN A. Tạo Client ID trên Google Cloud

### Bước A1. Tạo project
1. Mở https://console.cloud.google.com và đăng nhập tài khoản Google bạn muốn lưu sách.
2. Bấm vào ô chọn project ở góc trên bên trái, chọn **New Project**.
3. Đặt tên (ví dụ `pdf-library`), bấm **Create**, rồi chọn project vừa tạo.

### Bước A2. Bật Google Drive API
1. Menu ☰ → **APIs & Services** → **Library**.
2. Gõ `Google Drive API`, bấm vào kết quả, bấm **Enable**.

### Bước A3. Cấu hình màn hình đồng ý (OAuth consent screen)
1. **APIs & Services** → **OAuth consent screen** (hoặc mục **Google Auth Platform** → **Branding** nếu giao diện mới).
2. Bấm **Get started**, điền:
   - App name: `PDF Library`
   - User support email: email của bạn
   - Audience: chọn **External**
   - Contact email: email của bạn
3. Đồng ý điều khoản, bấm **Create**.
4. Vào mục **Data Access** (hoặc **Scopes**) → **Add or remove scopes**, tìm và tick `.../auth/drive.file`, bấm **Update** rồi **Save**.
5. Vào mục **Audience**:
   - Bấm **Publish app** để chuyển sang **In production** (khuyên dùng, đăng nhập không bị hết hạn sau 7 ngày). Khi đăng nhập sẽ thấy cảnh báo "Google hasn't verified this app": bấm **Advanced** → **Go to PDF Library (unsafe)**. Đây là app của chính bạn nên an toàn.
   - Hoặc để ở chế độ **Testing** và thêm email của bạn vào **Test users** (đăng nhập sẽ bị yêu cầu lại sau 7 ngày).

### Bước A4. Tạo OAuth Client ID
1. **APIs & Services** → **Credentials** (hoặc **Clients**) → **Create credentials** → **OAuth client ID**.
2. Application type: **Web application**. Name: `pdf-web`.
3. Mục **Authorized JavaScript origins** → **Add URI**, điền địa chỉ GitHub Pages của bạn, **chỉ phần gốc, không có dấu `/` ở cuối và không có tên repo**:
   ```
   https://TEN-GITHUB-CUA-BAN.github.io
   ```
   (Làm xong phần B bạn sẽ biết chính xác tên. Nếu chưa chắc, cứ điền theo mẫu trên và sửa lại sau.)
4. Để trống mục Redirect URIs, bấm **Create**.
5. Copy **Client ID** (dạng `123456-abc.apps.googleusercontent.com`). Không cần Client Secret.

---

## PHẦN B. Đưa lên GitHub

### Bước B1. Sửa Client ID
Mở `index.html` bằng Notepad hoặc VS Code, tìm dòng:
```js
const CLIENT_ID='YOUR-CLIENT-ID.apps.googleusercontent.com';
```
Thay bằng Client ID vừa copy, lưu file.

### Bước B2. Tạo repo và upload
1. Vào https://github.com/new, đặt tên repo (ví dụ `pdf-library`), chọn **Public**, bấm **Create repository**.
2. Bấm **uploading an existing file**, kéo `index.html` và `README.md` vào, bấm **Commit changes**.

### Bước B3. Bật GitHub Pages
1. Repo → **Settings** → **Pages**.
2. Source: **Deploy from a branch**; Branch: **main**, thư mục **/ (root)**, bấm **Save**.
3. Đợi 1-2 phút, trang sẽ hiện link `https://TEN-GITHUB.github.io/pdf-library/`.

---

## PHẦN C. Hoàn tất và sử dụng

1. Kiểm tra lại **Authorized JavaScript origins** ở bước A4 khớp với `https://TEN-GITHUB.github.io` (phần gốc, không kèm `/pdf-library`).
2. Mở link GitHub Pages trên máy tính → **Đăng nhập Google** → chấp nhận quyền.
3. Bấm **Tải PDF lên**. App tự tạo thư mục `PDF Library` trong Drive.
4. Trên điện thoại: mở cùng link → đăng nhập cùng tài khoản → thấy sách. Menu trình duyệt → **Thêm vào Màn hình chính** để dùng như app.

### Thanh công cụ khi đọc
- **Xem**: cuộn bình thường.
- **Highlight**: kéo ngón tay/chuột để tô vùng.
- **Chèn chữ**: chạm vào vị trí, nhập nội dung.
- **Xóa**: chạm vào highlight hoặc chữ cần xóa.
- Ghi chú tự lưu sau 2 giây (xem chữ "Đã lưu ✓").

---

## Lỗi thường gặp

| Lỗi | Cách xử lý |
|---|---|
| `redirect_uri_mismatch` / `origin_mismatch` | Origin ở bước A4 sai. Phải đúng dạng `https://TEN.github.io`, không có `/` cuối, không có tên repo. Sửa xong đợi vài phút. |
| `Access blocked: app not verified` | Ở chế độ Testing, thêm email của bạn vào **Test users** (A3.5). |
| Không thấy file mình tự bỏ vào thư mục Drive | Quyền `drive.file` chỉ cho app thấy file do **chính app tải lên**. Hãy tải sách bằng nút **Tải PDF lên** trong app. |
| Cửa sổ đăng nhập không hiện trên điện thoại | Cho phép popup cho trang này, hoặc bấm lại **Đăng nhập Google**. |
| Sau một thời gian phải đăng nhập lại | Bình thường. Phiên Google chỉ kéo dài khoảng 1 giờ, app tự gia hạn nếu trình duyệt cho phép. |
| Sách lớn mở lần đầu chậm | Lần đầu phải tải về máy. Các lần sau mở từ bộ nhớ trình duyệt nên rất nhanh. |

## Ghi chú
- File PDF gốc không bị thay đổi. Ghi chú lưu riêng thành file `<mã>.ann.json` trong cùng thư mục Drive (đừng xóa file này nếu muốn giữ ghi chú).
- Xóa sách trong app sẽ xóa file PDF trên Drive (vào thùng rác Drive).
