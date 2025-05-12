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

Trong ví dụ này, mình sẽ code một kernel đơn giản là `vector_add`: cộng hai vector

Đầu tiên, mình sẽ tạo một folder `lablib`, đây sẽ là nơi chứa code `.cpp` và `.cu` của mình. Tiếp đến mình sẽ tạo hai file là `vector_add_cuda.cu` và `vector_add.cpp`:

File `vector_add_cuda.cu`:
```cpp
#include <cuda.h>
#include <cuda_runtime.h>
#include <torch/extension.h>
  
// CUDA kernel for vector addition
__global__ void vector_add_kernel(float* a, float* b, float* out, int n) {
    int idx = blockIdx.x * blockDim.x + threadIdx.x;
    if (idx < n) {
        out[idx] = a[idx] + b[idx];
    }
}


// Wrapper function to launch the kernel
void vector_add_cuda(torch::Tensor a, torch::Tensor b, torch::Tensor out) {
    int n = a.size(0);
    int threads = 256;
    int blocks = (n + threads - 1) / threads;

    vector_add_kernel<<<blocks, threads>>>(
        a.data_ptr<float>(),
        b.data_ptr<float>(),
        out.data_ptr<float>(),
        n
    );

    cudaError_t err = cudaGetLastError();
    TORCH_CHECK(err == cudaSuccess, "CUDA error: ", cudaGetErrorString(err));
}
```

File 'vector_add.cpp':

```cpp
#include <torch/extension.h>

void vector_add_cuda(torch::Tensor a, torch::Tensor b, torch::Tensor out);

torch::Tensor vector_add(torch::Tensor a, torch::Tensor b) {
    TORCH_CHECK(a.device().is_cuda(), "a must be a CUDA tensor");
    TORCH_CHECK(b.device().is_cuda(), "b must be a CUDA tensor");
    TORCH_CHECK(a.sizes() == b.sizes(), "Tensor sizes must match");
    TORCH_CHECK(a.dtype() == torch::kFloat32, "Tensors must be float32");

    auto out = torch::empty_like(a);
    vector_add_cuda(a, b, out);
    return out;
}

PYBIND11_MODULE(TORCH_EXTENSION_NAME, m) {
    m.def("vector_add", &vector_add, "Add two vectors on CUDA");
}
```

Đoạn code:
```cpp
PYBIND11_MODULE(TORCH_EXTENSION_NAME, m) {
    m.def("vector_add", &vector_add, "Add two vectors on CUDA");
}
```
Trong đó:
- `TORCH_EXTENSION_NAME`: là một biến sẽ được thay thế lúc chạy, biến này là tên của module bạn đang build (tí nữa bạn sẽ hiểu).
- string `vector_add` là tên của hàm mà bạn muốn đặt. Tức là hàm hiện tại đang được nối là `vector_add` (có dấu tham chiếu ở đối số thứ hai), tuy nhiên bạn hoàn toàn có thể đặt cho nó một tên khác bằng cách thay đổi string ở đối số thứ nhất, bạn đặt tên thế nào thì một tí nữa gọi ra dùng phải xài tên đó.

Tiếp theo ta cần code file `setup.py` :
```python
from setuptools import setup
from torch.utils.cpp_extension import BuildExtension, CUDAExtension

setup(
    name='vector_add',
    ext_modules=[
        CUDAExtension('hihi', [
            'lablib/vector_add.cpp',
            'lablib/vector_add_cuda.cu',
        ]),  
    ],
    cmdclass={
        'build_ext': BuildExtension
    }
)
```

Xong rồi, giờ bạn chỉ cần chạy `python setup.py install` để build. Và sau khi build xong, bạn chỉ cần `import hihi` và gọi `hihi.vector_add` là được.

```python
import torch
import hihi 

# Tạo 2 tensor trên GPU
a = torch.randn(5, device='cuda')
b = torch.randn(5, device='cuda')

# Gọi hàm vector_add của bạn
out = hihi.vector_add(a, b)

print(out)
print(torch.allclose(out, a + b))  # Kiểm tra xem kết quả có đúng không
```

Đó, vậy là chúng ta vừa build được một kernel `CUDA` và bind nó vào python để dùng rồi!


==Note==: Mình đặt tên extension là `hihi` là có mục đích, bạn có thấy tham số `name='vector_add'` ở trên không? Thực ra nếu bạn hỏi GPT hay Grok, tham số `name` này và cái tên trong `CUDAExtension` nó sẽ để giống nhau (tức cả hai đều là `vector_add`). Điều này ban đầu gây cho mình thắc mắc là.

> Vậy `name` để làm gì?

Chà, nó là tên package của bạn, tức là khi phân phối hoặc muốn xóa package bạn vừa build, bạn phải gõ lệnh:
```bash
# You shoud use this
pip uninstall vector_add

# NOT THIS 
pip uninstall hihi
```
Nếu không tin thì bạn cứ `pip show  hihi`, nó sẽ chẳng tìm thấy package nào cho bạn đâu, phải là `pip show vector_add` mới phải.

À bạn còn nhớ `TORCH_EXTENSION_NAME` không? Trong quá trình build, nó sẽ được thay bằng `hihi` và sẽ được nối với `vector_add` của chúng ta. Nhờ vậy mà bạn mới gọi được `hihi.vector_add` đó.


==Important==: Hãy đọc [[Build extension]] để hiểu các file được generate ra và thao tác đúng cách.