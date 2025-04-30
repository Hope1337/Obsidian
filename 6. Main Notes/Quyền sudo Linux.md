Nhóm `sudo` là một **nhóm đặc biệt** trên các hệ điều hành Linux (đặc biệt là Debian-based như Ubuntu). Thành viên trong nhóm này có **quyền thực hiện các lệnh với quyền quản trị (root)** bằng cách thêm `sudo` trước lệnh.

Kiểm tra ai thuộc nhóm `sudo`:
```bash
getent group sudo

# or
groups <username>
```

#### 1. Thêm user vào nhóm sudo:
```bash
sudo usermod -aG sudo username
```

#### 2. Xóa một user khỏi nhóm sudo:
```bash
sudo deluser <username> sudo
# or
sudo gpasswd -d <username> sudo
```


==Nếu lỡ xóa chính mình ra khỏi nhóm sudo thì làm sao?==

Nếu bạn vô tình xóa chính mình khỏi nhóm `sudo`, bạn sẽ mất quyền chạy lệnh với tư cách admin. Cách khắc phục phụ thuộc vào tình huống:

==**Nếu bạn vẫn có quyền truy cập root** (qua `su` hoặc tài khoản root trực tiếp)==:
  - Đăng nhập vào root:
```bash    
su -    
```
  - Thêm lại mình vào nhóm `sudo`:
```bash    
usermod -aG sudo <username>    
```
  - Kiểm tra:
```bash
   groups <username>    
```

==**Nếu hệ thống có user khác trong nhóm sudo**==:
  - Yêu cầu họ chạy lệnh để thêm bạn lại vào nhóm `sudo`:
    ```bash
    sudo usermod -aG sudo <username>
    ```
	
==**Nếu bạn không có quyền root và không có user nào khác trong nhóm sudo**==:
  - **Khởi động vào chế độ Recovery Mode**:
    1. Khởi động lại máy và vào menu GRUB (thường nhấn `Shift` hoặc `Esc` khi khởi động).
    2. Chọn tùy chọn "Recovery Mode" hoặc "Advanced Options" và chọn kernel với chế độ recovery.
    3. Trong menu recovery, chọn `root` để vào shell root.
    4. Remount filesystem ở chế độ read-write:
       ```bash
       mount -o remount,rw /
       ```
    5. Thêm lại user vào nhóm `sudo`:
       ```bash
       usermod -aG sudo <username>
       ```
    6. Đồng bộ dữ liệu và khởi động lại:
       ```bash
       sync
       reboot
       ```

==**Nếu không có cách nào truy cập được**==:
  - Sử dụng đĩa cài đặt Linux (Live CD/USB) để khởi động, mount phân vùng gốc, và chỉnh sửa file `/etc/group` để thêm user vào nhóm `sudo`.
