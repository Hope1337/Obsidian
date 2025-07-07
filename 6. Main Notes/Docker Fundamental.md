## Objects in Docker
#### Images
Image là một read-only template với những instructions để tạo một Docker container. Thường thì một image sẽ dựa trên một image khác (như kiểu dependency). Ví dụ một image có thể dựa trên `ubuntu` image chẳng hạn nhưng tải thêm `conda` cũng như cấu hình nhiều thứ khác để chạy. 

Có thể tưởng tượng image là một class trong python.

#### Containers
Có thể xem container là một instance của class (image). Ta có thể kết nối nhiều image hoặc networks lại với nhau, nối storage.

## Docker Compose
Dùng để stack nhiều container lại với nhau trong 1 lần chạy, có thể setup cách giao tiếp giữa các container qua mạng `bridge`. Đây là mạng nội bộ mà docker dùng để các container gọi lẫn nhau.

## Docker File
Docker file giống như một bảng hướng dẫn cho docker biết làm cách nào để build được môi trường cho image. Dưới đây là một docker file mẫu:
```dockerfile
# 1. Base image có sẵn PyTorch, CUDA (nếu cần)
FROM pytorch/pytorch:2.2.2-cuda12.1-cudnn8-runtime

# 2. Thiết lập thư mục làm việc trong image
WORKDIR /app

# 3. Copy toàn bộ nội dung project từ máy host vào image
COPY . .

# 4. Cài đặt dependencies (nếu có requirements.txt)
RUN pip install --no-cache-dir -r requirements.txt

# 5. Thiết lập biến môi trường (tuỳ chọn)
ENV PYTHONUNBUFFERED=1

# 6. Mở port nếu chạy server (ví dụ: Flask, FastAPI, etc.)
EXPOSE 8080

# 7. Chạy dưới user mặc định (hoặc bạn có thể tạo user riêng nếu cần)
# USER root  # hoặc USER appuser nếu bạn tạo riêng

# 8. Command mặc định khi container chạy
CMD ["python", "main.py"]
```
- `FROM <image>`: Đây là lệnh nói rằng image hiện tại được build dựa trên image có sẵn trước đó, tức là đã có sẵn os các thứ, build từ đầu thì có mà chết=))))
- `WORDIR <path>`: Tạo một folder nơi mà code được copy từ host và được chạy. Do có nhiều file hệ thống trong `/` nên việc tách biệt này là cần thiết để tránh ghi đè không đáng có.
- `COPY <host-path> <image-path>`: copy toàn bộ nội dung thư mục trên host vào image. 
- `RUN <command>`: dùng để cài môi trường (dependencies)
- `ENV <name> <value>`: Khai báo biến môi trường đó ní
- `EXPOSE  <port>`: là cổng bên trong container, lưu ý đây chỉ là một dạng comment cho dev hiểu thôi, chưa có dùng được, phải map (từ từ sẽ biết map như nào).
- `CMD ["<command>", "<arg1>"]`: Chạy các lệnh khi container đã sẵn sàng, còn `RUN` là khi đang build. Giống như `RUN` là lúc `conda install` mấy packages vậy, còn `RUN` là sau khi build env xong, ta `python main.py`.
## `.dockerignore`
Giống như `.gitignore`
## Multi-stage builds
Build ở nhiều bước, ở bước cuối chỉ giữ lại những gì thực sự cần thiết cho image. Ví dụ image đang cần package `A` và `B`, nhưng để build `A` và `B` cần `C`, `D`, `E`. Tuy nhiên ở cuối thì image ko cần mấy package kia lắm mà chỉ cần `A` và `B` thôi là đủ chạy rồi thì việc mang theo nhiều ko có ý nghĩa lắm. Ví dụ

```dockerfile
# =============================
# STAGE 1 – Build dependencies
# =============================
FROM python:3.10 AS builder

WORKDIR /app

# Cài requirements vào thư mục tạm
COPY requirements.txt .
RUN pip install --no-cache-dir --prefix=/install -r requirements.txt

# Copy source code
COPY . .

# =============================
# STAGE 2 – Runtime-only image
# =============================
FROM pytorch/pytorch:2.2.2-cuda12.1-cudnn8-runtime

WORKDIR /app

# Copy cài đặt từ builder (chỉ những gì cần)
COPY --from=builder /install /usr/local

# Copy source code
COPY --from=builder /app /app

# Run app
CMD ["python", "main.py"]
```

## Port mapping

Mỗi container như một môi trường biệt lập, do đó có thể giao tiếp giữa các container với nhau hoặc cung cấp API, ta cần mở port cho tụi nó.

Publishing một port là **kết nối port của container vào port của host**

```bash
docker run -p <host-port>:<container-port> image-name
```


---
Tiếp theo đến với khái niệm persistent data. Vì mỗi khi kết thúc phiên chạy container, dữ liệu đều sẽ mất đi. Để lưu trữ lại dữ liệu này thì có 2 cách:
- **Volume mount**: Vùng nhớ do docker quản lý
- **Bind mount**: Nối hẳn bộ nhớ của host vào container
Mặc định docker khuyến khích dùng volume mount vì vấn đề hiệu suất.
## Volume Mount

- Volume được quản lý bởi docker
- Volume có hỗ trợ lưu vào remote hoặc cloud

**Sử dụng Volume khi**: 
- Chia sẻ dữ liệu giữ các container
- Lưu vào remote hoặc cloud
- Backup, restore hoặc migrate data từ host này sang host khác

## Bind Mount

Có vài lưu ý về phần này và phần volume mount mà vẫn chưa hiểu lắm, cụ thể là khi mount vào folder đã có file từ trước.

## Docker Ulities
- `build`: để build image từ source code
- `models`: run model llm với giao diện chat có sẵn (adu)
- `mcp serve`: cái này mới
