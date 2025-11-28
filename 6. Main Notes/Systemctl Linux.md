## 📌 `systemctl` là gì?

- `systemctl` là **công cụ dòng lệnh** để quản lý **systemd**.
- `systemd` là **init system** (chương trình đầu tiên chạy khi Linux khởi động), chịu trách nhiệm:
    
    - Khởi chạy dịch vụ (services)
    - Quản lý tiến trình (process)
    - Mount filesystem
    - Quản lý log (`journalctl`)
    - …

---

## 📌 Cú pháp chung

```bash
systemctl [COMMAND] [SERVICE]
```

### Một số lệnh hay dùng:

|Lệnh|Ý nghĩa|
|---|---|
|`systemctl start <service>`|Chạy dịch vụ ngay|
|`systemctl stop <service>`|Dừng dịch vụ|
|`systemctl restart <service>`|Khởi động lại|
|`systemctl reload <service>`|Nạp lại config mà không restart|
|`systemctl enable <service>`|Cho phép chạy khi boot|
|`systemctl disable <service>`|Ngăn chạy khi boot|
|`systemctl status <service>`|Xem trạng thái dịch vụ|
|`systemctl is-enabled <service>`|Kiểm tra có bật auto-start không|
|`systemctl list-units --type=service`|Liệt kê tất cả service đang chạy|

---

## 📌 Ví dụ thực tế:

- Kiểm tra trạng thái SSH:

```bash
systemctl status ssh
```

- Khởi động lại network manager:

```bash
sudo systemctl restart NetworkManager
```

- Bật Docker chạy khi khởi động:

```bash
sudo systemctl enable docker
```

---

## 📌 `systemctl start` vs `systemctl enable`

### 1. `systemctl start <service>`

- Chạy dịch vụ **ngay lập tức**.
    
- Dịch vụ bắt đầu hoạt động cho đến khi bạn tắt hoặc restart máy.
    
- **Không ảnh hưởng** đến việc dịch vụ có chạy tự động khi boot lại hay không.
    

Ví dụ:

```bash
sudo systemctl start ssh
```

→ SSH server chạy ngay, nhưng reboot xong thì **chưa chắc chạy lại**.

---

### 2. `systemctl enable <service>`

- Không chạy ngay.
    
- Tạo symlink để dịch vụ **tự động chạy khi máy khởi động**.
    
- Có hiệu lực cho các lần reboot sau.
    

Ví dụ:

```bash
sudo systemctl enable ssh
```

→ SSH server sẽ khởi động mỗi lần boot, nhưng nếu hiện tại nó đang tắt thì vẫn tắt (chưa chạy ngay).

---

### 3. Kết hợp cả hai

Thường khi cài dịch vụ mới (ví dụ cài SSH server, Docker, Nginx…), ta muốn **chạy ngay và cả sau khi reboot** → dùng cả hai:

```bash
sudo systemctl start ssh
sudo systemctl enable ssh
```

---

### 4. Bổ sung: `systemctl enable --now`

- Để gộp 2 thao tác vào một lệnh duy nhất:
    

```bash
sudo systemctl enable --now ssh
```

→ vừa enable auto-start, vừa start ngay bây giờ.

---

## 📌 Tóm gọn

- `start` = chạy bây giờ, nhưng reboot mất.
    
- `enable` = tự chạy khi boot, nhưng không chạy ngay.
    
- Dùng cả hai nếu muốn chạy ngay + giữ cho lần boot sau.
    
- Hoặc dùng `enable --now`.
    

---
