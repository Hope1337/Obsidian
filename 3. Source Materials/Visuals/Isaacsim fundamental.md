# Nvidia Course 1:

==Objectives==:
- **Làm quen với giao diện của Isaacsim**
- **Xây dựng một mô hình robot đơn giản**
- **Tùy chỉnh Physics Properties**
- **Sử dụng cơ chế điều khiển (ROS2)**
- **Tích hợp các cảm biến**
- **Stream dữ liệu cảm biến tới ROS2**

---
### Simple example

![[Pasted image 20251123090603.png]]

Trong documentation có để là phải tạo physics scene mới có vật lí, tuy nhiên khi không tạo và vẫn bấm play thì con robot vẫn rơi và va chạm vào mặt đất. Còn cube thì chỉ mesh thuần (object chỉ phục vụ phần nhìn, chưa có vật lí).

Để thêm vật lí cho cube, bấm vào add rồi tạo **Rigid Body with Colliders Preset**

---

# Creating simple robot

> Tài liệu để là có physic scene thì mới có vật lí, cơ mà import robot vào và bấm play thì vẫn thấy rơi (có trọng lực), tại sao vậy?

Đây là hành vi mặc định của play trong isaacsim. Tuy nhiên việc thêm physic scene vẫn là cần thiết nếu muốn môi trường hoạt động ổn định (gemini bảo vậy).

==Note==: 
- Nhấn giữ `Control` để chọn nhiều mục, nhấn giữ `Shift` để chọn nhiều mục liên tiếp. 
- Khi tạo `joint`, thứ tự click vào các thành phần cũng rất quan trọng, component được chọn đầu tiên sẽ đóng vai trò là parent (cố định), cái sau sẽ là child (di chuyển theo parent).

> Tạo joint phải định nghĩa trục xoay, local position và local rotation, nó là gì vậy?

- **trục xoay**: object sẽ xoay theo trục local được chỉ định
- **local position 0/1**: vị trí mà joint sẽ gắn vào component parent và child
- **local rotation 0**: góc mà joint bị lệch so với parent, ví dụ bánh xe lệch ra chứ không thẳng đứng.
- **local rotation 1**: căn chỉnh lại trục xoay local của child.

---

![[Pasted image 20251125010807.png]]

> Tại sao `SimpleRobot` add articulation root được còn `spot_micro` thì không?

Vì `base_link` đã sẵn là `articulation root` rồi, lưu ý việc `base_link` nằm ở đâu không quan trọng, `PhysX` sẽ kết nối các part qua joint.

==Important==: Ariticulation root phải được add trước differential controller (nhớ add drive trước)