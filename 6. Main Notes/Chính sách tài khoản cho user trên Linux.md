## 1. Cách Thiết Lập (Setting) Chính Sách Hết Hạn Mật Khẩu

Để thiết lập các chính sách liên quan đến hết hạn mật khẩu và tài khoản, bạn có thể sử dụng lệnh **chage** hoặc chỉnh sửa trực tiếp tệp `/etc/shadow` (**không khuyến khích trừ khi cần thiết**). Dưới đây là cách thực hiện:

### 1.1. Sử dụng Lệnh chage

Lệnh `chage` (change aging) là công cụ chính để quản lý các chính sách hết hạn mật khẩu và tài khoản. Nó cho phép thiết lập các trường liên quan trong `/etc/shadow`.

**Cú pháp**:
```bash
sudo chage [tùy chọn] tên_user
```

**Các cờ quan trọng**:
- `-m` (minimum): Đặt số ngày tối thiểu trước khi đổi mật khẩu.
- `-M `(maximum): Đặt số ngày tối đa trước khi mật khẩu hết hạn.
- `-W` (warning): Đặt số ngày cảnh báo trước khi mật khẩu hết hạn.
- `-I` (inactive): Đặt số ngày vô hiệu hóa sau khi mật khẩu hết hạn.
- `-E` (expiration): Đặt ngày hết hạn tài khoản (định dạng YYYY-MM-DD hoặc số ngày kể từ 01/01/1970).
- `-d` (last day): Đặt ngày thay đổi mật khẩu cuối cùng (định dạng YYYY-MM-DD hoặc số ngày).
- `-l`: Hiển thị thông tin chính sách hết hạn của user.

**Ví dụ**:
1. **Thiết lập mật khẩu hết hạn sau 90 ngày, cảnh báo 7 ngày trước**:
```bash
  sudo chage -M 90 -W 7 john
   ```
- Mật khẩu của john sẽ hết hạn sau 90 ngày.
- Hệ thống cảnh báo 7 ngày trước khi hết hạn.

2. **Đặt thời gian tối thiểu 5 ngày trước khi đổi mật khẩu**:
````bash
sudo chage -m 5 john	
````
- User john phải đợi 5 ngày trước khi được phép đổi mật khẩu.

3. **Đặt tài khoản hết hạn vào ngày 31/12/2025**:
```bash
sudo chage -E 2025-12-31 john
```
- Tài khoản john sẽ bị vô hiệu hóa sau ngày 31/12/2025.

4. **Đặt thời gian vô hiệu hóa 10 ngày sau khi mật khẩu hết hạn**:
```bash
sudo chage -I 10 john
```
- Nếu mật khẩu hết hạn, john có 10 ngày để đăng nhập và đổi mật khẩu trước khi tài khoản bị khóa.

5. **Kiểm tra chính sách của user**:
```bash
sudo chage -l john
```
Kết quả ví dụ:
`Last password change : Dec 31, 2022 Password expires : Mar 31, 2023 Password inactive : Apr 10, 2023 Account expires : Dec 31, 2025 Minimum number of days between password change : 5 Maximum number of days between password change : 90 Number of days of warning before password expires : 7`

### 1.2. Chỉnh Sửa Trực Tiếp /etc/shadow

Đây là lưu file lại mật khẩu (dưới dạng hash) và một số thông tin khác về user account đó.
```ruby
root:$6$asd...DfgE$:19000:0:99999:7:::
john:$6$Kjl...xZp/:19000:0:99999:7:::
```
Cấu trúc của nó là:
```ruby
username:password:last_change:min:max:warn:inactive:expire:reserved
```

| Trường số | Tên           | Ý nghĩa chi tiết                                                                 |
| --------- | ------------- | -------------------------------------------------------------------------------- |
| 1️⃣       | `username`    | Tên đăng nhập của user                                                           |
| 2️⃣       | `password`    | Mật khẩu đã mã hóa (hash). Nếu là `!` hoặc `*` thì user bị khóa                  |
| 3️⃣       | `last_change` | Ngày thay đổi mật khẩu gần nhất (tính từ **01/01/1970**)                         |
| 4️⃣       | `min`         | Số ngày tối thiểu trước khi cho phép đổi mật khẩu tiếp theo                      |
| 5️⃣       | `max`         | Số ngày tối đa bắt buộc phải đổi mật khẩu                                        |
| 6️⃣       | `warn`        | Số ngày trước khi hết hạn mật khẩu thì cảnh báo người dùng                       |
| 7️⃣       | `inactive`    | Số ngày sau khi mật khẩu hết hạn thì tài khoản bị vô hiệu hóa                    |
| 8️⃣       | `expire`      | Ngày tài khoản hết hạn (cũng tính từ 01/01/1970)                                 |
| 9️⃣       | `reserved`    | Chưa sử dụng (linux dự phòng trường này cho mục đích phát triển trong tương lai) |


==**Cảnh báo**==: Chỉ chỉnh sửa /etc/shadow nếu bạn hiểu rõ cấu trúc và đã sao lưu tệp. Sử dụng trình chỉnh sửa như `vipw -s` để đảm bảo an toàn.
==note==: `sudo vipw -s`sẽ mở  `/etc/shadow` lên cho bạn chỉnh sửa và kiểm tra sau khi hoàn thành.

**Ví dụ**: Để đặt mật khẩu hết hạn sau `90` ngày và tài khoản hết hạn vào `31/12/2025`:

1. Mở `/etc/shadow`:
```bash
sudo vipw -s
```
2. Tìm dòng của user john, ví dụ: `john:$6$abc...xyz:19345:0:99999:7:::`
3. Sửa thành: `john:$6$abc...xyz:19345:0:90:7::21185:`
- **90**: Mật khẩu hết hạn sau 90 ngày.
- **21185**: Tài khoản hết hạn vào ngày 31/12/2025 (tức là số ngày tính từ 01/01/1970).

**Lưu ý**:

- Sao lưu /etc/shadow trước khi chỉnh sửa:
```bash
sudo cp /etc/shadow /etc/shadow.bak
```
- Sử dụng `pwck` để kiểm tra tính toàn vẹn sau khi chỉnh sửa:
```bash
sudo pwck
```

### 1.3. Thiết Lập Chính Sách Toàn Hệ Thống

Để áp dụng chính sách mật khẩu mặc định cho tất cả user mới, chỉnh sửa tệp `/etc/login.defs`:

**Các tham số liên quan**:
- `PASS_MAX_DAYS`: Số ngày tối đa trước khi mật khẩu hết hạn.
- `PASS_MIN_DAYS`: Số ngày tối thiểu trước khi đổi mật khẩu.
- `PASS_WARN_AGE`: Số ngày cảnh báo trước khi mật khẩu hết hạn.
**Ví dụ**: Mở `/etc/login.defs`:
```bash
sudo nano /etc/login.defs
```
Sửa các dòng: `PASS_MAX_DAYS 90 PASS_MIN_DAYS 5 PASS_WARN_AGE 7`
Lưu và thoát. Chính sách này áp dụng cho user mới được tạo sau khi sửa.

**Lưu ý**:
- Các user hiện có không bị ảnh hưởng bởi thay đổi trong `/etc/login.defs`. Dùng chage để cập nhật, tức là bạn phải cập nhật thủ công (hoặc viết bash script để tự động cập nhật).

