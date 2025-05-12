#CUDA

### 2.1 Data parallelism

- Trong parallel computing, không hẳn do khối lượng tính toán lớn khiến chương trình chậm, thực tế load data mới chậm thực sự.

### 2.2 CUDA C program structure

- Mặc dù là parallel nhưng những phần code được parallel vẫn có thể là sequential. Giả sử bạn cần tính tổng phần tử của vector, và có 1000 vector như vậy, tức là có 1000 kernel CUDA C có thể gọi, mỗi kernel tính tổng của 1 vector duy nhất

> Sao không đệ quy kernel luôn?

Chi phí gọi một kernel không ít đến mức ta có thể làm như vậy.

### 2.4 Device global memory and data transfer

- `cudaMalloc` không cấp phát **trực tiếp** bộ nhớ 2D mà chỉ cấp phát một khối bộ nhớ tuyến tính trên gpu, do đó cần flatten mảng đầu vào và sử dụng các kĩ thuật mapping (row)

- Trước khi gọi `cudaMalloc`, cần ép kiểu con trỏ về `(void **)`. Lý do là C++ mặc định pass by value (khác với python pass **object reference** **by value**), để

### 2.5 Kernel functions and threading

CUDA có những built-in variables:
- `gridDim`: kiểu `dim3`, `gridDim.x`, `gridDim.y`, `gridDim.z` là số lượng block trong mỗi chiều của grid.
- `blockDim`: kiểu `dim3`, `blockDim.x`, `blockDim.y`, `blockDim.z` là số lượng thread trong mỗi chiều của block.
- `blockIdx`: xác định index của block trong mỗi chiều.
- `threadIdx`: xác định index của thread trong mỗi chiều.
- `warpSize`: idk, nghe mới z 😃

CUDA cũng phân ra làm một hệ thống phân cấp gồm hai cấp độ:
- Grid
- Block

Các kernel được gọi và thực thi theo cách khác nhau, cụ thể là:

|              |  Callable from   | Excuted on |         Excuted by         |
| :----------: | :--------------: | :--------: | :------------------------: |
|  `__host__`  |       Host       |    Host    |     Caller host thread     |
| `__global__` | Host (or device) |   Device   | New grid of device threads |
| `__device__` |      Device      |   Device   |    Caller device thread    |
==Observation==: Việc các kernel `__host__` và `__device__` được thực thi bằng chính các thread gọi nó tức là đệ quy trên các kernel này không làm tăng tốc quá trình thực thi.