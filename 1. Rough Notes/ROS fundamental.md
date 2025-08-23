ROS stands for *Robot Operating System*. ROS cho chúng ta các khả năng sau:
- Message Passing: giao tiếp giữa các module trong một distributed system
- Package Management
- Hardware Abstraction

---

## Problem Statement

Giả sử bạn muốn build một con humanoid robot thực hiện một tác vụ đơn giản là đi xung quanh vùng chỉ định trước tìm rác, nhặt rác bỏ vào thùng, và cứ lặp đi lặp lại như thế.

Ta có thể chia các thành phần của robot làm 3 categories chính:
- perception: các sensor như camera, gia tốc, vận tốc ...
- brain: đầu não xử lí
- actuation: các khớp chuyển động
![[Drawing 2025-07-19 10.49.31.excalidraw]]
Vậy làm sao để các module này giao tiếp được với nhau?

Một giải giáp đơn giản đó là polling: các thành phần này liên tục ghi và đọc một bộ nhớ chung để update thông tin của nhau. Dễ thấy là cách làm này không hiệu quả về mặt tính toán lắm, chúng ta có thể sử dụng event-based system, như kiểu web-hook ý.

ROS sẽ chịu trách nhiệm về việc communication.

> package management trong ROS là gì? Chẳng phải có conda quản lý gói rồi sao?

Ngoài các thư viện ngoài ra thì chúng ta hoàn toàn có thể tạo một package cho từng module của robot, lúc này sẽ cần ROS để quản lý, cụ thể vào code là sẽ hiểu.

---

## Nhưng cách giao tiếp giữa các module

#### Message
- topic nghĩa đen là topic luôn, đó là chủ đề mà các thành phần muốn trao đổi về. Publisher là thành phần mà update thông tin về topic đó và subscriber là thành phần mà muốn nhận thông tin về topic đó.

#### Service
- Như kiểu service trong mô hình client-server. Nó khác với message ở đây là thay vì đăng kí nhận thông tin, nó chủ động đòi hỏi thông tin hoặc yêu cầu thực hiện một tác vụ gì đó.

#### Action
- Action khá giống với service khi nó cũng yêu cầu một tác vụ gì đó, tuy nhiên service lại hoàn thành mọi thứ trước khi report về cho client. Action thì không giống vậy, client nhận được feedback khi đang thực hiện, cancel nếu muốn dừng giữa chừng, phù hợp cho task dài. 

---

## Workspace

Bình thường khi code chúng ta sẽ chỉ cần tạo một folder workspace, tuy nhiên ROS2 yêu cầu ta dùng lệnh
```bash
colcon build 
```
Để build các package bên trong.