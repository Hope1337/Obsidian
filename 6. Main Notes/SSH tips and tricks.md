[[SSH]]
- `Ctrl + D`  để dis ra khỏi SSH
- Trong `/homne/[user_name]/.ssh` có thể sẽ có 3 files:  
	- `id_ed25519`: lưu private key
	- `id_ed25519.pub`: lưu public key
	- `known_hosts`: lưu fingerprint của các hosts đã từng kết nối
- Có thể tạo thêm file `config` để hoạt động như một DNS server:
```
Host [host_name]
	Hostname [ip_address]
	Port [port]
	User [user_name]
```


#### SSH key

```shell
ssh-keygen -t [ed25519/rsa] -C "youremail.com"
```
- `t`: chỉ định loại khóa
- `C`: thêm comment

Nó sẽ yêu cầu bạn nhập passphraseđể kể cả khi có ai lấy được keyssh, họ cũng sẽ phải có passphrase để giải mã được key và dùng.

- Trên remote server, tạo file `authorized_keys` tron `.ssh` folder để lưu public keys. Mỗi key trên một dòng

Có thể add key vào luôn trên terminal từ máy client
```
ssh-copy-id -i ~/.ssh/[file].pub [user]@[ip_address]
```

---
## Managing SSH keys

SSH chỉ định key:
```shell
ssh -i [key_file].pub
```

#### ssh-agent
Có nhiệm vụ giữ key đã được giải mã trong bộ nhớ tạm thời (khỏi nhập passphrase nhiều lần)

Khởi động agent
```shell
eval "$(ssh-agent) -s"
```
Nó sẽ hiện ra PID hiểu
```shell
Agent pid 12345
```

Thêm private key vào `ssh-agent`
```
ssh-add ~/.ssh.id_ed25519
```

Kiểm tra các key đang được giữ
```shell
ssh-add -l
```
Ví dụ một output
```shell
256 SHA256:abcd123... your_email@example.com (ED25519)
```

## Config SSH server

`/etc/ssh/sshd_config` cho phép config server, tức là cách máy người khác kết nối vào máy của bạn (`ssh_config` là config cho client). Đây cũng là nơi để bạn chỉnh:
- Port SSH
- Có cho đăng nhập bằng mật khẩu hay không
==Note==: Nhớ restart service lại để nó kích hoạt