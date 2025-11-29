- Isaac ROS build trên ROS2,  tuy nhiên các node được accelerated bằng các phương thức:
	- NITROS: NVIDIA Isaac Transport for ROS


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