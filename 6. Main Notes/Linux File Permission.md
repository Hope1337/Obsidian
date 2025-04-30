File permission (quyền truy cập tệp) trên Ubuntu là một phần quan trọng trong việc quản lý hệ thống Linux, giúp kiểm soát ai có thể đọc, ghi hoặc thực thi một tệp hoặc thư mục. 

## 1. Hiểu về File Permission

Trong Ubuntu (và các hệ thống Linux nói chung), mỗi tệp hoặc thư mục đều có ba loại quyền cơ bản:

- **Read (r)**: Quyền đọc, cho phép xem nội dung tệp hoặc liệt kê nội dung thư mục.
    
- **Write (w)**: Quyền ghi, cho phép sửa đổi nội dung tệp hoặc thêm/xóa tệp trong thư mục.
    
- **Execute (x)**: Quyền thực thi, cho phép chạy tệp (nếu là chương trình hoặc script) hoặc truy cập vào thư mục.
    

Quyền này được áp dụng cho ba nhóm người:

- **Owner (u)**: Người sở hữu tệp/thư mục.
    
- **Group (g)**: Nhóm người dùng được gán cho tệp/thư mục.
    
- **Others (o)**: Những người dùng khác không thuộc owner hoặc group.
    

### Xem quyền của tệp/thư mục

Để xem quyền của một tệp hoặc thư mục, sử dụng lệnh `ls -l` trong Terminal. Kết quả sẽ hiển thị như sau:

```bash
-rwxr-xr-x  1 user group  4096 Apr 30 2025 example.txt
```

Trong đó:

- -rwxr-xr-x: Chuỗi 10 ký tự thể hiện quyền:
    
    - Ký tự đầu tiên (-): Loại tệp (- là tệp, d là thư mục).
        
    - 3 ký tự tiếp theo (rwx): Quyền của owner (đọc, ghi, thực thi).
        
    - 3 ký tự tiếp theo (r-x): Quyền của group (đọc, không ghi, thực thi).
        
    - 3 ký tự cuối (r-x): Quyền của others (đọc, không ghi, thực thi).
        
- **user**: Tên người sở hữu.
    
- **group**: Tên nhóm được gán.
    
- **example.txt**: Tên tệp/thư mục.
    

## 2. Quản lý File Permission với chmod

Lệnh chmod (change mode) được sử dụng để thay đổi quyền truy cập của tệp/thư mục. Có hai cách sử dụng chmod: **Symbolic mode** và **Numeric mode**.

### Symbolic Mode

Sử dụng các ký hiệu như u (owner), g (group), o (others), a (all), và +, -, = để thêm, xóa hoặc gán quyền.

Ví dụ:

- Thêm quyền thực thi cho owner:
    
    ```bash
    chmod u+x example.txt
    ```
    
- Xóa quyền ghi của group:
    
    ```bash
    chmod g-w example.txt
    ```
    
- Gán quyền đọc và ghi cho tất cả:
    
    ```bash
    chmod a=rw example.txt
    ```
    

### Numeric Mode

Quyền được biểu diễn bằng số octal (0-7), trong đó:

- 4 = read (r)
    
- 2 = write (w)
    
- 1 = execute (x)
    
- 0 = không có quyền
    

Kết hợp các số này để tạo quyền cho owner, group và others. Ví dụ:

- 755: Owner có đầy đủ quyền (rwx = 4+2+1), group và others chỉ đọc và thực thi (r-x = 4+1).
    
- 644: Owner đọc và ghi (rw- = 4+2), group và others chỉ đọc (r-- = 4).
    

Ví dụ lệnh:

- Gán quyền 755 cho tệp:
    
    ```bash
    chmod 755 example.txt
    ```
    
- Gán quyền 644 cho thư mục:
    
    ```bash
    chmod 644 myfolder
    ```
    

### Áp dụng đệ quy

Để thay đổi quyền cho cả thư mục và các tệp bên trong, thêm tùy chọn -R (recursive):

```bash
chmod -R 755 myfolder
```

## 3. Thay đổi Owner và Group với chown và chgrp

### Thay đổi Owner (chown)

Lệnh chown (change owner) thay đổi người sở hữu tệp/thư mục. Ví dụ:

- Chuyển owner của tệp sang người dùng newuser:
    
    ```bash
    sudo chown newuser example.txt
    ```
    
- Thay đổi cả owner và group:
    
    ```bash
    sudo chown newuser:newgroup example.txt
    ```
    
- Áp dụng đệ quy:
    
    ```bash
    sudo chown -R newuser:newgroup myfolder
    ```
    

### Thay đổi Group (chgrp)

Lệnh chgrp (change group) thay đổi nhóm của tệp/thư mục. Ví dụ:

- Gán nhóm newgroup cho tệp:
    
    ```bash
    sudo chgrp newgroup example.txt
    ```
    
- Áp dụng đệ quy:
    
    ```bash
    sudo chgrp -R newgroup myfolder
    ```
    

## 4. Một số lưu ý quan trọng

- **Sử dụng** sudo: Một số lệnh thay đổi quyền hoặc owner yêu cầu quyền root, đặc biệt khi bạn không phải là owner của tệp.
    
- **Cẩn thận với quyền 777**: Quyền 777 (rwx cho tất cả) có thể gây rủi ro bảo mật, đặc biệt trên các tệp/thư mục quan trọng.
    
- **Kiểm tra quyền trước khi thực thi**: Luôn dùng ls -l để kiểm tra quyền hiện tại trước khi thay đổi.
    
- **Sticky Bit, SUID, SGID**: Đây là các quyền đặc biệt, thường dùng cho các thư mục hoặc chương trình hệ thống. Ví dụ, sticky bit (t) trên thư mục /tmp đảm bảo chỉ owner mới có thể xóa tệp của họ.
    

Ví dụ thiết lập sticky bit:

```bash
chmod +t myfolder
```

## 5. Ví dụ thực tế

Giả sử bạn có một thư mục /var/www/html chứa các tệp web:

- Gán owner là www-data (thường dùng cho web server):
    
    ```bash
    sudo chown -R www-data:www-data /var/www/html
    ```
    
- Đặt quyền 755 cho thư mục và 644 cho tệp:
    
    ```bash
    sudo chmod -R 755 /var/www/html
    find /var/www/html -type f -exec chmod 644 {} \;
    ```