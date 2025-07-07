#### DHCP
**Dynamic Host Configuration Protocol** (Tạm dịch: Giao thức cấp phát cấu hình động cho thiết bị mạng)

Khi một thiết bị (laptop, điện thoại, v.v.) **kết nối vào mạng**, nó cần:
- Địa chỉ IP
- Gateway (cổng ra Internet)
- DNS server (để truy cập web)
- Subnet mask
-**DHCP sẽ tự động cấp những thông tin đó**.  

---

#### IP Address
Các quy ước chính

| Dải IP             | Được hiểu là                        | Xử lý ra sao?                                    |
| ------------------ | ----------------------------------- | ------------------------------------------------ |
| `10.0.0.0/8`       | Private (nội bộ – LAN)              | **Giữ trong mạng nội bộ**, không gửi ra Internet |
| `172.16.0.0/12`    | Private                             | Tương tự                                         |
| `192.168.0.0/16`   | Private                             | Tương tự                                         |
| `127.0.0.1`        | Loopback (vào chính mình)           | Không gửi ra ngoài                               |
| `169.254.0.0/16`   | Link-local (tự gán IP khi mất mạng) | Không ra Internet                                |
| **Tất cả còn lại** | Public IP (WAN – Internet)          | Được gửi ra Internet nếu có route                |

Trước giờ mình tưởng mạng LAN và WAN dùng chung 1 dải IP và chỉ phân biệt qua 1 flag nào đó thôi. Tuy nhiên IP hoạt động theo quy ước, tức là chỉ cần bạn nhập một IP được quy ước là mạng WAN, ví dụ `132.568.137.2` chẳng hạn, nó mặc định đây là IP Internet và gửi request cho router. Còn gọi `192.168.1.1` thì là vào setting router đó.

---

## Port

| Port Range        | Tên gọi               | Mục đích sử dụng                                    |
| ----------------- | --------------------- | --------------------------------------------------- |
| **0 – 1023**      | Well-known ports      | Các dịch vụ chuẩn & phổ biến (HTTP, SSH, DNS...)    |
| **1024 – 49151**  | Registered ports      | Dành cho ứng dụng người dùng & dịch vụ được đăng ký |
| **49152 – 65535** | Dynamic/Private ports | Dùng tạm thời (ephemeral) bởi hệ điều hành hoặc app |

|Dịch vụ|Port|Ghi chú|
|---|---|---|
|**HTTP**|`80`|Web không bảo mật|
|**HTTPS**|`443`|Web bảo mật (SSL/TLS)|
|**SSH**|`22`|Truy cập từ xa|
|**FTP**|`21`|Truyền file (không bảo mật)|
|**SFTP/SSH File**|`22`|Truyền file an toàn qua SSH|
|**SMTP**|`25` / `587`|Gửi email|
|**IMAP/POP3**|`143` / `110`|Nhận email|
|**MySQL**|`3306`|Database|
|**PostgreSQL**|`5432`|Database|
|**Redis**|`6379`|Cache/database in-memory|
|**Docker daemon**|`2375`, `2376`|Docker remote API|
|**Nextcloud**|`80`, `443`, hoặc `8080+`|Tuỳ bạn cấu hình|

---

## CG NAT

Trước tiên là nói về NAT (Network Address Translation), đây là cơ chế dịch IP nội bộ trong mạng LAN ra IP tĩnh WAN. Tức là lúc này router đóng vai trò thay các devices cục bộ đi ra ngoài internet nhận hoặc gửi thông tin. Lý do cho việc này là do IPv4 không đủ phân phát cho toàn bộ devices nên cần phải NAT.

CG NAT là khi nhà mạng NAT thêm một lần nữa, tức là nhà mạng mới thực sự đóng vai trò có mặt trên IP public ra WAN, các router chỉ gửi về nhà mạng mà thôi. 

CG NAT cản trở không cho ta port forwarding do modem lúc này là nhà mạng, ta ko được phép setting. 

Có vẻ như muốn có IP tĩnh trong WAN thì phải mua public IP, và giá khá cao á. 