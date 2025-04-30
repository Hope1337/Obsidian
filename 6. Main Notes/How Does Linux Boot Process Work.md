> Chuyện gì xảy ra khi ta nhấn nút nguồn máy tính có hệ điều hành linux?

Sau khi bạn nhấn nút power start: tín hiệu được gửi đến bo mạch chủ, bật nguồn cho CPU, RAM, ổ cứng và các thiết bị khác.

#### 1. BIOS/UEFI

Đầu tiêu, một chương trình trong máy gọi là `BIOS` hoặc `UEFI` sẽ được khởi động. Chắc các bạn lúc vọc máy tính đều quen với `BIOS` cả rồi:v. `BIOS` hay `UEFI` đều thực hiện cùng một chức năng cầu nối giữa hệ điều hành và thiết bị phần cứng của máy tính, giúp chuẩn bị các thành phần cần thiết để bạn dùng. Trong đó `UEFI` là một thế hệ mới hơn khi nó có thời gian boot nhanh hơn, hỗ trợ ổ đĩa lớn hơn 2TB (dùng GPT - GUID Partition Table) và có secure boots - một tính năng bảo mật của `UEFI`, được thiết kế để đảm bảo rằng máy tính chỉ khởi động với phần mềm đáng tin cậy, ngăn chặn các phần mềm độc hại hoặc hệ điều hành không được ủy quyền chạy trong quá trình khởi động. 

#### 2. Power-on-self-test (POST)

Tiếp đến, `BIOS/UEFI` sẽ chạy một bài kiểm tra tên là `POST` nhằm để kiểm tra các phần cứng có hoạt động đúng cách chưa trước khi bật tụi nó lên.

#### 3. Bootloader software

Tiếp theo, `BIOS/UEFI` chuyển quyền điều khiển sang cho `bootloader`, đây là một chương trình nhỏ chịu trách nhiệm load kernel và các thành phần cần thiết vào bộ nhớ. Đây là lúc bootloader phải tim kernel, mặc định thì nó tìm ở hard drive trước và sau đó là USB hoặc CD. Đó là lý do bạn cần đổi thứ tự tìm kiếm khi muốn boot từ USB. Công việc chính của bootloader có thể được tóm tắt như sau:
- Xác định vị trí của kernel trên bộ nhớ 
- Load kernel lên bộ nhớ chính
- Chạy kernel
==Note==: Boot loader phổ biến bạn có thể thấy là GRUB2
Sau đó, bootloader trao lại quyền cho kernel 
#### 4. Kernel run

Kernel sẽ đảm nhận một số công việc như khởi tạo phần cứng, gắn kết hệ thống têp... (mình không rõ cụ thể lắm) trước khi trao quyền lại cho hệ thống init.

#### 5. Init

**Init là gì?**: Hệ thống init (như **systemd**, **SysVinit**, hoặc **OpenRC**) chịu trách nhiệm khởi động các dịch vụ và đưa hệ thống vào trạng thái hoạt động. Bước này rất phức tạp.

