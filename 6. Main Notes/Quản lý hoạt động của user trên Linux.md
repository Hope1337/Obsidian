
### 1. Cách lock và unlock một user

**Lock một user** (khóa tài khoản người dùng để ngăn đăng nhập):
- Khi một tài khoản bị khóa, người dùng không thể đăng nhập vào hệ thống, nhưng dữ liệu và cấu hình của họ vẫn được giữ nguyên.
- Sử dụng lệnh `passwd` hoặc `usermod` để khóa tài khoản:
  ```bash
  sudo passwd -l <username>
  ```
  - Lệnh này thêm một ký tự `!` vào trước mật khẩu trong file `/etc/shadow`, làm cho tài khoản không thể đăng nhập.
  - Thay `<username>` bằng tên người dùng cần khóa.

  Hoặc sử dụng `usermod`:
  ```bash
  sudo usermod -L <username>
  ```
  - Tác dụng tương tự như `passwd -l`.

- **Kiểm tra trạng thái khóa**:
  ```bash
  sudo passwd -S <username>
  ```
  - Nếu tài khoản bị khóa, trạng thái sẽ hiển thị `L` (locked).

**Unlock một user** (mở khóa tài khoản để cho phép đăng nhập lại):
- Sử dụng lệnh `passwd` hoặc `usermod` để mở khóa:
  ```bash
  sudo passwd -u <username>
  ```
  - Lệnh này xóa ký tự `!` trong file `/etc/shadow`, khôi phục khả năng đăng nhập.

  Hoặc sử dụng `usermod`:
  ```bash
  sudo usermod -U <username>
  ```

- **Lưu ý**:
  - Nếu tài khoản đã bị khóa bằng cách đặt shell thành `/bin/false` hoặc `/sbin/nologin`, bạn cần khôi phục shell bằng:
    ```bash
    sudo usermod -s /bin/bash <username>
    ```
  - Kiểm tra file `/etc/shadow` để xác nhận trạng thái:
    ```bash
    sudo grep <username> /etc/shadow
    ```

---

### 2. Cách theo dõi các hoạt động của một user

Để theo dõi hoạt động của một người dùng trên Linux, bạn có thể sử dụng các công cụ và phương pháp sau:

#### a. **Theo dõi lịch sử lệnh (Command History)**
- Mỗi user có file lịch sử lệnh (thường là `~/.bash_history`) lưu các lệnh đã chạy trong terminal.
- Xem lịch sử lệnh của user:
  ```bash
  sudo cat /home/<username>/.bash_history
  ```
- **Lưu ý**:
  - File `.bash_history` chỉ được cập nhật khi user thoát phiên terminal, nên không theo dõi được thời gian thực.
  - User có thể xóa hoặc chỉnh sửa file này nếu có quyền.

- Để ghi lại thời gian thực hiện lệnh, thêm vào file `/etc/bash.bashrc` hoặc `~/.bashrc` của user:
  ```bash
  export HISTTIMEFORMAT="%d/%m/%y %T "
  ```
  - Sau đó, khi xem `.bash_history`, mỗi lệnh sẽ có dấu thời gian.

#### b. **Sử dụng công cụ `auditd` để giám sát**
- **Cài đặt `auditd`** (nếu chưa có):
  ```bash
  sudo apt install auditd
  ```
  (Trên CentOS/RHEL: `sudo yum install audit` hoặc `sudo dnf install audit`).

- **Cấu hình để theo dõi user**:
  - Thêm quy tắc để giám sát hoạt động của user dựa trên UID:
    ```bash
    sudo auditctl -a always,exit -F uid=<uid> -S execve
    ```
    - Thay `<uid>` bằng ID của user (tìm bằng `id <username>`).
    - Lệnh này theo dõi tất cả các lệnh được thực thi bởi user.

- **Xem log audit**:
  - Log được lưu trong `/var/log/audit/audit.log`. Xem log:
    ```bash
    sudo ausearch -u <username>
    ```
  - Để tìm kiếm theo thời gian thực:
    ```bash
    sudo tail -f /var/log/audit/audit.log
    ```

- **Lưu ý**: `auditd` cung cấp chi tiết về lệnh, thời gian, và ngữ cảnh, nhưng cần phân tích kỹ vì log có thể rất dài.

#### c. **Sử dụng `ps` và `htop` để theo dõi tiến trình**
- Xem các tiến trình mà user đang chạy:
  ```bash
  ps -u <username>
  ```
  - Hiển thị tất cả tiến trình của user.

- Theo dõi thời gian thực với `htop` (cần cài đặt trước: `sudo apt install htop`):
  ```bash
  htop -u <username>
  ```
  - Lọc tiến trình theo user.

#### d. **Theo dõi hoạt động file (File Access)**
- Sử dụng `auditd` để giám sát truy cập file:
  ```bash
  sudo auditctl -w /path/to/file -p warx -k file-watch
  ```
  - Theo dõi đọc (`r`), ghi (`w`), thực thi (`x`), hoặc thay đổi thuộc tính (`a`) trên file/thư mục.
  - Xem log:
    ```bash
    sudo ausearch -k file-watch
    ```

- Hoặc sử dụng `inotify-tools` để theo dõi thay đổi file trong thời gian thực:
  - Cài đặt:
    ```bash
    sudo apt install inotify-tools
    ```
  - Theo dõi thư mục:
    ```bash
    inotifywait -m /home/<username> -e modify -e create -e delete
    ```

#### e. **Theo dõi đăng nhập (Login Activity)**
- Kiểm tra lịch sử đăng nhập:
  ```bash
  last <username>
  ```
  - Hiển thị thời gian và nguồn đăng nhập của user.

- Theo dõi thời gian thực các phiên đăng nhập:
  ```bash
  sudo tail -f /var/log/auth.log
  ```
  (Trên CentOS: `/var/log/secure`).

#### f. **Sử dụng công cụ giám sát nâng cao**
- **Snoopy**: Ghi lại tất cả lệnh được thực thi bởi user.
  - Cài đặt:
    ```bash
    sudo apt install snoopy
    ```
  - Log được ghi vào `/var/log/auth.log` hoặc syslog. Xem:
    ```bash
    sudo grep snoopy /var/log/auth.log
    ```

- **OSSEC** hoặc **Fail2Ban**: Dùng để giám sát và phân tích hành vi bất thường của user.

#### g. **Lưu ý quan trọng**
- **Quyền riêng tư**: Theo dõi user phải tuân thủ luật pháp và chính sách nội bộ. Chỉ thực hiện trên hệ thống bạn có quyền quản lý.
- **Hiệu suất**: Các công cụ như `auditd` hoặc `inotify` có thể tiêu tốn tài nguyên nếu giám sát quá nhiều.
- **Bảo mật**: Đảm bảo user không có quyền chỉnh sửa log hoặc vô hiệu hóa giám sát (ví dụ: không thuộc nhóm `sudo`).
