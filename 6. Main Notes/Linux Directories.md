## 1. Tổng Quan về Cấu Trúc Thư Mục Linux

Trong Linux, tất cả các thư mục đều bắt nguồn từ thư mục gốc (**root directory**), được biểu thị bằng ký tự /. Từ thư mục gốc, các thư mục con được tổ chức theo chức năng cụ thể, phục vụ các mục đích như lưu trữ tệp hệ thống, dữ liệu người dùng, hoặc tệp cấu hình. Cấu trúc này mang tính phân cấp (hierarchical), giúp dễ dàng quản lý và truy cập.

## 2. Các Thư Mục Quan Trọng trong Linux

### /bin - Binary Files

- **Chức năng**: Chứa các tệp thực thi (executable binaries) cần thiết cho hệ thống, ví dụ: các lệnh cơ bản như ls, cp, mv, hoặc cat.
    
- **Đặc điểm**: Các lệnh trong /bin có thể được sử dụng bởi cả người dùng thường và quản trị viên (root), ngay cả trong chế độ **single-user mode**.
    
- **Ví dụ**: /bin/ls, /bin/bash.
    

### /etc - Configuration Files

- **Chức năng**: Lưu trữ các tệp cấu hình hệ thống, chẳng hạn như cấu hình mạng, người dùng, hoặc dịch vụ.
    
- **Đặc điểm**: Chủ yếu là tệp văn bản, có thể chỉnh sửa bằng trình soạn thảo như nano hoặc vim.
    
- **Ví dụ**:
    
    - /etc/passwd: Danh sách người dùng.
        
    - /etc/fstab: Cấu hình hệ thống tệp.
        
    - /etc/apt/sources.list: Danh sách nguồn phần mềm (trên hệ Debian/Ubuntu).
        

### /home - User Home Directories

- **Chức năng**: Chứa thư mục cá nhân của từng người dùng (trừ root). Mỗi người dùng có thư mục riêng để lưu trữ dữ liệu cá nhân.
    
- **Đặc điểm**: Người dùng thường có toàn quyền trong thư mục /home/username của mình.
    
- **Ví dụ**: /home/john chứa tệp và thư mục của người dùng john.
    

### /root - Root User Home Directory

- **Chức năng**: Thư mục home của người dùng root (quản trị viên).
    
- **Đặc điểm**: Không nằm trong /home để đảm bảo an toàn và tách biệt.
    
- **Ví dụ**: /root/.bashrc chứa cấu hình shell của root.
    

### /var - Variable Data

- **Chức năng**: Lưu trữ dữ liệu thay đổi trong quá trình hoạt động của hệ thống, như tệp nhật ký (log), dữ liệu tạm thời, hoặc tệp email.
    
- **Đặc điểm**: Thư mục này thường có kích thước lớn trên các hệ thống bận rộn.
    
- **Ví dụ**:
    
    - /var/log: Chứa tệp nhật ký như /var/log/syslog.
        
    - /var/www: Thư mục mặc định cho các tệp web server.
        

### /tmp - Temporary Files

- **Chức năng**: Lưu trữ các tệp tạm thời được tạo bởi hệ thống hoặc ứng dụng.
    
- **Đặc điểm**: Tệp trong /tmp thường bị xóa khi khởi động lại hoặc theo lịch trình.
    
- **Ví dụ**: Tệp tạm từ trình duyệt hoặc phần mềm.
    

### /usr - User System Resources

- **Chức năng**: Chứa các tệp và chương trình không cần thiết cho khởi động hệ thống, thường dành cho người dùng.
    
- **Các thư mục con**:
    
    - /usr/bin: Các lệnh không cần thiết cho hệ thống (như gcc, python).
        
    - /usr/lib: Thư viện cho các chương trình trong /usr/bin.
        
    - /usr/local: Phần mềm cài đặt thủ công bởi quản trị viên.
        
- **Ví dụ**: /usr/bin/firefox.
    

### /sbin - System Binaries

- **Chức năng**: Chứa các lệnh hệ thống cần thiết cho quản trị, thường chỉ root sử dụng.
    
- **Ví dụ**: /sbin/fdisk, /sbin/reboot.
    

### /lib - Libraries

- **Chức năng**: Chứa các thư viện cần thiết cho các chương trình trong /bin và /sbin.
    
- **Đặc điểm**: Thường bao gồm các tệp .so (shared objects).
    
- **Ví dụ**: /lib/libc.so.6.
    

### /mnt và /media - Mount Points

- **Chức năng**:
    
    - /mnt: Điểm gắn kết tạm thời cho thiết bị (do quản trị viên gắn thủ công).
        
    - /media: Điểm gắn kết tự động cho thiết bị như USB hoặc ổ đĩa CD.
        
- **Ví dụ**: /media/username/usb cho ổ USB được gắn tự động.
    

### /dev - Device Files

- **Chức năng**: Chứa các tệp đại diện cho thiết bị phần cứng (như ổ đĩa, bàn phím).
    
- **Đặc điểm**: Các tệp này không phải là dữ liệu thông thường mà là giao diện với thiết bị.
    
- **Ví dụ**: /dev/sda (ổ cứng), /dev/null (thiết bị "hố đen").
    

### /proc - Process Information

- **Chức năng**: Hệ thống tệp ảo chứa thông tin về các tiến trình và trạng thái hệ thống.
    
- **Đặc điểm**: Tồn tại trong bộ nhớ, không chiếm dung lượng ổ đĩa.
    
- **Ví dụ**: /proc/cpuinfo cung cấp thông tin về CPU.
    

### /boot - Boot Loader Files

- **Chức năng**: Chứa các tệp cần thiết để khởi động hệ thống, như kernel và cấu hình bootloader.
    
- **Ví dụ**: /boot/vmlinuz (kernel), /boot/grub/grub.cfg (cấu hình GRUB).