- Isaac ROS build trên ROS2,  tuy nhiên các node được accelerated bằng các phương thức:
	- NITROS (NVIDIA Isaac Transport for ROS): Các node trong ROS2 giao tiếp thông qua serialization, tức là biến đổi cấu trúc dữ liệu thành chuỗi byte và truyền đi, sau đó deserialize ở đầu nhận. Quy trình này tốn kém về mặt tính toán ở CPU và gây ra độ trễ. Hơn nữa việc copy data từ user space và kernel space cũng tạo nên độ trễ.
	- Type Apdation: ROS2 thường làm việc với các kiểu dữ liệu chung chung và phải chuyển đổi giữa chúng rất nhiều. Tuy nhiên phần cứng như GPU lại yêu cầu dữ liệu được sắp xếp trong bộ nhớ thông qua một kiểu dữ liệu cụ thể. NITROS giải quyết vấn đề về định dạng dữ liệu thông qua Type Adaption:  cho phép các node Isaac ROS làm việc trực tiếp với định dạng tối ưu cho phần cứng.
	- Type Negotiation: Giả sử có hai Node A và B, A dữ liệu cho B và A báo rằng dữ liệu đang nằm trên GPU, nếu B báo ngược lại B cũng đang chờ ở GPU thì có thể đọc dữ liệu trực tiếp mà không cần switch context.
Hệ thống GEMS:

Isaac ROS được tổ chức thành các packages được gọi là GEMs. Mỗi GEM là một node ROS2 được tăng tốc trên GPU, giải quyết một bài toán cụ thể, ví dụ:
- **Isaac ROS Visual SLAM:** Sử dụng hình ảnh từ stereo camera để tính toán vị trí của robot (Odometry) và lập bản đồ môi trường thưa (sparse map). Nó sử dụng thư viện `cuVSLAM` chạy trên GPU để theo dõi hàng nghìn điểm đặc trưng đồng thời, mang lại độ chính xác cao ngay cả trong môi trường thiếu đặc trưng hoặc chuyển động nhanh.   

- **Isaac ROS DNN Inference:** Các gói như `isaac_ros_tensorrt` cho phép chạy các mô hình Deep Learning (YOLO, DetectNet, RT-DETR) đã được tối ưu hóa bằng TensorRT. Điều này đảm bảo robot có thể nhận diện vật thể với tốc độ khung hình cao (FPS) mà không làm nghẽn hệ thống.   

- **Isaac ROS Nvblox:** Một giải pháp tái tạo 3D dày đặc (dense reconstruction). Nó sử dụng hàm khoảng cách có dấu (TSDF) để xây dựng mô hình 3D của môi trường trong thời gian thực, cho phép robot tránh các chướng ngại vật phức tạp mà bản đồ 2D truyền thống không thấy được (ví dụ: mặt bàn nhô ra, vật thể treo lơ lửng).   

- **Isaac ROS Image Pipeline:** Cung cấp các công cụ xử lý ảnh cơ bản nhưng quan trọng như _Rectification_ (khử méo ống kính), _Resize_, _Format Conversion_, tất cả đều chạy trên GPU hoặc phần cứng VIC (Video Image Compositor) chuyên dụng trên Jetson.

---



> Tại sao Isaac ROS không cung cấp một docker image luôn đi? Bắt clone repo về nữa làm gì

Để docker hoạt được được với GPU, nhận camera, hiển thị được rviz ... thì scripts sẽ trông kinh khủng khiếp như này:
```shell
docker run -it --rm \
    --gpus all \                                     # Để nhận GPU
    --runtime=nvidia \
    -e NVIDIA_VISIBLE_DEVICES=all \
    -e NVIDIA_DRIVER_CAPABILITIES=all \
    -v /tmp/.X11-unix:/tmp/.X11-unix \               # Để hiện cửa sổ Rviz
    -e DISPLAY=$DISPLAY \
    -v $HOME/.Xauthority:/root/.Xauthority \
    --network host \                                 # Để giao tiếp ROS2
    --privileged \                                   # Để truy cập USB/Camera
    -v /dev/*:/dev/* \
    -v $(pwd):/workspaces/isaac_ros-dev \            # Mount code của bạn vào
    --user $(id -u):$(id -g) \                       # Map user ID để không bị lỗi quyền file
    ... (và còn chục dòng nữa tuỳ cấu hình) ...
    nvidia/isaac-ros-dev:latest
```

Hơn nữa còn một số vấn đề khác như tương thích phần cứng như trên x86 và jetpack, do đó isaac ros repo cung cấp hết cái này cho mình, chỉ cần gọi lên mà dùng thôi.


---

