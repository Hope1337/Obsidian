### 3.1 Multidimensional grids and data

- Trong CUDA, mọi thread trong một grid thực thi cùng một kernel
- `gridDim` có thể rất to: `gridDim.x` từ khoảng $1$ đến $2^{31}-1$ , `gridDim.y` và `gridDim.z` từ $1$ đến $2^{16} - 1$ (khoảng $65,535$) . 
- `blockDim` bị giới hạn, **tổng số các thread** của một block không được vượt quá $1024$ (đối với các phiên bản CUDA hiện nay).
Nguyên nhân cho việc trên là `block` bị giới hạn ở $1024$ thread là do block được thực thi ở SM, mà SM có giới hạn phần cứng nên không thực thi một block quá to được. Còn grid, nó không gắn cố định cho một phần cứng nào phải thực thi mà chỉ là lưới các block, thực thi lúc nào, theo thứ tự nào cũng được, chính các block của grid mới được xử lý bởi SM.

==Note==: Mỗi SM có thể xử lý nhiều block, miễn là đủ tài nguyên.


