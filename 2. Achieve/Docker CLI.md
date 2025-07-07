Dưới đây là danh sách các lệnh Docker CLI thông dụng – được phân nhóm và ghi chú ngắn gọn để bạn dễ take note và sử dụng hằng ngày:

---

## 🐳 **Docker cơ bản**

|Lệnh|Mô tả|
|---|---|
|`docker --version`|Kiểm tra phiên bản Docker|
|`docker info`|Thông tin hệ thống Docker|
|`docker help`|Trợ giúp các lệnh Docker|

---

## 📦 **Image**

|Lệnh|Mô tả|
|---|---|
|`docker images`|Liệt kê image local|
|`docker pull <image>`|Tải image từ Docker Hub|
|`docker build -t <name> .`|Build image từ Dockerfile|
|`docker rmi <image>`|Xoá image|
|`docker tag <src> <repo>:<tag>`|Gắn thẻ cho image|

---

## 🧱 **Container**

| Lệnh                        | Mô tả                          |
| --------------------------- | ------------------------------ |
| `docker ps`                 | Xem container đang chạy        |
| `docker ps -a`              | Xem tất cả container           |
| `docker run -it <image>`    | Chạy container có tương tác    |
| `docker run -d <image>`     | Chạy container nền (detached)  |
| `docker start <id>`         | Bật lại container              |
| `docker stop <id>`          | Dừng container                 |
| `docker rm <id>`            | Xoá container                  |
| `docker exec -it <id> bash` | Truy cập shell trong container |
| `docker logs <id>`          | Xem log container              |

---

## 🗃️ **Volumes & Networks**

|Lệnh|Mô tả|
|---|---|
|`docker volume ls`|Danh sách volume|
|`docker volume create <name>`|Tạo volume mới|
|`docker network ls`|Liệt kê mạng Docker|
|`docker network create <name>`|Tạo mạng Docker mới|

---

## 🧰 **Khác**

|Lệnh|Mô tả|
|---|---|
|`docker inspect <id>`|Xem thông tin chi tiết (JSON)|
|`docker cp <src> <container>:<dest>`|Copy file vào container|
|`docker cp <container>:<src> <dest>`|Copy file ra khỏi container|
|`docker-compose up`|Chạy với docker-compose.yml|
|`docker-compose down`|Dừng và xoá tài nguyên|

---

> 💡 Gợi ý alias bash/zsh để dùng nhanh:

```bash
alias dps="docker ps"
alias dpsa="docker ps -a"
alias drm="docker rm"
alias drmi="docker rmi"
alias dexec="docker exec -it"
```

Bạn muốn mình chuyển bảng này thành file `.md`, `.pdf`, hoặc dạng cheat sheet hình ảnh không?