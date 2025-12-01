ROS bag được dùng để record và phát lại data (ảnh, sensor, âm thanh ...), điều này giúp quá trình kiểm nghiệm code trở nên đơn giản hơn vì có thể test trực tiếp lên pre-recorded data mà không cần phải setup giả lập hay phần cứng thực tế.

## Demo

Mở 3 terminal, trên terminal 1, gõ:

```shell
ros2 run turtlesim turtlesim_node
```

để chạy visualization cho turtle sim. 
Tiếp đến là run 1 node nhận lệnh:

```shell
ros2 run turtlesim turtle_teleop_key
```

Record lệnh này:
```shell
ros2 bag record /turtle1/cmd_vel
```


Tiến hành điều khiển robot, sau khi hoàn thành, một tệp `.bag` sẽ được gen ra trong thư mục hiện tại. Khởi động lại turtle sim và chạy lệnh sau đây để phát lại dữ liệu `vel_cmd`:
```shell
ros2 bag play [record_file]
```

Có thể sẽ cần thời gian để connections được thiết  lập nên lệnh đầu được phát sẽ không nhận, do vậy có thể thêm cờ `-d` và kèm theo số giây ros bag cần đợi trước khi phát lệnh trực tiếp.
