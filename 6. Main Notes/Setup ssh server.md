#ssh 
##### I. Một chút kiến thức về mạng

Mạng Internet có thể được xem như một lưới các máy kết nối với nhau, mỗi máy được phân biệt qua một IP duy nhất. Tuy nhiên, thường thì máy tính cá nhân của chúng ta sẽ không có cái IP duy nhất này mà chỉ có "IP cục bộ". 

Để dùng được mạng, bạn phải có modem wifi đúng không? Chính nó sẽ có IP toàn cục (duy nhất) hiện diện trong internet, modem này sẽ cấp các địa chỉ IP cho các máy tính kết nối với nó, được gọi là IP cục bộ. Khi một máy tính nào (trong mạng cục bộ của modem) muốn kết nối vào internet để truy vấn hoặc gửi dữ liệu, nó sẽ phải thông qua modem, tức gửi request cho modem, modem lên Internet nhận data và trả về.

Tuy nhiên, một số dịch vụ như ssh yêu cầu bạn phải setup cái được gọi là **forwarding** để có thể sử dụng.

> Forwarding là gì và tại sao phải setup forwarding mới dùng ssh được?

Khi một máy từ ngoài mạng internet muốn ssh đến máy cá nhân của bạn, như đã nói, máy của bạn không có IP global để nhận diện, chỉ có modem của bạn là có. Thế cho nên khi ssh, thực tế là bạn gửi request tới modem trước và modem sẽ dựa vào thông tin được setup trong forwarding mà gửi request này về lại máy của bạn.

> Mình dùng trình duyệt web để xem youtube có cần phải setup gì đâu? Modem vẫn gửi về máy mình đó thôi?

Chuẩn rầu. Có thể hiểu thế này: khi xem youtube, máy của bạn đến modem và nói "hãy gửi về giúp tôi cái video này", tức là modem biết người yêu cầu là ai và khi nó nhận được thông tin rồi cũng sẽ biết địa chỉ để trả về. Tuy nhiên ssh là do máy từ ngoài yêu cầu kết nối vào và modem không rõ máy nào trong mạng của mình đang được yêu cầu ssh cho nên phải nói cho modem biết, và cái này là thông qua setup forwarding.

> Sao không lúc ssh không nói cái IP cục bộ của máy nhận luôn đi cho đỡ mắc công?

Chà, cái này mình cũng không biết, tuy nhiên có khả năng là:
- Modem có IP cố định, điều này dễ hơn cho các máy khác ssh vào thay vì IP cục bộ có thể bị thay đổi nhiều.
- Bảo mật của cách làm trên không cao

##### II. Setup forwarding

Mình đang dùng Viettel, có thể một số bước sẽ khác một tí (không nhiều).

###### Bước 1: Đăng nhập vào modem.
Vào `https://192.168.1.1/` , đăng nhập tài khoản và mật khẩu (dưới cục modem có ghi ý).

###### Bước 2: Setup ssh fowarding

Vào **Advance** -> **Internet** -> **Security** -> **Port Forwarding

![[Pasted image 20250422044357.png]]

- **WAN**: Wide Area Network, tức là cục modem.
- **LAN** : Local Area Network, tức là máy cục bộ trong mạng.
- **Port** : Có nhiều cổng để nhận thông tin chứ không dồn hết vào một chỗ, cái này gọi là port.

| Trường                  | Ý nghĩa                         | Giải thích cụ thể                                                                                                                            |
| ----------------------- | ------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| **Name**                | Tên rule                        | Tùy đặt, ví dụ: `ssh` cho dễ nhớ. Không ảnh hưởng kỹ thuật.                                                                                  |
| **Protocol**            | Giao thức                       | Bạn chọn **TCP And UDP**, nghĩa là cả hai đều được forward (dù SSH chỉ dùng TCP).                                                            |
| **WAN Connection**      | Kết nối WAN nào dùng            | Thường để Auto là OK. Nếu modem có nhiều WAN (ví dụ có cả mạng chính và phụ) thì chọn đúng WAN mới đúng.                                     |
| **WAN Host IP Address** | IP ngoài nào được phép truy cập | Đang để `0.0.0.0 ~ 0.0.0.0` nghĩa là **mọi IP từ internet đều được phép truy cập** vào port này.                                             |
| **LAN Host**            | IP nội bộ máy đích              | Ở đây là `192.168.1.35`, nghĩa là máy bạn muốn mở cổng SSH tới. Máy này phải có SSH server chạy và mở port 2200.                             |
| **WAN Port**            | Cổng bên ngoài                  | Port trên IP công cộng mà người ta sẽ kết nối tới. Ở đây là `2200`. Nếu bạn SSH từ ngoài, sẽ dùng `ssh -p 2200`.                             |
| **LAN Host Port**       | Cổng nội bộ                     | Port trên máy nội bộ sẽ nhận kết nối. Ở đây cũng là `2200`, tức là bạn định máy kia cũng đang chạy SSH ở port 2200 (không phải mặc định 22). |

