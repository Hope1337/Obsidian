==Stage==
Stage là nơi chứa toàn bộ thế giới (file usd) mà bạn đang nhìn trên màn hình 3D trong Isaacsim đó, trong stage gồm nhiều prim.

> Tôi thấy trong stage có prim World và Environment nằm cùng một level, tại sao World không bao quát Env luôn?

Đây là quy ước của Isaacsim, trong đó `/World` chứa những thứ chuyển động trong khi `/Environment` chứa nhứng thứ tĩnh. Ví dụ bạn setup một cánh tay robot trong warehouse, sau đó muốn giữa nguyên giả lập nhưng ở ngoài trời, chỉ cần thay `/Environment` là được.

==Composition==:
**Đặt vấn đề**: Giả sử bạn có một con robot và bạn tạo $N$ bản copy của nó, tuy nhiên bạn nhận ra con robot này hơi sai thiết kế, bạn sửa lại file gốc, xóa $N$ con robot ban đầu và thêm lại $N$ con đã sửa. Nhận ra vấn đề rồi chứ?

Composition là một giái pháp tuyệt vời, trong đó bạn không sửa trực tiếp trên các file gốc mà tạo ra một layer ghi đè lên layer cũ (docker cũng có cách tiếp cận giống như này).

==Xform==:
**Đặt vấn đề**: Giả sử bạn thiết kế bối cảnh, trong đó có một cái bàn và $N$ món đồ được đặt trên bàn, bạn muốn di chuyển cái bàn sang một ví trí khác, theo lẽ thường, bạn cũng sẽ muốn $N$ món đồ di chuyển theo vị trí tương đối so với cái bàn đúng không? Xform là thứ giúp bạn giải quyết được vấn đề này, trong đó các vật thể có cùng một `Xform` sẽ di chuyển tương đối theo nhau.

==Mesh==:
Đây là khái niệm khá thú vị, ví dụ bạn muốn display một quả bóng thì hình dạng, màu sắc của quả bóng đó được gọi là mesh. Tuy nhiên có hai loại mesh là `visual mesh` và `collision mesh`. Trong đó `visual mesh` là những gì bạn nhìn được, `collision mesh` là hình dạng thực sự mà `Physics Engine` dùng để tính toán va chạm (kiểu như hitbox ấy), chủ yếu để tối ưu tài nguyên.

==Prim==:
Mọi thứ xuất hiện trong tree của stage đều được gọi là prim, mỗi prim có $3$ đặc điểm quan trọng:
- **Path**: địa chỉ trong stage
- **Type**: là loại gì, ví dụ:
	- `Xform`: trục tọa độ tương đối
	- `Mesh`: vật thể hình học
	- `Material`: vật liệu
- **Attributes**: các thuộc tính, thông số
