## docker.service could not be found
Khi muốn dùng ROS2 với docker, phải dùng bản docker egine chứ không dùng docker desktop. Docker desktop chạy một máy ảo nhỏ nữa nên việc setup `--net=host` là không có tác dụng. Hơn nữa docker desktop trong WSL sẽ có sẵn engine (`default` context), docker desktop bản thường thì không có

---
### permission denied
Có thể add `sudo` vào trước lệnh cũng được, tuy nhiên vậy hơi bất, tiện. Gọn nhất là tạo group `docker` 

```shell
sudo usermod -aG docker $USER
newgrp docker #kích hoạt ngay trong terminal hiện tại 
```

Muốn active docker group thì phải reboot lại phát.

---
### ROS2 docker không giao tiếp được với ROS2 native

Này là do cơ chế shared mem mà docker dùng để tối ưu, buộc dùng UDPv4 là được:
```shell
# Sửa UPD -> UDP
export FASTDDS_BUILTIN_TRANSPORTS=UDPv4
```

==Important==: nhớ thêm cờ `--net=host`.

