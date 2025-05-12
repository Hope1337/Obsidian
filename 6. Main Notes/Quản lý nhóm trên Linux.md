Phân quyền trong Linux được quản lý thông qua **quyền truy cập tệp** (permissions) và **nhóm** (groups). Các lệnh chính bao gồm chmod, chown, và usermod. Lý do tồn tại của **nhóm** là để tiện cho việc cấp quyền, ví dụ bạn cấp cho một user với một số quyền mặc định và nếu có nhiều user khác cũng cần được cấp các quyền tương tự như vậy thì bạn hoàn toàn có thể đưa user đó vào nhóm, dưới một nhóm thì họ có quyền như nhau (trừ khi được specify cụ thể hơn)

==Note==: mỗi file trong linux chỉ được phép gán cho một nhóm.

## 1. Tạo nhóm

Để tạo một nhóm mới, sử dụng lệnh groupadd. Cú pháp cơ bản:
```bash
sudo groupadd tên_nhóm
```

**Ví dụ:**
```bash
sudo groupadd developers
```

Để tạo nhóm với ID cụ thể (GID), thêm tùy chọn `-g`:
```bash
sudo groupadd -g 1001 testers
```

**Kiểm tra nhóm đã tạo:** Sử dụng lệnh getent để xem thông tin nhóm:
```bash
getent group developers
```
Kết quả mẫu: `developers:x:1000`:

## 2. Xóa nhóm

Để xóa một nhóm, sử dụng lệnh groupdel:
```bash
sudo groupdel tên_nhóm
```
**Ví dụ:**
```bash
sudo groupdel testers
```
**Lưu ý:** Không thể xóa nhóm nếu vẫn còn người dùng thuộc nhóm đó với tư cách là nhóm chính (primary group). Hãy kiểm tra và thay đổi nhóm chính của người dùng trước khi xóa.

## 3. Thêm người dùng vào nhóm

Để thêm người dùng vào một nhóm, sử dụng lệnh usermod với tùy chọn `-aG`:
```bash
sudo usermod -aG tên_nhóm tên_người_dùng
```
- `-a`: Thêm vào nhóm mà không xóa người dùng khỏi các nhóm hiện tại.
- `-G`: Chỉ định nhóm cần thêm.

## 4. Xóa người dùng ra khỏi nhóm

```bash
sudo deluser <username> <groupname>
# or
sudo gpasswd -d <username> <groupname>
```
## 4. Quản lý phân quyền cho nhóm

Phân quyền trên Linux được quản lý thông qua hệ thống quyền truy cập (permissions) với ba loại: đọc (`r`), ghi (`w`), và thực thi (`x`). Nhóm sở hữu tệp/thư mục có thể được gán các quyền cụ thể.

### 4.1 Gán nhóm sở hữu cho tệp/thư mục

Sử dụng lệnh `chgrp` để thay đổi nhóm sở hữu:
```bash
sudo chgrp tên_nhóm đường_dẫn
```
**Ví dụ:**
```bash
sudo chgrp developers /var/www/project
```
Để áp dụng đệ quy (cho cả thư mục và các tệp con):
```bash
sudo chgrp -R developers /var/www/project
```

### 4.2. Thiết lập quyền truy cập

Sử dụng lệnh `chmod` để gán quyền cho nhóm. Quyền được biểu diễn bằng số (octal) hoặc ký tự.

- **Dùng số octal:**
    - 4: Đọc (`r`)
    - 2: Ghi (`w`)
    - 1: Thực thi (`x`)

Ví dụ: Quyền `rw-` cho nhóm là 6 (4+2).
```bash
sudo chmod g=rw đường_dẫn
```
**Ví dụ:**
```bash
sudo chmod -R g=rw /var/www/project
```
- **Dùng ký tự:**
```bash
sudo chmod g+rw đường_dẫn
```
**Kiểm tra quyền:**
```bash
ls -l /var/www/project
```
Kết quả mẫu: `-rw-rw-r-- 1 user developers 0 Apr 30 2025 project`
**Note**: 
- `user` là tên chủ sở hữu của file
- `developers` là tên nhóm mà file thuộc về

### 4.3. Quyền mặc định cho tệp/thư mục mới

Để đảm bảo các tệp/thư mục mới trong một thư mục kế thừa nhóm sở hữu, sử dụng tùy chọn `setgid`:
```bash
sudo chmod g+s /var/www/project
```
Kiểm tra: Thư mục sẽ hiển thị s trong quyền nhóm:
```bash
ls -ld /var/www/project
```
Kết quả mẫu: `drwxrwsr-x 2 user developers 4096 Apr 30 2025 project`

Ngoài ra, sử dụng umask để kiểm soát quyền mặc định cho các tệp/thư mục mới. Ví dụ, đặt umask 002 để nhóm có quyền đọc/ghi:
```bash
umask 002
```

## 5. Quản lý nhóm chính (Primary Group)

Mỗi người dùng có một nhóm chính, được gán khi tạo tài khoản. Để thay đổi nhóm chính:
```bash
sudo usermod -g tên_nhóm tên_người_dùng
```
**Ví dụ:**
```bash
sudo usermod -g developers alice
```
==Note==:
Trong Linux, **nhóm chính** (primary group) là nhóm mặc định được gán cho một người dùng khi họ được tạo trên hệ thống. Nhóm chính đóng vai trò quan trọng trong việc quản lý quyền truy cập và xác định quyền sở hữu tệp/thư mục mà người dùng tạo ra:
- Mỗi người dùng trong Linux phải thuộc ít nhất một nhóm chính.
- Nhóm chính được lưu trong tệp `/etc/passwd`, ở trường GID (Group ID) tương ứng với người dùng.
- Khi người dùng tạo một tệp hoặc thư mục mới, tệp/thư mục đó sẽ tự động thuộc về **nhóm chính** của người dùng (trừ khi có thiết lập đặc biệt như setgid).

