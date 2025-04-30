#CUDA #pybind 

Trong documentation của pytorch có hai bài viết, một là [Custom C++ and CUDA Extensions](https://pytorch.org/tutorials/advanced/cpp_extension.html#custom-c-and-cuda-extensions) và một bài viết khác là [Custom C++ and CUDA Operators](https://pytorch.org/tutorials/advanced/cpp_custom_ops.html#custom-c-and-cuda-operators). Hai bài viết này khá tương tự nhau và thú thật mình cũng chưa rõ lắm khi nào dùng cái nào.

Tuy nhiên, bài viết đầu tiên có vẻ đơn giản hơn nhiều và phù hợp với mục đích viết một kernel CUDA rồi gọi nó dùng trong python kết hợp với pytorch, vậy cho nên mình sẽ tập trung vào nó trước.

==Important==: Check qua bài viết [[Setup compiler CUDA trên Window và Ubuntu]] trước khi đọc tiếp, nó sẽ giúp bạn chuẩn bị những thứ cần thiết 

### 1. Tổng quan

==Để dùng được một hàm CUDA (hoặc C++) trong python ta cần viết một số file sau==:

- File `.cu`: chứa kernel CUDA.
- File `.cpp`:
	- Chứa các hàm thuần `C++` (optional, nếu bạn muốn có thêm các hàm `C++`).
	- Chứa các hàm wrapper của kernel `CUDA`, các hàm này sẽ phụ trách các bước kiểm tra đầu vào (hoặc tiền xử lý gì đó) trước khi thực sự gọi kernel `CUDA` và trả về kết quả.
	- Code binding với python (sẽ được làm rõ sau)
- File `setup.py`: cấu hình và build 

Trước khi tiếp tục, mình sẽ cần làm rõ một số điểm như sau:

>Binding là gì?

Để dùng được hàm của `C++` trong python, ta phải thực hiện một thứ gọi là `binding`, tức là ta nối hàm `C++` được chỉ định vào một hàm trong python để có thể dùng được, vì dù gì tụi nó cũng là hai ngôn ngữ khác nhau mà.

> `setup.py` là một file python bình thường thôi hả?

Cũng không hẳn, nó là quy ước của `setuptools`- một bộ công cụ trong python giúp bạn có thể đóng gói code của bạn thành một package. Các bạn hay dùng `pip install` rồi import các thư viện đúng không? Bạn hoàn toàn có thể code một package, đóng gói nó rồi dùng tương tự code của mình như một package thông thường vậy đó.

> Package thì liên quan gì đến việc binding CUDA C và python?

Thực ra là có, `setuptools` sẽ build code `.cu` và `.cpp` của các bạn thành file `.so`, file này là mã máy nhị phân, cũng là package mà các bạn sẽ dùng. Tức là sau khi được code `CUDA` và `C++` của các bạn được biên dịch thành file `.so`, bạn có thể import nó trong python như một thư viện bình thường, và đây cũng chính là cách mà `setuptools` làm để build các thư viện khác nữa đó.

==Next==: Sau khi bạn đã có được ba file như trên rồi (sẽ được hướng dẫn ở bên dưới) thì việc tiếp theo là build, tức là biên dịch `.cu` và `.cpp` thành file `.so` để có thể import:

```bash
python setup.py install
```

Lúc này quá trình `build` sẽ diễn ra, nó sẽ tạo ra một số file cần thiết, sau đó bạn chỉ cần import (hướng dẫn ở bên dưới) hàm bạn vừa build là được rồi.
### 2. Let's get hands on

Trong ví dụ này, mình sẽ code một 

Đầu tiên mình sẽ tạo một file vector.cu
