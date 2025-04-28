

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

Theo nguồn tin (chuẩn, đôi khi ko chuẩn) từ ChatGPT và Grok, `setuptools`, `nvcc` và `python` cũng được build dựa trên `MSVC` (trên window) nên nó cần `MSVC` và `Window SDK` (chứa các header cần thiết) để thực hiện quá trình build. Thế cho nên `g++` thôi là chưa đủ (khỏi cần `g++`, chỉ cần `MSVC` thôi là đủ).

> Tải ở đâu?

Vào `Visual Studio Installer`, tìm `MSVC` và `Window SDK` và tải về là được.

### 2. Trên Ubuntu