##### III. Setup ssh server trên máy

Để sử dụng SSH trên PC của bạn với Ubuntu, sau khi đã thiết lập port forwarding (chuyển tiếp cổng) để lắng nghe trên cổng 5000, bạn cần thực hiện các bước sau:

1. **Kiểm tra dịch vụ SSH đã được cài đặt và đang chạy**:
   - Mở terminal trên Ubuntu và chạy lệnh:
     ```bash
     sudo apt update
     sudo apt install openssh-server
     ```
     Điều này đảm bảo gói `openssh-server` đã được cài đặt.
   - Kiểm tra trạng thái dịch vụ SSH:
     ```bash
     sudo systemctl status ssh
     ```
     Nếu dịch vụ chưa chạy, khởi động nó:
     ```bash
     sudo systemctl start ssh
     sudo systemctl enable ssh
     ```

1. **Cấu hình SSH để lắng nghe trên cổng khác ngoài cổng mặc định**:
   - Mở tệp cấu hình SSH:
     ```bash
     sudo nano /etc/ssh/sshd_config
     ```
   - Tìm dòng `#Port 22` và thêm hoặc sửa thành:
     ```bash
     Port 5000
     ```
     Lưu ý: Nếu bạn muốn SSH lắng nghe cả cổng 22 và 5000, chỉ cần thêm dòng `Port 5000` mà không xóa `Port 22`.
   - Lưu tệp và thoát (Ctrl+O, Enter, Ctrl+X trong nano).

3. **Khởi động lại dịch vụ SSH**:
   - Sau khi thay đổi cấu hình, khởi động lại SSH để áp dụng:
     ```bash
     sudo systemctl restart ssh
     ```

==Note==: Từ ubuntu 23.10 trở đi, việc sửa file config này thôi là chưa đủ 

3. **Kiểm tra firewall (nếu có)**:
   - Đảm bảo cổng 5000 được mở trên firewall của Ubuntu:
     ```bash
     sudo ufw allow 5000/tcp
     sudo ufw status
     ```
     Nếu bạn không sử dụng `ufw`, kiểm tra bất kỳ firewall nào khác (như `iptables`).

5. **Xác minh port forwarding trên router**:
   - Đảm bảo router của bạn đã được cấu hình để chuyển tiếp cổng 5000 từ địa chỉ IP công cộng đến địa chỉ IP nội bộ của PC (ví dụ: 192.168.x.x). Bạn có thể kiểm tra địa chỉ IP nội bộ của PC bằng:
     ```bash
     ip addr show
     ```
     Tìm địa chỉ IP trong phần `inet` của giao diện mạng (thường là `eth0` hoặc `wlan0`).

6. **Kết nối qua SSH**:
   - Từ một máy tính khác (hoặc từ chính PC nếu kiểm tra cục bộ), sử dụng lệnh SSH để kết nối:
     ```bash
     ssh -p 5000 username@your_public_ip
     ```
     - Thay `username` bằng tên người dùng trên Ubuntu.
     - Thay `your_public_ip` bằng địa chỉ IP công cộng của mạng (kiểm tra IP công cộng tại https://www.whatismyipaddress.com/).
   - Nếu kết nối cục bộ (trong cùng mạng LAN), bạn có thể dùng IP nội bộ:
     ```bash
     ssh -p 5000 username@192.168.x.x
     ```

7. **Kiểm tra lỗi (nếu không kết nối được)**:
   - Kiểm tra xem SSH có đang lắng nghe trên cổng 5000 không:
     ```bash
     sudo netstat -tuln | grep 5000
     ```
   - Nếu không thấy kết quả, kiểm tra lại cấu hình SSH và khởi động lại dịch vụ.
   - Đảm bảo router chuyển tiếp cổng chính xác và không có tường lửa chặn cổng 5000.
   - Xem log SSH để tìm lỗi:
     ```bash
     sudo tail -f /var/log/auth.log
     ```

7. **(Tùy chọn) Thiết lập khóa SSH để bảo mật**:

	Xem thêm về ssh key tại : [[Git thông qua ssh]]
	
   - Tạo cặp khóa SSH trên máy khách:
     ```bash
     ssh-keygen
     ```
   - Sao chép khóa công khai đến máy chủ Ubuntu:
     ```bash
     ssh-copy-id -p 5000 username@your_public_ip
     ```
   - Sau đó, bạn có thể vô hiệu hóa đăng nhập bằng mật khẩu trong `/etc/ssh/sshd_config`:
     ```bash
     PasswordAuthentication no
     ```
     Khởi động lại SSH sau khi chỉnh sửa.

Nếu bạn đã làm đúng các bước trên, bạn sẽ có thể sử dụng SSH trên cổng 5000. Nếu gặp vấn đề, hãy cung cấp thêm chi tiết (ví dụ: lỗi cụ thể, cấu hình mạng) để tôi hỗ trợ thêm!