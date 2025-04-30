
### 1. Trên Window

Chuyện lúc build: project của mình có ba loại file: `.py`, '.cu' và '.cpp', máy mình đã cài `g++` (compiler cho `C++`) và `nvcc` (compiler cho `CUDA`). Thế nhưng lúc build, log lại đòi thêm hai thứ là `MSVC` và `Window SDK`. 

> Nó là gì vậy?

- `MSVC` là Microsoft Visual C++, là một compiler `C/C++` của Microsoft, nó bao gồm:
	- `cl.exe`: compiler `C/C++` 
	- `link.exe`: linker để nối object file thành excutable/shared lib.
	- `lib.exe`, 'nmake.exe'

- `Window SDK` (Software Development Kit) chứa các header files, lib files, APIs để có thể viết và chạy chương trình trên Window, nó gồm:
	- `.h` header files
	- `.lib` các static lib
	- Các tools phụ trợ

Theo nguồn tin (chuẩn, đôi khi ko chuẩn) từ ChatGPT và Grok, `python` được build dựa trên `MSVC` (trên window), hơn nữa, `setuptools` và `nvcc` cũng cần `MSVC`. Kèm theo đó `Window SDK` (chứa các header cần thiết) để thực hiện quá trình build. Thế cho nên `g++` thôi là chưa đủ (khỏi cần `g++`, chỉ cần `MSVC` và `Window SDK` thôi là đủ).

> Tải ở đâu?

Vào `Visual Studio Installer`, tìm `MSVC` và `Window SDK` và tải về là được.

### 2. Trên Ubuntu

Trên `Ubuntu` thì mọi chuyện bớt lằng nhằng hơn, bạn khỏi cần phải tải gì cả (trừ `CUDA toolkit' và tất nhiên là driver) vì `gcc/g++` của linux đã đủ để hỗ trợ build.

À nhớ đảm bảo phiên bản `CUDA` của `nvcc` khớp với `CUDA` của pytorch là được.

==Note==: `IntelliSense` đòi cả ba path bên dưới đây mới hoạt động:
```bash
...miniconda3/envs/lab/Lib/site-packages/torch/include, 
...miniconda3/envs/lab/Lib/site-packages/torch/include/torch/csrc/api/include,
...miniconda3/envs/lab/include
```
Path thứ ba là cho python nó hoạt động, `Pylance` thì tiện hơn do nó hoạt động ăn khớp với `conda` luôn, tức là nó sẽ tự kiếm python phù hợp với môi trường conda hiện tại.

Nếu chỉ thêm path đầu tiên (và path thứ ba) thì các thư viện như `#include <torch/extension.h>` vẫn hoạt động ở `.cpp`, tuy nhiên qua `.cu` thì nhót. Mình cũng không biết tại sao nhưng Grok giải thích phần này cũng khá lằng nhằng.

> Từ từ, `IntelliSense` hay `Pylance` là gì vậy đại ca Mạnh ơi?

Nhớ lúc code có mấy cái nhắc khéo tên hàm, tham số hàm... không? Nó là `IntelliSense/Pylance` (tương ứng cho `C++` và `python`) đó.

> Lười cài quá, bỏ được không?

Nhắm nhớ hết cú pháp thì cứ việc=))))))