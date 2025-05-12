Trước tiên ta cần biết về hai file trong hệ thống là `/etc/passwd` và `/etc/shadow`. Hai file này sẽ có liên quan đến quá trình tạo và quản lý user của chúng ta.
### 1. File `/etc/passwd`
```bash
/etc/passwd
```
Đây là tập tin chứa thông tin về tất cả người dùng trên hệ thống. Dưới đây là ví dụ thực tế về nội dung trong file `/etc/passwd` trên một hệ thống Linux thông thường:
```ruby
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
john:x:1001:1001:John Doe:/home/john:/bin/bash
```

Cùng xem xét dòng:

```ruby
john:x:1001:1001:John Doe:/home/john:/bin/bash
```

| Trường       | Ý nghĩa                          |
| ------------ | -------------------------------- |
| `john`       | Username                         |
| `x`          | Mật khẩu nằm trong `/etc/shadow` |
| `1001`       | UID (user ID)                    |
| `1001`       | GID (group ID)                   |
| `John Doe`   | Mô tả (full name)                |
| `/home/john` | Home directory                   |
| `/bin/bash`  | Shell đăng nhập (bash shell)     |

==note==: user id < 1000 được xem là system account

### 3. Tạo User trên Linux
#### 3.1. Sử dụng Lệnh useradd

Lệnh useradd là công cụ cấp thấp để tạo người dùng mới. Tuy nhiên, nó yêu cầu cấu hình thủ công nhiều bước.

**Cú pháp cơ bản**:
```bash
sudo useradd [tùy chọn] tên_user
```

**Các cờ quan trọng**:

- `-m`: Tạo thư mục home cho user (thường là /home/tên_user). Nếu không có cờ này, thư mục home sẽ không được tạo. Nếu có cờ này thì `/home/` sẽ xuất hiện dir có tên cùng với `user_name`
- `-s`: Chỉ định shell mặc định (ví dụ: /bin/bash, /bin/zsh). Nếu không chỉ định, hệ thống có thể gán shell mặc định hoặc /bin/sh.
- `-d`: Chỉ định đường dẫn thư mục home tùy chỉnh (ví dụ: -d /custom/home/tên_user).
- `-g`: Chỉ định nhóm chính (primary group) của user (theo tên nhóm hoặc GID).
- `-G`: Thêm user vào các nhóm bổ sung (secondary groups), cách nhau bằng dấu phẩy.
- `-u`: Chỉ định UID (User ID) cụ thể. Nếu không, hệ thống tự chọn UID tiếp theo.
- `-c`: Thêm mô tả hoặc bình luận cho user (thường là tên đầy đủ).

**Ví dụ**:
```bash
sudo useradd -m -s /bin/bash -c "John Doe" -u 1001 john
```
- Tạo user john với:
    - Thư mục home tại `/home/john`.
    - Shell là `/bin/bash`.
    - Mô tả là "John Doe".
    - UID là 1001.

**Lưu ý**:
- Sau khi tạo user bằng useradd, bạn cần thiết lập mật khẩu:
```bash
 sudo passwd john
```
- Nếu không dùng `-m`, bạn phải tạo thư mục home thủ công:
```bash
sudo mkdir /home/john
sudo chown john:john /home/john
sudo chmod 700 /home/john
```
#### 3.2. Sử dụng Lệnh adduser

Lệnh adduser là phiên bản thân thiện hơn, tự động hóa nhiều bước như tạo thư mục home, thiết lập mật khẩu, và cấu hình cơ bản.

**Cú pháp**:
```bash
sudo adduser tên_user
```

**Quy trình**:
1. Hệ thống yêu cầu nhập mật khẩu và xác nhận.
2. Yêu cầu nhập thông tin bổ sung (tên đầy đủ, số điện thoại, v.v.), có thể bỏ qua bằng cách nhấn Enter.
3. Tự động tạo thư mục home, sao chép tệp cấu hình từ `/etc/skel`, và thiết lập quyền.


**Các cờ quan trọng**:
- --home: Chỉ định thư mục home tùy chỉnh.
- --shell: Chỉ định shell mặc định.
- --uid: Chỉ định UID.
- --gecos: Đặt thông tin mô tả (như tên đầy đủ).


**Ví dụ**:

```bash
sudo adduser --home /custom/home/john --shell /bin/zsh john
```
- Tạo user john với thư mục home tại /custom/home/john và shell là /bin/zsh.

**Lưu ý**:

- adduser thường là lựa chọn tốt hơn cho người mới vì ít phải cấu hình thủ công.
- Kiểm tra user vừa tạo trong /etc/passwd:
    ```bash
    grep john /etc/passwd
    ```

### 4. Xóa User trên Linux

#### 4.1. Sử dụng Lệnh userdel

Lệnh userdel được sử dụng để xóa user khỏi hệ thống.
**Cú pháp**:
```bash
sudo userdel [tùy chọn] tên_user
```

**Các cờ quan trọng**:

- `-r`: Xóa cả thư mục home và các tệp liên quan của user.
- `-f`: Buộc xóa user, ngay cả khi user đang đăng nhập (cẩn thận khi dùng).


**Ví dụ**:
```bash
sudo userdel -r john
```
- Xóa user john và thư mục home `/home/john`.


**Lưu ý**:

- Nếu không dùng `-r`, thư mục home và tệp của user vẫn còn, cần xóa thủ công:
```bash
sudo rm -rf /home/john
```

- Kiểm tra xem user đã xóa chưa:
```bash
   grep john /etc/passwd
  ```

- Nếu user đang đăng nhập, bạn có thể cần ngắt kết nối:
```bash
 sudo pkill -u john
 ```

#### 4.2. Sử dụng Lệnh deluser

Lệnh deluser (thường có trên hệ Debian/Ubuntu) là cách thân thiện hơn để xóa user.

**Cú pháp**:
```bash
sudo deluser [tùy chọn] tên_user
```

**Các cờ quan trọng**:

- `--remove-home`: Xóa thư mục home.
- `--remove-all-files`: Xóa tất cả tệp thuộc sở hữu của user trên hệ thống.
- `--backup`: Sao lưu thư mục home trước khi xóa.


**Ví dụ**:
```bash
sudo deluser --remove-home --backup john
```
- Xóa user john, thư mục home, và tạo bản sao lưu tại `/home/john.tar.gz`.


**Lưu ý**:
- Sao lưu là cần thiết nếu bạn muốn giữ dữ liệu trước khi xóa.
- Kiểm tra tệp nhật ký `/var/log` để xác nhận xóa thành công.
